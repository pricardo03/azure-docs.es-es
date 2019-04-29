---
title: 'Integración continua e implementación continua: Azure IoT Edge | Microsoft Docs'
description: 'Configuración de integración continua e implementación continua: Azure IoT Edge con Azure DevOps, Azure Pipelines'
author: shizn
manager: philmea
ms.author: xshi
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: f449449c542ce6ac04daa58ff37a3577f0d75aee
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61222058"
---
# <a name="continuous-integration-and-continuous-deployment-to-azure-iot-edge"></a>Integración continua e implementación continua en Azure IoT Edge

Puede adoptar fácilmente DevOps con las aplicaciones de Azure IoT Edge con las tareas integradas de Azure IoT Edge en Azure Pipelines. En este artículo se muestra cómo puede usar las características de integración continua e implementación continua de Azure Pipelines para compilar, probar e implementar aplicaciones de forma rápida y eficaz en su instancia de Azure IoT Edge. 

En este artículo, aprenderá a usar las tareas integradas de Azure IoT Edge para Azure Pipelines para crear dos canalizaciones para la solución de IoT Edge. La primera toma el código y compila la solución, insertando las imágenes del módulo en el registro de contenedores y creando un manifiesto de implementación. La segunda implementa los módulos en dispositivos de IoT Edge de destino.  

![Diagrama: ramas CI y CD para desarrollo y producción](./media/how-to-ci-cd/cd.png)


## <a name="prerequisites"></a>Requisitos previos

