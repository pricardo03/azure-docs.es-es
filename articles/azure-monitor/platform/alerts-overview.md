---
title: Información general de alertas y supervisión de notificaciones en Azure
description: Información general de las alertas de Azure Alertas, alertas clásicas e interfaz de alertas.
author: rboucher
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 01/28/2018
ms.author: robb
ms.subservice: alerts
ms.openlocfilehash: 59b190065c678ef61b3e0dec8df13db4215a8229
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55486307"
---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Información general sobre las alertas en Microsoft Azure 

En este artículo se explica qué son las alertas, sus beneficios y cómo empezar a utilizarlas.  


## <a name="what-are-alerts-in-microsoft-azure"></a>¿Qué son las alertas en Microsoft Azure?
Las alertas le informan de forma proactiva cuando se detectan condiciones importantes en los datos que supervisa. Le permiten identificar y solucionar los problemas antes de que los usuarios del sistema puedan verlos. 

En este artículo se describe la experiencia unificada de alertas en Azure Monitor, que ahora incluye Log Analytics y Application Insights. La [experiencia de alerta anterior](alerts-classic.overview.md) y los tipos de alertas se denominan **alertas clásicas**. Para ver esta experiencia anterior y el tipo de alerta anterior, haga clic en **Ver alertas clásicas** en la parte superior de la página de alertas. 

## <a name="overview"></a>Información general

En el siguiente diagrama se representa el flujo de alertas. 

![Flujo de alertas](media/alerts-overview/Azure-Monitor-Alerts.svg)

Las reglas de alertas están separadas de las alertas y las acciones que realizan cuando se activa una alerta. 

**Regla de alertas**: la regla de alertas captura el destino y los criterios de las alertas. La regla de alertas puede tener el estado deshabilitado o habilitado. Las alertas solo se activan cuando están habilitadas. 

Los atributos clave de las reglas de alertas son:

**Recurso de destino**: define el ámbito y las señales disponibles para las alertas. Un destino puede ser cualquier recurso de Azure. Destinos de ejemplo: una máquina virtual, una cuenta de almacenamiento, un conjunto de escalado de máquinas virtuales, un área de trabajo de Log Analytics o un recurso de Application Insights. Para determinados recursos (por ejemplo, Virtual Machines), puede especificar varios recursos como destino de la regla de alertas.

**Señal**: el recurso de destino emite las señales y pueden ser de varios tipos. Métrica, registro de actividad, Application Insights y registro.

**Criterios**: los criterios son la combinación de la señal y la lógica aplicadas en un recurso de destino. Ejemplos: 
   - Porcentaje de la CPU superior al 70 %
   - Tiempo de respuesta del servidor superior a 4 ms 
   - Recuento de resultados de una consulta de registro superior a 100

**Nombre de la alerta**: nombre específico para la regla de alertas configurado por el usuario

**Descripción de la alerta**: descripción para la regla de alertas configurada por el usuario

**Gravedad**: gravedad de la alerta, una vez que se cumplen los criterios especificados en la regla de alertas. La gravedad puede tener un valor entre 0 y 4.

**Acción**: una acción específica llevada a cabo al desencadenarse la alerta. Para más información, vea [Grupos de acciones](../../azure-monitor/platform/action-groups.md).

## <a name="what-you-can-alert-on"></a>Tipo de alertas que se pueden realizar

Puede enviar alertas sobre métricas y registros tal y como se describe en el artículo sobre los [orígenes de datos de supervisión](../../azure-monitor/platform/data-sources-reference.md). Estas incluyen, pero no se limitan a:
- Valores de métrica
- Consultas de búsqueda de registros
- Eventos del registro de actividad
- Estado de la plataforma Azure subyacente
- Pruebas de disponibilidad del sitio web

Anteriormente, las métricas de Azure Monitor, Application Insights, Log Analytics y Service Health tenían funcionalidades independientes de generación de alertas. Con el tiempo, Azure ha mejorado y combinado la interfaz de usuario y los distintos métodos de generación de alertas. Esta consolidación aún está en proceso. Como resultado, algunas funcionalidades de alertas aún no se encuentran en el nuevo sistema de alertas.  

| **Origen de supervisión** | **Tipo de señal**  | **Descripción** | 
|-------------|----------------|-------------|
| Estado del servicio | Registro de actividades  | No compatible. Consulte [Creación de alertas del registro de actividad en notificaciones del servicio](../../azure-monitor/platform/alerts-activity-log-service-notifications.md).  |
| Application Insights | Pruebas de disponibilidad web | No compatible. Consulte [Alertas de pruebas web](../../azure-monitor/app/monitor-web-app-availability.md). Disponible para cualquier sitio web instrumentado para enviar datos a Application Insights. Reciba una notificación cuando la disponibilidad o la capacidad de respuesta de un sitio web está por debajo de las expectativas. |

## <a name="manage-alerts"></a>Administrar alertas
Puede establecer el estado de una alerta para especificar dónde se encuentra en el proceso de resolución. Cuando se cumplen los criterios especificados en la regla de alertas, se crea o se desencadena una alerta, que tiene el estado *Nuevo*. Puede cambiar el estado cuando se confirma una alerta y cuando se cierra. Todos los cambios de estado se almacenan en el historial de la alerta.

Se admiten los siguientes estados de alerta.

| Estado | DESCRIPCIÓN |
|:---|:---|
| Nuevo | Se acaba de detectar el problema y todavía no se ha revisado. |
| Confirmado | Un administrador revisó la alerta y empezó a trabajar en ella. |
| Closed | Se resolvió el problema. Después de cerrar una alerta, puede volver a abrirla mediante el cambio a otro estado. |

