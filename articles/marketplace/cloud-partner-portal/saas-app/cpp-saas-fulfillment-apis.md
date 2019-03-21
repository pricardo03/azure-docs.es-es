---
title: 'API de entrega SaaS: Azure Marketplace | Microsoft Docs'
description: Presenta las versiones de la realización de las API que le permiten integrar su SaaS ofrece con Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
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
ms.date: 02/27/2019
ms.author: pbutlerm
ms.openlocfilehash: 9c3fbe7cd7ebd41f59be360f40d66b8d38dbce5e
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2019
ms.locfileid: "57319280"
---
# <a name="saas-fulfillment-apis"></a>API de realización de SaaS

La API de entrega SaaS permite a proveedores de software independientes (ISV) integrar sus aplicaciones SaaS con Azure Marketplace. Esta API permite que las aplicaciones de ISV participar en todos los canales de comercio electrónico: directo, dirigidos por asociados (distribuidor) y liderado por el campo.  Esta API es un requisito para la lista de que ofertas de SaaS permiten transacciones en Azure Marketplace.

La versión actual de esta API es la versión 2, que debe usarse para SaaS nuevo todas las ofertas.  Versión 1 de la API está en desuso y se mantiene por razones de compatibilidad con las ofertas existentes.


## <a name="business-model-support"></a>Compatibilidad con el modelo empresarial

Esta API admite las siguientes capacidades de modelo de negocio; Puedes:

* Especifique varios planes de una oferta. Estos planes tienen una funcionalidad diferente y es posible que precios diferentes.
* Proporcionar una oferta en una por sitio o una por cada modelo de facturación de usuario.
* Proporcionar mensuales y anuales (pagadas por adelantado) opciones de facturación.
* Proporcionar precios privada a un cliente basado en un acuerdo de negocio negociado.


## <a name="next-steps"></a>Pasos siguientes

Use la versión más reciente de esta interfaz para el desarrollo: [Versión 2 de la API de entrega SaaS](./cpp-saas-fulfillment-api-v2.md).
