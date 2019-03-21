---
title: Registro de Azure Key Vault | Microsoft Docs
description: Use este tutorial para tener ayuda para empezar a trabajar con el registro de Azure Key Vault.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: 43f96a2b-3af8-4adc-9344-bc6041fface8
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: afec42551f124890dd2cc7b03cce48c359fc88c4
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57194102"
---
# <a name="azure-key-vault-logging"></a>Registro de Azure Key Vault

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Después de crear uno o varios almacenes de claves, probablemente le interese supervisar cómo y cuándo los almacenes de claves son acceder y por quién. Puede hacerlo al habilitar el registro para Azure Key Vault, que guarda la información en una cuenta de almacenamiento de Azure que proporcione. Un nuevo contenedor denominado **insights-logs-auditevent** se crea automáticamente para su cuenta de almacenamiento especificada. Puede usar esta misma cuenta de almacenamiento para recopilar registros de varios almacenes de claves.

Se puede tener acceso a la información de registro 10 minutos (como máximo) después de la operación de almacén de claves. En la mayoría de los casos, será más rápido que esto.  Es decisión suya administrar los registros en la cuenta de almacenamiento:

* Utilice los métodos de control de acceso de Azure estándar para proteger los registros mediante la restricción de quién puede tener acceso a ellos.
* Elimine los registros que ya no desee mantener en la cuenta de almacenamiento.

Use este tutorial para tener ayuda para empezar a trabajar con el registro de Azure Key Vault. Deberá crear una cuenta de almacenamiento, habilitar el registro e interpretar la información de registro recopilados.  

> [!NOTE]
> Este tutorial no incluye instrucciones sobre cómo crear almacenes de claves, claves o secretos. Para obtener más información, consulte [¿Qué es Azure Key Vault?](key-vault-overview.md). O bien, para obtener instrucciones de CLI de Azure multiplataforma, consulte [este tutorial equivalente](key-vault-manage-with-cli2.md).
>
> Este artículo proporciona instrucciones de Azure PowerShell para actualizar el registro de diagnóstico. También puede actualizar el registro de diagnóstico mediante el uso de Azure Monitor en el **los registros de diagnóstico** sección del portal de Azure. 
>

