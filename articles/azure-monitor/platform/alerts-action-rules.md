---
title: Reglas de acción para las alertas de Azure Monitor
description: Descripción de qué son las reglas de acción y cómo configurarlas y administrarlas.
author: anantr
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: 6e97826499842a257f6402bd5268edc4cd6a486e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66734919"
---
# <a name="action-rules-preview"></a>Reglas de acción (versión preliminar)

Las reglas de acciones le permiten definir acciones (o suprimir acciones) en cualquier ámbito del Administrador de recursos (Suscripción, Grupo de recursos o Recurso). Tienen diversos filtros que le permiten restringir el subconjunto específico de instancias de la alerta en el que desea actuar. 

## <a name="why-and-when-should-you-use-action-rules"></a>¿Por qué y cuándo deben usar reglas de acción?

### <a name="suppression-of-alerts"></a>Supresión de alertas

A menudo hay muchos escenarios donde sería útil suprimir las notificaciones generadas por las alertas, que podrían abarcar desde la supresión durante una ventana de mantenimiento planeado a la supresión durante el horario no comercial. Por ejemplo, el equipo responsable de "ContosoVM" desea suprimir las notificaciones de alerta durante el fin de semana próximo, puesto que "ContosoVM" está llevando a cabo un mantenimiento planeado. Aunque puede deshabilitar cada regla de alerta configurada en "ContosoVM" manualmente (y volver a habilitarla tras el mantenimiento), no es una experiencia sencilla. Las reglas de acciones le permiten definir la supresión de alertas a escala con la capacidad de configurar el período de supresión de forma flexible. Volviendo al ejemplo anterior, el equipo ahora puede definir una regla de acción en "ContosoVM" que suprimirá todas las notificaciones de alerta durante el fin de semana.


### <a name="actions-at-scale"></a>Acciones a escala

Aunque las reglas de alertas le permiten definir el grupo de acciones que se desencadena cuando se genera la alerta, los clientes a menudo tienden a disponer de un grupo de acciones comunes en su ámbito de operaciones. Por ejemplo, un equipo responsable del grupo de recursos "ContosoRG" probablemente definirá el mismo grupo de acciones para todas las reglas de alerta definidas dentro de "ContosoRG". Las reglas de acción permiten simplificar este proceso al permitirle definir acciones a escala, por lo que se puede activar un grupo de acciones para cualquier alerta generada en el ámbito configurado. Volviendo al ejemplo anterior, el equipo ahora puede definir una regla de acción en "ContosoRG" que desencadenará el mismo grupo de acciones para todas las alertas generadas dentro de él.


## <a name="configuring-an-action-rule"></a>Configuración de una regla de acciones

Para acceder a la característica, puede seleccionar **Administrar acciones** desde la página de inicio de Alertas, en Azure Monitor. Después, seleccione **Reglas de acción (versión preliminar)** . Puede acceder a ellas si selecciona **Reglas de acción (versión preliminar)** desde el panel de la página de inicio de Alertas.

![Reglas de acción desde la página de inicio de Azure Monitor](media/alerts-action-rules/action-rules-landing-page.png)

Seleccione **+ Nueva regla de acción**. 

![Incorporación de una nueva regla de acción](media/alerts-action-rules/action-rules-new-rule.png)

Como alternativa, también puede crear una regla de acción al configurar una regla de alerta.

![Incorporación de una nueva regla de acción](media/alerts-action-rules/action-rules-alert-rule.png)

Ahora debería ver abierto el flujo de creación de reglas de acción. Configure los elementos siguientes: 

![Nuevo flujo de creación de reglas de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow.png)

### <a name="scope"></a>Ámbito

En primer lugar, elija el ámbito, es decir, el recurso de destino, el grupo de recursos o la suscripción. También tiene la capacidad de realizar una selección múltiple de una combinación de cualquiera de las opciones anteriores (dentro de una única suscripción). 

![Ámbito de la regla de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow-scope.png)

### <a name="filter-criteria"></a>Criterios de filtro

Además, puede definir filtros para restringir aún más un subconjunto específico de las alertas en el ámbito definido. 

