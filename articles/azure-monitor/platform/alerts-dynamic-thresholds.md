---
title: Creación de alertas con umbrales dinámicos en Azure Monitor
description: Creación de alertas con umbrales dinámicos basados en aprendizaje automático
author: yanivlavi
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 11/29/2018
ms.author: yalavi
ms.reviewer: mbullwin
ms.openlocfilehash: 92a6d0f0cd9ef9a7d246624f89315a87a7fb26f9
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55097816"
---
# <a name="metric-alerts-with-dynamic-thresholds-in-azure-monitor-public-preview"></a>Alertas de métricas con umbrales dinámicos in Azure Monitor (versión preliminar pública)

La alerta de métricas con detección de umbrales dinámicos aprovecha el aprendizaje automático avanzado para obtener información acerca del comportamiento histórico de las métricas e identificar patrones y anomalías que indican problemas posibles para los servicios. Proporciona soporte técnico tanto de una interfaz de usuario simple como de operaciones a escala, ya que permite a los usuarios configurar reglas de alerta a través de la API de Azure Resource Manager de forma totalmente automática.

Una vez que se cree una regla de alerta, solo se activará solo cuando la métrica supervisada no se comporte según lo previsto, en función de sus umbrales personalizados.

Agradecemos sus comentarios, así que puede enviarlos a <azurealertsfeedback@microsoft.com>.

## <a name="why-and-when-is-using-dynamic-condition-type-recommended"></a>¿Por qué y cuándo se recomienda el uso de un tipo de condición dinámica?

1. **Alertas escalable** : las reglas de alertas de umbrales pueden crear umbrales personalizados para cientos de series de métricas a la vez. A pesar de ello, la operación es igual de sencilla que si se definiera una regla de alertas en una sola métrica. El uso de la interfaz de usuario o de la API de Azure Resource Manager reduce el número de reglas de alertas que hay que administrar. El enfoque escalable es especialmente útil cuando se trabaja con dimensiones de métricas o cuando se aplican a varios recursos, como a todos los recursos de la suscripción. Esto se traduce en un importante ahorro del tiempo que se dedica a la administración y creación de reglas de alertas. [Obtenga más información acerca de cómo configurar alertas de métricas con umbrales dinámicos mediante plantillas](alerts-metric-create-templates.md).

1. **Reconocimiento de patrones de métricas inteligentes**: mediante nuestra exclusiva tecnología Machine Learning podemos detectar automáticamente patrones de métrica y adaptar los cambios que se producen en las métricas con el tiempo, lo que a menudo puede incluir estacionalidad (cada hora, cada día o cada semana). La adaptación al comportamiento de las métricas con el paso del tiempo y la generación de alertas en función de las desviaciones de su patrón permite que no sea necesario conocer el umbral “correcto” de cada métrica. El algoritmo de Machine Learning que se usa en umbrales dinámicos está diseñado para evitar umbrales con ruido (poca precisión) o amplios (poco recuerdo) que no tengan un patrón esperado.

1. **Configuración intuitiva**: los umbrales dinámicos permiten configurar alertas de métricas mediante conceptos de alto nivel, lo que alivia la necesidad de tener un amplio conocimiento del dominio acerca de la métrica.

## <a name="how-to-configure-alerts-rules-with-dynamic-thresholds"></a>¿Cómo se configuran reglas de alertas con umbrales dinámicos?

Las alertas con umbrales dinámicos pueden configurarse a través de alertas de métricas en Azure Monitor. [Más información acerca de cómo configurar alertas de métricas](alerts-metric.md).

## <a name="how-are-the-thresholds-calculated"></a>¿Cómo se calculan los umbrales?

Los umbrales dinámicos aprenden continuamente los datos de las series de métricas e intentan modelarlos mediante un conjunto de algoritmos y métodos. Detecta en los datos patrones como la estacionalidad (cada hora, cada día o cada semana) y puede controlar métricas con ruido (como la memoria o la CPU del equipo), así como las métricas con poca dispersión (como la disponibilidad y la tasa de error).

Los umbrales se seleccionan de forma que una desviación de estos umbrales indica una anomalía en el comportamiento de la métrica.

## <a name="what-does-sensitivity-setting-in-dynamic-thresholds-mean"></a>¿Qué significa el valor "Sensibilidad" en los umbrales dinámicos?

La sensibilidad del umbral de alerta es un concepto de alto nivel que controla la cantidad de desviación del comportamiento de la métrica necesaria para desencadenar una alerta.
Esta opción no requiere conocimiento acerca del umbral estático similar a una métrica. Las opciones disponibles son:

