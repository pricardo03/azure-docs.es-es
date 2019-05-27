---
title: Escalado automático de clústeres de HDInsight de Azure (versión preliminar)
description: Usar la característica de escalabilidad automática de HDInsight para escalar clústeres automáticamente
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: hrasheed
ms.openlocfilehash: 6ec981164de0ff61b0e83d54255d046a1418ed96
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66000100"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Escalado automático de clústeres de HDInsight de Azure (versión preliminar)

> [!Important]
> La característica de escalado automático solo funciona para los clústeres de Spark, Hive y MapReduce creados después del 8 de mayo de 2019. 

Característica de escalado automático del Azure HDInsight clúster escala automáticamente el número de nodos de trabajo en un clúster hacia arriba y abajo. No se puede escalar otros tipos de nodos del clúster actualmente.  Durante la creación de un nuevo clúster de HDInsight, se puede establecer un número mínimo y máximo de nodos de trabajo. Escalado automático, a continuación, supervisa los requisitos de recursos de la carga de análisis y se puede escalar el número de nodos de trabajo hacia arriba o hacia abajo. No hay ningún cargo adicional por esta característica.

## <a name="cluster-compatibility"></a>Compatibilidad de clúster

La tabla siguiente describen los tipos de clúster y las versiones son compatibles con la característica de escalado automático.

| Version | Spark | Hive | LLAP | HBase | Kafka | Storm | ML |
|---|---|---|---|---|---|---|---|
| HDInsight 3.6 sin ESP | Sí | Sí | No | No | No | No | No |
| HDInsight 4.0 sin ESP | Sí | Sí | No | No | No | No | No |
| HDInsight 3.6 con ESP | Sí | Sí | No | No | No | No | No |
| HDInsight 3.6 con ESP | Sí | Sí | No | No | No | No | No |

## <a name="how-it-works"></a>Funcionamiento

Puede elegir la escala basada en la carga o basadas en programación escalado para el clúster de HDInsight. Escalado basado en la carga cambia el número de nodos del clúster, dentro de un intervalo que se establece, para asegurar la utilización óptima de CPU y minimizar el costo de ejecución.

Cambios de escalado basado en programación el número de nodos del clúster según las condiciones que se aplica a horas específicas. Estas condiciones escala el clúster con un número de nodos deseado.

### <a name="metrics-monitoring"></a>Supervisión de métricas

La escalabilidad automática supervisa continuamente el clúster y recopila las métricas siguientes:

* **Total Pending CPU** (Total de CPU pendiente): El número total de núcleos necesarios para iniciar la ejecución de todos los contenedores pendientes.
* **Total Pending Memory** (Total de memoria pendiente): La memoria total (en MB) necesaria para iniciar la ejecución de todos los contenedores pendientes.
* **Total Free CPU** (Total de CPU libre): La suma de todos los núcleos sin usar en los nodos de trabajo activos.
* **Total Free Memory** (Total de memoria libre): La suma de la memoria sin usar (en MB) en los nodos de trabajo activos.
* **Used Memory per Node** (Memoria usada por nodo): La carga en un nodo de trabajo. Un nodo de trabajo donde se usan 10 GB de memoria se considera bajo más carga que un trabajo con 2 GB de memoria usada.
* **Number of Application Masters per Node** (Número de patrones de aplicación por nodo): El número de contenedores de patrones de aplicación (AM) que se ejecutan en un nodo de trabajo. Un nodo de trabajo que hospeda dos contenedores de a. M., se considera más importante que un nodo de trabajo que hospeda cero contenedores AM.

Las métricas anteriores se comprueban cada 60 segundos. Escalado automático toma decisiones de escalado vertical y horizontalmente en función de estas métricas.

### <a name="load-based-cluster-scale-up"></a>Escalado del clúster basado en la carga

Cuando se detectan las condiciones siguientes, el escalado automático emitirá una solicitud de escalado vertical:

* Total pendiente de la CPU es mayor que el total de CPU disponible durante más de 3 minutos.
* Total de memoria pendientes es mayor que el total de memoria libre durante más de 3 minutos.

El servicio HDInsight calcula cuántas nuevos nodos de trabajo son necesarios para satisfacer los requisitos de memoria y CPU actual y, a continuación, emite una solicitud de escalado vertical para agregar el número necesario de nodos.

### <a name="load-based-cluster-scale-down"></a>Reducción de clúster en función de carga

Cuando se detectan las condiciones siguientes, el escalado automático emitirá una solicitud de reducción:

* El total de CPU pendiente es menor que el total de CPU libre durante más de 10 minutos.
* El total de memoria pendiente es menor que el total de memoria libre durante más de 10 minutos.

