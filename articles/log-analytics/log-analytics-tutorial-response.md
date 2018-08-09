---
title: Respuesta a eventos con las alertas de Azure Log Analytics | Microsoft Docs
description: Este tutorial le ayuda a comprender las alertas de Log Analytics para identificar información importante en el área de trabajo y avisarle proactivamente de problemas o invocar acciones para intentar corregirlos.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: c6c7b3f897e38fbd67098c9f881380bc073f13da
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432657"
---
# <a name="respond-to-events-with-azure-monitor-alerts"></a>Respuesta a eventos con las alertas de Azure Monitor
Alertas de Azure crea las reglas de búsqueda de registros para ejecutar automáticamente consultas de registros especificadas a intervalos regulares.  Si los resultados de la consulta de registros coinciden con determinados criterios, se crea un registro de alertas. La regla, luego, puede ejecutar de forma automática una o varias acciones con [grupos de acciones](../monitoring-and-diagnostics/monitoring-action-groups.md).  Este tutorial es la continuación del tutorial [Creación y uso compartido de paneles de datos de Log Analytics](log-analytics-tutorial-dashboards.md).   

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una regla de alerta
> * Configurar un grupo de acciones para enviar una notificación por correo electrónico

Para completar el ejemplo en este tutorial, debe disponer de una máquina virtual existente [conectada al área de trabajo de Log Analytics](log-analytics-quick-collect-azurevm.md).

## <a name="sign-in-to-azure-portal"></a>Inicio de sesión en Azure Portal
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-alerts"></a>Creación de alertas
Las alertas se crean mediante reglas de alertas en Azure Monitor y pueden ejecutar automáticamente consultas guardadas o búsquedas de registros personalizadas a intervalos regulares.  Puede crear alertas basadas en métricas de rendimiento específicas o cuando se creen determinados eventos, haya un evento ausente o se cree una serie de eventos dentro de una ventana de tiempo determinada.  Por ejemplo, se pueden usar alertas para avisarle cuando el uso medio de la CPU supere un determinado umbral, cuando se detecta una actualización que falta o cuando se genera un evento al detectar que un servicio de Windows específico o un demonio de Linux no funcionan.  Si los resultados de la búsqueda de registros coinciden con determinados criterios, se crea una alerta. Luego, la regla puede ejecutar automáticamente una o varias acciones, como notificarle de la alerta o invocar otro proceso.

En el ejemplo siguiente, se crea una regla de alertas de medidas de métricas basada en la consulta guardada *Máquinas virtuales de Azure: uso de procesador* del [Tutorial de visualización de datos](log-analytics-tutorial-dashboards.md). Se crea una alerta para cada máquina virtual que supera el umbral del 90 %.

1. En Azure Portal, haga clic en **Todos los servicios**. En la lista de recursos, escriba **Monitor**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Seleccione **Monitor**.
1. En el panel izquierdo, seleccione **Alertas** y, a continuación, haga clic en **Nueva regla de alertas** en la parte superior de la página para crear una nueva alerta.

    ![Creación de una nueva regla de alertas](./media/log-analytics-tutorial-response/alert-rule-02.png)

1. Como primer paso, en la sección **Crear alerta**, seleccione el área de trabajo de Log Analytics como el recurso, ya que se trata de una señal de alerta basada en el registro.  Filtre los resultados seleccionando la **Suscripción** específica en la lista desplegable si tiene más de una, la cual contiene la máquina virtual y el área de trabajo de Log Analytics creados anteriormente.  Filtre el **Tipo de recurso** seleccionando **Log Analytics** en la lista desplegable.  Por último, seleccione el **Recurso** **DefaultLAWorkspace** y, a continuación, haga clic en **Listo**.

    ![Paso 1 de la tarea de creación de alertas](./media/log-analytics-tutorial-response/alert-rule-03.png)

1. En la sección **Criterios de alerta**, haga clic en **Agregar criterios** para definir la consulta y especifique la lógica de la regla de alertas. En el panel **Configurar lógica de señal**, seleccione **Custom log search** (Personalizar búsqueda de registros) como nombre de señal y escriba la consulta en **Consulta de búsqueda**.

    Por ejemplo: 
    ```
    Perf
    | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total"
    | summarize AggregatedValue=avg(CounterValue) by bin(TimeGenerated, 1m)
    ```

    El panel se actualiza para presentar los valores de configuración de la alerta.  En la parte superior, se muestran los resultados de los últimos 30 minutos de la señal seleccionada.

