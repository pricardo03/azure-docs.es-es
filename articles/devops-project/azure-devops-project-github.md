---
title: 'Tutorial: Creación de una canalización de CI/CD para el código existente mediante Azure DevOps Projects'
description: Con Azure DevOps Projects es fácil empezar a usar Azure. DevOps Projects le ayuda a usar su propio código y repositorio GitHub para iniciar una aplicación en un servicio Azure de su elección en pocos pasos rápidos.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: gwallace
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/27/2019
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 4a48b5c413f6dfb58a1ba3f896ded49250cde306
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286307"
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
> * Limpieza de recursos

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Puede obtener una gratuita mediante [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).
* Acceda a un repositorio de GitHub o a un repositorio de Git externo que contenga código .NET, Java, PHP, Node, Python o código web estático.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Azure DevOps Projects crea una canalización de CI/CD en Azure Pipelines. Puede crear una organización de Azure DevOps nueva o usar una existente. Azure DevOps Projects también crea recursos de Azure en la suscripción de Azure que prefiera.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. En el panel izquierdo, seleccione **+Crear un recurso** en la barra de navegación izquierda, busque **DevOps Projects** y seleccione **Crear**.

   ![El panel de DevOps Projects](_img/azure-devops-project-github/azuredashboard.png)

3. Seleccione **Traiga su propio código** y, después, **Siguiente**.

## <a name="configure-access-to-your-github-repo-and-choose-a-framework"></a>Configurar el acceso a un repositorio de GitHub y elegir un marco

1. Seleccione **GitHub** o un repositorio de código de **Git** externo. Para este tutorial, seleccione **GitHub**. Es posible que se le pida que se autentique en GitHub la primera vez para que Azure pueda acceder al repositorio de GitHub.

2. Para finalizar, seleccione los valores de **Repositorio** y **Rama** y, después, **Siguiente**.

3. Si usa contenedores de Docker, cambie **¿Está la aplicación en un contenedor de Docker?** a **SÍ**; para este tutorial, deje **NO** seleccionado y seleccione **Siguiente**. Para más información sobre el uso de contenedores de Docker, mantenga el mouse sobre el icono **i**.

   ![Marco .NET](_img/azure-devops-project-github/appframework.png)

4. En las listas desplegables, seleccione el **entorno de ejecución de la aplicación** y el **marco** y, después, **Siguiente**. El marco de trabajo de la aplicación que elija determina el tipo de destino de implementación del servicio de Azure disponible.

5. Seleccione el **servicio de Azure** para implementar también la aplicación y, después, **Siguiente**.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Configuración de Azure DevOps y una suscripción de Azure

1. Escriba un **nombre de proyecto**.

2. Cree una **organización de Azure DevOps** gratuita o elija una existente en la lista desplegable.

3. Seleccione la **suscripción de Azure**, escriba el nombre de la **aplicación web** o use el nombre predeterminado. Seleccione una **Ubicación** y, después, **Listo**. En unos minutos, la descripción general de la implementación de DevOps Projects se muestra en Azure Portal.

4. Seleccione **Ir al recurso** para ver el panel de DevOps Projects. En la esquina superior derecha, ancle el **proyecto** a su panel para un acceso rápido. Azure DevOps Projects configura automáticamente una compilación de integración continua y un desencadenador de versión. El código permanece en el repositorio de GitHub o en otro repositorio externo. Una aplicación de ejemplo se configura en un repositorio de su **organización de Azure DevOps**. Una compilación se ejecuta y su aplicación se implementa en Azure.

   ![Vista del panel de DevOps Projects](_img/azure-devops-project-github/projectsdashboard.png)

5. El panel proporciona visibilidad del repositorio de código, la canalización de CI/CD y la aplicación de Azure. A la derecha, en Recursos de Azure, seleccione **Examinar** para ver su aplicación en ejecución.

## <a name="commit-changes-to-github-and-automatically-deploy-them-to-azure"></a>Confirmar cambios en GitHub e implementarlos automáticamente en Azure

