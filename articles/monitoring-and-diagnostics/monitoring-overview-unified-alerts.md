---
title: Alertas unificadas en Azure Monitor
description: Descripción de las alertas unificadas en Azure que permiten administrar las alertas y las reglas de alertas a través de los servicios de Azure.
author: manishsm-msft
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/07/2018
ms.author: mamit
ms.component: alerts
ms.openlocfilehash: 30b2d60868702c6113612668b8e4cf9975aa2c40
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50962204"
---
# <a name="unified-alerts-in-azure-monitor"></a>Alertas unificadas en Azure Monitor

## <a name="overview"></a>Información general

> [!NOTE]
>  Actualmente, hay disponible en versión preliminar pública una nueva experiencia de alerta unificada que permite administrar las alertas de varias suscripciones y presenta los grupos inteligentes y los estados de alerta. Consulte la última sección de este artículo para ver una descripción de esta experiencia mejorada y del proceso para habilitarla.


En este artículo se describe la experiencia de alerta unificada en Azure Monitor. La [experiencia de alerta anterior](monitoring-overview-alerts.md) está disponible en la opción **Alertas (clásico)** en el menú de Azure Monitor. 

## <a name="features-of-the-unified-alert-experience"></a>Características de la experiencia de alerta unificada

La experiencia unificada presenta las siguientes ventajas en comparación con la experiencia clásica:

-   **Sistema de notificación mejorado**: los [grupos de acciones](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) son grupos de notificaciones y acciones que pueden reutilizarse en varias alertas. 
- **Experiencia de creación unificada**: las alertas y las reglas de alertas para las métricas, los registros y los registros de actividad en Azure Monitor, Log Analytics y Application Insights se pueden administrar en un solo lugar. 
- **Vista de alertas activadas de Log Analytics en Azure Portal**: ahora, las alertas de Log Analytics pueden verse con alertas de otros orígenes en Azure Portal. Anteriormente, las alertas de otros orígenes se encontraban en un portal independiente.
- **Separación de alertas activadas y reglas de alertas**: ahora, las reglas de alertas se distinguen de las alertas. Las reglas de alertas son las definiciones de una condición que desencadena una alerta. Una alerta es una instancia de la activación de una regla de alertas.
- **Flujo de trabajo mejorado**: la experiencia unificada de creación de alertas lo guía a través del proceso de configuración de una regla de alertas.
 
Las alertas de métrica presentan estas mejoras en comparación con las alertas de métrica clásicas:

-   **Latencia mejorada**: las alertas de métrica se pueden ejecutar con una frecuencia de una vez por minuto. Las alertas de métrica clásicas siempre se ejecutan con una frecuencia de una vez cada 5 minutos. Las alertas de registro todavía tienen un retraso de más de un minuto debido al tiempo que se tarda en ingerir los registros. 
-   **Compatibilidad con métricas multidimensionales**: puede enviar alertas sobre las métricas dimensionales, lo que significa que puede supervisar una instancia específica de la métrica.
-   **Más control sobre las condiciones de las métricas**: puede definir reglas de alertas más avanzadas que permitan supervisar el valor máximo, mínimo, promedio y total de las métricas.
-   **Supervisión combinada de varias métricas**: puede supervisar hasta dos métricas con una sola regla. Si ambas métricas incumplen sus respectivos umbrales durante el período especificado, se desencadena una alerta.
-   **Métricas de registros** (versión preliminar pública limitada): algunos datos de registro que van a Log Analytics ya se pueden extraer y convertir en métricas de Azure Monitor y luego en alertas como cualquier otra métrica. 


## <a name="alert-rules"></a>Reglas de alertas
La experiencia de alertas unificadas usa los conceptos siguientes para separar las reglas de alertas de las alertas a la vez que unifica la experiencia de creación en los distintos tipos de alertas.

