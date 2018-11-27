---
title: 'Tutorial: Creación de una canalización de CI/CD para el código existente mediante Azure DevOps Projects'
description: Con Azure DevOps Projects es fácil empezar a usar Azure. DevOps Projects le ayuda a usar su propio código y repositorio GitHub para iniciar una aplicación en un servicio Azure de su elección en pocos pasos rápidos.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 88ee15a3b5cc53542d9e098dee485b8a526bb9a6
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52161761"
---
# <a name="tutorial-create-a-cicd-pipeline-for-your-existing-code-by-using-azure-devops-projects"></a>Tutorial: Creación de una canalización de CI/CD para el código existente mediante Azure DevOps Projects

Azure DevOps Projects ofrece una experiencia simplificada en la que puede utilizar su código existente y el repositorio de Git, o elegir una aplicación de ejemplo para crear una canalización de integración continua (CI) y entrega continua (CD) en Azure.

Podrá:

> [!div class="checklist"]
> * Usar DevOps Projects para crear una canalización de CI/CD
> * Configurar el acceso a un repositorio de GitHub y elegir un marco
> * Configurar Azure DevOps y una suscripción de Azure 
> * Confirmar cambios en GitHub e implementarlos automáticamente en Azure
> * Examen de la canalización de CI/CD de Azure Pipelines
> * Configurar la supervisión de Azure Application Insights
> * Limpieza de recursos

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Puede obtener una gratuita mediante [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).
* Acceda a un repositorio de GitHub o a un repositorio de Git externo que contenga código .NET, Java, PHP, Node, Python o código web estático.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Azure DevOps Projects crea una canalización de CI/CD en Azure Pipelines. Puede crear una organización de Azure DevOps nueva o usar una existente. Azure DevOps Projects también crea recursos de Azure en la suscripción de Azure que prefiera.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. En el panel izquierdo, seleccione **Nuevo**.

1. En el cuadro de búsqueda, escriba **DevOps Projects** y, después, seleccione **Crear**.

    ![El panel de DevOps Projects](_img/azure-devops-project-github/fullbrowser.png)

1. Seleccione **Traiga su propio código** y, después, **Siguiente**.

## <a name="configure-access-to-your-github-repo-and-choose-a-framework"></a>Configuración del acceso a un repositorio de GitHub y elección de un marco

1. Seleccione **GitHub** o un repositorio Git externo y, después, seleccione el repositorio y la rama que contiene la aplicación.

1. Seleccione su marco web y, después, seleccione **Siguiente**.

    ![Marco .NET](_img/azure-devops-project-github/webframework.png)

    El marco de trabajo de la aplicación que ha elegido antes determina el tipo de destino de implementación del servicio de Azure que está disponible aquí. 
    
1. Seleccione el servicio de destino y, después, seleccione **Siguiente**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Configuración de Azure DevOps y una suscripción de Azure 

1. Cree una organización de Azure DevOps o elija una existente.

     a. Escriba el nombre del proyecto en Azure DevOps. 
    
    b. Seleccione la suscripción de Azure y la ubicación, escriba el nombre de la aplicación y seleccione **Listo**.

    En unos minutos, el panel de DevOps Projects se muestra en Azure Portal. Una aplicación de ejemplo se configura en un repositorio en la organización de Azure DevOps, se ejecuta una compilación y la aplicación se implementa en Azure. Este panel proporciona visibilidad sobre el repositorio de código de GitHub, la canalización de CI/CD y la aplicación de Azure. 
    
1. Seleccione **Examinar** para ver la aplicación en ejecución.

    ![Vista del panel de DevOps Projects](_img/azure-devops-project-github/dashboardnopreview.png) 
    
Azure DevOps Projects configura automáticamente una compilación de integración continua y un desencadenador de versión. El código permanece en el repositorio de GitHub o en otro repositorio externo. 

## <a name="commit-changes-to-github-and-automatically-deploy-them-to-azure"></a>Confirmación de cambios en GitHub y su implementación automática en Azure 

