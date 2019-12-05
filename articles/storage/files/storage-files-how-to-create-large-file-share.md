---
title: Habilitación y creación de recursos compartidos de archivos grandes - Azure Files
description: En este artículo aprenderá a habilitar y crear recursos compartidos de archivos grandes.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 11/20/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 34b8af56a8f2f108b96ca07fa73f90bb9eb5bf13
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2019
ms.locfileid: "74422730"
---
# <a name="how-to-enable-and-create-large-file-shares"></a>Habilitación y creación de recursos compartidos de archivos grandes

Originalmente, los recursos compartidos de archivos estándar solo podían escalar verticalmente hasta 5 TiB; ahora, con los recursos compartidos de archivos grandes, pueden hacerlo hasta 100 TiB. Los recursos compartidos de archivos Premium escalan verticalmente de forma predeterminada hasta 100 TiB. 

Para escalar verticalmente hasta 100 TiB mediante recursos compartidos de archivos estándar, debe habilitar en la cuenta de almacenamiento el uso de recursos compartidos de archivos grandes. Puede habilitar una cuenta existente o crear una nueva para usar recursos compartidos de archivos grandes.

## <a name="prerequisites"></a>Requisitos previos

- Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Si piensa usar la CLI de Azure, asegúrese de [instalar la versión más reciente](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
- Si piensa usar Azure PowerShell, asegúrese de [instalar la versión más reciente](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0).

## <a name="restrictions"></a>Restricciones

Las cuentas habilitadas para recursos compartidos de archivos grandes admiten LRS o ZRS. Por el momento, estas cuentas no admiten GZRS, GRS ni RA-GRS. La habilitación de recursos compartidos de archivos grandes en una cuenta es un proceso irreversible; una vez efectuada, la cuenta no se puede convertir a GZRS, GRS ni RA-GRS.

## <a name="create-a-new-storage-account"></a>Creación de una cuenta de almacenamiento nueva

### <a name="portal"></a>Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. En Azure Portal, seleccione **Todos los servicios**. En la lista de recursos, escriba **Cuentas de almacenamiento**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Cuentas de almacenamiento**.
1. En la ventana **Cuentas de almacenamiento** que aparece, elija **Agregar**.
1. Seleccione la suscripción en la que se va a crear la cuenta de almacenamiento.
1. En el campo **Grupo de recursos**, haga clic en **Crear nuevo**. Escriba un nombre para el nuevo grupo de recursos, como se muestra en la siguiente imagen.

    ![Captura de pantalla que muestra cómo crear un grupo de recursos en el portal](media/storage-files-how-to-create-large-file-share/create-large-file-share.png)

1. Después, escriba un nombre para la cuenta de almacenamiento. El nombre que elija debe ser único en Azure. El nombre debe tener también una longitud de entre 3 y 24 caracteres y solo puede contener números y letras minúsculas.
1. Seleccione una ubicación para la cuenta de almacenamiento, asegúrese de que es [una de las regiones compatibles con LFS](storage-files-planning.md#regional-availability).
1. Establezca la replicación en **Almacenamiento con redundancia local** o en **Almacenamiento con redundancia de zona**.
1. Deje estos campos con sus valores predeterminados:

   |Campo  |Valor  |
   |---------|---------|
   |Modelo de implementación     |Resource Manager         |
   |Rendimiento     |Estándar         |
   |Tipo de cuenta     |StorageV2 (uso general v2)         |
   |Nivel de acceso     |Acceso frecuente         |

1. Seleccione **Avanzado** y después **Habilitado** en **Large file shares** (Recursos compartidos de archivos grandes).
1. Seleccione **Revisar y crear** para revisar la configuración de la cuenta de almacenamiento y crear la cuenta.

    ![large-file-shares-advanced-enable.png](media/storage-files-how-to-create-large-file-share/large-file-shares-advanced-enable.png)

1. Seleccione **Crear**.


### <a name="cli"></a>CLI

En primer lugar, asegúrese de [instalar la versión más reciente](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), para poder habilitar esta característica.

Para crear una cuenta de almacenamiento con recursos compartidos de archivos grandes habilitados, reemplace `<yourStorageAccountName>`, `<yourResourceGroup>` y `<yourDesiredRegion>` por sus valores y, a continuación, use el siguiente comando:

```azurecli-interactive
## This command creates a large file share enabled account, it will not support GZRS, GRS, or RA-GRS
az storage account create –name <yourStorageAccountName> -g <yourResourceGroup> -l <yourDesiredRegion> –sku Standard_LRS --kind StorageV2 –enable-large-file-share
```

### <a name="powershell"></a>PowerShell

En primer lugar, asegúrese de [instalar la versión más reciente](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.0.0), para poder habilitar esta característica.

Para crear una cuenta de almacenamiento con recursos compartidos de archivos grandes habilitados, reemplace `<yourStorageAccountName>`, `<yourResourceGroup>` y `<yourDesiredRegion>` por sus valores y, a continuación, use el siguiente comando:

```PowerShell
## This command creates a large file share enabled account, it will not support GZRS, GRS, or RA-GRS
New-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -Location <yourDesiredRegion> -SkuName Standard_LRS -EnableLargeFileShare;
```

## <a name="enable-on-existing-account"></a>Habilitación en una cuenta existente

### <a name="portal"></a>Portal

También puede habilitar los recursos compartidos de archivos grandes en cuentas existentes. Si lo hace así, la cuenta ya no podrá convertir a GZRS, GRS ni RA-GRS. Esta opción es irreversible en esta cuenta.

1. Abra [Azure Portal](https://portal.azure.com) y navegue hasta la cuenta de almacenamiento en la que desea habilitar los recursos compartidos de archivos grandes.
1. Abra la cuenta de almacenamiento y seleccione **Configuración**.
1. Seleccione **Habilitado** en **Large file shares** (Recursos compartidos de archivos grandes) y, a continuación, seleccione Guardar.
1. Seleccione **Información general** y elija **Actualizar**.

![enable-large-file-shares-on-existing.png](media/storage-files-how-to-create-large-file-share/enable-large-file-shares-on-existing.png)

Ahora ha habilitado los recursos compartidos de archivos grandes en la cuenta de almacenamiento.

Si aparece el siguiente mensaje de error: "Aún no hay recursos compartidos de archivos grandes para la cuenta". Puede esperar un tiempo, ya que es probable que su región esté en pleno proceso de implementación o, si necesita esta opción con urgencia, puede ponerse en contacto con el servicio de soporte técnico.

### <a name="cli"></a>CLI

Puede habilitar los recursos compartidos de archivos grandes en las cuentas existentes. Si lo hace así, la cuenta ya no podrá convertir a GZRS, GRS ni RA-GRS. Esta opción es irreversible en esta cuenta.

Reemplace `<yourStorageAccountName>` y `<yourResourceGroup>` en el siguiente comando y, a continuación, úselo para habilitar los recursos compartidos de archivos grandes en la cuenta existente:

```azurecli-interactive
az storage account update –name <yourStorageAccountName> -g <yourResourceGroup> –enable-large-file-share
```

### <a name="powershell"></a>PowerShell

Puede habilitar los recursos compartidos de archivos grandes en las cuentas existentes. Si lo hace así, la cuenta ya no podrá convertir a GZRS, GRS ni RA-GRS. Esta opción es irreversible en esta cuenta.

Reemplace `<yourStorageAccountName>` y `<yourResourceGroup>` en el siguiente comando y, a continuación, úselo para habilitar los recursos compartidos de archivos grandes en la cuenta existente:

```PowerShell
Set-AzStorageAccount -ResourceGroupName <yourResourceGroup> -Name <yourStorageAccountName> -EnableLargeFileShare
```

## <a name="create-a-large-file-share"></a>Creación de un recurso compartido de archivos grandes

### <a name="portal"></a>Portal

La creación de un recurso compartido de archivos grandes es casi idéntica a la creación de un recurso compartido de archivos estándar. La principal diferencia es que puede establecer una cuota de hasta 100 TiB.

1. En la cuenta de almacenamiento, seleccione **Recurso compartido de archivos**.
1. Seleccione **+ Recurso compartido de archivos**.
1. Escriba un nombre para el recurso compartido de archivos y, si lo desea, el tamaño de cuota —hasta un máximo de 100 TiB—; después, seleccione **Crear**. 

![large-file-shares-create-share.png](media/storage-files-how-to-create-large-file-share/large-file-shares-create-share.png)

### <a name="cli"></a>CLI

Después de habilitar los recursos compartidos de archivos grandes en la cuenta de almacenamiento, puede crear recursos compartidos en esa cuenta con cuotas más altas. Reemplace `<yourStorageAccountName>`, `<yourStorageAccountKey>` y `<yourFileShareName>` por sus valores en el siguiente comando y, a continuación, puede usarlo para crear un recurso compartido de archivos grandes:

```azurecli-interactive
az storage share create --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName>
```

### <a name="powershell"></a>PowerShell

Después de habilitar los recursos compartidos de archivos grandes en la cuenta de almacenamiento, puede crear recursos compartidos en esa cuenta con cuotas más altas. Reemplace `<YourStorageAccountName>`, `<YourStorageAccountKey>` y `<YourStorageAccountFileShareName>` por sus valores en el siguiente comando y, a continuación, puede usarlo para crear un recurso compartido de archivos grandes:

```PowerShell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
New-AzStorageShare -Name $shareName -Context $ctx
```

## <a name="expand-existing-file-shares"></a>Expansión de recursos compartidos de archivos existentes

### <a name="portal"></a>Portal

Después de habilitar los recursos compartidos de archivos grandes en la cuenta de almacenamiento, puede expandir los recursos compartidos existentes a la cuota más alta.

1. En la cuenta de almacenamiento, seleccione **Recurso compartido de archivos**.
1. Haga clic con el botón derecho en el recurso compartido de archivos y seleccione **Cuota**.
1. Escriba el nuevo tamaño que desee y, a continuación, seleccione **Aceptar**.

![update-large-file-share-quota.png](media/storage-files-how-to-create-large-file-share/update-large-file-share-quota.png)

### <a name="cli"></a>CLI

Después de habilitar los recursos compartidos de archivos grandes en la cuenta de almacenamiento, puede expandir los recursos compartidos existentes en la cuenta a las cuotas más altas. Reemplace `<yourStorageAccountName>`, `<yourStorageAccountKey>` y `<yourFileShareName>` por sus valores en el siguiente comando y, a continuación, puede usarlo para establecer la cuota en el tamaño máximo:

```azurecli-interactive
az storage share update --account-name <yourStorageAccountName> --account-key <yourStorageAccountKey> --name <yourFileShareName> --quota 102400
```

### <a name="powershell"></a>PowerShell

Después de habilitar los recursos compartidos de archivos grandes en la cuenta de almacenamiento, puede expandir los recursos compartidos existentes en la cuenta a las cuotas más altas. Reemplace `<YourStorageAccountName>`, `<YourStorageAccountKey>` y `<YourStorageAccountFileShareName>` por sus valores en el siguiente comando y, a continuación, puede usarlo para establecer la cuota en el tamaño máximo:

```PowerShell
##Config
$storageAccountName = "<YourStorageAccountName>"
$storageAccountKey = "<YourStorageAccountKey>"
$shareName="<YourStorageAccountFileShareName>"
$ctx = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
# update quota
Set-AzStorageShareQuota -ShareName $shareName -Context $ctx -Quota 102400
```

## <a name="next-steps"></a>Pasos siguientes

* [Conectar y montar un recurso compartido de archivos - Windows](storage-how-to-use-files-windows.md)
* [Conectar y montar un recurso compartido de archivos - Linux](../storage-how-to-use-files-linux.md)
* [Conectar y montar un recurso compartido de archivos - macOS](storage-how-to-use-files-mac.md)