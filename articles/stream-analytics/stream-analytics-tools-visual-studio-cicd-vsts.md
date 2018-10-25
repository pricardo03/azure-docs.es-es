---
title: Tutorial de implementación de un trabajo de Azure Stream Analytics con CI/CD mediante Azure DevOps Services
description: En este artículo se describe cómo implementar un trabajo de Stream Analytics con CI/CD mediante Azure DevOps Services.
services: stream-analytics
author: su-jie
ms.author: sujie
manager: kfile
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: tutorial
ms.date: 07/10/2018
ms.openlocfilehash: 0f729725a04b19a513ca92953e997b51e4558884
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49986272"
---
# <a name="tutorial-deploy-an-azure-stream-analytics-job-with-cicd-using-azure-pipelines"></a>Tutorial: implementación de un trabajo de Azure Stream Analytics con CI/CD mediante Azure Pipelines
En este tutorial se describe cómo configurar la integración y la implementación continuas de un trabajo de Azure Stream Analytics mediante Azure Pipelines. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar un control de código fuente al proyecto
> * Crear una canalización de compilación en Azure Pipelines
> * Crear una canalización de versión en Azure Pipelines
> * Implementar y actualizar una aplicación automáticamente

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar, asegúrese de que dispone de lo siguiente:

* Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* Instale [Visual Studio](stream-analytics-tools-for-visual-studio-install.md) y las cargas de trabajo de **desarrollo de Azure** o de **almacenamiento y procesamiento de datos**.
* Cree un [proyecto de Stream Analytics en Visual Studio](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-quick-create-vs).
* Cree una organización de [Azure DevOps](https://visualstudio.microsoft.com/team-services/).

## <a name="configure-nuget-package-dependency"></a>Configuración de dependencias de paquetes NuGet
Para realizar la compilación y la implementación automáticas en una máquina arbitraria, deberá usar el paquete NuGet `Microsoft.Azure.StreamAnalytics.CICD`. Proporciona las herramientas de implementación, ejecución local y MSBuild que admite la integración continua y el proceso de implementación de los proyectos Stream Analytics para Visual Studio. Para más información, consulte las [herramientas de CI/CD de Stream Analytics](stream-analytics-tools-for-visual-studio-cicd.md).

Agregue **packages.config** al directorio del proyecto.

```xml
<?xml version="1.0" encoding="utf-8"?>
<packages>
<package id="Microsoft.Azure.StreamAnalytics.CICD" version="1.0.0" targetFramework="net452" />
</packages>
```

## <a name="share-your-visual-studio-solution-to-a-new-azure-repos-git-repo"></a>Solución de Visual Studio compartida en un nuevo repositorio Git de Azure Repos

Comparta sus archivos de origen de la aplicación en un proyecto de Azure DevOps para generar compilaciones.  

1. Cree un repositorio de Git local para el proyecto; para ello, seleccione **Add to Source Control** (Agregar al control de código fuente) y luego **Git** en la barra de estado de la esquina inferior derecha de Visual Studio. 

2. En la vista **Sincronización** de **Team Explorer**, seleccione el botón **Publicar repositorio Git** en **Insertar en Azure DevOps Services**.

   ![Inserción en el repositorio de Git](./media/stream-analytics-tools-visual-studio-cicd-vsts/publishgitrepo.png)

3. Verifique su correo electrónico y seleccione su organización en la lista desplegable **Azure DevOps Services Domain** (Dominio de Azure DevOps Services). Escriba el nombre del repositorio y seleccione **Publicar repositorio**.

   ![Repositorio Git de inserción](./media/stream-analytics-tools-visual-studio-cicd-vsts/publishcode.png)

    La publicación del repositorio crea un proyecto en su organización con el mismo nombre que el repositorio local. Para crear el repositorio en un proyecto existente, haga clic en **Avanzado**, junto al nombre del **repositorio**, y seleccione un proyecto. Para ver el código en el explorador, seleccione **See it on the web** (Verlo en la Web).
 
## <a name="configure-continuous-delivery-with-azure-devops"></a>Configuración de la entrega continua con Azure DevOps
Una canalización de compilación de Azure Pipelines describe un flujo de trabajo compuesto por pasos de compilación que se ejecutan de manera secuencial. Más información sobre las [canalizaciones de compilación de Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/get-started-designer?view=vsts&tabs=new-nav). 

Una canalización de versión de Azure Pipelines describe un flujo de trabajo que implementa un paquete de aplicación en un clúster. Cuando se usan juntas, la canalización de compilación y la de versión ejecutan el flujo de trabajo completo empezando por los archivos de origen y terminando por una aplicación en el clúster. Más información sobre las [canalizaciones de versión de Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts).

### <a name="create-a-build-pipeline"></a>Creación de una canalización de compilación
Abra un explorador web y vaya hasta el proyecto que acaba de crear en [Azure DevOps](https://app.vsaex.visualstudio.com/). 

1. En la pestaña **Build & Release** (Compilación y versión), seleccione **Compilaciones** y, luego, **+Nuevo**.  Seleccione **Azure DevOps Services Git** (Git de Azure DevOps Services) y **Continuar**.
    
    ![Seleccionar origen](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-select-source.png)

2. En **Seleccionar una plantilla**, haga clic en **Proceso vacío** para comenzar con una canalización vacía.
    
    ![Elegir la plantilla de compilación](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-select-template.png)

3. En **Desencadenadores**, active el estado del desencadenador **Habilitar la integración continua** para habilitar la integración continua.  Seleccione **Guardar y poner en cola** para iniciar manualmente una compilación. 
    
    ![Estado del desencadenador](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-trigger.png)

4. Las compilaciones se desencadenan también tras el envío de cambios o la inserción en el repositorio. Para comprobar el progreso de la compilación, vaya a la pestaña **Compilaciones**.  Cuando haya verificado que la compilación se ejecuta correctamente, debe determinar una canalización de versión que implemente la aplicación en un clúster. Haga clic con el botón derecho en el botón de puntos suspensivos situado junto a la canalización de compilación y seleccione **Editar**.

5.  En **Tareas**, escriba "Hospedado" como **Cola de agentes**.
    
    ![Selección de la cola de agentes](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-agent-queue.png) 

6. En **Phase 1** (Fase 1), haga clic en **+** y agregue una tarea de **NuGet**.
    
    ![Adición de una tarea de NuGet](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-nuget.png)

7. Expanda **Avanzado** y agregue `$(Build.SourcesDirectory)\packages` al **directorio de destino**. Mantenga el resto de valores de configuración predeterminados de NuGet.

   ![Configuración de la tarea de NuGet](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-nuget-config.png)

8. En **Phase 1** (Fase 1), haga clic en **+** y agregue una tarea de **MSBuild**.

   ![Adición de una tarea de MSBuild](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-msbuild-task.png)

9. Cambie los **argumentos de MSBuild** por lo siguiente:

   ```
   /p:CompilerTaskAssemblyFile="Microsoft.WindowsAzure.StreamAnalytics.Common.CompileService.dll"  /p:ASATargetsFilePath="$(Build.SourcesDirectory)\packages\Microsoft.Azure.StreamAnalytics.CICD.1.0.0\build\StreamAnalytics.targets"
   ```

   ![Configuración de la tarea de MSBuild](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-msbuild.png)

10. En **Phase 1** (Fase 1), haga clic en **+** y agregue una tarea de **implementación de un grupo de recursos de Azure**. 
    
    ![Adición de una tarea de implementación de un grupo de recursos de Azure](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-deploy.png)

11. Expanda **Detalles de Azure** y rellene la configuración con los siguientes valores:
    
    |**Configuración**  |**Valor sugerido**  |
    |---------|---------|
    |Subscription  |  Elija su suscripción.   |
    |.  |  Creación o actualización del grupo de recursos   |
    |Grupo de recursos  |  Escriba el nombre de un grupo de recursos.   |
    |Plantilla  | [La ruta de acceso a la solución]\bin\Debug\Deploy\\[El nombre del proyecto].JobTemplate.json   |
    |Parámetros de plantilla  | [La ruta de acceso a la solución]\bin\Debug\Deploy\\[El nombre del proyecto].JobTemplate.parameters.json   |
    |Reemplazo de los parámetros de plantilla  | Escriba los parámetros de plantilla para reemplazar en el cuadro de texto. Ejemplo: –storageName fabrikam –adminUsername $(vmusername) -adminPassword $(password) –azureKeyVaultName $(fabrikamFibre). Esta propiedad es opcional, pero la compilación generará errores si no se reemplazan los parámetros de clave.    |
    
    ![Establecimiento de las propiedades](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-deploy-2.png)

12. Haga clic en **Guardar y poner en cola** para probar la canalización de compilación.
    
    ![Establecimiento de los parámetros de reemplazo](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-save-queue.png)

### <a name="failed-build-process"></a>Error en el proceso de compilación
Si no reemplazó los parámetros de plantilla en la tarea de **implementación de un grupo de recursos de Azure** de la canalización de compilación, puede que reciba errores de los parámetros de implementación nulos. Vuelva a la canalización de compilación y reemplace los parámetros nulos para resolver el error.

   ![Error en el proceso de compilación](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-process-failed.png)

### <a name="commit-and-push-changes-to-trigger-a-release"></a>Confirmación y envío de cambios para desencadenar una versión
Para verificar que la canalización de integración continua funciona, inserte en el repositorio algunos cambios de código para Azure DevOps.    

A medida que escribe el código, los cambios se controlan automáticamente con Visual Studio. Para confirmar los cambios en el repositorio de Git local, seleccione el icono de cambios pendientes en la barra de estado de la parte inferior derecha.

1. En la vista **Cambios** de Team Explorer, agregue un mensaje que describa la actualización y confirme los cambios.

    ![Confirmación y envío de cambios](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-push-changes.png)

2. Seleccione el icono de la barra de estado de cambios no publicados o la vista de sincronización de Team Explorer. Seleccione **Insertar** para actualizar el código en Azure DevOps.

    ![Confirmación y envío de cambios](./media/stream-analytics-tools-visual-studio-cicd-vsts/build-push-changes-2.png)

La inserción de los cambios en Azure DevOps Services desencadena automáticamente una compilación.  Cuando la canalización de compilación se ha completado correctamente, se crea automáticamente una versión y empieza a actualizarse el trabajo en el clúster.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no los necesite, elimine el grupo de recursos, el trabajo de streaming y todos los recursos relacionados. La eliminación del trabajo evita la facturación de las unidades de streaming utilizadas por el trabajo. Si piensa utilizar el trabajo en el futuro, puede detenerlo y volver a iniciarlo más adelante cuando sea necesario. Si no va a seguir usando este trabajo, siga estos pasos para eliminar todos los recursos creados en este tutorial:

1. En el menú de la izquierda de Azure Portal, haga clic en **Grupos de recursos** y en el nombre del recurso que creó.  
2. En la página del grupo de recursos, haga clic en **Eliminar**, escriba en el cuadro de texto el nombre del recurso que quiere eliminar y haga clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Para aprender más sobre el uso de herramientas de Azure Stream Analytics para Visual Studio para configurar un proceso de integración e implementación continuas, continúe con el artículo para configurar la canalización de CI/CD:

> [!div class="nextstepaction"]
> [Integración y desarrollo continuos con las herramientas de Stream Analytics](stream-analytics-tools-for-visual-studio-cicd.md)
