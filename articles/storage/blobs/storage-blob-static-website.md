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
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427963"
---
# <a name="static-website-hosting-in-azure-storage"></a>Hospedaje de sitios web estáticos en Azure Storage

Puede proporcionar contenido estático (HTML, CSS, JavaScript y archivos de imagen) directamente desde un contenedor de almacenamiento llamado *$web*. El hospedaje de contenido en Azure Storage permite usar arquitecturas sin servidor que incluyen [Azure Functions](/azure/azure-functions/functions-overview) y otros servicios de plataforma como servicio (PaaS).

> [!NOTE]
> Si su sitio depende del código del lado servidor, use [Azure App Service](/azure/app-service/overview).

## <a name="setting-up-a-static-website"></a>Configuración de un sitio web estático

El hospedaje de sitios web estáticos es una característica que permite habilitar la cuenta de almacenamiento.

Para habilitar el hospedaje de sitios web estáticos, seleccione el nombre del archivo predeterminado y, opcionalmente, proporcione una ruta de acceso a una página 404 personalizada. Si aún no existe un contenedor de almacenamiento de blobs denominado **$web** en la cuenta, se creará uno. Agregue los archivos del sitio a ese contenedor.

Para obtener instrucciones detalladas, consulte [Hospedaje de sitios web estáticos en Azure Storage](storage-blob-static-website-how-to.md).

![Métrica de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-blob-container.png)

Los archivos del contenedor **$web** distinguen mayúsculas de minúsculas, se proporcionan mediante solicitudes de acceso anónimo y solo están disponibles a través operaciones de lectura.

## <a name="uploading-content"></a>Carga de contenido

Puede usar cualquiera de estas herramientas para cargar contenido en el contenedor **$web**:

