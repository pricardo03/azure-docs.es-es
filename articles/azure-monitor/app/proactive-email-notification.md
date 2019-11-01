---
title: 'Detección inteligente de Azure Application Insights: Próximos cambios en los destinatarios de la notificación predeterminados | Microsoft Docs'
description: Supervise los seguimientos de las aplicaciones con Azure Application Insights para detectar patrones poco habituales de telemetría de seguimiento.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: harelbr
ms.author: harelbr
ms.date: 03/13/2019
ms.reviewer: mbullwin
ms.openlocfilehash: fa1f98b5a9ee592a4c702e87e365eff7941194d4
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72820612"
---
# <a name="smart-detection-e-mail-notification-change"></a>Cambio de las notificaciones por correo electrónico de detección inteligente

Según los comentarios de los clientes, el 1 de abril de 2019 vamos a cambiar los roles predeterminados de quienes reciben notificaciones por correo electrónico de detección inteligente.

## <a name="what-is-changing"></a>¿Qué está cambiando?

Actualmente, las notificaciones por correo electrónico de detección inteligente se envían de forma predeterminada a los roles de _propietario de la suscripción_, _colaborador de la suscripción_ y _lector de la suscripción_. Estos roles suelen incluir usuarios que no participan activamente en la supervisión, lo que hace que muchos de estos usuarios reciban notificaciones innecesariamente. Para mejorar esta experiencia, estamos haciendo un cambio para que las notificaciones por correo electrónico solo vayan, de manera predeterminada, a los roles de [lector de supervisión ](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) y [colaborador de supervisión](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor).

## <a name="scope-of-this-change"></a>Ámbito de este cambio

Este cambio afectará a todas las reglas de detección inteligente, excepto los siguientes:

* Las reglas de detección inteligente marcadas como versión preliminar. Estas reglas de detección inteligente no admiten notificaciones por correo electrónico en este momento.

* Reglas de anomalías de error. Esta regla comenzará a dirigir a los nuevos roles predeterminados una vez que se migre de una alerta clásica a la plataforma de alertas unificada (hay más información disponible [aquí](https://docs.microsoft.com/azure/azure-monitor/platform/monitoring-classic-retirement)).

## <a name="how-to-prepare-for-this-change"></a>¿Cómo prepararse para este cambio?

Para garantizar que las notificaciones por correo electrónico de detección inteligente se envíen a los usuarios pertinentes, estos usuarios deben tener asignados los roles de [lector de supervisión](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-reader) y [colaborador de supervisión](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#monitoring-contributor) de la suscripción.

Para asignar usuarios a los roles de lector de supervisión o de colaborador de supervisión mediante Azure Portal, siga los pasos descritos en el artículo [Adición de una asignación de roles](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal#add-a-role-assignment). Asegúrese de seleccionar _lector de supervisión_ o _colaborador de supervisión_ como el rol al que se asignan los usuarios.

> [!NOTE]
> Los destinatarios específicos de las notificaciones de detección inteligente, configurados mediante la opción _Destinatarios de correo electrónico adicionales_ en la configuración de la regla, no se verán afectados por este cambio. Estos destinatarios seguirá recibiendo las notificaciones por correo electrónico.

Si tiene alguna pregunta o duda acerca de este cambio, no dude en [ponerse en contacto con nosotros](mailto:smart-alert-feedback@microsoft.com).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la detección inteligente:

- [Anomalías de error](../../azure-monitor/app/proactive-failure-diagnostics.md)
- [Fugas de memoria](../../azure-monitor/app/proactive-potential-memory-leak.md)
- [Anomalías de rendimiento](../../azure-monitor/app/proactive-performance-diagnostics.md)