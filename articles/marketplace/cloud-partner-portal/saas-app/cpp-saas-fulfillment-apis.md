---
title: API de entrega SaaS | Azure Marketplace
description: Presenta las versiones de la realización de las API que le permiten integrar su SaaS ofrece con Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: pabutler
ms.openlocfilehash: e7a01af1eba865b0a088b0fa7226273527abd70e
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66257406"
---
# <a name="saas-fulfillment-apis"></a>API de suministro de SaaS

> [!IMPORTANT] 
> SaaS ofrecen funcionalidad se ha migrado a la [Microsoft Partner Center](https://partner.microsoft.com/dashboard/directory).  Todos los publicadores nuevo deben usar el centro de partners para crear nuevas ofertas de SaaS y administrar ofertas existentes.  Los editores actuales con ofertas de SaaS se están migrando batchwise de Cloud Partner Portal para el centro de partners.  Cloud Partner Portal mostrará mensajes de estado para indicar cuándo se han migrado las ofertas existentes específicas.
> Para obtener más información, consulte [crear una nueva oferta de SaaS](../../partner-center-portal/create-new-saas-offer.md).

Las API de entrega SaaS habilitar proveedores de software independientes (ISV) integrar sus aplicaciones SaaS con Azure Marketplace. Estas API permiten a las aplicaciones de ISV participar en todos los canales de comercio electrónico: directo, dirigidos por asociados (distribuidor) y liderado por el campo.  Son un requisito para enumerar permiten transacciones ofertas de SaaS en Azure Marketplace.

> [!WARNING]
> La versión actual de esta API es la versión 2, que debe usarse para SaaS nuevo todas las ofertas.  Versión 1 de la API está en desuso y se mantiene por razones de compatibilidad con las ofertas existentes.


## <a name="business-model-support"></a>Compatibilidad con el modelo empresarial

Esta API admite las siguientes capacidades de modelo de negocio; Puedes:

* Especifique varios planes de una oferta. Estos planes tienen una funcionalidad diferente y es posible que precios diferentes.
* Proporcionar una oferta en una por sitio o una por cada modelo de facturación de usuario.
* Proporcionar mensuales y anuales (pagadas por adelantado) opciones de facturación.
* Proporcionar precios privada a un cliente basado en un acuerdo de negocio negociado.


## <a name="next-steps"></a>Pasos siguientes

Si no lo ha hecho ya, registre la aplicación de SaaS en la [portal Azure](https://ms.portal.azure.com) como se explica en [registrar una aplicación de Azure AD](./cpp-saas-registration.md).  Después, use la versión más reciente de esta interfaz para el desarrollo: [Versión 2 de la API de entrega SaaS](./cpp-saas-fulfillment-api-v2.md).
