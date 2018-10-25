---
title: Eliminación de una oferta o SKU de Azure Marketplace | Microsoft Docs
description: Pasos para eliminar una oferta o SKU.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: cc172e35e8964fad3b1a1410d1f1f3240c423ab3
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48807603"
---
<a name="delete-an-offer-or-sku-from-azure-marketplace"></a>Eliminación de una oferta o SKU de Azure Marketplace
==========================================

Puede decidir eliminar su oferta de Marketplace por varios motivos. La eliminación de ofertas garantiza que los clientes nuevos ya no puedan adquirir ni implementar la oferta, pero afecta a los clientes existentes.
La finalización de la oferta es el proceso de terminación del servicio o contrato de licencia entre usted y los clientes existentes. Las directrices y las directivas relacionadas con la retirada y la terminación de la oferta se rigen en virtud del [contrato del anunciante de Microsoft Marketplace](http://go.microsoft.com/fwlink/?LinkID=699560) (véase la sección 7)
7) y las [directivas de participación](https://azure.microsoft.com/support/legal/marketplace/participation-policies/) (véase la sección 6.2). En este artículo se habla de los diferentes escenarios de eliminación admitidos y los pasos que puede seguir en ellos.

<a name="delete-a-live-sku-from-azure-marketplace"></a>Eliminación de una SKU en directo de Azure Marketplace
----------------------------------------

Puede eliminar una SKU en directo de Azure Marketplace mediante estos pasos:

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  Seleccione la oferta en la pestaña **Todas las ofertas**.

3.  En el panel izquierdo de la pantalla, seleccione la pestaña **SKU**.

4.  Seleccione la SKU que quiera eliminar y haga clic en el botón Eliminar de esa SKU.

5.  [Vuelva a publicar](./cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) la oferta en Azure Marketplace.

Cuando la oferta esté en directo en Azure Marketplace, la SKU se eliminará de Azure Marketplace y Azure Portal.

<a name="roll-back-to-a-previous-sku-version"></a>Reversión a una versión anterior de una SKU
----------------------------------

Para eliminar la versión actual de una SKU en directo de Azure Marketplace siga estos pasos. Cuando se complete el proceso, la SKU se revertirá a la versión anterior.

1.  Inicie sesión en [Cloud Partner Portal](https://cloudpartner.azure.com/).

2.  Seleccione la oferta en la pestaña **Todas las ofertas**.

3.  En el panel izquierdo de la pantalla, seleccione la pestaña **SKU**.

4.  Elimine la **versión de disco** más reciente de la lista de versiones de disco publicada.

5.  [Vuelva a publicar](./cloud-partner-portal-make-offer-live-on-Azure-Marketplace.md) la oferta en Azure Marketplace.

Cuando la oferta esté en directo en Azure Marketplace, la versión actual de la SKU indicada se eliminará de Azure Marketplace y Azure Portal.
La SKU se revertirá a la versión anterior.

<a name="delete-a-live-offer"></a>Eliminación de una oferta en directo
-------------------

En el caso de una solicitud para quitar una oferta activa, son varios los aspectos que se deben tener en cuenta. Siga estos pasos si quiere recibir ayuda del equipo de soporte técnico para eliminar una oferta en directo de Azure Marketplace:

1.  Genere una incidencia de soporte técnico mediante este [vínculo](https://go.microsoft.com/fwlink/?linkid=844975), o bien haga clic en Soporte técnico en la esquina superior derecha de Cloud Partner Portal.

2.  Seleccione el tipo de oferta específico en la lista **Tipo de problema** y elija **Remove a published offer** (Quitar una oferta publicada) en la lista **Categoría**.

3.  Envíe la solicitud.

El equipo de soporte técnico le guiará en el proceso de eliminación de la oferta.

>[!NOTE]
>La eliminación de una oferta o SKU no afectará a las compras actuales de la oferta o la SKU. Estas ofertas o SKU seguirán funcionando como antes. No obstante, no estarán disponibles para las compras futuras.
