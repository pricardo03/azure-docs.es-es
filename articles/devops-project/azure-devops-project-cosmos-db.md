---
title: 'Tutorial: Implementación de aplicaciones de Node.js con tecnología de Azure Cosmos DB con Azure DevOps Projects'
description: Con Azure DevOps Projects es fácil empezar a usar Azure. Con DevOps Projects, puede implementar la aplicación de Node.js con tecnología de Azure Cosmos DB en Windows Web App en tan solo unos pocos pasos.
ms.author: mlearned
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/11/2019
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 4310807423600b96078ee48a04a5ad6dab68cd7e
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813121"
---
# <a name="deploy-nodejs-apps-powered-by-azure-cosmos-db-with-devops-projects"></a>Implementación de aplicaciones de Node.js con tecnología de Azure Cosmos DB con DevOps Projects

Azure DevOps Projects ofrece una experiencia simplificada en la que puede utilizar su código existente y el repositorio de Git, o seleccionar una aplicación de ejemplo para crear una canalización de integración (CI) y entrega continuas (CD) en Azure.

DevOps Projects también:

* Crea automáticamente recursos de Azure, como Azure Cosmos DB, Application Insights y App Service.

* Crea y configura una canalización de versión en Azure DevOps para CI/CD.

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * Usar DevOps Projects para la implementación de aplicaciones de Node.js con tecnología de Azure Cosmos DB
> * Configuración de Azure DevOps y una suscripción de Azure
> * Examinar la base de datos de Azure Cosmos DB
> * Examen de la canalización de CI
> * Examen de la canalización de CD
> * Confirmar los cambios en Git e implementarlos automáticamente en Azure
> * Limpiar los recursos

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Puede obtener una gratuita mediante [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/).

## <a name="use-devops-projects-to-deploy-nodejs-app"></a>Uso de DevOps Projects para implementar una aplicación de Node.js

DevOps Projects crea una canalización de CI/CD en Azure Pipelines. Puede crear una organización de Azure DevOps nueva o usar una existente. DevOps Projects también crea recursos de Azure, como Azure Cosmos DB, Application Insights, App Service y App Service Plan, en la suscripción de Azure que elija.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)

1. En el panel izquierdo, seleccione la sección **Crear un recurso**.

1. En el cuadro de búsqueda, escriba **DevOps Projects** y, después, haga clic en **Agregar**.

   ![DevOps Projects](_img/azure-devops-project-cosmos-db/devops-project.png)

1. Seleccione **Node. js** como entorno de ejecución y, después, seleccione **Siguiente**. En **Elegir un marco de trabajo de la aplicación**, seleccione **Express.js**.

1. Habilite la sección **Agregar una base de datos** para **Cosmos DB** y haga clic en **Siguiente**.

    ![Agregar base de datos](_img/azure-devops-project-cosmos-db/add-database.png)

    Cosmos DB admite diversos marcos de trabajo de la aplicación como **Express.js**, **Node.js** y **Sail.js**. En este tutorial, vamos a considerar **Express.js**.

1. Seleccione un servicio de Azure para implementar la aplicación. Tiene servicios diferentes como Windows Web App, Kubernetes Service y Web App for Containers. En este tutorial, usaremos **Windows Web App**. Haga clic en **Siguiente**.

## <a name="configure-azure-devops-and-azure-subscription"></a>Configuración de Azure DevOps y una suscripción de Azure

1. Escriba el nombre del proyecto de Azure DevOps.

1. Cree una organización de Azure DevOps nueva o seleccione una existente.

1. Seleccione su suscripción a Azure.

1. Para ver más opciones de configuración de Azure e identificar el plan de tarifa y la ubicación, haga clic en Configuración adicional. Este panel muestra varias opciones para configurar el plan de tarifa y la ubicación de los servicios de Azure.

1. Salga del área de configuración de Azure y seleccione **Listo**.

1. Pocos minutos después el proceso se ha completado. Una aplicación de Node.js de ejemplo se configura en un repositorio de Git de su organización de Azure DevOps, se crea una instancia de Azure Cosmos DB, App Service, un plan de App Service y Application Insights, se ejecuta una canalización de CI/CD y se implementa la aplicación en Azure.

   Cuando todo esto finaliza, se muestra el panel de Azure DevOps Projects en Azure Portal. También puede ir al panel de DevOps Projects directamente desde la opción **Todos los recursos** de Azure Portal.

   Este panel proporciona visibilidad sobre el repositorio de código de Azure DevOps, la canalización de CI/CD y la instancia de Azure Cosmos DB. En la canalización de Azure DevOps se pueden configurar otras opciones de CI/CD. A la derecha, seleccione **Azure Cosmos DB** para que se vea.

## <a name="examine-the-azure-cosmos-db"></a>Examinar la base de datos de Azure Cosmos DB

DevOps Projects configura automáticamente una instancia de Cosmos DB que puede explorar y personalizar. Para familiarizarse con ella, siga estos pasos:

1. Vaya al panel de DevOps Projects.

    ![Panel de DevOps Projects](_img/azure-devops-project-cosmos-db/devops-project-dashboard.png)

1. A la derecha, seleccione el servicio Cosmos DB. Se abre un panel para la instancia de Cosmos DB. En esta vista puede realizar varias acciones, como la supervisión de operaciones y la búsqueda de registros.

    ![Function App](_img/azure-devops-project-cosmos-db/cosmos-db.png)

## <a name="examine-the-ci-pipeline"></a>Examen de la canalización de CI

DevOps Projects configura automáticamente una canalización de CI/CD de Azure completa en su organización de Azure DevOps. Puede explorar y personalizar la canalización. Para familiarizarse con ella, siga estos pasos:

