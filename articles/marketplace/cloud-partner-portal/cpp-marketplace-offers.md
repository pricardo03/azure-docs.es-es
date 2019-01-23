---
title: Ofertas del Marketplace de Azure y AppSource | Microsoft Docs
description: Creación y administración de las ofertas del Marketplace de Azure y AppSource
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
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
ms.date: 01/09/2019
ms.author: pbutlerm
ms.openlocfilehash: 0d6879c6b9be06af4bb289761cc8f26b7e56d18e
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355338"
---
# <a name="azure-and-appsource-marketplace-offers"></a>Ofertas del Marketplace de Azure y AppSource

En la primera parte de esta sección se presentan las operaciones generales que se usan para crear y administrar las ofertas del Marketplace de Azure y AppSource.  En esta parte se proporcionan los antecedentes necesarios para administrar ciertos tipos de ofertas, así la información técnica que es común a todos los tipos de ofertas.  La mayor parte de esta sección contiene instrucciones detalladas sobre cómo crear y administrar tipos de ofertas específicas.  

En el siguiente vídeo se presentan las diversas funcionalidades y diferentes tipos d ofertas disponibles en Azure Marketplace o AppSource.  También explica importantes aspectos técnicos y empresariales para la publicación de aplicaciones o servicios en dichos Marketplace.

> [!VIDEO https://channel9.msdn.com/Events/Build/2018/BRK2513/player]

**Creación de aplicaciones y servicios para Azure Marketplace y AppSource: Build 2018**

Para obtener más información sobre los Marketplace, consulte la [guía de publicación de Azure Marketplace y AppSource](../marketplace-publishers-guide.md).


## <a name="azure-marketplace-and-appsource-offer-types"></a>Tipos de oferta de Azure Marketplace y AppSource

En la tabla siguiente se enumeran los tipos de oferta actuales compatibles con [Cloud Partner Portal](https://cloudpartner.azure.com).  Para cada tipo de oferta, se muestran los Marketplace en los que se puede publicar la oferta, así como una descripción general de la tecnología de solución de la oferta.

|                Tipo de oferta                |  Marketplace  |   DESCRIPCIÓN                                                           |
|                ----------                |  -----------  |   -----------                                                           |
| [Aplicación de Azure](./azure-applications/cpp-azure-app-offer.md) | Azure | Solución compuesta de una o más máquinas virtuales (VM) y código personalizado opcional de Azure. Se implementa a través de una plantilla de Azure Resource Manager.  La implementación puede realizarla el usuario a través de una plantilla de solución o el anunciante puede encargarse de ella. Este tipo de oferta se usa para una obtener una flexibilidad mayor que la de las ofertas de máquina virtual proporcionadas.  |
| [Servicio de consultoría](./consulting-services/cloud-partner-portal-consulting-services-publishing-offer.md) | Ambos | Los consultores de Microsoft que cumplen los requisitos pueden publicar servicios específicos para un dominio en Azure Marketplace o AppSource.  Su experiencia ayuda a los clientes a evaluar sus problemas y crear e implementar las soluciones correctas para cumplir sus objetivos empresariales.  |
| [Contenedor](./containers/cpp-containers-offer.md)  | Azure | Imagen de contenedor de Docker aprovisionada como un servicio basado en Kubernetes o como una instancia de contenedor de Azure. |
| [Dynamics 365 Business Central](../cloud-partner-portal-orig/cpp-business-central-offer.md) | AppSource | Paquete que extiende al sistema de planeación de recursos de empresariales (ERP) y administración empresarial. |
| [Dynamics 365 for Customer Engagement ](./dyn365ce/cpp-customer-engagement-offer.md) | AppSource | Paquete que extiende este sistema de administración de recursos del usuario (CRM) gracias a los módulos de ventas, servicios, servicio de proyecto y servicio de campo.  |
| [Dynamics 365 for Finance and Operations](../cloud-partner-portal-orig/cpp-dynamics-365-operations-offer.md) | AppSource | Paquete que extiende al servicio de planeación de recursos empresariales (ERP) que admite finanzas avanzadas, operaciones, fabricación y administración de la cadena de suministro. |
| [Módulo IoT Edge](./iot-edge-module/cpp-offer-process-parts.md) | Azure | Contenedor compatible con Docker que se ejecuta en un dispositivo IoT Edge.  Contiene módulos pequeños de cálculo que usan una combinación de código personalizado, otros servicios de Azure y servicios de terceros. |
| [Aplicación SaaS](./saas-app/cpp-saas-offer.md) | Azure | Suscripción de software como servicio administrada por el anunciante en la que los usuarios inician sesión mediante una interfaz personalizada que aprovecha las funciones de Azure Active Directory. |
| [Máquina virtual](./virtual-machine/cpp-virtual-machine-offer.md)  | Azure  | Solución contenida dentro de una sola máquina virtual que se implementa en la suscripción del cliente.  |
| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; |   |   |

Para obtener más información, consulte la [Guía de publicación por tipo de oferta](../publisher-guide-by-offer-type.md).


## <a name="next-steps"></a>Pasos siguientes

Obtendrá información sobre las operaciones generales que puede realizar en las ofertas de Marketplace y sus atributos y recursos técnicos comunes en el tema [Administración de ofertas](./manage-offers/cpp-manage-offers.md).
