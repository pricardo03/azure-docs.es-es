---
title: Creación de un clúster de Kubernetes con Application Gateway como controlador de entrada con Azure Kubernetes Service (AKS)
description: Tutorial que ilustra cómo crear un clúster de Kubernetes con Azure Kubernetes Service con Application Gateway como controlador de entrada
services: terraform
ms.service: azure
keywords: terraform, devops, virtual machine, azure, kubernetes, ingress, application gateway
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 1/10/2019
ms.openlocfilehash: 477b2ec1af4c52f51c3ab20ac2ddf7ef043dfcc7
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57994350"
---
# <a name="create-a-kubernetes-cluster-with-application-gateway-ingress-controller-using-azure-kubernetes-service-and-terraform"></a>Creación de un clúster de Kubernetes con el controlador de entrada de Application Gateway mediante Azure Kubernetes Service y Terraform
[Azure Kubernetes Service (AKS)](/azure/aks/) administra el entorno hospedado de Kubernetes. AKS facilita y acelera la implementación y administración de aplicaciones en contenedores sin necesidad de tener conocimientos de orquestación de contenedores. También permite eliminar la carga de las operaciones en curso y las de mantenimiento mediante el aprovisionamiento, actualización y escalado de los recursos a petición, sin tener que desconectar las aplicaciones.

Un controlador de entrada es un software que proporciona el proxy inverso, el enrutamiento del tráfico configurable y la terminación de TLS para los servicios de Kubernetes. Los recursos de entrada de Kubernetes se usan para configurar las reglas de entrada y las rutas de los distintos servicios de Kubernetes. Mediante reglas de entrada y un controlador de entrada, se puede usar una sola dirección IP para enrutar el tráfico a varios servicios en un clúster de Kubernetes. Azure [Application Gateway](/azure/Application-Gateway/) proporciona todas las funcionalidades anteriores, lo que convierte a este servicio en un controlador de entrada perfecto para Kubernetes en Azure. 

