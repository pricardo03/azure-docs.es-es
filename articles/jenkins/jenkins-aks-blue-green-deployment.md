---
title: Implementación en Azure Kubernetes Service (AKS) con Jenkins y el patrón de implementación azul/verde
description: Obtenga información sobre cómo realizar implementaciones en Azure Kubernetes Service (AKS) con Jenkins y el patrón de implementación azul/verde.
services: app-service\web
documentationcenter: ''
author: tomarcher
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 07/23/2018
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 472622f78303593b7a4d5d5136aa47f34a1f44b1
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39227932"
---
# <a name="deploy-to-azure-kubernetes-service-aks-using-jenkins-and-bluegreen-deployment-pattern"></a>Implementación en Azure Kubernetes Service (AKS) con Jenkins y el patrón de implementación azul/verde

Azure Kubernetes Service (AKS) permite administrar el entorno hospedado de Kubernetes, lo que hace que sea fácil y rápido implementar y administrar aplicaciones en contenedores sin necesidad de tener conocimientos de orquestación de contenedores. AKS también permite eliminar la carga de las operaciones en curso y las de mantenimiento mediante el aprovisionamiento, actualización y escalado de los recursos a petición, sin tener que desconectar las aplicaciones. Para más información sobre AKS, vea la [documentación de AKS](/azure/aks/).

La implementación azul/verde es un patrón de entrega continua (CD) que se basa en mantener una versión existente (azul) activa mientras se implementa una nueva (verde). Normalmente, este patrón emplea el equilibrio de carga para aumentar directamente las cantidades de tráfico para la implementación verde. Si la supervisión detecta un incidente, se puede redirigir el tráfico a la implementación azul que sigue en ejecución. Para obtener más información sobre la entrega continua, consulte el artículo [What is Continuous Delivery](/azure/devops/what-is-continuous-delivery) (Qué es la entrega continua).

En este tutorial, aprenderá a realizar las siguientes tareas al aprender cómo realizar implementaciones en AKS con Jenkins y el patrón de implementación azul/verde:

> [!div class="checklist"]
> * Obtener información sobre el patrón de implementación azul/verde
> * Crear un clúster de Kubernetes administrado
> * Ejecutar un script de ejemplo para configurar un clúster de Kubernetes
> * Configurar manualmente un clúster de Kubernetes
> * Crear y ejecutar un trabajo de Jenkins

