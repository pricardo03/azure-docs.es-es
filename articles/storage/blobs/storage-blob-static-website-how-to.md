---
title: Hospedar un sitio Web estático en el almacenamiento de Azure
description: Aprenda a servir el contenido estático (HTML, CSS, JavaScript y los archivos de imagen) directamente desde un contenedor en una cuenta de GPv2 de almacenamiento de Azure.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.author: normesta
ms.date: 05/28/2019
ms.openlocfilehash: 7a1aef14a2a32266c893933482527c361f17d7fb
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66428566"
---
# <a name="host-a-static-website-in-azure-storage"></a>Hospedar un sitio Web estático en el almacenamiento de Azure

Puede servir contenido estático (HTML, CSS, JavaScript y los archivos de imagen) directamente desde un contenedor en una cuenta de GPv2 de almacenamiento de Azure. Para obtener más información, consulte [hospedaje del sitio Web estático en Azure Storage](storage-blob-static-website.md).

Este artículo muestra cómo habilitar el hospedaje del sitio Web estático mediante el uso de Azure portal, PowerShell o la CLI de Azure.

<a id="portal" />

## <a name="use-the-azure-portal"></a>Uso de Azure Portal

Para ver un tutorial paso a paso, consulte [Tutorial: Hospedaje de un sitio web estático en Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host).

Después de habilitar el hospedaje de sitios Web estático, puede ver las páginas del sitio desde un explorador mediante la dirección URL pública del sitio Web.

<a id="portal-find-url" />

### <a name="find-the-website-url-by-using-the-azure-portal"></a>Buscar la dirección URL del sitio Web mediante el portal de Azure

En el panel que aparece junto a la página de información general de la cuenta de la cuenta de almacenamiento, seleccione **sitio Web estático**. La dirección URL del sitio aparece en el **punto de conexión principal** campo.

![Métrica de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-url.png)

<a id="cli" />

## <a name="use-the-azure-cli"></a>Uso de la CLI de Azure

Puede habilitar el hospedaje del sitio Web estático mediante el uso de la [interfaz de línea de comandos (CLI) de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest).

1. En primer lugar, abra el [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest), o si ha [instalado](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) la CLI de Azure localmente, abra una aplicación de consola de comandos, como Windows PowerShell.

2. En la ventana de comandos que ya ha abierto, instale la extensión de la versión preliminar de almacenamiento.

   ```azurecli-interactive
   az extension add --name storage-preview
   ```

3. Si la identidad está asociada a más de una suscripción, a continuación, establezca su activo una suscripción a otra de la cuenta de almacenamiento que se va a hospedar el sitio Web estático.

   ```azurecli-interactive
   az account set --subscription <subscription-id>
   ```

   Reemplace el `<subscription-id>` valor de marcador de posición con el identificador de la suscripción.

4. Habilitar el hospedaje del sitio Web estático.

   ```azurecli-interactive
   az storage blob service-properties update --account-name <storage-account-name> --static-website --404-document <error-document-name> --index-document <index-document-name>
   ```

   * Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

   * Reemplace el `<error-document-name>` marcador de posición con el nombre del documento de error que aparecerá para los usuarios cuando un explorador solicita una página en el sitio que no existe.

   * Reemplace el `<index-document-name>` marcador de posición con el nombre del documento de índice. Este documento suele ser "index.html".

5. Cargue los objetos al contenedor *$web* desde un directorio de origen.

   > [!NOTE]
   > Si usa Azure Cloud Shell, asegúrese de agregar un `\` carácter de escape cuando se hace referencia a la `$web` contenedor (por ejemplo: `\$web`). Si usa una instalación local de la CLI de Azure, no tendrá que utilizar el carácter de escape.

   En este ejemplo se da por supuesto que está ejecutando los comandos de la sesión de Azure Cloud Shell.

   ```azurecli-interactive
   az storage blob upload-batch -s <source-path> -d \$web --account-name <storage-account-name>
   ```

   * Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

   * Reemplace el `<source-path>` marcador de posición con una ruta de acceso a la ubicación de los archivos que desea cargar.

   > [!NOTE]
   > Si usa una instalación de ubicación de la CLI de Azure, puede usar la ruta de acceso a cualquier ubicación en el equipo local (por ejemplo: `C:\myFolder`.
   >
   > Si usa Azure Cloud Shell, deberá hacer referencia a un recurso compartido de archivos que sea visible para el Shell en la nube. Esta ubicación podría ser el recurso compartido de archivos de la nube comparten sí mismo o a un recurso compartido archivos existentes que se monta en Cloud Shell. Para obtener información sobre cómo hacerlo, consulte [conservar los archivos en Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage).

<a id="cli-find-url" />

### <a name="find-the-website-url-by-using-the-azure-cli"></a>Buscar la dirección URL del sitio Web mediante la CLI de Azure

Puede ver el contenido desde un explorador utilizando la dirección URL pública del sitio Web.

Buscar la dirección URL mediante el comando siguiente:

```azurecli-interactive
az storage account show -n <storage-account-name> -g <resource-group-name> --query "primaryEndpoints.web" --output tsv
```

* Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

* Reemplace el `<resource-group-name>` valor de marcador de posición con el nombre del grupo de recursos.

<a id="powershell" />

## <a name="use-powershell"></a>Uso de PowerShell

Puede habilitar el hospedaje del sitio Web estático con el módulo de PowerShell de Azure.

1. Abra una ventana de comandos de Windows PowerShell.

2. Compruebe que dispone de Azure PowerShell versión del módulo Az 0,7 o posterior.

   ```powershell
   Get-InstalledModule -Name Az -AllVersions | select Name,Version
   ```

   Si necesita instalarla o actualizarla, consulte el artículo sobre [cómo instalar el módulo de Azure PowerShell](/powershell/azure/install-Az-ps).

3. Inicie sesión en la suscripción a Azure con el comando `Connect-AzAccount` y siga las instrucciones de la pantalla.

   ```powershell
   Connect-AzAccount
   ```

4. Si la identidad está asociada a más de una suscripción, a continuación, establezca su activo una suscripción a otra de la cuenta de almacenamiento que se va a hospedar el sitio Web estático.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```

   Reemplace el `<subscription-id>` valor de marcador de posición con el identificador de la suscripción.