| item | Definición |
|:---|:---|
| Regla de alertas | Definición de la condición para crear una alerta. Una regla de alertas consta de un _recurso de destino_, una _señal_, _criterios_ y una _lógica_. Una regla de alertas solo está activa si tiene un estado _activado_.
| Recurso de destino | Define las señales y los recursos específicos disponibles para las alertas. Un destino puede ser cualquier recurso de Azure.<br><br>Ejemplos: máquina virtual, cuenta de almacenamiento, conjunto de escalado de máquinas virtuales, área de trabajo de Log Analytics, recurso de Application Insights |
| Señal | Origen de datos que el recurso de destino emite. Los tipos de señales compatibles son *Métrica*, *Registro de actividad*, *Application Insights* y *Registro*. |
| Criterios | Combinación de _señal_ y _lógica_ aplicadas en un recurso de destino.<br><br>Ejemplos: Porcentaje de CPU > 70 %, tiempo de respuesta del servidor > 4 ms, recuento de resultados de una consulta de registros > 100, etc. |
| Lógica | Lógica definida por el usuario para verificar si la señal está dentro del alcance o los valores esperados. |
| . | Acción que se realizará cuando se activa la alerta. Se pueden producir varias acciones al desencadenarse una alerta. Estas alertas admiten grupos de acciones.<br><br>Ejemplos: enviar un mensaje de correo electrónico a una dirección de correo electrónico o llamar a una dirección URL de webhook. |
| Condición de supervisión | Indica si la condición que creó una alerta de métrica se resolvió. Las reglas de alertas de métrica toman una muestra de una métrica específica a intervalos regulares. Si se cumplen los criterios de la regla de alertas, se crea una alerta nueva con una condición "fired" (activada).  Cuando se vuelve a tomar una muestra de la métrica, si todavía se cumplen los criterios, todo sigue igual.  Pero si no se cumplen, la condición de la alerta cambia a "resolved" (resuelta). La próxima vez que se cumplan los criterios, se creará otra alerta con una condición "fired" (activada). |


## <a name="alert-pages"></a>Páginas de alertas
Las alertas unificadas proporcionan un lugar único para ver y administrar todas las alertas de Azure. En las secciones siguientes se describen las funciones de cada página individual de la experiencia unificada.

### <a name="alerts-overview-page"></a>Página de información general de las alertas
En la página de información **Alertas** se muestra un resumen agregado de todas las alertas activadas y el número total de reglas de alertas activadas. Al cambiar las suscripciones o los parámetros de filtro se actualizan los agregados y la lista de alertas desencadenadas.

 ![Información general de las alertas](./media/monitoring-overview-unified-alerts/alerts-preview-overview2.png) 

