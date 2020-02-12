---
title: Hospedaje de un sitio web estático en Azure Storage
description: Obtenga información sobre cómo proporcionar contenido estático (HTML, CSS, JavaScript y archivos de imagen) directamente desde un contenedor en una cuenta GPv2 de Azure Storage.
author: normesta
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.author: normesta
ms.date: 05/28/2019
ms.openlocfilehash: 4214c4eb9fbe1d3e39d1ee16289f30b893b94653
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "76906606"
---
# <a name="host-a-static-website-in-azure-storage"></a>Hospedaje de un sitio web estático en Azure Storage

Puede proporcionar contenido estático (HTML, CSS, JavaScript y archivos de imagen) directamente desde un contenedor en una cuenta GPv2 de Azure Storage. Para obtener más información, consulte [Hospedaje de sitios web estáticos en Azure Storage](storage-blob-static-website.md).

En este artículo se le muestra cómo habilitar el hospedaje de sitios web estáticos con Azure Portal, la CLI de Azure o PowerShell.

<a id="portal" />

## <a name="portaltabazure-portal"></a>[Portal](#tab/azure-portal)

Para seguir un tutorial paso a paso, consulte [Tutorial: Hospedaje de un sitio web estático en Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host).

Una vez que habilite el hospedaje de sitios web estáticos, podrá ver las páginas del sitio en un explorador mediante la dirección URL pública del sitio web.

<a id="portal-find-url" />

### <a name="find-the-website-url-by-using-the-azure-portal"></a>Búsqueda de la dirección URL del sitio web mediante Azure Portal

En el panel que aparece junto a la página de información general de la cuenta de su cuenta de almacenamiento, seleccione **Sitio web estático**. La dirección URL del sitio aparece en el campo **Punto de conexión principal**.

![Métrica de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-url.png)

<a id="cli" />

## <a name="azure-clitabazure-cli"></a>[CLI de Azure](#tab/azure-cli)