1. Vaya al panel de DevOps Projects.

1. Haga clic en el hipervínculo en **Compilar**. Una pestaña del explorador muestra la canalización de compilación del nuevo proyecto.

    ![Compilación](_img/azure-devops-project-cosmos-db/build.png)

1. Seleccione **Editar**. En este panel puede examinar las distintas tareas de la canalización de compilación. La compilación ejecuta varias tareas, como capturar códigos fuente del repositorio de Git, compilar la aplicación, ejecutar pruebas unitarias y publicar salidas que se usan para implementaciones.

1. Seleccione **Desencadenadores**. DevOps Projects crea automáticamente un desencadenador de integración continua y cada confirmación al repositorio inicia una compilación. Opcionalmente, puede elegir incluir o excluir ramas del proceso de integración continua.

1. Seleccione **Retención**. En función del escenario, puede especificar directivas para conservar o quitar un determinado número de compilaciones.

1. En la parte superior de la canalización de compilación, seleccione el nombre de esta.

1. Cambie el nombre de la canalización de compilación por otro más descriptivo y seleccione **Guardar** en el menú desplegable **Guardar y poner en cola**.

1. En el nombre de la canalización de compilación, seleccione **Historial**. Este panel muestra un registro de auditoría de los cambios recientes de la compilación. Azure DevOps realiza un seguimiento de los cambios realizados en la canalización de compilación y permite comparar las versiones.

## <a name="examine-the-cd-release-pipeline"></a>Examen de la canalización de versión de la implementación continua

DevOps Projects crea y configura automáticamente los pasos necesarios para implementar desde la organización de Azure DevOps en la suscripción de Azure. Dichos pasos incluyen la configuración de una conexión de servicio de Azure para realizar la autenticación de Azure DevOps en la suscripción de Azure. La automatización también crea una canalización de versión, lo que proporciona la implementación continua en Azure. Para más información acerca de la canalización de versión, siga estos pasos:

1. Vaya a **Canalizaciones | Versiones**.

1. Haga clic en **Editar**.

1. En **Artefactos**, seleccione **Colocar**. La canalización de compilación que ha examinado en los pasos anteriores genera la salida que se usa para el artefacto.

1. A la derecha del icono **Colocar**, seleccione **Desencadenador de implementación continua**. Esta canalización de versión tiene un desencadenador de implementación continua habilitado, que ejecuta una implementación cada vez que hay un nuevo artefacto de compilación disponible. Opcionalmente, puede deshabilitar el desencadenador, con lo que las implementaciones van a requerir una ejecución manual.

1. A la derecha, seleccione la sección **Ver versiones** para mostrar un historial de las versiones.

1. Haga clic en la versión que mostrará la canalización. Haga clic en cualquier entorno para comprobar los **Elementos de trabajo** asociados al **resumen y confirmaciones** de la versión.

1. Seleccione **Confirmaciones**. Esta vista muestra las confirmaciones de código que están asociadas a esta implementación. Compare las versiones para ver las diferencias de confirmación entre las implementaciones.

1. Seleccione **Ver registros**. Los registros contienen información útil sobre el proceso de implementación. Se pueden ver tanto durante las implementaciones como después de ellas.

## <a name="commit-code-changes-and-execute-cicd"></a>Confirmación de los cambios de código y ejecución de CI/CD

> [!NOTE]
> El siguiente procedimiento prueba la canalización de CI/CD. Para ello, realiza un cambio de texto simple.

A partir de ese momento ya puede empezar a colaborar con un equipo en una aplicación mediante el uso de un proceso de CI/CD que implemente automáticamente el trabajo más reciente en Azure App Service. Cada cambio que se realiza en el repositorio de Git inicia una compilación en Azure DevOps y una canalización de CD ejecuta una implementación en Azure. Siga el procedimiento descrito en esta sección o utilice otra técnica para confirmar los cambios en el repositorio. Por ejemplo, puede clonar el repositorio de Git en su herramienta favorita o IDE, y luego insertar los cambios en este repositorio.

1. En el menú de Azure DevOps, seleccione **Repositorio | Archivos** y, a continuación, vaya al repositorio.

1. El repositorio ya incluye código basado en el lenguaje de la aplicación que eligió en el proceso de creación. Abra el archivo **Application/views/index.pug**.

1. Seleccione **Editar** y, a continuación, haga un cambio en el **número de línea 15**. Por ejemplo, puede actualizarlo a **Mi primera implementación en Azure App Service con tecnología de Azure Cosmos DB**

1. En la parte superior derecha, seleccione **Confirmar** y, después, seleccione **Confirmar** de nuevo para insertar el cambio.

     Tras unos instantes, se inicia una compilación se inicia en Azure DevOps y se ejecuta una versión para implementar los cambios. Supervise el estado de la compilación en el panel de DevOps Projects o en el explorador con la organización de Azure DevOps.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no necesite los recursos relacionados que haya creado puede eliminarlos. Use la funcionalidad de **eliminación** del panel de DevOps Projects.

## <a name="next-steps"></a>Pasos siguientes

Si lo desea, puede modificar estas canalizaciones de compilación y de versión para satisfacer las necesidades de su equipo. También puede usar este patrón de CI/CD como plantilla para las demás canalizaciones. En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Usar DevOps Projects para la implementación de aplicaciones de Node.js con tecnología de Azure Cosmos DB
> * Configuración de Azure DevOps y una suscripción de Azure 
> * Examinar la base de datos de Azure Cosmos DB
> * Examen de la canalización de CI
> * Examen de la canalización de CD
> * Confirmación de los cambios en Git y su implementación automática en Azure
> * Limpieza de recursos