1. Configure la alerta con la siguiente información:  
   a. En la lista desplegable **Basado en*, seleccione **Medida de métricas**.  Una medida de métricas creará una alerta por cada objeto de la consulta con un valor que supere el umbral especificado.  
   b. Para **Condición**, seleccione **Mayor que** y escriba **90** para **Umbral**.  
   c. En la sección Desencadenar alerta según, seleccione **Infracciones consecutivas** y, en la lista desplegable, seleccione **Mayor que** y escriba un valor de 3.  
   d. En la sección Evaluación según, acepte los valores predeterminados. La regla se ejecutará cada cinco minutos y devuelve los registros que se crearon dentro de este intervalo de la hora actual.  
1. Haga clic en **Listo** para finalizar la regla de alertas.

    ![Configuración de la señal de alerta](./media/log-analytics-tutorial-response/alert-signal-logic-02.png)

1. Ahora, en el segundo paso, proporcione un nombre para la alerta en el campo **Nombre de la regla de alertas**, como **Porcentaje de CPU supera el 90%**.  Especifique una **Descripción** con los detalles específicos de la alerta y seleccione **Crítico (Gravedad 0)** para el valor **Gravedad** de las opciones proporcionadas.

    ![Configuración de los detalles de la alerta](./media/log-analytics-tutorial-response/alert-signal-logic-04.png)

1. Para activar inmediatamente la regla de alertas en la creación, acepte el valor predeterminado de **Habilitar regla tras la creación**.  
1. En el paso tercero y último, especifique un **Grupo de acciones**, lo que garantiza que se realizan las mismas acciones cada vez que se desencadena una alerta y se puede utilizar para cada regla que se define.  Configure un nuevo grupo de acciones con la información siguiente:  
   a. Seleccione **Nuevo grupo de acciones** y aparecerá el panel **Agregar grupo de acciones**.  
   b. En **Nombre del grupo de acciones**, especifique un nombre, como **Notificar las operaciones de TI -** y un **Nombre corto**, como **itops-n**.  
   c. Compruebe que los valores predeterminados de **Suscripción** y **Grupo de recursos** son correctos. Si no es así, seleccione los valores correctos en la lista desplegable.  
   d. En la sección Acciones, especifique un nombre para la acción, como **Enviar correo electrónico** y en **Tipo de acción**, seleccione **Correo electrónico/SMS/Push/Voz** en la lista desplegable. El panel de propiedades **Correo electrónico/SMS/Push/Voz** se abrirá a la derecha para proporcionar información adicional.  
   e. En el panel **Correo electrónico/SMS/Push/Voz**, habilite **Correo electrónico** y proporcione una dirección SMTP válida para entregar el mensaje de correo electrónico.  
   f. Haga clic en **Aceptar** para guardar los cambios.  
       ![Crear nuevo grupo de acciones](./media/log-analytics-tutorial-response/action-group-properties-01.png)

1. Seleccione **Aceptar** para crear el grupo de acciones.
1. Haga clic en **Crear regla de alertas** para finalizar la regla de alertas. Se iniciará la ejecución de inmediato.

    ![Finalización de la creación de una nueva regla de alertas](./media/log-analytics-tutorial-response/alert-rule-01.png)

## <a name="view-your-alerts-in-azure-portal"></a>Visualización de las alertas en Azure Portal
Ahora que ha creado una alerta, puede ver las alertas de Azure en un solo panel y administrar todas las reglas de alertas de las suscripciones de Azure. Se enumeran todas las reglas de alertas (habilitadas o deshabilitadas), que se pueden ordenar según los recursos de destino, los grupos de recursos, el nombre de regla o el estado. Se incluye el resumen agregado de todas las alertas desencadenadas, así como el total de reglas de alertas configuradas y habilitadas.

![Página de estado de las Alertas de Azure](./media/log-analytics-tutorial-response/azure-alerts-02.png)

Cuando se desencadena la alerta, la tabla refleja la condición y el número de veces que se ha producido dentro del intervalo de tiempo seleccionado (el valor predeterminado es las últimas seis horas).  Debería haber un correo electrónico correspondiente en la Bandeja de entrada similar al siguiente ejemplo, que muestra la máquina virtual causante del error y los primeros resultados que coinciden con la consulta de búsqueda, en este caso.

![Ejemplo de acción de correo electrónico de la alerta](./media/log-analytics-tutorial-response/azure-alert-email-notification-01.png)

## <a name="next-steps"></a>Pasos siguientes
En este tutorial ha aprendido cómo las reglas de alerta pueden identificar un problema y resolverlo de forma proactiva cuando se ejecuten búsquedas de registro en intervalos programados y coincidan con criterios determinados.

Siga este vínculo para ver ejemplos de scripts de Log Analytics creados previamente.

> [!div class="nextstepaction"]
> [Ejemplos de scripts de Log Analytics](powershell-samples.md)
