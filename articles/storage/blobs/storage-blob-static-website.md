---
title: Hospedaje de sitios web estáticos en Azure Storage
description: Azure Storage le ofrece hospedaje de sitios web estáticos, lo que le proporciona una solución rentable y escalable para hospedar aplicaciones web modernas.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/25/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 1794aa26fc725207c4a901c11c345eeaa3d2f65d
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56867748"
---
# <a name="static-website-hosting-in-azure-storage"></a>Hospedaje de sitios web estáticos en Azure Storage
Las cuentas GPv2 de Azure Storage le permiten proporcionar contenido estático (HTML, CSS, JavaScript y archivos de imagen) directamente desde un contenedor de almacenamiento llamado *$web*. Si aprovecha el hospedaje que ofrece Azure Storage, podrá usar arquitecturas sin servidor, incluyendo[Azure Functions](/azure/azure-functions/functions-overview) y otros servicios de PaaS.

A diferencia del hospedaje de sitios web estáticos, los sitios dinámicos que dependen del código del lado servidor se hospedan mejor si se usa [Azure App Service](/azure/app-service/overview).

## <a name="how-does-it-work"></a>¿Cómo funciona?
Cuando habilita el hospedaje de sitios web estáticos en su cuenta de almacenamiento, debe seleccionar el nombre del archivo predeterminado y, opcionalmente, proporcionar una ruta de acceso a una página 404 personalizada. Cuando la característica está habilitada, se crea un contenedor llamado *$web* si este aún no existe.

Los archivos del contenedor *$web* tienen las siguientes características:

- se proporcionan a través de solicitudes de acceso anónimas.
- solo están disponibles a través de operaciones de lectura de objetos.
- Distingue mayúsculas de minúsculas.
- están disponibles en la web pública si se sigue este patrón:
    - `https://<ACCOUNT_NAME>.<ZONE_NAME>.web.core.windows.net/<FILE_NAME>`
- están disponibles a través de un punto de conexión de Blob Storage si se sigue este patrón:
    - `https://<ACCOUNT_NAME>.blob.core.windows.net/$web/<FILE_NAME>`

Debe usar el punto de conexión de Blob Storage para cargar archivos. Por ejemplo, el archivo que se cargó a esta ubicación:

```bash
https://contoso.blob.core.windows.net/$web/image.png
```

está disponible en el explorador en una ubicación similar a la siguiente:

```bash
https://contoso.z4.web.core.windows.net/image.png
```

El nombre de archivo predeterminado que se seleccionó se usa en la raíz y en cualquier subdirectorio cuando no se proporciona ningún nombre de archivo. Si el servidor devuelve un error 404 y no proporciona ninguna ruta de acceso al documento de error, entonces se devuelve una página 404 predeterminada al usuario.

## <a name="cdn-and-ssl-support"></a>Compatibilidad con SSL y la red CDN

Para hacer que los archivos del sitio web estático estén disponibles a través de HTTPS, consulte [Uso de Azure CDN para obtener acceso a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md). Como parte de este proceso, debe *apuntar su red CDN al punto de conexión web* en lugar de al punto de conexión del blob. Es posible que deba esperar unos minutos antes de que su contenido sea visible, ya que la configuración de CDN no se ejecuta de inmediato.

Cuando se actualiza el sitio Web estático, asegúrese de borrar el contenido almacenado en caché en los servidores perimetrales CDN mediante la purga el punto de conexión CDN. Para más información, consulte [Purgar un punto de conexión de Azure CDN](../../cdn/cdn-purge-endpoint.md).

## <a name="custom-domain-names"></a>Nombres de dominio personalizados

Puede [configurar un nombre de dominio personalizado para su cuenta de Azure Storage](storage-custom-domain-name.md) para que su sitio web estático esté disponible a través de un dominio personalizado. Para obtener información detallada sobre cómo hospedar su dominio en Azure, consulte [Hospedaje del dominio en Azure DNS](../../dns/dns-delegate-domain-azure-dns.md).

