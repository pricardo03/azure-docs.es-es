---
title: Actualización de oferta existente de aplicaciones SaaS de Azure | Azure Marketplace
description: Cómo actualizar una oferta de aplicación SaaS existente en Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: dan-wesley
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: pbutlerm
ms.openlocfilehash: 2195c9a5e1f0d3683ea8cf6564d97cbabd072f81
ms.sourcegitcommit: 3ced637c8f1f24256dd6ac8e180fff62a444b03c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65834208"
---
# <a name="update-an-existing-saas-application-offer"></a>Actualización de una oferta de aplicación SaaS existente

Hay varios tipos de actualizaciones que puede que quiera realizar en su oferta una vez que se ha publicado y está activa. Cualquier cambio que realice en una nueva versión de la oferta se debe guardar y volver a publicar para que quede reflejado en Marketplace. Este artículo le lleva por los diferentes aspectos de la actualización de la oferta de SaaS en [Cloud Partner Portal](https://cloudpartner.azure.com/).

> [!IMPORTANT] 
> SaaS ofrecen funcionalidad se está migrando a la [Microsoft Partner Center](https://partner.microsoft.com/dashboard/directory).  Todos los publicadores nuevo deben usar el centro de partners para crear nuevas ofertas de SaaS y administrar ofertas existentes.  Los editores actuales con ofertas de SaaS se están migrando batchwise de Cloud Partner Portal para el centro de partners.  Cloud Partner Portal mostrará mensajes de estado para indicar cuándo se han migrado las ofertas existentes específicas.

Hay varios motivos por los que puede que quiera actualizar la oferta, como:

- Agregar una nueva versión a una aplicación existente.
- Actualizar una aplicación.
- Agregar nuevas características a una aplicación.
- Actualizar los metadatos de Marketplace para la oferta.

Para ayudarle con estas modificaciones, el portal ofrece las características **Comparar** e **Historial**.

## <a name="unpermitted-changes-to-a-saas-offer"></a>Cambios no permitidos en la oferta de SaaS

Hay atributos de una oferta de SaaS que no se pueden cambiar después de la publicación de la oferta en Azure Marketplace. No se pueden cambiar los valores siguientes:

- Id. de oferta e Id. de publicador de la oferta
- Etiquetas de versión, por ejemplo: `1.0.1`
- Cambios en el modelo de facturación o licencia en las ofertas existentes.

## <a name="common-update-operations"></a>Operaciones de actualización comunes
 
Las operaciones de actualización siguientes son comunes.

### <a name="update-offer-contacts"></a>Actualización de los contactos de la oferta

Siga los pasos a continuación para actualizar los contactos de soporte técnico de la oferta.

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En **Todas las ofertas**, busque la oferta que quiera actualizar.
3. Vaya a la pestaña **Contactos**. Actualice los contactos.
4. Seleccione **Publicar** para iniciar el flujo de trabajo para publicar los cambios.


### <a name="update-offer-marketplace-metadata"></a>Actualizar los metadatos de una oferta de Marketplace

Use los pasos siguientes para actualizar los metadatos de Marketplace asociados con la oferta. (Por ejemplo: nombre de empresa, logotipos, etc.).

1. Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2. En **Todas las ofertas**, busque la oferta que quiera actualizar.
3. Vaya a la pestaña**Detalles del escaparate**. Siga las instrucciones del artículo [Publicación de una oferta de aplicación SaaS](./cpp-publish-offer.md) para hacer cambios en los metadatos.
4. Seleccione **Publicar** para iniciar el flujo de trabajo para publicar los cambios.

## <a name="compare-feature"></a>Característica Comparar

Al hacer cambios en una oferta publicada, puede usar la característica Comparar para auditar los cambios hechos. La siguiente captura de pantalla muestra la opción Comparar para una oferta publicada.

![Use Comparar para ver los cambios de la oferta en Cloud Partner Portal](./media/saas-offer-compare.png)

### <a name="to-use-the-compare-feature"></a>Para usar la característica Comparar:

1. En cualquier punto del proceso de edición, seleccione Comparar para la oferta.
2. Vea las versiones en paralelo de recursos de marketing y metadatos.

## <a name="publishing-history"></a>Historial de publicación

Para ver la actividad de publicación histórica, seleccione la pestaña **Historial** de la barra de menús de navegación izquierda de Cloud Partner Portal. Puede ver las acciones con marca de tiempo que se han realizado durante la vigencia de las ofertas de Azure Marketplace.

![El historial de ofertas en Cloud Partner Portal](./media/saas-offer-history.png)

Puede usar la página del historial de auditoría para buscar una oferta específica y aplicar filtros como publicador, oferta y el tipo de evento (por ejemplo, OfferGoLiveRequested.) También puede descargar el historial de auditoría como archivo csv.


## <a name="next-steps"></a>Pasos siguientes

[Oferta de aplicación de Saas](./cpp-saas-offer.md)