---
title: Ofertas privadas | Azure Marketplace
description: Ofertas privadas en Azure Marketplace para los editores de aplicaciones y servicios.
services: Azure, Marketplace, Compute
author: qianw211
manager: pabutler
ms.service: marketplace
ms.topic: article
ms.date: 11/1/2018
ms.author: pabutler
ms.openlocfilehash: cd61a30ccc9de08c3f0950d811d0d10ec5cd842a
ms.sourcegitcommit: b4f201a633775fee96c7e13e176946f6e0e5dd85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2019
ms.locfileid: "72598372"
---
# <a name="private-offers"></a>Ofertas privadas

Las ofertas privadas en [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/) permiten que los editores creen SKU que solo son visibles para clientes específicos.

## <a name="unlock-enterprise-deals-with-private-offers"></a>Desbloqueo de ofertas empresariales con ofertas privadas

Los clientes empresariales usan cada vez más los marketplaces en línea para buscar, probar y comprar soluciones en la nube. Con las ofertas privadas, ahora los editores pueden usar Marketplace para compartir de manera privada soluciones personalizadas con clientes específicos que tengan las funcionalidades que requieren las empresas:

- Los *precios negociados* permiten que los editores extiendan descuentos y precios fuera de la lista más allá de las ofertas disponibles a todo público.
- Los *términos y condiciones privados* permiten que los editores adapten los términos y condiciones a un cliente específico.
- Las *configuraciones especializadas* permiten que los editores adapten su oferta de Virtual Machines, Aplicaciones de Azure y Aplicaciones de SaaS a las necesidades de un cliente individual. Esta opción también permite que los editores brinden acceso de versión preliminar a características de productos nuevos, antes de lanzarlos de manera más amplia a todos los clientes.

Las ofertas privadas permiten que los editores aprovechen la disponibilidad global y a escala de un marketplace público, con la flexibilidad y el control que se necesitan para negociar y brindar ofertas y configuraciones personalizadas. En conjunto, estas características son el primer paso para que las empresas adopten con firmeza los marketplaces de la nube.  Ahora las empresas pueden comprar y vender como lo esperan y exigen.

Las ofertas privadas ya están disponibles para las ofertas de Virtual Machines, Aplicación de Azure (implementadas como plantillas de solución o aplicaciones administradas) y Aplicaciones de SaaS. Del mismo modo que ocurre con las ofertas públicas, las ofertas privadas se pueden crear y administrar a través de [Cloud Partner Portal](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md).  Es posible conceder o revocar el acceso de los clientes a las ofertas privadas en cuestión de minutos.

## <a name="creating-private-offers-using-skus-and-plans"></a>Creación de ofertas privadas con SKU y planes

Para las *ofertas nuevas o existentes con SKU o planes públicos*, los editores pueden crear fácilmente variaciones privadas nuevas si crean SKU o planes nuevos y los marcan como privados.  Las [SKU y los planes privados](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md) son componentes de una oferta y solo clientes específicos pueden verlos y comprarlos. Las SKU y los planes privados pueden volver a usar las imágenes base o metadatos de oferta ya publicados para una SKU o un plan público. Esta opción permite que los editores creen múltiples variaciones privadas de una oferta pública sin tener que publicar varias versiones de la misma imagen base y metadatos de oferta. Solo en el caso de las ofertas de Virtual Machines y Aplicación de Azure, cuando una SKU privada comparte una imagen base con una SKU pública, los cambios que se hagan en la imagen base de la oferta se propagarán en todas las SKU, tanto públicas como privadas, que usen esa imagen base.

En el caso de las *ofertas nuevas que solo incluyen SKU o planes privados*, los editores pueden crear sus ofertas como cualquier otra oferta y, luego, marcar las SKU o los planes como privados. Los clientes que no estén asociados con la oferta no podrán detectar ni acceder a través de [Azure Marketplace](https://azuremarketplace.microsoft.com) o [Azure Portal](https://azure.microsoft.com/features/azure-portal/) a las ofertas que solo tienen SKU o planes privados.

## <a name="targeting-customers-with-private-offers"></a>Selección de clientes específicos para las ofertas privadas
Tanto para las ofertas privadas nuevas como existentes, los editores pueden seleccionar clientes específicos mediante el uso de identificadores de suscripción. Los editores que usan una oferta de Virtual Machines o Aplicación de Azure pueden restringir la disponibilidad de una SKU privada a un identificador de suscripción individual de Azure o cargar un archivo .csv con hasta 20 000 identificadores de suscripción de Azure. Cuando se usa una oferta privada de Aplicación de SaaS, los editores pueden asociar un identificador de suscripción de Azure o un identificador de inquilino para restringir la disponibilidad de un plan privado, ya sea con el enfoque manual o mediante la carga del archivo .csv.

Una vez que se certifica o publica una oferta, los clientes se pueden actualizar o quitar de la SKU o del plan en cuestión de minutos mediante la característica de sincronización de suscripciones privadas. Esta funcionalidad permite que los editores actualicen rápida y sencillamente la lista de clientes a los que se presenta la SKU o el plan privado sin que sea necesario volver a certificar ni a publicar la oferta.

## <a name="deploying-private-offers"></a>Implementación de las ofertas privadas

Las ofertas privadas solo se detectan a través de [Azure Portal](https://azure.microsoft.com/features/azure-portal/) y no se presentan mediante [Azure Marketplace](https://azuremarketplace.microsoft.com). Una vez que un cliente inicia sesión en Azure Portal, puede seleccionar el elemento de navegación Marketplace para acceder a sus ofertas privadas. Las ofertas privadas también aparecerán en los resultados de la búsqueda y se pueden implementar a través de la línea de comandos y plantillas de Azure Resource Manager como cualquier otra oferta.

![[Ofertas privadas]](./media/marketplace-publishers-guide/private-offer.png)

Las ofertas privadas también aparecerán en los resultados de la búsqueda. Simplemente busque el distintivo que indica "Privada".

>[!Note]
>Las ofertas privadas no son compatibles con las suscripciones que se establecen a través de un revendedor del programa Proveedor de soluciones en la nube (CSP).

## <a name="next-steps"></a>Pasos siguientes

Para empezar a usar ofertas privadas, siga los pasos de la guía de las [SKU y los planes privados](./cloud-partner-portal-orig/cloud-partner-portal-azure-private-skus.md).
