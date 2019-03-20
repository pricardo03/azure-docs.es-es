---
title: Escalado automático de clústeres de HDInsight de Azure (versión preliminar)
description: Usar la característica de escalabilidad automática de HDInsight para escalar clústeres automáticamente
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: hrasheed
ms.openlocfilehash: 28f04f5ab3cf8310a6ee3828405910d34b31591b
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58227695"
---
# <a name="automatically-scale-azure-hdinsight-clusters-preview"></a>Escalado automático de clústeres de HDInsight de Azure (versión preliminar)

>[!Important]
>La característica de HDInsight Autoscale está actualmente en versión preliminar. Envíe un correo electrónico a hdiautoscalepm@microsoft.com para que el escalado automático habilitado para su suscripción.

La característica de escalabilidad automática de clústeres de Azure HDInsight escala o reduce verticalmente el número de nodos de trabajo en un clúster automáticamente según la carga dentro de un intervalo predefinido. Durante la creación de un nuevo clúster de HDInsight, se puede establecer un número mínimo y máximo de nodos de trabajo. La escalabilidad automática luego supervisa los requisitos de recursos de la carga de análisis y escala o reduce verticalmente el número de nodos de trabajo según corresponda. El uso de esta característica tiene un costo adicional.

## <a name="getting-started"></a>Introducción

### <a name="create-a-cluster-with-the-azure-portal"></a>Creación de un clúster con Azure Portal

> [!Note]
> La escalabilidad automática actualmente solo se admite para clústeres de Azure HDInsight Hive, MapReduce y Spark versión 3.6.

Para habilitar la característica de escalado automático, haga lo siguiente como parte del proceso de creación de clúster normal:

1. Seleccione **Personalizado (tamaño, configuración, aplicaciones)** lugar de **Creación rápida**.
2. En el paso 5 de **Personalizado** (**Tamaño del clúster**), marque la casilla **Escalabilidad automática de nodos de trabajo**.
3. Escriba los valores deseados para las siguientes propiedades:  

    * El **número inicial de nodos de trabajo**.  
    * El número **mínimo** de nodos de trabajo.  
    * El número **máximo** de nodos de trabajo.  

![Habilitación de la opción de escalabilidad automática de nodos de trabajo](./media/hdinsight-autoscale-clusters/usingAutoscale.png)

El número inicial de nodos de trabajo debe estar comprendido entre los valores mínimo y máximo, ambos inclusives. Este valor define el tamaño inicial del clúster cuando se crea. El número mínimo de nodos de trabajo debe ser mayor que cero.

Después de elegir el tipo de VM para cada tipo de nodo, podrá ver el intervalo de costo estimado para todo el clúster. A continuación, puede ajustar esta configuración para que se ajuste a su presupuesto.

La suscripción tiene una cuota de capacidad para cada región. El número total de núcleos de los nodos principales junto con el número máximo de nodos de trabajo no puede superar la cuota de capacidad. Sin embargo, esta cuota tiene un límite flexible; sencillamente puede crear una incidencia de soporte técnico en cualquier momento para que la aumenten.

> [!Note]  
> Si se supera el límite de cuota de núcleos total, recibirá un mensaje de error que dice "the maximum node exceeded the available cores in this region, please choose another region or contact the support to increase the quota" (El nodo máximo superó los núcleos disponibles en esta región, elija otra región o póngase en contacto con soporte técnico para aumentar la cuota).

Para obtener más información sobre la creación de clústeres de HDInsight con Azure Portal, consulte [Crear clústeres basados en Linux en HDInsight con Azure Portal](hdinsight-hadoop-create-linux-clusters-portal.md).  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Creación de un clúster con una plantilla del Administrador de recursos

Para crear un clúster de HDInsight con una plantilla de Azure Resource Manager, agregue un nodo `autoscale` a la sección `computeProfile` > `workernode` con las propiedades `minInstanceCount` y `maxInstanceCount`, tal como se muestra en el siguiente fragmento de JSON.

```json
{                            
    "name": "workernode",
    "targetInstanceCount": 4,
    "autoscale": {
        "minInstanceCount": 2,
        "maxInstanceCount": 10
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

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Habilitación y deshabilitación de escalabilidad automática para un clúster en ejecución

Puede habilitar o deshabilitar el escalado automático para los clústeres de HDInsight creados después del 1 de enero de 2019 a través del portal de Azure.

## <a name="monitoring"></a>Supervisión

Puede ver el historial de escalado vertical y horizontalmente de clúster como parte de las métricas de clúster. También puede enumerar todas las acciones de escalado durante el último día, semana o período de tiempo más largo.

## <a name="how-it-works"></a>Cómo funciona

### <a name="metrics-monitoring"></a>Supervisión de métricas

La escalabilidad automática supervisa continuamente el clúster y recopila las métricas siguientes:

1. **Total Pending CPU** (Total de CPU pendiente): El número total de núcleos necesarios para iniciar la ejecución de todos los contenedores pendientes.
2. **Total Pending Memory** (Total de memoria pendiente): La memoria total (en MB) necesaria para iniciar la ejecución de todos los contenedores pendientes.
3. **Total Free CPU** (Total de CPU libre): La suma de todos los núcleos sin usar en los nodos de trabajo activos.
4. **Total Free Memory** (Total de memoria libre): La suma de la memoria sin usar (en MB) en los nodos de trabajo activos.
5. **Used Memory per Node** (Memoria usada por nodo): La carga en un nodo de trabajo. Un nodo de trabajo donde se usan 10 GB de memoria se considera bajo más carga que un trabajo con 2 GB de memoria usada.
6. **Number of Application Masters per Node** (Número de patrones de aplicación por nodo): El número de contenedores de patrones de aplicación (AM) que se ejecutan en un nodo de trabajo. Un nodo de trabajo que hospeda dos contenedores de a. M., se considera más importante que un nodo de trabajo que hospeda cero contenedores AM.

Las métricas anteriores se comprueban cada 60 segundos. Escalado automático deberá tomar decisiones de escalado vertical y horizontalmente en función de estas métricas.

### <a name="cluster-scale-up"></a>Escalado del clúster

Cuando se detectan las condiciones siguientes, el escalado automático emitirá una solicitud de escalado vertical:

* El total de CPU pendiente es mayor que el total de CPU libre durante más de 1 minuto.
* El total de memoria pendiente es mayor que el total de memoria libre durante más de 1 minuto.

Se calcula que un cierto número de nuevos nodos de trabajo se necesita para cumplir los requisitos de CPU y memoria actuales y, a continuación, emitir una solicitud de escalado que suma ese número de nuevos nodos de trabajo.

### <a name="cluster-scale-down"></a>Reducción del clúster

Cuando se detectan las condiciones siguientes, el escalado automático emitirá una solicitud de reducción:

* El total de CPU pendiente es menor que el total de CPU libre durante más de 10 minutos.
* El total de memoria pendiente es menor que el total de memoria libre durante más de 10 minutos.

En función del número de contenedores de a. M. por nodo y los requisitos de memoria y CPU actual, escalado automático emitirá una solicitud para quitar un cierto número de nodos, especificando los nodos que son posibles candidatas para eliminarse. De manera predeterminada, se eliminarán dos nodos en un ciclo.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre los procedimientos recomendados para el escalado manual de clústeres en los [procedimientos recomendados de escalado](hdinsight-scaling-best-practices.md)
