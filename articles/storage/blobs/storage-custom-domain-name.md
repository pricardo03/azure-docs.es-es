---
title: Configuración de un nombre de dominio personalizado para una cuenta de Azure Storage | Microsoft Docs
description: Utilice Azure Portal para asignar su propio nombre canónico (CNAME) al punto de conexión de Blob Storage o web en una cuenta de Azure Storage.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: e40b6fe115d6b6dea38ead9f0b2550d96bd04c7a
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58112643"
---
# <a name="configure-a-custom-domain-name-for-your-azure-storage-account"></a>Configuración de un nombre de dominio personalizado para una cuenta de Azure Storage

Puede configurar un dominio personalizado para obtener acceso a los datos Blob en la cuenta de almacenamiento de Azure. El punto de conexión predeterminado para Azure Blob Storage es *\<storage-account-name>.blob.core.windows.net*. También puede utilizar el punto de conexión web generado como parte de la [característica de sitios web estáticos (versión preliminar)](storage-blob-static-website.md). Si asigna un dominio y subdominio personalizados, como *www\.contoso.com*, hasta el extremo de blob o web para la cuenta de almacenamiento, los usuarios pueden usar ese dominio para acceder a los datos blob en la cuenta de almacenamiento.

> [!IMPORTANT]
> Azure Storage no admite aún HTTPS con dominios personalizados de forma nativa. Actualmente puede [usar Azure CDN para obtener acceso a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md).
> 
> 
> [!NOTE]
> Actualmente, las cuentas de almacenamiento admiten un único nombre de dominio personalizado por cuenta. No puede asignar un nombre de dominio personalizado a los puntos de conexión de servicio web y de blob.
> 
> [!NOTE]
> La asignación solo funciona para los subdominios (por ejemplo, www\.contoso.com). Si desea que el punto de conexión web disponible en el dominio raíz (por ejemplo, contoso.com), entonces deberá [usar CDN de Azure con dominios personalizados](storage-https-custom-domain-cdn.md)

La siguiente tabla muestra algunas direcciones URL de ejemplo para datos de blob ubicados en una cuenta de almacenamiento denominada *mystorageaccount*. El subdominio personalizado que está registrado para la cuenta de almacenamiento es *www\.contoso.com*:

| Tipo de recurso | Dirección URL predeterminada | URL de dominio personalizado |
| --- | --- | --- |
| Cuenta de almacenamiento | http://mystorageaccount.blob.core.windows.net | http://www.contoso.com |
| Blob |http://mystorageaccount.blob.core.windows.net/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Contenedor raíz | http://mystorageaccount.blob.core.windows.net/myblob o http://mystorageaccount.blob.core.windows.net/$root/myblob| http://www.contoso.com/myblob o http://www.contoso.com/$root/myblob |
| Web |  http://mystorageaccount.[zone].web.core.windows.net/$web/[indexdoc] o http://mystorageaccount.[zone].web.core.windows.net/[indexdoc] o http://mystorageaccount.[zone].web.core.windows.net/$web o http://mystorageaccount.[zone].web.core.windows.net/ | http://www.contoso.com/$web o http://www.contoso.com/ o http://www.contoso.com/$web/[indexdoc] o http://www.contoso.com/[indexdoc] |

> [!NOTE]  
> Como se muestra en las siguientes secciones, todos los ejemplos del punto de conexión de Blob service también se aplican al punto de conexión de servicio web.

## <a name="direct-vs-intermediary-cname-mapping"></a>Directa frente a asignación de CNAME intermedia

Puede apuntar el dominio personalizado como precedido un subdominio (por ejemplo, www\.contoso.com) hasta el extremo de blob para la cuenta de almacenamiento de dos maneras: 
* Use la asignación directa de CNAME.
* Use el subdominio intermedio *asverify*.

### <a name="direct-cname-mapping"></a>Asignación directa de CNAME

El primer método, y el más simple, es crear un registro de nombre canónico (CNAME) que se asigne el dominio y el subdominio personalizados directamente al punto de conexión del blob. Un registro CNAME es una característica del sistema de nombres de dominio (DNS) que asigna un dominio de origen a un dominio de destino. En nuestro ejemplo, el dominio de origen es su propio dominio y subdominio personalizados (*www\.contoso.com*, por ejemplo). El dominio de destino es su punto de conexión de Blob service (por ejemplo, *mystorageaccount.blob.core.windows.net*).

El método directo se trata en la sección "Registro de un dominio personalizado".

### <a name="intermediary-mapping-with-asverify"></a>Asignación intermedia con *asverify*

El segundo método también utiliza los registros CNAME. Para evitar tiempos de inactividad, primero emplea un subdominio especial *asverify* que es reconocido por Azure.

