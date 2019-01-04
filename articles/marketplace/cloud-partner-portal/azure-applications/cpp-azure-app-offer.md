---
title: Oferta de aplicación de Azure | Microsoft Docs
description: Descripción general del proceso de publicación de una oferta de aplicación de Azure en Azure Marketplace.
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
ms.date: 12/07/2018
ms.author: pbutlerm
ms.openlocfilehash: 63b7ee4e0d9cb9d0d1f26119fe73573b124d04e8
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196330"
---
# <a name="azure-application-offer"></a>Oferta de aplicación de Azure

En esta sección se explica cómo publicar una nueva oferta de aplicación de Azure en Microsoft <a href="https://azuremarketplace.microsoft.com">Azure Marketplace</a>.
|    |    |
|-----------------------------------------------------------------|------------------------------------------|
| Cada aplicación de Azure contiene una plantilla de Azure Resource Manager que define todos los recursos técnicos utilizados por la aplicación, que normalmente incluye una o más máquinas virtuales y otros servicios de soporte técnico basados en web o en Azure. | ![Icono de aplicaciones de Azure](./media/azureapp-icon1.png)  |

## <a name="benefits"></a>Ventajas

Algunas de las ventajas de anunciar las aplicaciones en Microsoft Marketplace incluyen:

* Llegar a 100 millones de usuarios de Azure Active Directory en Office 365 y Dynamics 365.

* Ampliar su equipo de ventas: llegue a usuarios de empresa de todo el mundo y obtenga un canal de ventas que interactúa con los usuarios finales, ayuda a generar clientes potenciales e inicia conversaciones con nuevos clientes de diferentes sectores.

* Obtener información práctica: compartimos información detallada sobre cómo evoluciona su aplicación en AppSource, qué funciona bien y cómo mejorar más los procedimientos de ventas.

## <a name="types-of-azure-applications"></a>Tipos de aplicaciones de Azure

Hay dos tipos de aplicaciones de Azure: una aplicación administrada y una plantilla de solución. Si bien son similares, hay algunas diferencias importantes.

### <a name="solution-template"></a>Plantilla de solución

Las plantillas de solución son uno de los principales mecanismos para publicar una solución en Marketplace. Este tipo de oferta se usa cuando la solución requiere automatización adicional de implementación y configuración para algo más que una sola máquina virtual (VM). Puede automatizar proporcionando más de una VM mediante una plantilla de solución. Esto incluye el aprovisionamiento de recursos de red y de almacenamiento para proporcionar soluciones complejas de IaaS. Para obtener información general de los requisitos de una plantilla de solución y el modelo de facturación, consulte [Aplicaciones de Azure: guía de publicación de ofertas de plantillas de solución](https://docs.microsoft.com/azure/marketplace/marketplace-solution-templates).

### <a name="managed-application"></a>Aplicación administrada

Una aplicación administrada es similar a una plantilla de solución de Marketplace, aunque hay una diferencia importante. En una aplicación administrada, los recursos se implementan en un grupo de recursos que está administrado por el editor de la aplicación. El grupo de recursos está presente en la suscripción del consumidor, pero una identidad en el inquilino del editor tiene acceso al grupo de recursos. Como editor, puede especificar el costo del soporte técnico continuado de la solución. Use aplicaciones administradas de Azure para crear y entregar con facilidad a los clientes aplicaciones llave en mano totalmente administradas.

Además de Marketplace, también puede ofrecer a las aplicaciones administradas en un catálogo de servicios. El catálogo de servicios es un catálogo interno de soluciones aprobadas para los usuarios de una organización. Use el catálogo para cumplir los estándares organizativos y ofrecer al mismo tiempo soluciones para los grupos en una organización. Los empleados usarán el catálogo para encontrar fácilmente las aplicaciones que el departamento de TI recomienda y aprueba.

Para más información sobre las ventajas y los tipos de aplicaciones administradas, consulte [Introducción a las aplicaciones administradas de Azure](https://docs.microsoft.com/azure/managed-applications/overview).

## <a name="publishing-overview"></a>Introducción a la publicación

En el siguiente vídeo, [Building Solution Templates, and Managed Applications for the Azure Marketplace](https://channel9.msdn.com/Events/Build/2018/BRK3603) (Creación de plantillas de soluciones y aplicaciones administradas para Azure Marketplace), se ofrece información general sobre cómo crear una plantilla de Azure Resource Manager para definir una solución de aplicación de Azure y cómo publicar posteriormente la oferta de aplicación en Azure Marketplace.

>[!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK3603/player]

## <a name="publishing-process-workflow"></a>Flujo de trabajo del proceso de publicación

El siguiente diagrama muestra el proceso general para publicar una oferta de aplicación de Azure.

![Flujo de trabajo para publicar una oferta](./media/new-offer-process.png)

## <a name="offer-components"></a>Componentes de la oferta

En esta sección se describen los elementos de la publicación de una oferta de aplicación administrada, y está pensada como una guía para el publicador en Azure Marketplace. La publicación se divide en las siguientes partes principales: 

* [Requisitos previos](./cpp-prerequisites.md): Se enumeran los requisitos técnicos y empresariales antes de crear o publicar una oferta de aplicación administrada. 
* [Creación de la oferta](./cpp-create-offer.md): Se enumeran los pasos necesarios para crear una entrada de oferta de aplicación administrada mediante Cloud Partner Portal. 
* [Publicación de la oferta](./cpp-publish-offer.md): Describe cómo enviar la oferta para publicarla en Azure Marketplace.

## <a name="steps-in-the-publishing-process"></a>Pasos del proceso de publicación

Los pasos generales para publicar una oferta de aplicación de Azure son:

1. Crear la oferta: proporcione información detallada sobre la oferta. Esta información incluye: la descripción de la oferta, materiales de marketing, información de soporte técnico y especificaciones de recursos.

2. Creación de los recursos técnicos y empresariales: se crean los recursos empresariales (documentos legales y materiales de marketing) y los recursos técnicos de la solución asociada.

3. Crear las SKU: cree las SKU asociadas a la oferta. Se necesita una SKU única para cada imagen que se vaya a publicar.

4. Certificar y publicar la oferta: después de completar la oferta y los recursos técnicos, puede enviar la oferta. Este envío inicia el proceso de publicación. Durante este proceso, la solución se prueba, se valida, se certifica y luego se publica en Azure Marketplace.

## <a name="next-steps"></a>Pasos siguientes

Antes de considerar estos pasos, debe cumplir los [requisitos técnicos y empresariales](./cpp-prerequisites.md) para publicar una aplicación administrada en Microsoft Azure Marketplace.