## <a name="prerequisites"></a>Requisitos previos
- [Cuenta de GitHub](https://github.com): se necesita una cuenta de GitHub para clonar el repositorio de ejemplo.
- [CLI de Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest): se usa la CLI de Azure 2.0 para crear el clúster de Kubernetes.
- [Chocolatey](https://chocolatey.org): un administrador de paquetes que se usó para instalar kubectl.
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/): una interfaz de la línea de comandos para ejecutar comandos en clústeres de Kubernetes.
- [jq](https://stedolan.github.io/jq/download/): un procesador JSON ligero de línea de comandos.

## <a name="clone-the-sample-app-from-github"></a>Clonación de la aplicación de ejemplo desde GitHub

Se trata de una aplicación de ejemplo en la que se ilustra cómo implementar en AKS con Jenkins y el patrón azul/verde en el repositorio de Microsoft en GitHub. En esta sección, se crea una bifurcación del repositorio en su GitHub y se clona la aplicación en el sistema local.

1. Vaya al repositorio de GitHub para la aplicación de ejemplo [todo-app-java-on-azure](https://github.com/microsoft/todo-app-java-on-azure.git).

    ![Aplicación de ejemplo en el repositorio de GitHub de Microsoft.](./media/jenkins-aks-blue-green-deployment/github-sample-msft.png)

1. Bifurque el repositorio seleccionando **Bifurcación** en la esquina superior derecha de la página y siga las instrucciones para bifurcar el repositorio en su cuenta de GitHub.

    ![Bifurque la aplicación de ejemplo a su cuenta de GitHub.](./media/jenkins-aks-blue-green-deployment/github-sample-msft-fork.png)

1. Una vez que bifurque el repositorio, verá que cambia el nombre de cuenta del nombre de cuenta, y una nota indica desde dónde se bifurcó el repositorio (Microsoft).

    ![Aplicación de ejemplo tras la bifurcación a otra cuenta de GitHub.](./media/jenkins-aks-blue-green-deployment/github-sample-msft-forked.png)

1. Seleccione **Clone or download** (Clonar o descargar).

    ![GitHub permite clonar o descargar un repositorio rápidamente.](./media/jenkins-aks-blue-green-deployment/github-sample-clone.png)

1. En la ventana **Clone with HTTPS** (Clonar con HTTP), seleccione el icono de copia.

    ![Copie la dirección URL de clonación en el Portapapeles.](./media/jenkins-aks-blue-green-deployment/github-sample-copy.png)

1. Abra una ventana de Bash o terminal.

1. Cambie los directorios a la ubicación deseada en la que desea almacenar la copia local (clon) del repositorio.

1. Mediante el comando `git clone`, clone la dirección URL copiada anteriormente.

    ![Escriba "git clone" y la dirección URL de clonación para crear un clon del repositorio.](./media/jenkins-aks-blue-green-deployment/git-clone-command.png)

1. Seleccione la tecla &lt;ENTRAR> para iniciar el proceso de clonación.

    ![El comando "git clone" crea una copia personal del repositorio en el que puede probar.](./media/jenkins-aks-blue-green-deployment/git-clone-results.png)

1. Cambie los directorios al directorio recién creado que contiene el clon del origen de la aplicación.

## <a name="create-and-configure-a-managed-kubernetes-cluster"></a>Creación y configuración de un clúster de Kubernetes administrado

En esta sección se realizarán los pasos siguientes:

- Use la CLI de Azure 2.0 para crear un clúster de Kubernetes administrado.
- Obtenga información sobre cómo configurar un clúster mediante el script de configuración o manualmente.
- Cree la instancia de Azure Container Registry.

> [!NOTE]   
> AKS se encuentra actualmente en versión preliminar. Para obtener información sobre cómo habilitar la versión preliminar de la suscripción de Azure, vea [Guía de inicio rápido: Implementación de un clúster de Azure Kubernetes Service (AKS)](/azure/aks/kubernetes-walkthrough#enabling-aks-preview-for-your-azure-subscription).

### <a name="use-the-azure-cli-20-to-create-a-managed-kubernetes-cluster"></a>Uso de la CLI de Azure 2.0 para crear un clúster de Kubernetes administrado
Para crear un clúster de Kubernetes administrado con la [CLI de Azure 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest), asegúrese de que está usando la CLI de Azure 2.0.25 o posterior.

1. Inicie sesión en la cuenta de Azure. Una vez que escriba el siguiente comando `az login`, se proporcionan instrucciones que explican cómo completar el inicio de sesión. 
    
    ```bash
    az login
    ```

1. Al ejecutar el comando `az login` en el paso anterior, se muestra una lista de todas las suscripciones de Azure (junto con sus identificadores de suscripción). En este paso, se establece la suscripción de Azure predeterminada. Reemplace el marcador de posición &lt;your-subscription-id> por el identificador de suscripción de Azure deseado. 

    ```bash
    az account set -s <your-subscription-id>
    ```

1. Cree un grupo de recursos. Reemplace el marcador de posición &lt;your-resource-group-name> por el nombre del nuevo grupo de recursos y reemplace el marcador de posición &lt;your-location> con la ubicación. El comando `az account list-locations` muestra todas las ubicaciones de Azure. Durante la versión preliminar de AKS, no todas las ubicaciones están disponibles. Si escribe una ubicación que no es válida en este momento, el mensaje de error enumerará las ubicaciones disponibles.

    ```bash
    az group create -n <your-resource-group-name> -l <your-location>
    ```

1. Cree un clúster de Kubernetes. Reemplace el marcador de posición &lt;your-resource-group-name> con el nombre del grupo de recursos creado en el paso anterior y reemplace el marcador de posición &lt;you-kubernetes-cluster-name> con el nombre del nuevo clúster. (Este proceso puede tardar varios minutos en completarse).

    ```bash
    az aks create -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --generate-ssh-keys --node-count 2
    ```

### <a name="set-up-the-kubernetes-cluster"></a>Configuración del clúster de Kubernetes

La configuración de una implementación azul/verd en AKS puede realizarse con un script de configuración proporcionado en el ejemplo clonado anteriormente o manualmente. En esta sección, verá cómo aplicar ambas opciones.

#### <a name="set-up-the-kubernetes-cluster-via-the-sample-setup-script"></a>Configuración del clúster de Kubernetes con el script de configuración de ejemplo
1. Edite el archivo **deploy/aks/setup/setup.sh** y reemplace los siguientes marcadores de posición con los valores apropiados del entorno: 

    - **&lt;your-resource-group-name>**
    - **&lt;your-kubernetes-cluster-name>**
    - **&lt;your-location>**
    - **&lt;your-dns-name-suffix>**

    ![El script setup.sh contiene varios marcadores de posición que se pueden modificar para su entorno.](./media/jenkins-aks-blue-green-deployment/edit-setup-script.png)

1. Ejecute el script de configuración.

    ```bash
    sh setup.sh
    ```

#### <a name="set-up-a-kubernetes-cluster-manually"></a>Configuración manual de un clúster de Kubernetes 
1. Descargue la configuración de Kubernetes en la carpeta de perfil.

    ```bash
    az aks get-credentials -g <your-resource-group-name> -n <your-kubernetes-cluster-name> --admin
    ```

1. Cambie el directorio al directorio **deploy/aks/setup**. 

1. Ejecute los siguientes comandos **kubectl** para configurar los servicios para el punto de conexión público y los dos puntos de conexión de prueba.

    ```bash
    kubectl apply -f  service-green.yml
    kubectl apply -f  test-endpoint-blue.yml
    kubectl apply -f  test-endpoint-green.yml
    ```

1. Actualice el nombre DNS para los puntos de conexión público y de prueba. Al crear un clúster de Kubernetes, también se crea un [grupo de recursos adicional](https://github.com/Azure/AKS/issues/3) con el patrón de nomenclatura de **MC_&lt;your-resource-group-name>_&lt;your-kubernetes-cluster-name>_&lt;your-location>**.

    Localización de las direcciones IP públicas del grupo de recursos

    ![Dirección IP pública del grupo de recursos](./media/jenkins-aks-blue-green-deployment/publicip.png)

    Para cada uno de los servicios, busque la dirección IP externa mediante la ejecución del comando siguiente:
    
    ```bash
    kubectl get service todoapp-service
    ```
    
    Actualice el nombre DNS para la dirección IP correspondiente con el siguiente comando:

    ```bash
    az network public-ip update --dns-name aks-todoapp --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    Repita la llamada para `todoapp-test-blue` y `todoapp-test-green`:

    ```bash
    az network public-ip update --dns-name todoapp-blue --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>

    az network public-ip update --dns-name todoapp-green --ids /subscriptions/<your-subscription-id>/resourceGroups/MC_<resourcegroup>_<aks>_<location>/providers/Microsoft.Network/publicIPAddresses/kubernetes-<ip-address>
    ```

    El nombre DNS debe ser único en la suscripción. `<your-dns-name-suffix>` puede usarse para garantizar la exclusividad.

### <a name="create-azure-container-registry"></a>Creación de una instancia de Azure Container Registry

1. Ejecute el comando `az acr create` para crear una instancia de Azure Container Registry. Una vez creada la instancia de Azure Container Registry, use `login server` como la dirección URL de registro de Docker en la siguiente sección.

    ```bash
    az acr create -n <your-registry-name> -g <your-resource-group-name>
    ```

1. Ejecute el comando `az acr credential` para mostrar las credenciales de Azure Container Registry. Anote el nombre de usuario y la contraseña del registro de Docker, ya que se usarán en la siguiente sección.

    ```bash
    az acr credential show -n <your-registry-name>
    ```

## <a name="prepare-the-jenkins-server"></a>Preparación del servidor Jenkins

En esta sección, verá cómo preparar el servidor de Jenkins para ejecutar una compilación, que sea adecuada para realizar pruebas. Sin embargo, como se explica en el artículo de Jenkins sobre las [implicaciones de seguridad de compilación en el servidor maestro](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master), se aconseja usar un [agente de máquina virtual de Azure](https://plugins.jenkins.io/azure-vm-agents) o un [agente de Azure Container](https://plugins.jenkins.io/azure-container-agents) para poner en marcha un agente en Azure a fin de ejecutar las compilaciones. 

1. Implemente un [servidor maestro Jenkins en Azure](https://aka.ms/jenkins-on-azure).

1. Conéctese al servidor a través de SSH e instale las herramientas de compilación en el servidor donde se ejecuta la compilación.
   
   ```bash
   sudo apt-get install git maven 
   ```
   
1. [Instale Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-docker-ce). Asegúrese de que el usuario `jenkins` tiene permiso para ejecutar los comandos `docker`.

1. [Instale kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/).

1. [Descargue jq](https://stedolan.github.io/jq/download/).

1. Instale jq con el comando siguiente:

   ```bash
   sudo apt-get install jq
   ```
   
1. Instale los complementos en Jenkins mediante la aplicación de los pasos siguientes en el panel de Jenkins:

    1. Seleccione **Manage Jenkins > Manage Plugins > Available** (Administrar Jenkins > Administrar complementos > Disponibles).
    1. Busque e instale el complemento Azure Container Service.

1. Debe agregar las credenciales que se usarán para administrar los recursos de Azure. Si aún no tiene el complemento, instale **Azure Credential**.

1. Agregue la credencial de la entidad de servicio de Azure como el tipo **Entidad de servicio de Microsoft Azure**.

1. Agregue el nombre de usuario y la contraseña del registro de Docker para Azure (según haya obtenido en la sección **Creación de una instancia de Azure Container Registry**) como el tipo **Nombre de usuario con contraseña**.

## <a name="edit-the-jenkinsfile"></a>Edición del archivo de Jenkins

1. En su propio repositorio, vaya a `/deploy/aks/` y abra `Jenkinsfile`.

2. Actualice el archivo como sigue:

    ```groovy
    def servicePrincipalId = '<your-service-principal>'
    def resourceGroup = '<your-resource-group-name>'
    def aks = '<your-kubernetes-cluster-name>'

    def cosmosResourceGroup = '<your-cosmodb-resource-group>'
    def cosmosDbName = '<your-cosmodb-name>'
    def dbName = '<your-dbname>'

    def dockerRegistry = '<your-acr-name>.azurecr.io'
    ```
    
    Además, actualice el identificador de credenciales de ACR:
    
    ```groovy
    def dockerCredentialId = '<your-acr-credential-id>'
    ```

## <a name="create-the-job"></a>Creación del trabajo
1. Agregue un nuevo trabajo de tipo **Canalización**.

1. Seleccione el **script Canalización > Definición > Canalización de SCM**.

1. Escriba la dirección URL del repositorio de SCM con &lt;your-forked-repo>.

1. Escriba la ruta de acceso del script como `deploy/aks/Jenkinsfile`.

## <a name="run-the-job"></a>Ejecución del trabajo

1. Verifique que puede ejecutar el proyecto correctamente en el entorno local. [Ejecución del proyecto en el equipo local](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/README.md#run-it)

1. Ejecute el trabajo de Jenkins. Al ejecutar el trabajo de Jenkins por primera vez, Jenkins implementará la aplicación de lista de tareas en el entorno azul, que es el entorno inactivo predeterminado. 

1. Para verificar que el trabajo se ejecutó, examine las direcciones URL:
    - Punto de conexión público: `http://aks-todoapp<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Punto de conexión azul: `http://aks-todoapp-blue<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`
    - Punto de conexión verde: `http://aks-todoapp-green<your-dns-name-suffix>.<your-location>.cloudapp.azure.com`

Los puntos de conexión de prueba público y azul tienen la misma actualización, mientras que el punto de conexión verde muestra la imagen de Tomcat predeterminada. 

Si ejecuta la compilación más de una vez, recorre las implementaciones azul y verde. En otras palabras, si el entorno actual es azul, el trabajo se implementará/probará en el entorno verde y luego se actualizará el punto de conexión público de la aplicación para enrutar el tráfico al entorno verde si las pruebas se realizan correctamente.

## <a name="additional-information"></a>Información adicional

Para más información sobre la implementación sin tiempo de inactividad, consulte esta [plantilla de inicio rápido](https://github.com/Azure/azure-quickstart-templates/tree/master/301-jenkins-aks-zero-downtime-deployment). 

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, elimine los recursos creados en este tutorial.

```bash
az group delete -y --no-wait -n <your-resource-group-name>
```

## <a name="troubleshooting"></a>solución de problemas

Si detecta algún error con los complementos de Jenkins, envíe un problema en [Jenkins JIRA](https://issues.jenkins-ci.org/) para el componente específico.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información sobre cómo realizar implementaciones en Azure Kubernetes Service (AKS) con Jenkins y el patrón de implementación azul/verde. Para obtener más información sobre el proveedor de Jenkins para Azure, consulte el sitio de Jenkins en Azure.

> [!div class="nextstepaction"]
> [Jenkins en Azure](/azure/jenkins/)