A partir de ese momento ya puede empezar a colaborar con un equipo en una aplicación mediante el uso de un proceso de CI/CD que implemente automáticamente el trabajo más reciente en su sitio web. Cada cambio que se realiza en el repositorio de GitHub inicia una compilación en Azure DevOps y una canalización de CD ejecuta una implementación en Azure.

1. Realice un cambio en la aplicación y confírmelo en el repositorio de GitHub.  
    Poco después se inicia una compilación en Azure Pipelines. El estado de dicha compilación se puede supervisar en el panel de DevOps Projects, pero también se puede supervisar en el explorador con la organización de Azure DevOps.

1. Una vez que se complete la compilación, actualice la aplicación para comprobar los cambios.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Examen de la canalización de CI/CD de Azure Pipelines

Azure DevOps Projects configura automáticamente una canalización de CI/CD en Azure Pipelines. Explore y personalice la canalización según sea necesario. Para familiarizarse con las canalizaciones de compilación y de versión, siga estos pasos:

1. En la parte superior del panel de DevOps Projects, seleccione **Compilar canalizaciones**.  
    Una pestaña del explorador muestra la canalización de compilación del nuevo proyecto.

1. Elija el campo **Estado** y seleccione los puntos suspensivos (...).  
    Un menú muestra varias opciones, como poner en cola una nueva compilación, poner en pausa una compilación y editar la canalización de compilación.

1. Seleccione **Editar**.

1. En este panel puede examinar las distintas tareas de la canalización de compilación.  
    La compilación ejecuta varias tareas, como capturar códigos fuente del repositorio Git, restaurar dependencias y publicar salidas usadas para implementaciones.

1. En la parte superior de la canalización de compilación, seleccione el nombre de esta.

1. Cambie el nombre de la canalización de compilación por otro más descriptivo, seleccione **Guardar y poner en cola** y, luego, **Guardar**.

1. En el nombre de la canalización de compilación, seleccione **Historial**.  
    Verá un registro de auditoría de los cambios recientes de la compilación. Azure DevOps realiza un seguimiento de los cambios realizados en la canalización de compilación y permite comparar las versiones.

1. Seleccione **Desencadenadores**.  
    Azure DevOps Projects crea automáticamente un desencadenador de integración continua y cada confirmación al repositorio inicia una compilación. Opcionalmente, puede elegir incluir o excluir ramas del proceso de integración continua.

1. Seleccione **Retención**.  
        En función del escenario, puede especificar directivas para conservar o quitar un determinado número de compilaciones.

1. Seleccione **Compilación y versión** y, después, **Versiones**.  
    Azure DevOps Projects crea una canalización de versión para administrar implementaciones en Azure.

1. Seleccione los puntos suspensivos (...) que se encuentran junto a la canalización de versión, y, después, **Editar**.  
    La canalización de versión contiene una *canalización*, que define el proceso de lanzamiento. 
    
1. En **Artefactos**, seleccione **Colocar**.  
    La canalización de compilación que ha examinado en los pasos anteriores genera la salida que se usa para el artefacto. 

1. Al lado del icono **Colocar**, seleccione **Desencadenador de implementación continua**.  
    Esta canalización de versión tiene un desencadenador de CD habilitado, que ejecuta una implementación cada vez que hay un nuevo artefacto de compilación disponible. Opcionalmente, puede deshabilitar el desencadenador, con lo que las implementaciones van a requerir una ejecución manual. 

1. En el lado izquierdo, seleccione **Tareas**.  
    Las tareas son las actividades que ejecuta el proceso de implementación. En este ejemplo, se crea una tarea para implementarse en Azure App Service.

1. A la derecha, seleccione **Ver versiones** para mostrar un historial de las versiones.

1. Seleccione los puntos suspensivos (...) que se encuentran junto a una de las versiones y, después, **Abrir**.  
    Hay varios menús para explorar, como un resumen de versiones, elementos de trabajo asociados y pruebas.

