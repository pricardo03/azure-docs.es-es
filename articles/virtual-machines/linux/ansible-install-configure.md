---
title: Instalación de Ansible en máquinas virtuales de Azure
description: Obtenga información sobre cómo instalar Ansible y configurarlo para administrar recursos de Azure en Ubuntu, CentOS y SLES.
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: fcb2da93a39bd4e1a81f7767dc40b3a24fd7d213
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "40250663"
---
# <a name="install-ansible-on-azure-virtual-machines"></a>Instalación de Ansible en máquinas virtuales de Azure

Ansible permite automatizar la implementación y la configuración de recursos en un entorno. Puede usar Ansible para administrar máquinas virtuales (VM) en Azure al igual que podría hacerlo con cualquier otro recurso. En este artículo se indica cómo instalar Ansible y los módulos de SDK de Python de Azure para algunas de las distribuciones más habituales de Linux. Es posible instalar Ansible en otras distribuciones ajustando los paquetes instalados para adaptarlos a la plataforma en concreto. Para crear recursos de Azure de forma segura, también obtendrá información sobre cómo crear credenciales y definirlos para uso en Ansible. Para una lista de las herramientas adicionales disponibles en Cloud Shell, consulte las [Características y herramientas de Bash en Azure Cloud Shell](../../cloud-shell/features.md#tools).

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: si no tiene una suscripción de Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Acceso a Linux o a una máquina virtual Linux**: si no tiene una máquina Linux, cree una [máquina virtual Linux](/virtual-machines/linux/quick-create-cli.md).

- **Entidad de servicio de Azure**: siga las instrucciones de la sección **Creación de la entidad de servicio** del artículo [Creación de una entidad de servicio de Azure con la CLI de Azure 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal). Tome nota de los valores de **appId**, **displayName**, **password** y **tenant**.

## <a name="install-ansible-on-an-azure-linux-virtual-machine"></a>Instalación de Ansible en una máquina virtual Linux de Azure

Inicie sesión en su equipo Linux y seleccione una de las distribuciones siguientes para obtener información sobre cómo instalar Ansible:

- [CentOS 7.4](#centos-74)
- [Ubuntu 16.04 LTS](#ubuntu1604-lts)
- [SLES 12 SP2](#sles-12-sp2)

### <a name="centos-74"></a>CentOS 7.4

Escriba los siguientes comandos en una ventana de terminal o de Bash para instalar los paquetes necesarios para los módulos del SDK de Python de Azure y Ansible:

```bash
## Install pre-requisite packages
sudo yum check-update; sudo yum install -y gcc libffi-devel python-devel openssl-devel epel-release
sudo yum install -y python-pip python-wheel

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Siga las instrucciones descritas en la sección [Creación de credenciales de Azure](#create-azure-credentials).

### <a name="ubuntu-1604-lts"></a>Ubuntu 16.04 LTS

Escriba los siguientes comandos en una ventana de terminal o de Bash para instalar los paquetes necesarios para los módulos del SDK de Python de Azure y Ansible:


```bash
## Install pre-requisite packages
sudo apt-get update && sudo apt-get install -y libssl-dev libffi-dev python-dev python-pip

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]
```

Siga las instrucciones descritas en la sección [Creación de credenciales de Azure](#create-azure-credentials).

### <a name="sles-12-sp2"></a>SLES 12 SP2

Escriba los siguientes comandos en una ventana de terminal o de Bash para instalar los paquetes necesarios para los módulos del SDK de Python de Azure y Ansible:

```bash
## Install pre-requisite packages
sudo zypper refresh && sudo zypper --non-interactive install gcc libffi-devel-gcc5 make \
    python-devel libopenssl-devel libtool python-pip python-setuptools

## Install Ansible and Azure SDKs via pip
sudo pip install ansible[azure]

# Remove conflicting Python cryptography package
sudo pip uninstall -y cryptography
```

Siga las instrucciones descritas en la sección [Creación de credenciales de Azure](#create-azure-credentials).

## <a name="create-azure-credentials"></a>Creación de credenciales de Azure

La combinación del identificador de suscripción y la información devuelta en la creación de la entidad de servicio se utiliza para configurar las credenciales de Ansible de una de estas dos maneras:

- [Creación de un archivo de credenciales de Ansible](#file-credentials)
- [Uso de variables de entorno de Ansible](#env-credentials)

Si va a usar herramientas como Ansible Tower o Jenkins, deberá usar la opción de declarar los valores de la entidad de servicio como variables de entorno.

### <a name="span-idfile-credentials-create-ansible-credentials-file"></a><span id="file-credentials"/> Creación de un archivo de credenciales de Ansible

En esta sección se explica cómo crear un archivo de credenciales local para proporcionar credenciales a Ansible. Para obtener más información sobre cómo definir credenciales de Ansible, consulte [Providing Credentials to Azure Modules](https://docs.ansible.com/ansible/guide_azure.html#providing-credentials-to-azure-modules) (Provisión de credenciales a módulos de Azure).

Para entornos de desarrollo, cree un archivo *credentials* para Ansible en la máquina virtual host de la siguiente forma:

```bash
mkdir ~/.azure
vi ~/.azure/credentials
```

Inserte las líneas siguientes en el archivo *credentials*: reemplace los marcadores de posición por la información de la creación de la entidad de servicio.

```bash
[default]
subscription_id=<your-subscription_id>
client_id=<security-principal-appid>
secret=<security-principal-password>
tenant=<security-principal-tenant>
```

Guarde y cierre el archivo.

### <a name="span-idenv-credentialsuse-ansible-environment-variables"></a><span id="env-credentials"/> Uso de variables de entorno de Ansible

En esta sección se explica cómo configurar las credenciales de Ansible exportándolas como variables de entorno.

Escriba los siguientes comandos en una ventana de terminal o de Bash:

```bash
export AZURE_SUBSCRIPTION_ID=<your-subscription_id>
export AZURE_CLIENT_ID=<security-principal-appid>
export AZURE_SECRET=<security-principal-password>
export AZURE_TENANT=<security-principal-tenant>
```

## <a name="verify-the-configuration"></a>Comprobar la configuración
Para comprobar si la configuración es correcta, ahora puede usar Ansible para crear un grupo de recursos.

[!INCLUDE [create-resource-group-with-ansible.md](../../../includes/ansible-create-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"] 
> [Uso de Ansible para crear una máquina virtual Linux en Azure](./ansible-create-vm.md)