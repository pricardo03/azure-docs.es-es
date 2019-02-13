---
title: Escalado del clúster del Explorador de datos de Azure para ajustarse a los cambios en la demanda
description: En este artículo se describen los pasos para escalar y reducir horizontalmente un clúster del Explorador de datos de Azure en función de los cambios en la demanda.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 38dc7b70630276d51c75ca7e87f0b69ea7fe040a
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735128"
---
# <a name="manage-cluster-scale-out-to-accommodate-changing-demand"></a>Administración del escalado horizontal de clúster para ajustarse a los cambios en la demanda

Ajustar el tamaño de un clúster de forma adecuada es fundamental para el rendimiento del Explorador de datos de Azure. Pero no se puede predecir la demanda en un clúster con una precisión del 100 %. El tamaño de un clúster estático puede provocar una infrautilización o sobreutilización, y ninguna de estas situaciones es la ideal. Un enfoque mejor es *escalar* un clúster, es decir, agregar y quitar capacidad con los cambios en la demanda. En este artículo se muestra cómo administrar el escalado horizontal de clústeres.

Navegue hasta el clúster y, en **Configuración**, seleccione **Escalar horizontalmente**. En **Configurar**, seleccione **Habilitar escalado automático**.

![Habilitar escalado automático](media/manage-cluster-scaling/enable-autoscale.png)

En el gráfico siguiente se muestra el flujo de los pasos siguientes. A continuación del gráfico podrá encontrar más detalles.

![Regla de escalado](media/manage-cluster-scaling/scale-rule.png)

1. En **Nombre de ajuste de escalado automático**, escriba un nombre, como *Scale-out: cache utilization*.

1. En **Modo de escala**, seleccione **Escalado basado en una métrica**. Este modo proporciona escalado dinámico; también puede seleccionar **Escalar a un número específico de instancias**.

1. Seleccione **Agregar una regla**.

1. En la sección**Escalar regla** de la derecha, proporcione valores para cada ajuste.

    | Configuración | Descripción y valor |
    | --- | --- | --- |
    | **Agregación de tiempo** | Seleccione un criterio de agregación, como **Promedio**. |
    | **Nombre de la métrica** | Seleccione la métrica en la que quiera que se base la operación de escalado, como **Cache Utilization**. |
    | **Estadísticas de intervalo de agregación** | Elija entre **Promedio**, **Mínimo**, **Máximo** y **Suma**. |
    | **Operador** | Elija la opción adecuada, como **Mayor o igual que**. |
    | **Umbral** | Elija un valor apropiado. Por ejemplo, para el uso de memoria caché, el 80 % es un buen punto inicial. |
    | **Duration** | Elija un período apropiado para que el sistema busque al calcular las métricas. Comience con el valor predeterminado de 10 minutos. |
    | **operación** | Elija la opción adecuada para reducir o escalar horizontalmente. |
    | **Recuento de instancias** | Elija el número de nodos o instancias que quiere agregar o quitar cuando se cumpla una condición de métrica. |
    | **Tiempo de finalización (minutos)** | Elija un intervalo de tiempo adecuado para esperar entre las operaciones de escalado. Comience con el valor predeterminado de cinco minutos. |
    |  |  |

1. Seleccione **Agregar**.

1. En la sección **Límites de instancia** de la izquierda, indique valores para cada configuración.

    | Configuración | Descripción y valor |
    | --- | --- | --- |
    | *Mínimo* | Este es el número de instancias por debajo del cual el clúster no se escalará, independientemente del uso. |
    | *Máximo* | Este es el número de instancias por encima del cual el clúster no se escalará, independientemente del uso. |
    | *Valor predeterminado* | El número predeterminado de instancias, que se utiliza si hay un problema al leer las métricas de recursos. |
    |  |  |

1. Seleccione **Guardar**.

Ha configurado una operación de escalado horizontal para el clúster del Explorador de datos de Azure. Agregue otra regla para una operación de reducción horizontal. Esto permite que el clúster se escale dinámicamente en función de las métricas que especifique.

Si necesita ayuda por problemas relacionados con el escalado de un clúster, abra una solicitud de soporte técnico en [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).