---
title: Modelo de implementación clásica de Azure | Microsoft Docs
description: modelo de implementación clásica de Azure
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/20/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 5ae2a56c084116ae8d57a16696223e7990258558
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313220"
---
# <a name="classic-deployment-model"></a>Modelo de implementación clásica

El modelo de implementación clásica es el modo de implementación de Azure de la generación anterior que impone un límite de cuota de vCPU global para máquinas virtuales y conjuntos de escalado de máquinas virtuales. El modelo de implementación clásica ya no se recomienda y ahora se reemplaza por el modelo de Resource Manager. Para más información sobre estos dos modelos de implementación y las ventajas de Resource Manager, consulte la página del modelo de implementación de Resource Manager. Cuando se crea una nueva suscripción, se asigna una cuota predeterminada de vCPU a ella. Cada vez que se implementa una nueva máquina virtual mediante el modelo de implementación clásica, la suma del uso de vCPU nuevas y existentes en todas las regiones no debe superar la cuota de vCPU aprobada para dicho modelo. Más información sobre cuotas en la página de límites de servicio y suscripción de Azure.

Puede solicitar un aumento en el límite de vCPU para el modelo de implementación clásica a través de la hoja Ayuda y soporte técnico o la hoja Usages + Quota (Usos y cuota) en el portal.

