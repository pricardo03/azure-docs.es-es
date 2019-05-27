---
title: Migrar las alertas clásicas en Azure Monitor mediante la herramienta de migración voluntaria
description: Obtenga información sobre cómo usar la herramienta de migración voluntaria para migrar las reglas de alertas clásicas.
author: snehithm
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: snmuvva
ms.subservice: alerts
ms.openlocfilehash: 00229cca1d7fb238b330ec98cd35d0bb59bc821a
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66015618"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Usar la herramienta de migración voluntaria para migrar las reglas de alertas clásicas

Como [anunciaron](monitoring-classic-retirement.md), las alertas clásicas en Azure Monitor se retirará en septiembre de 2019 (era originalmente de 2019 de julio). Una herramienta de migración está disponible en el portal de Azure a los clientes que utilizan las reglas de alerta clásicas y que desean desencadenar la migración. En este artículo se explica cómo usar la herramienta de migración para migrar voluntariamente las reglas de alertas clásicas antes de que comience la migración automática en septiembre de 2019.

> [!NOTE]
> Debido a un retraso en la puesta en servicio de herramienta de migración, ha sido la fecha de retirada para la migración de las alertas clásicas [extiende hasta el 31 de agosto de 2019](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) desde la fecha anunciada originalmente del 30 de junio de 2019.

## <a name="benefits-of-new-alerts"></a>Ventajas de nuevas alertas

Las alertas clásicas están siendo remplazadas por alerta nueva y unificada en Azure Monitor. La nueva plataforma de alertas consta de las siguientes ventajas:

- Puede enviar alertas sobre una variedad de métricas multidimensionales para [muchos servicios de Azure más](alerts-metric-near-real-time.md#metrics-and-dimensions-supported).
- Soporte técnico de alertas de la nueva métrica [las reglas de alertas de múltiples recursos](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) que reducir en gran medida la sobrecarga de administrar muchas reglas.
- El mecanismo de notificación unificada, que es compatible con:
  - [Grupos de acciones](action-groups.md), un mecanismo de notificación modular que funciona con todos los nuevos tipos de alertas (métrica, registro y registro de actividad).
  - Nuevos mecanismos de notificación como conector ITSM, voz y SMS.
- El [alerta experiencia unificada](alerts-overview.md) incorpora todas las alertas en las señales que (métrica, registro y registro de actividad) en un solo lugar.

## <a name="before-you-migrate"></a>Antes de migrar

El proceso de migración convierte las reglas de alertas clásicas a las reglas de alerta nueva, equivalente y crea grupos de acciones. En la preparación, tenga en cuenta los siguientes aspectos:

- El formato de carga de notificación y las API para crear y administrar nuevas reglas de alerta son diferentes de los de las reglas de alerta clásicas porque admiten más características. [Obtenga información sobre cómo preparar la migración](alerts-prepare-migration.md).

- No se puede migrar algunas reglas de alerta clásicas mediante la herramienta. [Obtenga información sobre las reglas que no se pueden migrar y qué hacer con ellos](alerts-understand-migration.md#which-classic-alert-rules-can-be-migrated).

    > [!NOTE]
    > El proceso de migración no afecta a la evaluación de las reglas de alertas clásicas. Seguirán ejecutándose y enviando alertas hasta que se hayan migrado y las nuevas reglas de alerta surtan efecto.

## <a name="how-to-use-the-migration-tool"></a>Cómo usar la herramienta de migración

Para desencadenar la migración de las reglas de alertas clásicas en Azure portal, siga estos pasos:

1. En [portal Azure](https://portal.azure.com), seleccione **Monitor**.

1. Seleccione **alertas**y, a continuación, seleccione **administrar reglas de alerta** o **ver las alertas clásicas**.

1. Seleccione **migrar a nuevas reglas** para ir a la página de aterrizaje de la migración. Esta página muestra una lista de todas las suscripciones y su estado de migración:

    ![migración de aterrizaje](media/alerts-migration/migration-landing.png "migrar las reglas")

    Todas las suscripciones que se pueden migrar mediante la herramienta se marcan como **listo para migrar**.

    > [!NOTE]
    > La herramienta de migración está implementando en fases en todas las suscripciones que usan reglas de alerta clásicas. En las primeras fases del lanzamiento, es posible que vea algunas suscripciones marcadas como no preparadas para la migración.

1. Seleccione una o varias suscripciones y, a continuación, seleccione **obtener una vista previa de migración**.

    La página resultante muestra los detalles de las reglas de alertas clásicas se migrarán para una suscripción a la vez. También puede seleccionar **descargar los detalles de la migración de esta suscripción** para obtener los detalles en un formato CSV.

    ![vista previa de migración](media/alerts-migration/migration-preview.png "obtener una vista previa de migración")

1. Especifique una o varias direcciones de correo electrónico para recibir una notificación de estado de la migración. Una vez completada la migración, o si es necesaria ninguna acción del usuario, recibirá correo electrónico.

1. Seleccione **Iniciar migración**. Lea la información que se muestra en el cuadro de diálogo de confirmación y confirme que está listo para iniciar el proceso de migración.

    > [!IMPORTANT]
    > Después de iniciar la migración de una suscripción, no podrá editar o crear reglas de alerta clásicas para esa suscripción. Esta restricción garantiza que ningún cambio en las reglas de alertas clásicas se pierden durante la migración a las nuevas reglas. Aunque no podrá cambiar las reglas de alertas clásicas, aún seguirán para ejecutar y para proporcionar alertas hasta que migró. Una vez completada la migración de su suscripción, ya no puede usar las reglas de alerta clásicas.

    ![Confirmar de migración](media/alerts-migration/migration-confirm.png "confirmar Iniciar migración")

1. Cuando se completa la migración, o si es necesaria que la acción, recibirá un correo electrónico a las direcciones que proporcionó anteriormente. Puede comprobar periódicamente el estado en la página de aterrizaje de la migración en el portal.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="why-is-my-subscription-listed-as-not-ready-for-migration"></a>¿Por qué mi suscripción aparece como no está listo para la migración?

La herramienta de migración está implementando en los clientes en fases. En las primeras fases, la mayoría o todas las suscripciones pueden marcarse como **no está listo para la migración**. 

Cuando una suscripción queda lista para la migración, el propietario de la suscripción recibirá un mensaje de correo electrónico que indica que la herramienta está disponible. Esté atento para este mensaje.

### <a name="who-can-trigger-the-migration"></a>¿Quién puede desencadenar la migración?

Los usuarios que tengan el rol de colaborador de supervisión asignado en el nivel de suscripción pueden desencadenar la migración. [Más información sobre el Control de acceso basado en roles para el proceso de migración](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-will-the-migration-take"></a>¿Cuánto tiempo tardará la migración?

Migración completada para la mayoría de las suscripciones en menos de una hora. Puede mantener un seguimiento de estado de la migración en la página de aterrizaje de la migración. Durante la migración, se garantiza que las alertas todavía se está ejecutando en el sistema de alertas clásicas o en una nueva.

### <a name="what-can-i-do-if-i-run-into-a-problem-during-migration"></a>¿Qué puedo hacer si surge algún problema durante la migración?

Consulte la [Guía de solución](alerts-understand-migration.md#common-problems-and-remedies) para obtener ayuda con problemas que se produzcan durante la migración. Si es necesaria ninguna acción del usuario para completar la migración, se le notificará en las direcciones de correo electrónico que proporcionó al configurar la herramienta.

## <a name="next-steps"></a>Pasos siguientes

- [Preparar la migración](alerts-prepare-migration.md)
- [Comprender el funcionamiento de la herramienta de migración](alerts-understand-migration.md)
