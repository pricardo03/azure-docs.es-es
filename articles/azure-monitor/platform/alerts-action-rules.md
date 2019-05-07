---
title: Reglas de acción para las alertas de Azure Monitor
description: Descripción de ¿cuáles son las reglas de acción y cómo configurar y administrarlos.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: e5d04fd136848684e866fae9768b252e3b6ca77f
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65138123"
---
# <a name="action-rules-preview"></a>Reglas de acción (versión preliminar)

Este artículo describe qué son las reglas de acción y cómo configurar y administrar.

## <a name="what-are-action-rules"></a>¿Cuáles son las reglas de acción?

Reglas de acciones le permiten definir acciones (o supresión de acciones) en cualquier ámbito de administrador de recursos (suscripción, grupo de recursos o recurso). Tienen una variedad de filtros que le permiten restringir al subconjunto específico de instancias de la alerta que desea actuar en. 

Con las reglas de acción, puede:

* Suprimir notificaciones y acciones si se han planeado las ventanas de mantenimiento o para fines de semana y vacaciones, en lugar de tener que deshabilitar cada regla de alerta individualmente.
* Definir acciones y las notificaciones a escala: En lugar de tener que definir un grupo de acciones individualmente para cada regla de alerta, ahora puede definir un grupo de acciones para desencadenar alertas generadas en cualquier ámbito. Por ejemplo, puedo elegir que el desencadenador de 'ContosoActionGroup' grupo de acción para todas las alertas generadas dentro de mi suscripción.

## <a name="configuring-an-action-rule"></a>Configurar una regla de acción

Puede acceder a la función seleccionando **administrar acciones** desde las alertas en la página de inicio en Azure Monitor. A continuación, seleccione **reglas de acción (versión preliminar)**. Puede tener acceso a ellos seleccionando **reglas de acción (versión preliminar)** desde el panel de la página de aterrizaje para las alertas.

![Reglas de acción desde la página de aterrizaje de Azure Monitor](media/alerts-action-rules/action-rules-landing-page.png)

Seleccione **+ nueva regla de acción**. 

![Agregar nueva regla de acción](media/alerts-action-rules/action-rules-new-rule.png)

Como alternativa, también puede crear una regla de acción al configurar una regla de alerta.

![Agregar nueva regla de acción](media/alerts-action-rules/action-rules-alert-rule.png)

Ahora debería ver el flujo de creación de reglas de acción Abrir. Configure los siguientes elementos: 

