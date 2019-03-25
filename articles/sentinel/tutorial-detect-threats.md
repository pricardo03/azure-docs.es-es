---
title: Investigar alertas con la versión preliminar de Azure Sentinel | Microsoft Docs
description: Use este tutorial para aprender a investigar alertas con Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: b5fbc5ac-68b2-4024-9c1b-bd3cc41a66d0
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: rkarlin
ms.openlocfilehash: a0a16fbda662ee1d3718dbafc4231de67aab277e
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400667"
---
# <a name="tutorial-detect-threats-with-azure-sentinel-preview"></a>Tutorial: Detecte amenazas con vista previa de Centinela de Azure

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Después de [los orígenes de datos conectados](quickstart-onboard.md) a Sentinel, Azure, que desea recibir una notificación cuando ocurre algo sospechoso. Para poder hacer esto, Sentinel Azure permite que crear reglas de alerta, que generan los casos que se pueden asignar y avanzadas uso profundamente investigar las anomalías y las amenazas en su entorno. 

Este tutorial le ayudará a detectar amenazas con Azure Sentinel.
> [!div class="checklist"]
> * Crear reglas de detección
> * Respuesta a amenazas

## <a name="create-detection-rules"></a>Crear reglas de detección

Para investigar los casos, primero debe crear reglas de detección. 

> [!NOTE]
> Están disponibles a través de las alertas generadas en Azure Sentinel [Microsoft Graph Security](https://aka.ms/securitygraphdocs). Hacer referencia a la [documentación de alertas de seguridad de Microsoft Graph](https://aka.ms/graphsecurityreferencebetadocs) para obtener más detalles y asociados de integración.

Reglas de detección se basan en los tipos de amenazas y anomalías que podrían ser sospechosas en su entorno que desea saber acerca de inmediato, asegurarse de aparece, investigar y corregir. 

1. En el portal de Azure en Azure Sentinel, seleccione **Analytics**.

   ![Análisis](./media/tutorial-detect-threats/alert-rules.png)

2. En la barra de menús superior, haga clic en **+ agregar**.  

   ![Crear regla de alertas](./media/tutorial-detect-threats/create-alert-rule.png)

3. En **crear regla de alertas**, proporcione un nombre descriptivo y establecer el **gravedad** según sea necesario. 

4. Crear la consulta de Log Analytics y, a continuación, péguelo en el **regla de alerta conjunto** campo. Esta es una consulta de ejemplo que podría generar alertas cuando se crea un número anómalo de recursos en la actividad de Azure.

        AzureActivity
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"
        | where ActivityStatus == "Succeeded"
        | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller

5. En el **asignación de entidad** sección, utilice los campos de **tipo de entidad** para asignar las columnas de la consulta a los campos de entidad reconocidos por Azure Sentinel. Para cada campo, asigne la columna correspondiente en la consulta que creó en Log Analytics, en el campo de entidad correspondiente. Seleccione el nombre de columna correspondiente en el **propiedad**. Cada entidad incluye varios campos, por ejemplo, SID, GUID, etcetera. Puede asignar la entidad de acuerdo con los de los campos, no solo la entidad de nivel superior.

6. Definir condiciones de desencadenador de la alerta en **desencadenador alerta**. Esto define las condiciones que desencadenan la alerta. 

7. Establecer el **frecuencia** con qué frecuencia se ejecuta la consulta - como con frecuencia cada 5 minutos o tan pocas veces como una vez al día. 

8. Establecer el **período** para controlar el período de tiempo para la cantidad de datos se ejecuta la consulta-por ejemplo, se puede ejecutar cada hora en 60 minutos de datos.

9. También puede establecer el **supresión**. Supresión es útil cuando desea detener las alertas duplicadas se desencadene para el mismo incidente. De esta manera, puede detener las alertas se desencadene durante un período específico. Esto puede ayudar a evitar alertas duplicadas para el mismo incidente y permiten Suprimir alertas consecutivas durante un período de tiempo. Por ejemplo, si la **la programación de alerta** **frecuencia** se establece en 60 minutos y el **período de programación de alerta** se establece en dos horas, y los resultados de consulta supera el definido umbral, desencadenará una alerta dos veces, una vez cuando se detecta en primer lugar en los últimos 60 minutos, y otra vez cuando se encuentra en los primeros 60 minutos de 2 horas de datos que se va a muestrear. Se recomienda que, si se desencadena una alerta, la supresión debe ser para la cantidad de tiempo establecido en el periodo de alerta. En nuestro ejemplo, es posible que desea establecer supresión durante 60 minutos, por lo que solo se desencadenan alertas para eventos que se produjeron durante la hora más reciente.

8. Después de pegar la consulta en el **regla de alerta conjunto** campo, puede ver inmediatamente una simulación de la alerta en **simulación alert Logic** para que pueda obtener la descripción de la cantidad de datos será se genera en un intervalo de tiempo específico para la alerta que creó. Esto dependerá de lo que ha establecido para **frecuencia** y **umbral**. Si ve que en promedio, la alerta se desencadena con demasiada frecuencia, desea establecer el número de resultados mayor para que esté por encima de la línea de base promedio.

9. Haga clic en **crear** para inicializar la regla de alerta. Una vez creada la alerta, se crea un caso de que contiene la alerta. Puede ver las reglas de detección definidos como filas en el **análisis de seguridad** ficha. También puede ver el número de coincidencias para cada regla: las alertas activadas. En esta lista que puede habilitar, deshabilitar o eliminar cada regla. Puede también derecho de seleccionar el botón de puntos suspensivos (...) al final de la fila para cada alerta editar, deshabilitar, clonar, mostrar a coincidencias o eliminar una regla. El **Analytics** página es una galería de todas las reglas de alertas activas, incluidas plantillas habilita y se crea en función de las plantillas de reglas de alerta.

1. Se pueden ver los resultados de las reglas de alerta en el **casos** página, donde puede clasificarlo, [investigar casos](tutorial-investigate-cases.md), y solucionar tales amenazas.



## <a name="respond-to-threats"></a>Respuesta a amenazas

Sentinel Azure ofrece dos opciones principales para responder a amenazas con cuadernos de estrategias. Puede establecer un cuaderno de estrategias para ejecutar automáticamente cuando se desencadena una alerta, o puede ejecutar manualmente un guión de procedimientos como respuesta a una alerta.

- Establezca un cuaderno de estrategias para ejecutar automáticamente cuando se desencadena una alerta cuando se configura el cuaderno de estrategias. 

- Ejecutar manualmente un cuaderno de estrategias desde dentro de la alerta, haciendo clic en **ver playbooks** y, a continuación, seleccionando un cuaderno de estrategias de ejecución.




## <a name="next-steps"></a>Pasos siguientes
En este tutorial, ha aprendido cómo comenzar a detectar amenazas mediante Azure Sentinel. 

Para obtener información sobre cómo automatizar sus respuestas a las amenazas, [cómo responder a amenazas mediante guiones de procedimientos automatizados](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Responder a amenazas](tutorial-respond-threats-playbook.md) para automatizar sus respuestas a las amenazas.