En función del número de contenedores de a. M. por nodo y los requisitos de memoria y CPU actual, el escalado automático emite una solicitud para quitar un cierto número de nodos. El servicio también detecta que los nodos son candidatos para la eliminación según la ejecución de trabajo actual. La operación de reducción en primer lugar se encarga de desmantelar los nodos y, a continuación, los quita del clúster.

## <a name="get-started"></a>Introducción

### <a name="create-a-cluster-with-load-based-autoscaling"></a>Crear un clúster con el escalado automático basado en la carga

Para habilitar la característica de escalado automático con escalado basado en la carga, complete los pasos siguientes como parte del proceso de creación de clúster normal:

1. Seleccione **Personalizado (tamaño, configuración, aplicaciones)** lugar de **Creación rápida**.
1. En **personalizado** paso 5 (**tamaño del clúster**), compruebe el **escalado automático de nodo de trabajo** casilla de verificación.
1. Seleccione la opción **basado en la carga** en **escalado automático tipo**.
1. Escriba los valores deseados para las siguientes propiedades:  

    * El **número inicial de nodos de trabajo**.  
    * El número **mínimo** de nodos de trabajo.  
    * El número **máximo** de nodos de trabajo.  

    ![Habilitar la opción de escalado automático basado en la carga del nodo de trabajo](./media/hdinsight-autoscale-clusters/usingAutoscale.png)

El número inicial de nodos de trabajo debe estar comprendido entre los valores mínimo y máximo, ambos inclusives. Este valor define el tamaño inicial del clúster cuando se crea. El número mínimo de nodos de trabajo debe ser mayor que cero.

### <a name="create-a-cluster-with-schedule-based-autoscaling"></a>Crear un clúster con el escalado automático basado en programación

Para habilitar la característica de escalado automático con escalado basado en programación, complete los pasos siguientes como parte del proceso de creación de clúster normal:

1. Seleccione **Personalizado (tamaño, configuración, aplicaciones)** lugar de **Creación rápida**.
1. En **personalizado** paso 5 (**tamaño del clúster**), compruebe el **escalado automático de nodo de trabajo** casilla de verificación.
1. Escriba el **número de nodos de trabajo**, que controla el límite para escalar verticalmente el clúster.
1. Seleccione la opción **basadas en programación** en **escalado automático tipo**.
1. Haga clic en **configurar** para abrir el **configuración de escalado automático** ventana.
1. Seleccione la zona horaria y, a continuación, haga clic en **+ Agregar condición**
1. Seleccione los días de la semana que debería aplicarse la condición de nuevo.
1. Edite el tiempo que tardará la condición de efecto y el número de nodos que se debe escalar el clúster a.
1. Agregue más condiciones Si es necesario.

    ![Habilitar la opción de escalado automático basado en programación de nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-schedule-creation.png)

El número de nodos debe estar entre 1 y el número de nodos de trabajo que ha escrito antes de agregar condiciones.

### <a name="final-creation-steps"></a>Pasos de creación final

Para escalar tanto en función de carga basadas en programación, seleccione el tipo de máquina virtual para nodos de trabajo haciendo clic en **tamaño** y **tamaño de nodo principal**. Después de elegir el tipo de máquina virtual para cada tipo de nodo, puede ver el intervalo de costo estimado de todo el clúster. Ajustar los tipos de máquina virtual para ajustarse a su presupuesto.

![Habilitar la opción de escalado automático basado en programación de nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-node-size-selection.png)

La suscripción tiene una cuota de capacidad para cada región. El número total de núcleos de los nodos principales junto con el número máximo de nodos de trabajo no puede superar la cuota de capacidad. Sin embargo, esta cuota tiene un límite flexible; sencillamente puede crear una incidencia de soporte técnico en cualquier momento para que la aumenten.

> [!Note]  
> Si se supera el límite de cuota de núcleos total, recibirá un mensaje de error que dice 'el nodo máximo superado los núcleos disponibles en esta región, elija otra región o póngase en contacto con el soporte técnico para aumentar la cuota.'

