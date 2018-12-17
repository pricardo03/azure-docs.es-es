---
title: Uso de Ansible con Azure
description: Introducción al uso de Ansible para automatizar el aprovisionamiento en la nube, la administración de configuración y las implementaciones de aplicaciones.
ms.service: ansible
keywords: ansible, azure, devops, introducción, aprovisionamiento en la nube, administración de configuración, implementación de aplicaciones, módulos de ansible, guiones de procedimientos de ansible
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 09/02/2018
ms.topic: article
ms.openlocfilehash: 22eeb3993cd408a8369236683da3db466a348a30
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52956787"
---
# <a name="ansible-with-azure"></a>Ansible con Azure

[Ansible](https://www.ansible.com) es un producto de código abierto que automatiza el aprovisionamiento en la nube, la administración de configuración y las implementaciones de aplicaciones. Con Ansible puede aprovisionar maquinas virtuales, contenedores e infraestructuras en la nube y de red. Además, Ansible permite automatizar la implementación y la configuración de recursos en un entorno.

Este artículo proporciona una introducción básica a algunas de las ventajas del uso de Ansible con Azure.

## <a name="ansible-playbooks"></a>guiones de procedimientos de Ansible

[Los guiones de procedimientos de Ansible](https://docs.ansible.com/ansible/latest/playbooks.html) son el lenguaje de configuración, implementación y orquestación de Ansible. Pueden describir una directiva que desee que apliquen sus sistemas remotos o un conjunto de pasos de un proceso de TI general. Un guión de procedimientos se crea mediante YAML, que define un modelo de una configuración o un proceso.

## <a name="ansible-modules"></a>Módulos de Ansible

Ansible incluye un conjunto de [módulos de Ansible](https://docs.ansible.com/ansible/latest/modules_by_category.html) que se pueden ejecutar directamente en hosts remotos o mediante [guiones de procedimientos](https://docs.ansible.com/ansible/latest/playbooks.html). Los usuarios también pueden crear sus propios módulos. Los módulos pueden utilizarse para controlar recursos del sistema (como servicios, paquetes o archivos) o para ejecutar comandos del sistema.

Para interactuar con los servicios de Azure, Ansible incluye un conjunto de [módulos de nube de Ansible](https://docs.ansible.com/ansible/list_of_cloud_modules.html#azure) que proporciona las herramientas para crear y organizar su infraestructura de Azure fácilmente. 

## <a name="migrate-existing-workload-to-azure"></a>Migración de la carga de trabajo existente a Azure

Tras usar Ansible para definir la infraestructura, puede aplicar el guión de procedimientos de su aplicación para permitir que Azure escale automáticamente su entorno según sea necesario. 

## <a name="automate-cloud-native-application-in-azure"></a>Automatización de aplicaciones nativas de la nube en Azure

Ansible permite automatizar aplicaciones nativas de la nube en Azure con microservicios de Azure como [Azure Functions](https://azure.microsoft.com//services/functions/) y [Kubernetes en Azure](https://azure.microsoft.com/services/container-service/kubernetes/).  

## <a name="manage-deployments-with-dynamic-inventory"></a>Administración de implementaciones mediante el inventario dinámico
A través de su característica de [inventario dinámico](https://docs.ansible.com/ansible/intro_dynamic_inventory.html), Ansible proporciona la capacidad de extraer el inventario de recursos de Azure. A continuación, puede etiquetar las implementaciones existentes de Azure y administrar esas implementaciones etiquetadas mediante Ansible.

## <a name="additional-azure-marketplace-options"></a>Opciones adicionales de Azure Marketplace
La imagen en Azure Marketplace de [Ansible Tower](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.ansible-tower) de Red Hat ayuda a las organizaciones a escalar la automatización de TI y administrar implementaciones complejas en infraestructuras físicas, virtuales y en la nube. Ansible Tower incluye funcionalidades que proporcionan los niveles adicionales de visibilidad, control, seguridad y eficacia que necesitan las empresas de hoy en día. Ansible Tower cifra las credenciales como claves de Azure y SSH de forma que puede delegar trabajos a empleados con menos experiencia sin el riesgo de exponer las credenciales.

## <a name="ansible-module-and-version-matrix-for-azure"></a>Matriz de módulo y versión de Ansible para Azure
Ansible se suministra con una serie de módulos que se pueden ejecutar directamente en los hosts remotos o mediante guiones de procedimientos.
En [Matriz de módulo y versión de Ansible](./ansible-matrix.md) se enumeran los módulos de Ansible para Azure que pueden aprovisionar recursos en la nube de Azure, como máquinas virtuales, redes y servicios de contenedor. 

## <a name="next-steps"></a>Pasos siguientes
- [Configuración de Ansible](/azure/virtual-machines/linux/ansible-install-configure?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
- [Creación de una máquina virtual Linux](/azure/virtual-machines/linux/ansible-create-vm?toc=%2Fen-us%2Fazure%2Fansible%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json)
