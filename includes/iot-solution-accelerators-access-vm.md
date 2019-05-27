---
title: archivo de inclusión
description: archivo de inclusión
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/16/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: db1af4f046bd8849fddee299e949d6edbdaae86a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66143388"
---
## <a name="access-the-virtual-machine"></a>Acceso a la máquina virtual

En los siguientes pasos, use el comando `az` en Azure Cloud Shell. Si lo prefiere, también puede [Instalar la CLI 2.0 de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) al desarrollar la máquina, y ejecutar los comandos de forma local.

En los pasos siguientes se muestra cómo configurar la máquina virtual de Azure para permitir el acceso de **SSH**. En los pasos que se muestran se asume que el nombre que eligió para el acelerador de soluciones es **contoso-simulation**; reemplace este valor con el nombre de la implementación:

1. Lista de contenidos del grupo de recursos que contiene los recursos del acelerador de soluciones:

    ```azurecli-interactive
    az resource list -g contoso-simulation -o table
    ```

    Tome nota del nombre de la máquina virtual, la dirección IP pública y el grupo de seguridad de red; necesitará estos valores más adelante.

1. Actualice el grupo de seguridad de red para permitir el acceso de SSH. En el comando siguiente se da por supuesto que el nombre del grupo de seguridad de red es **contoso-simulation-nsg**; reemplace este valor con el nombre de su grupo de seguridad de red:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name contoso-simulation-nsg -g contoso-simulation --access Allow -o table
    ```

    Solo se debe habilitar el acceso de SSH durante el desarrollo y las pruebas. Si habilita SSH, [debe deshabilitarlo nuevamente tan pronto como sea posible](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices#disable-rdpssh-access-to-virtual-machines).

1. Actualice la contraseña de la cuenta **azureuser** en la máquina virtual, con una contraseña que conozca. Elija su propia contraseña cuando ejecute el siguiente comando:

    ```azurecli-interactive
    az vm user update --name vm-vikxv --username azureuser --password YOURSECRETPASSWORD  -g contoso-simulation
    ```

1. Busque la dirección IP pública de la máquina virtual. En el comando siguiente se da por supuesto que el nombre de la máquina virtual es **vm vikxv**; reemplace este valor con el nombre de la máquina virtual que anotó anteriormente:

    ```azurecli-interactive
    az vm list-ip-addresses --name vm-vikxv -g contoso-simulation -o table
    ```

    Anote la dirección IP pública de la máquina virtual.
