---
title: 'Integración continua e implementación continua: Azure IoT Edge | Microsoft Docs'
description: 'Configuración de integración continua e implementación continua: Azure IoT Edge con Azure DevOps, Azure Pipelines'
author: shizn
manager: philmea
ms.author: xshi
ms.date: 12/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: a714cec5ce05473887f9f06d47c75563bf878081
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53386832"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Integración continua e implementación continua en Azure IoT Edge

Puede adoptar fácilmente DevOps con las aplicaciones de Azure IoT Edge con las tareas integradas de Azure IoT Edge en Azure Pipelines o el [complemento de Azure IoT Edge para Jenkins](https://plugins.jenkins.io/azure-iot-edge) en el servidor de Jenkins. En este artículo se muestra cómo puede usar las características de integración continua e implementación continua de Azure Pipelines para compilar, probar e implementar aplicaciones de forma rápida y eficaz en su instancia de Azure IoT Edge. 

En este artículo, aprenderá a:
* Cree e inserte en el repositorio una solución de IoT Edge de ejemplo.
* Configure la integración continua (CI) para compilar la solución.
* Configurar la implementación continua (CD) para implementar la solución y ver las respuestas

![Diagrama: ramas CI y CD para desarrollo y producción](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Creación de una solución de Azure IoT Edge de ejemplo mediante Visual Studio Code

En esta sección, creará una solución de IoT Edge de ejemplo que contendrá pruebas unitarias que puede ejecutar como parte del proceso de compilación. Antes de seguir las instrucciones de esta sección, complete los pasos que se indican en [Desarrollo de una solución de IoT Edge con varios módulos en Visual Studio Code](how-to-develop-multiple-modules-vscode.md).

1. En la paleta de comandos de VS Code, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución de IoT Edge). A continuación, seleccione la carpeta del área de trabajo, proporcione el nombre de la solución (el nombre predeterminado es **EdgeSolution**) y cree un módulo de C# (**SampleModule**) como primer módulo de usuario en esta solución. También debe especificar el repositorio de imágenes de Docker para el primer módulo. El repositorio de imágenes predeterminado se basa en un registro de Docker local (`localhost:5000/filtermodule`). Cámbielo a Azure Container Registry(`<your container registry address>/filtermodule`) o Docker Hub para mejorar la integración continua.

    ![Configuración de Azure Container Registry](./media/how-to-ci-cd/acr.png)

2. La ventana de Visual Studio Code cargará el área de trabajo de la solución de IoT Edge. Opcionalmente, puede escribir y ejecutar **Azure IoT Edge: agregue el módulo IoT Edge**  para agregar más módulos. Hay una carpeta `modules`, una carpeta `.vscode` y un archivo de plantilla de manifiesto de implementación en la carpeta raíz. Todos los códigos del módulo de usuario serán subcarpetas en la carpeta `modules`. `deployment.template.json` es la plantilla del manifiesto de implementación. Algunos de los parámetros de este archivo se analizarán desde `module.json`, que existe en cada carpeta del módulo.

3. Ahora su solución de IoT Edge de ejemplo ya está lista. El módulo predeterminado de C# actúa como módulo de mensajes de canalización. En `deployment.template.json`, verá que esta solución contiene dos módulos. El mensaje se genera a partir del módulo `tempSensor` y se canaliza directamente mediante `FilterModule`, para finalmente enviarse a su centro de IoT.

4. Guarde estos proyectos y, a continuación, comprométase con su instancia de Azure Repos.
    
> [!NOTE]
> Para más información sobre el uso Azure Repos, vea [Share your code with Visual Studio and Azure Repos](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts) (Compartir el código con Visual Studio y Azure Repos).


## <a name="configure-azure-pipelines-for-continuous-integration"></a>Configuración de Azure Pipelines para integración continua
En esta sección, creará una canalización de compilación que está configurada para ejecutarse automáticamente al insertar en el repositorio los cambios realizados en la solución de IoT Edge de ejemplo, y mostrará los registros de compilación en Azure Pipelines.

1. Inicie sesión en la organización de Azure DevOps (**https://dev.azure.com/{your organización}**) y abra el proyecto donde insertó en el repositorio la aplicación de ejemplo.

    ![Inserción del código en el repositorio en Azure Pipelines](./media/how-to-ci-cd/init-project.png)

1. En su instancia de Azure Pipelines, abra la pestaña **Compilaciones**, elija **+ Nueva canalización**. O bien, si ya tiene canalizaciones de compilación, elija el botón **+ Nuevo**. Seleccione luego **Nueva canalización de compilación**.

    ![Creación de una canalización de compilación](./media/how-to-ci-cd/add-new-build.png)

1. Si se le pide, seleccione Azure Repos para su origen. Luego, seleccione el proyecto, el repositorio y la rama donde está ubicado el código. Elija **Continuar**.

    ![Selección de Git de Azure Repos](./media/how-to-ci-cd/select-vsts-git.png)

    En la ventana **Seleccione una plantilla** , elija **Proceso vacío** para comenzar con un proceso vacío.

    ![Empezar con un proceso vacío](./media/how-to-ci-cd/start-with-empty.png)

1. En el editor de canalizaciones, elija el grupo de agentes. 
    
    * Elija si desea compilar los módulos en la plataforma amd64 para contenedores Linux, elija **Hosted Ubuntu 1604**.
    * Elija si desea compilar los módulos en la plataforma amd64 para contenedores Windows, elija **Hosted VS2017**. 
    * Si quiere compilar los módulos en la plataforma arm32v7 para contenedores Linux, tiene que configurar su propio agente de compilación haciendo clic en el botón **Administrar**.
    
    ![Configuración del grupo de agentes de compilación](./media/how-to-ci-cd/configure-env.png)

1. En Trabajo de agente, haga clic en "+" para agregar tres tareas en la canalización de compilación. Las dos primeras son de **Azure IoT Edge**. Y la tercera es de **Publicar los artefactos de la compilación**.
    
    ![Adición de tareas a la canalización de compilación](./media/how-to-ci-cd/add-tasks.png)

1. En la primera tarea de **Azure IoT Edge**, actualice el **Nombre para mostrar** con **Azure IoT Edge - Build module images** (Azure IoT Edge: Compilar imágenes de módulo) y, en la lista desplegable **Acción**, seleccione **Build module images** (Compilar imágenes de módulo). En el control **.template.json file**, seleccione el archivo **deployment.template.json**, que describe su solución IoT Edge. Después elija **Plataforma predeterminada** y asegúrese de seleccionar la misma plataforma del dispositivo de IoT Edge. Esta tarea compilará todos los módulos de la solución con la plataforma de destino que se especificó. Y también generará el archivo **deployment.json**; encontrará la ruta de acceso del archivo en Variables de salida. Defina el alias en `edge` para esta variable.
    
    ![Configuración de la tarea de imágenes del módulo de compilación](./media/how-to-ci-cd/build-and-push.png)

1. En la segunda tarea de **Azure IoT Edge**, actualice el **Nombre para mostrar** con **Azure IoT Edge - Push module images** (Azure IoT Edge: Insertar imágenes de módulo) y, en la lista desplegable **Acción**, seleccione **Push module images** (Insertar imágenes de módulo). Elija Tipo de Container Registry y asegúrese de que configura y selecciona el mismo registro en el código (module.json). En el control **.template.json file**, seleccione el archivo **deployment.template.json**, que describe su solución IoT Edge. Después elija **Plataforma predeterminada** y asegúrese de seleccionar la misma plataforma para las imágenes de módulo compiladas. Esta tarea insertará todas las imágenes de módulo en la instancia de Container Registry que seleccionó. Y también agregará las credenciales de Container Registry en el archivo **deployment.json**; encontrará la ruta de acceso del archivo en Variables de salida. Defina el alias en `edge` para esta variable. Si tiene varias instancias de Container Registry para hospedar las imágenes de módulo, tiene que duplicar esta tarea, seleccionar otra instancia de Container Registry y usar **Bypass module(s)** (Omitir módulos) en la configuración avanzada para omitir las imágenes que no son de este registro específico.

    ![Configuración de la tarea de imágenes del módulo de inserción](./media/how-to-ci-cd/push.png)

1. En la tarea **Publicar los artefactos de la compilación**, debe especificar el archivo de implementación generado por la tarea de compilación. Establezca la **Ruta de acceso para publicar** en `$(edge.DEPLOYMENT_FILE_PATH)`.

    ![Configuración de la tarea de publicación de artefacto](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Abra la pestaña **Desencadenadores** y active el desencadenador **Integración continua**. Asegúrese de que se incluye la rama que contiene el código.

    ![Activación del desencadenador de integración continua](./media/how-to-ci-cd/configure-trigger.png)

    Guarde la nueva canalización de compilación. Haga clic en el botón **Save** (Guardar).


## <a name="configure-azure-pipelines-for-continuous-deployment"></a>Configuración de Azure Pipelines para implementación continua
En esta sección, creará una canalización de versión que está configurada para ejecutarse automáticamente cuando la canalización de compilación coloca artefactos, y mostrará los registros de implementación en Azure Pipelines.

1. En la pestaña **Versiones**, elija **+ Nueva canalización**. O bien, si ya tiene canalizaciones de versión, elija el botón **+ Nuevo** y haga clic en **+ Nueva canalización de versión**.  

    ![Agregar canalización de versión](./media/how-to-ci-cd/add-release-pipeline.png)

    En la ventana **Seleccione una plantilla** , elija **empiece por un Fase vacía**.

    ![Empezar con un trabajo vacío](./media/how-to-ci-cd/start-with-empty-job.png)

2. A continuación, la canalización de versión se inicializaría con una fase: **Fase 1**. Cambie el nombre de **Fase 1** a **QA** y trátela como un entorno de prueba. En una canalización de implementación continua típica, suelen existir varias fases, puede crear más en función de sus prácticas de DevOps.

    ![Creación de la fase del entorno de prueba](./media/how-to-ci-cd/QA-env.png)

3. Vincule la versión a los artefactos de compilación. Haga clic en **Agregar** en el área de artefactos.

    ![Agregar artefactos](./media/how-to-ci-cd/add-artifacts.png)  
    
    En la página **Agregar un artefacto**, elija el tipo de origen **Compilar**. A continuación, seleccione el proyecto y la canalización de compilación que ha creado. A continuación, haga clic en **Agregar**.

    ![Adición de un artefacto de compilación](./media/how-to-ci-cd/add-an-artifact.png)

    Abra el desencadenador de implementación continua para que se cree una versión nueva cada vez que haya disponible una nueva compilación.

    ![Configuración del desencadenador de implementación continua](./media/how-to-ci-cd/add-a-trigger.png)

4. Vaya a la **fase QA** y configure las tareas en esta fase.

    ![Configurar las tareas de QA](./media/how-to-ci-cd/view-stage-tasks.png)

   La tarea de implementación no distingue la plataforma, lo que significa que puede elegir **Hosted VS2017** o **Hosted Ubuntu 1604** en el **Grupo de agentes** (o cualquier otro agente administrado por usted mismo). Haga clic en "+" y agregue una tarea.

    ![Agregar tareas para QA](./media/how-to-ci-cd/add-task-qa.png)

5. En la tarea Azure IoT Edge, vaya a la lista desplegable **Acción**, seleccione **Deploy to IoT Edge devices** (Implementar en dispositivos IoT Edge). Seleccione la **suscripción de Azure** y escriba el **nombre de IoT Hub**. También puede realizar la implementación en uno o varios dispositivos. En va a implementar en **varios dispositivos**, debe especificar la **condición del destino** del dispositivo. La condición de destino es un filtro para asociar un conjunto de dispositivos de Edge en IoT Hub. Si quiere usar etiquetas de dispositivo como condición, debe actualizar las etiquetas de dispositivo correspondientes con el dispositivo gemelo de IoT Hub. Actualice el **identificador de implementación de IoT Edge** a "deploy-qa" en Opciones avanzadas. Suponga que tiene varios dispositivos de IoT Edge etiquetados como "qa", la configuración de la tarea debe ser como se muestra en la captura de pantalla siguiente. 

    ![Implementar en QA](./media/how-to-ci-cd/deploy-to-qa.png)

    Guarde la nueva canalización de versión. Haga clic en el botón **Save** (Guardar). Y, luego, haga clic en **Canalización** para volver a la canalización.

6. La segunda fase es para su entorno de producción. Para agregar una nueva fase "PROD", puede clonar la fase "QA" y cambiar el nombre de la fase clonada a **PROD**.

    ![Clonar fase](./media/how-to-ci-cd/clone-stage.png)

7. Configure las tareas para su entorno de producción. Suponga que tiene varios dispositivos de IoT Edge etiquetados como "prod" en las configuraciones de la tarea, actualice la condición de destino "prod" y establezca el identificador de implementación en "deploy-prod" en Opciones avanzadas. Haga clic en el botón **Save** (Guardar). Y, luego, haga clic en **Canalización** para volver a la canalización.
    
    ![Implementar en producción](./media/how-to-ci-cd/deploy-to-prod.png)

7. Actualmente, el artefacto de compilación se desencadenará continuamente en la fase **QA** y, luego, en la fase **PROD**. Pero la mayoría de las veces tendrá que integrar algunos casos de prueba en los dispositivos de QA y aprobar manualmente los bits. Más adelante los bits se implementarán en el entorno de PROD. Configure una aprobación en la fase PROD tal y como se muestra en la siguiente captura de pantalla.

    1. Abra el panel de configuración **Condiciones anteriores a la implementación**.

        ![Abrir Condiciones anteriores a la implementación](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Establezca **Habilitado** en **Aprobaciones anteriores a la implementación**. Y rellene la entrada **Aprobadores**. A continuación, haga clic en **Guardar**.
    
        ![Establecer las condiciones](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. Ahora la canalización de versión se ha configurado tal y como se muestra en la siguiente captura de pantalla.

    ![Canalización de versión](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Comprobación de CI/CD de IoT Edge con las canalizaciones de compilación y de versión

En esta sección, desencadenará una compilación para hacer funcionar la canalización de CI/CD. Luego verifique que la implementación se complete correctamente.

1. Para desencadenar un trabajo de compilación, puede insertar una confirmación en el repositorio de código fuente o desencadenarlo manualmente. Para desencadenar un trabajo de compilación en la canalización de compilación puede hacer clic en el botón **Cola**, como en la siguiente captura de pantalla.

    ![Desencadenador manual](./media/how-to-ci-cd/manual-trigger.png)

2. Si la canalización de compilación se completa correctamente, desencadenará una versión a la fase **QA**. Navegue a los registros de canalización de compilación donde debe ver la siguiente captura de pantalla.

    ![Registros de compilación](./media/how-to-ci-cd/build-logs.png)

3. La implementación correcta en la fase **QA** desencadenará una notificación al aprobador. Navegue a la canalización de versión, donde puede ver la siguiente captura de pantalla. 

    ![Pendiente de aprobación](./media/how-to-ci-cd/pending-approval.png)


4. Después de que el aprobador aprueba este cambio, se puede implementar en **PROD**.

    ![Implementar en PROD](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Pasos siguientes

* Puede encontrar información sobre la implementación de IoT Edge en [Descripción de las implementaciones de IoT Edge en un único dispositivo o a escala](module-deployment-monitoring.md).
* Siga los pasos para crear, actualizar o eliminar una implementación en [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-monitor.md).