A partir de ese momento ya puede empezar a colaborar con un equipo en una aplicación mediante el uso de un proceso de CI/CD que implemente automáticamente el trabajo más reciente en su sitio web. Cada cambio que se realiza en el repositorio de GitHub inicia una compilación en Azure DevOps y una canalización de CD ejecuta una implementación en Azure.

1. En el panel de DevOps Projects, seleccione **Repositorios**. El repositorio de GitHub se abre en una nueva pestaña del explorador. Realice un cambio en la aplicación y haga clic en **Confirmar cambios**.

2. Poco después se inicia una compilación en Azure Pipelines. El estado de la compilación se puede supervisar en el panel de DevOps Projects, pero también desde la organización de Azure DevOps al seleccionar la pestaña **Canalizaciones de compilación** desde el panel del proyecto.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Examen de la canalización de CI/CD de Azure Pipelines

Azure DevOps Projects configura automáticamente una canalización de CI/CD en Azure Pipelines. Explore y personalice la canalización según sea necesario. Para familiarizarse con las canalizaciones de compilación y de versión, siga estos pasos:

1. En el panel de DevOps Projects, seleccione **Canalizaciones de compilación**.

2. Una vez abierta la página **Azure Pipelines**, verá un historial de las compilaciones más recientes y el estado de cada una de ellas.

   ![Compilaciones de la canalización de Azure DevOps](_img/azure-devops-project-github/pipelinesbuildpage.png)

3. En la esquina superior derecha de la página **Compilaciones**, verá opciones para **Editar** la compilación actual, **Cola** para poner en cola una nueva compilación y los puntos suspensivos ( **&#8942;** ) para abrir un menú con más opciones; seleccione **Editar**.

4. La compilación ejecuta varias tareas, como capturar código fuente del repositorio, restaurar dependencias y publicar salidas para implementaciones. A la derecha, debajo de **Nombre**, cambie el nombre de la canalización de compilación por uno más descriptivo. Seleccione **Guardar y poner en cola** y, después, **Guardar**; deje un comentario y vuelva a seleccionar **Guardar**.

   ![Página de compilaciones de Azure DevOps](_img/azure-devops-project-github/buildpage.png)

5. Seleccione el panel **Historial** para ver un registro de auditoría de los cambios recientes en la compilación. Azure DevOps realiza el seguimiento de los cambios realizados en la canalización de compilación y permite comparar las versiones.

6. Seleccione la pestaña **Desencadenadores**. El proyecto de Azure DevOps crea automáticamente un desencadenador de CI con algunas opciones de configuración predeterminadas. Se pueden establecer desencadenadores como **Habilitar la integración continua** para ejecutar una compilación cada vez que se confirma un cambio de código o se pueden programar compilaciones para que se ejecuten en momentos concretos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando dejen de ser necesarios, puede eliminar Azure App Service y los recursos relacionados que ha creado en este tutorial. Para ello, utilice la funcionalidad de **eliminación** del panel de DevOps Projects.

## <a name="next-steps"></a>Pasos siguientes

Cuando configuró el proceso de CI/CD en este tutorial, se crearon automáticamente una canalización de compilación y una canalización de versión en Azure DevOps Projects. Puede modificar estas canalizaciones de compilación y de versión para satisfacer las necesidades de su equipo. Ha aprendido a:

> [!div class="checklist"]
>  * Usar DevOps Projects para crear una canalización de CI/CD
> * Configurar el acceso a un repositorio de GitHub y elegir un marco
> * Configurar Azure DevOps y una suscripción de Azure
> * Confirmar cambios en GitHub e implementarlos automáticamente en Azure
> * Examen de la canalización de CI/CD de Azure Pipelines
> * Limpieza de recursos

Para más información acerca de la canalización de CI/CD, consulte:

> [!div class="nextstepaction"]
> [Definición de la canalización de implementación continua (CD) en varias fases](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

Para más información sobre la supervisión de aplicaciones, consulte:
  
 > [!div class="nextstepaction"]
 > [Qué es Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview)
