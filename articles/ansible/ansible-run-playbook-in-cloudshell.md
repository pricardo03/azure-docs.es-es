---
title: Ejecución de Ansible con Bash en Azure Cloud Shell
description: Información acerca de cómo realizar diversas tareas de Ansible con Bash en Azure Cloud Shell
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 08/07/2018
ms.topic: quickstart
ms.openlocfilehash: 61b23b5bc2620c82051b0ba1de4bb54b44a495e0
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359194"
---
# <a name="run-ansible-with-bash-in-azure-cloud-shell"></a>Ejecución de Ansible con Bash en Azure Cloud Shell

En este tutorial, aprenderá a usar Bash en Cloud Shell para configurar una suscripción a Azure como área de trabajo de Ansible. 

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: si no tiene una suscripción de Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Configuración de Azure Cloud Shell**: si es la primera vez que usa Azure Cloud Shell, el artículo [Guía de inicio rápido para Bash en Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) muestra cómo iniciar y configurar Cloud Shell. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Configuración automática de credenciales

Cuando se inicia sesión en Cloud Shell, Ansible se autentica con Azure para administrar la infraestructura sin ninguna configuración adicional. Si tiene más de una suscripción, puede elegir con qué suscripción debe funcionar Ansible exportando la variable de entorno `AZURE_SUBSCRIPTION_ID`. Para obtener una lista de las suscripciones a Azure, ejecute el siguiente comando:

```azurecli-interactive
az account list
```

Con la **id** de la suscripción con la que quiere trabajar, establezca **AZURE_SUBSCRIPTION_ID** como sigue:

```azurecli-interactive
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Comprobar la configuración
Para comprobar si la configuración es correcta, use Ansible para crear un grupo de recursos.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Creación de una máquina virtual básica en Azure con Ansible](/azure/virtual-machines/linux/ansible-create-vm)