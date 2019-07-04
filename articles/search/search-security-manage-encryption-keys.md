---
title: 'Cifrado en reposo mediante claves administradas por el cliente en Azure Key Vault (versión preliminar): Azure Search'
description: Complemente el cifrado del lado servidor con índices y mapas de sinónimos en Azure Search mediante las claves que se crean y se administran en Azure Key Vault.
author: NatiNimni
manager: jlembicz
ms.author: natinimn
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.custom: ''
ms.openlocfilehash: 949628fa52b4b020d70b75f4a0e7895f1e0f8bba
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485316"
---
# <a name="azure-search-encryption-using-customer-managed-keys-in-azure-key-vault"></a>Cifrado de Azure Search mediante claves administradas por el cliente en Azure Key Vault

> [!Note]
> El cifrado con claves administradas por el cliente se encuentra en versión preliminar y no se ha pensado para su uso en producción. En la [API REST, versión 2019-05-06-Preview](search-api-preview.md) se proporciona esta característica. También puede usar .NET SDK versión 8.0-preview.
>
> Esta característica no está disponible para los servicios gratis. Debe utilizar un servicio de búsqueda facturable creado a partir del 2019-01-01-01. En este momento no es compatible con el portal.

De forma predeterminada, Azure Search cifra el contenido de usuario en reposo con [claves administradas por el servicio](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest#data-encryption-models). Puede complementar el cifrado predeterminado con un nivel de cifrado adicional con las claves que se crean y administran en Azure Key Vault. Este artículo le guía a través de los pasos.

Se admite el cifrado de lado servidor mediante la integración con [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview). Puede crear sus propias claves de cifrado y almacenarlas en un almacén de claves, o puede usar las API de Azure Key Vault para generar las claves de cifrado. Con Azure Key Vault, también puede auditar el uso de claves. 

El cifrado con claves administradas por el cliente se configura a nivel de índice o de mapa de sinónimos cuando se crean esos objetos, y no a nivel de servicio de búsqueda. No se puede cifrar el contenido que ya existe. 

Puede usar claves diferentes desde distintos almacenes de claves. Esto significa que un único servicio de búsqueda puede hospedar varios mapas de sinónimos e índices cifrados, cada uno de los cuales puede utilizar una clave diferente administrada por un cliente, junto con asignaciones de sinónimos o índices que no están cifrados mediante claves administradas por el cliente. 

## <a name="prerequisites"></a>Requisitos previos

En este ejemplo se usan los servicios siguientes. 

+ [Cree un servicio Azure Search](search-create-service-portal.md) o [busque un servicio existente](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) en su suscripción actual. Puede usar un servicio gratuito para este tutorial.

+ [Cree un recurso de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-portal#create-a-vault) o busque un almacén existente en su suscripción.

+ Se usan [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) o la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) para las tareas de configuración.

+ Se utilizan [Postman](search-get-started-postman.md), [Azure PowerShell](search-create-index-rest-api.md) y [SDK de Azure Search](https://aka.ms/search-sdk-preview) para llamar a la API REST de versión preliminar. No hay compatibilidad con el portal o con el SDK de .NET para el cifrado administrado por el cliente en este momento.

## <a name="1---enable-key-recovery"></a>1\. Habilitación de la recuperación de claves

Este paso es opcional, pero muy recomendable. Después de crear el recurso de Azure Key Vault, habilite **Soft Delete** (Eliminación temporal) y **Purge Protection** (Protección de purgas) en el almacén de claves seleccionado mediante la ejecución de los siguientes comandos de PowerShell o de la CLI de Azure:   

```powershell
$resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "<vault_name>").ResourceId

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enableSoftDelete" -Value 'true'

$resource.Properties | Add-Member -MemberType NoteProperty -Name "enablePurgeProtection" -Value 'true'

Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

```azurecli-interactive
az keyvault update -n <vault_name> -g <resource_group> --enable-soft-delete --enable-purge-protection
```

>[!Note]
> Debido a la naturaleza misma del cifrado con la característica de claves administradas por el cliente, Azure Search no podrá recuperar los datos si se elimina la clave del almacén de Azure Key Vault. Para evitar la pérdida de datos causada por las eliminaciones accidentales de claves de Key Vault, se recomienda encarecidamente que habilite las opciones de eliminación temporal y de protección de purgas en el almacén de claves seleccionado. Para más información, consulte el artículo sobre la [eliminación temporal de Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).   

## <a name="2---create-a-new-key"></a>2\. Creación de una nueva clave

Si utiliza una clave existente para cifrar el contenido de Azure Search, omita este paso.

1. [Inicie sesión en Azure Portal](https://portal.azure.com) y vaya al panel del almacén de claves.

1. Seleccione la configuración de las **claves** de panel de navegación izquierdo y haga clic en **+ Generate/Import** (+Generar/importar).

1. En el panel **Crear una clave**, forme la lista de **Opciones** y elija el método que quiera usar para crear una clave. También puede **generar** una nueva clave, **cargar** una clave existente, o usar **Restaurar copia de seguridad** para seleccionar una copia de seguridad de una clave.

1. Especifique un **nombre** para la clave y, opcionalmente, seleccione otras propiedades clave.

1. Haga clic en el botón **Crear** para iniciar la implementación.

Tome nota del identificador de la clave: se compone del **Uri del valor de clave**, el **nombre de clave** y la **versión de clave**. Los necesitará para definir un índice cifrado en Azure Search.
 
![Crear una nueva clave de almacén de claves](./media/search-manage-encryption-keys/create-new-key-vault-key.png "Create a new key vault key")

## <a name="3---create-a-service-identity"></a>3\. Creación de una identidad de servicio

La asignación de una identidad a su servicio de búsqueda le permite conceder permisos de acceso a Key Vault a su servicio de búsqueda. El servicio de búsqueda usará su identidad para autenticarse con Azure Key Vault.

Azure Search admite dos formas de asignar la identidad: una identidad administrada o una aplicación de Azure Active Directory administrada externamente. 

Si es posible, utilice una identidad administrada. Es la forma más sencilla de asignar una identidad al servicio de búsqueda y debería funcionar en la mayoría de los casos. Si está utilizando varias claves para índices y mapas de sinónimos, o si la solución se encuentra en una arquitectura distribuida que descalifica la autenticación basada en identidad, utilice el [enfoque avanzado de Azure Active Directory administrado externamente](#aad-app), que se describe al final de este artículo.

 En general, una identidad administrada permite que el servicio de búsqueda se autentique en Azure Key Vault sin almacenar las credenciales en código. El ciclo de vida de este tipo de identidad administrada está ligado al ciclo de vida del servicio de búsqueda, que solo puede tener una identidad administrada. [Más información sobre identidades administradas](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

1. Para crear una identidad administrada, [inicie sesión en Azure Portal](https://portal.azure.com) y abra el panel del servicio de búsqueda. 

1. Haga clic en **Identidad** en el panel de navegación izquierdo, cambie su estado a **Activado** y haga clic en **Guardar**.

![Habilitación de una identidad administrada](./media/search-enable-msi/enable-identity-portal.png "Enable a managed identity")

## <a name="4---grant-key-access-permissions"></a>4\. Concesión de permisos de acceso clave

Para habilitar el servicio de búsqueda y usar la clave de Key Vault, deberá conceder al servicio de búsqueda determinados permisos de acceso.

Los permisos de acceso se pueden revocar en cualquier momento. Una vez revocados, cualquier índice o mapa de sinónimos de servicio de búsqueda que utilice ese almacén de claves quedará inutilizable. La restauración de los permisos de acceso al almacén de claves en un momento posterior restaurará el acceso al índice o mapa de sinónimos. Para más información, consulte [Protección del acceso a un almacén de claves](https://docs.microsoft.com/azure/key-vault/key-vault-secure-your-key-vault).

1. [Inicie sesión en Azure Portal](https://portal.azure.com) y abra la página de información general del almacén de claves. 

1. Seleccione la opción **Directivas de acceso** en panel de navegación izquierdo y haga clic en **+ Agregar nuevo**.

   ![Adición de una nueva directiva de acceso del almacén de claves](./media/search-manage-encryption-keys/add-new-key-vault-access-policy.png "Add new key vault access policy")

1. Haga clic en **Seleccionar la entidad de seguridad** y seleccione el servicio Azure Search. Puede buscarlo por nombre o por el identificador de objeto que se mostró después de habilitar la identidad administrada.

   ![Selección de la entidad de seguridad de la directiva de acceso del almacén de claves](./media/search-manage-encryption-keys/select-key-vault-access-policy-principal.png "Select key vault access policy principal")

1. Haga clic en **Permisos de clave** y seleccione *Obtener*, *Desencapsular clave* y *Encapsular clave*. Puede usar la plantilla *Azure Data Lake Storage o Azure Storage* para seleccionar rápidamente los permisos necesarios.

   A Azure Search se le deben conceder los siguientes [permisos de acceso](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-operations):

   * *Obtener*: permite al servicio de búsqueda recuperar las partes públicas de una clave en un almacén de Key Vault
   * *Encapsular clave*: permite que el servicio de búsqueda utilice la clave para proteger la clave de cifrado interno
   * *Desencapsular clave*: permite que el servicio de búsqueda utilice la clave para desencapsular la clave de cifrado interno

   ![Selección de los permisos de clave de la directiva de acceso de almacén de claves](./media/search-manage-encryption-keys/select-key-vault-access-policy-key-permissions.png "Select key vault access policy key permissions")

1. Haga clic en **Aceptar** y **guarde** los cambios de directiva de acceso.

> [!Important]
> El contenido cifrado de Azure Search está configurado para utilizar una clave específica de Azure Key Vault con una **versión** específica. Si cambia la clave o versión, debe actualizarse el índice o el mapa de sinónimos para utilizar la nueva versión de clave **antes de** eliminar la versión de clave anterior. Si no lo hace, el índice o el mapa de sinónimos quedará inutilizable, ya que no podrá descifrar el contenido una vez que se pierda la clave de acceso.   

## <a name="5---encrypt-content"></a>5\. Cifrado del contenido

La creación de un índice o mapa de sinónimos cifrado con una clave administrada por el cliente todavía no es posible mediante Azure Portal. Utilice la API REST de Azure Search para crear este tipo de índice o mapa de sinónimos.

Tanto el índice como el mapa de sinónimos admiten una nueva propiedad **encryptionKey** de nivel superior utilizada para especificar la clave. 

Con el **URI del almacén de claves**, el **nombre de la clave** y la  **versión de clave** de la clave del almacén de claves, podemos crear una definición de **encryptionKey**:

```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
> [!Note] 
> Ninguno de estos detalles del almacén de claves se considera secreto y se pueden recuperar fácilmente yendo a la página de claves relevante de Azure Key Vault en Azure Portal.

Si utiliza una aplicación de AAD para la autenticación del almacén de Key Vault en lugar de utilizar una identidad administrada, agregue las **credenciales de acceso** de la aplicación de AAD a la clave de cifrado: 
```json
{
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660",
    "accessCredentials": {
      "applicationId": "00000000-0000-0000-0000-000000000000",
      "applicationSecret": "myApplicationSecret"
    }
  }
}
```

## <a name="example-index-encryption"></a>Ejemplo: Cifrado del índice
Los detalles de la creación de un nuevo índice a través de API REST se encuentran en [Create Index Map (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-index) (Creación de una asignación de índice [API REST de Azure Search Service]), donde la única diferencia es la especificación de los detalles de la clave de cifrado como parte de la definición del índice: 

```json
{
 "name": "hotels",  
 "fields": [
  {"name": "HotelId", "type": "Edm.String", "key": true, "filterable": true},
  {"name": "HotelName", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": true, "facetable": false},
  {"name": "Description", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "en.lucene"},
  {"name": "Description_fr", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false, "analyzer": "fr.lucene"},
  {"name": "Category", "type": "Edm.String", "searchable": true, "filterable": true, "sortable": true, "facetable": true},
  {"name": "Tags", "type": "Collection(Edm.String)", "searchable": true, "filterable": true, "sortable": false, "facetable": true},
  {"name": "ParkingIncluded", "type": "Edm.Boolean", "filterable": true, "sortable": true, "facetable": true},
  {"name": "LastRenovationDate", "type": "Edm.DateTimeOffset", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Rating", "type": "Edm.Double", "filterable": true, "sortable": true, "facetable": true},
  {"name": "Location", "type": "Edm.GeographyPoint", "filterable": true, "sortable": true},
 ],
 "encryptionKey": {
   "keyVaultUri": "https://demokeyvault.vault.azure.net",
   "keyVaultKeyName": "myEncryptionKey",
   "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
 }
}
```
Ahora puede enviar la solicitud de creación del índice y, a continuación, empiece a usar el índice con normalidad.

## <a name="example-synonym-map-encryption"></a>Ejemplo: Cifrado del mapa de sinónimos

Los detalles de la creación de un nuevo mapa de sinónimos a través de API REST se encuentran en [Create Synonym Map (Azure Search Service REST API)](https://docs.microsoft.com/rest/api/searchservice/create-synonym-map) (Creación de un mapa de sinónimos [API REST de Azure Search Service]), donde la única diferencia es la especificación de los detalles de la clave de cifrado como parte de la definición del mapa de sinónimos: 

```json
{   
  "name" : "synonymmap1",  
  "format" : "solr",  
  "synonyms" : "United States, United States of America, USA\n
  Washington, Wash. => WA",
  "encryptionKey": {
    "keyVaultUri": "https://demokeyvault.vault.azure.net",
    "keyVaultKeyName": "myEncryptionKey",
    "keyVaultKeyVersion": "eaab6a663d59439ebb95ce2fe7d5f660"
  }
}
```
Ahora puede enviar la solicitud de creación del mapa de sinónimos y, a continuación, empiece a usarlo con normalidad.

>[!Important] 
> Aunque **encryptionKey** no se puede agregar a los índices de Azure Search ni a las asignaciones de sinónimos existentes, se puede actualizar al proporcionar valores diferentes para cualquiera de los tres detalles del almacén de claves (por ejemplo, mediante la actualización de la versión de la clave). Al cambiar a una nueva clave del almacén de claves o a una nueva versión de la clave, cualquier índice de Azure Search o mapa de sinónimos que utilice la clave debe actualizarse primero para utilizar la nueva versión de clave **antes de** eliminar la versión de clave anterior. Si no lo hace, el índice o el mapa de sinónimos quedarán inutilizables, ya que no podrá descifrar el contenido una vez que se pierda el acceso a la clave.   
> La restauración de los permisos de acceso al almacén de claves en un momento posterior restaurará el acceso al contenido.

## <a name="aad-app"></a> Avanzado: Uso de una aplicación de Azure Active Directory administrada externamente

Cuando una identidad administrada no es posible, puede crear una aplicación de Azure Active Directory con una entidad de seguridad para el servicio de Azure Search. En concreto, una identidad administrada no es viable en estas condiciones:

* No puede conceder directamente a su servicio de búsqueda permisos de acceso al almacén de claves (por ejemplo, si el servicio de búsqueda se encuentra en un inquilino de Active Directory diferente al de Azure Key Vault).

* Se requiere un único servicio de búsqueda para hospedar varias asignaciones de sinónimos e índices cifrados, cada una de las cuales utiliza una clave diferente de un almacén de claves diferente, en el que cada almacén de claves debe utilizar una **identidad diferente** para la autenticación. Si no es necesario utilizar una identidad diferente para administrar distintos almacenes de claves, considere la posibilidad de utilizar la opción de identidad administrada anterior.  

Para dar cabida a estas topologías, Azure Search admite el uso de aplicaciones de Azure Active Directory (AAD) para la autenticación entre el servicio de búsqueda y Key Vault.    
Para crear una aplicación AAD en el portal:

1. [Crear una aplicación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application).

1. [Obtenga la clave de autenticación y el identificador de aplicación](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) como los que se necesitarán para crear un índice cifrado. Los valores que deberá proporcionar incluyen el **identificador de la aplicación** y la **clave de autenticación**.

>[!Important]
> Cuando se decide utilizar una aplicación de autenticación de AAD en lugar de una identidad administrada, hay que tener en cuenta el hecho de que Azure Search no está autorizado para administrar la aplicación de AAD en nombre del usuario, y que es el usuario quien debe administrar la aplicación de AAD, como la rotación periódica de la clave de autenticación de la aplicación.
> Al cambiar una aplicación de AAD o su clave de autenticación, cualquier índice de Azure Search o mapa de sinónimos que utilice esa aplicación debe actualizarse primero para utilizar la nueva clave o identificador de aplicación **antes** de eliminar la aplicación anterior o su clave de autorización, y antes de revocar el acceso del almacén de claves a la misma.
> Si no lo hace, el índice o el mapa de sinónimos quedarán inutilizables, ya que no podrá descifrar el contenido una vez que se pierda el acceso a la clave.   

## <a name="next-steps"></a>Pasos siguientes

Si no está familiarizado con la arquitectura de seguridad de Azure, revise la [documentación sobre seguridad de Azure](https://docs.microsoft.com/azure/security/), y en particular, este artículo:

> [!div class="nextstepaction"]
> [Cifrado en reposo de datos](https://docs.microsoft.com/azure/security/azure-security-encryption-atrest)
