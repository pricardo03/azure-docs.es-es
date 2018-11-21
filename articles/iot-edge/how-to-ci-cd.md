---
title: Integración continua e implementación continua de Azure IoT Edge | Microsoft Docs
description: Información general sobre la integración continua y la implementación continua de Azure IoT Edge
author: shizn
manager: ''
ms.author: xshi
ms.date: 11/12/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 06dec64a55aaece4cd67ebf0485e34aa206a8936
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51633740"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Integración continua e implementación continua en Azure IoT Edge

Puede adoptar fácilmente DevOps con las aplicaciones de Azure IoT Edge con [Azure IoT Edge para Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) o el [complemento de Azure IoT Edge para Jenkins](https://plugins.jenkins.io/azure-iot-edge). En este artículo se muestra cómo puede usar las características de integración continua e implementación continua de Azure Pipelines y Microsoft Team Foundation Server (TFS) para compilar, probar e implementar aplicaciones de forma rápida y eficaz en su instancia de Azure IoT Edge. 

En este artículo, aprenderá a:
* Cree e inserte en el repositorio una solución de IoT Edge de ejemplo.
* Instale la extensión de Azure IoT Edge para Azure DevOps.
* Configure la integración continua (CI) para compilar la solución.
* Configurar la implementación continua (CD) para implementar la solución y ver las respuestas

Tardará 30 minutos en completar los pasos descritos en este artículo.

![CI y CD](./media/how-to-ci-cd/cd.png)


## <a name="create-a-sample-azure-iot-edge-solution-using-visual-studio-code"></a>Creación de una solución de Azure IoT Edge de ejemplo mediante Visual Studio Code

En esta sección, creará una solución de IoT Edge de ejemplo que contendrá pruebas unitarias que puede ejecutar como parte del proceso de compilación. Antes de seguir las instrucciones de esta sección, complete los pasos que se indican en [Desarrollo de una solución de IoT Edge con varios módulos en Visual Studio Code](tutorial-multiple-modules-in-vscode.md).

1. En la paleta de comandos de VS Code, escriba y ejecute el comando **Azure IoT Edge: New IoT Edge solution** (Azure IoT Edge: nueva solución IoT Edge). A continuación, seleccione la carpeta del área de trabajo, proporcione el nombre de la solución (el nombre predeterminado es **EdgeSolution**) y cree un módulo de C# (**SampleModule**) como primer módulo de usuario en esta solución. También debe especificar el repositorio de imágenes de Docker para el primer módulo. El repositorio de imágenes predeterminado se basa en un registro de Docker local (`localhost:5000/filtermodule`). Deberá cambiarlo por Azure Container Registry(`<your container registry address>/filtermodule`) o Docker Hub para mejorar la integración continua.

    ![Configurar ACR](./media/how-to-ci-cd/acr.png)

2. La ventana de Visual Studio Code cargará el área de trabajo de la solución de IoT Edge. Opcionalmente, puede escribir y ejecutar **Azure IoT Edge: Agregar módulo IoT Edge** para agregar más módulos. Hay una carpeta `modules`, una carpeta `.vscode` y un archivo de plantilla de manifiesto de implementación en la carpeta raíz. Todos los códigos del módulo de usuario serán subcarpetas en la carpeta `modules`. `deployment.template.json` es la plantilla del manifiesto de implementación. Algunos de los parámetros de este archivo se analizarán desde `module.json`, que existe en cada carpeta del módulo.

3. Ahora su solución de IoT Edge de ejemplo ya está lista. El módulo predeterminado de C# actúa como módulo de mensajes de canalización. En `deployment.template.json`, verá que esta solución contiene dos módulos. El mensaje se genera a partir del módulo `tempSensor` y se canaliza directamente mediante `FilterModule`, para finalmente enviarse a su centro de IoT.

4. Guarde estos proyectos e insértelos en el repositorio de Azure Repos o TFS.
    
> [!NOTE]
> Para más información sobre el uso Azure Repos, vea [Share your code with Visual Studio and Azure Repos](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts) (Compartir el código con Visual Studio y Azure Repos).


## <a name="configure-azure-pipelines-for-continuous-integration"></a>Configuración de Azure Pipelines para integración continua
En esta sección, creará una canalización de compilación que está configurada para ejecutarse automáticamente al insertar en el repositorio los cambios realizados en la solución de IoT Edge de ejemplo, y mostrará los registros de compilación en Azure Pipelines.

1. Inicie sesión en la organización de Azure DevOps (**https://dev.azure.com/{your organización}**) y abra el proyecto donde insertó en el repositorio la aplicación de ejemplo.

    ![Inserción del código en el repositorio](./media/how-to-ci-cd/init-project.png)

1. Visite [Azure IoT Edge para Azure Pipelines](https://marketplace.visualstudio.com/items?itemName=vsc-iot.iot-edge-build-deploy) en Azure DevOps Marketplace. Haga clic en **Obtener gratis** y siga los pasos del asistente para instalar esta extensión en la organización de Azure DevOps o descargarla en su TFS.

    ![Instalación de la extensión](./media/how-to-ci-cd/install-extension.png)

1. En su instancia de Azure Pipelines, abra el centro **Compilación y Versión** y, en la pestaña **Compilaciones**, elija **+ Nueva canalización**. O bien, si ya tiene canalizaciones de compilación, elija el botón **+ Nuevo**.

    ![Nueva canalización](./media/how-to-ci-cd/add-new-build.png)

1. Si se le solicite, seleccione **Git** en tipo de origen. Luego, seleccione el proyecto, el repositorio y la rama donde está ubicado el código. Elija **Continuar**.

    ![Seleccionar Git](./media/how-to-ci-cd/select-vsts-git.png)

    En la ventana **Seleccione una plantilla** , elija **Proceso vacío** para comenzar con un proceso vacío.

    ![Seleccione una plantilla:](./media/how-to-ci-cd/start-with-empty.png)

1. En el editor de canalizaciones, elija el grupo de agentes. 
    
    * Elija si desea compilar los módulos en la plataforma amd64 para contenedores Linux, elija **Hosted Ubuntu 1604**.
    * Elija si desea compilar los módulos en la plataforma amd64 para contenedores Windows, elija **Hosted VS2017**. 
    * Si quiere compilar los módulos en la plataforma arm32v7 para contenedores Linux, tiene que configurar su propio agente de compilación haciendo clic en el botón **Administrar**.
    
    ![Configuración del agente de compilación](./media/how-to-ci-cd/configure-env.png)

1. En Trabajo de agente, haga clic en "+" para agregar dos tareas en la canalización de compilación. La primera de ellas es de **Azure IoT Edge**. Y la segunda es de **Publicar los artefactos de la compilación**
    
    ![Adición de tareas](./media/how-to-ci-cd/add-tasks.png)

1. En la primera tarea de **Azure IoT Edge**, actualice el **Nombre para mostrar** con **Module Build and Push** y, en la lista desplegable **Acción**, seleccione **Build and Push** (Compilar e insertar). En el cuadro de texto **Archivo Module.json**, agregue a continuación la ruta de acceso a él. Luego, elija **Tipo de Container Registry** y asegúrese de que configura y selecciona el mismo registro en el código (module.json). Esta tarea compila e inserta todos los módulos en la solución y los publica en el registro de contenedor especificado. Si los módulos se insertan en diferentes registros, puede tener varias tareas **Module Build and Push** (Compilación e inserción del módulo). En caso de que la solución de IoT Edge no esté bajo la raíz del repositorio de código, puede especificar **Path of Edge solution root** (Ruta de acceso de la raíz de la solución Edge) en la definición de compilación.
    
    ![Compilación e inserción](./media/how-to-ci-cd/build-and-push.png)

1. En la tarea **Publicar los artefactos de la compilación**, debe especificar el archivo de implementación generado por la tarea de compilación. Establezca **Ruta de acceso para publicar** en "config/deployment.json". Si establece **Path of Edge solution root** en la última tarea, tendrá que combinar la ruta de acceso de la raíz aquí. Por ejemplo, si la ruta de acceso de la raíz de la solución de Edge es "./ edgesolution", la **Ruta de acceso para publicar** debe ser "./edgesolution/config/deployment.json". El archivo `deployment.json` se genera durante el tiempo de compilación, por lo que es seguro pasar por alto las líneas de error rojas en el cuadro de texto. 

    ![Publicar artefacto](./media/how-to-ci-cd/publish-build-artifacts.png)

1. Abra la pestaña **Desencadenadores** y active el desencadenador **Integración continua**. Asegúrese de que se incluye la rama que contiene el código.

    ![Configuración del desencadenador](./media/how-to-ci-cd/configure-trigger.png)

    Guarde la nueva canalización de compilación. Haga clic en el botón **Save** (Guardar).


## <a name="configure-azure-pipelines-for-continuous-deployment"></a>Configuración de Azure Pipelines para implementación continua
En esta sección, creará una canalización de versión que está configurada para ejecutarse automáticamente cuando la canalización de compilación coloca artefactos, y mostrará los registros de implementación en Azure Pipelines.

1. En la pestaña **Versiones**, elija **+ Nueva canalización**. O bien, si ya tiene canalizaciones de versión, elija el botón **+ Nuevo**.  

    ![Agregar canalización de versión](./media/how-to-ci-cd/add-release-pipeline.png)

    En la ventana **Seleccione una plantilla** , elija **empiece por un Fase vacía**.

    ![Empezar por una fase vacía](./media/how-to-ci-cd/start-with-empty-job.png)

2. A continuación, la canalización de versión se inicializaría con una fase: **Fase 1**. Cambie el nombre de **Fase 1** a **QA** y trátela como un entorno de prueba. En una canalización de implementación continua típica, suelen existir varias fases, puede crear más en función de sus prácticas de DevOps.

    ![Crear fase](./media/how-to-ci-cd/QA-env.png)

3. Vincule la versión a los artefactos de compilación. Haga clic en **Agregar** en el área de artefactos.

    ![Agregar artefactos](./media/how-to-ci-cd/add-artifacts.png)  
    
    En la página **Agregar un artefacto**, elija el tipo de origen **Compilar**. A continuación, seleccione el proyecto y la canalización de compilación que ha creado. A continuación, haga clic en **Agregar**.

    ![Agregar un artefacto](./media/how-to-ci-cd/add-an-artifact.png)

    Abra el desencadenador de implementación continua para que se cree una versión nueva cada vez que haya disponible una nueva compilación.

    ![Configuración del desencadenador](./media/how-to-ci-cd/add-a-trigger.png)

4. Vaya a la **fase QA** y configure las tareas en esta fase.

    ![Configurar las tareas de QA](./media/how-to-ci-cd/view-stage-tasks.png)

   La tarea de implementación no distingue la plataforma, lo que significa que puede elegir **Hosted VS2017** o **Hosted Ubuntu 1604** en el **Grupo de agentes** (o cualquier otro agente administrado por usted mismo). Haga clic en "+" y agregue una tarea.

    ![Agregar tareas para QA](./media/how-to-ci-cd/add-task-qa.png)

5. En la tarea Azure IoT Edge, vaya a la lista desplegable **Acción**, seleccione **Deploy to IoT Edge devices** (Implementar en dispositivos IoT Edge). Seleccione la **suscripción de Azure** y escriba el **nombre de IoT Hub**. Puede especificar un **identificador de implementación** de IoT Edge y la **prioridad** de implementación. También puede elegir realizar la implementación en uno o varios dispositivos. En va a implementar en **varios dispositivos**, debe especificar la **condición del destino** del dispositivo. La condición de destino es un filtro para asociar un conjunto de dispositivos de Edge en IoT Hub. Si quiere usar etiquetas de dispositivo como condición, debe actualizar las etiquetas de dispositivo correspondientes con el dispositivo gemelo de IoT Hub. Suponga que tiene varios dispositivos de IoT Edge etiquetados como "qa", la configuración de la tarea debe ser como se muestra en la captura de pantalla siguiente. 

    ![Implementar en QA](./media/how-to-ci-cd/deploy-to-qa.png)

    Guarde la nueva canalización de versión. Haga clic en el botón **Save** (Guardar). Y, luego, haga clic en **Canalización** para volver a la canalización.

6. La segunda fase es para su entorno de producción. Para agregar una nueva fase "PROD", simplemente clone la fase "QA" y cambie el nombre de la fase clonada a **PROD**.

    ![Clonar fase](./media/how-to-ci-cd/clone-stage.png)

7. Configure las tareas para su entorno de producción. Suponga que tiene varios dispositivos de IoT Edge etiquetados como "prod" en las configuraciones de la tarea, actualice la condición de destino "prod" y establezca el identificador de implementación en "deploy-prod". Haga clic en el botón **Save** (Guardar). Y, luego, haga clic en **Canalización** para volver a la canalización.
    
    ![Implementar en producción](./media/how-to-ci-cd/deploy-to-prod.png)

7. Actualmente, el artefacto de compilación se desencadenará continuamente en la fase **QA** y, luego, en la fase **PROD**. Pero la mayoría de las veces tendrá que integrar algunos casos de prueba en los dispositivos de QA y aprobar manualmente los bits. Más adelante los bits se implementarán en el entorno de PROD. Configure una aprobación en la fase PROD como sigue.

    1. Abra el panel de configuración **Condiciones anteriores a la implementación**.

        ![Abrir Condiciones anteriores a la implementación](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Establezca **Habilitado** en **Aprobaciones anteriores a la implementación**. Y rellene la entrada **Aprobadores**. A continuación, haga clic en **Guardar**.
    
        ![Establecer las condiciones](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


8. Ahora la canalización de versión se ha establecido como sigue.

    ![Canalización de versión](./media/how-to-ci-cd/release-pipeline.png)

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Comprobación de CI/CD de IoT Edge con las canalizaciones de compilación y de versión

En esta sección, desencadenará una compilación para hacer funcionar la canalización de CI/CD. Luego verifique que la implementación se complete correctamente.

1. Para desencadenar un trabajo de compilación, puede insertar una confirmación en el repositorio de código fuente o desencadenarlo manualmente. Para desencadenar un trabajo de compilación en la canalización de compilación puede hacer clic en el botón **Cola**, como en la siguiente captura de pantalla.

    ![Desencadenador manual](./media/how-to-ci-cd/manual-trigger.png)

2. Si la canalización de compilación se completa correctamente, desencadenará una versión a la fase **QA**. Desplácese a los registros de canalización de compilación y verá lo siguiente.

    ![Registros de compilación](./media/how-to-ci-cd/build-logs.png)

3. La implementación correcta en la fase **QA** desencadenará una notificación al aprobador. Desplácese a la canalización de versión, donde puede ver lo siguiente. 

    ![Pendiente de aprobación](./media/how-to-ci-cd/pending-approval.png)


4. Después de que el aprobador aprueba este cambio, se puede implementar en **PROD**.

    ![Implementar en PROD](./media/how-to-ci-cd/approve-and-deploy-to-prod.png)


## <a name="next-steps"></a>Pasos siguientes

* Puede encontrar información sobre la implementación de IoT Edge en [Descripción de las implementaciones de IoT Edge en un único dispositivo o a escala](module-deployment-monitoring.md).
* Siga los pasos para crear, actualizar o eliminar una implementación en [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-monitor.md).
