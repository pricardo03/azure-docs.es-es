---
title: Creación de una red virtual del concentrador con Terraform en Azure
description: Tutorial que muestra cómo crear una red virtual del concentrador en Azure que sirva como punto de conexión común entre otras redes
services: terraform
ms.service: azure
keywords: terraform, hub-and-spoke, redes, redes híbridas, devops, máquina virtual, azure, emparejamiento VNet, hub-spoke, hub.
author: VaijanathB
manager: jeconnoc
ms.author: vaangadi
ms.topic: tutorial
ms.date: 09/20/2019
ms.openlocfilehash: 12538c0348efc1621d3f8f6ee0cb93d73c712898
ms.sourcegitcommit: f2771ec28b7d2d937eef81223980da8ea1a6a531
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2019
ms.locfileid: "71173429"
---
# <a name="tutorial-create-a-hub-virtual-network-with-terraform-in-azure"></a>Tutorial: Creación de una red virtual del concentrador con Terraform en Azure

La red virtual del concentrador sirve como el punto de conectividad central a la red local. La red virtual hospeda los servicios compartidos que consumen las cargas de trabajo hospedadas en las redes virtuales de radio. Para la demostración, no se implementarán servicios compartidos en este tutorial.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Uso de HCL (HashiCorp Language) para implementar la red virtual del concentrador en topología en estrella tipo hub-and-spoke
> * Uso de Terraform para crear una máquina virtual de JumpBox de concentrador
> * Uso de Terraform para crear una puerta de enlace de red privada virtual de concentrador
> * Use Terraform para crear conexiones de puerta de enlace locales y de concentrador

## <a name="prerequisites"></a>Requisitos previos

1. [Creación de una topología de red híbrida en estrella tipo hub-and-spoke con Terraform en Azure](./terraform-hub-spoke-introduction.md).
1. [Creación de una red virtual local con Terraform en Azure](./terraform-hub-spoke-on-prem.md).

## <a name="create-the-directory-structure"></a>Creación de la estructura de directorios

La red de concentrador consta de estos componentes:

- Red virtual de concentrador
- Puerta de enlace de red virtual de concentrador
- Conexiones de puerta de enlace de concentrador 

Los recursos se definen en el archivo de configuración de Terraform siguientes:

