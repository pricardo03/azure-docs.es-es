---
title: Uso de Terraform para crear un conjunto de escalado de máquinas virtuales de Azure
description: Tutorial sobre el uso de Terraform para configurar y controlar las versiones de un conjunto de escalado de máquinas virtuales de Azure junto con una red virtual y discos asociados administrados
services: terraform
ms.service: azure
keywords: terraform, devops, maquina virtual, Azure, conjunto de escalado, red, almacenamiento, módulos
author: tomarchermsft
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: a6bc0879d07cadc6c5b0b1a21b11b3075ec69719
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71169878"
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale-set"></a>Uso de Terraform para crear un conjunto de escalado de máquinas virtuales de Azure

[Los conjuntos de escalado de máquinas virtuales de Azure](/azure/virtual-machine-scale-sets) permiten crear y administrar un grupo de máquinas virtuales idénticas y con equilibrado de carga en las que el número de instancias de máquinas virtuales puede aumentar o disminuir automáticamente en respuesta a la demanda o a una programación definida.

En este tutorial, obtendrá información acerca de cómo usar [Azure Cloud Shell](/azure/cloud-shell/overview) para realizar las tareas siguientes:

> [!div class="checklist"]
> * Configurar una implementación de Terraform
> * Usar variables y salidas para la implementación de Terraform
> * Crear e implementar la infraestructura de red
> * Crear e implementar un conjunto de escalado de máquinas virtuales y asociarlo a la red
> * Crear e implementar un Jumpbox para conectarse a las máquinas virtuales a través de SSH

