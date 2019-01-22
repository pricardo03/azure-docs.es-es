---
title: Creación de paneles personalizados en Azure Application Insights | Microsoft Docs
description: Tutorial para crear paneles de indicadores clave de rendimiento (KPI) personalizados con Azure Application Insights.
keywords: ''
services: application-insights
author: lgayhardt
ms.author: lagayhar
ms.date: 01/11/2019
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 5ce99e06ea1a8e72a8767367ddfd7bbb845c8400
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54318430"
---
# <a name="create-custom-kpi-dashboards-using-azure-application-insights"></a>Creación de paneles de indicadores clave de rendimiento (KPI) personalizados con Azure Application Insights

Puede crear varios paneles en Azure Portal de manera que cada uno incluya iconos en los que se visualicen datos procedentes de varios recursos de Azure en diferentes suscripciones y grupos de recursos.  Puede anclar distintos gráficos y vistas de visión de Azure Application Insights para crear paneles personalizados que le proporcionen una descripción completa del estado y el rendimiento de la aplicación.  Este tutorial le guía a través de la creación de un panel personalizado que incluye varios tipos de datos y visualizaciones de Azure Application Insights.  Aprenderá a:

> [!div class="checklist"]
> * Crear un panel personalizado en Azure
> * Agregar un icono desde la Galería de iconos
> * Agregar métricas estándar de Application Insights al panel 
> * Agregar un gráfico de métricas personalizado de Application Insights al panel
> * Agregar los resultados de una consulta de Analytics al panel 



## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial:

- Implemente una aplicación de .NET en Azure y [habilite el SDK de Application Insights](../../azure-monitor/app/asp-net.md). 

## <a name="log-in-to-azure"></a>Inicio de sesión en Azure
Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-new-dashboard"></a>Creación de un nuevo panel
Un solo panel puede contener recursos de varias aplicaciones, grupos de recursos y suscripciones.  Comience el tutorial creando un nuevo panel para la aplicación.  

2.  En el panel, seleccione **Nuevo panel**.

    ![Nuevo panel](media/tutorial-app-dashboards/1newdashboard.png)

3. Escriba un nombre para el panel.
4. Eche un vistazo a la **Galería de iconos** para ver la variedad de iconos que se pueden agregar al panel.  Además de agregar iconos de la galería, puede anclar gráficos y otras vistas directamente desde Application Insights al panel.
5. Busque el icono **Markdown** y arrástrelo hasta el panel.  Este icono le permite agregar texto en formato Markdown, que es idóneo para agregar texto descriptivo al panel.
6. Agregue texto a las propiedades del icono y cambie su tamaño en el lienzo del panel.
    
    ![Edición del mosaico de Markdown](media/tutorial-app-dashboards/2dashboard-text.png)

6. Haga clic en **Personalización finalizada** en la parte superior de la pantalla para salir del modo de personalización de iconos.

## <a name="add-health-overview"></a>Adición de información general de estado
Un panel que tenga solo texto estático no resulta muy interesante; así pues, agregue ahora un icono de Application Insights para mostrar información acerca de la aplicación.  Puede agregar iconos de Application Insights desde la Galería de iconos, o puede anclarlos directamente desde las pantallas de Application Insights.  Esto le permite configurar gráficos y vistas con las que ya está familiarizado antes de anclarlos al panel.  Empiece por agregar la información general de estado estándar para la aplicación.  Esto no requiere configuración y permite una personalización mínima en el panel.


1. Seleccione el recurso de **Application Insights** en la pantalla de inicio.
2. En el panel **Información general**, haga clic en el icono de la chincheta para agregar el icono al último panel que ha visto.  

    ![Anclaje de Escala de tiempo con información general](media/tutorial-app-dashboards/3overview.png)
 
3. En la esquina superior derecha aparecerá una notificación que indica que el icono estaba anclado al panel. Haga clic en **Anclada al panel** en la notificación para volver al panel o use el panel.
4. El icono ya se ha agregado al panel. Seleccione **Editar** para cambiar el posicionamiento del icono. Haga clic y arrástrelo hasta su posición y haga clic en **Personalización finalizada**. Ahora, el panel tiene un icono con información útil.

    ![Panel con Escala de tiempo con información general](media/tutorial-app-dashboards/4dashboard-edit.png)



## <a name="add-custom-metric-chart"></a>Adición del gráfico de métricas personalizado
El panel **Métricas** panel le permite representar una métrica recopilada por Application Insights a lo largo del tiempo con filtros opcionales y agrupación.  Al igual que todo lo demás en Application Insights, puede agregar este gráfico al panel.  Para ello se requiere antes un poco de personalización.

