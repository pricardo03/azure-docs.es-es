---
title: Conexión privada a una cuenta de almacenamiento mediante el punto de conexión privado de Azure
description: Aprenda cómo conectarse de forma privada a una cuenta de almacenamiento en Azure mediante un punto de conexión privado.
services: private-link
author: KumudD
ms.service: private-link
ms.topic: article
ms.date: 09/16/2019
ms.author: kumud
ms.openlocfilehash: bb0c6e9d20c12df3532a52df1fe4d9574344d4b3
ms.sourcegitcommit: 1c9858eef5557a864a769c0a386d3c36ffc93ce4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71104726"
---
# <a name="connect-privately-to-a-storage-account-using-azure-private-endpoint"></a>Conexión privada a una cuenta de almacenamiento mediante el punto de conexión privado de Azure
Un punto de conexión privado de Azure es el bloque de creación fundamental para Private Link en Azure. Permite que los recursos de Azure, como las máquinas virtuales, se comuniquen de manera privada con recursos de Private Link.

En este inicio rápido, obtendrá información sobre cómo crear una máquina virtual en una red virtual de Azure, una cuenta de almacenamiento con un punto de conexión privado mediante Azure Portal. Después, puede acceder de forma segura a la cuenta de almacenamiento desde la VM.


## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en https://portal.azure.com.

## <a name="create-a-vm"></a>Crear una VM
En esta sección, va a crear una red virtual y la subred para hospedar la máquina virtual que se usa para acceder al recurso de Private Link (una cuenta de almacenamiento en este ejemplo).

### <a name="create-the-virtual-network"></a>Crear la red virtual

En esta sección, va a crear una red virtual y una subred para hospedar la máquina virtual que se usa para acceder al recurso de Private Link.

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Red virtual**.
1. En **Creación de una red virtual**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | NOMBRE | Escriba *MyVirtualNetwork*. |
    | Espacio de direcciones | Escriba *10.1.0.0/16*. |
    | Subscription | Seleccione su suscripción.|
    | Resource group | Seleccione **Crear nuevo**, escriba *myResourceGroup* y, después, seleccione **Aceptar**. |
    | Location | Seleccione **WestCentralUS**.|
    | Subred: nombre | Escriba *mySubnet*. |
    | Subred: intervalo de direcciones | Escriba *10.1.0.0/24*. |
    |||
1. Deje el resto tal como está y seleccione **Crear**.


### <a name="create-virtual-machine"></a>Crear máquina virtual

1. En la parte superior izquierda de Azure Portal, seleccione **Crear un recurso** > **Proceso** > **Máquina virtual**.

1. En **Creación de una máquina virtual: conceptos básicos**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | **DETALLES DEL PROYECTO** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior.  |
    | **DETALLES DE INSTANCIA** |  |
    | Nombre de la máquina virtual | Escriba *myVm*. |
    | Region | Seleccione **WestCentralUS**. |
    | Opciones de disponibilidad | Deje el valor predeterminado **No se requiere redundancia de la infraestructura**. |
    | Imagen | Seleccione **Windows Server 2019 Datacenter**. |
    | Size | Deje el valor predeterminado **Estándar DS1 v2**. |
    | **CUENTA DE ADMINISTRADOR** |  |
    | Nombre de usuario | Escriba un nombre de usuario de su elección. |
    | Contraseña | Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Confirm Password | Vuelva a escribir la contraseña. |
    | **REGLAS DE PUERTO DE ENTRADA** |  |
    | Puertos de entrada públicos | Deje el valor predeterminado **Ninguno**. |
    | **AHORRE DINERO** |  |
    | ¿Ya tiene una licencia de Windows? | Deje el valor predeterminado **No**. |
    |||

1. Seleccione **Siguiente: Discos**.

1. En **Creación de una máquina virtual: Discos**, deje los valores predeterminados y seleccione **Siguiente: Redes**.

1. En **Creación de una máquina virtual: Redes**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | Virtual network | Deje el valor predeterminado **MyVirtualNetwork**.  |
    | Espacio de direcciones | Deje el valor predeterminado **10.1.0.0/24**.|
    | Subnet | Deje el valor predeterminado **mySubnet (10.1.0.0/24)** .|
    | Dirección IP pública | Deje el valor predeterminado **(new) myVm-ip**. |
    | Puertos de entrada públicos | Seleccione **Permitir los puertos seleccionados**. |
    | Selección de puertos de entrada | Seleccione **HTTP** y **RDP**.|
    ||

1. Seleccione **Revisar + crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración.

1. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**.

## <a name="create-your-private-endpoint"></a>Creación de un punto de conexión privado
En esta sección, creará una cuenta de almacenamiento privada con un punto de conexión privado para ella. 

1. En la parte superior izquierda de la pantalla en Azure Portal, seleccione **Crear un recurso** > **Almacenamiento** > **Cuenta de almacenamiento**.

1. En **Crear cuenta de almacenamiento: Aspectos básicos**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | **DETALLES DEL PROYECTO** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior.|
    | **DETALLES DE INSTANCIA** |  |
    | Nombre de la cuenta de almacenamiento  | Escriba *mystorageaccount*. Si el nombre ya existe, cree uno único. |
    | Region | Seleccione **WestCentralUS**. |
    | Rendimiento| Deje el valor predeterminado **Estándar**. |
    | Tipo de cuenta | Deje el valor predeterminado **Storage (uso general v2)** . |
    | Replicación | Seleccione **Almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS)** . |
    |||
  