5. Obtiene el contexto de la cuenta de almacenamiento que define la cuenta de almacenamiento que desea usar.

   ```powershell
   $storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
   $ctx = $storageAccount.Context
   ```

   * Reemplace el `<resource-group-name>` valor de marcador de posición con el nombre del grupo de recursos.

   * Reemplace el valor de marcador de posición `<storage-account-name>` por el nombre de la cuenta de almacenamiento.

6. Habilitar el hospedaje del sitio Web estático.

   ```powershell
   Enable-AzStorageStaticWebsite -Context $ctx -IndexDocument <index-document-name> -ErrorDocument404Path <error-document-name>
   ```

   * Reemplace el `<error-document-name>` marcador de posición con el nombre del documento de error que aparecerá para los usuarios cuando un explorador solicita una página en el sitio que no existe.

   * Reemplace el `<index-document-name>` marcador de posición con el nombre del documento de índice. Este documento suele ser "index.html".

7. Cargue los objetos al contenedor *$web* desde un directorio de origen.

    ```powershell
    # upload a file
    set-AzStorageblobcontent -File "<path-to-file>" `
    -Container `$web `
    -Blob "<blob-name>" `
    -Context $ctx
     ```

   * Reemplace el `<path-to-file>` valor de marcador de posición con la ruta de acceso completa al archivo que desea cargar (por ejemplo: `C:\temp\index.html`).

   * Reemplace el `<blob-name>` valor de marcador de posición con el nombre que desea dar al blob resultante (por ejemplo: `index.html`).

<a id="powershell-find-url" />

### <a name="find-the-website-url-by-using-powershell"></a>Buscar la dirección URL del sitio Web mediante PowerShell

Puede ver el contenido desde un explorador utilizando la dirección URL pública del sitio Web.

Buscar la dirección URL mediante el comando siguiente:

```powershell
$context = Get-AzSubscription -SubscriptionId <subscription-d>
Set-AzContext $context
```

Reemplace el `<subscription-id>` valor de marcador de posición con el identificador de la suscripción.

<a id="metrics" />

## <a name="enable-metrics-on-static-website-pages"></a>Habilitar las métricas en las páginas del sitio Web estático

Una vez que ha habilitado las métricas, tráfico de las estadísticas de archivos en el **$web** contenedor se muestran en el panel de métricas.

1. Haga clic en **configuración** > **supervisión** > **métricas**.

   Los datos de las métricas se generan al enlazarse en diferentes API de métricas. El portal solo muestra los miembros de API que se usan en un período de tiempo determinado, para centrarse únicamente en los miembros que devuelven datos. Con el fin de asegurarse de que pueda seleccionar al miembro de la API necesario, el primer paso es expandir el intervalo de tiempo.

2. Haga clic en el botón de período de tiempo y seleccione **últimas 24 horas** y, a continuación, haga clic en **aplicar**.

   ![Intervalo de tiempo de las métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

3. Seleccione **Blob** desde el *Namespace* lista desplegable.

   ![Espacio de nombres de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

4. A continuación, seleccione la métrica **Egress**.

   ![Métrica de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

5. Seleccione **Sum** del selector *Aggregation* (Agregación).

   ![Agregación de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

6. Haga clic en el **Agregar filtro** botón y elija **nombre de la API** desde el *propiedad* selector.

   ![Nombre de API de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

7. la casilla de verificación junto a **GetWebContent** en el *valores* selector para rellenar el informe de métricas.

   ![GetWebContent de métricas de sitios web de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

## <a name="next-steps"></a>Pasos siguientes

* [Hospedaje de sitios web estáticos en Azure Storage](storage-blob-static-website.md)
* [Usar la red CDN de Azure para acceder a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md)
* [Configurar un nombre de dominio personalizado para el punto de conexión web o de blob](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [Crear la primera aplicación web sin servidor](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Tutorial: Hospedaje del dominio en Azure DNS](../../dns/dns-delegate-domain-azure-dns.md)
