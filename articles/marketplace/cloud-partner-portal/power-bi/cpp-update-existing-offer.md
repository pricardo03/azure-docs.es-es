---
title: 'Actualización de una oferta existente de aplicación de Power BI: Azure Marketplace | Microsoft Docs'
description: Actualice una oferta de aplicación de Power BI una vez publicado en Microsoft AppSource Marketplace.
services: Azure, AppSource, Marketplace, Cloud Partner Portal, Power BI
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: pbutlerm
ms.openlocfilehash: 635e2e71bb952aaee761df6a1d5d87c46db531f6
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55665706"
---
# <a name="update-an-existing-power-bi-app-offer"></a>Actualización de una oferta existente de aplicación de Power BI

En este artículo se repasan los diferentes aspectos de la actualización de una oferta de aplicación de Power BI en [Cloud Partner Portal](https://cloudpartner.azure.com/) y de la nueva publicación de la oferta.  Hay varios motivos comunes para actualizar la oferta, incluidos los siguientes:

- Actualizar el contenido de la aplicación en Power BI y obtener una dirección URL nueva de instalación de la aplicación recién empaquetada
- Actualizar los metadatos de Marketplace para la oferta: información o recursos de ventas, marketing o soporte técnico
 
Para ayudarle con estas modificaciones, el portal ofrece las características **Comparar** e **Historial**.


## <a name="unpermitted-changes-to-offer"></a>Cambios no permitidos en la oferta

Hay algunos atributos de una oferta de aplicación de Power BI que no se pueden modificar una vez que la oferta está publicada en AppSource, principalmente **Id. de la oferta** e **Id. del editor**.


## <a name="common-update-operations"></a>Operaciones de actualización comunes

Aunque hay una amplia gama de características que se pueden cambiar en una oferta de aplicación de Power BI, las operaciones siguientes son comunes.


### <a name="update-app-content-in-power-bi"></a>Actualizar el contenido de la aplicación en Power BI

Es habitual que una aplicación en Power BI se actualice periódicamente con nuevo contenido, revisiones de seguridad, características adicionales, etc. En estos escenarios, le interesa actualizar la dirección URL a la instalación de nuevo contenido de la aplicación mediante los pasos siguientes:

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2.  En **Todas las ofertas**, busque la oferta que se va a actualizar.
3.  En la pestaña **Información técnica**, escriba una nueva dirección URL del instalador.
4.  Haga clic en **Publicar** para iniciar el flujo de trabajo para publicar la nueva versión de la aplicación en AppSource.


### <a name="update-offer-marketplace-metadata"></a>Actualizar los metadatos de una oferta de Marketplace

Siga los pasos siguientes para actualizar los metadatos de Marketplace (nombre de la empresa, logotipo, etc.) asociados con la oferta:

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).
2.  En **Todas las ofertas**, busque la oferta que quiere actualizar.
3.  Vaya a la pestaña**Detalles del escaparate electrónico**, luego, siga las instrucciones de la pestaña [Power BI Apps Storefront Details](./cpp-storefront-details-tab.md) (Detalles del escaparate electrónico de aplicaciones de Power BI) para realizar cambios en los metadatos.
4.  Haga clic en **Publicar** para iniciar el flujo de trabajo para publicar los cambios.


## <a name="compare-feature"></a>Característica Comparar

Al realizar cambios en una oferta ya publicada, puede usar la característica **Comparar** para auditar los cambios realizados. Para usar esta característica:

1.  En cualquier punto del proceso de edición, haga clic en el botón **Comparar** de la oferta.

    ![Botón de la característica Comparar](./media/compare-feature-button.png)

2.  Vea las versiones en paralelo de los recursos de marketing y metadatos.


## <a name="history-of-publishing-actions"></a>Historial de acciones de publicación

Para ver la actividad de publicación histórica, haga clic en la pestaña **Historial** en la barra de menús de navegación de la izquierda de Cloud Partner Portal. Aquí podrá ver las acciones con marca de tiempo que se han realizado durante la vigencia de las ofertas de AppSource.


## <a name="next-steps"></a>Pasos siguientes

Debe usar con regularidad la característica [Seller Insights](../../cloud-partner-portal-orig/si-getting-started.md) de [Cloud Partner Portal](https://cloudpartner.azure.com/#insights) para ofrecer perspectivas sobre los clientes y el uso de Marketplace.  
