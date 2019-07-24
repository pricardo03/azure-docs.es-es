---
title: Escalado automático de clústeres de Azure HDInsight (versión preliminar)
description: Usar la característica de escalabilidad automática de HDInsight para escalar clústeres automáticamente
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: hrasheed
ms.openlocfilehash: 6ec981164de0ff61b0e83d54255d046a1418ed96
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66000100"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Escalado automático de clústeres de Azure HDInsight (versión preliminar)

> [!Important]
> La característica de Escalabilidad automática solo funciona para clústeres de Spark, Hive y MapReduce creados después del 8 de mayo de 2019. 

La característica de escalabilidad automática de clústeres de Azure HDInsight escala o reduce verticalmente el número de nodos de trabajo. Actualmente no se pueden escalar otros tipos de nodos del clúster.  Durante la creación de un nuevo clúster de HDInsight, se puede establecer un número mínimo y máximo de nodos de trabajo. La escalabilidad automática luego supervisa los requisitos de recursos de la carga de análisis y escala o reduce verticalmente el número de nodos de trabajo. El uso de esta característica no tiene un cargo adicional.

## <a name="cluster-compatibility"></a>Compatibilidad con el clúster

En la tabla siguiente se describen las versiones y los tipos de clúster que son compatibles con la característica de escalabilidad automática.

| Versión | Spark | Hive | LLAP | HBase | Kafka | Storm | ML |
|---|---|---|---|---|---|---|---|
| HDInsight 3.6 sin ESP | Sí | Sí | No | No | No | No | Sin |
| HDInsight 4.0 sin ESP | Sí | Sí | No | No | No | No | Sin |
| HDInsight 3.6 sin ESP | Sí | Sí | No | No | No | No | Sin |
| HDInsight 3.6 sin ESP | Sí | Sí | No | No | No | No | Sin |

## <a name="how-it-works"></a>Cómo funciona

Puede elegir el escalado basado en carga o el escalado basado en programación para el clúster de HDInsight. El escalado basado en carga cambia el número de nodos del clúster, dentro de un intervalo establecido, para garantizar la utilización de CPU óptima y minimizar el costo de ejecución.

El escalado basado en programación cambia el número de nodos del clúster en función de las condiciones que se aplican en momentos específicos. Estas condiciones escalan el clúster a un número deseado de nodos.

### <a name="metrics-monitoring"></a>Supervisión de métricas

La escalabilidad automática supervisa continuamente el clúster y recopila las métricas siguientes:

* **Total Pending CPU** (Total de CPU pendiente): El número total de núcleos necesarios para iniciar la ejecución de todos los contenedores pendientes.
* **Total Pending Memory** (Total de memoria pendiente): La memoria total (en MB) necesaria para iniciar la ejecución de todos los contenedores pendientes.
* **Total Free CPU** (Total de CPU libre): La suma de todos los núcleos sin usar en los nodos de trabajo activos.
* **Total Free Memory** (Total de memoria libre): La suma de la memoria sin usar (en MB) en los nodos de trabajo activos.
* **Used Memory per Node** (Memoria usada por nodo): La carga en un nodo de trabajo. Un nodo de trabajo donde se usan 10 GB de memoria se considera bajo más carga que un trabajo con 2 GB de memoria usada.
* **Number of Application Masters per Node** (Número de patrones de aplicación por nodo): El número de contenedores de patrones de aplicación (AM) que se ejecutan en un nodo de trabajo. Un nodo de trabajo que hospeda dos contenedores de AM se considera más importante que un nodo de trabajo que no hospeda ninguno de estos contenedores.

Las métricas anteriores se comprueban cada 60 segundos. La escalabilidad automática toma decisiones de escalar o reducir verticalmente según estas métricas.

### <a name="load-based-cluster-scale-up"></a>Escalado vertical de clústeres basados en carga

Cuando se detectan las condiciones siguientes, Escalabilidad automática emitirá un solicitud de escalado vertical:

