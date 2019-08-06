---
title: Investigar alertas con Azure Sentinel, versión preliminar | Microsoft Docs
description: Use este tutorial para aprender a investigar alertas con Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: b5fbc5ac-68b2-4024-9c1b-bd3cc41a66d0
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2019
ms.author: rkarlin
ms.openlocfilehash: ad9c752898733286701db2d0f0b1fc40029b7521
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68370717"
---
# <a name="tutorial-detect-threats-with-azure-sentinel-preview"></a>Tutorial: Detección de amenazas con la versión preliminar de Azure Sentinel

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Después de [conectar los orígenes de datos](quickstart-onboard.md) a Azure Sentinel, querrá recibir una notificación cuando suceda algo sospechoso. Para ello, Azure Sentinel le permite crear reglas de alerta avanzadas, que generan casos que se pueden asignar y usar para investigar las anomalías y las amenazas del entorno de manera exhaustiva. 

Este tutorial ayuda a detectar amenazas con Azure Sentinel.
> [!div class="checklist"]
> * Crear reglas de detección
> * Automatizar las respuestas frente a amenazas

## <a name="create-detection-rules"></a>Crear reglas de detección

Para investigar los casos, primero debe crear las reglas de detección. 

> [!NOTE]
> Las alertas generadas en Azure Sentinel están disponibles a través de [Seguridad de Microsoft Graph](https://aka.ms/securitygraphdocs). Consulte la [documentación de alertas de seguridad de Microsoft Graph](https://aka.ms/graphsecurityreferencebetadocs) para obtener más detalles y asociados de integración.

Las reglas de detección se basan en los tipos de amenazas y anomalías que quiere detectar de inmediato y que pueden resultar sospechosas en su entorno; gracias a ello, puede asegurarse de detectarlas, investigarlas y corregirlas. 

1. En Azure Portal en Azure Sentinel, seleccione **Analytics**.

   ![Análisis](./media/tutorial-detect-threats/alert-rules.png)

2. En la barra de menú de la parte superior, haga clic en **+ Agregar**.  

   ![Crear regla de alertas](./media/tutorial-detect-threats/create-alert-rule.png)

3. En **Crear regla de alerta**, proporcione un nombre descriptivo y establezca la **Gravedad** según sea necesario. 

4. Cree la consulta en Log Analytics y péguela en el campo **Establecer regla de alerta**. Aquí tiene una consulta de ejemplo que le alertará cuando se cree una cantidad anómala de recursos en Azure Activity.

        AzureActivity
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"
        | where ActivityStatus == "Succeeded"
        | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller

   > [!NOTE]
   > La longitud de la consulta debe estar entre 1 y 10 000 caracteres y no puede contener las palabras "buscar *" y "unión *".


5. En la sección **Asignación de entidades**, use los campos del **tipo de entidad** para asignar las columnas en su consulta a los campos de entidades que reconozca Azure Sentinel. Para cada campo, asigne la columna relevante en la consulta que creó en Log Analytics, al campo de entidad apropiado. Seleccione el nombre de la columna correspondiente en la **propiedad**. Cada entidad incluye varios campos, por ejemplo SID, GUID, etc. Puede asignar la entidad a cualquiera de los campos, y no solo a la entidad de nivel superior.

6. Defina las condiciones de activación de las alertas en **Activación de alertas**. Esta opción definirá las condiciones que activarán las alertas. 

7. Establezca la **frecuencia** para ejecutar la consulta; puede establecer frecuencias de 5 minutos o una vez al día. 

8. Establezca el **período** para controlar la ventana de tiempo correspondiente a la cantidad de datos en los que se ejecuta la consulta; por ejemplo, puede ejecutarse cada hora en 60 minutos de datos.

9. También puede configurar la **supresión**. La supresión es útil cuando quiere evitar que se activen alertas duplicadas del mismo incidente. De esta manera, puede evitar que las alertas se activen durante un período específico. Esto puede ayudarle a evitar alertas duplicadas del mismo incidente y permitirle suprimir alertas consecutivas durante un período de tiempo. Por ejemplo, si la opción **Programación de alertas**  **Frecuencia** se establece en 60 minutos, el **período de programación de alertas** se establece en dos horas y los resultados de la consulta superan el umbral definido, se activará una alerta dos veces: una vez que se detecte por primera vez en los últimos 60 minutos y nuevamente cuando se encuentre en los primeros 60 minutos de las 2 horas de los datos que se van a muestrear. Le recomendamos que, si se activa una alerta, la supresión debe ser en función de la cantidad de tiempo establecida en el período de alerta. En nuestro ejemplo, es posible que quiera establecer la supresión durante 60 minutos, de modo que las alertas solo se activarán para los eventos que ocurrieron durante la hora más reciente.

8. Después de pegar su consulta en el campo para **establecer la regla de alerta**, puede ver inmediatamente una simulación de la alerta en **Simulación de alerta lógica** y que así pueda comprobar la cantidad de datos que se generarán durante un intervalo de tiempo específico para la alerta que creó. Esto dependerá de lo que establezca en **Frecuencia** y en **Umbral**. Si ve que su alerta se activa con demasiada frecuencia, le recomendamos que establezca un número mayor de resultados para que esté por encima de la línea de base promedio.

9. Haga clic en **Crear** para inicializar la regla de alerta. Después de crearla, se crea un caso que contiene la alerta. Puede ver las reglas de detección definidas como filas en la pestaña de **Security Analytics**. También puede ver el número de coincidencias de cada regla: las alertas activadas. Desde esta lista puede habilitar, deshabilitar o eliminar cada regla. También puede seleccionar los puntos suspensivos (...) que están a la derecha y al final de la fila para que cada alerta edite, deshabilite, clone, muestre coincidencias o elimine una regla. La página **Analytics** es una galería de todas sus reglas de alerta activas, incluidas las plantillas que habilite y las reglas de alerta que cree basándose en ellas.

1. Los resultados de las reglas de alerta se pueden ver en la página **Casos**, donde puede clasificar, [investigar casos](tutorial-investigate-cases.md) y solucionar las amenazas.



## <a name="automate-threat-responses"></a>Automatizar las respuestas frente a amenazas

Los equipos de SIEM y SOC pueden verse inundados con alertas de seguridad periódicamente. El volumen de las alertas generadas es tan grande que los administradores de seguridad disponibles están desbordados. Esto genera, con demasiada frecuencia, situaciones en las que no se pueden investigar muchas alertas, lo que hace que la organización sea vulnerable a ataques que pasan desapercibidos. 

Muchas de estas alertas, si no la mayoría, se ajustan a los patrones recurrentes que se pueden resolver mediante acciones de corrección específicas y definidas. Azure Sentinel ya le permite definir la corrección en los cuadernos de estrategias. También es posible establecer la automatización en tiempo real como parte de la definición del cuaderno de estrategias para que pueda automatizar completamente una respuesta definida ante alertas de seguridad determinadas. Con la automatización en tiempo real, los equipos de respuesta pueden reducir de forma significativa su carga de trabajo al automatizar completamente las respuestas rutinarias a tipos de alertas recurrentes, lo que le permite concentrarse más en las alertas singulares, en el analizar patrones, en la búsqueda de amenazas y mucho más.

Para automatizar las respuestas:

1. Elija la alerta para la que quiere automatizar la respuesta.
1. En el menú de navegación del área de trabajo de Azure Sentinel, seleccione **Análisis**.
1. Seleccione la alerta que quiere automatizar. 
1. En la página**Editar regla de alerta**, en **Real-time automation** (Automatización en tiempo real), elija el **Triggered playbook** (Cuaderno de estrategias desencadenado) que quiera ejecutar cuando se cumpla esta regla de alerta.
1. Seleccione **Guardar**.

   ![automatización en tiempo real](./media/tutorial-detect-threats/rt-configuration.png)


Además, puede corregir manualmente una alerta y ejecuta un cuaderno de estrategias desde dentro de la alerta con si hace clic en **View playbooks** (Ver cuaderno de estrategias) y, luego, selecciona un cuaderno de estrategias para ejecutarlo. Para información sobre cómo crear un cuaderno de estrategias nuevo o editar uno existente, consulte el artículo sobre cómo [trabajar con cuadernos de estrategias en Azure Sentinel](tutorial-respond-threats-playbook.md).



## <a name="next-steps"></a>Pasos siguientes
En este tutorial ha aprendido a detectar amenazas casos mediante Azure Sentinel. 

Para aprender a automatizar sus respuestas a las amenazas, consulte [cómo responder a las amenazas mediante los cuadernos de estrategias automáticos](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Responda a amenazas](tutorial-respond-threats-playbook.md) para automatizar sus respuestas a estas.