* Un repositorio de Azure Repos. Si no tiene uno, puede [crear un nuevo repositorio de Git en el proyecto](https://docs.microsoft.com/azure/devops/repos/git/create-new-repo?view=vsts&tabs=new-nav).
* Una solución de IoT Edge confirmada e insertada en el repositorio. Si quiere crear una nueva solución de ejemplo para hacer pruebas según este artículo, siga los pasos para [desarrollar y depurar módulos en Visual Studio Code](how-to-vs-code-develop-module.md) o [desarrollar y depurar módulos de C# en Visual Studio](how-to-visual-studio-develop-csharp-module.md).
   * En este artículo, todo lo que necesita es la carpeta de la solución creada mediante las plantillas de IoT Edge en Visual Studio Code o Visual Studio. No es necesario compilar, insertar, implementar ni depurar este código antes de continuar. Configurará esos procesos en Azure Pipelines. 
   * Si está creando una nueva solución, clone el repositorio localmente en primer lugar. A continuación, cuando cree la solución, puede elegir crearlo directamente en la carpeta del repositorio. Puede confirmar e insertar fácilmente los nuevos archivos desde allí. 
* Un registro de contenedor donde pueda insertar imágenes del módulo. Puede usar [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) o un registro de terceros. 
* Un [centro de IoT](../iot-hub/iot-hub-create-through-portal.md) activo con al menos dispositivos de IoT Edge para hacer pruebas de las fases independientes de implementación de prueba y producción. Puede seguir los artículos de la guía de inicio rápido para crear un dispositivo IoT Edge en [Linux](quickstart-linux.md) o [Windows](quickstart.md)


Para más información sobre el uso Azure Repos, consulte [Share your code with Visual Studio and Azure Repos](https://docs.microsoft.com/azure/devops/repos/git/share-your-code-in-git-vs?view=vsts) (Compartir el código con Visual Studio y Azure Repos).

## <a name="configure-continuous-integration"></a>Configuración de la integración continua
En esta sección, creará una nueva canalización de compilación. Configure la canalización para que se ejecute automáticamente cuando se inserte algún cambio en la solución de IoT Edge de ejemplo y publique registros de compilación.

>[!NOTE]
>En este artículo se usa el diseñador visual de Azure DevOps. Antes de seguir los pasos descritos en esta sección, desactive la característica en vista previa para la nueva experiencia de creación de canalizaciones YAML. 
>1. En Azure DevOps, seleccione el icono del perfil y seleccione **Características de vista previa**.
>2. Desactive **Nueva experiencia de creación de canalizaciones de YAML**. 
>
>Para más información, consulte [Create a build pipeline](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav#create-a-build-pipeline) (Creación de una canalización de compilación).

1. Inicio de sesión en su organización de Azure DevOps (**https:\//dev.azure.com/{your organización} /**) y abra el proyecto que contiene el repositorio de la solución de IoT Edge.

   En este artículo, hemos creado un repositorio denominado **IoTEdgeRepo**. Ese repositorio contiene **IoTEdgeSolution**, que tiene el código de un módulo denominado **filtermodule**. 

   ![Abrir el proyecto de DevOps](./media/how-to-ci-cd/init-project.png)

2. Vaya a Azure Pipelines en el proyecto. Abra la pestaña **Compilaciones** y seleccione **Nueva canalización**. O bien, si ya tiene canalizaciones de compilación, seleccione el botón **Nuevo**. Luego elija **Nueva canalización de compilación**.

    ![Creación de una canalización de compilación](./media/how-to-ci-cd/add-new-build.png)

3. Siga las indicaciones para crear la canalización. 

   1. Proporcione la información de origen para la nueva canalización de compilación. Seleccione **GIT de Azure Repos** como origen y, luego, seleccione el proyecto, el repositorio y la rama donde está ubicado el código de la solución de IoT Edge. Después, seleccione **Continuar**. 

      ![Seleccione el origen de la canalización](./media/how-to-ci-cd/pipeline-source.png)

   2. Seleccione **Fase vacía** en lugar de una plantilla. 

      ![Empezar con un proceso vacío](./media/how-to-ci-cd/start-with-empty.png)

4. Una vez creada la canalización, irá al editor de canalizaciones. En la descripción de la canalización, elija el grupo de agentes correcto en función de la plataforma de destino: 
    
   * Elija si desea compilar los módulos en la plataforma amd64 para contenedores Linux, elija **Hosted Ubuntu 1604**.

   * Si quiere compilar los módulos en la plataforma amd64 para contenedores de Windows 1809, deberá [configurar un agente autohospedado en Windows](https://docs.microsoft.com/azure/devops/pipelines/agents/v2-windows?view=vsts).

   * Si quiere compilar los módulos en la plataforma arm32v7 para contenedores de Linux, deberá [configurar un agente autohospedado en Linux](https://blogs.msdn.microsoft.com/iotdev/2018/11/13/setup-azure-iot-edge-ci-cd-pipeline-with-arm-agent/).
    
     ![Configuración del grupo de agentes de compilación](./media/how-to-ci-cd/configure-env.png)

5. La canalización viene preconfigurada con un trabajo denominado **Trabajo del agente 1**. Seleccione el signo de más (**+**) para agregar tres tareas al trabajo: **Azure IoT Edge** dos veces, y **Publicar los artefactos de la compilación** una vez. (Mantenga el mouse sobre el nombre de cada tarea para ver el botón **Agregar**).

   ![Agregar la tarea Azure IoT Edge](./media/how-to-ci-cd/add-iot-edge-task.png)

   Cuando se hayan agregado las tres tareas, el Trabajo del agente se verá similar al ejemplo siguiente:
    
   ![Las tres tareas en la canalización de compilación](./media/how-to-ci-cd/add-tasks.png)

6. Seleccione la primera tarea **Azure IoT Edge** para editarla. Esta tarea crea todos los módulos en la solución con la plataforma de destino que especifique, también genera el archivo **deployment.json** que indica a sus dispositivos de IoT Edge cómo configurar la implementación.

   * **Nombre para mostrar**: Acepte el valor predeterminado **Azure IoT Edge - Build module images**.
   * **Acción**: Acepte el valor predeterminado **Build module images**. 
   * **Archivo .template.json**: Seleccione los puntos suspensivos (**...**) y navegue hasta el archivo **deployment.template.json** en el repositorio que contiene la solución de IoT Edge. 
   * **Plataforma predeterminada**: Seleccione la plataforma adecuada para los módulos en función de su dispositivo de IoT Edge de destino. 
   * **Variables de salida**: Las variables de salida incluyen un nombre de referencia que puede usar para configurar la ruta de acceso del archivo donde se generará el archivo deployment.json. Establezca el nombre de referencia en algo fácil de recordar, como **edge**. 

7. Seleccione la segunda tarea **Azure IoT Edge** para editarla. Esta tarea insertará todas las imágenes del módulo en el registro de contenedor que seleccionó. También agrega las credenciales del registro de contenedor al archivo **deployment.json** para que el dispositivo de IoT Edge puede acceder a las imágenes del módulo. 

   * **Nombre para mostrar**: El nombre para mostrar se actualiza automáticamente cuando cambia el campo de acción. 
   * **Acción**: Use la lista desplegable para seleccionar **Push module images**. 
   * **Tipo de registro de contenedor**: Seleccione el tipo de registro de contenedor que usa para almacenar las imágenes del módulo. Dependiendo del tipo de registro que elija, el formulario cambia. Si elige **Azure Container Registry**, use las listas desplegables para seleccionar la suscripción de Azure y el nombre del registro de contenedor. Si elige **Generic Container Registry** (Registro de contenedor genérico), seleccione **Nuevo** para crear una conexión del servicio del registro. 
   * **Archivo .template.json**: Seleccione los puntos suspensivos (**...**) y navegue hasta el archivo **deployment.template.json** en el repositorio que contiene la solución de IoT Edge. 
   * **Plataforma predeterminada**: Seleccione la misma plataforma que las imágenes del módulo compilado.

   Si tiene varias instancias de Container Registry para hospedar las imágenes de módulo, tiene que duplicar esta tarea, seleccionar otra instancia de Container Registry y usar **Bypass module(s)** (Omitir módulos) en la configuración avanzada para omitir las imágenes que no son de este registro específico.

8. Seleccione la tarea **Publicar los artefactos de la compilación** para editarla. Proporcione la ruta de acceso al archivo de implementación generado por la tarea de compilación. Establezca el valor **Ruta de acceso para publicar** para que coincida con la variable de salida que estableció en la tarea del módulo de compilación. Por ejemplo, `$(edge.DEPLOYMENT_FILE_PATH)`. Deje las restantes opciones con sus valores predeterminados. 

9. Abra la pestaña **Desencadenadores** y marque la casilla para **Habilitar la integración continua**. Asegúrese de que se incluye la rama que contiene el código.

    ![Activación del desencadenador de integración continua](./media/how-to-ci-cd/configure-trigger.png)

10. Guarde la nueva canalización de compilación con el botón **Guardar**.

Esta canalización ahora está configurada para ejecutarse automáticamente al insertar código nuevo en el repositorio. La última tarea, la publicación de los artefactos de la canalización, desencadena una canalización de versión. Continúe con la sección siguiente para compilar la canalización de versión. 

## <a name="configure-continuous-deployment"></a>Configuración de la implementación continua
En esta sección, se crea una canalización de versión que está configurada para ejecutarse automáticamente cuando la canalización de compilación coloca artefactos, y mostrará los registros de implementación en Azure Pipelines.

En esta sección, creará dos fases diferentes, una para las implementaciones de prueba y otra para las implementaciones de producción. 

### <a name="create-test-stage"></a>Creación de la fase de prueba

Cree una nueva canalización y configure su primera fase para las implementaciones de control de calidad (QA). 

1. En la pestaña **Versiones**, elija **+ Nueva canalización**. O bien, si ya tiene canalizaciones de versión, elija el botón **+ Nuevo** y seleccione **+ Nueva canalización de versión**.  

    ![Agregar canalización de versión](./media/how-to-ci-cd/add-release-pipeline.png)

2. Cuando se le pida que seleccione una plantilla, elija empezar con una **Fase vacía**.

    ![Empezar con un trabajo vacío](./media/how-to-ci-cd/start-with-empty-job.png)

3. La nueva canalización de versión se inicializa con una fase, llamada **Fase 1**. Cambie el nombre de Fase 1 a **QA** y trátela como un entorno de prueba. Por lo general, las canalizaciones de implementación continua tienen varias fases. Puede crear más en función de sus prácticas de DevOps. Cierre la ventana de detalles de la fase una vez que haya cambiado el nombre. 

    ![Creación de la fase del entorno de prueba](./media/how-to-ci-cd/QA-env.png)

4. Vincule la versión con los artefactos de compilación que publica la canalización de compilación. Haga clic en **Agregar** en el área de artefactos.

   ![Agregar artefactos](./media/how-to-ci-cd/add-artifacts.png)  
    
5. En la página **Agregar un artefacto**, seleccione el tipo de origen **Compilar**. A continuación, seleccione el proyecto y la canalización de compilación que ha creado. Después, seleccione **Agregar**.

   ![Adición de un artefacto de compilación](./media/how-to-ci-cd/add-an-artifact.png)

6. Abra los desencadenadores de artefacto y seleccione el botón de alternancia para habilitar el desencadenador de implementación continua. Ahora, se creará una nueva versión cada vez que haya disponible una nueva compilación.

   ![Configuración del desencadenador de implementación continua](./media/how-to-ci-cd/add-a-trigger.png)

7. La fase **QA** está preconfigurada con un trabajo y cero tareas. En el menú de la canalización, seleccione **Tareas**, a continuación, elija la fase **QA**.  Seleccione el recuento de trabajos y tareas para configurar las tareas en esta fase.

    ![Configurar las tareas de QA](./media/how-to-ci-cd/view-stage-tasks.png)

8. En la fase QA, debería ver un **Trabajo del agente** predeterminado. Puede configurar los detalles sobre el trabajo del agente, pero la tarea de implementación no distingue la plataforma, por lo que puede elegir **Hosted VS2017** o **Hosted Ubuntu 1604** en el **Grupo de agentes** (o cualquier otro agente administrado por usted mismo). 

9. Haga clic en el signo de más (**+**) para agregar una tarea. Busque y agregue **Azure IoT Edge**. 

    ![Agregar tareas para QA](./media/how-to-ci-cd/add-task-qa.png)

10. Seleccione la nueva tarea de Azure IoT Edge y configúrela con los valores siguientes:

    * **Nombre para mostrar**: El nombre para mostrar se actualiza automáticamente cuando cambia el campo de acción. 
    * **Acción**: Use la lista desplegable para seleccionar **Deploy to IoT Edge device** (Implementar en dispositivo IoT Edge). Al cambiar el valor de la acción, también se actualiza el nombre para mostrar de la tarea para que coincidan.
    * **Suscripción de Azure**: Seleccione la suscripción que contiene el centro de IoT.
    * **Nombre de la instancia de IoT Hub**: Seleccione IoT Hub. 
    * **Choose single/multiple device** (Elegir dispositivo único/varios): Elija si quiere que la canalización de versión se implemente en uno o varios dispositivos. 
      * Si implementa en un único dispositivo, escriba el **IoT Edge device ID** (Id. de dispositivo IoT Edge). 
      * Si va a implementar en varios dispositivos, especifique la **condición de destino** del dispositivo. La condición de destino es un filtro para asociar un conjunto de dispositivos de Edge en IoT Hub. Si quiere usar etiquetas de dispositivo como condición, debe actualizar las etiquetas de dispositivo correspondientes con el dispositivo gemelo de IoT Hub. Actualice **IoT Edge deployment ID** (Id. de implementación de IoT Edge) e **IoT Edge deployment priority** (Prioridad de implementación de IoT Edge) en la configuración avanzada. Para más información acerca de cómo crear una implementación para varios dispositivos, consulte [Descripción de las implementaciones automáticas de IoT Edge](module-deployment-monitoring.md).

11. Seleccione **Guardar** para guardar los cambios en la nueva canalización de versión. Vuelva a la vista de canalización seleccionando **Canalización** en el menú. 

### <a name="create-production-stage"></a>Creación de una fase de producción

Cree una segunda fase en la canalización de versión para la implementación de producción. 

1. Cree una segunda fase de producción clonando la fase QA. Mantenga el cursor sobre la fase QA y luego seleccione el botón Clonar. 

    ![Clonar fase](./media/how-to-ci-cd/clone-stage.png)

2. Seleccione la nueva fase, llamada **Copia de QA** para abrir sus propiedades. Cambie el nombre de la fase a **PROD**, para producción. Cierre la ventana de propiedades de la fase. 

3. Para abrir las tareas de la fase PROD, seleccione **Tareas** en el menú de la canalización y, luego, elija la fase **PROD**. 

4. Seleccione la tarea de Azure IoT Edge para configurar si es para el entorno de producción. Probablemente, la configuración de implementación sea la misma para QA que para PROD, salvo que quiere establecer como destino un dispositivo o un conjunto de dispositivos diferentes en producción. Actualice el campo de Id. de dispositivo, o los campos de condición de destino e id. de implementación para los dispositivos de producción. 

5. Guárdelo con el botón **Guardar**. Luego, seleccione **Canalización** para volver a la vista de canalización.
    
6. De la manera en que está configurada actualmente esta canalización de versión, el artefacto de compilación desencadenará la fase **QA** y, luego, la fase **PROD** cada vez que se complete una compilación nueva. Sin embargo, normalmente querrá integrar algunos casos de prueba en los dispositivos de QA y aprobar manualmente la implementación para producción. Siga los pasos a continuación para crear una condición de aprobación para la fase PROD:

    1. Abra el panel de configuración **Condiciones anteriores a la implementación**.

        ![Abrir Condiciones anteriores a la implementación](./media/how-to-ci-cd/pre-deploy-conditions.png)    

    2. Cambie la condición **Aprobaciones anteriores a la implementación** a **Habilitado**. Agregue uno o varios usuarios o grupos en el campo **Aprobadores** y personalice las demás directivas de aprobación que quiera. Para guardar los cambios, cierre el panel condiciones anteriores a la implementación.
    
       ![Establecer las condiciones](./media/how-to-ci-cd/set-pre-deployment-conditions.png)


7. Guarde la canalización de versión con el botón **Guardar**. 

    
## <a name="verify-iot-edge-cicd-with-the-build-and-release-pipelines"></a>Comprobación de CI/CD de IoT Edge con las canalizaciones de compilación y de versión

Para desencadenar un trabajo de compilación, puede insertar una confirmación en el repositorio de código fuente o desencadenarlo manualmente. En esta sección, desencadenará manualmente la canalización de CI/CD para probar que funcione. Luego verifique que la implementación se complete correctamente.

1. Vaya a la canalización de compilación que creó al principio de este artículo. 

2. Para desencadenar un trabajo de compilación en la canalización de compilación puede seleccionar el botón **Cola**, como en la siguiente captura de pantalla.

    ![Desencadenador manual](./media/how-to-ci-cd/manual-trigger.png)

3. Seleccione el trabajo de compilación para ver su progreso. Si la canalización de compilación se completa correctamente, desencadena una versión a la fase **QA**. 

    ![Registros de compilación](./media/how-to-ci-cd/build-logs.png)

4. La implementación correcta en la fase **QA** desencadena una notificación al aprobador. Verifique que los módulos se hayan implementado correctamente en el o los dispositivos de destino con la fase QA. A continuación, vaya a la canalización de versión y apruebe que la versión pase a la fase PROD seleccionando el botón **PROD** y, luego, seleccionando **Aprobar**. 

    ![Pendiente de aprobación](./media/how-to-ci-cd/pending-approval.png)

5. Después de que el aprobador aprueba este cambio, se puede implementar en **PROD**.

## <a name="next-steps"></a>Pasos siguientes

* Puede encontrar información sobre la implementación de IoT Edge en [Descripción de las implementaciones de IoT Edge en un único dispositivo o a escala](module-deployment-monitoring.md).
* Siga los pasos para crear, actualizar o eliminar una implementación en [Implementación y supervisión de módulos de IoT Edge a escala](how-to-deploy-monitor.md).
