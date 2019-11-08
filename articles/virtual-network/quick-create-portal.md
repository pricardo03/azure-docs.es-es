---
title: 'Inicio rápido: Creación de una red virtual mediante Azure Portal'
titlesuffix: Azure Virtual Network
description: En esta guía de inicio rápido, aprenderá a crear una red virtual mediante Azure Portal. Una red virtual permite que los recursos de Azure, como las máquinas virtuales, se comuniquen de manera segura entre sí y con Internet
services: virtual-network
documentationcenter: virtual-network
author: KumudD
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can securely communicate with each other and with the internet.
ms.service: virtual-network
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 07/08/2019
ms.author: kumud
ms.openlocfilehash: d8e95f9c345a943eb458800b852640e3f1fde907
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73488482"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-portal"></a>Inicio rápido: Creación de una red virtual mediante el Portal de Azure

Una red virtual es el bloque de creación básico de una red privada en Azure. Permite que los recursos de Azure, como las máquinas virtuales (VM), se comuniquen de manera segura entre sí y con Internet. En este inicio rápido, aprenderá a crear una red virtual desde Azure Portal. A continuación, puede implementar dos VM en la red virtual, establecer una comunicación segura entre las dos VM y conectarse a las VM desde Internet.


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ahora.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en el [Azure Portal](https://portal.azure.com).

## <a name="create-a-virtual-network"></a>Creación de una red virtual

1. En el menú de Azure Portal, seleccione **Crear un recurso**.

2. En Azure Marketplace, seleccione **Redes** > **Red virtual**.

3. En **Creación de una red virtual**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | NOMBRE | Escriba *myVirtualNetwork*. |
    | Espacio de direcciones | Escriba *10.1.0.0/16*. |
    | Subscription | Seleccione su suscripción.|
    | Resource group | Seleccione **Crear nuevo**, escriba *myResourceGroup* y, después, seleccione **Aceptar**. |
    | Location | Seleccione **Este de EE. UU**.|
    | Subred: nombre | Escriba *myVirtualSubnet*. |
    | Subred: intervalo de direcciones | Escriba *10.1.0.0/24*. |

4. Deje el resto tal como está y seleccione **Crear**.

## <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Cree dos máquinas virtuales en la red virtual:

### <a name="create-the-first-vm"></a>Creación de la primera máquina virtual

1. En el menú de Azure Portal, seleccione **Crear un recurso**.

2. En Azure Marketplace, seleccione **Proceso** > **Windows Server 2019 Datacenter**.

3. En **Creación de una máquina virtual: conceptos básicos**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | **DETALLES DEL PROYECTO** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. Lo creó en la sección anterior. |
    | **DETALLES DE INSTANCIA** |  |
    | Nombre de la máquina virtual | Escriba *myVm1*. |
    | Region | Seleccione **Este de EE. UU**. |
    | Opciones de disponibilidad | Deje el valor predeterminado **No se requiere redundancia de la infraestructura**. |
    | Imagen | Deje el valor predeterminado **Windows Server 2019 Datacenter**. |
    | Size | Deje el valor predeterminado **Estándar DS1 v2**. |
    | **CUENTA DE ADMINISTRADOR** |  |
    | Nombre de usuario | Escriba un nombre de usuario de su elección. |
    | Contraseña | Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Confirm Password | Vuelva a escribir la contraseña. |
    | **REGLAS DE PUERTO DE ENTRADA** |  |
    | Puertos de entrada públicos | Deje el valor predeterminado **Ninguno**. |
    | **AHORRE DINERO** |  |
    | ¿Ya tiene una licencia de Windows? | Deje el valor predeterminado **No**. |

4. Seleccione **Siguiente: Discos**.

5. En **Creación de una máquina virtual: Discos**, deje los valores predeterminados y seleccione **Siguiente: Redes**.

6. En **Creación de una máquina virtual: Redes**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | Virtual network | Deje el valor predeterminado **myVirtualNetwork**. |
    | Subnet | Deje el valor predeterminado **myVirtualSubnet (10.1.0.0/24)** . |
    | Dirección IP pública | Deje el valor predeterminado **(new) myVm-ip**. |
    | Puertos de entrada públicos | Seleccione **Permitir los puertos seleccionados**. |
    | Selección de puertos de entrada | Seleccione **HTTP** y **RDP**.

7. Seleccione **Siguiente: Administración**.

8. En **Creación de una máquina virtual: Administración**, para **Cuenta de almacenamiento de diagnóstico**, seleccione **Crear nuevo**.

9. En **Crear cuenta de almacenamiento**, escriba o seleccione esta información:

    | Configuración | Valor |
    | ------- | ----- |
    | NOMBRE | Escriba *myvmstorageaccount*. Si el nombre ya existe, cree uno único.|
    | Tipo de cuenta | Deje el valor predeterminado **Storage (uso general v1)** . |
    | Rendimiento | Deje el valor predeterminado **Estándar**. |
    | Replicación | Deje el valor predeterminado **Almacenamiento con redundancia local (LRS)** . |

10. Seleccione **Aceptar**.

11. Seleccione **Revisar + crear**. Se le remitirá a la página **Revisar y crear**, donde Azure validará la configuración.

12. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**.

### <a name="create-the-second-vm"></a>Creación de la segunda máquina virtual

1. Complete los pasos 1 y 9 anteriores.

    > [!NOTE]
    > En el paso 2, para el **nombre de máquina virtual**, escriba *myVm2*.
    >
    > En el paso 7, para **Cuenta de almacenamiento de diagnóstico**, asegúrese de seleccionar **myvmstorageaccount**.

2. Seleccione **Revisar + crear**. Se le remite a la página **Revisar y crear** y Azure valida la configuración.

3. Cuando reciba el mensaje **Validación superada**, seleccione **Crear**.

## <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Después de crear *myVm1*, conéctese a Internet.

1. En la barra de búsqueda del portal, escriba *myVm1*.

2. Seleccione el botón **Conectar**.

    ![Conexión a una máquina virtual](./media/quick-create-portal/connect-to-virtual-machine.png)

    Después de seleccionar el botón **Conectar**, se abre **Conectar a máquina virtual**.

3. Seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( *.rdp*) y lo descarga en su equipo.

4. Abra el archivo *.rdp* descargado.

    1. Cuando se le pida, seleccione **Conectar**.

    2. Escriba el nombre de usuario y la contraseña que especificó al crear la VM.

        > [!NOTE]
        > Es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta** para especificar las credenciales que escribió al crear la máquina virtual.

5. Seleccione **Aceptar**.

6. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Si recibe una advertencia de certificado, seleccione **Sí** o **Continuar**.

7. Una vez que aparezca el escritorio de la máquina virtual, minimícelo para volver a su escritorio local.

## <a name="communicate-between-vms"></a>Comunicarse entre máquinas virtuales

1. En el Escritorio remoto de *myVm1*, abra PowerShell.

2. Escriba `ping myVm2`.

    Recibirá un mensaje similar a este:

    ```powershell
    Pinging myVm2.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.clouda
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.1.0.5:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Se produce un error de `ping`, porque `ping` usa el Protocolo de mensajes de control de Internet (ICMP). De manera predeterminada, el protocolo ICMP no puede atravesar el Firewall de Windows.

3. Para permitir que *myVm2* haga ping a *myVm1* en un paso posterior, escriba este comando:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Ese comando permite una conexión entrante de ICMP a través del Firewall de Windows:

4. Cierre la conexión de Escritorio remoto a *myVm1*.

5. Vuelva a realizar los pasos de [Conexión a una máquina virtual desde Internet](#connect-to-a-vm-from-the-internet), pero conéctese a *myVm2*.

6. Desde un símbolo del sistema, escriba `ping myvm1`.

    Obtendrá un mensaje similar al siguiente:

    ```powershell
    Pinging myVm1.0v0zze1s0uiedpvtxz5z0r0cxg.bx.internal.cloudapp.net [10.1.0.4] with 32 bytes of data:
    Reply from 10.1.0.4: bytes=32 time=1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.1.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.1.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 1ms, Average = 0ms
    ```

    Recibe respuestas de *myVm1* porque permitió ICMP a través del Firewall de Windows en la VM *myVm1* en el paso 3.

7. Cierre la conexión de Escritorio remoto a *myVm2*.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado de usar la red virtual y las VM, elimine el grupo de recursos y todos los recursos que contiene:

1. Escriba *myResourceGroup* en el cuadro **Buscar** de la parte superior del portal y seleccione **myResourceGroup** en los resultados de búsqueda.

2. Seleccione **Eliminar grupo de recursos**.

3. Escriba *myResourceGroup* para **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, ha creado una red virtual predeterminada y dos VM. Se ha conectado a una VM desde Internet y se ha comunicado de forma segura entre las dos VM. Para más información sobre la configuración de red virtual, consulte [Administración de una red virtual](manage-virtual-network.md).

De forma predeterminada, Azure permite una comunicación segura sin restricciones entre las VM. En cambio, Azure solo permite conexiones de Escritorio remoto entrantes a las máquinas virtuales Windows desde Internet. Para más información sobre cómo configurar distintos tipos de comunicaciones de red de VM, vaya al tutorial sobre el [filtrado del tráfico de red](tutorial-filter-network-traffic.md).