Para obtener información general sobre Key Vault, consulte [¿qué es Azure Key Vault?](key-vault-whatis.md). Para obtener información acerca de dónde Key Vault está disponible, consulte el [página de precios](https://azure.microsoft.com/pricing/details/key-vault/).

## <a name="prerequisites"></a>Requisitos previos

Para realizar este tutorial, necesitará lo siguiente:

* Un Almacén de claves existente que ha utilizado.  
* Azure PowerShell, versión mínima de 1.0.0. Para instalar Azure PowerShell y asociarlo con una suscripción de Azure, consulte [Instalación y configuración de Azure PowerShell](/powershell/azure/overview). Si ya ha instalado Azure PowerShell y no sabe la versión, desde la consola de Azure PowerShell, escriba `$PSVersionTable.PSVersion`.  
* Suficiente almacenamiento en Azure para sus registros de Key Vault.

## <a id="connect"></a>Conéctese a su suscripción de almacén de claves

El primer paso para configurar el registro de clave está a punto de Azure PowerShell al almacén de claves que se desea iniciar sesión.

Inicie una sesión de Azure PowerShell e inicie sesión en su cuenta de Azure con el siguiente comando:  

```PowerShell
Connect-AzAccount
```

En la ventana emergente del explorador, escriba el nombre de usuario y la contraseña de su cuenta de Azure. Azure PowerShell Obtiene todas las suscripciones que están asociadas con esta cuenta. De forma predeterminada, PowerShell usa la primera de ellas.

Es posible que deba especificar la suscripción que usó para crear el almacén de claves. Escriba el siguiente comando para ver las suscripciones de su cuenta:

```PowerShell
Get-AzSubscription
```

A continuación, para especificar la suscripción que está asociado con el almacén de claves que se iniciará sesión, escriba:

```PowerShell
Set-AzContext -SubscriptionId <subscription ID>
```

Señalando PowerShell a la suscripción adecuada es un paso importante, especialmente si tiene varias suscripciones asociadas con su cuenta. Para obtener más información sobre cómo configurar PowerShell de Azure, consulte [Instalación y configuración de PowerShell de Azure](/powershell/azure/overview).

## <a id="storage"></a>Crear una cuenta de almacenamiento para los registros

Aunque puede usar una cuenta de almacenamiento para sus registros, vamos a crear una cuenta de almacenamiento que se dedicará a los registros de Key Vault. Por comodidad para cuando tengamos que especificarlos más adelante, almacenaremos los detalles en una variable denominada **sa**.

Para mayor facilidad de administración, también vamos a usar el mismo grupo de recursos que contiene el almacén de claves. Desde el [tutorial de introducción](key-vault-get-started.md), este grupo de recursos se denomina **ContosoResourceGroup**, y vamos a seguir usar la ubicación Asia oriental. Reemplace estos valores por los suyos propios, según corresponda:

```PowerShell
 $sa = New-AzStorageAccount -ResourceGroupName ContosoResourceGroup -Name contosokeyvaultlogs -Type Standard_LRS -Location 'East Asia'
```

> [!NOTE]
> Si decide usar una cuenta de almacenamiento existente, debe usar la misma suscripción como el almacén de claves. Y debe usar el modelo de implementación de Azure Resource Manager, en lugar de modelo de implementación clásica.
>
>

## <a id="identify"></a>Identificación del Almacén de claves para los registros

En el [tutorial de introducción](key-vault-get-started.md), el nombre del almacén de claves era **ContosoKeyVault**. Vamos a seguir usando ese nombre y almacenar los detalles en una variable denominada **kv**:

```PowerShell
$kv = Get-AzKeyVault -VaultName 'ContosoKeyVault'
```

## <a id="enable"></a>Habilitación del registro

Para habilitar el registro para Key Vault, usaremos el **conjunto AzDiagnosticSetting** cmdlet, junto con las variables que hemos creado para la nueva cuenta de almacenamiento y el almacén de claves. También estableceremos la **-habilitado** marca **$true** y establece la categoría en **AuditEvent** (la única categoría para el registro de Key Vault):

```PowerShell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

El resultado tendrá un aspecto similar al siguiente:

    StorageAccountId   : /subscriptions/<subscription-GUID>/resourceGroups/ContosoResourceGroup/providers/Microsoft.Storage/storageAccounts/ContosoKeyVaultLogs
    ServiceBusRuleId   :
    StorageAccountName :
        Logs
        Enabled           : True
        Category          : AuditEvent
        RetentionPolicy
        Enabled : False
        Days    : 0

Este resultado confirma que el registro está habilitado ahora para el almacén de claves y guardará la información a la cuenta de almacenamiento.

Si lo desea, puede establecer una directiva de retención para los registros de forma que los registros más antiguos se eliminan automáticamente. Por ejemplo, establecer la directiva de retención estableciendo la **- RetentionEnabled** marca **$true**y establezca el **- RetentionInDays** parámetro **90**para que los registros de más de 90 días se eliminan automáticamente.

```PowerShell
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent -RetentionEnabled $true -RetentionInDays 90
```

Qué se registra:

* Todos los autentican las solicitudes de API de REST, incluidas las solicitudes con error como resultado de los permisos de acceso, errores del sistema o solicitudes incorrectas.
* Las operaciones en la clave del almacén de sí mismo, incluida la creación, eliminación y establecimiento de directivas de acceso de almacén de claves y actualizar los atributos del almacén de claves como las etiquetas.
* Operaciones en claves y secretos del almacén de claves, incluidos:
  * Crear, modificar o eliminar estas claves o secretos.
  * Firma, comprobar, cifrar, descifrar, encapsular y desencapsular claves, obtener secretos y lista de claves y secretos (y sus versiones).
* Solicitudes no autenticadas que dan como resultado una respuesta 401. Algunos ejemplos son las solicitudes que no tienen un token de portador, que son un formato incorrecto o ha expirado o que tienen un token no válido.  

## <a id="access"></a>Acceso a los registros

Registros de Key Vault se almacenan en el **insights-logs-auditevent** contenedor en la cuenta de almacenamiento que ha proporcionado. Para ver los registros, tendrá que descargar blobs.

En primer lugar, cree una variable para el nombre del contenedor. Usará esta variable en el resto del tutorial.

```PowerShell
$container = 'insights-logs-auditevent'
```

Para obtener una lista de todos los blobs de este contenedor, escriba:

```PowerShell
Get-AzStorageBlob -Container $container -Context $sa.Context
```

La salida es similar a esta:

```
Container Uri: https://contosokeyvaultlogs.blob.core.windows.net/insights-logs-auditevent

Name

- - -
resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=05/h=01/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=02/m=00/PT1H.json

resourceId=/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSORESOURCEGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT/y=2016/m=01/d=04/h=18/m=00/PT1H.json
```

Como puede ver en esta salida, los blobs siguen una convención de nomenclatura: `resourceId=<ARM resource ID>/y=<year>/m=<month>/d=<day of month>/h=<hour>/m=<minute>/filename.json`

Los valores de fecha y hora usan UTC.

Dado que puede usar la misma cuenta de almacenamiento para recopilar registros de varios recursos, es útil para acceder o descargar solo los blobs que necesita el identificador de recurso completo en el nombre del blob. Pero antes de hacerlo, primero explicaremos cómo descargar todos los blobs.

Cree una carpeta para descargar los blobs. Por ejemplo: 

```PowerShell 
New-Item -Path 'C:\Users\username\ContosoKeyVaultLogs' -ItemType Directory -Force
```

A continuación, obtenga una lista de todos los blobs:  

```PowerShell
$blobs = Get-AzStorageBlob -Container $container -Context $sa.Context
```

Canalice esta lista mediante **Get AzStorageBlobContent** para descargar los blobs en la carpeta de destino:

```PowerShell
$blobs | Get-AzStorageBlobContent -Destination C:\Users\username\ContosoKeyVaultLogs'
```

Al ejecutar este segundo comando, el delimitador **/** de los nombres de los blobs crea una estructura de carpeta completa en la carpeta de destino. Esta estructura se usará para descargar y almacenar los blobs como archivos.

Para descargar blobs de forma selectiva, utilice caracteres comodín. Por ejemplo: 

* Si tiene varios almacenes de claves y desea descargar los registros solo para un Almacén de claves, denominado CONTOSOKEYVAULT3:

  ```PowerShell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/VAULTS/CONTOSOKEYVAULT3
  ```

* Si tiene varios grupos de recursos y desea descargar los registros solo de un grupo de recursos específico, use `-Blob '*/RESOURCEGROUPS/<resource group name>/*'`:

  ```PowerShell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/RESOURCEGROUPS/CONTOSORESOURCEGROUP3/*'
  ```

* Si desea descargar todos los registros del mes de enero de 2019, utilice `-Blob '*/year=2019/m=01/*'`:

  ```PowerShell
  Get-AzStorageBlob -Container $container -Context $sa.Context -Blob '*/year=2016/m=01/*'
  ```

Ahora está listo para comenzar a ver lo que está en los registros. Pero antes de pasar a la, debe saber más de dos comandos:

* Para consultar el estado de la configuración del diagnóstico del recurso del almacén de claves: `Get-AzDiagnosticSetting -ResourceId $kv.ResourceId`
* Para deshabilitar el registro del recurso del almacén de claves: `Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $false -Category AuditEvent`

## <a id="interpret"></a>Interpretación de los registros de Key Vault

Los blobs individuales se almacenan como texto, con formato de blob JSON. Echemos un vistazo a una entrada de registro de ejemplo. Ejecute este comando:

```PowerShell
Get-AzKeyVault -VaultName 'contosokeyvault'`
```

Devuelve una entrada de registro similar a ésta:

```json
    {
        "records":
        [
            {
                "time": "2016-01-05T01:32:01.2691226Z",
                "resourceId": "/SUBSCRIPTIONS/361DA5D4-A47A-4C79-AFDD-XXXXXXXXXXXX/RESOURCEGROUPS/CONTOSOGROUP/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/CONTOSOKEYVAULT",
                "operationName": "VaultGet",
                "operationVersion": "2015-06-01",
                "category": "AuditEvent",
                "resultType": "Success",
                "resultSignature": "OK",
                "resultDescription": "",
                "durationMs": "78",
                "callerIpAddress": "104.40.82.76",
                "correlationId": "",
                "identity": {"claim":{"http://schemas.microsoft.com/identity/claims/objectidentifier":"d9da5048-2737-4770-bd64-XXXXXXXXXXXX","http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn":"live.com#username@outlook.com","appid":"1950a258-227b-4e31-a9cf-XXXXXXXXXXXX"}},
                "properties": {"clientInfo":"azure-resource-manager/2.0","requestUri":"https://control-prod-wus.vaultcore.azure.net/subscriptions/361da5d4-a47a-4c79-afdd-XXXXXXXXXXXX/resourcegroups/contosoresourcegroup/providers/Microsoft.KeyVault/vaults/contosokeyvault?api-version=2015-06-01","id":"https://contosokeyvault.vault.azure.net/","httpStatusCode":200}
            }
        ]
    }
```

En la tabla siguiente se enumera los nombres de campo y descripciones:

| Nombre del campo | DESCRIPCIÓN |
| --- | --- |
| **time** |Fecha y hora en UTC. |
| **resourceId** |Identificador de recursos de Azure Resource Manager. Para los registros de Key Vault, siempre es el identificador de recurso de Key Vault. |
| **operationName** |Nombre de la operación, como se documenta en la tabla siguiente. |
| **operationVersion** |Versión de API de REST solicitada por el cliente. |
| **category** |Tipo de resultado. Para los registros de Key Vault, **AuditEvent** es el único valor disponible. |
| **resultType** |Resultado de la solicitud de API de REST. |
| **resultSignature** |Estado de HTTP |
| **resultDescription** |Descripción adicional acerca del resultado, cuando esté disponible. |
| **durationMs** |Tiempo que tardó en atender la solicitud de API de REST, en milisegundos. Esto no incluye la latencia de red, por lo que el tiempo que se mide en el cliente podría no coincidir con este tiempo. |
| **callerIpAddress** |Dirección IP del cliente que realizó la solicitud. |
| **correlationId** |Un GUID opcional que el cliente puede pasar para correlacionar los registros del lado cliente con los registros del lado servicio (Key Vault). |
| **identity** |Identidad del token que se ha presentado en la solicitud de API de REST. Esto suele ser un "usuario", "entidad de servicio" o la combinación de "usuario + appId," en el caso de una solicitud que se origina un cmdlet de PowerShell de Azure. |
| **properties** |Información que varía en función de la operación (**operationName**). En la mayoría de los casos, este campo contiene información de cliente (la cadena de agente usuario pasada por el cliente), el URI de solicitud de API de REST exacta y el código de estado HTTP. Además, cuando se devuelve un objeto como resultado de una solicitud (por ejemplo, **KeyCreate** o **VaultGet**), también contiene la clave de identificador URI o URI del secreto del almacén de URI (como "id"). |

El **operationName** son los valores de campo en *ObjectVerb* formato. Por ejemplo: 

* Todas las operaciones de almacén de claves tienen el `Vault<action>` dar formato, como `VaultGet` y `VaultCreate`.
* Todas las operaciones claves tienen el `Key<action>` dar formato, como `KeySign` y `KeyList`.
* Todas las operaciones de secreto tienen el `Secret<action>` dar formato, como `SecretGet` y `SecretListVersions`.

La siguiente tabla se enumeran los **operationName** valores y comandos de la API de REST correspondientes:

| operationName | Comando de API de REST |
| --- | --- |
| **Autenticación** |Autenticar a través del punto de conexión de Azure Active Directory |
| **VaultGet** |[Obtener información acerca de un almacén de claves](https://msdn.microsoft.com/library/azure/mt620026.aspx) |
| **VaultPut** |[Crear o actualizar un almacén de claves](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultDelete** |[Eliminar un almacén de claves](https://msdn.microsoft.com/library/azure/mt620022.aspx) |
| **VaultPatch** |[Crear o actualizar un almacén de claves](https://msdn.microsoft.com/library/azure/mt620025.aspx) |
| **VaultList** |[Lista de todos los almacenes de claves en un grupo de recursos](https://msdn.microsoft.com/library/azure/mt620027.aspx) |
| **KeyCreate** |[Crear una clave](https://msdn.microsoft.com/library/azure/dn903634.aspx) |
| **KeyGet** |[Obtener información sobre una clave](https://msdn.microsoft.com/library/azure/dn878080.aspx) |
| **KeyImport** |[Importar una clave a un almacén](https://msdn.microsoft.com/library/azure/dn903626.aspx) |
| **KeyBackup** |[Realizar una copia de seguridad de una clave](https://msdn.microsoft.com/library/azure/dn878058.aspx) |
| **KeyDelete** |[Eliminar una clave](https://msdn.microsoft.com/library/azure/dn903611.aspx) |
| **KeyRestore** |[Restauración de una clave](https://msdn.microsoft.com/library/azure/dn878106.aspx) |
| **KeySign** |[Firmar con una clave](https://msdn.microsoft.com/library/azure/dn878096.aspx) |
| **KeyVerify** |[Comprobar con una clave](https://msdn.microsoft.com/library/azure/dn878082.aspx) |
| **KeyWrap** |[Encapsular una clave](https://msdn.microsoft.com/library/azure/dn878066.aspx) |
| **KeyUnwrap** |[Desencapsular una clave](https://msdn.microsoft.com/library/azure/dn878079.aspx) |
| **KeyEncrypt** |[Cifrar con una clave](https://msdn.microsoft.com/library/azure/dn878060.aspx) |
| **KeyDecrypt** |[Descifrado con una clave](https://msdn.microsoft.com/library/azure/dn878097.aspx) |
| **KeyUpdate** |[Actualizar una clave](https://msdn.microsoft.com/library/azure/dn903616.aspx) |
| **KeyList** |[Enumeración de las claves en un almacén](https://msdn.microsoft.com/library/azure/dn903629.aspx) |
| **KeyListVersions** |[Enumerar las versiones de una clave](https://msdn.microsoft.com/library/azure/dn986822.aspx) |
| **SecretSet** |[Crear un secreto](https://msdn.microsoft.com/library/azure/dn903618.aspx) |
| **SecretGet** |[Obtener un secreto](https://msdn.microsoft.com/library/azure/dn903633.aspx) |
| **SecretUpdate** |[Actualizar un secreto](https://msdn.microsoft.com/library/azure/dn986818.aspx) |
| **SecretDelete** |[Eliminar un secreto](https://msdn.microsoft.com/library/azure/dn903613.aspx) |
| **SecretList** |[Enumerar secretos en un almacén](https://msdn.microsoft.com/library/azure/dn903614.aspx) |
| **SecretListVersions** |[Enumerar versiones de un secreto](https://msdn.microsoft.com/library/azure/dn986824.aspx) |

## <a id="loganalytics"></a>Usar registros de Azure Monitor

Puede usar la solución Key Vault en los registros de Azure Monitor para revisar el almacén de claves **AuditEvent** registros. En los registros de Azure Monitor, las consultas de registro se usan para analizar los datos y obtener la información que necesita. 

Para obtener más información, incluida la forma de configurar esta opción, consulte [solución Azure Key Vault en los registros de Azure Monitor](../azure-monitor/insights/azure-key-vault.md). En este artículo también contiene instrucciones si necesita migrar desde la solución Key Vault antigua que se ofrecieron durante los registros de Azure Monitor, una vista previa, donde se enrutan primero los registros a una cuenta de almacenamiento de Azure y los registros de Monitor de Azure configurado para leer desde allí.

## <a id="next"></a>Pasos siguientes

Para ver un tutorial que utiliza Azure Key Vault en una aplicación web. NET, consulte [uso de Azure Key Vault desde una aplicación web](tutorial-net-create-vault-azure-web-app.md).

Para conocer las referencias de programación, consulte la [Guía del desarrollador de Azure Key Vault](key-vault-developers-guide.md).

Para obtener una lista de cmdlets de PowerShell de Azure 1.0 para Azure Key Vault, consulte [cmdlets de Azure Key Vault](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault).

Para un tutorial sobre la rotación de claves y auditoría de registros con Azure Key Vault, consulte [configurar Key Vault con rotación de claves-to-end y auditoría](key-vault-key-rotation-log-monitoring.md).
