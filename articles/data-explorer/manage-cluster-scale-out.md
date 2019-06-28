---
title: Escalado horizontal de un clúster de Azure Data Explorer
description: En este artículo se describen los pasos para escalar y reducir horizontalmente un clúster de Azure Data Explorer en función de los cambios en la demanda.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/05/2019
ms.openlocfilehash: 24bbddd28943adc929fbaea456eeae8165db290c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60758677"
---
# <a name="manage-cluster-scale-out-to-accommodate-changing-demand"></a>Administración del escalado horizontal de clúster para ajustarse a los cambios en la demanda

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión absoluta. El tamaño de un clúster estático puede provocar una infrautilización o sobreutilización, y ninguna de estas situaciones es la ideal.

Un enfoque mejor es *escalar* un clúster, es decir, agregar y quitar capacidad con los cambios en la demanda. Existen dos flujos de trabajo de escalado: escalado vertical y escalado horizontal. En este artículo se explica el flujo de trabajo de escalabilidad horizontal.

En este artículo se muestra cómo administrar la escalabilidad horizontal de clústeres, también conocida como escalabilidad automática. La escalabilidad automática permite escalar horizontalmente el total de instancias automáticamente en función de las programaciones y reglas predefinidas. Especifique la configuración de escalabilidad automática para el clúster en Azure Portal, tal como se describe en este artículo.

## <a name="steps-to-configure-autoscale"></a>Pasos para configurar la escalabilidad automática

En Azure Portal, vaya al recurso de clúster de Data Explorer. En el encabezado **Configuración**, seleccione **Escalar horizontalmente**. En la pestaña **Configurar**, seleccione **Habilitar escalado automático**.

   ![Habilitar escalado automático](media/manage-cluster-scaling/enable-autoscale.png)

En el gráfico siguiente se muestra el flujo de los pasos siguientes. Más detalles después del gráfico.

1. En la casilla **Nombre de ajuste de escalado automático**, escriba un nombre, como *Escalado horizontal: utilización de caché*. 

   ![Regla de escalado](media/manage-cluster-scaling/scale-rule.png)

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

Ha configurado una operación de escalado horizontal para el clúster del Explorador de datos de Azure. Agregue otra regla para una operación de reducción horizontal. Esta configuración permite que el clúster se escale dinámicamente en función de las métricas que especifique.

También puede [administrar el escalado vertical del clúster](manage-cluster-scale-up.md) para ajustar correctamente el tamaño de un clúster.

Si necesita ayuda por problemas relacionados con el escalado de un clúster, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="next-steps"></a>Pasos siguientes

[Supervisión del rendimiento, el mantenimiento y el uso de Azure Data Explorer con métricas](using-metrics.md)