1. Seleccione el recurso de **Application Insights** en la pantalla de inicio.
1. Seleccione **Métricas**.  
2. Ya se ha creado un gráfico vacío, y se le pedirá que agregue una métrica.  Agregue una métrica al gráfico y, opcionalmente, agregue un filtro y una agrupación.  En el ejemplo siguiente se muestra el número de solicitudes de servidor agrupadas según se hayan completado correctamente o no.  De este modo se obtiene una vista de ejecución de las solicitudes correctas e incorrectas.

    ![Adición de métrica](media/tutorial-app-dashboards/5sumserverrequests.png)

4. Seleccione **Anclar al panel** a la derecha. De este modo, se agrega la vista al último panel con el que estaba trabajando.

    ![Anclaje de gráfico de métricas](media/tutorial-app-dashboards/6sumserverrequests-pin.png)

3.  En la esquina superior derecha aparecerá una notificación que indica que el icono estaba anclado al panel. Haga clic en **Anclada al panel** en la notificación para volver al panel o use el panel del panel.

4. El icono ya se ha agregado al panel. Seleccione **Editar** para cambiar el posicionamiento del icono. Haga clic y arrástrelo hasta su posición y haga clic en **Personalización finalizada**.

    ![Panel con métricas](media/tutorial-app-dashboards/7dashboard-edit2.png)

## <a name="add-analytics-query"></a>Adición de una consulta de Analytics
Azure Application Insights Analytics proporciona un lenguaje de consulta completo que le permite analizar todos los datos recopilados por Application Insights.  Al igual que los gráficos y otras vistas, puede agregar el resultado de una consulta de Analytics al panel.   

Dado que Azure Applications Insights Analytics es un servicio independiente, debe compartir el panel para que incluya una consulta de Analytics. Al compartir un panel de Azure, este se publica como un recurso de Azure que puede poner a disposición de otros usuarios y recursos.  

1. En la parte superior de la pantalla del panel, haga clic en **Compartir**.

    ![Publicación del panel](media/tutorial-app-dashboards/8dashboard-share.png)

2. Mantenga el mismo **Nombre del panel** y seleccione el **Nombre de la suscripción** para compartir el panel.  Haga clic en **Publicar**.  El panel ahora está disponible para otros servicios y suscripciones.  Si lo desea, puede definir los usuarios específicos que deben tener acceso al panel.
1. Seleccione el recurso de **Application Insights** en la pantalla de inicio.
2. Haga clic en **Analytics** en la parte superior de la pantalla para abrir el portal de Analytics.

    ![Inicio de Analytics](media/tutorial-app-dashboards/9analytics.png)

3. Escriba la siguiente consulta, que devuelve las 10 páginas más solicitadas y su número de solicitudes:

    ```
    requests
    | summarize count() by name
    | sort by count_ desc
    | take 10 
    ```

4. Haga clic en **Ejecutar** para validar los resultados de la consulta.
5. Haga clic en el icono de anclaje y seleccione el nombre del panel. La razón por la que esta opción le solicita que seleccione un panel, a diferencia de los pasos anteriores en los que se utilizaba el último panel, es que la consola de Analytics es un servicio independiente y tiene seleccionar entre todos los paneles compartidos disponibles.

    ![Anclaje de una consulta de Analytics](media/tutorial-app-dashboards/10query.png)

5. Antes de volver al panel, agregue otra consulta, pero esta vez procésela como un gráfico para ver las distintas maneras de visualizar una consulta de Analytics en un panel.  Comience con la siguiente consulta, que resume las 10 operaciones principales con la mayoría de las excepciones.

    ```
    exceptions
    | summarize count() by operation_Name
    | sort by count_ desc
    | take 10 
    ```

6. Seleccione **Gráfico** y, a continuación, cambie a **Anillo** para visualizar el resultado.

    ![Gráfico de Analytics](media/tutorial-app-dashboards/11querychart.png)

6. Haga clic en el icono de anclaje para anclar el gráfico en el panel y esta vez, seleccione el vínculo para volver al panel.
4. Los resultados de las consultas se agregan ahora al panel en el formato que seleccionó.  Haga clic todos ellos y arrástrelos hasta su posición, y haga clic en **Personalización finalizada**.
5. Seleccione el icono del lápiz en cada título para darles un título descriptivo.

    ![Panel con Analytics](media/tutorial-app-dashboards/12edit-title.png)

5. Seleccione **Compartir** para volver a publicar los cambios en el panel, que ahora incluye una serie de gráficos y visualizaciones de Application Insights.


## <a name="next-steps"></a>Pasos siguientes
Ahora que ha aprendido a crear paneles personalizados, eche un vistazo al resto de la documentación de Application Insights, que incluye un caso práctico.

> [!div class="nextstepaction"]
> [Diagnósticos detallados](../../azure-monitor/app/devops.md)