La asignación de un dominio personalizado a un punto de conexión de blob puede provocar un período breve de inactividad mientras registra el dominio en [Azure Portal](https://portal.azure.com). Si el dominio ya incluye una aplicación con un Acuerdo de Nivel de Servicio (SLA) que requiere que no exista tiempo de inactividad, use el subdominio *asverify* de Azure como paso de registro intermedio. Este paso garantiza que los usuarios puedan acceder al dominio mientras se realiza la asignación de DNS.

El método intermedio se trata en Registro de un dominio personalizado mediante el subdominio *asverify*.

## <a name="register-a-custom-domain"></a>Registro de un dominio personalizado
Registre el dominio mediante el procedimiento de esta sección si se aplican las siguientes instrucciones:
* No le preocupa que el dominio no esté disponible brevemente para los usuarios.
* El dominio personalizado no hospeda actualmente una aplicación. 

Puede utilizar Azure DNS para configurar un nombre DNS personalizado para el almacén de blobs de Azure. Para más información, consulte [Usar Azure DNS para proporcionar la configuración de un dominio personalizado para un servicio de Azure](https://docs.microsoft.com/azure/dns/dns-custom-domain#blob-storage).

Si el dominio personalizado ya incluye una aplicación que no puede tener ningún tiempo de inactividad, use el procedimiento Registro de un dominio personalizado mediante el subdominio *asverify*.

Para configurar un nombre de dominio personalizado, cree un registro CNAME en DNS. El registro CNAME especifica un alias para un nombre de dominio. En nuestro ejemplo, asigna la dirección del dominio personalizado al punto de conexión de Blob Storage de la cuenta de almacenamiento.

Por lo general, puede administrar la configuración de DNS de su dominio en el sitio web del registrador de su dominio. Cada registrador dispone de un método similar pero ligeramente distinto de especificación de un registro CNAME. Sin embargo, el concepto es el mismo. Puesto que algunos paquetes de registro de domino básicos no ofrecen configuración DNS, puede que sea necesario actualizar el paquete de registro de dominios antes de crear el registro CNAME.

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento.

1. En el panel de menú, en **Blob service**, seleccione **Dominio personalizado**.  
   Se abre el panel **Dominio personalizado**.

1. Inicie sesión en el sitio web del registrador de su dominio y vaya a la página de administración de DNS.  
   Podría encontrar la página en una sección como **Nombre de dominio**, **DNS** o **Administración del servidor de nombres**.

1. Busque la sección para la administración de CNAME.  
   Tiene que dirigirse a la página de configuración avanzada y buscar **CNAME**, **Alias** o **Subdomains**.

1. Crear un nuevo registro CNAME, escriba un alias de subdominio como **www** o **fotos** (subdominio es obligatorio, no se admiten los dominios raíz) y, a continuación, proporcione un nombre de host.  
   El nombre de host es el punto de conexión de Blob service. Su formato es *\<mystorageaccount>.blob.core.windows.net*, donde *mystorageaccount* es el nombre de la cuenta de almacenamiento. El nombre de host que se usará aparece en el elemento n.º 1 del panel **Dominio personalizado** en [Azure Portal](https://portal.azure.com). 

1. En el cuadro de texto del panel **Dominio personalizado**, escriba el nombre de su dominio personalizado, incluido el subdominio.  
   Por ejemplo, si su dominio es *contoso.com* y su alias de subdominio es *www*, escriba **www\.contoso.com**. Si su subdominio es *fotos*, escriba **fotos.contoso.com**.

1. Para registrar el dominio personalizado, seleccione **Guardar**.  
   Si el registro se realiza correctamente, en el portal se notificará que la cuenta de almacenamiento se actualizó correctamente.

Una vez que el nuevo registro CNAME se propaga mediante DNS, si los usuarios tienen los permisos apropiados, pueden ver datos de blob mediante su dominio personalizado.

## <a name="register-a-custom-domain-by-using-the-asverify-subdomain"></a>Registro de un dominio personalizado mediante el subdominio *asverify*
Si el dominio personalizado actualmente es compatible con una aplicación con un Acuerdo de Nivel de Servicio que requiera que no exista tiempo de inactividad, registre el dominio personalizado con el uso del procedimiento de esta sección. Si crea un CNAME que apunte de *asverify.\<subdomain>.\<customdomain>* a *asverify.\<storageaccount>.blob.core.windows.net*, puede registrar previamente el dominio con Azure. A continuación, puede crear un segundo CNAME que apunte de *\<subdomain>.\<customdomain>* a *\<storageaccount>.blob.core.windows.net* y después el tráfico al dominio personalizado se dirigirá al punto de conexión del blob.

El subdominio *asverify* es un subdominio especial reconocido por Azure. Si antepone *asverify* a su propio subdominio, permite que Azure reconozca el dominio personalizado sin tener que modificar el registro DNS para el dominio. Cuando modifique el registro DNS para el dominio, se asignará al punto de conexión del blob sin que exista tiempo de inactividad.

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento.

1. En el panel de menú, en **Blob service**, seleccione **Dominio personalizado**.  
   Se abre el panel **Dominio personalizado**.

1. Inicie sesión en el sitio web del proveedor de DNS y vaya a la página de administración de DNS.  
   Podría encontrar la página en una sección como **Nombre de dominio**, **DNS** o **Administración del servidor de nombres**.

1. Busque la sección para la administración de CNAME.  
   Tiene que dirigirse a la página de configuración avanzada y buscar **CNAME**, **Alias** o **Subdomains**.

1. Cree un registro CNAME, proporcione un alias de subdominio que incluya el subdominio *asverify*, como **asverify.www** o **asverify.fotos**, y después proporcione un nombre de host.  
   El nombre de host es el punto de conexión de Blob service. Su formato es *asverify.\<mystorageaccount>.blob.core.windows.net*, donde *mystorageaccount* es el nombre de la cuenta de almacenamiento. El nombre de host que se usará aparece en el elemento n.º 2 del panel *Dominio personalizado* en [Azure Portal](https://portal.azure.com).

1. En el cuadro de texto del panel **Dominio personalizado**, escriba el nombre de su dominio personalizado, incluido el subdominio.  
   No incluya *asverify*. Por ejemplo, si su dominio es *contoso.com* y su alias de subdominio es *www*, escriba **www\.contoso.com**. Si su subdominio es *fotos*, escriba **fotos.contoso.com**.

1. Seleccione la casilla **Usar validación CNAME indirecta**.

1. Para registrar el dominio personalizado, seleccione **Guardar**.  
   Si el registro se realiza correctamente, en el portal se notificará que la cuenta de almacenamiento se actualizó correctamente. Azure ha verificado el dominio personalizado, pero aún no se ha realizado el enrutamiento del tráfico al dominio en la cuenta de almacenamiento.

1. Vuelva al sitio web del proveedor de DNS y cree otro registro CNAME que asigne su subdominio al punto de conexión de Blob service.  
   Por ejemplo, especifique el subdominio como *www* o *fotos* (sin *asverify*) y el nombre de host como *\<mystorageaccount>.blob.core.windows.net*, donde *mystorageaccount* es el nombre de la cuenta de almacenamiento. Con este paso se completa el registro del dominio personalizado.

1. Finalmente, puede eliminar el registro CNAME que acaba de crear y que contiene el subdominio *asverify*, que solo era necesario como paso intermedio.

Una vez que el nuevo registro CNAME se propaga mediante DNS, si los usuarios tienen los permisos apropiados, pueden ver datos de blob mediante su dominio personalizado.

## <a name="test-your-custom-domain"></a>Prueba de un dominio personalizado

Para confirmar que el dominio personalizado se haya asignado al punto de conexión de Blob service, cree un blob en un contenedor público en la cuenta de almacenamiento. A continuación, en un explorador web, acceda al blob mediante el uso de un identificador URI con el siguiente formato: `http://<subdomain.customdomain>/<mycontainer>/<myblob>`.

Por ejemplo, para acceder a un formulario web del contenedor *myforms* en el subdominio personalizado *photos.contoso.com*, debe usar el siguiente identificador URI: `http://photos.contoso.com/myforms/applicationform.htm`.

## <a name="deregister-a-custom-domain"></a>Anulación del registro de un dominio personalizado

Para anular el registro de un dominio personalizado para su punto de conexión de Blob Storage, use uno de los procedimientos siguientes.

### <a name="azure-portal"></a>Azure Portal

Para quitar la configuración del dominio personalizado, haga lo siguiente:

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento.

1. En el panel de menú, en **Blob service**, seleccione **Dominio personalizado**.  
   Se abre el panel **Dominio personalizado**.

1. Borre el contenido del cuadro de texto que incluye el nombre de dominio personalizado.

1. Seleccione el botón **Guardar**.

Una vez se haya quitado correctamente el dominio personalizado, verá una notificación del portal que indica que la cuenta de almacenamiento se actualizó correctamente.

### <a name="azure-cli"></a>Azure CLI

Para quitar un registro de dominio personalizado, use el comando de la CLI [az storage account update](https://docs.microsoft.com/cli/azure/storage/account) y especifique una cadena vacía (`""`) para el valor del argumento `--custom-domain`.

* Formato de comando:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Ejemplo de comando:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Para quitar un registro de dominio personalizado, use el cmdlet de PowerShell [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) y especifique una cadena vacía (`""`) para el valor del argumento `-CustomDomainName`.

* Formato de comando:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Ejemplo de comando:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Pasos siguientes
* [Asignación de un dominio personalizado a un punto de conexión de Azure Content Delivery Network (CDN)](../../cdn/cdn-map-content-to-custom-domain.md)
* [Uso de Azure CDN para obtener acceso a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md)
* [Hospedaje de sitios web estáticos en Azure Blob Storage (versión preliminar)](storage-blob-static-website.md)
