---
title: Automatización de procesos de registro de Azure Monitor con Microsoft Flow
description: Obtenga información acerca de cómo puede utilizar Microsoft Flow para automatizar rápidamente los procesos repetibles mediante el conector de Azure Log Analytics.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/29/2017
ms.openlocfilehash: 92f0d2916b0f28760f7d028ee3e6dc0be37c32d2
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77672317"
---
# <a name="automate-azure-monitor-log-processes-with-the-connector-for-microsoft-flow"></a>Automatización de procesos de registro de Azure Monitor con el conector para Microsoft Flow
[Microsoft Flow](https://ms.flow.microsoft.com) le permite crear flujos de trabajo automatizados, con cientos de acciones para diversos servicios. La salida de una acción se puede utilizar como entrada de otra lo que le permite crear una integración entre los diferentes servicios.  El conector de Azure Log Analytics para Microsoft Flow le permite compilar flujos de trabajo que incluyen datos recuperados de las búsquedas de registros de un área de trabajo de Log Analytics en Azure Monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Por ejemplo, puede usar Microsoft Flow para usar los datos de registro de Azure Monitor incluidos en una notificación de correo electrónico de Office 365, crear un error en Azure DevOps o publicar un mensaje de Slack.  Puede desencadenar un flujo de trabajo mediante una programación simple o a partir de alguna acción de un servicio conectado, como cuando se recibe un correo electrónico o un tweet.  

El tutorial de este artículo le muestra cómo crear un flujo que envíe automáticamente los resultados de una consulta de registros de Log Azure Monitor por correo electrónico, lo cual es solo un ejemplo de cómo puede usar el conector de Log Analytics en Microsoft Flow. 


## <a name="step-1-create-a-flow"></a>Paso 1: Creación de un flujo
1. Inicie sesión en [Microsoft Flow](https://flow.microsoft.com) y seleccione **Mis flujos**.
2. Haga clic en **+ Crear desde cero**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>Paso 2: Creación de un desencadenador para el flujo
1. Haga clic en **Search hundreds of connectors and triggers** (Buscar cientos de conectores y desencadenadores).
2. Escriba **programación** en el cuadro de búsqueda.
3. Elija **Programación** y, a continuación, **Programación: Periodicidad**.
4. En el cuadro **Frecuencia**, seleccione **Día** y, en el cuadro **Intervalo**, escriba **1**.<br><br>![Cuadro de diálogo del desencadenador de Microsoft Flow](media/flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>Paso 3: Adición de una acción de Log Analytics
1. Haga clic en **+ Nuevo paso** y, a continuación, en **Agregar una acción**.
2. Busque **Log Analytics**.
3. Haga clic en **Azure Log Analytics: ejecutar consulta y ver resultados**.<br><br>![Ventana Ejecutar consulta de Log Analytics](media/flow-tutorial/flow02.png)

## <a name="step-4-configure-the-log-analytics-action"></a>Paso 4: Configuración de la acción de Log Analytics

1. Especifique los detalles del área de trabajo incluido el identificador de suscripción, el grupo de recursos y el nombre del área de trabajo.
2. En la ventana **Consulta**, agregue la siguiente consulta de registro.  Esta es solo una consulta de ejemplo y puede reemplazarla por cualquier otra que devuelva datos.
   ```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computer
   ```

2. En el cuadro **Tipo de gráfico**, seleccione **Tabla HTML**.<br><br>![Acción de Log Analytics](media/flow-tutorial/flow03.png)

## <a name="step-5-configure-the-flow-to-send-email"></a>Paso 5: Configuración del flujo para enviar correo electrónico

1. Haga clic en **Nuevo paso** y, a continuación, en **+ Agregar una acción**.
2. Busque **Office 365 Outlook**.
3. Haga clic en **Office 365 Outlook: Enviar un correo electrónico**.<br><br>![Ventana de selección de Office 365 Outlook](media/flow-tutorial/flow04.png)

4. Especifique la dirección de correo electrónico de un destinatario en la ventana **Para** y un asunto para el correo electrónico en **Asunto**.
5. Haga clic en cualquier lugar del cuadro **Cuerpo**.  Se abre una ventana de **contenido dinámico** con los valores de las acciones anteriores.  
6. Seleccione **Cuerpo**.  Estos son los resultados de la consulta de la acción de Log Analytics.
6. Haga clic en **Mostrar opciones avanzadas**.
7. En el campo **Es HTML**, seleccione **Sí**.<br><br>![Ventana de configuración de correo electrónico de Office 365](media/flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>Paso 6: Guardado y prueba del flujo
1. En el cuadro **Nombre de flujo**, agregue un nombre para el flujo y, a continuación, haga clic en **Crear flujo**.<br><br>![Guardar flujo](media/flow-tutorial/flow06.png)
2. El flujo ya se ha creado y se ejecutará una vez transcurrido un día, que es la programación que especificó. 
3. Para probar el flujo de inmediato, haga clic en **Ejecutar ahora** y, a continuación, en **Ejecutar flujo**.<br><br>![Ejecutar flujo](media/flow-tutorial/flow07.png)
3. Cuando se complete el flujo, compruebe el correo electrónico del destinatario que especificó.  Debe haber recibido un correo electrónico con un cuerpo similar al siguiente:<br><br>![Correo electrónico de ejemplo](media/flow-tutorial/flow08.png)


## <a name="next-steps"></a>Pasos siguientes

- Más información sobre las [consultas de registro en Azure Monitor](../log-query/log-query-overview.md).
- Más información sobre [Microsoft Flow](https://ms.flow.microsoft.com).