![Nuevo flujo de creación de reglas de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>Scope

En primer lugar, elija el ámbito, es decir, el recurso de destino, grupo de recursos o suscripción. También tiene la capacidad de realizar una selección múltiple una combinación de cualquiera de las opciones anteriores (dentro de una sola suscripción). 

![Ámbito de la regla de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>criterios de filtro

Además, puede definir filtros para restringir aún más abajo para un subconjunto específico de las alertas en el ámbito definido. 

Los filtros disponibles son: 

* **Gravedad**: Seleccione uno o más niveles de gravedad de alerta. Gravedad = gravedad 1 significa que la regla de acción es aplicable a todas las alertas con gravedad como gravedad 1.
* **Supervisar servicio**: Filtro basado en el servicio de supervisión de origen. Esto también es una selección múltiple. Por ejemplo, un Monitor de servicio = "Application Insights" significa que la regla de la acción se aplica a todos los "Application Insights" en función de las alertas.
* **Tipo de recurso**: Filtrar según un tipo de recurso específico. Esto también es una selección múltiple. Por ejemplo, el tipo de recurso = significa "Máquinas virtuales" que la regla de acción es aplicable a todas las máquinas virtuales.
* **Id. de regla de alerta**: Permite filtrar por las reglas de alerta específicas mediante el identificador del Administrador de recursos de la regla de alerta.
* **Supervisar la condición**: Filtro para instancias de la alerta con "Activado" o "Resuelto" como la condición del monitor.
* **Descripción**: Expresión regular de coincidencia dentro de la descripción que se define como parte de la regla de alerta.
* **Contexto de alerta (carga)**: Coincidencia de RegEx en la [contexto de alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) campos de una instancia de alerta.

Estos filtros se aplican junto a uno del otro. Por ejemplo, si se define en 'Tipo de recurso' = "Máquinas virtuales" y "Gravedad" = 'Sev0', he filtrado para todas las alertas de 'Sev0' en sólo mis máquinas virtuales. 

![Filtros de regla de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Configuración del grupo de acción o supresión

A continuación, configure la regla de acción para la supresión de alertas o compatibilidad con grupos de acción. No puede elegir ambos. La configuración actúa en todas las instancias de alerta que coincida con el ámbito definido previamente y filtros.

#### <a name="suppression"></a>Supresión

Si selecciona **supresión**, configurar la duración de la supresión de notificaciones y acciones. Elija una de las siguientes acciones:
* **Desde ahora (siempre)**: Suprime todas las notificaciones de forma indefinida.
* **A la hora programada**: Suprimir notificaciones dentro de una duración limitada.
* **Con una periodicidad**: Suprimir según una programación de periodicidad, que puede ser diaria, semanal o mensual.

![Supresión de la regla de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>Grupo de acciones

Si selecciona **grupo de acciones** en el botón de alternancia, agregue un grupo de acciones existente o crear uno nuevo. 

> [!NOTE]
> Puede asociar un solo grupo de acciones con una regla de acción.

![Grupo de acciones de regla de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Detalles de la regla de acción

Por último, configure los siguientes detalles de la regla de acción
* NOMBRE
* En el que se guardará el grupo de recursos
* DESCRIPCIÓN 

## <a name="example-scenarios"></a>Escenarios de ejemplo

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Escenario 1: Supresión de alertas en función de gravedad

Contoso desea suprimir notificaciones para todas las alertas de Sev4 en todas las máquinas virtuales dentro de su suscripción 'ContosoSub' cada fin de semana.

**Solución:** Cree una regla de acción con
* Ámbito = 'ContosoSub'
* Filtros
    * Gravedad = 'Sev4'
    * Tipo de recurso = "Virtual Machines"
* Supresión con periodicidad establecida en semanal y comprueban "Sábado" y "Domingo"

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Escenario 2: Supresión de alertas basadas en el contexto de alerta (carga)

Contoso desea suprimir notificaciones para todos los registros de las alertas generadas para 'Equipo-01' en 'ContosoSub' indefinidamente como va a través del mantenimiento.

**Solución:** Cree una regla de acción con
* Ámbito = 'ContosoSub'
* Filtros
    * Supervisar servicio = 'Iniciar análisis'
    * Contexto de alerta (carga) contiene 'Equipo-01'
* Supresión establecido en ' desde ahora (siempre)'

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Escenario 3: Grupo de acciones definido en un grupo de recursos

Contoso ha definido [una alerta de métrica en un nivel de suscripción](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview#monitoring-at-scale-using-metric-alerts-in-azure-monitor), pero desea definir las acciones que desencadenan alertas por separado para su grupo de recursos 'ContosoRG'.

**Solución:** Cree una regla de acción con
* Ámbito = 'ContosoRG'
* No hay filtros
* Grupo de acciones se establece en 'ContosoActionGroup'

## <a name="managing-your-action-rules"></a>Administrar las reglas de acción

Puede ver y administrar las reglas de acción de la vista de lista como se muestra a continuación.

![Vista de lista de reglas de acción](media/alerts-action-rules/action-rules-list-view.png)

Desde aquí, puede reglas habilitar/deshabilitar/eliminar acción a escala seleccionando la casilla de verificación junto a ellos. Al hacer clic en cualquier regla de acción se abre su página de configuración, lo que permite actualizar su definición y activarla o desactivarla.

## <a name="best-practices"></a>Procedimientos recomendados

Registrar las alertas creadas con el ['número de resultados'](https://docs.microsoft.com/azure-monitor/platform/alerts-unified-log) opción Generar **una sola instancia de alerta** con el resultado de búsqueda completa (que podría ser en varios equipos por ejemplo). En este escenario, si una regla de acción utiliza el filtro 'Contexto de alerta (carga)', actuará sobre la instancia de alerta, siempre hay una coincidencia. En el escenario 2 como se describió anteriormente, si los resultados de búsqueda para la alerta de registro generada contienen 'Equipo-01' y 'Equipo-02', se suprime la notificación completa (es decir, no hay ninguna notificación generada para 'Equipo-02' en absoluto).

![Las reglas de acción y las alertas del registro (número de resultados)](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

A alertas del registro aprovechar mejor con las reglas de acción, se aconseja crear alertas del registro con el ['unidades métricas'](https://docs.microsoft.com/azure-monitor/platform/alerts-unified-log) opción. Con esta opción, instancias independientes de la alerta se generan basándose en el campo de grupo definido. A continuación, en el escenario 2, instancias independientes de la alerta se generan para 'Equipo-01' y 'Equipo 02'. Con la regla de acción que se describe en el escenario, solo la notificación para 'Equipo-01' podría suprimirse mientras la notificación para 'Equipo-02' seguiría se activan con normalidad.

![Las reglas de acción y las alertas del registro (número de resultados)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>Preguntas más frecuentes

* P: Al configurar una regla de acción, gustaría ver todos los posibles superposición de acción de reglas de modo que evitar notificaciones duplicadas. ¿Es posible hacerlo?

    A. Una vez definido un ámbito al configurar una regla de acción, puede ver una lista de reglas de acción que se superponen en el mismo ámbito (si existe). Esta superposición puede ser una de las siguientes opciones:
    * Una coincidencia exacta: Por ejemplo, la regla de acción que se va a definir y la regla de acción que se superponen están en la misma suscripción.
    * Un subconjunto: Por ejemplo, es la regla de acción que se va a definir en una suscripción, y es la regla de acción que se superponen en un grupo de recursos dentro de la suscripción.
    * Un superconjunto: Por ejemplo, la regla de acción que se está definiendo se encuentra en un grupo de recursos, y es la regla de acción que se superponen en la suscripción que contiene el grupo de recursos.
    * Una intersección: Por ejemplo, se encuentra la regla de acción que se va a definir en 'VM1' y 'VM2' y se encuentra la regla de acción que se superponen en 'VM2' y 'VM3'.

    ![Reglas de acción que se superponen](media/alerts-action-rules/action-rules-overlapping.png)

* P: ¿Al configurar una regla de alerta, es posible saber si ya hay definido por las reglas de acción que puede actuar en la regla de alerta que defino?

    A. Una vez que defina el recurso de destino para la regla de alerta, puede ver la lista de reglas de acción que actúan en el mismo ámbito (si existe), haga clic en 'Vista había configurado acciones' en la sección "Acciones". Esta lista se rellena en función de los siguientes escenarios para el ámbito:
    * Una coincidencia exacta: Por ejemplo, la regla de alerta que se está definiendo y la regla de acción están en la misma suscripción.
    * Un subconjunto: Por ejemplo, la regla de alerta que se está definiendo se encuentra en una suscripción y la regla de acción se encuentra en un grupo de recursos dentro de la suscripción.
    * Un superconjunto: Por ejemplo, la regla de alerta que se está definiendo se encuentra en un grupo de recursos y la regla de acción se encuentra en la suscripción que contiene el grupo de recursos.
    * Una intersección: Por ejemplo, la regla de alerta que se está definiendo se encuentra en 'VM1' y 'VM2' y la regla de acción se encuentra en 'VM2' y 'VM3'.
    
    ![Reglas de acción que se superponen](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

* P: ¿Ver las alertas que se han suprimido por una regla de acción?

    A. En el [página de la lista de alertas](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-managing-alert-instances), hay una columna adicional que se puede elegir llamado "estado de supresión". Si se suprime la notificación para una instancia de alerta, mostraría ese estado en la lista.

    ![Suprime las instancias de alerta](media/alerts-action-rules/action-rules-suppressed-alerts.png)

* P: Si hay una regla de acción con un grupo de acciones y otra con supresión activa en el mismo ámbito, ¿qué ocurre?

    A. **Supresión siempre tiene prioridad en el mismo ámbito**.

* P: ¿Qué ocurre si tengo un recurso que se supervisan en dos reglas de acción independiente? ¿Puedo obtener una o dos notificaciones? Por ejemplo 'VM2' en este escenario:

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1'
      action rule 'AR2' defined for 'VM2' and 'VM3' with action group 'AG1'

    A. Para cada alerta de 'VM1' y 'VM3', 'AG1' del grupo de acciones se desencadenaría una vez. Para cada alerta en 'VM2', 'AG1' del grupo de acciones se desencadenará dos veces (**reglas acción desduplicar acciones**). 

* P: ¿Qué ocurre si tengo un recurso que se supervisan en dos reglas de acción independiente y llama a uno para la acción y otro para la supresión? Por ejemplo, 'VM2' en este escenario:

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1' 
      action rule 'AR2' defined for 'VM2' and 'VM3' with suppression

    A. Para cada alerta en 'VM1', 'AG1' del grupo de acciones se desencadenaría una vez. Se suprimirán las acciones y las notificaciones para todas las alertas en 'VM2' y 'VM3'. 

* P: ¿Qué ocurre si tengo una regla de alerta y una regla de acción definido para el mismo recurso de una llamada a grupos de acciones diferentes? Por ejemplo, 'VM1' en este escenario:

      alert rule  'rule1' on          'VM1' with action group 'AG2'
      action rule 'AR1'   defined for 'VM1' with action group 'AG1' 
 
    A. Para cada alerta en 'VM1', 'AG1' del grupo de acciones se desencadenaría una vez. Cada vez que se desencadena la regla de alerta 'rule1', también se desencadena además 'AG2'. **Grupos de acciones definen en las reglas de acción y las reglas de alertas funcionan de forma independiente, con ningún desduplicación**. 

## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre las alertas en Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)
