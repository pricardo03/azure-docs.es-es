---
title: Creación de reglas de análisis personalizadas para detectar amenazas sospechosas con Azure Sentinel | Microsoft Docs
description: Use este tutorial para aprender a crear reglas de análisis personalizadas para detectar amenazas sospechosas con Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: 18c11198f6b81e72e371b3ab06ed3a7330078c52
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72023776"
---
# <a name="tutorial-create-custom-analytic-rules-to-detect-suspicious-threats"></a>Tutorial: Creación de reglas de análisis personalizadas para detectar amenazas sospechosas

Después de [conectar los orígenes de datos](quickstart-onboard.md) con Azure Sentinel, puede crear reglas personalizadas que busquen criterios específicos en el entorno y generen incidentes si se detectan estos criterios para que pueda investigarlos. Este tutorial le ayuda a crear reglas personalizadas para detectar amenazas con Azure Sentinel.

Este tutorial ayuda a detectar amenazas con Azure Sentinel.
> [!div class="checklist"]
> * Creación de reglas de análisis
> * Automatizar las respuestas frente a amenazas

## <a name="create-custom-analytic-rules"></a>Creación de reglas de análisis personalizadas

Puede crear reglas de análisis personalizadas que le ayuden a buscar los tipos de amenazas y anomalías que sean sospechosas en el entorno. La regla garantiza que se le notificará de inmediato, de modo que pueda evaluar, investigar y corregir las amenazas.

1. En Azure Portal en Azure Sentinel, seleccione **Analytics**.

1. En la barra de menús superior, seleccione **+Crear** y seleccione**Regla de consulta programada**. Esto abrirá el **asistente para la creación de reglas personalizadas**.

    ![Crear una consulta programada](media/tutorial-detect-threats-custom/create-scheduled-query.png)

1. En la pestaña**General**, proporcione un nombre descriptivo y una descripción. Establezca **Gravedad de la alerta** según sea necesario. Cuando cree la regla puede habilitarla, lo cual hará que se pueda ejecutar inmediatamente después de finalizar su creación. Si la crea como deshabilitada, la regla se agregará a la pestaña**Reglas activas** y podrá habilitarla desde allí cuando sea necesario.

    ![Empezar a crear una regla de análisis personalizada](media/tutorial-detect-threats-custom/general-tab.png)

1. En la pestaña **Configuración**, puede escribir una consulta directamente o crearla en Log Analytics y pegarla a continuación en el campo **Consulta de búsqueda**. A medida que cambia y configura la consulta, Azure Sentinel simula los resultados de la consulta en la ventana **Vista previa de resultados**, situada a la derecha. Esto le permite obtener información sobre cuántos datos se generarían durante un intervalo de tiempo específico para la alerta que está creando. La cantidad depende de lo que haya establecido en **Run query every** (Ejecutar consulta cada) y en **Lookup data from the last**(Buscar datos desde el más reciente). Si ve que su alerta se activaría con demasiada frecuencia, puede establecer un número mayor de resultados para que esté por encima de la línea de base promedio.

   ![Crear una consulta en Azure Sentinel](media/tutorial-detect-threats-custom/settings-tab.png)

   Aquí tiene una consulta de ejemplo que le alertará cuando se cree una cantidad anómala de recursos en Azure Activity.

    `AzureActivity
    \| where OperationName == "Create or Update Virtual Machine" or OperationName =="Create Deployment"
    \| where ActivityStatus == "Succeeded"
    \| make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller`

   > [!NOTE]
   > La longitud de la consulta debe estar entre 1 y 1,0000 caracteres y no puede contener las palabras "buscar \*" ni "unión \*".

    1. En **Query scheduling** (Programación de consultas), establezca los siguientes parámetros:

        1.  Establezca la opción **Run query every** (Ejecutar consulta cada) para establecer la **frecuencia** de ejecución de la consulta; puede establecer frecuencias de 5 minutos o de una vez al día.

        1.  Establezca la opción **Lookup data from the last** (Buscar datos desde el más reciente) para controlar la ventana de tiempo correspondiente a la cantidad de datos en los que se ejecuta la consulta; por ejemplo, puede ejecutarse cada hora en 60 minutos de datos.

        1. Puede establecer Azure Sentinel en **Stop running the query after alert is generated** (Detener la ejecución de la consulta después de que se genere la alerta) si solo desea recibir la alerta cuando se produce el incidente. Debe establecer el intervalo durante el cual la consulta debe dejar de ejecutarse, 24 horas como máximo.

    1. Defina las condiciones de activación de las alertas en **Activación de alertas**. En **Asignación de entidades**, puede asignar las columnas en su consulta a los campos de entidades que reconoce Azure Sentinel. Para cada campo, asigne la columna relevante en la consulta que creó en Log Analytics, al campo de entidad apropiado. Cada entidad incluye varios campos, por ejemplo SID y GUID. Puede asignar la entidad a cualquiera de los campos, y no solo a la entidad de nivel superior.

1.  En la pestaña **Automate responses** (Automatizar respuestas), seleccione todos los cuadernos de estrategias que desee ejecutar automáticamente cuando la regla personalizada genere una alerta. Para más información sobre la creación y automatización de los cuadernos de estrategias, consulte [Respuesta a amenazas](tutorial-respond-threats-playbook.md).

    ![Automatización de la respuesta a las amenazas en Azure Sentinel](media/tutorial-detect-threats-custom/response-automation-custom.png)

1. Seleccione **Revisar** para revisar todos los valores de la nueva regla de alertas y, después, seleccione **Create to initialize your alert rule** (Crear para inicializar la regla de alertas).

   ![Revisar la consulta personalizada](media/tutorial-detect-threats-custom/review-tab.png)

1.  Una vez creada la alerta, se agrega una regla personalizada a la tabla en **Active rules** (Reglas activas). Desde esta lista puede habilitar, deshabilitar o eliminar cada regla.

1.  Para ver los resultados de las reglas de alertas, vaya a la página de **incidentes**, donde puede evaluar las prioridades,  [investigar los incidentes](tutorial-investigate-cases.md) y solucionar las amenazas.


> [!NOTE]
> Las alertas generadas en Azure Sentinel están disponibles mediante  [Microsoft Graph Security](https://aka.ms/securitygraphdocs). Para más información, consulte la [documentación sobre alertas de Microsoft Graph Security](https://aka.ms/graphsecurityreferencebetadocs).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha aprendido a detectar amenazas casos mediante Azure Sentinel.

Para aprender a automatizar las respuestas a las amenazas, consulte  [Configuración de respuestas automatizadas frente a amenazas en Azure Sentinel](tutorial-respond-threats-playbook.md).