> [!div class="checklist"]
> * [CLI de Azure](storage-blob-static-website-how-to.md#cli)
> * [Módulo de Azure PowerShell](storage-blob-static-website-how-to.md#powershell)
> * [AzCopy](../common/storage-use-azcopy-v10.md)
> * [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/)
> * [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)
> * [Extensión de Visual Studio Code](https://code.visualstudio.com/tutorials/static-website/getting-started)

## <a name="viewing-content"></a>Visualización de contenido

Los usuarios pueden ver contenido del sitio desde un explorador usando la dirección URL pública del sitio web. Puede buscar la dirección URL usando Azure Portal, CLI de Azure o PowerShell. Use esta tabla como guía.

|Herramienta| Guía |
|----|----|
|**Azure Portal** | [Búsqueda de la dirección URL del sitio web mediante Azure Portal](storage-blob-static-website-how-to.md#portal-find-url) |
|**CLI de Azure** | [Búsqueda de la dirección URL del sitio web mediante la CLI de Azure](storage-blob-static-website-how-to.md#cli-find-url) |
|**Módulo de Azure PowerShell** | [Búsqueda de la dirección URL del sitio web mediante PowerShell](storage-blob-static-website-how-to.md#powershell-find-url) |

La dirección URL del sitio contiene un código regional. Por ejemplo, la dirección URL `https://contosoblobaccount.z22.web.core.windows.net/` contiene el código regional `z22`.

Aunque el código debe permanecer en la dirección URL, solo es para uso interno y no se tiene que usar de ninguna otra manera.

El documento del índice que especifique al habilitar el hospedaje de sitios web estáticos aparece cuando los usuarios abren el sitio y no especifican un archivo concreto (por ejemplo, `https://contosoblobaccount.z22.web.core.windows.net`).  

Si el servidor devuelve un error 404 y no se ha especificado un documento de error al habilitar el sitio web, se devuelve una página 404 predeterminada al usuario.

## <a name="impact-of-the-setting-the-public-access-level-of-the-web-container"></a>Impacto de la configuración del nivel de acceso público del contenedor web

Puede modificar el nivel de acceso público del contenedor **$web**, lo cual no afecta al punto de conexión principal del sitio web estático porque estos archivos se proporcionan a través de solicitudes de acceso anónimo. Eso significa acceso público (de solo lectura) a todos los archivos.

En la siguiente captura de pantalla se muestra la configuración del nivel de acceso público en Azure Portal:

![Captura de pantalla que muestra cómo establecer el nivel de acceso público en el portal](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

Aunque el punto de conexión principal del sitio web estático no se ve afectado, un cambio en el nivel de acceso público afecta al punto de conexión principal de Blob service.

Por ejemplo, si cambia el nivel de acceso público del contenedor **$web** de **Privado (sin acceso anónimo)** a **Blob (acceso anónimo de lectura solo para blobs)** , el nivel de acceso público al punto de conexión principal del sitio web estático `https://contosoblobaccount.z22.web.core.windows.net/index.html` no cambia.

Sin embargo, el acceso público al punto de conexión principal de Blob service `https://contosoblobaccount.blob.core.windows.net/$web/index.html` cambia de privado a público. Ahora los usuarios pueden abrir ese archivo mediante cualquiera de estos dos punto de conexión.

## <a name="content-delivery-network-cdn-and-secure-socket-layer-ssl-support"></a>Compatibilidad con Content Delivery Network (CDN) y Secure Socket Layer (SSL)

Para hacer que los archivos del sitio web estático estén disponibles a través de su dominio personalizado y HTTPS, consulte [Uso de Azure CDN para obtener acceso a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md). Como parte de este proceso, debe apuntar su CDN al punto de conexión principal del *sitio web estático* en lugar de al punto de conexión principal de *Blob service*. Es posible que deba esperar unos minutos antes de que su contenido sea visible, ya que la configuración de CDN no se ejecuta de inmediato.

Al actualizar el sitio web estático, asegúrese de borrar el contenido almacenado en caché de los servidores perimetrales de CDN purgando el punto de conexión de CDN. Para más información, consulte [Purgar un punto de conexión de Azure CDN](../../cdn/cdn-purge-endpoint.md).

> [!NOTE]
> HTTPS se admite de forma nativa a través del punto de conexión web de la cuenta, de manera que se puede acceder al punto de conexión web a través de HTTP y HTTPS. Sin embargo, si la cuenta de almacenamiento está configurada para requerir la transferencia segura a través de HTTPS, los usuarios deben usar el punto de conexión HTTPS. Para más información, consulte [Requerir transferencia segura en Azure Storage](../common/storage-require-secure-transfer.md).
>
> Actualmente, el uso de dominios personalizados a través de HTTPS requiere usar Azure CDN.

## <a name="custom-domain-names"></a>Nombres de dominio personalizados

Puede hacer que el sitio web estático esté disponible a través de un dominio personalizado. Para más información, consulte [Configuración de un nombre de dominio personalizado para una cuenta de Azure Storage](storage-custom-domain-name.md).

Para obtener información detallada sobre cómo hospedar su dominio en Azure, consulte [Hospedaje del dominio en Azure DNS](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="pricing"></a>Precios

Puede habilitar el hospedaje de sitios web estáticos de forma gratuita. Se le factura solo por el almacenamiento de blobs que el sitio usa y por los costes de las operaciones. Para obtener más información acerca de los precios de Azure Blob Storage, consulte la [página de precios de Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="metrics"></a>Métricas

Puede habilitar métricas en páginas de sitios web estáticos. Después de habilitar las métricas, las estadísticas de tráfico de los archivos en el contenedor **$web** aparecen en el panel de métricas.

Para habilitar métricas en las páginas de su sitio web estático, consulte [Enable metrics on static website pages](storage-blob-static-website-how-to.md#metrics) (Habilitación de métricas en páginas de sitios web estáticos).

## <a name="next-steps"></a>Pasos siguientes

* [Hospedaje de sitios web estáticos en Azure Storage](storage-blob-static-website-how-to.md)
* [Uso de Azure CDN para acceder a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md)
* [Configurar un nombre de dominio personalizado para el punto de conexión web o de blob](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [Crear la primera aplicación web sin servidor](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Tutorial: Hospedaje del dominio en Azure DNS](../../dns/dns-delegate-domain-azure-dns.md)
