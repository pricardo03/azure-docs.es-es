---
title: Hospedaje de sitios web estáticos en Azure Storage
description: Azure Storage le ofrece hospedaje de sitios web estáticos, lo que le proporciona una solución rentable y escalable para hospedar aplicaciones web modernas.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.author: normesta
ms.reviewer: seguler
ms.date: 05/29/2019
ms.subservice: blobs
ms.openlocfilehash: 36cc8cebdb567cb9650ad1ad3baf72a0b5478247
ms.sourcegitcommit: ef06b169f96297396fc24d97ac4223cabcf9ac33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66427963"
---
# <a name="static-website-hosting-in-azure-storage"></a>Hospedaje de sitios web estáticos en Azure Storage

Puede servir contenido estático (HTML, CSS, JavaScript y los archivos de imagen) directamente desde un contenedor de almacenamiento denominado *$web*. Hospedar el contenido en el almacenamiento de Azure le permite usar arquitecturas sin servidor que incluyen [Azure Functions](/azure/azure-functions/functions-overview) y otros servicios de plataforma como servicio (PaaS).

> [!NOTE]
> Si su sitio depende del código del lado servidor, utilice [Azure App Service](/azure/app-service/overview) en su lugar.

## <a name="setting-up-a-static-website"></a>Configurar un sitio Web estático

Hospedaje del sitio Web estático es una característica que se debe habilitar en la cuenta de almacenamiento.

Para habilitar el hospedaje de sitios Web estático, seleccione el nombre del archivo de forma predeterminada y, a continuación, opcionalmente, proporcione una ruta de acceso a una página 404 personalizada. Si un contenedor de almacenamiento de blobs denominado **$web** ya no existe en la cuenta, se crea uno automáticamente. Agregue los archivos de su sitio para este contenedor.

Para obtener instrucciones detalladas, consulte [hospedar un sitio Web estático en Azure Storage](storage-blob-static-website-how-to.md).

![Métrica de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-blob-container.png)

Archivos en el **$web** contenedor distinguen mayúsculas de minúsculas, entregados a través de las solicitudes de acceso anónimo y están disponibles solo a través de las operaciones de lectura.

## <a name="uploading-content"></a>Carga de contenido

Puede usar cualquiera de estas herramientas para cargar contenido en el **$web** contenedor:

> [!div class="checklist"]
> * [CLI de Azure](storage-blob-static-website-how-to.md#cli)
> * [Módulo de Azure PowerShell](storage-blob-static-website-how-to.md#powershell)
> * [AzCopy](../common/storage-use-azcopy-v10.md)
> * [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/)
> * [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)
> * [Extensión de Visual Studio Code](https://code.visualstudio.com/tutorials/static-website/getting-started)

## <a name="viewing-content"></a>Visualización del contenido

Los usuarios pueden ver el contenido del sitio desde un explorador utilizando la dirección URL pública del sitio Web. Puede encontrar la dirección URL mediante el uso de Azure portal, CLI de Azure o PowerShell. Use esta tabla como guía.

|Herramienta| Guía |
|----|----|
|**Azure Portal** | [Buscar la dirección URL del sitio Web mediante el portal de Azure](storage-blob-static-website-how-to.md#portal-find-url) |
|**CLI de Azure** | [Buscar la dirección URL del sitio Web mediante la CLI de Azure](storage-blob-static-website-how-to.md#cli-find-url) |
|**Módulo de Azure PowerShell** | [Buscar la dirección URL del sitio Web mediante PowerShell](storage-blob-static-website-how-to.md#powershell-find-url) |

La dirección URL del sitio contiene un código de región. Por ejemplo, la dirección URL `https://contosoblobaccount.z22.web.core.windows.net/` contiene código regional `z22`.

Mientras que el código debe permanecer en la dirección URL, es solo para uso interno y no tendrá que usar ese código en cualquier otra forma.

El documento de índice que especifique al habilitar el hospedaje del sitio Web estático, aparece cuando los usuarios abrir el sitio y no especifican un archivo concreto (por ejemplo: `https://contosoblobaccount.z22.web.core.windows.net`).  

Si el servidor devuelve un error 404, y no ha especificado un documento de error cuando se habilita el sitio Web, se devuelve una página 404 predeterminada para el usuario.

## <a name="impact-of-the-setting-the-public-access-level-of-the-web-container"></a>Impacto de la configuración del nivel de acceso público del contenedor web

Puede modificar el nivel de acceso público de la **$web** contenedor, pero esto no influye en el punto de conexión principal sitio Web estático porque estos archivos se sirven a través de las solicitudes de acceso anónimo. Eso significa acceso público a (de solo lectura) a todos los archivos.

Captura de pantalla siguiente muestra la configuración del nivel de acceso público en el portal de Azure:

![Captura de pantalla que muestra cómo establecer el nivel de acceso público en el portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

Mientras no se vea afectado el punto de conexión principal sitio Web estático, un cambio en el nivel de acceso público afectar el extremo del servicio principal de blob.

Por ejemplo, si cambia el nivel de acceso público de la **$web** contenedor desde **privado (sin acceso anónimo)** a **Blob (acceso de lectura anónimo solo para blobs)** , el nivel de acceso público al punto de conexión principal sitio Web estático `https://contosoblobaccount.z22.web.core.windows.net/index.html` no cambia.

Sin embargo, el acceso público a la réplica principal punto de conexión de servicio de blob `https://contosoblobaccount.blob.core.windows.net/$web/index.html` cambiar de privado a público. Ahora los usuarios pueden abrir ese archivo mediante cualquiera de estos dos extremos.

## <a name="content-delivery-network-cdn-and-secure-socket-layer-ssl-support"></a>Content Delivery Network (CDN) y soporte técnico de Secure Socket Layer (SSL)

Para que los archivos del sitio Web estático disponible a través de su dominio personalizado y HTTPS, consulte [mediante la red CDN de Azure para tener acceso a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md). Como parte de este proceso, se necesita para que apunte a la red CDN a la réplica principal *del sitio Web estático* punto de conexión en lugar de la réplica principal *servicio blob* punto de conexión. Es posible que deba esperar unos minutos antes de que el contenido está visible, como la configuración de red CDN no se ejecuta inmediatamente.

Cuando se actualiza el sitio Web estático, asegúrese de borrar el contenido almacenado en caché en los servidores perimetrales CDN mediante la purga el punto de conexión CDN. Para más información, consulte [Purgar un punto de conexión de Azure CDN](../../cdn/cdn-purge-endpoint.md).

> [!NOTE]
> HTTPS admite de forma nativa a través del extremo web cuenta, por lo que el punto de conexión web es accesible a través de HTTP y HTTPS. Sin embargo, si la cuenta de almacenamiento está configurada para requerir a transferencia segura a través de HTTPS, los usuarios deben usar el extremo HTTPS. Para obtener más información, consulte [requerir transferencia segura en Azure Storage](../common/storage-require-secure-transfer.md).
>
> El uso de dominios personalizados a través de HTTPS requiere el uso de la red CDN de Azure en este momento.

## <a name="custom-domain-names"></a>Nombres de dominio personalizados

Puede hacer que el sitio Web estático disponible a través de un dominio personalizado. Para obtener más información, consulte [configurar un nombre de dominio personalizado para la cuenta de almacenamiento de Azure](storage-custom-domain-name.md).

Para información detallada de hospedar el dominio en Azure, consulte [hospede su dominio en DNS de Azure](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="pricing"></a>Precios

Puede habilitar el hospedaje de sitios Web estático de forma gratuita. Se factura solo para el almacenamiento de blobs que usa el sitio y los costos de las operaciones. Para obtener más información acerca de los precios de Azure Blob Storage, consulte la [página de precios de Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="metrics"></a>metrics

Puede habilitar las métricas en las páginas del sitio Web estático. Una vez que ha habilitado las métricas, tráfico de las estadísticas de archivos en el **$web** contenedor se muestran en el panel de métricas.

Para habilitar las métricas en las páginas del sitio Web estático, vea [habilitar las métricas en las páginas del sitio Web estático](storage-blob-static-website-how-to.md#metrics).

## <a name="next-steps"></a>Pasos siguientes

* [Hospedar un sitio Web estático en el almacenamiento de Azure](storage-blob-static-website-how-to.md)
* [Usar la red CDN de Azure para acceder a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md)
* [Configurar un nombre de dominio personalizado para el punto de conexión web o de blob](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [Crear la primera aplicación web sin servidor](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Tutorial: Hospedaje del dominio en Azure DNS](../../dns/dns-delegate-domain-azure-dns.md)
