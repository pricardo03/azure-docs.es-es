---
title: 'Guía de inicio rápido: Creación de una máquina virtual Linux en Azure Portal | Microsoft Docs'
description: En esta guía de inicio rápido aprenderá a usar Azure Portal para crear una máquina virtual Linux.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/12/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 693f9144d1cb454b0a9dd98b5ae63938abd7d26d
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50420411"
---
# <a name="quickstart-create-a-linux-virtual-machine-in-the-azure-portal"></a>Guía de inicio rápido: Creación de una máquina virtual Linux en Azure Portal

Las máquinas virtuales de Azure pueden crearse mediante Azure Portal. Azure Portal es una interfaz de usuario basada en explorador para crear máquinas virtuales y recursos asociados. En esta guía de inicio rápido se muestra cómo usar Azure Portal para implementar una máquina virtual Linux en Azure que ejecuta Ubuntu 16.04 LTS. Para ver la máquina virtual en acción, conéctese a ella mediante SSH e instale el servidor web NGINX.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-ssh-key-pair"></a>Creación del par de claves SSH

Necesita un par de claves SSH para completar esta guía de inicio rápido. Si ya tiene un par de claves SSH, puede omitir este paso.

Abra un shell de Bash y utilice [ssh-keygen](https://www.ssh.com/ssh/keygen/) para crear un par de claves SSH. Si no tiene un shell de Bash en la máquina local, puede usar [Azure Cloud Shell](https://shell.azure.com/bash).  

```bash
ssh-keygen -t rsa -b 2048
```

El comando anterior genera claves públicas y privadas con el nombre predeterminado `id_rsa` en `~/.ssh directory`. El comando devuelve la ruta de acceso completa a la clave pública. Utilice la ruta de acceso a la clave pública para mostrar su contenido con `cat`.

```bash 
cat ~/.ssh/id_rsa.pub
```

Guarde la salida de este comando. Lo necesitará al configurar la cuenta de administrador para iniciar sesión en la máquina virtual.

Para más información sobre cómo crear pares de claves SSH, incluido el uso de PuTTy, consulte [Uso de claves SSH con Windows](ssh-from-windows.md).

Si crea el par de claves SSH mediante Cloud Shell, se almacenará en un recurso compartido de archivos de Azure que [Cloud Shell monta automáticamente](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage). No elimine esta cuenta de almacenamiento ni el recurso compartido de archivos hasta que haya recuperado las claves o perderá el acceso a la máquina virtual. 

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en el [Azure Portal](https://portal.azure.com).

## <a name="create-virtual-machine"></a>Crear máquina virtual

1. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.

1. En el cuadro de búsqueda encima de la lista de recursos de Azure Marketplace, busque y seleccione **Ubuntu Server 16.04 LTS** de Canonical y, a continuación, elija **Crear**.

1. En la pestaña **Aspectos básicos**, en **Detalles del proyecto**, asegúrese de que esté seleccionada la suscripción correcta y luego elija **Crear nuevo** en **Grupo de recursos**. En el menú emergente, escriba *myResourceGroup* para el nombre del grupo de recursos y luego elija **Aceptar**. 

    ![Crear un nuevo grupo de recursos para la máquina virtual](./media/quick-create-portal/project-details.png)

1. En **Detalles de instancia**, escriba *myVM* en **Nombre de máquina virtual** y elija *Este de EE. UU.* como **Región**. Deje los demás valores predeterminados.

    ![Sección Detalles de instancia](./media/quick-create-portal/instance-details.png)

1. En **Cuenta de administrador**, seleccione **Clave pública SSH**, escriba el nombre de usuario y luego pegue la clave pública en el cuadro de texto. Quite los espacios en blanco finales o iniciales de la clave pública.

    ![Cuenta de administrador](./media/quick-create-portal/administrator-account.png)

1. En **Reglas de puerto de entrada** > **Puertos de entrada públicos**, elija **Permitir los puertos seleccionados** y luego seleccione **SSH (22)** y **HTTP (80)** en la lista desplegable. 

    ![Puertos abiertos para RDP y HTTP](./media/quick-create-portal/inbound-port-rules.png)

1. Deje los valores predeterminados restantes y luego seleccione el botón **Revisar + crear** en la parte inferior de la página.

1. En la página **Crear una máquina virtual** verá los detalles de la máquina virtual que va a crear. Cuando esté preparado, seleccione **Crear**.

La implementación de la máquina virtual tardará unos minutos. Cuando finalice la implementación, pase a la siguiente sección.

    
## <a name="connect-to-virtual-machine"></a>Conexión a la máquina virtual

Cree una conexión SSH con la máquina virtual.

1. Seleccione el botón **Conectar** en la página de información general de la máquina virtual. 

    ![Portal 9](./media/quick-create-portal/portal-quick-start-9.png)

2. En la página **Conectarse a una máquina virtual**, mantenga las opciones predeterminadas para conectarse por la dirección IP a través del puerto 22. En **Login using VM local account** (Iniciar sesión mediante la cuenta local de máquina virtual), se muestra un comando de conexión. Haga clic en el botón para copiar el comando. En el ejemplo siguiente se muestra el aspecto que tiene el comando de conexión SSH:

    ```bash
    ssh azureuser@10.111.12.123
    ```

3. Con el mismo shell de Bash que usó para crear el par de claves SSH (como [Azure Cloud Shell](https://shell.azure.com/bash) o el shell de Bash local) pegue el comando de conexión de SSH en el shell para crear una sesión de SSH. 

## <a name="install-web-server"></a>Instalación del servidor web

Para ver la máquina virtual en acción, instale al servidor de web de NGINX. En la sesión de SSH, actualice los orígenes de paquete e instale el paquete NGINX más reciente.

```bash
sudo apt-get -y update
sudo apt-get -y install nginx
```

Cuando haya terminado, escriba `exit` para salir de la sesión de SSH.


## <a name="view-the-web-server-in-action"></a>Visualización del servidor web en acción

Use un explorador web de su elección para ver la página de bienvenida predeterminada de NGINX. Escriba la dirección IP pública de la máquina virtual como dirección web. La dirección IP pública se encuentra en la página de introducción de la máquina virtual o como parte de la cadena de conexión de SSH que usó anteriormente.

![Sitio NGINX predeterminado](./media/quick-create-cli/nginx.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede eliminar el grupo de recursos, la máquina virtual y todos los recursos relacionados. Para ello, seleccione el grupo de recursos de la máquina virtual, seleccione **Eliminar** y luego confirme el nombre del grupo de recursos para eliminar.

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, implementó una máquina virtual sencilla, creó un grupo de seguridad de red y una regla e instaló un servidor web básico. Para más información acerca de las máquinas virtuales de Azure, continúe con el tutorial de máquinas virtuales Linux.

> [!div class="nextstepaction"]
> [Tutoriales de máquinas virtuales Linux de Azure](./tutorial-manage-vm.md)
