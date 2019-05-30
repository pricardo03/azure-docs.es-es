---
title: archivo de inclusión
description: archivo de inclusión
services: azure-monitor
author: rboucher
tags: azure-service-management
ms.topic: include
ms.date: 02/07/2019
ms.author: robb
ms.custom: include file
ms.openlocfilehash: 050d3314345e64e3d69a07367a0e9acc318fa106
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66271590"
---
| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Opciones de escala automática |100 por región y suscripción. | Igual que el valor predeterminado. |
| Alertas de métricas (clásico) |100 reglas alertas activas por suscripción. | Llame al soporte técnico. |
| Alertas de métricas |1000 reglas de alertas activas por suscripción (en nubes públicas) y 100 reglas de alertas activas por suscripción de Azure China y Azure Government. | Llame al soporte técnico. |
| Alertas de registros de actividad | 100 reglas alertas activas por suscripción. | Igual que el valor predeterminado. |
| Alertas de registro | 512 | Llame al soporte técnico. |
| Grupos de acciones |2.000 grupos de acciones por suscripción. | Llame al soporte técnico. |

**Límites específicos del grupo de acción**

| Recurso | Límite predeterminado | Límite máximo |
| --- | --- | --- |
| Inserción de la aplicación de Azure | 10 acciones de aplicación de azure por grupo de acciones. | Llame al soporte técnico. |
| Email | 1000 acciones de correo electrónico en un grupo de acciones. Consulte también el [información sobre las limitaciones](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Llame al soporte técnico. |
| ITSM | 10 acciones de ITSM en un grupo de acciones. | Llame al soporte técnico. | 
| Aplicación lógica | 10 acciones de aplicación lógica en un grupo de acciones. | Llame al soporte técnico. |
| Runbook | 10 acciones de runbook en un grupo de acciones. | Llame al soporte técnico. |
| sms | 10 acciones de SMS en un grupo de acciones. Consulte también el [información sobre las limitaciones](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Llame al soporte técnico. |
| Voz | 10 acciones de voz en un grupo de acciones. Consulte también el [información sobre las limitaciones](../articles/azure-monitor/platform/alerts-rate-limiting.md). | Llame al soporte técnico. |
| webhook | 10 acciones de webhook en un grupo de acciones.  Número máximo de llamadas de webhook es 1500 por minuto por suscripción. Están disponibles en otros límites [información específica de la acción](../articles/azure-monitor/platform/action-groups.md#action-specific-information).  | Llame al soporte técnico. |
