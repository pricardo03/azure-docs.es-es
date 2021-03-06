---
title: 'Inicio rápido: Ejecución de cuadernos de estrategias de Ansible mediante Bash en Azure Cloud Shell'
description: En este inicio rápido aprenderá a realizar diversas tareas de Ansible con Bash en Azure Cloud Shell
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
ms.topic: quickstart
ms.date: 04/30/2019
ms.openlocfilehash: 2a938179cf2e07a61749042db32ef9e1c9d843ba
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2020
ms.locfileid: "78247891"
---
# <a name="quickstart-run-ansible-playbooks-via-bash-in-azure-cloud-shell"></a>Inicio rápido: Ejecución de cuadernos de estrategias de Ansible a través de Bash en Azure Cloud Shell

Azure Cloud Shell es un shell interactivo, accesible desde el explorador, para administrar recursos de Azure. Cloud Shell le permite usar la línea de comandos de Bash o PowerShell. En este artículo, va a usar Bash en Azure Cloud Shell para ejecutar un cuaderno de estrategias de Ansible.

## <a name="prerequisites"></a>Prerrequisitos

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Configuración de Azure Cloud Shell**: si no está familiarizado con Azure Cloud Shell, consulte [Inicio rápido para Bash en Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart).
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Configuración automática de credenciales

Cuando se inicia sesión en Cloud Shell, Ansible se autentica con Azure para administrar la infraestructura sin ninguna configuración adicional. 

Si trabaja con varias suscripciones, especifique la suscripción que usa Ansible exportando la variable de entorno `AZURE_SUBSCRIPTION_ID`. 

Para obtener una lista de las suscripciones a Azure, ejecute el siguiente comando:

```azurecli-interactive
az account list
```

Mediante el identificador de suscripción de Azure, establezca `AZURE_SUBSCRIPTION_ID` como sigue:

```console
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Comprobar la configuración
Para comprobar si la configuración es correcta, use Ansible para crear un grupo de recursos de Azure.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Inicio rápido: Configuración de una máquina virtual en Azure con Ansible](./ansible-create-vm.md)