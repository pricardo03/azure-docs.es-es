---
title: 'Canalización de CI/CD con Azure DevOps Projects: Azure IoT Edge | Microsoft Docs'
description: Con Azure DevOps Projects es fácil empezar a usar Azure. En pocos y rápidos pasos le ayuda a iniciar la aplicación de Azure IoT Edge que prefiera.
author: shizn
manager: ''
ms.author: xshi
ms.date: 12/04/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: ebb7e515f9d9205f364d50b3d686c68a2988f86a
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074221"
---
# <a name="create-a-cicd-pipeline-for-iot-edge-with-azure-devops-projects-preview"></a>Creación de una canalización de CI/CD para IoT Edge con Azure DevOps Projects (versión preliminar)

Configure la integración continua (CI) y la integración continua (CD) para la aplicación de IoT Edge con DevOps Projects. DevOps Projects simplifica la configuración inicial de una canalización de compilación y de versión en Azure Pipelines.

Si no tiene una suscripción activa a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de comenzar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

DevOps Projects crea una canalización de CI/CD en Azure DevOps. Puede crear una organización de Azure DevOps o usar una existente. DevOps Projects también crea recursos de Azure en la suscripción de Azure que prefiera.

1. Inicie sesión en el [Portal de Microsoft Azure](https://portal.azure.com).

1. En el panel izquierdo, seleccione el icono **Crear un recurso** en la barra de navegación izquierda y busque **DevOps Projects**.  

1.  Seleccione **Crear**.

## <a name="select-a-sample-application-and-azure-service"></a>Seleccione una aplicación de ejemplo y el servicio de Azure

1. Los módulos de Azure IoT Edge se pueden escribir en [C#](tutorial-csharp-module.md), [Node.js](tutorial-node-module.md), [Python](tutorial-python-module.md), [C](tutorial-c-module.md) y [Java](tutorial-java-module.md). Seleccione el lenguaje que prefiera para iniciar una aplicación nueva. Del mismo modo, puede seleccionar **.NET**, **Node.js**, **Python**, **C** o **Java** y, luego, hacer clic en **Siguiente**.

    ![Selección del lenguaje para crear una aplicación](./media/how-to-devops-project/select-language.png)

2. Seleccione **IoT simple (versión preliminar)** y haga clic en **Siguiente**.

    ![Selección de la plataforma IoT simple](media/how-to-devops-project/select-iot.png)

3. Seleccione **IoT Edge** y, luego, **Siguiente**.

    ![Selección del servicio IoT Edge](media/how-to-devops-project/select-iot-edge.png)

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Configuración de Azure DevOps y una suscripción de Azure

1. Cree una organización de Azure DevOps gratuita o elija una existente.

     a. Elija un nombre para el proyecto. 

    b. Seleccione la suscripción de Azure y la ubicación, elija el nombre de la aplicación y seleccione **Listo**.  

    ![Nombre y creación de la aplicación](media/how-to-devops-project/select-devops.png)

1. En unos minutos, el panel de DevOps Projects se muestra en Azure Portal. Una aplicación IoT Edge de ejemplo se configura en un repositorio en la organización de Azure DevOps, se ejecuta una compilación y la aplicación se implementa en el dispositivo IoT Edge. Este panel proporciona visibilidad sobre el repositorio de código, la canalización de CI/CD y la aplicación de Azure.

    ![Visualización de la aplicación en el portal de DevOps](./media/how-to-devops-project/devops-portal.png)


## <a name="commit-code-changes-and-execute-cicd"></a>Confirmación de los cambios de código y ejecución de CI/CD

DevOps Projects ha creado un repositorio Git en Azure Repos o GitHub. Para ver el repositorio y realizar cambios en el código de la aplicación, siga estos pasos:

1. En el lado izquierdo del panel de DevOps Projects, seleccione el vínculo de la rama **maestra**.  
Este vínculo abre una vista al repositorio de Git recién creado.

1. Para ver la dirección URL de clonación del repositorio, seleccione **Clonar** en la parte superior derecha del explorador. Puede clonar el repositorio GIT en VS Code o en otras herramientas que prefiera. En los pasos siguientes, puede usar el explorador web para realizar los cambios en el código y confirmarlos directamente en la rama maestra.

    ![Clonación del repositorio GIT](media/how-to-devops-project/clone.png)

1. A la izquierda del explorador, vaya al archivo **modules/FilterModule/module.json**.

1. Seleccione **Editar** y haga un cambio en `"version"` en `"tag"`. Por ejemplo, lo puede actualizar a `"version": "${BUILD_BUILDID}"` para usar las [variables de compilación de Azure DevOps](https://docs.microsoft.com/azure/devops/pipelines/build/variables?view=vsts#build-variables) como parte de la etiqueta de imagen de módulo Azure IoT Edge.

    ![Edición de la versión para que acepte variables de compilación](media/how-to-devops-project/update-module-json.png)

1. Seleccione **Confirmar** y guarde los cambios.

1. En el explorador, vaya al panel de proyectos de Azure DevOps.  Debería ver que hay una compilación en curso. Los cambios que ha realizado se compilan e implementan automáticamente a través de una canalización de CI/CD.

    ![Visualización del estado en curso](media/how-to-devops-project/ci-cd-in-progress.png)

## <a name="examine-the-cicd-pipeline"></a>Examen de la canalización de CI/CD

En el paso anterior, Azure DevOps Projects configuró automáticamente una canalización de CI/CD completa para la aplicación de IoT Edge. Explore y personalice la canalización según sea necesario. Para familiarizarse con las canalizaciones de compilación y versión de Azure DevOps, siga estos pasos:

1. En la parte superior del panel de DevOps Projects, seleccione **Compilar canalizaciones**.  
Este vínculo abre una pestaña del explorador y la canalización de compilación de Azure DevOps del nuevo proyecto.

1. Seleccione **Editar**.

    ![Edición de la canalización de compilación](media/how-to-devops-project/click-edit-button.png)

1. En este panel puede examinar las distintas tareas de la canalización de compilación. La compilación realiza varias tareas que se usan para las implementaciones, como capturar orígenes del repositorio GIT, crear imágenes de módulo IoT Edge, insertar módulos de IoT Edge y publicar las salidas usadas. Para más información sobre las tareas de Azure IoT Edge para CI, puede visitar [Configuración de Azure Pipelines para integración continua](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-integration).

    ![Visualización de las tareas de integración continua](media/how-to-devops-project/ci.png)

1. En la parte superior de la canalización de compilación, seleccione el nombre de esta.

1. Cambie el nombre de la canalización de compilación por otro más descriptivo, seleccione **Guardar y poner en cola** y, luego, **Guardar**.

1. En el nombre de la canalización de compilación, seleccione **Historial**.   
En el panel **Historial** verá un registro de auditoría de los cambios recientes en la compilación.  Azure Pipelines realiza un seguimiento de los cambios que se realizan en la canalización de compilación y permite comparar versiones.

1. Seleccione **Desencadenadores**. DevOps Projects creó automáticamente un desencadenador de integración continua y cada confirmación en el repositorio inicia una compilación.  Si lo desea, puede elegir incluir o excluir ramas del proceso de CI.

1. Seleccione **Retención**. En función del escenario, puede especificar directivas para conservar o quitar un determinado número de compilaciones.

1. Seleccione **Versión** en **Canalizaciones**. DevOps Projects crea una canalización de versión para administrar implementaciones en Azure IoT Edge.

    ![Visualización de la canalización de versión](media/how-to-devops-project/release-pipeline.png)

1. Seleccione **Editar**. La canalización de versión contiene una canalización que define el proceso de versión.  

1. En **Artefactos**, seleccione **Colocar**. La canalización de compilación que ha examinado en los pasos anteriores genera la salida usada por el artefacto. 

1. Al lado del icono **Colocar**, seleccione **Desencadenador de implementación continua**.  
Esta canalización de versión tiene un desencadenador de implementación continua habilitado que ejecuta una implementación cada vez que hay un nuevo artefacto de compilación disponible. Opcionalmente, puede deshabilitar el desencadenador, con lo que las implementaciones van a requerir una ejecución manual.  

1. En el lado izquierdo, seleccione **Tareas**. Las tareas son las actividades que ejecuta el proceso de implementación. En este ejemplo se creó una tarea para implementar las imágenes de módulo en Azure IoT Edge. Para más información sobre las tareas de Azure IoT Edge para CD, puede visitar [Configuración de Azure Pipelines para implementación continua](https://docs.microsoft.com/azure/iot-edge/how-to-ci-cd#configure-azure-pipelines-for-continuous-deployment).

    ![Visualización de las tareas de implementación continua](media/how-to-devops-project/dev-release.png)

1. A la derecha, seleccione **Ver versiones**. Esta vista muestra un historial de versiones.

1. Seleccione los puntos suspensivos (...) junto a una de las versiones y, luego, seleccione **Abrir**.  
Hay varios menús para explorar, como un resumen de las versiones, elementos de trabajo asociados y las pruebas.

1. Seleccione **Confirmaciones**. Esta vista muestra las confirmaciones de código asociadas a la implementación concreta. 

1. Seleccione **Registros**. Los registros contienen información útil sobre el proceso de implementación. Pueden verse durante y después de las implementaciones.


## <a name="clean-up-resources"></a>Limpieza de recursos

Azure App Service y otros recursos relacionados que haya creado se pueden eliminar cuando no se necesiten. Use la funcionalidad de **eliminación** del panel de DevOps Projects.

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información sobre las tareas de Azure IoT Edge en Azure DevOps en [Integración continua e implementación continua en Azure IoT Edge](how-to-ci-cd.md)
* Puede encontrar información sobre la implementación de IoT Edge en [Descripción de las implementaciones de IoT Edge en un único dispositivo o a escala](module-deployment-monitoring.md).
* Siga los pasos para crear, actualizar o eliminar una implementación en [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-monitor.md).