* El total de CPU pendiente es mayor que el total de CPU libre durante más de 3 minutos.
* El total de memoria pendiente es mayor que el total de memoria libre durante más de 3 minutos.

El servicio de HDInsight calcula cuántos nodos de trabajo nuevos se necesitan para cumplir con los requisitos actuales de CPU y memoria y, luego, emite una solicitud de escalado vertical para agregar el número de nodos necesario.

### <a name="load-based-cluster-scale-down"></a>Reducción vertical de clústeres basados en carga

Cuando se detectan las condiciones siguientes, Escalabilidad automática emitirá un solicitud de reducción vertical:

* El total de CPU pendiente es menor que el total de CPU libre durante más de 10 minutos.
* El total de memoria pendiente es menor que el total de memoria libre durante más de 10 minutos.

En función del número de contenedores de AM por nodo y de los requisitos actuales de CPU y memoria, Escalabilidad automática emite una solicitud para quitar un número determinado de nodos. El servicio detecta también qué nodos son candidatos para la eliminación en función de la ejecución del trabajo actual. En primer lugar, la operación de reducción vertical retira los nodos y, luego, los quita del clúster.

## <a name="get-started"></a>Introducción

### <a name="create-a-cluster-with-load-based-autoscaling"></a>Creación de un clúster con Escalabilidad automática basada en carga

Para habilitar la característica Escalabilidad automática con escalado basado en carga, complete estos pasos como parte del proceso de creación de clúster normal:

1. Seleccione **Personalizado (tamaño, configuración, aplicaciones)** lugar de **Creación rápida**.
1. En el paso 5 de **Personalizado** (**Tamaño del clúster**), active la casilla **Escalabilidad automática de nodos de trabajo**.
1. Seleccione la opción **Load-based** (Basada en carga) en **Tipo de escalabilidad automática**.
1. Escriba los valores deseados para estas propiedades:  

    * El **número inicial de nodos de trabajo**.  
    * El número **mínimo** de nodos de trabajo.  
    * El número **máximo** de nodos de trabajo.  

    ![Habilitación de la opción de escalabilidad automática basada en carga del nodo de trabajo](./media/hdinsight-autoscale-clusters/usingAutoscale.png)

El número inicial de nodos de trabajo debe estar comprendido entre los valores mínimo y máximo, ambos inclusives. Este valor define el tamaño inicial del clúster cuando se crea. El número mínimo de nodos de trabajo debe ser mayor que cero.

### <a name="create-a-cluster-with-schedule-based-autoscaling"></a>Creación de un clúster con Escalabilidad automática basada en programación

Para habilitar la característica Escalabilidad automática con escalado basado en programación, complete estos pasos como parte del proceso de creación de clúster normal:

1. Seleccione **Personalizado (tamaño, configuración, aplicaciones)** lugar de **Creación rápida**.
1. En el paso 5 de **Personalizado** (**Tamaño del clúster**), active la casilla **Escalabilidad automática de nodos de trabajo**.
1. Especifique el **número de nodos de trabajo**, que controla el límite para escalar verticalmente el clúster.
1. Seleccione la opción **Schedule-based** (Basada en programación) en **Tipo de escalabilidad automática**.
1. Haga clic en **Configurar** para abrir la ventana **Configuración de la escalabilidad automática**.
1. Seleccione la zona horaria y haga clic en **+ Agregar condición**.
1. Seleccione los días de la semana en que se deberá aplicar la condición nueva.
1. Edite la hora en que debería aplicarse la condición y el número de nodos a los que se debe escalar el clúster.
1. Si es necesario, agregue más condiciones.

    ![Habilitación de la opción de escalabilidad automática basada en programación del nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-schedule-creation.png)

El número de nodos debe estar entre 1 y el número de nodos de trabajo que especificó antes de agregar las condiciones.

### <a name="final-creation-steps"></a>Pasos finales de creación

