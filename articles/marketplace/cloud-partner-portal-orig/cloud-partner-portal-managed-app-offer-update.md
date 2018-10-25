---
title: Actualización de una oferta existente para Azure Marketplace
description: Actualización de una oferta existente para Azure Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: qianw211
manager: pbutlerm
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5633392bdf1293ee9196fafe67cf901e0d8c8014
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807563"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Actualización de una oferta existente para Azure Marketplace 
==============================================

Hay varios tipos de actualizaciones que puede hacer a su oferta antes de transmitirla.

1.  Incorporación de un \"paquete\" nuevo a una SKU existente
2.  Incorporación de nuevas SKU a una oferta existente
3.  Actualización de los metadatos de Marketplace de la oferta o SKU

Debe actualizar la oferta en Cloud Partner Portal y volver a publicarla. Este artículo le guiará por los distintos aspectos de la actualización de la oferta de una aplicación de Azure.

<a name="unpermitted-changes-to-azure-application-offersku"></a>Cambios no permitidos en la oferta o SKU de una aplicación de Azure 
--------------------------------------------------

Hay atributos de una oferta o SKU de una aplicación de Azure que no se pueden modificar una vez que la oferta se transmite a Azure Marketplace.

1.  Id. de la oferta e id. del publicador de la oferta.
2.  ID de la SKU de SKU existentes.
3.  Actualice un paquete que se ha publicado.

<a name="adding-a-new-package-to-an-existing-sku"></a>Incorporación de un paquete nuevo a una SKU existente 
---------------------------------------

El anunciante puede querer agregar una versión nueva del paquete para actualizar un paquete existente. Para ello, puede cargar un paquete nuevo con otro número de versión.

1.  Inicie sesión en [Cloud Partner Portal](http://cloudpartner.azure.com)
2.  En Todas las ofertas, busque la que desea actualizar
3.  En el formulario de las SKU, haga clic en la SKU cuyo paquete desea actualizar
4.  Haga clic en \"Nuevo paquete\" y especifique una nueva versión
5.  Cargue un archivo .zip nuevo que contenga el archivo de plantilla actualizado
6.  Haga clic en Publicar para iniciar el flujo de trabajo de publicación para que la SKU nueva se transmita.

<a name="adding-a-new-sku-to-an-existing-offer"></a>Incorporación de una nueva SKU a una oferta existente
-------------------------------------

Puede hacer que una SKU nueva esté disponible para su oferta existente. Para ello, siga los pasos que aparecen a continuación.

1.  Inicie sesión en el [Cloud Partner Portal](http://cloudpartner.azure.com).
2.  En Todas las ofertas, busque la que desea actualizar.
3.  En el formulario de las SKU, haga clic en Add new SKU (Agregar nueva SKU) y especifique un identificador de SKU en el elemento emergente.
4.  Siga el resto de los pasos como se especifica [aquí](./cloud-partner-portal-managed-app-publish.md).
5.  Haga clic en Publicar para iniciar el flujo de trabajo de publicación para que la SKU nueva se transmita.

<a name="updating-offer-marketplace-metadata"></a>Actualización de los metadatos del Marketplace de la oferta 
-----------------------------------

Puede que existan escenarios donde tenga que actualizar los metadatos del Marketplace asociado con su oferta, como la actualización de los logotipos de la empresa, etc. Siga estos pasos:

1.  Inicie sesión en Cloud Partner Portal.
2.  En Todas las ofertas, busque la que desea actualizar.
3.  Vaya al formulario de Marketplace y siga las instrucciones que aparecen [aquí](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-push-to-staging) para realizar los cambios.
4.  Haga clic en Publicar para iniciar el flujo de trabajo de publicación para que los cambios se transmitan.
