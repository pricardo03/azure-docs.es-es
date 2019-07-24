---
title: Escalado horizontal de un clúster de Azure Data Explorer
description: En este artículo se describen los pasos para escalar y reducir horizontalmente un clúster de Azure Data Explorer en función de los cambios en la demanda.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: 29bfcc42462a667850f0b2e1bbda3d29cd1597ab
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571266"
---
# <a name="manage-cluster-horizontal-scaling-to-accommodate-changing-demand"></a>Administración del escalado horizontal de clústeres para ajustarse a los cambios en la demanda

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión absoluta. El tamaño de un clúster estático puede provocar una infrautilización o sobreutilización, y ninguna de estas situaciones es la ideal.

Un enfoque mejor es *escalar* un clúster, es decir, agregar y quitar capacidad con los cambios en la demanda. Existen dos flujos de trabajo de escalado: 
* Escalado horizontal, también denominado reducción horizontal.
* Escalado vertical, también denominado reducción vertical.

En este artículo se explica el flujo de trabajo del escalado horizontal.

El escalado horizontal permite escalar el recuento de instancias automáticamente en función de las programaciones y reglas predefinidas. Especifique la configuración de escalabilidad automática para el clúster en Azure Portal, tal como se describe en este artículo.

## <a name="steps-to-configure-horizontal-scaling"></a>Pasos para configurar el escalado horizontal

En Azure Portal, vaya al recurso de clúster de Data Explorer. En el encabezado **Configuración**, seleccione **Escalar horizontalmente**. 

Seleccione el método de escalabilidad automática deseado: **Escala manual**, **Optimized autoscale** (Escalabilidad automática optimizada) o **Custom autoscale** (Escalabilidad automática personalizada).

### <a name="manual-scale"></a>Escala manual

La escala manual es la configuración predeterminada con la creación del clúster. Significa que el clúster tiene una capacidad de clúster estática que no cambiará automáticamente. Puede elegir la capacidad estática mediante la barra y no cambiará hasta la próxima vez que modifique la configuración de escalado horizontal del clúster.

   ![Método de escala manual](media/manage-cluster-horizontal-scaling/manual-scale-method.png)

### <a name="optimized-autoscale"></a>Escalabilidad automática optimizada

La escalabilidad automática optimizada es el método recomendado de escalabilidad automática. Pasos para configurar la escalabilidad automática optimizada:

1. Seleccione la opción Optimized autoscale (Escalabilidad automática optimizada), y elija un límite inferior y un límite superior para la cantidad de instancias del clúster: la escalabilidad automática se realizará entre estos límites.
2. Haga clic en Guardar.

   ![Método de escalabilidad automática optimizada](media/manage-cluster-horizontal-scaling/optimized-autoscale-method.png)

Después de hacer clic en Guardar, el mecanismo de escalabilidad automática optimizada empezará a funcionar y sus acciones estarán visibles en el registro de actividad del clúster. Este método de escalabilidad automática optimiza el rendimiento del clúster y los costos: si el clúster se acerca a un estado de infrautilización, se reducirá horizontalmente, lo que deja el rendimiento igual y reduce los costos, y si el clúster se acerca a un estado de sobreutilización, se escalará horizontalmente para asegurarse de que funcione correctamente.

### <a name="custom-autoscale"></a>Escalabilidad automática personalizada

Este método de escalabilidad automática personalizada le permite escalar dinámicamente el clúster en función de las métricas que especifique. El gráfico siguiente muestra el flujo y los pasos para configurar la escalabilidad automática personalizada. Más detalles después del gráfico.

1. En la casilla **Nombre de ajuste de escalado automático**, escriba un nombre, como *Escalado horizontal: utilización de caché*. 

   ![Regla de escalado](media/manage-cluster-horizontal-scaling/custom-autoscale-method.png)

2. En **Modo de escala**, seleccione **Escalado basado en una métrica**. Este modo proporciona escalado dinámico. También puede seleccionar **Escalar a un número específico de instancias**.

3. Seleccione **+Agregar una regla**.

4. En la sección**Escalar regla** de la derecha, proporcione valores para cada ajuste.

    **Criterios**

    | Configuración | Descripción y valor |
    | --- | --- |
    | **Agregación de tiempo** | Seleccione un criterio de agregación, como **Promedio**. |
    | **Nombre de la métrica** | Seleccione la métrica en la que quiera que se base la operación de escalado, como **Cache Utilization**. |
    | **Estadísticas de intervalo de agregación** | Elija entre **Promedio**, **Mínimo**, **Máximo** y **Suma**. |
    | **Operador** | Elija la opción adecuada, como **Mayor o igual que**. |
    | **Umbral** | Elija un valor apropiado. Por ejemplo, para el uso de memoria caché, el 80 % es un buen punto inicial. |
    | **Duración (en minutos)** | Elija un período apropiado para que el sistema busque al calcular las métricas. Comience con el valor predeterminado de 10 minutos. |
    |  |  |

    **Acción**

    | Configuración | Descripción y valor |
    | --- | --- |
    | **operación** | Elija la opción adecuada para reducir o escalar horizontalmente. |
    | **Recuento de instancias** | Elija el número de nodos o instancias que quiere agregar o quitar cuando se cumpla una condición de métrica. |
    | **Tiempo de finalización (minutos)** | Elija un intervalo de tiempo adecuado para esperar entre las operaciones de escalado. Comience con el valor predeterminado de cinco minutos. |
    |  |  |

5. Seleccione **Agregar**.

6. En la sección **Límites de instancia** de la izquierda, indique valores para cada configuración.

    | Configuración | Descripción y valor |
    | --- | --- |
    | **Mínimo** | Número de instancias por debajo del cual el clúster no se escalará, independientemente del uso. |
    | **Máximo** | Número de instancias por encima del cual el clúster no se escalará, independientemente del uso. |
    | **Valor predeterminado** | El número predeterminado de instancias. Esta configuración se utiliza si hay problemas con la lectura de las métricas de recursos. |
    |  |  |

7. Seleccione **Guardar**.

Ha configurado una operación de escalado horizontal para el clúster del Explorador de datos de Azure. Agregue otra regla para una operación de reducción horizontal. Si necesita ayuda por problemas relacionados con el escalado de un clúster, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="next-steps"></a>Pasos siguientes

* [Supervisión del rendimiento, el mantenimiento y el uso de Azure Data Explorer con métricas](using-metrics.md)
* [Administración del escalado vertical de clústeres](manage-cluster-vertical-scaling.md) para ajustar correctamente el tamaño de un clúster.
