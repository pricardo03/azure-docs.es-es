---
title: ¿Qué es Azure Stack? | Microsoft Docs
description: Azure Stack le permite ejecutar servicios de Azure en su centro de datos.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 10/25/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.custom: mvc
ms.openlocfilehash: 041d679cfc428278737a5668887e73f13ac694b1
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50137060"
---
# <a name="what-is-azure-stack"></a>¿Qué es Azure Stack?

Microsoft Azure Stack es una plataforma en la nube híbrida que le permite proporcionar servicios de Azure en su centro de datos. Dicha plataforma está diseñada para dar soporte a los requisitos en constante evolución de su empresa. Azure Stack puede habilitar nuevos escenarios para las aplicaciones modernas, como los entornos perimetrales y los entornos desconectados, o satisfacer determinados requisitos de seguridad y de cumplimiento.

Azure Stack se ofrece en dos opciones de implementación para satisfacer sus necesidades.

## <a name="azure-stack-integrated-systems"></a>Sistemas integrados de Azure Stack
Los sistemas integrados de Azure Stack se ofrecen a través de una asociación de Microsoft y sus [asociados de hardware](https://azure.microsoft.com/overview/azure-stack/integrated-systems/), lo que crea una solución que ofrece innovación dirigida a la nube y simplicidad en la administración de la computación. Dado que Azure Stack se ofrece como un sistema en el que el software y el hardware están perfectamente integrados, tendrá la flexibilidad y el control que necesita, así como la capacidad de innovar desde la nube. El tamaño de los sistemas integrados de Azure Stack va de los 4 a los 16 nodos y tiene un soporte técnico conjunto por parte de un asociado de hardware y Microsoft.  Use los sistemas integrados de Azure Stack para crear nuevos escenarios e implementar nuevas soluciones para las cargas de trabajo de producción.

## <a name="azure-stack-development-kit"></a>Kit de desarrollo de Azure Stack

El [Kit de desarrollo de Azure Stack](.\asdk\asdk-what-is.md) (ASDK) es una implementación en un solo nodo de Azure Stack que puede usar para evaluar y conocer Azure Stack.  También lo puede usar como entorno de desarrollo para compilar aplicaciones con API y herramientas coherentes con Azure.

>[!Note]
>El Kit de desarrollo de Azure Stack no está diseñado para usarse como entorno de producción.

El Kit de desarrollo de Azure Stack tiene las siguientes limitaciones:

* El Kit de desarrollo de Azure Stack está asociado a un solo proveedor de identidades de Azure Active Directory (Azure AD) o de Servicios de federación de Active Directory (AD FS). Puede crear varios usuarios en este directorio y asignar las suscripciones a cada usuario.
* Como los componentes de Azure Stack se implementan en un único equipo host, hay un número limitado de recursos físicos para los recursos de los inquilinos. Esta configuración no está pensada para la evaluación del rendimiento ni para realizar un escalado.
* Los escenarios de red están limitados porque hay un solo host y por los requisitos de implementación de NIC.

## <a name="next-steps"></a>Pasos siguientes

[Características y conceptos principales](azure-stack-key-features.md)

[Azure Stack: Una extensión de Azure (PDF)](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/)
