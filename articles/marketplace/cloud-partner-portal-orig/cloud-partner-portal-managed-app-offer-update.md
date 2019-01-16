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
ms.openlocfilehash: 2bfa10441cf5531c9383527a21b033da26322b34
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073245"
---
<a name="update-an-existing-offer-for-azure-marketplace"></a>Actualización de una oferta existente para Azure Marketplace 
==============================================

Hay varios tipos de actualizaciones que puede hacer a su oferta antes de transmitirla, por ejemplo:

1.  Incorporación de un \"paquete\" nuevo a una SKU existente
2.  Incorporación de nuevas SKU a una oferta existente
3.  Actualización de los metadatos de Marketplace de la oferta o SKU

Utilizará Cloud Partner Portal para implementar estos cambios y luego volverá a publicar su oferta. En este artículo se le guiará por los distintos aspectos de la actualización de la oferta de una aplicación de Azure.

<a name="unpermitted-changes-to-azure-application-offer"></a>Cambios no permitidos en la oferta de una aplicación de Azure 
-----------------------------------------------

Hay atributos de una oferta o SKU de una aplicación de Azure que no se pueden modificar una vez que la oferta se transmite a Azure Marketplace.

* Id. de la oferta e id. del publicador de la oferta.
* ID de la SKU de SKU existentes.
* Actualice un paquete que se ha publicado.

<a name="adding-a-new-package-to-an-existing-sku"></a>Incorporación de un paquete nuevo a una SKU existente 
---------------------------------------

El anunciante puede querer agregar una versión nueva del paquete para actualizar un paquete existente. Para ello, puede cargar un paquete nuevo con otro número de versión.

1.  Inicie sesión en [Cloud Partner Portal](http://cloudpartner.azure.com)
2.  En **Todas las ofertas**, busque la que desea actualizar
3.  En el formulario de las **SKU**, haga clic en la SKU cuyo paquete desea actualizar
4.  Haga clic en \"Nuevo paquete\" y especifique una nueva versión
5.  Cargue un archivo .zip nuevo que contenga el archivo de plantilla actualizado
6.  Haga clic en Publicar para iniciar el flujo de trabajo de publicación para que la SKU nueva se transmita.

<a name="adding-a-new-sku-to-an-existing-offer"></a>Incorporación de una nueva SKU a una oferta existente
-------------------------------------

Siga los pasos de abajo para poner una nueva SKU a disposición de la oferta existente:

1.  Inicio de sesión en [Cloud Partner Portal](http://cloudpartner.azure.com)
2.  En **Todas las ofertas**, busque la que desea actualizar
3.  En el formulario de las **SKU**, haga clic en Add new SKU (Agregar nueva SKU) y especifique un identificador de SKU en el elemento emergente.
4.  Siga el resto de los pasos como se especifica [aquí](./cloud-partner-portal-managed-app-publish.md).
5.  Haga clic en **Publicar** para iniciar el flujo de trabajo de publicación para que la SKU nueva se transmita.

<a name="updating-offer-marketplace-metadata"></a>Actualización de los metadatos del Marketplace de la oferta 
-----------------------------------

Puede que existan escenarios donde tenga que actualizar los metadatos del Marketplace asociado con su oferta, como la actualización de los logotipos de la empresa, etc. Siga estos pasos:

1.  Inicie sesión en Cloud Partner Portal.
2.  En **Todas las ofertas**, busque la que desea actualizar
3.  Vaya al formulario de Marketplace y siga las instrucciones que aparecen [aquí](../cloud-partner-portal/azure-applications/cpp-marketplace-tab.md) para realizar los cambios.
4.  Haga clic en Publicar para iniciar el flujo de trabajo de publicación para que los cambios se transmitan.
