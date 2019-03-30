---
title: Migrar las alertas clásicas en Azure Monitor con la herramienta de migración voluntaria
description: Obtenga información sobre cómo usar la herramienta de migración voluntaria para migrar las reglas de alertas clásicas.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 58c664beee942fe7115c7fff38a039c23bed6ac3
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58632520"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Usar la herramienta de migración voluntaria para migrar las reglas de alertas clásicas

Como [anunciaron](monitoring-classic-retirement.md), las alertas clásicas en Azure Monitor se retirará en julio de 2019. La herramienta de migración para desencadenar voluntariamente la migración está disponible en Azure portal y está implementando en los clientes que usan reglas de alerta clásicas. Este artículo le guiará a través de sobre cómo usar la herramienta de migración para migrar voluntariamente las reglas de alertas clásicas antes de que comience la migración automática en julio de 2019.

## <a name="benefits-of-new-alerts"></a>Ventajas de nuevas alertas

Las alertas clásicas están siendo remplazadas por alertas unificada nueva en Azure Monitor. La nueva plataforma de alertas consta de las siguientes ventajas:

- Alerta sobre una variedad de métricas multidimensionales para [muchos más servicios de Azure](alerts-metric-near-real-time.md#metrics-and-dimensions-supported)
- Soporte técnico de alertas de métrica nueva [las reglas de alertas de múltiples recursos](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) que reducir en gran medida la sobrecarga de administrar muchas reglas.
- Mecanismo de notificación unificada
  - [Grupos de acciones](action-groups.md) es un mecanismo de notificaciones modular que funciona con todos los nuevos tipos de alertas (métrica, registro y registro de actividad)
  - También podrá aprovechar las ventajas de los nuevos mecanismos de notificación como SMS, voz y conector de ITSM
- El [alerta experiencia unificada](alerts-overview.md) aporta todas las alertas en las señales diferentes (actividad métrica, registro y registro) en un solo lugar

## <a name="before-you-migrate"></a>Antes de migrar

Como parte de la migración, las reglas de alerta clásicas se convierten en reglas de alerta nueva equivalentes y se crean grupos de acciones.

- El formato de carga de notificación, así como las API para crear y administrar nuevas reglas de alerta es diferente de las reglas de alerta clásicas ya que admiten más características. Obtenga información sobre [cómo prepararse para la migración](alerts-prepare-migration.md).

- No se puede migrar algunas reglas de alerta clásicas mediante la herramienta. [Obtenga información sobre las reglas que no son que se puede migrar y vea cómo migrarlos](alerts-understand-migration.md#which-classic-alert-rules-can-be-migrated).

    > [!NOTE]
    > Proceso de migración no afectará a la evaluación de las reglas de alertas clásicas. Seguirán ejecutándose y enviando alertas hasta que se migran y empezar a evaluar las reglas de alerta nueva.


## <a name="how-to-use-the-migration-tool"></a>Cómo usar la herramienta de migración

El siguiente procedimiento describe cómo desencadenar la migración de las reglas de alertas clásicas en Azure portal:

1. En [Azure Portal](https://portal.azure.com), haga clic en **Monitor**.

2. Haga clic en **alertas** , a continuación, haga clic en **administrar reglas de alerta** o **ver las alertas clásicas**.

3. Haga clic en **migrar a nuevas reglas** para ir a la página de aterrizaje de la migración. Esta página muestra una lista de todas las suscripciones y el estado de migración para ellos.

    ![migración de aterrizaje](media/alerts-migration/migration-landing.png "migrar las reglas")

4. Todas las suscripciones que se pueden migrar mediante la herramienta se marcará como **listo para migrar**.

    > [!NOTE]
    > La herramienta de migración está implementando en fases en todas las suscripciones que usan reglas de alerta clásicas. En las primeras fases de puesta en servicio, es posible que vea algunas suscripciones como no está listo para la migración.

5. Seleccione una o varias suscripciones y haga clic en **obtener una vista previa de migración**

6. En esta página, puede ver los detalles de las reglas de alertas clásicas se migrarán para una suscripción a la vez. También puede **descargar los detalles de la migración de esta suscripción** en un formato CSV.

    ![vista previa de migración](media/alerts-migration/migration-preview.png "obtener una vista previa de migración")

7. Proporcionar uno o varios **direcciones de correo electrónico** para recibir una notificación de estado de la migración. Enviaremos un correo electrónico cuando se complete la migración o se necesita una acción del usuario.

8. Haga clic en **Iniciar migración**. Lea la información que se muestra en el cuadro de diálogo de confirmación y confirme si está listo para comenzar el proceso de migración.

    >[!IMPORTANT]
    > Una vez que inicia el proceso de migración de una suscripción, no podrá editar o crear reglas de alerta clásicas para la suscripción. Sin embargo, las reglas de alertas clásicas seguirán ejecutando y proporcionando alertas hasta que se migren. Esto es para garantizar la fidelidad entre las reglas de alertas clásicas y las reglas nuevas creadas durante la migración. Una vez completada la migración de su suscripción, no se puede usar las reglas de alerta clásicas ya.

    ![Confirmar de migración](media/alerts-migration/migration-confirm.png "confirmar Iniciar migración")

9. Como se complete la migración o necesita una acción del usuario, recibirá un correo electrónico en las direcciones de correo electrónico en el paso 8. Periódicamente también puede comprobar el estado de la página de aterrizaje de la migración en el portal.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="why-is-my-subscriptions-listed-as-not-ready-for-migration"></a>**¿Por qué mis suscripciones aparece como no preparado para la migración?**

La herramienta de migración se está implementando en fases a todos los clientes. En las primeras fases, la mayoría o todas las suscripciones pueden marcarse como **no está listo para la migración**. Sin embargo, por medio de abril, todas las suscripciones deberían estar listos para migrar.

Cuando una suscripción queda lista para la migración, los propietarios de suscripción recibirá un correo electrónico que le notifica la disponibilidad de la herramienta. Esté atento para esta notificación.

### <a name="who-can-trigger-the-migration"></a>**¿Quién puede desencadenar la migración?**

Los usuarios que tengan el rol de colaborador de supervisión asignado en el nivel de suscripción podrá desencadenar la migración. Obtenga más información sobre [Role Based Access Control para el proceso de migración](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-is-the-migration-going-to-take"></a>**¿Cuánto tiempo la migración va a necesitar?**

Para la mayoría de las suscripciones, migración normalmente se completa en una hora. Puede realizar un seguimiento del progreso de la migración desde la página de aterrizaje de la migración.  Durante este tiempo, por favor, se garantiza que las alertas todavía se están ejecutando en el sistema de alertas clásicas o uno nuevo.

### <a name="what-can-i-do-if-i-run-into-an-issue-during-migration"></a>**¿Qué puedo hacer si surge un problema durante la migración?**

Siga el [guía para ver los pasos de corrección para los problemas que se produzcan durante la migración de solución](alerts-understand-migration.md#common-issues-and-remediations). Si es necesaria ninguna acción del usuario para completar la migración, se le notificará sobre las direcciones de correo electrónico proporcionadas durante la migración.

## <a name="next-steps"></a>Pasos siguientes

- [Preparación para la migración](alerts-prepare-migration.md)
- [Comprender el funcionamiento de la herramienta de migración](alerts-understand-migration.md)
