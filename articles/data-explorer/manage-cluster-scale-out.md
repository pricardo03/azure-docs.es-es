---
title: Escalar horizontalmente un clúster del explorador de datos de Azure
description: En este artículo se describe los pasos para el escalado horizontal y vertical en un clúster del explorador de datos de Azure basado en la variable a petición.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/05/2019
ms.openlocfilehash: 24bbddd28943adc929fbaea456eeae8165db290c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60758677"
---
# <a name="manage-cluster-scale-out-to-accommodate-changing-demand"></a>Administración del escalado horizontal de clúster para ajustarse a los cambios en la demanda

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión absoluta. Un tamaño de clúster estático puede provocar infrautilización o sobreutilización, ninguno de los cuales es ideal.

Un enfoque mejor es *escalar* un clúster, es decir, agregar y quitar capacidad con los cambios en la demanda. Hay dos flujos de trabajo para el escalado: escalado vertical y horizontal. En este artículo se explica el flujo de trabajo de escalabilidad horizontal.

En este artículo se muestra cómo administrar el clúster escalado horizontal, también conocida como escalado automático. El escalado automático permite escalar horizontalmente el recuento de instancias automáticamente según las programaciones y reglas predefinidas. Especifique la configuración de escalado automático para el clúster en Azure portal, tal como se describe en este artículo.

## <a name="steps-to-configure-autoscale"></a>Pasos para configurar el escalado automático

En el portal de Azure, vaya a su recurso de clúster del explorador de datos. En el **configuración** encabezado, seleccione **escalar horizontalmente**. En el **configurar** ficha, seleccione **habilitar escalado automático**.

   ![Habilitar escalado automático](media/manage-cluster-scaling/enable-autoscale.png)

En el gráfico siguiente se muestra el flujo de los pasos siguientes. Más detalles a continuación en el gráfico.

1. En el **nombre de la configuración de escalado automático** cuadro, proporcione un nombre, como *escalada: uso de caché*. 

   ![Regla de escalado](media/manage-cluster-scaling/scale-rule.png)

2. Para **modo escala**, seleccione **escalado según una métrica**. Este modo proporciona escalado dinámico. También puede seleccionar **escala a un número específico de instancias**.

3. Seleccione **+ agregar una regla**.

4. En la sección**Escalar regla** de la derecha, proporcione valores para cada ajuste.

    **Criterios**

    | Configuración | Descripción y valor |
    | --- | --- |
    | **Agregación de tiempo** | Seleccione un criterio de agregación, como **Promedio**. |
    | **Nombre de la métrica** | Seleccione la métrica en la que quiera que se base la operación de escalado, como **Cache Utilization**. |
    | **Estadísticas de intervalo de agregación** | Elija entre **Promedio**, **Mínimo**, **Máximo** y **Suma**. |
    | **Operador** | Elija la opción adecuada, como **Mayor o igual que**. |
    | **Umbral** | Elija un valor apropiado. Por ejemplo, para la utilización de memoria caché, el 80 por ciento es un buen punto de partida. |
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

Ha configurado una operación de escalado horizontal para el clúster del Explorador de datos de Azure. Agregue otra regla para una operación de reducción horizontal. Esta configuración permite que el clúster se escale dinámicamente según las métricas que especifique.

También puede [administrar clúster escalado](manage-cluster-scale-up.md) para ajustar el tamaño adecuado de un clúster.

Si necesita ayuda con problemas de escalado de clústeres, [abrir una solicitud de soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) en Azure portal.

## <a name="next-steps"></a>Pasos siguientes

[Supervisar el rendimiento, mantenimiento y uso con métricas de explorador de datos de Azure](using-metrics.md)