- Alto: los umbrales serán estrictos y estarán próximos al patrón de la serie de métricas. La regla de alertas se desencadenará ante la mínima desviación, lo que generará más alertas.
- Mediano: umbrales menos estrechos y más equilibrados, menos alertas que con la sensibilidad alta (valor predeterminado).
- Bajo: los umbrales serán laxos y habrá más distancia desde el patrón de la serie de métricas. La regla de alerta solo se desencadenará si la desviación es grande, lo que generará menos alertas.

## <a name="what-are-the-operator-setting-options-in-dynamic-thresholds"></a>¿Cuáles son las opciones de configuración de "Operador" en los umbrales dinámicos?

La regla de alertas de los umbrales dinámicos puede crear umbrales personalizados en función del comportamiento de las métricas en los límites superior e inferior de uso de la misma regla de alerta.
Puede elegir que la alerta que se va a desencadenar en una de las tres condiciones siguientes:

- Mayor que el umbral superior o menor que el umbral inferior (valor predeterminado)
- Mayor que el umbral superior
- Menor que el umbral inferior

## <a name="what-do-the-advanced-settings-in-dynamic-thresholds-mean"></a>¿Qué significa la configuración avanzada de los umbrales dinámicos?

**Períodos de error**: los umbrales dinámicos también permiten configurar el número de infracciones necesarias para que se desencadene, que es el número mínimo de desviaciones requeridas en un período concreto para que el sistema genere una alerta (el valor predeterminado es cuatro desviaciones en 20 minutos). El usuario puede configurar los períodos de error y elegir los motivos por los que va a recibir alertas. Para ello solo debe cambiar los períodos de error y el período de tiempo. Esta capacidad reduce el ruido de las alertas que generan los picos transitorios. Por ejemplo: 

Para desencadenar una alerta cuando el problema es continuo durante 20 minutos, 4 veces consecutivas en un período dado de 5 minutos, utilice la siguiente configuración:

![La configuración de los períodos de error en un problema que se da de forma continua durante 20 minutos, 4 veces consecutivas en una agrupación de períodos dada de 5 minutos](media/alerts-dynamic-thresholds/0008.png)

Para desencadenar una alerta cuando se ha producido una infracción de un umbral dinámico en 20 minutos de los últimos 30 con un período de 5 minutos, utilice la siguiente configuración:

![La configuración de los períodos de error en un problema que se da durante 20 minutos, de los últimos 30, con una agrupación de períodos de 5 minutos](media/alerts-dynamic-thresholds/0009.png)

**Omitir los datos anteriores a**: los usuarios también tienen la opción de definir la fecha a partir de la que el sistema debe comenzar a calcular los umbrales. Un caso de uso típico puede producirse cuando un recurso se ejecutaba en modo de prueba y ahora se ha promocionado a servir una carga de trabajo de producción y, por lo tanto, no debe tenerse en cuenta el comportamiento de las métrica durante la fase de pruebas.

## <a name="will-slow-behavior-change-in-the-metric-trigger-an-alert"></a>¿Desencadenará una alerta el lento cambio de comportamiento lento en la métrica?

Probablemente no. Los umbrales dinámicos son buenos para detectar desviaciones importantes, en lugar de problemas de evolución lenta.

## <a name="how-much-data-is-used-to-preview-and-then-calculate-thresholds"></a>¿Cuántos datos se usan para obtener una vista previa y, después, calcular los umbrales?

Los umbrales que aparecen en el gráfico, antes de que se cree una regla de alertas en la métrica, se calculan en función los últimos diez días de datos históricos; una vez que se cree una alerta, los umbrales dinámicos adquirirán datos históricos adicionales que estén disponibles y aprenderán de manera continua, en función de los datos nuevos, a crear umbrales más precisos.

## <a name="dynamic-thresholds-best-practices"></a>Procedimientos recomendados para umbrales dinámicos

Los umbrales dinámicos se puede aplicar a cualquier plataforma o métrica personalizada de Azure Monitor y también se ha optimizado para las métricas comunes de aplicaciones y de infraestructura.
Los elementos siguientes son procedimientos recomendados sobre cómo configurar alertas en algunas de estas métricas con umbrales dinámicos.

### <a name="dynamic-thresholds-on-virtual-machine-cpu-percentage-metrics"></a>Umbrales dinámicos en las métricas de porcentaje de CPU de máquina virtual

