---
title: Solución de problemas con entradas de Azure Stream Analytics
description: En este artículo se describen técnicas para solucionar problemas de las conexiones de entrada en los trabajos de Azure Stream Analytics.
author: sidram
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/07/2018
ms.custom: seodec18
ms.openlocfilehash: dac3037f82c38980c9ac16685aa7fddac68a2e7b
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76720306"
---
# <a name="troubleshoot-input-connections"></a>Solución de problemas de conexiones de entrada

En esta página se describen problemas comunes con las conexiones de entrada y cómo solucionarlos.

## <a name="input-events-not-received-by-job"></a>El trabajo no recibe los eventos de entrada 
1.  Pruebe la conectividad. Compruebe la conectividad a entradas y salidas con el botón **Probar conexión** para cada entrada y salida.

2.  Examine los datos de entrada.

    1. Para comprobar si los datos de entrada pasan al Centro de eventos, use [Service Bus Explorer](https://code.msdn.microsoft.com/windowsapps/Service-Bus-Explorer-f2abca5a) (Explorador de Service Bus) para conectarse al centro de eventos de Azure (si se usa la entrada de centro de eventos).
        
    1. Use el botón [**Datos de ejemplo**](stream-analytics-sample-data-input.md) para cada entrada. Descargue los datos de ejemplo de entrada.
        
    1. Inspeccione los datos de ejemplo para entender la forma de los datos; esto es, el esquema y los [tipos de datos](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics).

3.  Asegúrese de que ha seleccionado un intervalo de tiempo en la versión preliminar de la entrada. Elija **Seleccionar intervalo de tiempo** y, a continuación, escriba una duración de ejemplo antes de probar la consulta.

## <a name="malformed-input-events-causes-deserialization-errors"></a>Los eventos de entrada con formato incorrecto provocan errores de deserialización 
Los problemas de deserialización surgen cuando el flujo de entrada del trabajo de Stream Analytics contiene mensajes con un formato incorrecto. Por ejemplo, que un mensaje tenga un formato incorrecto podría deberse a la falta de un paréntesis o una llave en un objeto JSON, o a un formato de marca de tiempo incorrecto en un campo de tiempo. 
 
Cuando un trabajo de Stream Analytics recibe un mensaje con formato incorrecto desde una entrada, descarta el mensaje y le notifica con una advertencia. Se muestra un símbolo de advertencia en el icono **Entradas** del trabajo de Stream Analytics. Esta advertencia persiste mientras el trabajo está en estado de ejecución:

![Icono de entradas en Azure Stream Analytics](media/stream-analytics-malformed-events/stream-analytics-inputs-tile.png)

Habilite los registros de diagnóstico para ver los detalles de la advertencia. Para los eventos de entrada con formato incorrecto, los registros de ejecución contienen una entrada con un mensaje similar al siguiente: 
```
Could not deserialize the input event(s) from resource <blob URI> as json.
```

### <a name="what-caused-the-deserialization-error"></a>El motivo del error de deserialización
Puede realizar los pasos siguientes para analizar los eventos de entrada en detalle y así obtener una idea clara de qué produjo el error de deserialización. A continuación, puede corregir el origen del evento para generar eventos en el formato correcto y así evitar que el problema surja de nuevo.

1. Navegue hasta el icono de entrada y haga clic en los símbolos de advertencia para ver la lista de problemas.

2. El icono de detalles de entrada muestra una lista de advertencias con detalles sobre cada problema. El mensaje de advertencia de ejemplo que se muestra a continuación incluye la partición, el desplazamiento y los números de secuencia donde hay datos JSON con un formato incorrecto. 

   ![Mensaje de advertencia de Stream Analytics con desplazamiento](media/stream-analytics-malformed-events/warning-message-with-offset.png)
   
3. Para encontrar los datos JSON con formato incorrecto, ejecute el código CheckMalformedEvents.cs, disponible en el [repositorio de ejemplos de GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/CheckMalformedEventsEH). Este código lee el identificador de la partición y el desplazamiento e imprime los datos de este último. 

4. Una vez leídos los datos, puede analizar y corregir el formato de serialización.

5. También puede consultar [How to read events from an IoT Hub with the Service Bus Explorer](https://code.msdn.microsoft.com/How-to-read-events-from-an-1641eb1b) (Cómo leer eventos de una instancia de IoT Hub con Service Bus Explorer).

## <a name="job-exceeds-maximum-event-hub-receivers"></a>El trabajo excede el máximo de receptores de centro de eventos
Una práctica recomendada para el uso de Event Hubs es usar varios grupos de consumidores para garantizar la escalabilidad del trabajo. El número de lectores en el trabajo de Stream Analytics para una entrada específica afecta a la cantidad de lectores en un grupo de consumidores único. El número exacto de destinatarios se basa en los detalles de implementación interna para la lógica de la topología de escalado horizontal y no está expuesto de forma externa. El número de lectores puede cambiar cuando inicia un trabajo o durante las actualizaciones del mismo.

El error que se muestra cuando el número de receptores supera el máximo: `The streaming job failed: Stream Analytics job has validation errors: Job will exceed the maximum amount of Event Hub Receivers.`

> [!NOTE]
> Cuando el número de lectores cambia durante una actualización de trabajo, se escriben advertencias transitorias en registros de auditoría. Los trabajos de Stream Analytics se recuperan automáticamente de estos problemas transitorios.

### <a name="add-a-consumer-group-in-event-hubs"></a>Agregar un grupo de consumidores a Event Hubs
Para agregar un nuevo grupo de consumidores a la instancia de Event Hubs, siga estos pasos:

1. Inicie sesión en Azure Portal.

2. Busque la instancia de Event Hubs.

3. Seleccione **Event Hubs** en el encabezado **Entidades**.

4. Seleccione un centro de eventos según el nombre.

5. En la página **Instancia de Event Hubs**, en el encabezado **Entidades**, seleccione **Grupos de consumidores**. Aparecerá un grupo de consumidores con nombre **$Default**.

6. Seleccione **+ Grupo de consumidores** para agregar un nuevo grupo de consumidores. 

   ![Agregar un grupo de consumidores a Event Hubs](media/stream-analytics-event-hub-consumer-groups/new-eh-consumer-group.png)

7. Cuando se crea la entrada en el trabajo de Stream Analytics para que apunte al centro de eventos, es necesario especificar el grupo de consumidores ahí. $Default se utiliza cuando no se especifica ninguno. Una vez que cree un nuevo grupo de consumidores, edite la entrada de Event Hubs en el trabajo de Stream Analytics y especifique el nombre del nuevo grupo de consumidores.


## <a name="readers-per-partition-exceeds-event-hubs-limit"></a>El número de lectores por partición supera el límite de Event Hubs

Si la sintaxis de la consulta de streaming hace referencia varias veces al mismo recurso de entrada de Event Hubs, el motor de trabajo puede utilizar varios lectores por cada consulta de ese mismo grupo de consumidores. Cuando hay demasiadas referencias al mismo grupo de consumidores, el trabajo puede superar el límite establecido de cinco y se produce un error. En tales circunstancias, puede dividir aún más mediante el uso de múltiples entradas en varios grupos de consumidores; para ello, use la solución que se describe en la siguiente sección. 

Los escenarios en los que el número de lectores por partición supera el límite de Event Hubs de cinco incluyen los siguientes:

* Varias instrucciones SELECT: si usa varias instrucciones SELECT que hacen referencia a la **misma** entrada de centro de eventos, cada instrucción SELECT provoca que se cree un nuevo destinatario.
* UNION: cuando se utiliza UNION, es posible tener varias entradas que hacen referencia al **mismo** centro de eventos y grupo de consumidores.
* SELF JOIN: cuando se usa una operación SELF JOIN, es posible hacer referencia al **mismo** centro de eventos varias veces.

Las siguientes prácticas recomendadas pueden ayudar a reducir los escenarios en los que el número de lectores por partición supera el límite de Event Hubs de cinco.

### <a name="split-your-query-into-multiple-steps-by-using-a-with-clause"></a>División de la consulta en varios pasos mediante una cláusula WITH

La cláusula WITH especifica un conjunto de resultados con nombre temporal al que se puede hacer referencia mediante una cláusula FROM en la consulta. Defina la cláusula WITH en el ámbito de ejecución de una única instrucción SELECT.

Por ejemplo, en lugar de esta consulta:

```SQL
SELECT foo 
INTO output1
FROM inputEventHub

SELECT bar
INTO output2
FROM inputEventHub 
…
```

Utilice esta consulta:

```SQL
WITH data AS (
   SELECT * FROM inputEventHub
)

SELECT foo
INTO output1
FROM data

SELECT bar
INTO output2
FROM data
…
```

### <a name="ensure-that-inputs-bind-to-different-consumer-groups"></a>Asegúrese de que las entradas se enlazan a grupos de consumidores diferentes

Para las consultas en las que hay conectadas tres o más entradas al mismo grupo de consumidores de Event Hubs, cree grupos de consumidores independientes. Esto requiere la creación de entradas de Stream Analytics adicionales.

## <a name="get-help"></a>Obtener ayuda

Para obtener más ayuda, pruebe nuestro [foro de Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Stream Analytics](stream-analytics-introduction.md)
* [Introducción al uso de Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Escalación de trabajos de Azure Stream Analytics](stream-analytics-scale-jobs.md)
* [Referencia del lenguaje de consulta de Azure Stream Analytics](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Referencia de API de REST de administración de Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)