Tanto para el escalado basado en carga como el basado en programación, seleccione el tipo de máquina virtual de los nodos de trabajo con un clic en **Tamaño del nodo de trabajo** y **Tamaño del nodo principal**. Después de elegir el tipo de máquina virtual para cada tipo de nodo, puede ver el intervalo de costo estimado para todo el clúster. Ajuste los tipos de máquina virtual que se ajusten a su presupuesto.

![Habilitación de la opción de escalabilidad automática basada en programación del nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-node-size-selection.png)

La suscripción tiene una cuota de capacidad para cada región. El número total de núcleos de los nodos principales junto con el número máximo de nodos de trabajo no puede superar la cuota de capacidad. Sin embargo, esta cuota tiene un límite flexible; sencillamente puede crear una incidencia de soporte técnico en cualquier momento para que la aumenten.

> [!Note]  
> Si se supera el límite de cuota de núcleos total, recibirá un mensaje de error que dice "the maximum node exceeded the available cores in this region, please choose another region or contact the support to increase the quota" (El nodo máximo superó los núcleos disponibles en esta región, elija otra región o póngase en contacto con soporte técnico para aumentar la cuota).

Para obtener más información sobre la creación de clústeres de HDInsight con Azure Portal, consulte [Crear clústeres basados en Linux en HDInsight con Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Creación de un clúster con una plantilla del Administrador de recursos

#### <a name="load-based-autoscaling"></a>Escalado automático basado en carga

Para crear un clúster de HDInsight con el escalado automático basado en carga de una plantilla de Azure Resource Manager, agregue un nodo `autoscale` a la sección `computeProfile` > `workernode` con las propiedades `minInstanceCount` y `maxInstanceCount`, tal como se muestra en el fragmento de código JSON a continuación.

```json
{
  "name": "workernode",
  "targetInstanceCount": 4,
  "autoscale": {
      "capacity": {
          "minInstanceCount": 2,
          "maxInstanceCount": 10
      }
  },
  "hardwareProfile": {
      "vmSize": "Standard_D13_V2"
  },
  "osProfile": {
      "linuxOperatingSystemProfile": {
          "username": "[parameters('sshUserName')]",
          "password": "[parameters('sshPassword')]"
      }
  },
  "virtualNetworkProfile": null,
  "scriptActions": []
}
```

Para más información sobre cómo crear clústeres con plantillas de Resource Manager, consulte [Creación de clústeres de Apache Hadoop basados en Windows en HDInsight mediante plantillas de Azure Resource Manager](hdinsight-hadoop-create-linux-clusters-arm-templates.md).  

#### <a name="schedule-based-autoscaling"></a>Escalado automático basado en programación

Para crear un clúster de HDInsight con el escalado automático basado en programación de una plantilla de Azure Resource Manager, agregue un nodo `autoscale` a la sección `computeProfile` > `workernode`. El nodo `autoscale` contiene una `recurrence` que tiene una `timezone` y una `schedule` que describe cuándo se aplicará el cambio.

```json
{
  "autoscale": {
    "recurrence": {
      "timeZone": "Pacific Standard Time",
      "schedule": [
        {
          "days": [
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday"
          ],
          "timeAndCapacity": {
            "time": "11:00",
            "minInstanceCount": 10,
            "maxInstanceCount": 10
          }
        },
      ]
    }
  },
  "name": "workernode",
  "targetInstanceCount": 4,
}
```

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Habilitación y deshabilitación de escalabilidad automática para un clúster en ejecución

Para habilitar la escalabilidad automática en un clúster en ejecución, seleccione **Tamaño del clúster** en **Configuración**. Luego, haga clic en **Enable autoscale** (Habilitar escalabilidad automática). Seleccione el tipo de escalabilidad automática que quiere y especifique las opciones de escalado basado en carga o basado en programación. Finalmente, haga clic en **Guardar**.

![Habilitación de la opción de escalabilidad automática basada en programación del nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-enable-running-cluster.png)

## <a name="best-practices"></a>Procedimientos recomendados

### <a name="choosing-load-based-or-schedule-based-scaling"></a>Elección del escalado basado en carga o basado en programación

Tenga en cuenta estos factores antes de tomar una decisión con respecto al modo que va a elegir:

* Varianza de carga: si la carga del clúster sigue un patrón coherente a horas específicas en días específicos. Si no es así, la programación basada en carga es una mejor opción.
* Requisitos de SLA: el escalado de escalabilidad automática es reactivo en lugar de predictivo. ¿Habrá una demora suficiente entre el momento en que la carga empieza a aumentar y el momento en que el clúster tiene que alcanzar su tamaño de destino? Si los requisitos de SLA son estrictos y la carga es un patrón fijo conocido, la mejor opción es "basado en programación".

### <a name="consider-the-latency-of-scale-up-or-scale-down-operations"></a>Consideración de la latencia de las operaciones de escalado o reducción verticales

Una operación de escalado puede tardar entre 10 y 20 minutos en completarse. Cuando configure una programación personalizada, planee esta demora. Por ejemplo, si necesita que el tamaño del clúster sea 20 a las 9:00 a.m., establezca el desencadenador de programación más temprano, a eso de las 8:30 a.m., para que la operación de escalado se complete antes de las 9:00 a.m.

### <a name="preparation-for-scaling-down"></a>Preparación para la reducción vertical

Durante el proceso de reducción vertical de un clúster, la característica Escalabilidad automática retirará los nodos para cumplir con el tamaño de destino. Si hay tareas en ejecución en esos nodos, Escalabilidad automática esperará hasta que se completen las tareas. Como cada nodo de trabajo también tiene un rol en HDFS, los datos temporales se desplazarán a los nodos restantes. Por tanto, debe asegurarse de que haya espacio suficiente en los nodos restantes para hospedar todos los datos temporales. 

Los trabajos en ejecución se seguirán ejecutando y se completarán. Los trabajos pendientes esperarán ser programados como normales con menos nodos de trabajo disponibles.

## <a name="monitoring"></a>Supervisión

### <a name="cluster-status"></a>Estado del clúster

El estado del clúster que aparece en Azure Portal puede ayudarlo a supervisar las actividades de escalabilidad automática.

![Habilitación de la opción de escalabilidad automática basada en carga del nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-cluster-status.png)

Todos los mensajes de estado del clúster que podría ver se explican en la lista siguiente.

| Estado del clúster | Explicación |
|---|---|
| En ejecución | El clúster funciona normalmente. Todas las actividades de Escalabilidad automática anteriores se completaron correctamente. |
| Actualizando  | La configuración de Escalabilidad automática del clúster se está actualizando.  |
| Configuración de HDInsight  | Una operación de escalado o de reducción verticales de un clúster está en curso.  |
| Error de actualización  | HDInsight encontró problemas durante la actualización de la configuración de Escalabilidad automática. Los clientes pueden elegir si reintentan la actualización o deshabilitan la escalabilidad automática.  |
| Error  | Error en el clúster y no se puede usar. Elimine este clúster y cree uno nuevo.  |

Para ver el número actual de nodos en el clúster, vaya al gráfico **Tamaño del clúster** en la página de **información general** del clúster, o bien haga clic en **Tamaño del clúster** en **Configuración**.

### <a name="operation-history"></a>Historial de operaciones

Puede ver el historial de escalado y reducción verticales del clúster como parte de las métricas del clúster. También puede enumerar todas las acciones de escalado durante el último día, semana u otro período de tiempo.

Seleccione **Métricas** en **Supervisión**. Luego, haga clic en **Agregar métrica** y **Número de trabajos activos** en el cuadro desplegable **Métrica**. Haga clic en el botón que se encuentra en la esquina superior derecha para cambiar el intervalo de tiempo.

![Habilitación de la opción de escalabilidad automática basada en programación del nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-chart-metric.png)


## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre los procedimientos recomendados para el escalado manual de clústeres en los [procedimientos recomendados de escalado](hdinsight-scaling-best-practices.md)