3. Seleccione **Siguiente: Redes**.
4. En **Crear una cuenta de almacenamiento: Redes**, método de conectividad, seleccione **Punto de conexión privado**.
5. En **Crear una cuenta de almacenamiento: Redes**, seleccione **Agregar un punto de conexión privado**. 
6. En **Crear un punto de conexión privado**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | **DETALLES DEL PROYECTO** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior.|
    |Location|Seleccione **WestCentralUS**.|
    |NOMBRE|Escriba  *myPrivateEndpoint*.  |
    |Recurso secundario de almacenamiento|Deje el valor predeterminado  **Blob**. |
    | **REDES** |  |
    | Virtual network  | Seleccione *MyVirtualNetwork* en el grupo de recursos *myResourceGroup*. |
    | Subnet | Seleccione  *mySubnet*. |
    | **INTEGRACIÓN DE DNS PRIVADO**|  |
    | Integración con una zona DNS privada  | Deje el valor predeterminado **Sí**. |
    | Zona DNS privada  | Deje el valor predeterminado ** (New) privatelink.blob.core.windows.net**. |
    |||
7. Seleccione  **Aceptar**. 
8. Seleccione  **Revisar y crear**. Se le remite a la página  **Revisar y crear** , donde Azure valida la configuración. 
9. Cuando reciba el mensaje  **Validación superada** , seleccione  **Crear**. 
10. Vaya a la cuenta de almacenamiento que acaba de crear.
11. Seleccione  **Claves de acceso** en el menú de contenido de la izquierda.
12. Seleccione **Copiar** en la cadena de conexión para key1.
 
## <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Conéctese a la máquina virtual *myVm* desde Internet de la siguiente manera:

1. En la barra de búsqueda del portal, escriba *myVm*.

1. Seleccione el botón **Conectar**. Después de seleccionar el botón **Conectar**, se abre **Conectar a máquina virtual**.

1. Seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( *.rdp*) y lo descarga en su equipo.

1. Abra el archivo downloaded.rdp*.

    1. Cuando se le pida, seleccione **Conectar**.

    1. Escriba el nombre de usuario y la contraseña que especificó al crear la VM.

        > [!NOTE]
        > Es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta** para especificar las credenciales que escribió al crear la máquina virtual.

1. Seleccione **Aceptar**.

1. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe una advertencia de certificado, seleccione **Sí** o **Continuar**.

1. Una vez que aparezca el escritorio de la máquina virtual, minimícelo para volver a su escritorio local.  

## <a name="access-storage-account-privately-from-the-vm"></a>Acceso a la cuenta de almacenamiento de forma privada desde la máquina virtual

En esta sección, se conectará de manera privada a la cuenta de almacenamiento mediante el punto de conexión privado.

> [!IMPORTANT]
> La configuración de DNS para el almacenamiento requiere una modificación manual en el archivo de hosts para incluir el FQDN de la cuenta específica. Para ello, modifique el archivo siguiente con permisos de administrador en Windows: c:\Windows\System32\Drivers\etc\hosts or Linux /etc/hosts. Incluya la información de DNS de la cuenta del paso anterior en el siguiente formato [Dirección IP privada] myaccount.blob.core.windows.net.

1. En el Escritorio remoto de  *myVm*, abra PowerShell.
2. Escriba  `nslookup mystorageaccount.blob.core.windows.net`. Recibirá un mensaje similar a este:
    ```azurepowershell
    Server:  UnKnown
    Address:  168.63.129.16
    Non-authoritative answer:
    Name:    mystorageaccount123123.privatelink.blob.core.windows.net
    Address:  10.0.0.5
    Aliases:  mystorageaccount.blob.core.windows.net
3. Install [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=windows).
4. Select **Storage accounts** with the right-click.
5. Select **Connect to an azure storage**.
6. Select **Use a connection string**.
7. Select **Next**.
8. Enter the connection string by pasting the information previously copied.
9. Select **Next**.
10. Select **Connect**.
11. Browse the Blob containers from mystorageaccount 
12. (Optionally) Create folders and/or upload files to *mystorageaccount*. 
13. Close the remote desktop connection to *myVM*. 

Additional options to access the storage account:
- Microsoft Azure Storage Explorer is a standalone free app from Microsoft that enables you to work visually with Azure storage data on Windows, macOS, and Linux. You can install the application to browse privately the storage account content. 
 
- The AzCopy utility is another option for high-performance scriptable data transfer for Azure storage. Use AzCopy to transfer data to and from Blob, File, and Table storage. 


## Clean up resources 
When you're done using the Private Endpoint, storage account and the VM, delete the resource group and all of the resources it contains: 
1. Enter *myResourceGroup* in the **Search** box at the top of the portal and select *myResourceGroup* from the search results. 
2. Select **Delete resource group**. 
3. Enter *myResourceGroup* for **TYPE THE RESOURCE GROUP NAME** and select **Delete**. 

## Next steps
In this Quickstart, you created a VM on a virtual network and storage account and a Private Endpoint. You connected to one VM from the internet and securely communicated to the storage account using Private Link. To learn more about Private Endpoint, see [What is Azure Private Endpoint?](private-endpoint-overview.md).
