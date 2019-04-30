---
title: Publicación de una oferta de aplicación SaaS de Azure | Microsoft Docs
description: Proceso y pasos para la publicación de una oferta de aplicación SaaS en Azure Marketplace.
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
ms.date: 12/04/2018
ms.author: pbutlerm
ms.openlocfilehash: 25c9ab0008e7f056789536d8cc737b69e83d6db5
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62120244"
---
# <a name="publish-a-saas-application-offer"></a>Publicación de una oferta de aplicación SaaS

Después de crear una nueva oferta al proporcionar la información de la página **Nueva oferta**, puede publicar la oferta. Seleccione **Publicar** para iniciar el proceso de publicación.

En el siguiente diagrama se muestra el proceso general para publicar una nueva oferta de aplicación SaaS.

![Pasos para la publicación de una oferta](./media/offer-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Descripción detallada de los pasos de publicación

La tabla siguiente describe cada paso de publicación, con una estimación de tiempo (máxima) para completar cada paso.

|     **Step**       |     **Hora**      |  **Descripción**  |
|  ---------------   |  ---------------  |  ---------------  |
|         Certificación           |       Dos semanas            |          El equipo de certificación de Azure analiza la oferta. En este paso se llevan a cabo exámenes de virus, malware, cumplimiento de seguridad y problemas de seguridad. También verificará que esta oferta cumple todos los criterios de elegibilidad (consulte los [requisitos previos](./cpp-prerequisites.md)). Si se detecta un problema, se proporcionan comentarios.         |
|           Packaging         |       1 hora            |       Los recursos técnicos de la oferta se empaquetan para uso del cliente y se configuran los sistemas del cliente potencial.            |
|        Aprobación del publicador            |         -          |        Revisión final del publicador y confirmación antes del lanzamiento de la oferta. Puede implementar la oferta en las suscripciones seleccionadas (en los pasos de información de la oferta) para comprobar que cumple todos los requisitos. Seleccione **Transmitir** para que la oferta pueda pasar al paso siguiente.           |
|        Packaging            |        1 hora           |        La oferta terminada se replica en las regiones y los sistemas de producción de Marketplace.           |
|        En vivo            |       4 días            |         La oferta se lanza, se replica en las regiones necesarias y se pone a disposición del público.          |

Deje pasar hasta diez días laborables para que finalice el proceso de publicación y la oferta se publique. Cuando termine el proceso de publicación, la oferta SaaS se mostrará en [Microsoft Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1&subcategories=iot-edge-modules).

## <a name="next-steps"></a>Pasos siguientes

[Actualización de una oferta existente](./cpp-update-existing-offer.md)