Para obtener más información sobre la creación de clústeres de HDInsight con Azure Portal, consulte [Crear clústeres basados en Linux en HDInsight con Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Creación de un clúster con una plantilla del Administrador de recursos

#### <a name="load-based-autoscaling"></a>Escalado automático basado en la carga

Puede crear un clúster de HDInsight con el escalado automático basado en la carga una plantilla de Azure Resource Manager mediante la adición de un `autoscale` nodo a la `computeProfile`  >  `workernode` sección con las propiedades `minInstanceCount` y `maxInstanceCount` como se muestra en el siguiente fragmento de json.

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

Puede crear un clúster de HDInsight con el escalado automático basado en la programación de una plantilla de Azure Resource Manager, mediante la adición de un `autoscale` nodo a la `computeProfile`  >  `workernode` sección. El `autoscale` nodo contiene un `recurrence` que tiene un `timezone` y `schedule` que describe cuándo llevará a cabo el cambio.

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

Para habilitar el escalado automático en un clúster en ejecución, seleccione **tamaño del clúster** en **configuración**. A continuación, haga clic en **habilitar escalado automático**. Seleccione el tipo de escalado automático que desee y especifique las opciones de escalado basado en la carga o programación. Finalmente, haga clic en **Guardar**.

![Habilitar la opción de escalado automático basado en programación de nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-enable-running-cluster.png)

## <a name="best-practices"></a>Procedimientos recomendados

### <a name="choosing-load-based-or-schedule-based-scaling"></a>Elegir la escala en función de carga o basadas en programación

Tenga en cuenta los siguientes factores antes de tomar una decisión sobre qué modo para elegir:

* Varianza de carga: la carga del clúster sigue un patrón consistente en momentos específicos, en días específicos. Si no es así, basado en la carga de programación es una opción mejor.
* Requisitos del SLA: Escalado automático escala es reactiva en lugar de predicción. ¿Habrá un retraso entre suficiente cuando la carga comienza a aumentar y cuando el clúster debe estar en su tamaño de destino? Si hay requisitos estrictos de SLA y la carga es un patrón fijo conocido, 'programación según' es una opción mejor.

### <a name="consider-the-latency-of-scale-up-or-scale-down-operations"></a>Tenga en cuenta la latencia de la escala de seguridad o reducir verticalmente las operaciones

Puede tardar 10 a 20 minutos para completar una operación de escalado. Al configurar una programación personalizada, planee este retraso. Por ejemplo, si necesita el tamaño del clúster de 20 a las 9:00 A.M., establezca el desencadenador de programación a un momento anterior, como 8:30 A.M. para que se ha completado la operación de escalado por 9:00 AM.

### <a name="preparation-for-scaling-down"></a>Preparación para reducir verticalmente

Durante el proceso de escalado de clústeres, escalado automático de baja los nodos para cumplir con el tamaño de destino. Si se están ejecutando tareas en esos nodos, escalado automático esperará hasta que se completen las tareas. Puesto que cada nodo de trabajo también sirve para una función en HDFS, se desplazarán los datos temporales a los nodos restantes. Por lo que debe asegurarse de que hay suficiente espacio en los nodos restantes para hospedar todos los datos temporales. 

Los trabajos en ejecución seguirá ejecutan y completan. Los trabajos pendientes esperará que programarse como normal con menos nodos de trabajo disponibles.

## <a name="monitoring"></a>Supervisión

### <a name="cluster-status"></a>Estado del clúster

El estado del clúster aparecen en el portal de Azure puede ayudarle a supervisar las actividades de escalado automático.

![Habilitar la opción de escalado automático basado en la carga del nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-cluster-status.png)

Todos los mensajes de estado de clúster que podría ver se explican en la lista siguiente.

| Estado del clúster | Explicación |
|---|---|
| Ejecutando | El clúster está funcionando con normalidad. Todas las actividades anteriores del escalado automático han completado correctamente. |
| Actualizando  | Se está actualizando la configuración de escalado automático del clúster.  |
| Configuración de HdInsight  | Un clúster de escalado vertical o reducción de la operación está en curso.  |
| Error de actualización  | HDInsight encontrado problemas durante la actualización de la configuración de escalado automático. Los clientes pueden elegir volver a intentar la actualización o deshabilitar escalado automático.  |
| Error  | Algo va mal con el clúster y no es utilizable. Eliminar este clúster y crear uno nuevo.  |

Para ver el número actual de nodos del clúster, vaya a la **tamaño del clúster** del gráfico en el **Introducción** página para el clúster, o haga clic en **tamaño del clúster** en  **Configuración de**.

### <a name="operation-history"></a>Historial de la operación

Puede ver el historial de escalado vertical y horizontalmente de clúster como parte de las métricas de clúster. También puede enumerar todas las acciones de escalado durante el último día, semana u otro período de tiempo.

Seleccione **métricas** en **supervisión**. A continuación, haga clic en **agregar métrica** y **número de trabajadores activos** desde el **métrica** cuadro de lista desplegable. Haga clic en el botón en la esquina superior derecha para cambiar el intervalo de tiempo.

![Habilitar la opción de escalado automático basado en programación de nodo de trabajo](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-chart-metric.png)


## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre los procedimientos recomendados para el escalado manual de clústeres en los [procedimientos recomendados de escalado](hdinsight-scaling-best-practices.md)
