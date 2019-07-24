---
title: Uso del centro de implementación de Azure App Service
description: En Azure DevOps, el centro de implementación simplifica la configuración de una canalización de implementación sólida de Azure DevOps para su aplicación
ms.author: puagarw
ms.manager: douge
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/12/2019
author: pulkitaggarwl
monikerRange: vsts
ms.openlocfilehash: 8d1e467906b74c97c8b4f4e5c14af0814dd098f7
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67861519"
---
# <a name="deployment-center-launcher"></a>Iniciador del centro de implementación

En Azure DevOps, el centro de implementación simplifica la configuración de una canalización de implementación sólida de DevOps para su aplicación. De forma predeterminada, configura una canalización DevOps para implementar las actualizaciones de la aplicación en el clúster de Kubernetes. Puede extender la canalización de DevOps configurada predeterminada y agregar las funcionalidades de DevOps (aprobaciones previas a la implementación, aprovisionamiento de recursos de Azure adicionales, ejecución de scripts, actualización de la aplicación o incluso ejecución de pruebas de validación adicionales).

En este tutorial, aprenderá lo siguiente:

> [!div class="checklist"]
> * A configurar una canalización DevOps para implementar las actualizaciones de la aplicación en el clúster de k8s
> * Examen de la canalización de CI
> * Examen de la canalización de CD
> * A limpiar los recursos

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Puede obtener una de forma gratuita mediante [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/)

* Un clúster de Azure Kubernetes Service (AKS)

## <a name="create-aks-cluster"></a>Creación de un clúster de AKS