En este tutorial, aprenderá a realizar las tareas siguientes para crear un clúster de [Kubernetes](https://www.redhat.com/en/topics/containers/what-is-kubernetes) con AKS mediante Application Gateway como controlador de entrada:

> [!div class="checklist"]
> * Uso de HCL (HashiCorp Language) para definir un clúster de Kubernetes
> * Uso de Terraform para crear un recurso de Application Gateway
> * Uso de Terraform y AKS para crear un clúster de Kubernetes
> * Utilice la herramienta kubectl para probar la disponibilidad de un clúster de Kubernetes

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

- **Configuración de Terraform**: siga las instrucciones del artículo [Instalación y configuración de Terraform para aprovisionar máquinas virtuales y otras infraestructuras en Azure](/azure/virtual-machines/linux/terraform-install-configure).

- **Entidad de servicio de Azure**: siga las instrucciones de la sección **Creación de la entidad de servicio** del artículo [Creación de una entidad de servicio de Azure con la CLI de Azure](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Anote los valores de appId, displayName y contraseña.
  - Anote el identificador de objeto de la entidad de servicio mediante la ejecución del siguiente comando:

    ```bash
    az ad sp list --display-name <displayName>
    ```

## <a name="create-the-directory-structure"></a>Creación de la estructura de directorios
El primer paso es crear el directorio que contenga los archivos de configuración de Terraform para el ejercicio.

1. Vaya a [Azure Portal](https://portal.azure.com).

1. Abra [Azure Cloud Shell](/azure/cloud-shell/overview). Si no seleccionó un entorno previamente, seleccione **Bash** como entorno.

    ![Aviso de Cloud Shell](./media/terraform-k8s-cluster-appgw-with-tf-aks/azure-portal-cloud-shell-button-min.png)

1. Cambie al directorio `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Cree un directorio llamado `terraform-aks-k8s`.

    ```bash
    mkdir terraform-aks-appgw-ingress
    ```

1. Cambie los directorios al nuevo directorio:

    ```bash
    cd terraform-aks-appgw-ingress
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
        version = "~>1.18"
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
   ## <a name="define-input-variables"></a>Definición de las variables de entrada
   Creación del archivo de configuración de Terraform que muestra todas las variables necesarias para esta implementación
1. En Cloud Shell, cree un archivo denominado `variables.tf`.
    ```bash
    vi variables.tf
    ```
1. Seleccione la tecla I para acceder al modo de inserción.

2. Pegue el siguiente código en el editor:
    
    ```JSON
    variable "resource_group_name" {
      description = "Name of the resource group already created."
    }
    
    variable "location" {
      description = "Location of the cluster."
    }
    
    variable "aks_service_principal_app_id" {
      description = "Application ID/Client ID  of the service principal. Used by AKS to manage AKS related resources on Azure like vms, subnets."
    }
    
    variable "aks_service_principal_client_secret" {
      description = "Secret of the service principal. Used by AKS to manage Azure."
    }
    
    variable "aks_service_principal_object_id" {
      description = "Object ID of the service principal."
    }
    
    variable "virtual_network_name" {
      description = "Virtual network name"
      default     = "aksVirtualNetwork"
    }
    
    variable "virtual_network_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.0.0.0/8"
    }
    
    variable "aks_subnet_name" {
      description = "AKS Subnet Name."
      default     = "kubesubnet"
    }
    
    variable "aks_subnet_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.0.0.0/16"
    }
    
    variable "app_gateway_subnet_address_prefix" {
      description = "Containers DNS server IP address."
      default     = "15.1.0.0/16"
    }
    
    variable "app_gateway_name" {
      description = "Name of the Application Gateway."
      default = "ApplicationGateway1"
    }
    
    variable "app_gateway_sku" {
      description = "Name of the Application Gateway SKU."
      default = "Standard_v2"
    }
    
    
    variable "app_gateway_tier" {
      description = "Tier of the Application Gateway SKU."
      default = "Standard_v2"
    }
    
    
    variable "aks_name" {
      description = "Name of the AKS cluster."
      default     = "aks-cluster1"
    }
    variable "aks_dns_prefix" {
      description = "Optional DNS prefix to use with hosted Kubernetes API server FQDN."
      default     = "aks"
    }
    
    
    variable "aks_agent_os_disk_size" {
      description = "Disk size (in GB) to provision for each of the agent pool nodes. This value ranges from 0 to 1023. Specifying 0 will apply the default disk size for that agentVMSize."
      default     = 40
    }
    
    variable "aks_agent_count" {
      description = "The number of agent nodes for the cluster."
      default     = 3
    }
    
    variable "aks_agent_vm_size" {
      description = "The size of the Virtual Machine."
      default     = "Standard_D3_v2"
    }
    
    variable "kubernetes_version" {
      description = "The version of Kubernetes."
      default     = "1.11.5"
    }
    
    variable "aks_service_cidr" {
      description = "A CIDR notation IP range from which to assign service cluster IPs."
      default     = "10.0.0.0/16"
    }
    
    variable "aks_dns_service_ip" {
      description = "Containers DNS server IP address."
      default     = "10.0.0.10"
    }
    
    variable "aks_docker_bridge_cidr" {
      description = "A CIDR notation IP for Docker bridge."
      default     = "172.17.0.1/16"
    }
    
    variable "aks_enable_rbac" {
      description = "Enable RBAC on the AKS cluster. Defaults to false."
      default     = "false"
    }
    
    variable "vm_user_name" {
      description = "User name for the VM"
      default     = "vmuser1"
    }
    
    variable "public_ssh_key_path" {
      description = "Public key path for SSH."
      default     = "~/.ssh/id_rsa.pub"
    }
    
    variable "tags" {
      type = "map"
    
      default = {
        source = "terraform"
      }
    }
    ```

## <a name="define-the-resources"></a>Definición de los recursos 
Cree el archivo de configuración de Terraform que crea todos los recursos. 

1. En Cloud Shell, cree un archivo denominado `resources.tf`.

    ```bash
    vi resources.tf
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue los siguientes bloques de código en el editor:

     a. Crear un bloque de variables locales para que lo reutilicen las variables calculadas

    ```JSON
    # # Locals block for hardcoded names. 
    locals {
        backend_address_pool_name      = "${azurerm_virtual_network.test.name}-beap"
        frontend_port_name             = "${azurerm_virtual_network.test.name}-feport"
        frontend_ip_configuration_name = "${azurerm_virtual_network.test.name}-feip"
        http_setting_name              = "${azurerm_virtual_network.test.name}-be-htst"
        listener_name                  = "${azurerm_virtual_network.test.name}-httplstn"
        request_routing_rule_name      = "${azurerm_virtual_network.test.name}-rqrt"
        app_gateway_subnet_name = "appgwsubnet"
    }
    ```
    b. Crear un origen de datos para el grupo de recursos, la nueva identidad de usuario
    ```JSON
    data "azurerm_resource_group" "rg" {
      name = "${var.resource_group_name}"
    }
    
    # User Assigned Idntities 
    resource "azurerm_user_assigned_identity" "testIdentity" {
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
      location            = "${data.azurerm_resource_group.rg.location}"
    
      name = "identity1"
    
      tags = "${var.tags}"
    }
    ```
    c. Crear recursos de red base
   ```JSON
    resource "azurerm_virtual_network" "test" {
      name                = "${var.virtual_network_name}"
      location            = "${data.azurerm_resource_group.rg.location}"
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
      address_space       = ["${var.virtual_network_address_prefix}"]
    
      subnet {
        name           = "${var.aks_subnet_name}"
        address_prefix = "${var.aks_subnet_address_prefix}" 
      }
    
      subnet {
        name           = "appgwsubnet"
        address_prefix = "${var.app_gateway_subnet_address_prefix}"
      }
    
      tags = "${var.tags}"
    }
    
    data "azurerm_subnet" "kubesubnet" {
      name                 = "${var.aks_subnet_name}"
      virtual_network_name = "${azurerm_virtual_network.test.name}"
      resource_group_name  = "${data.azurerm_resource_group.rg.name}"
    }
    
    data "azurerm_subnet" "appgwsubnet" {
      name                 = "appgwsubnet"
      virtual_network_name = "${azurerm_virtual_network.test.name}"
      resource_group_name  = "${data.azurerm_resource_group.rg.name}"
    }
    
    # Public Ip 
    resource "azurerm_public_ip" "test" {
      name                         = "publicIp1"
      location                     = "${data.azurerm_resource_group.rg.location}"
      resource_group_name          = "${data.azurerm_resource_group.rg.name}"
      public_ip_address_allocation = "static"
      sku                          = "Standard"
    
      tags = "${var.tags}"
    }
    ```
    d. Crear un recurso de Application Gateway
    ```JSON
    resource "azurerm_application_gateway" "network" {
      name                = "${var.app_gateway_name}"
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
      location            = "${data.azurerm_resource_group.rg.location}"
    
      sku {
        name     = "${var.app_gateway_sku}"
        tier     = "Standard_v2"
        capacity = 2
      }
    
      gateway_ip_configuration {
        name      = "appGatewayIpConfig"
        subnet_id = "${data.azurerm_subnet.appgwsubnet.id}"
      }
    
      frontend_port {
        name = "${local.frontend_port_name}"
        port = 80
      }
    
      frontend_port {
        name = "httpsPort"
        port = 443
      }
    
      frontend_ip_configuration {
        name                 = "${local.frontend_ip_configuration_name}"
        public_ip_address_id = "${azurerm_public_ip.test.id}"
      }
    
      backend_address_pool {
        name = "${local.backend_address_pool_name}"
      }
    
      backend_http_settings {
        name                  = "${local.http_setting_name}"
        cookie_based_affinity = "Disabled"
        port                  = 80
        protocol              = "Http"
        request_timeout       = 1
      }
    
      http_listener {
        name                           = "${local.listener_name}"
        frontend_ip_configuration_name = "${local.frontend_ip_configuration_name}"
        frontend_port_name             = "${local.frontend_port_name}"
        protocol                       = "Http"
      }
    
      request_routing_rule {
        name                       = "${local.request_routing_rule_name}"
        rule_type                  = "Basic"
        http_listener_name         = "${local.listener_name}"
        backend_address_pool_name  = "${local.backend_address_pool_name}"
        backend_http_settings_name = "${local.http_setting_name}"
      }
    
      tags = "${var.tags}"
    
      depends_on = ["azurerm_virtual_network.test", "azurerm_public_ip.test"]
    }
    ```
    e. Crear asignaciones de roles
    ```JSON
    resource "azurerm_role_assignment" "ra1" {
      scope                = "${data.azurerm_subnet.kubesubnet.id}"
      role_definition_name = "Network Contributor"
      principal_id         = "${var.aks_service_principal_object_id }"
    
      depends_on = ["azurerm_virtual_network.test"]
    }
    
    resource "azurerm_role_assignment" "ra2" {
      scope                = "${azurerm_user_assigned_identity.testIdentity.id}"
      role_definition_name = "Managed Identity Operator"
      principal_id         = "${var.aks_service_principal_object_id}"
      depends_on           = ["azurerm_user_assigned_identity.testIdentity"]
    }
    
    resource "azurerm_role_assignment" "ra3" {
      scope                = "${azurerm_application_gateway.network.id}"
      role_definition_name = "Contributor"
      principal_id         = "${azurerm_user_assigned_identity.testIdentity.principal_id}"
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }
    
    resource "azurerm_role_assignment" "ra4" {
      scope                = "${data.azurerm_resource_group.rg.id}"
      role_definition_name = "Reader"
      principal_id         = "${azurerm_user_assigned_identity.testIdentity.principal_id}"
      depends_on           = ["azurerm_user_assigned_identity.testIdentity", "azurerm_application_gateway.network"]
    }
    ```
    f. Creación de un clúster de Kubernetes
    ```JSON
    resource "azurerm_kubernetes_cluster" "k8s" {
      name       = "${var.aks_name}"
      location   = "${data.azurerm_resource_group.rg.location}"
      dns_prefix = "${var.aks_dns_prefix}"
    
      resource_group_name = "${data.azurerm_resource_group.rg.name}"
    
      linux_profile {
        admin_username = "${var.vm_user_name}"
    
        ssh_key {
          key_data = "${file(var.public_ssh_key_path)}"
        }
      }
    
      addon_profile {
        http_application_routing {
          enabled = false
        }
      }
    
      agent_pool_profile {
        name            = "agentpool"
        count           = "${var.aks_agent_count}"
        vm_size         = "${var.aks_agent_vm_size}"
        os_type         = "Linux"
        os_disk_size_gb = "${var.aks_agent_os_disk_size}"
        vnet_subnet_id  = "${data.azurerm_subnet.kubesubnet.id}"
      }
    
      service_principal {
        client_id     = "${var.aks_service_principal_app_id}"
        client_secret = "${var.aks_service_principal_client_secret}"
      }
    
      network_profile {
        network_plugin     = "azure"
        dns_service_ip     = "${var.aks_dns_service_ip}"
        docker_bridge_cidr = "${var.aks_docker_bridge_cidr}"
        service_cidr       = "${var.aks_service_cidr}"
      }
    
      depends_on = ["azurerm_virtual_network.test", "azurerm_application_gateway.network"]
      tags       = "${var.tags}"
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

1. En la pestaña **Cuentas de almacenamiento**, seleccione el nombre de la cuenta de almacenamiento en la que Terraform va a almacenar el estado. Por ejemplo, puede utilizar la cuenta de almacenamiento creada cuando abrió Cloud Shell por primera vez.  El nombre de cuenta de almacenamiento creada por Cloud Shell normalmente comienza por `cs` seguido de una cadena aleatoria de números y letras. **Anote el nombre de la cuenta de almacenamiento que seleccione; lo necesitaremos más tarde.**

1. En la pestaña Cuenta de almacenamiento, seleccione **Claves de acceso**.

    ![Menú de la cuenta de almacenamiento](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account.png)

1. Anote el valor de la **clave** **clave1**. (Al seleccionar el icono a la derecha de la clave, se copia el valor al portapapeles).

    ![Claves de acceso de cuenta de almacenamiento](./media/terraform-k8s-cluster-appgw-with-tf-aks/storage-account-access-key.png)

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

    ![Ejemplo de resultados de "terraform init"](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-init-complete.png)

1. Cree un archivo de variables para proporcionar valores de entrada; en Cloud Shell, cree un archivo denominado `main.tf`.

    ```bash
    vi terraform.tfvars
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue las siguientes variables que creó anteriormente en el editor:

    ```JSON
      resource_group_name = <Name of the Resource Group already created>

      location = <Location of the Resource Group>
        
      aks_service_principal_app_id = <Service Principal AppId>
        
      aks_service_principal_client_secret = <Service Principal Client Secret>
        
      aks_service_principal_object_id = <Service Principal Object Id>
        
    ```

1. Seleccione la tecla **Esc** para acceder al modo de inserción.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

1. Ejecute el comando `terraform plan` para crear el plan de Terraform que define los elementos de infraestructura. 

    ```bash
    terraform plan -out out.plan
    ```

    El comando `terraform plan` muestra los recursos que se crearán cuando ejecute el comando `terraform apply`:

    ![Ejemplo de resultados de "terraform plan"](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-plan-complete.png)

1. Ejecute el comando `terraform apply` para aplicar el plan para crear el clúster de Kubernetes. El proceso para crear un clúster de Kubernetes puede tardar varios minutos, dado como resultado el agotamiento del tiempo de espera de la sesión de Cloud Shell. Si la sesión de Cloud Shell agota su tiempo de espera, puede seguir los pasos de la sección "Recuperación de un tiempo de espera de Cloud Shell" para poder completar el tutorial.

    ```bash
    terraform apply out.plan
    ```

    El comando `terraform apply` muestra los resultados de la creación de los recursos definidos en sus archivos de configuración:

    ![Ejemplo de resultados de "terraform apply"](./media/terraform-k8s-cluster-appgw-with-tf-aks/terraform-apply-complete.png)

1. En Azure Portal, seleccione **Grupos de recursos** en el menú izquierdo para ver los recursos creados para el nuevo clúster de Kubernetes en el grupo de recursos seleccionado.

    ![Aviso de Cloud Shell](./media/terraform-k8s-cluster-appgw-with-tf-aks/k8s-resources-created.png)

## <a name="recover-from-a-cloud-shell-timeout"></a>Recuperación de un tiempo de espera de Cloud Shell
Si se agota el tiempo de espera de la sesión de Cloud Shell, puede realizar los siguientes pasos para recuperarla:

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

    ![La herramienta kubectl le permite comprobar el mantenimiento de su clúster de Kubernetes](./media/terraform-k8s-cluster-appgw-with-tf-aks/kubectl-get-nodes.png)


## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a utilizar Terraform y AKS para crear un clúster de Kubernetes. Aquí tiene algunos recursos adicionales donde encontrará más información acerca de Terraform en Azure:
 
 > [!div class="nextstepaction"] 
 > [Centro de Terraform en Microsoft.com](https://docs.microsoft.com/azure/terraform/)
 
