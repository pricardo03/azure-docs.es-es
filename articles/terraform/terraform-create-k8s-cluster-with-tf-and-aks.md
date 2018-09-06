---
title: Creación de un clúster de Kubernetes con Azure Kubernetes Service (AKS) y Terraform
description: Tutorial que muestra la creación de un clúster de Kubernetes con Azure Kubernetes Service y Terraform
services: terraform
ms.service: terraform
keywords: terraform, devops, máquina virtual, Azure, kubernetes
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 06/11/2018
ms.openlocfilehash: 8a997c88943b0273d3136dbf02a784fbdb982527
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666814"
---
# <a name="create-a-kubernetes-cluster-with-azure-kubernetes-service-and-terraform"></a>Creación de un clúster de Kubernetes con Azure Kubernetes Service y Terraform
[Azure Kubernetes Service (AKS)](/azure/aks/) permite administrar el entorno hospedado de Kubernetes, lo que hace que sea fácil y rápido implementar y administrar aplicaciones en contenedores sin necesidad de tener conocimientos de orquestación de contenedores. También permite eliminar la carga de las operaciones en curso y las de mantenimiento mediante el aprovisionamiento, actualización y escalado de los recursos a petición, sin tener que desconectar las aplicaciones.

En este tutorial, aprenderá a realizar las tareas siguientes para crear un clúster de [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) mediante [Terraform](http://terraform.io) y AKS:

> [!div class="checklist"]
> * Uso de HCL (HashiCorp Language) para definir un clúster de Kubernetes
> * Uso de Terraform y AKS para crear un clúster de Kubernetes
> * Utilice la herramienta kubectl para probar la disponibilidad de un clúster de Kubernetes

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita de Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

- **Configuración de Terraform**: siga las instrucciones del artículo, [Instalación y configuración de Terraform para aprovisionar máquinas virtuales y otras infraestructuras en Azure](/azure/virtual-machines/linux/terraform-install-configure).

- **Entidad de servicio de Azure**: siga las instrucciones de la sección **Creación de la entidad de servicio** del artículo [Creación de una entidad de servicio de Azure con la CLI de Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal). Tome nota de los valores de appId, displayName, contraseña e inquilino.

## <a name="create-the-directory-structure"></a>Creación de la estructura de directorios
El primer paso es crear el directorio que contenga los archivos de configuración de Terraform para el ejercicio.

1. Vaya a [Azure Portal](http://portal.azure.com).

1. Abra [Azure Cloud Shell](/azure/cloud-shell/overview). Si no seleccionó un entorno previamente, seleccione **Bash** como entorno.

    ![Aviso de Cloud Shell](./media/terraform-create-k8s-cluster-with-tf-and-aks/azure-portal-cloud-shell-button-min.png)

1. Cambie al directorio `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Cree un directorio llamado `terraform-aks-k8s`.

    ```bash
    mkdir terraform-aks-k8s
    ```

1. Cambie los directorios al nuevo directorio:

    ```bash
    cd terraform-aks-k8s
    ```

## <a name="declare-the-azure-provider"></a>Declaración del proveedor de Azure
Cree el archivo de configuración de Terraform que declara el proveedor de Azure.

1. En Cloud Shell, cree un archivo denominado `main.tf`.

    ```bash
    vi main.tf
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue el siguiente código en el editor:

    ```JSON
    provider "azurerm" {
        version = "=1.5.0"
    }

    terraform {
        backend "azurerm" {}
    }

    ```

1. Seleccione la tecla **Esc** para acceder al modo de inserción.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

## <a name="define-a-kubernetes-cluster"></a>Definición de un clúster de Kubernetes
Cree el archivo de configuración de Terraform que declara los recursos para el clúster de Kubernetes.

1. En Cloud Shell, cree un archivo denominado `k8s.tf`.

    ```bash
    vi k8s.tf
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue el siguiente código en el editor:

    ```JSON
    resource "azurerm_resource_group" "k8s" {
        name     = "${var.resource_group_name}"
        location = "${var.location}"
    }

    resource "azurerm_kubernetes_cluster" "k8s" {
        name                = "${var.cluster_name}"
        location            = "${azurerm_resource_group.k8s.location}"
        resource_group_name = "${azurerm_resource_group.k8s.name}"
        dns_prefix          = "${var.dns_prefix}"

        linux_profile {
            admin_username = "ubuntu"

            ssh_key {
            key_data = "${file("${var.ssh_public_key}")}"
            }
        }

        agent_pool_profile {
            name            = "default"
            count           = "${var.agent_count}"
            vm_size         = "Standard_D2"
            os_type         = "Linux"
            os_disk_size_gb = 30
        }

        service_principal {
            client_id     = "${var.client_id}"
            client_secret = "${var.client_secret}"
        }

        tags {
            Environment = "Development"
        }
    }
    ```

    El código anterior establece el nombre del clúster, la ubicación y el resource_group_name. Además, se establece el valor de dns_prefix, que forma parte del nombre de dominio completo (FQDN) utilizado para acceder al clúster.

    El registro **linux_profile** le permite configurar las opciones que permiten iniciar sesión en los nodos de trabajo mediante SSH.

    Con AKS, solo se paga por los nodos de trabajo. El registro **agent_pool_profile** configura los detalles de estos nodos de trabajo. El registro **agent_pool_profile** incluye el número de nodos de trabajo que se van a crear y el tipo de nodos de trabajo. Si necesita escalar o reducir verticalmente el clúster en el futuro, modifique el valor de **count** en este registro.

1. Seleccione la tecla **Esc** para acceder al modo de inserción.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

## <a name="declare-the-variables"></a>Declaración de las variables

1. En Cloud Shell, cree un archivo denominado `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue el siguiente código en el editor:

    ```JSON
    variable "client_id" {}
    variable "client_secret" {}

    variable "agent_count" {
        default = 3
    }

    variable "ssh_public_key" {
        default = "~/.ssh/id_rsa.pub"
    }

    variable "dns_prefix" {
        default = "k8stest"
    }

    variable cluster_name {
        default = "k8stest"
    }

    variable resource_group_name {
        default = "azure-k8stest"
    }

    variable location {
        default = "Central US"
    }
    ```

1. Seleccione la tecla **Esc** para acceder al modo de inserción.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

## <a name="create-a-terraform-output-file"></a>Creación de un archivo de salida de Terraform
[Las salidas de Terraform](https://www.terraform.io/docs/configuration/outputs.html) permiten definir valores que se resaltarán al usuario cuando Terraform aplique un plan, y pueden consultarse con el comando `terraform output`. En esta sección, va a crear un archivo de salida que permita el acceso al clúster con [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/).

1. En Cloud Shell, cree un archivo denominado `output.tf`.

    ```bash
    vi output.tf
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue el siguiente código en el editor:

    ```JSON
    output "client_key" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_key}"
    }

    output "client_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.client_certificate}"
    }

    output "cluster_ca_certificate" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.cluster_ca_certificate}"
    }

    output "cluster_username" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.username}"
    }

    output "cluster_password" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.password}"
    }

    output "kube_config" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config_raw}"
    }

    output "host" {
        value = "${azurerm_kubernetes_cluster.k8s.kube_config.0.host}"
    }
    ```

1. Seleccione la tecla **Esc** para acceder al modo de inserción.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

## <a name="set-up-azure-storage-to-store-terraform-state"></a>Configuración del almacenamiento de Azure para almacenar el estado de Terraform
Terraform realiza un seguimiento del estado de manera local a través del archivo `terraform.tfstate`. Este patrón funciona bien en un entorno de persona único. Sin embargo, en un entorno más práctico de varias personas, debe realizar el seguimiento del estado en el servidor mediante [Azure Storage](/azure/storage/). En esta sección se recupera la información necesaria de la cuenta de almacenamiento (nombre y clave de cuenta) y se crea un contenedor de almacenamiento en el que se almacenará la información de estado de Terraform.

1. En Azure Portal, seleccione **Todos los servicios** en el menú de la izquierda.

1. Seleccione **Cuentas de almacenamiento**.

1. En la pestaña **Cuentas de almacenamiento**, seleccione el nombre de la cuenta de almacenamiento en la que Terraform va a almacenar el estado. Por ejemplo, puede utilizar la cuenta de almacenamiento creada cuando abrió Cloud Shell por primera vez.  El nombre de cuenta de almacenamiento creada por Cloud Shell normalmente comienza por `cs` seguido de una cadena aleatoria de números y letras. **Recuerde el nombre de la cuenta de almacenamiento que seleccione, ya que se necesitará más adelante.**

1. En la pestaña Cuenta de almacenamiento, seleccione **Claves de acceso**.

    ![Menú de la cuenta de almacenamiento](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account.png)

1. Anote el valor de la **clave** **clave1**. (Al seleccionar el icono a la derecha de la clave, se copia el valor al portapapeles).

    ![Claves de acceso de cuenta de almacenamiento](./media/terraform-create-k8s-cluster-with-tf-and-aks/storage-account-access-key.png)

1. En Cloud Shell, cree un contenedor en su cuenta de almacenamiento de Azure (reemplace los marcadores de posición &lt;YourAzureStorageAccountName> y &lt;YourAzureStorageAccountAccessKey> por los valores apropiados para su cuenta de almacenamiento de Azure).

    ```bash
    az storage container create -n tfstate --account-name <YourAzureStorageAccountName> --account-key <YourAzureStorageAccountKey>
    ```

## <a name="create-the-kubernetes-cluster"></a>Creación de un clúster de Kubernetes
En esta sección, puede ver cómo usar el comando `terraform init` para crear los recursos definidos en los archivos de configuración que se crearon en las secciones anteriores.

1. En Cloud Shell, inicialice Terraform (reemplace los marcadores de posición &lt;YourAzureStorageAccountName> y &lt;YourAzureStorageAccountAccessKey> por los valores apropiados para su cuenta de almacenamiento de Azure).

    ```bash
    terraform init -backend-config="storage_account_name=<YourAzureStorageAccountName>" -backend-config="container_name=tfstate" -backend-config="access_key=<YourStorageAccountAccessKey>" -backend-config="key=codelab.microsoft.tfstate" 
    ```
    
    El comando `terraform init` muestra el éxito de inicializar el back-end y el complemento del proveedor:

    ![Ejemplo de resultados de "terraform init"](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-init-complete.png)

1. Ejecute el comando `terraform plan` para crear el plan de Terraform que define los elementos de infraestructura. El comando solicitará dos valores: **var.client_id** y **var.client_secret**. Para la variable **var.client_id**, introduzca el valor **appId** asociado con la entidad de servicio. Para la variable **var.client_secret**, introduzca el valor **password** asociado con la entidad de servicio.

    ```bash
    terraform plan -out out.plan
    ```

    El comando `terraform plan` muestra los recursos que se crearán cuando ejecute el comando `terraform apply`:

    ![Ejemplo de resultados de "terraform plan"](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-plan-complete.png)

1. Ejecute el comando `terraform apply` para aplicar el plan para crear el clúster de Kubernetes. El proceso para crear un clúster de Kubernetes puede tardar varios minutos, dado como resultado el agotamiento del tiempo de espera de la sesión de Cloud Shell. Si la sesión de Cloud Shell agota su tiempo de espera, puede seguir los pasos de la sección ["Recuperación de un tiempo de espera de Cloud Shell"](#recover-from-a-dloud-shell-timeout) para poder completar el tutorial.

    ```bash
    terraform apply out.plan
    ```

    El comando `terraform apply` muestra los resultados de la creación de los recursos definidos en sus archivos de configuración:

    ![Ejemplo de resultados de "terraform apply"](./media/terraform-create-k8s-cluster-with-tf-and-aks/terraform-apply-complete.png)

1. En Azure Portal, seleccione **Todos los servicios** en el menú de la izquierda para ver los recursos creados para su nuevo clúster de Kubernetes.

    ![Aviso de Cloud Shell](./media/terraform-create-k8s-cluster-with-tf-and-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Recuperación de un tiempo de espera de Cloud Shell
Si se agota el tiempo de espera de la sesión de Cloud Shell, puede realizar los siguientes pasos para recuperarlo:

1. Inicie una sesión de Cloud Shell.

1. Cambie al directorio que contiene los archivos de configuración de Terraform.

    ```bash
    cd /clouddrive/terraform-aks-k8s
    ```

1. Ejecute el siguiente comando:

    ```bash
    export KUBECONFIG=./azurek8s
    ```
    
## <a name="test-the-kubernetes-cluster"></a>Prueba del clúster de Kubernetes
Las herramientas de Kubernetes se pueden usar para comprobar el clúster recién creado.

1. Obtenga la configuración de Kubernetes desde el estado de Terraform y almacénela en un archivo que kubectl puede leer.

    ```bash
    echo "$(terraform output kube_config)" > ./azurek8s
    ```

1. Establezca una variable de entorno para que kubectl seleccione la configuración correcta.

    ```bash
    export KUBECONFIG=./azurek8s
    ```

1. Compruebe el mantenimiento del clúster.

    ```bash
    kubectl get nodes
    ```

    Debería ver los detalles de sus nodos de trabajo, y todos deberían tener el estado **Listo**, como se muestra en la siguiente imagen:

    ![La herramienta kubectl le permite comprobar el mantenimiento de su clúster de Kubernetes](./media/terraform-create-k8s-cluster-with-tf-and-aks/kubectl-get-nodes.png)

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a utilizar Terraform y AKS para crear un clúster de Kubernetes. Aquí tiene algunos recursos adicionales donde encontrará más información acerca de Terraform en Azure: 

 [Centro de Terraform en Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Documentación del proveedor de Azure para Terraform](http://aka.ms/terraform)  
 [Origen del proveedor de Azure para Terraform](http://aka.ms/tfgit)  
 [Módulos de Azure para Terraform](http://aka.ms/tfmodules)