1. En [Azure Portal](https://portal.azure.com), haga clic en **Monitor**. La vista Monitor consolida todas las opciones de configuración y todos los datos de supervisión en una vista.

2. Haga clic en **Alertas** y, a continuación, en **+ Nueva regla de alertas**.

    > [!TIP]
    > La mayoría de las hojas de recursos también tienen la opción **Alertas** en el menú de recursos de la sección **Supervisión**, de modo que también podría crear alertas desde allí.

3. Haga clic en **Seleccionar destino**, en el panel de contexto que se carga, y seleccione un recurso de destino sobre el que quiera alertar. Use los menús desplegables **Suscripción** y **Tipo de recurso "Máquinas virtuales"** para buscar el recurso que quiere supervisar. También puede utilizar la barra de búsqueda para buscar su recurso.

4. Una vez haya seleccionado un recurso de destino, haga clic en **Agregar condición**.

5. Seleccione **"Porcentaje de CPU"**.

6. De manera opcional, puede restringir la métrica ajustando **Período** y **Agregación**. No se recomienda usar el tipo de agregación "máxima" para este tipo de métrica ya que es menos representativo del comportamiento. Para el tipo de agregación "máxima", el umbral estático puede ser más adecuado.

7. Verá un gráfico para la métrica de las últimas 6 horas. Defina los parámetros de la alerta:
    1. **Tipo de condición**: elija la opción "Dinámico".
    1. **Sensibilidad**: elija una sensibilidad media o baja para reducir el ruido de las alertas.
    1. **Operador**: elija "mayor que" excepto si el comportamiento representa el uso de la aplicación.
    1. **Frecuencia**: considere la posibilidad de reducirla según el impacto empresarial de la alerta.
    1. **Períodos de error** (Opción avanzada) la ventana temporal debe ser al menos de 15 minutos. Por ejemplo, si se establece el período en cinco minutos, los períodos de error deben ser tres como mínimo.

8. El gráfico de métricas mostrará los umbrales calculados según los datos recientes.

9. Haga clic en **Done**(Listo).

10. Rellene los **Detalles de alertas** como **Nombre de la regla de alertas**, **Descripción** y **Gravedad**.

11. Agregue un grupo de acciones a la alerta, ya sea seleccionando un grupo de acciones existente o creando uno nuevo.

12. Haga clic en **Listo** para guardar la regla de alertas de métrica.

> [!NOTE]
> Las reglas de alertas de métricas creadas mediante el portal se crean en el mismo grupo de recursos que el recurso de destino.

### <a name="dynamic-thresholds-on-application-insights-http-request-execution-time"></a>Umbrales dinámicos en el tiempo de ejecución de solicitudes HTTP de Application Insights

1. En [Azure Portal](https://portal.azure.com), haga clic en **Monitor**. La vista Monitor consolida todas las opciones de configuración y todos los datos de supervisión en una vista.

2. Haga clic en **Alertas** y, a continuación, en **+ Nueva regla de alertas**.

    > [!TIP]
    > La mayoría de las hojas de recursos también tienen la opción **Alertas** en el menú de recursos de la sección **Supervisión**, de modo que también podría crear alertas desde allí.

3. Haga clic en **Seleccionar destino**, en el panel de contexto que se carga, y seleccione un recurso de destino sobre el que quiera alertar. Use los menús desplegables **Suscripción** y **Tipo de recurso "Application Insights"** para buscar el recurso que quiere supervisar. También puede utilizar la barra de búsqueda para buscar su recurso.

4. Una vez haya seleccionado un recurso de destino, haga clic en **Agregar condición**.

5. Seleccione la opción **"Tiempo de ejecución de solicitud HTTP"**.

6. De manera opcional, puede restringir la métrica ajustando **Período** y **Agregación**. No se recomienda usar el tipo de agregación "máxima" para este tipo de métrica ya que es menos representativo del comportamiento. Para el tipo de agregación "máxima", el umbral estático puede ser más adecuado.

7. Verá un gráfico para la métrica de las últimas 6 horas. Defina los parámetros de la alerta:
    1. **Tipo de condición**: elija la opción "Dinámico".
    1. **Operador**: elija "mayor que" para reducir el número de alertas que se desencadenan con una mejora de la duración.
    1. **Frecuencia**: considere la posibilidad de reducirla según el impacto empresarial de la alerta.

8. El gráfico de métricas mostrará los umbrales calculados según los datos recientes.

9. Haga clic en **Done**(Listo).

10. Rellene los **Detalles de alertas** como **Nombre de la regla de alertas**, **Descripción** y **Gravedad**.

11. Agregue un grupo de acciones a la alerta, ya sea seleccionando un grupo de acciones existente o creando uno nuevo.

12. Haga clic en **Listo** para guardar la regla de alertas de métrica.

> [!NOTE]
> Las reglas de alertas de métricas creadas mediante el portal se crean en el mismo grupo de recursos que el recurso de destino.