## <a name="pricing"></a>Precios
El hospedaje de sitios web estáticos se proporciona sin costo adicional alguno. Para obtener más información acerca de los precios de Azure Blob Storage, consulte la [página de precios de Azure Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="quickstart"></a>Guía de inicio rápido

### <a name="azure-portal"></a>Azure Portal
Para comenzar, abra Azure Portal en https://portal.azure.com y siga los siguientes pasos en su cuenta de GPv2 de Storage:

1. Haga clic en **Configuración**.
2. Haga clic en **Sitio web estático**.
3. Escriba el *nombre del documento de índice*. (Un valor común es *index.html)*.
4. De forma opcional, escriba una *ruta de acceso de documento de error* a una página 404 personalizada. (Un valor común es *404.html)*.

![](media/storage-blob-static-website/storage-blob-static-website-portal-config.PNG)

A continuación, cargue los recursos al contenedor *$web* a través de Azure Portal o con el [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) para cargar los directorios completos. Asegúrese de incluir un archivo que coincida con el *nombre de documento del índice* que seleccionó cuando se habilitó la característica.

Por último, desplácese hasta el punto de conexión web para probar su sitio web.

### <a name="azure-cli"></a>Azure CLI
Instale la extensión de la versión preliminar de almacenamiento:

```azurecli-interactive
az extension add --name storage-preview
```
En el caso de tener varias suscripciones, establezca la CLI en la suscripción de la cuenta de GPv2 de Storage que quiera habilitar:

```azurecli-interactive
az account set --subscription <SUBSCRIPTION_ID>
```
Habilite la característica. Asegúrese de reemplazar todos los valores de los marcadores de posición, incluyendo las llaves, con sus propios valores:

```azurecli-interactive
az storage blob service-properties update --account-name <ACCOUNT_NAME> --static-website --404-document <ERROR_DOCUMENT_NAME> --index-document <INDEX_DOCUMENT_NAME>
```
Consulte la dirección URL del punto de conexión web:

```azurecli-interactive
az storage account show -n <ACCOUNT_NAME> -g <RESOURCE_GROUP> --query "primaryEndpoints.web" --output tsv
```

Cargue los objetos al contenedor *$web* desde un directorio de origen. No olvide usar el carácter de escapa correcto para la referencia al contenedor *$web* en el comando. Por ejemplo, si usa la CLI de Azure desde CloudShell en Azure Portal, use el carácter de escape para el contenedor *$web* como se muestra a continuación:

```azurecli-interactive
az storage blob upload-batch -s <SOURCE_PATH> -d \$web --account-name <ACCOUNT_NAME>
```

## <a name="deployment"></a>Implementación

Los métodos disponibles para implementar el contenido en un contenedor de almacenamiento son los siguientes:

- [AzCopy](../common/storage-use-azcopy.md)
- [Explorador de Storage](https://azure.microsoft.com/features/storage-explorer/)
- [Azure Pipelines](https://azure.microsoft.com/services/devops/pipelines/)
- [Extensión de Visual Studio Code](https://code.visualstudio.com/tutorials/static-website/getting-started)

En todos los casos, asegúrese de copiar los archivos en el contenedor *$web*.

## <a name="metrics"></a>Métricas

Para habilitar las métricas en las páginas de su sitio web estático, haga clic en **Settings** (Configuración)  > **Monitoring** (Supervisión)  > **Metrics**  (Métricas).

Los datos de las métricas se generan al enlazarse en diferentes API de métricas. El portal solo muestra los miembros de API que se usan en un período de tiempo determinado, para centrarse únicamente en los miembros que devuelven datos. Para asegurarse de que puede seleccionar el miembro de API necesario, el primer paso es expandir el período de tiempo.

Haga clic en el botón de marco de tiempo y seleccione **Last 24 hours** (Últimas 24 horas) y luego haga clic en **Apply** (Aplicar).

![Intervalo de tiempo de las métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-time-range.png)

A continuación, seleccione **Blob** en el menú desplegable *Namespace* (Espacio de nombres).

![Espacio de nombres de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-namespace.png)

A continuación, seleccione la métrica **Egress**.

![Métrica de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-metric.png)

Seleccione **Sum** del selector *Aggregation* (Agregación).

![Agregación de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-aggregation.png)

A continuación, haga clic en el botón **Add filter** (Agregar filtro) y elija **API name** (Nombre de API) en el selector *Property* (Propiedad).

![Nombre de API de métricas de sitios web estáticos de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-api-name.png)

Finalmente, marque la casilla junto a **GetWebContent** en el selector *Values* (Valores) para completar el informe de métricas.

![GetWebContent de métricas de sitios web de Azure Storage](./media/storage-blob-static-website/storage-blob-static-website-metrics-getwebcontent.png)

Una vez habilitadas, las estadísticas de tráfico de los archivos en el contenedor *$web* aparecen en el panel de métricas.

## <a name="faq"></a>Preguntas más frecuentes

**¿La característica de sitios web estáticos está disponible para todos los tipos de cuentas de almacenamiento?**  
No, el hospedaje de sitios web estáticos solo está disponible en las cuentas de almacenamiento estándar GPv2.

**¿Se admiten las reglas de firewall y VNET de almacenamiento con el nuevo punto de conexión web?**  
Sí, el nuevo punto de conexión web cumple las reglas de firewall y de VNET configuradas para la cuenta de almacenamiento.

**¿El punto de conexión web distingue entre mayúsculas y minúsculas?**  
Sí, el punto de conexión web distingue entre mayúsculas y minúsculas, al igual que el punto de conexión del blob. 

## <a name="next-steps"></a>Pasos siguientes
* [Uso de Azure CDN para obtener acceso a blobs con dominios personalizados a través de HTTPS](storage-https-custom-domain-cdn.md)
* [Configurar un nombre de dominio personalizado para el punto de conexión web o de blob](storage-custom-domain-name.md)
* [Azure Functions](/azure/azure-functions/functions-overview)
* [Azure App Service](/azure/app-service/overview)
* [Crear la primera aplicación web sin servidor](https://docs.microsoft.com/azure/functions/tutorial-static-website-serverless-api-with-database)
* [Tutorial: Hospedaje del dominio en Azure DNS](../../dns/dns-delegate-domain-azure-dns.md)
