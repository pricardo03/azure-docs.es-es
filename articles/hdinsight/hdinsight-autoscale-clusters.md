---
title: Escalado automático de clústeres de Azure HDInsight
description: Usar la característica de escalabilidad automática de HDInsight para escalar clústeres automáticamente
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: hrasheed
ms.openlocfilehash: 043c83e2039d87b1650ba17f770ce16a2ad2c13d
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54811169"
---
# <a name="automatically-scale-azure-hdinsight-clusters"></a>Escalado automático de clústeres de Azure HDInsight

La característica de escalabilidad automática de clústeres de Azure HDInsight escala o reduce verticalmente el número de nodos de trabajo en un clúster automáticamente según la carga dentro de un intervalo predefinido. Durante la creación de un nuevo clúster de HDInsight, se puede establecer un número mínimo y máximo de nodos de trabajo. La escalabilidad automática luego supervisa los requisitos de recursos de la carga de análisis y escala o reduce verticalmente el número de nodos de trabajo según corresponda. El uso de esta característica tiene un costo adicional.

## <a name="getting-started"></a>Introducción

### <a name="create-cluster-with-azure-portal"></a>Creación de un clúster con Azure Portal

> [!Note]
> La escalabilidad automática actualmente solo se admite para clústeres de Azure HDInsight Hive, MapReduce y Spark versión 3.6.

Siga los pasos de [Crear clústeres basados en Linux en HDInsight con el Portal de Azure](hdinsight-hadoop-create-linux-clusters-portal.md) y, al llegar al paso 5, **Tamaño del clúster**, seleccione **Escalabilidad automática de nodos de trabajo (versión preliminar)**  como se muestra a continuación. 

![Habilitación de la opción de escalabilidad automática de nodos de trabajo](./media/hdinsight-autoscale-clusters/worker-node-autoscale-option.png)

Al activar esta opción, puede especificar:

* El número inicial de nodos de trabajo
* El número mínimo de nodos de trabajo
* El número máximo de nodos de trabajo

El número inicial de nodos de trabajo debe estar comprendido entre los valores mínimo y máximo, ambos inclusives. Este valor define el tamaño inicial del clúster cuando se crea. El número mínimo de nodos de trabajo debe ser mayor que cero.

Después de elegir el tipo de VM para cada tipo de nodo, podrá ver el intervalo de costo estimado para todo el clúster. A continuación, puede ajustar esta configuración para que se ajuste a su presupuesto.

La suscripción tiene una cuota de capacidad para cada región. El número total de núcleos de los nodos principales junto con el número máximo de nodos de trabajo no puede superar la cuota de capacidad. Sin embargo, esta cuota tiene un límite flexible; sencillamente puede crear una incidencia de soporte técnico en cualquier momento para que la aumenten.

> [!Note]
> Si se supera el límite de cuota de núcleos total, recibirá un mensaje de error que dice "the maximum node exceeded the available cores in this region, please choose another region or contact the support to increase the quota" (El nodo máximo superó los núcleos disponibles en esta región, elija otra región o póngase en contacto con soporte técnico para aumentar la cuota).

### <a name="create-cluster-with-an-resource-manager-template"></a>Creación de un clúster con una plantilla de Resource Manager

Cuando crea un clúster de HDInsight con una plantilla de Resource Manager, debe agregar las siguientes opciones en la sección "computeProfile" "workernode":

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

### <a name="enable-and-disabling-autoscale-for-a-running-cluster"></a>Habilitación y deshabilitación de escalabilidad automática para un clúster en ejecución

Habilitar la escalabilidad automática para un clúster en ejecución no se admite durante la versión preliminar privada. Debe habilitarse durante la creación del clúster.

Deshabilitar la escalabilidad automática o modificar la configuración de escalabilidad automática no se admite en la versión preliminar privada. Debe eliminar el clúster y crear un nuevo clúster para eliminar o modificar la configuración.

## <a name="monitoring"></a>Supervisión

Puede ver el historial de escalado y reducción verticales del clúster como parte de las métricas del clúster. Puede enumerar todas las acciones de escalado durante el último día, semana o período de tiempo más largo.

## <a name="how-it-works"></a>Cómo funciona

### <a name="metrics-monitoring"></a>Supervisión de métricas

La escalabilidad automática supervisa continuamente el clúster y recopila las métricas siguientes:

1. **Total Pending CPU** (Total de CPU pendiente): El número total de núcleos necesarios para iniciar la ejecución de todos los contenedores pendientes.
2. **Total Pending Memory** (Total de memoria pendiente): La memoria total (en MB) necesaria para iniciar la ejecución de todos los contenedores pendientes.
3. **Total Free CPU** (Total de CPU libre): La suma de todos los núcleos sin usar en los nodos de trabajo activos.
4. **Total Free Memory** (Total de memoria libre): La suma de la memoria sin usar (en MB) en los nodos de trabajo activos.
5. **Used Memory per Node** (Memoria usada por nodo): La carga en un nodo de trabajo. Un nodo de trabajo donde se usan 10 GB de memoria se considera bajo más carga que un trabajo con 2 GB de memoria usada.
6. **Number of Application Masters per Node** (Número de patrones de aplicación por nodo): El número de contenedores de patrones de aplicación (AM) que se ejecutan en un nodo de trabajo. Un nodo de trabajo que hospeda contenedores de 2 AM se considera más importante que un nodo de trabajo que hospeda contenedores de 0 AM.

Las métricas anteriores se comprueban cada 60 segundos. La escalabilidad automática tomará decisiones de escalar verticalmente o reducir verticalmente según estas métricas.

### <a name="cluster-scale-up"></a>Escalado vertical de clústeres

Cuando se detectan las condiciones siguientes, Escalabilidad automática emitirá un solicitud de escalado vertical:

* El total de CPU pendiente es mayor que el total de CPU libre durante más de 1 minuto.
* El total de memoria pendiente es mayor que el total de memoria libre durante más de 1 minuto.

Calcularemos que se necesitan N nuevos nodos de trabajo para cumplir los requisitos actuales de CPU y memoria y, a continuación, emitiremos un solicitud de escalado vertical mediante la solicitud de N nuevos nodos de trabajo.

### <a name="cluster-scale-down"></a>Reducción vertical de clústeres

Cuando se detectan las condiciones siguientes, Escalabilidad automática emitirá un solicitud de reducción vertical:

* El total de CPU pendiente es menor que el total de CPU libre durante más de 10 minutos.
* El total de memoria pendiente es menor que el total de memoria libre durante más de 10 minutos.

En función del número de contenedores de AM por nodo, así como los requisitos actuales de CPU y memoria, Escalabilidad automática emitirá una solicitud para quitar N nodos, especificando los nodos que son posibles candidatas para eliminarse. De manera predeterminada, se eliminarán dos nodos en un ciclo.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre los procedimientos recomendados para el escalado manual de clústeres en los [procedimientos recomendados de escalado](hdinsight-scaling-best-practices.md)