1. Inicie sesión en [Azure Portal](https://portal.azure.com/)

1. Seleccione el botón [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) en el menú de la esquina superior derecha de Azure Portal.

1. Para crear el clúster de AKS, ejecute los siguientes comandos.

    ```cmd
    # The below command creates Resource Group in the south india location

    az group create --name azooaks --location southindia

    # The below command creates a cluster named azookubectl with one node. 

    az aks create --resource-group azooaks --name azookubectl --node-count 1 --enable-addons monitoring --generate-ssh-keys
    ```

## <a name="deploy-application-updates-to-k8s-cluster"></a>Implementación de actualizaciones de la aplicación en el clúster de K8s

1. Vaya al grupo de recursos que ha creado anteriormente.

1. Seleccione el clúster de AKS y haga clic en la configuración de la hoja izquierda en **Centro de implementación (versión preliminar)** . Haga clic en **Introducción**.

   ![configuración](media/deployment-center-launcher/settings.png)

1. Elija la ubicación del código y haga clic en **Siguiente**. Actualmente, los repositorios que se admiten son **,[Azure Repos ](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)** y **GitHub**. Puede seguir los pasos que se indican a continuación en función de la selección del repositorio.

    Azure Repos es un conjunto de herramientas de control de versiones que se puede usar para administrar el código. Independientemente de que el proyecto de software sea grande o pequeño, se recomienda usar el control de versiones lo antes posible.

    - **Azure Repos**: elija un repositorio en el proyecto y la organización existentes.

        ![Azure Repos](media/deployment-center-launcher/azure-repos.gif)

    - **GitHub**: Autorice y seleccione el repositorio para su cuenta de GitHub.

        ![GitHub](media/deployment-center-launcher/github.gif)


1. Vamos a analizar el repositorio y a detectar su Dockerfile. Si desea actualizarlo, puede editar el número de puerto identificado.

    ![Configuración de la aplicación](media/deployment-center-launcher/application-settings.png)

    Si el repositorio no contiene el Dockerfile, el sistema mostrará un mensaje para confirmar uno. 

    ![Dockerfile](media/deployment-center-launcher/dockerfile.png)

1. Seleccione o cree una instancia de Container Registry y haga clic en **Finalizar**. La canalización se creará automáticamente y pondrá en cola una compilación en [Azure Pipelines](https://docs.microsoft.com/azure/devops/pipelines/index?view=azure-devops).

    Azure Pipelines es un servicio en la nube que puede usar para compilar y probar automáticamente el proyecto del código y hacer que esté disponible para otros usuarios. Azure Pipelines combina la integración continua (CI) y la entrega continua (CD) para probar y compilar el código de forma constante y coherente, y enviarlo a cualquier destino.

    ![Container Registry](media/deployment-center-launcher/container-registry.png)

1. Haga clic en el vínculo para ver la canalización en curso.

1. Verá los registros correctos tal como se mostrarán una que la implementación se complete.

    ![Registros](media/deployment-center-launcher/logs.png)

## <a name="examine-the-ci-pipeline"></a>Examen de la canalización de CI

El centro de implementación configura automáticamente la canalización de CI/CD de su organización de Azure DevOps. Dicha canalización se puede explorar y personalizar. 

1. Vaya al panel del centro de implementación.  

1. Haga clic en el número de compilación de la lista de registros correctos para ver la canalización de compilación del proyecto. 

1. Haga clic en los puntos suspensivos (...) de la esquina superior derecha. Un menú muestra varias opciones, como poner en cola una nueva compilación, conservar una compilación y editar la canalización de compilación. Elija **Editar canalización**. 

1. En este panel puede examinar las distintas tareas de la canalización de compilación. La compilación realiza varias tareas, como recopilar los orígenes del repositorio de Git, crear una imagen, insertar una imagen en el registro de contenedor y publicar los resultados que se usan para las implementaciones.

1. Seleccione el nombre de la canalización de compilación en la parte superior de la misma.

1. Cambie el nombre de la canalización de compilación por otro más descriptivo, seleccione **Guardar y poner en cola** y, luego, seleccione **Guardar**.

1. Seleccione **Historial** en la canalización de compilación. En este panel se muestra una pista de auditoría de los últimos cambios en la compilación. Azure DevOps supervisa todos los cambios realizados en la canalización de compilación y permite comparar las versiones.

1. Elija **Desencadenadores**. Opcionalmente, las ramas se pueden incluir o excluir del proceso de integración continua.

1. Elija **Retención**. Puede especificar directivas para conservar o quitar un número determinado de compilaciones en función del escenario.

## <a name="examine-the-cd-pipeline"></a>Examen de la canalización de CD

El centro de implementación crea y configura automáticamente los pasos necesarios de su organización de Azure DevOps en su suscripción de Azure. Dichos pasos incluyen la configuración de una conexión de servicio de Azure para realizar la autenticación en su suscripción a Azure con Azure DevOps. La automatización también crea una canalización de versión, lo que proporciona la implementación continua en Azure.

1. Elija **Canalizaciones** y elija **Versiones**.

1. Para editar la canalización de la versión, haga clic en **Editar**.

1. Seleccione **Anular** en **Artifacts**. En los pasos anteriores, la canalización de la construcción que ha examinado genera la salida que se usa para el artefacto. 

1. Seleccione el desencadenador **Implementación continua**  a la derecha del icono **Anular**. Esta canalización de versión tiene un desencadenador de implementación continua habilitado que ejecuta una implementación cuando hay un nuevo artefacto de compilación disponible. También puede deshabilitar el desencadenador para exigir la ejecución manual de sus implementaciones.

1. Para examinar todas las tareas de la canalización, haga clic en **Tareas**. La versión establece el entorno de Tiller, configura imagePullSecrets, instala las herramientas de Helm e implementa los gráficos de Helm en el clúster de K8s.

1. Para ver el historial de versiones, haga clic en **Ver versiones**. 

1. Para ver el resumen, haga clic en **Versión**. Haga clic en cualquiera de las fases para explorar varios menús, como un resumen de las versiones, los elementos de trabajo asociados y las pruebas. 

1. Seleccione **Confirmaciones**. Esta vista muestra las confirmaciones de código relacionadas con esta implementación. Compare las versiones para ver las diferencias de confirmación entre las implementaciones.

1. Seleccione **Registros**. Los registros contienen información de implementación útil y se pueden ver tanto durante las implementaciones como después de ellas.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando no necesite los recursos relacionados que haya creado puede eliminarlos. Use la funcionalidad de eliminación del panel de DevOps Projects.

## <a name="next-steps"></a>Pasos siguientes

Si lo desea, puede modificar estas canalizaciones de compilación y de versión para satisfacer las necesidades de su equipo. También puede usar este patrón de CI/CD como plantilla para las demás canalizaciones. En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * A configurar una canalización DevOps para implementar las actualizaciones de la aplicación en el clúster de k8s
> * Examen de la canalización de CI
> * Examen de la canalización de CD
> * A limpiar los recursos
