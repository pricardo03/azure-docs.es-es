---
title: Tiempos de la ingesta de datos en Azure Log Analytics | Microsoft Docs
description: Explica los distintos factores que afectan a la latencia en la recopilación de datos en Azure Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: bwren
ms.openlocfilehash: f40c8ed7eb6bfae958b3b57c4b7d525963ab9741
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955269"
---
# <a name="data-ingestion-time-in-log-analytics"></a>Tiempos de la ingesta de datos en Log Analytics
Azure Log Analytics es un servicio de datos a gran escala de Azure Monitor que atiende a miles de clientes que envían terabytes de datos cada mes a un ritmo creciente. Con frecuencia se plantean preguntas sobre el tiempo necesario para que los datos estén disponibles en Log Analytics una vez que se han recopilado. En este artículo se explican los distintos factores que afectan a esta latencia.

## <a name="typical-latency"></a>Latencia típica
La latencia hace referencia al momento en el que los datos se crean en el sistema supervisado y el momento en el que están disponibles para su análisis en Log Analytics. La latencia típica para la ingesta de datos en Log Analytics es de entre 2 y 5 minutos. La latencia específica para cualquier dato determinado varía en función de una serie de factores que se explican más adelante.


## <a name="factors-affecting-latency"></a>Factores que afectan a la latencia
El tiempo total de ingesta para un determinado conjunto de datos puede dividirse en las siguientes áreas de alto nivel. 

- Tiempo del agente: el tiempo para detectar un evento, recopilarlo y enviarlo al punto de ingesta de Log Analytics como una entrada de registro. En la mayoría de los casos, este proceso se controla mediante un agente.
- Tiempo de canalización: el tiempo que le lleva a la canalización de ingesta procesar la entrada del registro. Esto incluye el análisis de las propiedades del evento y la incorporación potencial de información calculada.
- Tiempo de indexación: el tiempo dedicado a la ingesta de una entrada de registro en el almacén de macrodatos de Log Analytics.

A continuación se describen los detalles sobre las diferentes latencias que se presentan en este proceso.

### <a name="agent-collection-latency"></a>Latencia de la recopilación de agente
Los agentes y las soluciones de administración utilizan diferentes estrategias para recopilar datos de una máquina virtual, lo que puede afectar a la latencia. A continuación se incluyen algunos ejemplos:

- Los eventos de Windows, eventos de syslog y las métricas de rendimiento se recopilan inmediatamente. Los contadores de rendimiento de Linux se sondean a intervalos de 30 segundos.
- Los registros de IIS y los registros personalizados se recopilan cuando cambia su marca de tiempo. Para los registros de IIS, esto viene determinado por la [programación de sustitución configurada en IIS](log-analytics-data-sources-iis-logs.md). 
- La solución de Active Directory Replication realiza su valoración cada cinco días, mientras que la solución de Active Directory Assessment realiza una evaluación semanal de la infraestructura de Active Directory. El agente recopilará estos registros solo cuando la evaluación esté completada.

### <a name="agent-upload-frequency"></a>Frecuencia de carga del agente
Para asegurarse de que el agente de Log Analytics es ligero, el agente almacena en búfer los registros y los carga periódicamente en Log Analytics. La frecuencia de carga varía entre 30 segundos y 2 minutos, según el tipo de datos. La mayoría de los datos se carga en menos de 1 minuto. Las condiciones de red pueden afectar negativamente a la latencia de estos datos para acceder al punto de ingesta de Log Analytics.

### <a name="azure-logs-and-metrics"></a>Registros y métricas de Azure 
Los datos de registro de actividad tardarán aproximadamente 5 minutos en estar disponible en Log Analytics. Los datos de las métricas y los registros de diagnóstico pueden tardar de 1 a 5 minutos en estar disponibles, dependiendo del servicio de Azure. Luego llevará de 30 a 60 segundos más para los registros y 3 minutos más para las métricas para que los datos se envíen al punto de ingesta de Log Analytics.

### <a name="management-solutions-collection"></a>Recopilación de las soluciones de administración
Algunas soluciones no recopilan los datos de un agente y pueden usar un método de recopilación que introduce latencia adicional. Algunas soluciones recopilan los datos a intervalos regulares sin intentar la recopilación casi en tiempo real. A continuación se incluyen algunos ejemplos específicos:

- La solución Office 365 sondea los registros de actividad mediante la API de Actividad de administración de Office 365, que actualmente no proporciona garantías de latencia casi en tiempo real.
- Los datos de las soluciones de Windows Analytics (Update Compliance por ejemplo) se recopilan con una frecuencia diaria por la solución.

Consulte la documentación de cada solución para determinar su frecuencia de recopilación.

### <a name="pipeline-process-time"></a>Tiempo del proceso de canalización
Una vez que las entradas de registro se han ingerido en la canalización de Log Analytics, se escriben en un almacenamiento temporal para garantizar el aislamiento de inquilinos y para asegurarse de que no se pierden datos. Normalmente, este proceso agrega de 5 a 15 segundos. Algunas soluciones de administración implementan algoritmos más pesados para agregar datos y obtener perspectivas a medida que los datos van entrando. Por ejemplo, Network Performance Monitor agrega los datos de entrada en intervalos de 3 minutos, agregando así una latencia de 3 minutos. Otro proceso que agrega latencia es el proceso que controla los registros personalizados. En algunos casos, este proceso puede agregar algunos minutos de latencia a los registros recopilados de los archivos por el agente.

### <a name="new-custom-data-types-provisioning"></a>Aprovisionamiento de nuevos tipos de datos personalizados
Cuando se crea un nuevo tipo de datos personalizados desde un [registro personalizado](../log-analytics/log-analytics-data-sources-custom-logs.md) o la [API del recopilador de datos](../log-analytics/log-analytics-data-collector-api.md), el sistema crea un contenedor de almacenamiento dedicado. Se trata de una sobrecarga de un solo uso que se produce solo con la primera aparición de este tipo de datos.

### <a name="surge-protection"></a>Protección para los picos de datos
La máxima prioridad de Log Analytics es asegurarse de que no se pierda ningún dato de cliente, por ello el sistema tiene protección integrada para los picos en los que los datos aumentan. Esto incluye los búferes para asegurarse de que incluso con una carga enorme, el sistema seguirá funcionando. Bajo una carga normal, estos controles agregan menos de un minuto, pero en condiciones extremas y con errores pueden agregar un tiempo significativo mientras se aseguran de que los datos no se pierdan.

### <a name="indexing-time"></a>Tiempo de indexación
Hay un equilibrio integrado para cada plataforma de macrodatos entre, por un lado, proporcionar análisis y funcionalidades de búsqueda avanzada y por otro proporcionar acceso inmediato a los datos. Azure Log Analytics le permite ejecutar consultas muy eficaces en miles de millones de registros y obtener resultados en cuestión de segundos. Esto es posible porque la infraestructura transforma los datos de forma drástica durante su ingesta y los almacena en estructuras compactas únicas. El sistema almacena en búfer los datos hasta que haya disponibles los suficientes para crear estas estructuras. Esto se tiene que completar antes de que aparezca la entrada de registro en los resultados de búsqueda.

Actualmente, este proceso tarda aproximadamente 5 minutos cuando hay bajo volumen de datos, y menos tiempo con una frecuencia de datos más alta. Esto parece contradictorio, pero este proceso permite la optimización de latencia para cargas de trabajo de producción de gran volumen.



## <a name="checking-ingestion-time"></a>Comprobación del tiempo de ingesta de datos
Puede usar la tabla de **latido** para obtener una estimación de la latencia para los datos de los agentes. Dado que el latido se envía una vez por minuto, la diferencia entre la hora actual y el último registro de latido sería idealmente lo más cerca de un minuto que sea posible.

Use la siguiente consulta para mostrar los equipos con la mayor latencia.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | top 50 by IngestionTime asc

 
Use la siguiente consulta en entornos grandes para resumir la latencia por porcentajes diferentes del total de equipos.

    Heartbeat 
    | summarize IngestionTime = now() - max(TimeGenerated) by Computer 
    | summarize percentiles(IngestionTime, 50,95,99)



## <a name="next-steps"></a>Pasos siguientes
* Lea el [Acuerdo de Nivel de Servicio (SLA)](https://azure.microsoft.com/support/legal/sla/log-analytics/v1_1/) para Log Analytics.