Los filtros disponibles son: 

* **Gravedad**: Seleccione uno o varios niveles de gravedad de alerta. Severity = Sev1 significa que la regla de acción es aplicable a todas las alertas con la gravedad 1.
* **Supervisar servicio**: filtro basado en el servicio de supervisión de origen. Esto también es una selección múltiple. Por ejemplo, Monitor Service = "Application Insights" significa que la regla de acción se aplica a todas las alertas basadas en "Application Insights".
* **Tipo de recurso**: filtro según un tipo de recurso específico. Esto también es una selección múltiple. Por ejemplo, Tipo de recurso = "Máquinas virtuales" significa que la regla de acción es aplicable a todas las máquinas virtuales.
* **Id. de regla de alerta**: permite filtrar por reglas de alerta específicas mediante el identificador del Administrador de recursos de la regla de alerta.
* **Condición del monitor**: filtro para las instancias de alerta con "Desencadenados" o "Resueltos" como la condición del monitor.
* **Descripción**: expresión regular que coincide dentro de la descripción que se define como parte de la regla de alerta.
* **Contexto de alerta (carga)** : expresión regular que coincide dentro de los campos del [contexto de alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-common-schema-definitions#alert-context-fields) de una instancia de alerta.

Estos filtros se aplican junto con otro. Por ejemplo, si define "Tipo de recurso" = "Máquinas virtuales" y "Gravedad" = "Sev0", se filtran todas las alertas con "Sev0" en solo mis máquinas virtuales. 

![Filtros de reglas de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow-filters.png)

### <a name="suppression-or-action-group-configuration"></a>Configuración del grupo de acciones o supresiones

A continuación, configure la regla de acción para la supresión de alertas o la compatibilidad con grupos de acciones. No puede elegir ambos. La configuración actúa en todas las instancias de alerta que coincidan con el ámbito y los filtros definidos previamente.

#### <a name="suppression"></a>Supresión

Si selecciona **supresión**, configure la duración de la supresión de notificaciones y acciones. Elija alguna de las acciones siguientes:
* **Desde ahora (siempre)** : suprime todas las notificaciones de forma indefinida.
* **A la hora programada**: suprime las notificaciones dentro de una duración limitada.
* **Con periodicidad**: suprime una programación de periodicidad, que puede ser diaria, semanal o mensual.

![Supresión de la regla de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow-suppression.png)

#### <a name="action-group"></a>Grupo de acciones

Si selecciona **Grupo de acciones** en el botón de alternancia, agregue un grupo de acciones existente o cree uno nuevo. 

> [!NOTE]
> Puede asociar un solo grupo de acciones a una regla de acción.

![Grupo de acciones de regla de acción](media/alerts-action-rules/action-rules-new-rule-creation-flow-action-group.png)

### <a name="action-rule-details"></a>Detalles de la regla de acción

Por último, configure los siguientes detalles de la regla de acción
* NOMBRE
* Grupo de recursos en el que se guardará
* DESCRIPCIÓN 

## <a name="example-scenarios"></a>Escenarios de ejemplo

### <a name="scenario-1-suppression-of-alerts-based-on-severity"></a>Escenario 1: Supresión de alertas en función de la gravedad

Contoso desea suprimir las notificaciones para todas las alertas de Sev4 en todas las máquinas virtuales dentro de su suscripción "ContosoSub" cada fin de semana.

**Solución:** Cree una regla de acción con
* Ámbito = "ContosoSub"
* Filtros
    * Gravedad = "Sev4"
    * Tipo de recurso = "Virtual Machines"
* Supresión con la periodicidad establecida en semanal y las opciones "Sábado" y "Domingo" activadas

### <a name="scenario-2-suppression-of-alerts-based-on-alert-context-payload"></a>Escenario 2: Supresión de alertas basada en el contexto de alerta (carga)

Contoso desea suprimir las notificaciones para todos los registros de alertas generadas para "Equipo-01" en "ContosoSub" indefinidamente ya que va a someterse a mantenimiento.

**Solución:** Cree una regla de acción con
* Ámbito = "ContosoSub"
* Filtros
    * Supervisar servicio = "Log Analytics"
    * Contexto de alerta (carga) contiene "Equipo-01"
* Supresión establecido en "Desde ahora (siempre)"

### <a name="scenario-3-action-group-defined-at-a-resource-group"></a>Escenario 3: Grupo de acciones definido en un grupo de recursos

Contoso ha definido [una alerta de métrica en un nivel de suscripción](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-metric-overview#monitoring-at-scale-using-metric-alerts-in-azure-monitor), pero desea definir las acciones que se desencadenan específicamente para las alertas generadas desde su grupo de recursos "ContosoRG".

**Solución:** Cree una regla de acción con
* Ámbito = "ContosoRG"
* Sin filtros
* Grupo de acciones establecido en "ContosoActionGroup"

> [!NOTE]
> **Los grupos de acciones definidos dentro de las reglas de acción y las reglas de alertas funcionan de forma independiente, sin ninguna desduplicación**. En el escenario descrito anteriormente, si hay un grupo de acciones definido para la regla de alerta, se desencadenará junto con el grupo de acciones definido en la acción de regla. 

## <a name="managing-your-action-rules"></a>Administración de las reglas de acción

Puede ver y administrar las reglas de acción desde la vista de lista como se muestra a continuación.

![Vista de lista de reglas de acción](media/alerts-action-rules/action-rules-list-view.png)

Desde aquí, puede habilitar, deshabilitar o eliminar reglas de acción a escala si activa la casilla de verificación junto a ellos. Al hacer clic en cualquier regla de acción, se abre su página de configuración, lo que permite actualizar su definición y activarla o desactivarla.

## <a name="best-practices"></a>Procedimientos recomendados

Las alertas de registro creadas con la opción ["número de resultados"](alerts-unified-log.md) generan **una sola instancia de alerta** con el resultado de búsqueda completo (que, por ejemplo, podría estar en varios equipos). En este escenario, si una regla de acción utiliza el filtro "Contexto de alerta (carga)", actuará sobre la instancia de alerta siempre que haya una coincidencia. En el escenario 2 según se describió anteriormente, si los resultados de búsqueda para la alerta de registro generada contienen "Equipo-01" y "Equipo-02", se suprime la notificación completa (es decir, no hay ninguna notificación generada para "Equipo-02").

![Reglas de acción y alertas del registro (número de resultados)](media/alerts-action-rules/action-rules-log-alert-number-of-results.png)

Para aprovechar mejor las alertas de registro con las reglas de acción, se aconseja crear alertas de registro con la opción ["unidades métricas"](alerts-unified-log.md) Con esta opción, las instancias independientes de la alerta se generan según el campo de grupo definido. A continuación, en el escenario 2, se generan instancias independientes de la alerta para "Equipo-01" y "Equipo 02". Con la regla de acción que se describe en el escenario, solo la notificación para "Equipo-01" podría suprimirse mientras la notificación para "Equipo-02" seguiría activándose con normalidad.

![Reglas de acción y alertas del registro (número de resultados)](media/alerts-action-rules/action-rules-log-alert-metric-measurement.png)

## <a name="faq"></a>Preguntas más frecuentes

* P: Al configurar una regla de acción, me gustaría ver todas las posibles reglas de acción de superposición de modo que evitara las notificaciones duplicadas. ¿Es posible esto?

    A. Una vez definido un ámbito al configurar una regla de acción, puede ver una lista de reglas de acción que se superponen en el mismo ámbito (si existe). Esta superposición puede ser una de las siguientes opciones:
    * Una coincidencia exacta: Por ejemplo, la regla de acción que va a definir y la regla de acción que se superpone están en la misma suscripción.
    * Un subconjunto: Por ejemplo, la regla de acción que va a definir se encuentra en una suscripción y la regla de acción que se superpone se encuentra en un grupo de recursos dentro de la suscripción.
    * Un superconjunto: Por ejemplo, la regla de acción que va a definir se encuentra en un grupo de recursos y la regla de acción que se superpone se encuentra en la suscripción que contiene el grupo de recursos.
    * Una intersección: Por ejemplo, la regla de acción que va a definir se encuentra en "VM1" y "VM2", y la regla de acción que se superpone, en "VM2" y "VM3".

    ![Reglas de acción que se superponen](media/alerts-action-rules/action-rules-overlapping.png)

* P: Al configurar una regla de alerta, ¿es posible saber si ya hay definida reglas de acción que podrían actuar en la regla de alerta que voy a definir?

    A. Una vez que defina el recurso de destino para la regla de alerta, puede ver la lista de reglas de acción que actúan en el mismo ámbito (si existe); para ello, haga clic en "Ver acciones configuradas" en la sección "Acciones". Esta lista se rellena en función de los siguientes escenarios para el ámbito:
    * Una coincidencia exacta: Por ejemplo, la regla de alerta que va a definir y la regla de acción están en la misma suscripción.
    * Un subconjunto: Por ejemplo, la regla de alerta que va a definir se encuentra en una suscripción y la regla de acción se encuentra en un grupo de recursos dentro de la suscripción.
    * Un superconjunto: Por ejemplo, la regla de alerta que va a definir se encuentra en un grupo de recursos y la regla de acción se encuentra en la suscripción que contiene el grupo de recursos.
    * Una intersección: Por ejemplo, la regla de alerta que va a definir se encuentra en "VM1" y "VM2", y la regla de acción, en "VM2" y "VM3".
    
    ![Reglas de acción que se superponen](media/alerts-action-rules/action-rules-alert-rule-overlapping.png)

* P: ¿Puedo ver las alertas que han sido suprimidas por una regla de acción?

    A. En la [página de la lista de alertas](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-managing-alert-instances), hay una columna adicional que puede elegir denominada "Estado de supresión". Si se suprimiera la notificación para una instancia de alerta, mostraría ese estado en la lista.

    ![Instancias de alertas suprimidas](media/alerts-action-rules/action-rules-suppressed-alerts.png)

* P: Si hay una regla de acción con un grupo de acciones y otra con la supresión activa en el mismo ámbito, ¿qué ocurre?

    A. **La supresión siempre tiene prioridad en el mismo ámbito**.

* P: ¿Qué ocurre si tengo un recurso supervisado en dos reglas de acción independientes? ¿Puedo obtener una o dos notificaciones? Por ejemplo, "VM2", en este escenario:

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1'
      action rule 'AR2' defined for 'VM2' and 'VM3' with action group 'AG1'

    A. Para cada alerta de "VM1" y "VM3", el grupo de acciones "AG1" se desencadenaría una vez. Para cada alerta en "VM2", el grupo de acciones "AG1" se desencadenaría dos veces (**las reglas de acción no desduplican las acciones**). 

* P: ¿Qué ocurre si tengo un recurso supervisado en dos reglas de acción independientes y una llama a la acción mientras la otra lo hace a la supresión? Por ejemplo, "VM2", en este escenario:

      action rule 'AR1' defined for 'VM1' and 'VM2' with action group 'AG1' 
      action rule 'AR2' defined for 'VM2' and 'VM3' with suppression

    A. Para cada alerta de "VM1", el grupo de acciones "AG1" se desencadenaría una vez. Se suprimirán las acciones y las notificaciones para todas las alertas en "VM2" y "VM3". 

* P: ¿Qué ocurre si tengo una regla de alerta y una regla de acción definidas para el mismo recurso que llama a grupos de acciones diferentes? Por ejemplo, "VM1", en este escenario:

      alert rule  'rule1' on          'VM1' with action group 'AG2'
      action rule 'AR1'   defined for 'VM1' with action group 'AG1' 
 
    A. Para cada alerta de "VM1", el grupo de acciones "AG1" se desencadenaría una vez. Cada vez que se desencadene la regla de alerta "rule1", también se desencadenará además "AG2". **Los grupos de acciones definidos dentro de las reglas de acción y las reglas de alertas funcionan de forma independiente, sin ninguna desduplicación**. 

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga más información sobre alertas en Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)
