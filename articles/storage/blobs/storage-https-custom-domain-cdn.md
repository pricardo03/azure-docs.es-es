---
title: Uso de Azure CDN para obtener acceso a blobs con dominios personalizados a través de HTTPS
description: Obtenga información sobre cómo integrar Azure CDN con Blob Storage para tener acceso a blobs con dominios personalizados a través de HTTPS.
services: storage
author: michaelhauss
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: mihauss
ms.component: blobs
ms.openlocfilehash: 7c4acc7d0832442b94735619ea3a01cb319da993
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398262"
---
# <a name="using-the-azure-cdn-to-access-blobs-with-custom-domains-over-https"></a>Uso de Azure CDN para obtener acceso a blobs con dominios personalizados a través de HTTPS
Ahora, Azure Content Delivery Network (CDN) admite HTTPS para los nombres de dominio personalizados. Puede aprovechar esta característica para tener acceso a blobs de almacenamiento mediante su dominio personalizado a través de HTTPS. Para ello, primero debe habilitar Azure CDN en el punto de conexión de blob o web y asignar la red CDN a un nombre de dominio personalizado. Una vez que haya llevado a cabo estos pasos, el proceso de habilitar HTTPS para el dominio personalizado se simplifica mediante la habilitación con un solo clic y la administración de certificados completa, y todo ello al precio normal de la red CDN, sin ningún costo adicional.

Esta funcionalidad es importante porque permite proteger la privacidad y la integridad de la información confidencial de la aplicación web durante el tránsito. El uso del protocolo SSL para dar servicio al tráfico a través de HTTPS garantiza el cifrado de los datos cuando se envían a través de Internet. HTTPS proporciona confianza y autenticación, y protege las aplicaciones web de posibles ataques.

> [!NOTE]  
> Además de proporcionar compatibilidad con SSL para los nombres de dominio personalizados, Azure CDN puede ayudarle a escalar la aplicación para proporcionar contenido con un elevado ancho de banda en todo el mundo. Para obtener más información, vea [Información general de Azure CDN](../../cdn/cdn-overview.md).

## <a name="quick-start"></a>Inicio rápido
Estos son los pasos necesarios para habilitar HTTPS para el punto de conexión personalizado de Blob Storage:

1.  
      [Integración de una cuenta de una instancia de Azure Storage con Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).
Este artículo le explica cómo crear una cuenta de almacenamiento en Azure Portal, si aún no lo ha hecho.

    > [!NOTE]  
    > Durante la versión preliminar de soporte técnico de sitios web estáticos en Azure Storage, seleccione "origen personalizado" en el menú desplegable "Tipo de origen" para agregar el punto de conexión web de almacenamiento. En Azure Portal, debe hacerlo desde el perfil de CDN en lugar de directamente en la cuenta de almacenamiento.

2.  
  [Asignación del contenido de Azure CDN a un dominio personalizado](../../cdn/cdn-map-content-to-custom-domain.md).
3.  
  [Habilitación de HTTPS en un dominio personalizado de Azure CDN](../../cdn/cdn-custom-ssl.md).

## <a name="shared-access-signatures"></a>Las firmas de acceso compartido
Si el punto de conexión de Blob Storage está configurado para no permitir el acceso de lectura anónimo, debe proporcionar un token de [Firma de acceso compartido (SAS)](../common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) en cada solicitud que realice a su dominio personalizado. De forma predeterminada, los puntos de conexión de Blob Storage no permiten el acceso de lectura anónimo. Vea [Administración del acceso de lectura anónimo a contenedores y blobs](storage-manage-access-to-resources.md) para obtener más información sobre las firmas de acceso compartidas.

Azure CDN no respeta las restricciones agregadas al token de SAS. Por ejemplo, todos los tokens de SAS tienen una fecha de expiración. Esto significa que todavía se puede tener acceso al contenido mediante una SAS expirada mientras el contenido no se purgue de los nodos perimetrales de la red CDN. Puede controlar cuánto tiempo se almacenan en caché los datos en la red CDN mediante el establecimiento del encabezado de respuesta de la caché. Vea [Administración de la expiración de Azure Storage Blob en Azure CDN](../../cdn/cdn-manage-expiration-of-blob-content.md) para obtener instrucciones.

Si crea varias direcciones URL de SAS para el mismo punto de conexión de blob, se recomienda activar el almacenamiento en caché de la cadena de consulta para Azure CDN. Esto sirve para asegurarse de que cada dirección URL se trata como una entidad única. Vea [Control del comportamiento del almacenamiento en caché de Azure CDN con cadenas de consulta](../../cdn/cdn-query-string.md) para obtener más información.

## <a name="http-to-https-redirection"></a>Redireccionamiento de HTTP a HTTPS
Puede redirigir el tráfico de HTTP a HTTPS. Para ello, debe usar la oferta premium de Verizon para Azure CDN. Debe [invalidar el comportamiento HTTP mediante el motor de reglas de Azure CDN](../../cdn/cdn-rules-engine.md) con la regla siguiente:

![](./media/storage-https-custom-domain-cdn/redirect-to-https.png)

"Cdn-endpoint-name" hace referencia al nombre que ha configurado para el punto de conexión de la red CDN. Puede seleccionar este valor en la lista desplegable. "Origin-path" hace referencia a la ruta de acceso dentro de la cuenta de almacenamiento de origen, en donde reside el contenido estático. Si hospeda todo el contenido estático en un mismo contenedor, reemplace "origin-path" por el nombre del contenedor.

Para obtener información más detallada sobre las reglas, vea [Características del motor de reglas de Azure CDN](../../cdn/cdn-rules-engine-reference-features.md).

## <a name="pricing-and-billing"></a>Precios y facturación
Cuando tiene acceso a blobs a través de una instancia de Azure CDN, paga [tarifas de Blob Storage](https://azure.microsoft.com/pricing/details/storage/blobs/) para el tráfico entre los nodos perimetrales y el origen (Blob Storage) y [tarifas de la red CDN](https://azure.microsoft.com/pricing/details/cdn/) para los datos a los que tiene acceso desde los nodos perimetrales.

Por ejemplo, supongamos que tiene una cuenta de almacenamiento en el oeste de EE. UU. a la que se obtiene acceso mediante Azure CDN. Cuando una persona ubicada en el Reino Unido intenta tener acceso a uno de los blobs de esa cuenta de almacenamiento a través de la red CDN, Azure busca en primer lugar el nodo perimetral más cercano al Reino Unido para dicho blob. Si encuentra uno, obtendrá acceso a esa copia del blob y usará las tarifas de la red CDN, ya que tiene acceso a él en la red CDN. Si no encuentra ninguno, Azure copiará el blob en el nodo perimetral, lo que generará gastos de salida y transacción, tal como se especifica en las tarifas de Blob Storage, y obtendrá acceso al archivo en el nodo perimetral, lo que dará lugar a facturación según las tarifas de la red CDN.

Cuando consulte la [página de precios de la red CDN](https://azure.microsoft.com/pricing/details/cdn/), tenga en cuenta que la compatibilidad con HTTPS para los nombres de dominio personalizados solo está disponible para Azure CDN de productos de Verizon (ediciones Standard y Premium).

## <a name="next-steps"></a>Pasos siguientes
* [Configurar un nombre de dominio personalizado para el punto de conexión de Blob Storage](storage-custom-domain-name.md)
* [Hospedaje de sitios web estáticos en Azure Storage (versión preliminar)](storage-blob-static-website.md)
