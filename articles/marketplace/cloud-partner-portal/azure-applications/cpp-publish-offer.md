---
title: Publicación de una oferta de aplicación de Azure | Microsoft Docs
description: Describe el proceso y los pasos de publicación de una oferta de aplicación de Azure en Azure Marketplace.
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
ms.date: 12/05/2018
ms.author: pbutlerm
ms.openlocfilehash: fff751d531864faee7bd234ddcf543ae2992a617
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53196265"
---
# <a name="publish-azure-application-offer"></a>Publicación de una oferta de aplicación de Azure

Después de crear una nueva oferta al proporcionar la información de la página **Nueva oferta**, puede publicar la oferta. Seleccione **Publicar** para iniciar el proceso de publicación.

El diagrama siguiente muestra los pasos principales del proceso de publicación de una oferta para "lanzarla".

  ![Pasos para la publicación de una oferta](./media/offer-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Descripción detallada de los pasos de publicación

La tabla siguiente describe cada paso de publicación, con una estimación de tiempo (máxima) para completar cada paso.
En la tabla siguiente se describe cada uno de los pasos de publicación: También se proporciona un tiempo estimado para finalizar cada paso.


|  **Paso de publicación**           | **Hora**    | **Descripción**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Validar requisitos previos         | 15 minutos   | Se validan la información y la configuración de la oferta.                        |
| Certificación                  | 1 semana | El equipo de certificación de Azure analiza la oferta. Se examina la oferta en busca de virus, malware, cumplimiento de seguridad y problemas de seguridad. La oferta se comprueba para ver si cumple todos los criterios de elegibilidad. Para más información, consulte los [requisitos previos](./cpp-prerequisites.md). Si se detecta un problema, se proporcionan comentarios. |
| Packaging | 1 hora  | Los recursos técnicos de la oferta se empaquetan para uso del cliente y se configuran los sistemas del cliente potencial. |
|  Aprobación del publicador             |  -        | Revisión final del publicador y confirmación antes del lanzamiento de la oferta. Puede implementar la oferta en las suscripciones seleccionadas (en los pasos de información de la oferta) para comprobar que cumple todos los requisitos.  Seleccione **Transmitir** para que la oferta pueda pasar al paso siguiente. |
| Packaging                 | 1 hora | La oferta terminada se replica en las regiones y los sistemas de producción de Marketplace. | 
| En vivo                           | 4 días |La oferta se lanza, se replica en las regiones necesarias y se pone a disposición del público. |

Deje pasar hasta diez días laborables para que finalice el proceso de publicación y la oferta se publique. Cuando termina el proceso de publicación, la oferta se mostrará en [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/).

## <a name="next-steps"></a>Pasos siguientes

[Actualización de oferta existente](./cpp-update-existing-offer.md)