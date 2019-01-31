---
title: Uso de Azure CDN para obtener acceso a blobs con dominios personalizados mediante HTTPS
description: Obtenga información sobre cómo integrar Azure CDN con Blob Storage para acceder a blobs con dominios personalizados mediante HTTPS.
services: storage
author: michaelhauss
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: mihauss
ms.subservice: blobs
ms.openlocfilehash: 1166f98dcd39362ef5e28be12ff9e2f561ab5b1f
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238231"
---
# <a name="use-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>Uso de Azure CDN para obtener acceso a blobs con dominios personalizados mediante HTTPS

Azure Content Delivery Network (Azure CDN) ahora admite HTTPS para los nombres de dominio personalizados. Con Azure CDN, puede acceder a blobs con el nombre de dominio personalizado mediante HTTPS. Para ello, habilite Azure CDN en el punto de conexión de blob o web y asigne la red Azure CDN a un nombre de dominio personalizado. Cuando haya terminado, Azure simplifica la habilitación de HTTPS para el dominio personalizado mediante el acceso de un solo clic y la administración de certificados completa. No hay ningún aumento de los precios normales de Azure CDN.

Azure CDN ayuda a proteger tanto la privacidad como la integridad de datos de la información de las aplicaciones web mientras esté en tránsito. Con el uso del protocolo SSL para abastecer el tráfico mediante HTTPS, Azure CDN garantiza el cifrado de los datos cuando se envían a través de Internet. El uso de HTTPS con Azure CDN ayuda a proteger las aplicaciones web frente a ataques.

> [!NOTE]  
> Además de proporcionar compatibilidad con SSL para los nombres de dominio personalizados, Azure CDN puede ayudarle a escalar la aplicación para proporcionar contenido con un elevado ancho de banda en todo el mundo. Para más información, vea [Información general de Azure CDN](../../cdn/cdn-overview.md).

## <a name="quickstart"></a>Guía de inicio rápido

Para habilitar HTTPS para el punto de conexión personalizado de Blob Storage, haga lo siguiente:

1.  [Integración de una cuenta de una instancia de Azure Storage con Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).  
    En este artículo se explica cómo crear una cuenta de almacenamiento en Azure Portal, si aún no lo ha hecho.

    > [!NOTE]  
    > Para agregar el punto de conexión web de almacenamiento durante la versión preliminar de soporte técnico de sitios web estáticos en Azure Storage, seleccione **Origen personalizado** en la lista desplegable **Tipo de origen**. En Azure Portal, debe hacerlo desde el perfil de Azure CDN en lugar de directamente en la cuenta de almacenamiento.

2.  [Asignación del contenido de Azure CDN a un dominio personalizado](../../cdn/cdn-map-content-to-custom-domain.md).

3.  [Habilitación de HTTPS en un dominio personalizado de Azure CDN](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Firmas de acceso compartido

De forma predeterminada, los puntos de conexión de Blob Storage no permiten el acceso de lectura anónimo. Si el punto de conexión de Blob Storage está configurado para no permitir el acceso de lectura anónimo, proporcione un token de [firma de acceso compartido](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) en cada solicitud que realice a su dominio personalizado. Para más información, consulte [Administración del acceso de lectura anónimo a contenedores y blobs](storage-manage-access-to-resources.md).

Azure CDN no respeta las restricciones que se agregan al token de firma de acceso compartido. Por ejemplo, todos los tokens de firma de acceso compartido expiran. Todavía puede acceder a contenido con una firma de acceso compartido expirada hasta que se purgue ese contenido desde los nodos perimetrales de Azure CDN. Puede controlar cuánto tiempo se almacenan en caché los datos en Azure CDN mediante el establecimiento del encabezado de respuesta de caché. Para saber cómo, vea [Administración de la expiración de Azure Storage Blob en Azure CDN](../../cdn/cdn-manage-expiration-of-blob-content.md).

Si crea dos o más direcciones URL de firma de acceso compartido para el mismo punto de conexión de blob, se recomienda activar el almacenamiento en caché de la cadena de consulta para Azure CDN. Esta acción garantiza que Azure trata cada dirección URL como una entidad única. Para más información, consulte [Control del comportamiento del almacenamiento en caché de Azure CDN con cadenas de consulta](../../cdn/cdn-query-string.md).

## <a name="http-to-https-redirection"></a>Redireccionamiento de HTTP a HTTPS

Puede redirigir el tráfico HTTP a HTTPS. Para ello, debe usar la oferta premium de Verizon para Azure CDN. [Invalide el comportamiento HTTP mediante el motor de reglas de Azure CDN](../../cdn/cdn-rules-engine.md) con la aplicación de la regla siguiente:

![Regla de redireccionamiento de HTTP a HTTPS](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

*Cdn-endpoint-name*, que se selecciona en la lista desplegable, hace referencia al nombre que ha configurado para el punto de conexión de Azure CDN. *Origin-path* hace referencia a la ruta de acceso dentro de la cuenta de almacenamiento de origen, donde se almacena el contenido estático. Si hospeda todo el contenido estático en un mismo contenedor, reemplace *origin-path* por el nombre del contenedor.

Para obtener información más detallada sobre las reglas, vea [Características del motor de reglas de Azure CDN](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Precios y facturación

Cuando accede a blobs mediante una instancia de Azure CDN, paga [tarifas de Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/) para el tráfico entre los nodos perimetrales y el origen (Blob Storage). Además, paga las [tarifas de Azure CDN](https://azure.microsoft.com/pricing/details/cdn/) por los datos a los que se accede desde los nodos perimetrales.

Por ejemplo, supongamos que tiene una cuenta de almacenamiento en el Oeste de EE. UU. a la que se accede mediante Azure CDN. Cuando una persona ubicada en el Reino Unido intenta acceder a un blob de esa cuenta de almacenamiento mediante Azure CDN, Azure busca primero el blob en el nodo perimetral más cercano al Reino Unido. Si Azure encuentra el blob, accede a una copia y usa los preciso de Azure CDN, porque Azure CDN accede a ella. Si Azure no encuentra el blob, lo copia en el nodo perimetral. Esta acción genera cargos de salida y transacción, como se especifica en las tarifas de Blob Storage. Azure accede entonces al archivo en el nodo perimetral, lo que genera cargos en la facturación de Azure CDN.

En la [página de precios de Azure CDN](https://azure.microsoft.com/pricing/details/cdn/), el soporte técnico de HTTPS para nombres de dominio personalizados está disponible para Azure CDN solo en productos de Verizon Standard y Premium.

## <a name="next-steps"></a>Pasos siguientes

* [Configurar un nombre de dominio personalizado para el punto de conexión de Blob Storage](storage-custom-domain-name.md)
* [Hospedaje de sitios web estáticos en Azure Storage (versión preliminar)](storage-blob-static-website.md)