1. Seleccione **Confirmaciones**.  
    Esta vista muestra las confirmaciones de código que están asociadas a esta implementación. 

1. Seleccione **Registros**.  
    Los registros contienen información útil sobre el proceso de implementación. Se pueden ver tanto durante las implementaciones como después de ellas.

## <a name="configure-azure-application-insights-monitoring"></a>Configurar la supervisión de Azure Application Insights

Con Azure Application Insights puede supervisar fácilmente el rendimiento y la utilización de la aplicación. Azure DevOps Projects configura automáticamente un recurso de Application Insights para la aplicación. Puede seguir configurando diversas alertas y supervisando las funcionalidades según sea necesario.

1. En Azure Portal, vaya al panel de DevOps Projects. 

1. En la parte inferior derecha, seleccione el vínculo de **Application Insights** para la aplicación.  
    Se abre el panel **Application Insights**. Esta vista contiene información sobre la supervisión de la disponibilidad, el rendimiento y el uso de la aplicación.

    ![El panel Application Insights](_img/azure-devops-project-github/appinsights.png) 

1. Seleccione **Intervalo de tiempo** y, después, elija **Última hora**. Para filtrar los resultados, seleccione **Actualizar**.  
    Ya puede ver toda la actividad de los últimos 60 minutos. Para salir del intervalo de tiempo, seleccione **x**.

1. Seleccione **Alertas** y, después, **Agregar alerta de métrica**. 

1. Escriba el nombre de la alerta.

1. En la lista desplegable **Source Alter on** (Alteración de origen en), seleccione su **recurso de App Service**. <!-- Please confirm whether this should be "Source Alter on" or "Source Alert on" -->

1. En la lista desplegable **Métrica**, examine las distintas métricas de alertas.  
    La alerta predeterminada es para un **tiempo de respuesta de servidor mayor de 1 segundo**. Puede configurar fácilmente diversas alertas para mejorar las funcionalidades de supervisión de la aplicación.

1. Seleccione la casilla **Notify via Email owners, contributors, and readers** (Notificar a través de correo electrónico a lectores, colaboradores y propietarios).  
    Si lo desea, puede realizar acciones adicionales cuando se muestre una alerta mediante la ejecución de una aplicación de lógica de Azure.

1. Seleccione **Aceptar** para crear la alerta.  
    Poco después la alerta aparece como activa en el panel.
    
1. Salga del área **Alertas** y vuelva al panel **Application Insights**.

1. Seleccione **Disponibilidad** y, después, **Agregar prueba**. 

1. Escriba el nombre de una prueba y seleccione **Crear**.  
    Así se crea la prueba de ping simple para comprobar la disponibilidad de la aplicación. Después de unos minutos, los resultados de la prueba están disponibles y el panel de Application Insights muestra un estado de disponibilidad.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando dejen de ser necesarios, puede eliminar Azure App Service y los recursos relacionados que ha creado en este tutorial. Para ello, utilice la funcionalidad de **eliminación** del panel de DevOps Projects.

## <a name="next-steps"></a>Pasos siguientes

Cuando configuró el proceso de CI/CD en este tutorial, se crearon automáticamente una canalización de compilación y una canalización de versión en Azure DevOps Projects. Puede modificar estas canalizaciones de compilación y de versión para satisfacer las necesidades de su equipo. Ha aprendido a:

> [!div class="checklist"]
> * Usar DevOps Projects para crear una canalización de CI/CD
> * Configurar el acceso a un repositorio de GitHub y elegir un marco
> * Configurar Azure DevOps y una suscripción de Azure 
> * Confirmar cambios en GitHub e implementarlos automáticamente en Azure
> * Examen de la canalización de CI/CD de Azure Pipelines
> * Configurar la supervisión de Azure Application Insights
> * Limpieza de recursos

Para más información acerca de la canalización de CI/CD, consulte:

> [!div class="nextstepaction"]
> [Definición de la canalización de implementación continua (CD) en varias fases](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