El **estado de alerta** es diferente e independiente de la **condición de supervisión**. El estado de alerta lo establece el usuario. La condición de supervisión la establece el sistema. Cuando se desencadena una alerta, la condición de supervisión de la alerta se establece en *desencadenada*. Cuando desaparece la condición subyacente que provocó que se desencadenara la alerta, la condición de supervisión se establece en *resuelta*. El estado de alerta no cambia hasta que el usuario lo cambia. Obtenga información sobre [cómo cambiar el estado de las alertas y de los grupos inteligentes](https://aka.ms/managing-alert-smart-group-states).

## <a name="smart-groups"></a>Grupos inteligentes 
Los grupos inteligentes están en versión preliminar. 

Los grupos inteligentes son agregaciones de alertas basadas en algoritmos de aprendizaje automático que pueden ayudar a reducir el ruido de las alertas y contribuyen a la solución de problemas. [Más información acerca de los grupos inteligentes](https://aka.ms/smart-groups) y [cómo administrar los grupos inteligentes](https://aka.ms/managing-smart-groups).


## <a name="alerts-experience"></a>Experiencia de alertas 
La página Alertas predeterminada brinda un resumen de las alertas que se crean dentro un período de tiempo determinado. Muestra el total de alertas para cada gravedad con columnas que identifican el número total de alertas en cada estado para cada gravedad. Seleccione cualquiera de las gravedades para abrir la página [Todas las alertas](#all-alerts-page) filtrada según esa gravedad.

No muestra las [alertas clásicas](#classic-alerts) antiguas ni realiza ningún seguimiento de ellas. Puede cambiar las suscripciones o los parámetros de filtro para actualizar la página. 

![Página de alertas](media/alerts-overview/alerts-page.png)

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


## <a name="manage-alert-rules"></a>Administración de reglas de alerta
Haga clic en **Administrar reglas de alertas** para mostrar la página **Reglas**. **Reglas** es una sola página que permite administrar todas las reglas de alertas de las suscripciones a Azure. Se enumeran todas las reglas de alertas que se pueden ordenar según los recursos de destino, los grupos de recursos, el nombre de regla o el estado. En esta página también se pueden editar, habilitar o deshabilitar las reglas de alertas.  

 ![alertas-reglas](./media/alerts-overview/alerts-preview-rules.png)


## <a name="create-an-alert-rule"></a>Crear una regla de alerta
Las alertas se pueden crear de manera coherente independientemente del tipo de señal o del servicio de supervisión. Todas las alertas activadas y los detalles relacionados están disponibles en una sola página.
 
Siga estos pasos para crear una regla de alertas nueva:
1. Elija el _destino_ de la alerta.
1. Seleccione la _señal_ de las señales disponibles para el destino.
1. Especifique la _lógica_ que se aplicará a los datos desde la señal.
 
Este proceso simplificado de creación ya no requiere que el usuario sepa las señales o el origen de supervisión admitidos antes de seleccionar un recurso de Azure. La lista de señales disponibles se filtra automáticamente según el recurso de destino que seleccione. También en función de ese destino, se le guía por la definición de la lógica de la regla de alerta automáticamente.  

Puede obtener más información sobre cómo crear reglas de alertas en [Creación, visualización y administración de alertas mediante Azure Monitor](../../azure-monitor/platform/alerts-metric.md).

Las alertas están disponibles a través de varios servicios de supervisión de Azure. Para obtener más información sobre cómo y cuándo usar cada uno de estos servicios, consulte [Supervisión de aplicaciones y recursos de Azure](../../azure-monitor/overview.md). 


## <a name="all-alerts-page"></a>Página Todas las alertas 
Haga clic en Total de alertas para ver la página de todas las alertas. Aquí puede ver una lista de las alertas que se crearon en el período de tiempo seleccionado. Puede ver una lista de las alertas individuales o una lista de los grupos inteligentes que contienen las alertas. Seleccione el banner en la parte superior de la página para alternar entre las vistas.

![Página Todas las alertas](media/alerts-overview/all-alerts-page.png)

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

## <a name="alert-detail-page"></a>Página Detalles de la alerta
La página de detalles de la alerta aparece cuando se selecciona una alerta. Proporciona detalles de la alerta y permite cambiar su estado.

![Detalles de la alerta](media/alerts-overview/alert-detail2.png)

La página Detalles de la alerta incluye las secciones siguientes.

| Sección | DESCRIPCIÓN |
|:---|:---|
| Essentials | Muestra las propiedades y otra información importante sobre la alerta. |
| Historial | Muestra cada acción que realiza la alerta y los cambios hechos en la alerta. Actualmente se limita a cambios de estado. |
| Grupo inteligente | Información sobre el grupo inteligente en que se incluye la alerta. El *recuento de alertas* se refiere al número de alertas incluidas en el grupo inteligente. Incluye otras alertas en el mismo grupo inteligente que se crearon en los últimos 30 días, con independencia del filtro de tiempo de la página de lista de alertas. Seleccione una alerta para ver sus detalles. |
| Más detalles | Muestra información contextual adicional para la alerta que habitualmente es específica para el tipo de origen que creó la alerta. |


## <a name="classic-alerts"></a>Alertas clásicas 

La funcionalidad de creación de alertas del registro de actividad y de métricas de Azure Monitor disponible antes del junio de 2018 se denomina "Alertas (clásico)". 

Para más información, consulte [Alertas clásicas](./../../azure-monitor/platform/alerts-classic.overview.md).


## <a name="next-steps"></a>Pasos siguientes

- [Más información sobre los grupos inteligentes](https://aka.ms/smart-groups)
- [Información sobre los grupos de acciones](../../azure-monitor/platform/action-groups.md)
- [Administración de instancias de alertas en Azure](https://aka.ms/managing-alert-instances)
- [Administración de grupos inteligentes](https://aka.ms/managing-smart-groups)