Puede habilitar el hospedaje de sitios web estáticos mediante la [Interfaz de la línea de comandos (CLI) de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

1. En primer lugar, abra [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest) o, si ha [instalado](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) la CLI de Azure localmente, abra una aplicación de consola de comandos como Windows PowerShell.

2. Si su identidad se asocia a más de una suscripción, establezca su suscripción activa en la suscripción de la cuenta de almacenamiento que hospedará el sitio web estático.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   Reemplace el valor de marcador de posición `<subscription-id>` por el identificador de la suscripción.

3. Habilite el hospedaje de sitios web estáticos.

   ```azurecli-interactive
   az storage blob service-properties update --account-name <storage-account-name> --static-website --404-document <error-document-name> --index-document <index-document-name>
   ```

   * Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

   * Reemplace el marcador de posición `<error-document-name>` por el nombre del documento de error que verán los usuarios cuando un explorador solicite una página en el sitio que no existe.

   * Reemplace el marcador de posición `<index-document-name>` por el nombre del documento de índice. Este documento suele ser "index.html".

4. Cargue los objetos al contenedor *$web* desde un directorio de origen.

   > [!NOTE]
   > Si usa Azure Cloud Shell, asegúrese de agregar un carácter de escape `\` al hacer referencia al contenedor `$web` (por ejemplo: `\$web`). Si usa una instalación local de la CLI de Azure, no tendrá que utilizar el carácter de escape.

   En este ejemplo se da por hecho que ejecuta comandos desde la sesión de Azure Cloud Shell.

   ```azurecli-interactive
   az storage blob upload-batch -s <source-path> -d \$web --account-name <storage-account-name>
   ```

   * Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

   * Reemplace el marcador de posición `<source-path>` por una ruta de acceso a la ubicación de los archivos que desea cargar.

   > [!NOTE]
   > Si usa una instalación de ubicación de la CLI de Azure, puede usar la ruta de acceso a cualquier ubicación en el equipo local (por ejemplo: `C:\myFolder`).
   >
   > Si usa Azure Cloud Shell, tendrá que hacer referencia a un recurso compartido de archivos visible para Cloud Shell. Esta ubicación podría ser el recurso compartido de archivos del propio recurso compartido en la nube o un recurso compartido de archivos existente que monta desde Cloud Shell. Para obtener información sobre cómo hacerlo, consulte [Persistencia de archivos en Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage).

<a id="cli-find-url" />

### <a name="find-the-website-url-by-using-the-azure-cli"></a>Búsqueda de la dirección URL del sitio web mediante la CLI de Azure

Para ver el contenido desde un explorador, use la dirección URL pública del sitio web.

Busque la dirección URL mediante el siguiente comando:

```azurecli-interactive
az storage account show -n <storage-account-name> -g <resource-group-name> --query "primaryEndpoints.web" --output tsv
```

* Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

* Reemplace el marcador de posición `<resource-group-name>` por el nombre del grupo de recursos.

<a id="powershell" />

## <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Puede habilitar el hospedaje de sitios web estáticos con el módulo de Azure PowerShell.

1. Abra una ventana de comando de Windows PowerShell.

2. Compruebe que tiene la versión 0.7 o posterior del módulo Az de Azure PowerShell.

   ```powershell
   Get-InstalledModule -Name Az -AllVersions | select Name,Version
   ```

   Si necesita instalarla o actualizarla, consulte el artículo sobre [cómo instalar el módulo de Azure PowerShell](/powershell/azure/install-Az-ps).

3. Inicie sesión en la suscripción a Azure con el comando `Connect-AzAccount` y siga las instrucciones de la pantalla.

   ```powershell
   Connect-AzAccount
   ```

4. Si su identidad se asocia a más de una suscripción, establezca su suscripción activa en la suscripción de la cuenta de almacenamiento que hospedará el sitio web estático.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   Reemplace el valor de marcador de posición `<subscription-id>` por el identificador de la suscripción.

5. Obtenga el contexto de la cuenta de almacenamiento que define la cuenta de almacenamiento que desea usar.

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * Reemplace el marcador de posición `<resource-group-name>` por el nombre del grupo de recursos.

   * Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

6. Habilite el hospedaje de sitios web estáticos.

   ```powershell
   Enable-AzStorageStaticWebsite -Context $ctx -IndexDocument <index-document-name> -ErrorDocument404Path <error-document-name>
   ```

   * Reemplace el marcador de posición `<error-document-name>` por el nombre del documento de error que verán los usuarios cuando un explorador solicite una página en el sitio que no existe.

   * Reemplace el marcador de posición `<index-document-name>` por el nombre del documento de índice. Este documento suele ser "index.html".

7. Cargue los objetos al contenedor *$web* desde un directorio de origen.

    ```powershell
    # upload a file
    set-AzStorageblobcontent -File "<path-to-file>" `
    -Container `$web `
    -Blob "<blob-name>" `
    -Context $ctx
     ```

   * Reemplace el valor de marcador de posición `<path-to-file>` por la ruta de acceso absoluta al archivo que desea cargar (por ejemplo: `C:\temp\index.html`).

   * Reemplace el valor de marcador de posición `<blob-name>` por el nombre que desea dar al blob resultante (por ejemplo: `index.html`).

<a id="powershell-find-url" />

### <a name="find-the-website-url-by-using-powershell"></a>Búsqueda de la dirección URL del sitio web mediante PowerShell

Para ver el contenido desde un explorador, use la dirección URL pública del sitio web.

Busque la dirección URL mediante el siguiente comando:

```powershell
 $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -Name "<storage-account-name>"
Write-Output $storageAccount.PrimaryEndpoints.Web
```

* Reemplace el marcador de posición `<resource-group-name>` por el nombre del grupo de recursos.

* Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

<a id="metrics" />

---

## <a name="enable-metrics-on-static-website-pages"></a>Habilitación de métricas en páginas del sitio web estático

Después de habilitar las métricas, las estadísticas de tráfico de los archivos en el contenedor **$web** aparecen en el panel de métricas.

1. Haga clic en **Configuración** > **Supervisión** > **Métricas**.

   Los datos de las métricas se generan al enlazarse en diferentes API de métricas. El portal solo muestra los miembros de API que se usan en un período de tiempo determinado, para centrarse únicamente en los miembros que devuelven datos. Para asegurarse de que puede seleccionar el miembro de API necesario, el primer paso es expandir el período de tiempo.

2. Haga clic en el botón de marco de tiempo y seleccione **Últimas 24 horas** y luego haga clic en **Aplicar**.

   ![Intervalo de tiempo de las métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

3. A continuación, seleccione **Blob** en el menú desplegable *Espacio de nombres*.

   ![Espacio de nombres de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

4. A continuación, seleccione la métrica **Egress**.

   ![Métrica de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

5. Seleccione **Sum** del selector *Aggregation* (Agregación).

   ![Agregación de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

6. Haga clic en el botón **Agregar filtro** y elija **Nombre de API** en el selector *Propiedad*.

   ![Nombre de API de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

7. Active la casilla junto a **GetWebContent** en el selector *Valores* para completar el informe de métricas.

   ![GetWebContent de métricas de sitios web de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a configurar un dominio personalizado con su sitio web estático. Consulte [Asignación de un dominio personalizado a un punto de conexión de Azure Blob Storage](storage-custom-domain-name.md).