1. Vaya a [Azure Portal](https://portal.azure.com).

1. Abra [Azure Cloud Shell](/azure/cloud-shell/overview). Si no seleccionó un entorno previamente, seleccione **Bash** como entorno.

    ![Aviso de Cloud Shell](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. Cambie al directorio `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Cambie los directorios al nuevo directorio:

    ```bash
    cd hub-spoke
    ```

## <a name="declare-the-hub-vnet"></a>Declaración de la red virtual de concentrador

Cree el archivo de configuración de Terraform que declara la red virtual de concentrador.

1. En Cloud Shell, cree un archivo denominado `hub-vnet.tf`.

    ```bash
    code hub-vnet.tf
    ```

1. Pegue el siguiente código en el editor:

    ```hcl
    locals {
      prefix-hub         = "hub"
      hub-location       = "CentralUS"
      hub-resource-group = "hub-vnet-rg"
      shared-key         = "4-v3ry-53cr37-1p53c-5h4r3d-k3y"
    }

    resource "azurerm_resource_group" "hub-vnet-rg" {
      name     = "${local.hub-resource-group}"
      location = "${local.hub-location}"
    }

    resource "azurerm_virtual_network" "hub-vnet" {
      name                = "${local.prefix-hub}-vnet"
      location            = "${azurerm_resource_group.hub-vnet-rg.location}"
      resource_group_name = "${azurerm_resource_group.hub-vnet-rg.name}"
      address_space       = ["10.0.0.0/16"]

      tags {
        environment = "hub-spoke"
      }
    }

    resource "azurerm_subnet" "hub-gateway-subnet" {
      name                 = "GatewaySubnet"
      resource_group_name  = "${azurerm_resource_group.hub-vnet-rg.name}"
      virtual_network_name = "${azurerm_virtual_network.hub-vnet.name}"
      address_prefix       = "10.0.255.224/27"
    }

    resource "azurerm_subnet" "hub-mgmt" {
      name                 = "mgmt"
      resource_group_name  = "${azurerm_resource_group.hub-vnet-rg.name}"
      virtual_network_name = "${azurerm_virtual_network.hub-vnet.name}"
      address_prefix       = "10.0.0.64/27"
    }

    resource "azurerm_subnet" "hub-dmz" {
      name                 = "dmz"
      resource_group_name  = "${azurerm_resource_group.hub-vnet-rg.name}"
      virtual_network_name = "${azurerm_virtual_network.hub-vnet.name}"
      address_prefix       = "10.0.0.32/27"
    }

    resource "azurerm_network_interface" "hub-nic" {
      name                 = "${local.prefix-hub}-nic"
      location             = "${azurerm_resource_group.hub-vnet-rg.location}"
      resource_group_name  = "${azurerm_resource_group.hub-vnet-rg.name}"
      enable_ip_forwarding = true

      ip_configuration {
        name                          = "${local.prefix-hub}"
        subnet_id                     = "${azurerm_subnet.hub-mgmt.id}"
        private_ip_address_allocation = "Dynamic"
      }

      tags {
        environment = "${local.prefix-hub}"
      }
    }

    #Virtual Machine
    resource "azurerm_virtual_machine" "hub-vm" {
      name                  = "${local.prefix-hub}-vm"
      location              = "${azurerm_resource_group.hub-vnet-rg.location}"
      resource_group_name   = "${azurerm_resource_group.hub-vnet-rg.name}"
      network_interface_ids = ["${azurerm_network_interface.hub-nic.id}"]
      vm_size               = "${var.vmsize}"

      storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04-LTS"
        version   = "latest"
      }

      storage_os_disk {
        name              = "myosdisk1"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Standard_LRS"
      }

      os_profile {
        computer_name  = "${local.prefix-hub}-vm"
        admin_username = "${var.username}"
        admin_password = "${var.password}"
      }

      os_profile_linux_config {
        disable_password_authentication = false
      }

      tags {
        environment = "${local.prefix-hub}"
      }
    }

    # Virtual Network Gateway
    resource "azurerm_public_ip" "hub-vpn-gateway1-pip" {
      name                = "hub-vpn-gateway1-pip"
      location            = "${azurerm_resource_group.hub-vnet-rg.location}"
      resource_group_name = "${azurerm_resource_group.hub-vnet-rg.name}"

      allocation_method = "Dynamic"
    }

    resource "azurerm_virtual_network_gateway" "hub-vnet-gateway" {
      name                = "hub-vpn-gateway1"
      location            = "${azurerm_resource_group.hub-vnet-rg.location}"
      resource_group_name = "${azurerm_resource_group.hub-vnet-rg.name}"

      type     = "Vpn"
      vpn_type = "RouteBased"

      active_active = false
      enable_bgp    = false
      sku           = "VpnGw1"

      ip_configuration {
        name                          = "vnetGatewayConfig"
        public_ip_address_id          = "${azurerm_public_ip.hub-vpn-gateway1-pip.id}"
        private_ip_address_allocation = "Dynamic"
        subnet_id                     = "${azurerm_subnet.hub-gateway-subnet.id}"
      }
      depends_on = ["azurerm_public_ip.hub-vpn-gateway1-pip"]
    }

    resource "azurerm_virtual_network_gateway_connection" "hub-onprem-conn" {
      name                = "hub-onprem-conn"
      location            = "${azurerm_resource_group.hub-vnet-rg.location}"
      resource_group_name = "${azurerm_resource_group.hub-vnet-rg.name}"

      type           = "Vnet2Vnet"
      routing_weight = 1

      virtual_network_gateway_id      = "${azurerm_virtual_network_gateway.hub-vnet-gateway.id}"
      peer_virtual_network_gateway_id = "${azurerm_virtual_network_gateway.onprem-vpn-gateway.id}"

      shared_key = "${local.shared-key}"
    }

    resource "azurerm_virtual_network_gateway_connection" "onprem-hub-conn" {
      name                = "onprem-hub-conn"
      location            = "${azurerm_resource_group.onprem-vnet-rg.location}"
      resource_group_name = "${azurerm_resource_group.onprem-vnet-rg.name}"
      type                            = "Vnet2Vnet"
      routing_weight = 1
      virtual_network_gateway_id      = "${azurerm_virtual_network_gateway.onprem-vpn-gateway.id}"
      peer_virtual_network_gateway_id = "${azurerm_virtual_network_gateway.hub-vnet-gateway.id}"

      shared_key = "${local.shared-key}"
    }
    ```
    
1. Guarde el archivo y salga del editor.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Create a hub virtual network appliance with Terraform in Azure](./terraform-hub-spoke-hub-nva.md) (Creación de un dispositivo de red virtual de concentrador con Terraform en Azure)