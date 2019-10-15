---
title: Creación y cifrado de una máquina virtual Windows con Azure Portal
description: En este inicio rápido aprenderá a usar Azure Portal para crear y cifrar una máquina virtual Windows
author: msmbaldwin
ms.author: mbaldwin
ms.service: security
ms.topic: quickstart
ms.date: 10/02/2019
ms.openlocfilehash: 240c0e157d7fcd8b76ee7f42f4c780361df6281d
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72245775"
---
# <a name="quickstart-create-and-encrypt-a-windows-virtual-machine-with-the-azure-portal"></a>Inicio rápido: Creación y cifrado de una máquina virtual Windows desde Azure Portal

Las máquinas virtuales de Azure pueden crearse mediante Azure Portal. Azure Portal es una interfaz de usuario basada en explorador para crear máquinas virtuales y recursos asociados. En este inicio rápido se usará Azure Portal para implementar una máquina virtual Windows que usa Ubuntu 18.04 LTS, crear un almacén de claves para las claves de cifrado y cifrar la máquina virtual.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en el [Azure Portal](https://portal.azure.com).

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

1. Seleccione la opción **Crear un recurso** de la esquina superior izquierda de Azure Portal.
1. En el cuadro de búsqueda, escriba **Key Vault**.
1. En la lista de resultados, seleccione **Key Vault**.
1. En la sección Key Vault, seleccione **Crear**.
1. En la pantalla **Crear almacén de claves**, elija un nombre único para el nuevo almacén de claves.

    > [!Important]
    > Cada instancia de Key Vault debe tener un nombre único. En el siguiente ejemplo se crea una instancia de Key Vault llamada *myADEKV*, pero debe llamar a la suya de otra forma.

1. Seleccione una **suscripción**.
1.  En **Grupo de recursos**, seleccione **Crear nuevo**. En el menú emergente, escriba *myResourceGroup* para el nombre del grupo de recursos y luego elija **Aceptar**. 

    ![Pantalla de creación de grupos de recursos](../media/disk-encryption/portal-qs-keyvaultcreation.png)

1. En el menú desplegable **Ubicación**, elija **Este de EE. UU.**
1. Deje las restantes opciones con sus valores predeterminados.
1. Seleccione "Directivas de acceso", lo que le llevará a una nueva pantalla.
1. Active la casilla que esta junto a "Habilitar el acceso a Azure Disk Encryption para el cifrado de volúmenes".

    ![Pantalla de creación de grupos de recursos](../media/disk-encryption/portal-qs-keyvault-enable-encryption.png)

1. En la parte inferior de la pantalla de directivas de acceso, haga clic en "Revisar y crear".
1. Después de revisar, haga clic en "Crear".

## <a name="create-a-virtual-machine"></a>Creación de una máquina virtual

1. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.

1. En la página Nuevo, en Popular, seleccione **Windows Server 2016 Datacenter**.
1. En la pestaña **Datos básicos**, en **Detalles del proyecto**, asegúrese de que está seleccionada la suscripción correcta.
1. En **Grupo de recursos**, seleccione el grupo de recursos que creó al crear el almacén de claves (p.ej., **myResourceGroup**)
1. En **Nombre de máquina virtual** , escriba *MyVM* y elija 
1. En **Región**, seleccione la misma región que usó al crear el almacén de claves (p.ej., **Este de EE. UU.** ).
1. Asegúrese de que el valor de **Tamaño** es *Estándar D2s v3*.
1. En **Cuenta de administrador** , seleccione **Contraseña**. Escriba un nombre de usuario y una contraseña.
    ![Pantalla de creación de grupos de recursos](../media/disk-encryption/portal-qs-windows-vm-creation.png)
1. Seleccione la pestaña "Administración" y compruebe que tiene una cuenta de almacenamiento de diagnóstico. Si no tiene cuentas de almacenamiento, seleccione "Crear nuevo", asigne un nombre a la cuenta nueva y seleccione "Aceptar" ![Pantalla de creación de grupos de recursos](../media/disk-encryption/portal-qs-vm-creation-storage.png)
1. Haga clic en "Revisar y crear".
1. En la página **Crear una máquina virtual** verá los detalles de la máquina virtual que va a crear. Cuando esté preparado, seleccione **Crear**.

La implementación de la máquina virtual tardará unos minutos. Cuando finalice la implementación, pase a la siguiente sección.

## <a name="encrypt-the-virtual-machine"></a>Cifrado de la máquina virtual

1. Cuando la implementación de la máquina virtual se complete, seleccione **Ir al recurso**.
1. En la barra lateral izquierda, seleccione **Discos**.
1. En la pantalla Discos, seleccione **Cifrado**. 

    ![Selección de discos y de cifrado](../media/disk-encryption/portal-qs-disks-to-encryption.png)

1. En la pantalla de cifrado, en **Disks to encrypt** (Discos que se cifran), elija **OS and data disks** (Discos de SO y de datos).
1. En **Configuración de cifrado**, haga clic en "Seleccionar un almacén de claves y una clave para el cifrado".
1. En la barra lateral derecha, seleccione el nombre del almacén de claves que creó anteriormente como valor para *Almacén de claves** y haga clic en **Seleccionar**.

    ![Selección de discos y de cifrado](../media/disk-encryption/portal-qs-encrypt-vm-screen.png)
1. En la parte superior de la pantalla de cifrado, haga clic en "Guardar". Un elemento emergente le avisará que la máquina virtual se va a reiniciar la máquina virtual. Haga clic en **Sí**.


## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no los necesite, puede eliminar el grupo de recursos, la máquina virtual y todos los recursos relacionados. Para ello, seleccione el grupo de recursos de la máquina virtual, seleccione Eliminar y confirme el nombre del grupo de recursos que desea eliminar.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido ha creado un almacén de claves que se ha habilitado para las claves de cifrado, ha creado una máquina virtual y ha habilitado la máquina virtual para el cifrado.  

> [!div class="nextstepaction"]
> [Introducción a Azure Disk Encryption](disk-encryption-overview.md)