### <a name="alert-rules-management"></a>Administración de reglas de alertas
**Reglas** es una página única para administrar todas las reglas de alertas de todas las suscripciones de Azure. Se enumeran todas las reglas de alertas que se pueden ordenar según los recursos de destino, los grupos de recursos, el nombre de regla o el estado. En esta página también se pueden editar, habilitar o deshabilitar las reglas de alertas.

 ![alertas-reglas](./media/monitoring-overview-unified-alerts/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Crear una regla de alerta
Las alertas se pueden crear de manera coherente independientemente del tipo de señal o del servicio de supervisión. Todas las alertas activadas y los detalles relacionados están disponibles en una sola página.
 
Siga estos pasos para crear una regla de alertas nueva:
1. Elija el _destino_ de la alerta.
1. Seleccione la _señal_ de las señales disponibles para el destino.
1. Especifique la _lógica_ que se aplicará a los datos desde la señal.
 
Este proceso simplificado de creación ya no requiere que el usuario sepa las señales o el origen de supervisión admitidos antes de seleccionar un recurso de Azure. La lista de señales disponibles se filtra automáticamente según el recurso de destino seleccionado y lo ayuda a definir la lógica de la regla de alertas.

Puede obtener más información sobre cómo crear reglas de alertas en [Creación, visualización y administración de alertas mediante Azure Monitor](alert-metric.md).

Las alertas están disponibles a través de varios servicios de supervisión de Azure. Para obtener más información sobre cómo y cuándo usar cada uno de estos servicios, consulte [Supervisión de aplicaciones y recursos de Azure](../azure-monitor/overview.md). En la tabla siguiente se proporciona una lista de los tipos de reglas de alertas disponibles a través de Azure. También se muestra lo que admite actualmente la experiencia de alertas unificada.

| **Origen de supervisión** | **Tipo de señal**  | **Descripción** | 
|-------------|----------------|-------------|
| Azure Monitor | Métrica  | También se conocen como [alertas de métricas casi en tiempo real](monitoring-near-real-time-metric-alerts.md) y permiten evaluar las condiciones de métrica con una frecuencia de una vez por minuto y admiten reglas de varias métricas y de métricas multidimensionales. Hay disponible una lista de los tipos de recursos compatibles en [Nuevas alertas de métrica para los servicios de Azure de Azure Portal](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported).<br>Las [alertas de métrica clásicas](monitoring-overview-alerts.md) no son compatibles con la nueva experiencia de alertas. Puede encontrarlas en Alertas (clásicas) en Azure Portal. Las alertas clásicas admiten algunos tipos de métricas que todavía no se han migrado a las alertas más recientes. Para ver una lista completa, consulte las [métricas compatibles](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-supported-metrics). |
| Log Analytics | Registros  | Reciba notificaciones o ejecute acciones automáticas cuando una consulta de búsqueda de registros cumpla determinados criterios. Las alertas de Log Analytics se [copian en la experiencia nueva](monitoring-alerts-extend.md). Hay disponible una [versión preliminar de los *registros de Log Analytics como métricas*](monitoring-alerts-extend-tool.md). Esta versión preliminar permite tomar determinados tipos de registros, convertirlos en métricas y alertar sobre ellos mediante la nueva experiencia de alertas. Esta versión preliminar es útil si tiene registros que no son de Azure que quiere obtener junto con las métricas nativas de Azure Monitor. |
| Registros de actividad | Registro de actividades | Contiene los registros de todas las acciones de creación, actualización y eliminación que realizó el destino seleccionado. |
| Estado del servicio | Registro de actividades  | No se admite en las alertas unificadas. Consulte [Creación de alertas del registro de actividad en notificaciones del servicio](monitoring-activity-log-alerts-on-service-notifications.md).  |
| Application Insights | Registros  | Contiene registros con los detalles de rendimiento de la aplicación. Con la consulta de análisis, puede definir las condiciones para las acciones que se realizarán en función de los datos de la aplicación. |
| Application Insights | Métrica | No se admite en las alertas unificadas. Consulte [Alertas de métricas](../application-insights/app-insights-alerts.md). |
| Application Insights | Pruebas de disponibilidad web | No se admite en las alertas unificadas.  Consulte [Alertas de pruebas web](../application-insights/app-insights-monitor-web-app-availability.md). Disponible para cualquier sitio web instrumentado para enviar datos a Application Insights. Reciba una notificación cuando la disponibilidad o la capacidad de respuesta de un sitio web está por debajo de las expectativas. |

## <a name="enhanced-unified-alerts-public-preview"></a>Alertas unificadas mejoradas (versión preliminar pública)

Una experiencia de alertas unificadas mejoradas se publicó en versión preliminar pública para Azure Monitor el 1 de junio de 2018. Esta experiencia se basa en las ventajas de las [alertas unificadas](#overview), que se publicaron en marzo de 2018, y brinda la capacidad de administrar y agregar alertas individuales, así como de modificar el estado de las alertas. En esta sección se describen las características nuevas y cómo navegar en las páginas de alertas nuevas de Azure Portal.

### <a name="enhanced-unified-alerts"></a>Alertas unificadas mejoradas

La nueva experiencia proporciona las características siguientes que no están disponibles en la experiencia unificada clásica:

- **Ver alertas de varias suscripciones**: ahora puede ver y administrar instancias individuales de alertas de varias suscripciones en una sola vista.
- **Administrar el estado de las alertas**: las alertas ahora tienen un estado que indica si se confirma su cierre.
- **Organizar las alertas con los grupos inteligentes**: los grupos inteligentes agrupan de manera automática las alertas relacionadas para que pueda administrarlas como un conjunto en lugar de hacerlo de forma individual.

### <a name="enable-enhanced-unified-alerts"></a>Habilitación de las alertas unificadas mejoradas
Para habilitar la nueva experiencia de alertas unificadas, seleccione el banner que aparece en la parte superior de la página Alertas. Este proceso crea un almacén de alertas que incluye las alertas activadas en los últimos 30 días en los distintos dispositivos compatibles. Tras habilitar la nueva experiencia, puede cambiar entre esta experiencia y la anterior si hace clic en el banner.

> [!NOTE]
>  La habilitación inicial de la nueva experiencia puede tardar unos minutos.

![Banner](media/monitoring-overview-unified-alerts/opt-in-banner.png)

Todas las suscripciones a las que tiene acceso se inscriben al habilitar la nueva experiencia. Si bien se habilita toda la suscripción, solo los usuarios que seleccionan la nueva experiencia pueden verla. Los demás usuarios con acceso a la suscripción deberán habilitar la experiencia por separado.

Habilitar la nueva experiencia de alerta no afecta la configuración de los grupos de acciones ni las notificaciones en las reglas de alertas. Solo cambia la forma en que puede ver y administrar las instancias activadas de las alertas en Azure Portal.

### <a name="smart-groups"></a>Grupos inteligentes
Los grupos inteligentes reducen el ruido, ya que permiten administrar las alertas relacionadas como una sola unidad en lugar de como alertas individuales. Puede ver los detalles de los grupos inteligentes y establecer el estado de manera similar a las alertas. Cada alerta forma parte de un solo grupo inteligente.

Los grupos inteligentes se crean de manera automática a través del aprendizaje automático para combinar las alertas relacionadas que representan un solo problema. Cuando se crea una alerta, el algoritmo la agrega a un grupo inteligente nuevo o a un grupo inteligente existente según información como los patrones históricos, las propiedades similares y la estructura similar. 

Actualmente, el algoritmo solo considera las alertas provenientes del mismo servicio de supervisión dentro de una suscripción. Los grupos inteligentes pueden reducir hasta un 99 por ciento de ruido de alertas a través de esta consolidación. Puede ver la razón por la que las alertas se incluyeron en un grupo en la página de detalles del grupo inteligente.

El nombre de un grupo inteligente es el nombre de su primera alerta. No se puede crear ni cambiar el nombre de un grupo inteligente.


### <a name="alert-states"></a>Estados de alerta
Las alertas unificadas mejoradas presentan el concepto de estado de alerta. Puede establecer el estado de una alerta para especificar dónde se encuentra en el proceso de resolución. Cuando se crea una alerta, tiene un estado de *Nuevo*. Puede cambiar el estado cuando se confirma una alerta y cuando se cierra. Todos los cambios de estado se almacenan en el historial de la alerta.

Se admiten los siguientes estados de alerta.

| Estado | DESCRIPCIÓN |
|:---|:---|
| Nuevo | Se acaba de detectar el problema y todavía no se ha revisado. |
| Confirmado | Un administrador revisó la alerta y empezó a trabajar en ella. |
| Closed | Se resolvió el problema. Después de cerrar una alerta, puede volver a abrirla mediante el cambio a otro estado. |

El estado de una alerta es distinto de la condición de supervisión. Las reglas de alertas de métrica pueden establecer una alerta en una condición de _resuelta_ cuando ya no se cumple la condición de error. El estado de alerta lo establece el usuario y es independiente de la condición de supervisión. Si bien el sistema puede establecer la condición de supervisión en "resolved" (resuelta), el estado de la alerta no cambia hasta que el usuario lo cambia.

#### <a name="change-the-state-of-an-alert-or-smart-group"></a>Cambio del estado de una alerta o un grupo inteligente
Puede cambiar el estado de una alerta individual o administrar varias alertas juntas si establece el estado de un grupo inteligente.

Para cambiar el estado de una alerta, seleccione **Cambiar el estado de alerta** en la vista de detalle de la alerta. También puede cambiar el estado de un grupo inteligente seleccionando **Cambiar estado del grupo inteligente** en su vista de detalle. Cambie el estado de varios elementos a la vez seleccionándolos en primer lugar en una vista de lista y, a continuación, mediante la selección de **Cambiar estado** en la parte superior de la página. 

En ambos casos, seleccione un estado nuevo en el menú desplegable. A continuación, puede proporcionar un comentario. Si cambia un solo elemento, también tiene la opción de aplicar los mismos cambios a todas las alertas del grupo inteligente.

![Cambiar estado](media/monitoring-overview-unified-alerts/change-tate.png)

### <a name="alerts-page"></a>Página de alertas
La página Alertas predeterminada brinda un resumen de las alertas que se crean dentro un período de tiempo determinado. Muestra el total de alertas para cada gravedad con columnas que identifican el número total de alertas en cada estado para cada gravedad. Seleccione cualquiera de las gravedades para abrir la página [Todas las alertas](#all-alerts-page) filtrada según esa gravedad.

![Página de alertas](media/monitoring-overview-unified-alerts/alerts-page.png)

Para filtrar esta vista, seleccione valores en los menús desplegables que aparecen en la parte superior de la página.

| Columna | DESCRIPCIÓN |
|:---|:---|
| Subscription | Seleccione hasta cinco suscripciones a Azure. Solo las alertas de las suscripciones seleccionadas se incluyen en la vista. |
| Grupos de recursos | Seleccione un solo grupo de recursos. Solo las alertas con destinos en el grupo de recursos seleccionado se incluyen en la vista. |
| Intervalo de tiempo | Solo las alertas activadas dentro del período de tiempo seleccionado se incluyen en la vista. Los valores compatibles son Última hora, Últimas 24 horas, Últimos 7 días y Últimos 30 días. |

Seleccione los valores siguientes en la parte superior de la página Alertas para abrir otra página.

| Valor | DESCRIPCIÓN |
|:---|:---|
| Total de alertas | Número total de las alertas que coinciden con los criterios seleccionados. Seleccione este valor para abrir sin ningún filtro la vista Todas las alertas. |
| Grupos inteligentes | Número total de grupos inteligentes creados a partir de las alertas que coinciden con los criterios seleccionados. Seleccione este valor para abrir la lista de grupos inteligentes en la vista Todas las alertas.
| Total de reglas de alertas | Número total de reglas de alertas en la suscripción y el grupo de recursos que seleccionó. Seleccione este valor para abrir la vista Reglas filtrada en la suscripción y el grupo de recursos que seleccionó.


### <a name="all-alerts-page"></a>Página Todas las alertas 
Al usar la página Todas las alertas, puede ver una lista de las alertas que se crearon dentro del período de tiempo seleccionado. Puede ver una lista de las alertas individuales o una lista de los grupos inteligentes que contienen las alertas. Seleccione el banner en la parte superior de la página para alternar entre las vistas.

![Página Todas las alertas](media/monitoring-overview-unified-alerts/all-alerts-page.png)

Para filtrar la vista, seleccione los valores siguientes en los menús desplegables que aparecen en la parte superior de la página.

| Columna | DESCRIPCIÓN |
|:---|:---|
| Subscription | Seleccione hasta cinco suscripciones a Azure. Solo las alertas de las suscripciones seleccionadas se incluyen en la vista. |
| Grupos de recursos | Seleccione un solo grupo de recursos. Solo las alertas con destinos en el grupo de recursos seleccionado se incluyen en la vista. |
| Tipo de recurso | Seleccione uno o varios tipos de recurso. Solo las alertas con destinos del tipo seleccionado se incluyen en la vista. Esta columna solo está disponible tras especificar un grupo de recursos. |
| Recurso | Seleccione un recurso. Solo las alertas con ese recurso como destino se incluyen en la vista. Esta columna solo está disponible tras especificar un tipo de recurso. |
| Gravedad | Seleccione un nivel de gravedad de alerta o seleccione *Todo* para incluir alertas de todos los niveles de gravedad. |
| Condición de supervisión | Seleccione una condición de supervisión o seleccione *Todo* para incluir alertas de condiciones. |
| Estado de alerta | Seleccione un estado de alerta o seleccione *Todo* para incluir alertas de estados. |
| Servicio de supervisión | Seleccione un servicio o seleccione *Todo* para incluir todos los servicios. Solo se incluyen las alertas que se crean mediante las reglas que usan ese servicio como destino. |
| Intervalo de tiempo | Solo las alertas activadas dentro del período de tiempo seleccionado se incluyen en la vista. Los valores compatibles son Última hora, Últimas 24 horas, Últimos 7 días y Últimos 30 días. |

Seleccione **Columnas** en la parte superior de la página para seleccionar las columnas que quiere mostrar. 

### <a name="alert-detail-page"></a>Página Detalles de la alerta
La página de detalles de la alerta aparece cuando se selecciona una alerta. Proporciona detalles de la alerta y permite cambiar su estado.

![Detalles de la alerta](media/monitoring-overview-unified-alerts/alert-detail.png)

La página Detalles de la alerta incluye las secciones siguientes.

| Sección | DESCRIPCIÓN |
|:---|:---|
| Essentials | Muestra las propiedades y otra información importante sobre la alerta. |
| Historial | Muestra cada acción que realiza la alerta y los cambios hechos en la alerta. Se limita actualmente a los cambios de estado. |
| Grupo inteligente | Información sobre el grupo inteligente en que se incluye la alerta. El *recuento de alertas* se refiere al número de alertas incluidas en el grupo inteligente. Esto incluye las otras alertas del mismo grupo inteligente que se crearon en los últimos 30 días.  Esto es independiente del filtro de tiempo en la página de la lista de alertas. Seleccione una alerta para ver sus detalles. |
| Más detalles | Muestra información contextual adicional para la alerta que habitualmente es específica para el tipo de origen que creó la alerta. |


### <a name="smart-group-detail-page"></a>Página Detalles del grupo inteligente
La página de detalles del grupo inteligente se muestra al seleccionar un grupo inteligente. Proporciona detalles acerca del grupo inteligente, incluido el razonamiento que se usó para crear el grupo, y le permite cambiar su estado.
 
![Detalles del grupo inteligente](media/monitoring-overview-unified-alerts/smart-group-detail.png)


La página de detalles del grupo inteligente incluye las secciones siguientes.

| Sección | DESCRIPCIÓN |
|:---|:---|
| Alertas | Muestra las alertas individuales que se incluyen en el grupo inteligente. Seleccione una alerta para abrir su página de detalles de la alerta. |
| Historial | Muestra cada acción que realiza el grupo inteligente y los cambios hechos en él. Se limita actualmente a los cambios de estado y a los cambios de pertenencia de una alerta. |

## <a name="next-steps"></a>Pasos siguientes
- [Obtenga información sobre cómo usar la nueva experiencia de Alertas para crear, ver y administrar alertas](alert-metric.md).
- [Obtenga información sobre las alertas de registros en la experiencia de Alertas](monitor-alerts-unified-log.md).
- [Obtenga información sobre las alertas de métricas en la experiencia de Alertas](monitoring-near-real-time-metric-alerts.md).
- [Obtenga información sobre las alertas de registros de actividad en la experiencia de Alertas](alert-activity-log.md).