> [!NOTE]
> La última versión de los archivos de configuración de Terraform usados en este artículo se encuentran en el [repositorio de Awesome Terraform en GitHub](https://github.com/Azure/awesome-terraform/tree/master/codelab-vmss).

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.

- **Instalación de Terraform**: siga las instrucciones del artículo [Instalación y configuración de Terraform para aprovisionar máquinas virtuales y otras infraestructuras en Azure](/azure/virtual-machines/linux/terraform-install-configure).

- **Creación de un par de claves SSH**: si aún no tiene un par de claves SSH, siga las instrucciones que aparecen en el artículo [Pasos rápidos: Creación y uso de un par de claves pública-privada SSH para máquinas virtuales Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys).

## <a name="create-the-directory-structure"></a>Creación de la estructura de directorios

1. Vaya a [Azure Portal](https://portal.azure.com).

1. Abra [Azure Cloud Shell](/azure/cloud-shell/overview). Si no seleccionó un entorno previamente, seleccione **Bash** como entorno.

    ![Aviso de Cloud Shell](./media/terraform-create-vm-scaleset-network-disks-hcl/azure-portal-cloud-shell-button-min.png)

1. Cambie al directorio `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Cree un directorio llamado `vmss`.

    ```bash
    mkdir vmss
    ```

1. Cambie los directorios al nuevo directorio:

    ```bash
    cd vmss
    ```

## <a name="create-the-variables-definitions-file"></a>Creación del archivo de definiciones de variables
En esta sección, se definen las variables que personalizan los recursos creados por Terraform.

En Azure Cloud Shell, realice los pasos siguientes:

1. Cree un archivo llamado `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue el siguiente código en el editor:

   ```hcl
   variable "location" {
    description = "The location where resources will be created"
   }

   variable "tags" {
    description = "A map of the tags to use for the resources that are deployed"
    type        = "map"

    default = {
      environment = "codelab"
    }
   }

   variable "resource_group_name" {
    description = "The name of the resource group in which the resources will be created"
    default     = "myResourceGroup"
   }
   ```

1. Salga del modo de inserción mediante la selección de la tecla Esc.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

## <a name="create-the-output-definitions-file"></a>Creación del archivo de definiciones de salida
En esta sección, va a crear el archivo que describe la salida después de la implementación.

En Azure Cloud Shell, realice los pasos siguientes:

1. Cree un archivo llamado `output.tf`.

    ```bash
    vi output.tf
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue el código siguiente en el editor para exponer el nombre de dominio completo (FQDN) para las máquinas virtuales.
   :

   ```hcl
    output "vmss_public_ip" {
        value = "${azurerm_public_ip.vmss.fqdn}"
    }
   ```

1. Salga del modo de inserción mediante la selección de la tecla Esc.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

## <a name="define-the-network-infrastructure-in-a-template"></a>Definición de la infraestructura de red en una plantilla
En esta sección, se crea la siguiente infraestructura de red en un nuevo grupo de recursos de Azure:

  - Una red virtual con el espacio de direcciones de 10.0.0.0/16
  - Una subred con el espacio de direcciones de 10.0.2.0/24
  - Dos direcciones IP públicas. Una la usa el equilibrador de carga del conjunto de escalado de máquinas virtuales y la otra se usa para conectarse al Jumpbox de SSH.

En Azure Cloud Shell, realice los pasos siguientes:

1. Cree un archivo denominado `vmss.tf` para describir la infraestructura del conjunto de escalado de máquinas virtuales.

    ```bash
    vi vmss.tf
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue el código siguiente al final del archivo para exponer el nombre de dominio completo (FQDN) para las máquinas virtuales.

   ```hcl
   resource "azurerm_resource_group" "vmss" {
    name     = "${var.resource_group_name}"
    location = "${var.location}"
    tags     = "${var.tags}"
   }

   resource "random_string" "fqdn" {
    length  = 6
    special = false
    upper   = false
    number  = false
   }

   resource "azurerm_virtual_network" "vmss" {
    name                = "vmss-vnet"
    address_space       = ["10.0.0.0/16"]
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    tags                = "${var.tags}"
   }

   resource "azurerm_subnet" "vmss" {
    name                 = "vmss-subnet"
    resource_group_name  = "${azurerm_resource_group.vmss.name}"
    virtual_network_name = "${azurerm_virtual_network.vmss.name}"
    address_prefix       = "10.0.2.0/24"
   }

   resource "azurerm_public_ip" "vmss" {
    name                         = "vmss-public-ip"
    location                     = "${var.location}"
    resource_group_name          = "${azurerm_resource_group.vmss.name}"
    allocation_method = "Static"
    domain_name_label            = "${random_string.fqdn.result}"
    tags                         = "${var.tags}"
   }
   ```

1. Salga del modo de inserción mediante la selección de la tecla Esc.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

   ```bash
   :wq
   ```

## <a name="provision-the-network-infrastructure"></a>Aprovisionamiento de la infraestructura de red
Con Azure Cloud Shell desde el directorio donde creó los archivos de configuración (.tf), realice los siguientes pasos:

1. Inicialice Terraform.

   ```bash
   terraform init
   ```

1. Ejecute el siguiente comando para implementar la infraestructura definida de Azure.

   ```bash
   terraform apply
   ```

   Terraform le solicita un valor de "ubicación" ya que la variable **location** se define en `variables.tf`, pero nunca se ha establecido. Puede indicar cualquier ubicación válida, por ejemplo, "Oeste de EE. UU.", seguido de la selección de la tecla Entrar. (Use paréntesis alrededor de cualquier valor con espacios).

1. Terraform imprime la salida como se define en el archivo `output.tf`. Como se muestra en la siguiente captura de pantalla, el nombre de dominio completo toma la forma &lt;Id.>.&lt;ubicación>.cloudapp.azure.com. El valor de Id. es un valor calculado y ubicación es el valor que se proporciona al ejecutar Terraform.

   ![Nombre de dominio completo del conjunto de escalado de máquinas virtuales para la dirección IP pública](./media/terraform-create-vm-scaleset-network-disks-hcl/fqdn.png)

1. En el menú principal de Azure Portal, seleccione **Grupos de recursos**.

1. En la pestaña **Grupos de recursos**, seleccione **myResourceGroup** para ver los recursos que se crearon por Terraform.
   ![Recursos de red del conjunto de escalado de máquinas virtuales](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-resources.png)

## <a name="add-a-virtual-machine-scale-set"></a>Adición de un conjunto de escalado de máquinas virtuales

En esta sección, aprenderá a agregar los siguientes recursos a la plantilla:

- Un equilibrador de carga de Azure y reglas para dar servicio a la aplicación y asociarla a la dirección IP pública configurada anteriormente en este artículo.
- Un grupo de direcciones de back-end de Azure, que se asignará al equilibrador de carga.
- Un puerto de sondeo de mantenimiento que usa la aplicación y que se configura en el equilibrador de carga.
- Un conjunto de escalado de máquinas virtuales situado detrás del equilibrador de carga, que se ejecuta en la red virtual implementada anteriormente en este artículo.
- [Nginx](https://nginx.org/) en los nodos de escalado de máquinas virtuales que usan [cloud-init](https://cloudinit.readthedocs.io/en/latest/).

En Cloud Shell, realice los pasos siguientes:

1. Abra el archivo de configuración `vmss.tf`.

   ```bash
   vi vmss.tf
   ```

1. Vaya al final del archivo y acceda al modo Anexar mediante la selección de la tecla A.

1. Pegue el siguiente código al final del archivo:

   ```hcl
   resource "azurerm_lb" "vmss" {
    name                = "vmss-lb"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"

    frontend_ip_configuration {
      name                 = "PublicIPAddress"
      public_ip_address_id = "${azurerm_public_ip.vmss.id}"
    }

    tags = "${var.tags}"
   }

   resource "azurerm_lb_backend_address_pool" "bpepool" {
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id     = "${azurerm_lb.vmss.id}"
    name                = "BackEndAddressPool"
   }

   resource "azurerm_lb_probe" "vmss" {
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id     = "${azurerm_lb.vmss.id}"
    name                = "ssh-running-probe"
    port                = "${var.application_port}"
   }

   resource "azurerm_lb_rule" "lbnatrule" {
      resource_group_name            = "${azurerm_resource_group.vmss.name}"
      loadbalancer_id                = "${azurerm_lb.vmss.id}"
      name                           = "http"
      protocol                       = "Tcp"
      frontend_port                  = "${var.application_port}"
      backend_port                   = "${var.application_port}"
      backend_address_pool_id        = "${azurerm_lb_backend_address_pool.bpepool.id}"
      frontend_ip_configuration_name = "PublicIPAddress"
      probe_id                       = "${azurerm_lb_probe.vmss.id}"
   }

   resource "azurerm_virtual_machine_scale_set" "vmss" {
    name                = "vmscaleset"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    upgrade_policy_mode = "Manual"

    sku {
      name     = "Standard_DS1_v2"
      tier     = "Standard"
      capacity = 2
    }

    storage_profile_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_profile_os_disk {
      name              = ""
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    storage_profile_data_disk {
      lun          = 0
      caching        = "ReadWrite"
      create_option  = "Empty"
      disk_size_gb   = 10
    }

    os_profile {
      computer_name_prefix = "vmlab"
      admin_username       = "${var.admin_user}"
      admin_password       = "${var.admin_password}"
      custom_data          = "${file("web.conf")}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    network_profile {
      name    = "terraformnetworkprofile"
      primary = true

      ip_configuration {
        name                                   = "IPConfiguration"
        subnet_id                              = "${azurerm_subnet.vmss.id}"
        load_balancer_backend_address_pool_ids = ["${azurerm_lb_backend_address_pool.bpepool.id}"]
        primary = true
      }
    }

    tags = "${var.tags}"
   }
   ```

1. Salga del modo de inserción mediante la selección de la tecla Esc.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

1. Cree un archivo llamado `web.conf` que sirva como configuración de cloud-init para las máquinas virtuales que forman parte del conjunto de escalado.

    ```bash
    vi web.conf
    ```

1. Seleccione la tecla I para acceder al modo de inserción.

1. Pegue el siguiente código en el editor:

   ```hcl
   #cloud-config
   packages:
    - nginx
   ```

1. Salga del modo de inserción mediante la selección de la tecla Esc.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

     ```bash
     :wq
     ```

1. Abra el archivo de configuración `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Vaya al final del archivo y acceda al modo Anexar mediante la selección de la tecla A.

1. Personalice la implementación y pegue el código siguiente al final del archivo:

    ```hcl
    variable "application_port" {
       description = "The port that you want to expose to the external load balancer"
       default     = 80
    }

    variable "admin_user" {
       description = "User name to use as the admin account on the VMs that will be part of the VM Scale Set"
       default     = "azureuser"
    }

    variable "admin_password" {
       description = "Default password for admin account"
    }
    ```

1. Salga del modo de inserción mediante la selección de la tecla Esc.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

     ```bash
     :wq
     ```

1. Cree un plan de Terraform para visualizar la implementación del conjunto de escalado de máquinas virtuales. (Debe especificar una contraseña de su elección, así como la ubicación de los recursos).

    ```bash
    terraform plan
    ```

    La salida de este comando debe ser similar a la de la captura de pantalla siguiente:

    ![Salida de la creación del conjunto de escalado de máquinas virtuales](./media/terraform-create-vm-scaleset-network-disks-hcl/add-mvss-plan.png)

1. Implemente los nuevos recursos en Azure.

    ```bash
    terraform apply
    ```

    La salida de este comando debe ser similar a la de la captura de pantalla siguiente:

    ![Grupo de recursos del conjunto de escalado de máquinas virtuales de Terraform](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents.png)

1. Abra un explorador y conéctese al FQDN que devolvió el comando.

    ![Resultados de la exploración del FQDN](./media/terraform-create-vm-scaleset-network-disks-hcl/browser-fqdn.png)

## <a name="add-an-ssh-jumpbox"></a>Adición de un JumpBox SSH
Un *JumpBox* SSH es un servidor único por el que se "salta" para acceder a otros servidores de la red. En este paso, puede configurar los siguientes recursos:

- Una interfaz de red (o JumpBox) conectada a la misma subred que el conjunto de escalado de máquinas virtuales.

- Una máquina virtual conectada con esta interfaz de red. Se puede acceder a este Jumpbox de forma remota. Una vez conectado, puede aplicar SSH a cualquiera de las máquinas virtuales del conjunto de escalado.

1. Abra el archivo de configuración `vmss.tf`.

   ```bash
   vi vmss.tf
   ```

1. Vaya al final del archivo y acceda al modo Anexar mediante la selección de la tecla A.

1. Pegue el siguiente código al final del archivo:

   ```hcl
   resource "azurerm_public_ip" "jumpbox" {
    name                         = "jumpbox-public-ip"
    location                     = "${var.location}"
    resource_group_name          = "${azurerm_resource_group.vmss.name}"
    allocation_method = "Static"
    domain_name_label            = "${random_string.fqdn.result}-ssh"
    tags                         = "${var.tags}"
   }

   resource "azurerm_network_interface" "jumpbox" {
    name                = "jumpbox-nic"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"

    ip_configuration {
      name                          = "IPConfiguration"
      subnet_id                     = "${azurerm_subnet.vmss.id}"
      private_ip_address_allocation = "dynamic"
      public_ip_address_id          = "${azurerm_public_ip.jumpbox.id}"
    }

    tags = "${var.tags}"
   }

   resource "azurerm_virtual_machine" "jumpbox" {
    name                  = "jumpbox"
    location              = "${var.location}"
    resource_group_name   = "${azurerm_resource_group.vmss.name}"
    network_interface_ids = ["${azurerm_network_interface.jumpbox.id}"]
    vm_size               = "Standard_DS1_v2"

    storage_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_os_disk {
      name              = "jumpbox-osdisk"
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    os_profile {
      computer_name  = "jumpbox"
      admin_username = "${var.admin_user}"
      admin_password = "${var.admin_password}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    tags = "${var.tags}"
   }
   ```

1. Abra el archivo de configuración `output.tf`.

   ```bash
   vi output.tf
   ```

1. Vaya al final del archivo y acceda al modo Anexar mediante la selección de la tecla A.

1. Pegue el código siguiente al final del archivo para mostrar el nombre de host del JumpBox cuando finalice la implementación:

   ```hcl
   output "jumpbox_public_ip" {
      value = "${azurerm_public_ip.jumpbox.fqdn}"
   }
   ```

1. Salga del modo de inserción mediante la selección de la tecla Esc.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

1. Implemente el Jumpbox.

   ```bash
   terraform apply
   ```

Una vez completada la implementación, el contenido del grupo de recursos se parecerá al que se muestra en la captura de pantalla siguiente:

![Grupo de recursos del conjunto de escalado de máquinas virtuales de Terraform](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents-final.png)

> [!NOTE]
> La capacidad de iniciar sesión con una contraseña está deshabilitada en el JumpBox y en el conjunto de escalado de máquinas virtuales que ha implementado. Inicie sesión con SSH para acceder a las máquinas virtuales.

## <a name="environment-cleanup"></a>Limpieza de entorno

Para eliminar los recursos de Terraform que se crearon en este tutorial, escriba el siguiente comando en Cloud Shell:

```bash
terraform destroy
```

El proceso de destrucción puede tardar varios minutos en finalizar.

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido cómo usar Terraform para crear un conjunto de escalado de máquinas virtuales de Azure. Aquí tiene algunos recursos adicionales donde encontrará más información acerca de Terraform en Azure:

[Centro de Terraform en Microsoft.com](https://docs.microsoft.com/azure/terraform/)
[Documentación del proveedor de Terraform en Azure](https://aka.ms/terraform)
[Origen del proveedor de Terraform en Azure](https://aka.ms/tfgit)
[Módulos de Terraform en Azure](https://aka.ms/tfmodules)
