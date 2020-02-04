---
title: 'Enrutamiento del tráfico de red (tutorial): Azure Portal'
titlesuffix: Azure Virtual Network
description: En este tutorial, aprenderá a enrutar el tráfico de red con una tabla de rutas mediante Azure Portal.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/22/2019
ms.author: kumud
ms.openlocfilehash: 96b6788e48b845ef7f0add11767eb36b47cac36b
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76775276"
---
# <a name="tutorial-route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Tutorial: Enrutamiento del tráfico de red con una tabla de rutas mediante Azure Portal

De forma predeterminada, Azure enruta el tráfico entre todas las subredes de una red virtual. Sin embargo, puede crear sus propias rutas para invalidar las predeterminadas de Azure. La posibilidad de crear rutas personalizadas resulta de utilidad si, por ejemplo, quiere enrutar el tráfico entre subredes por medio de una aplicación virtual de red (NVA). En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Creación de una tabla de rutas
> * Creación de una ruta
> * Creación de una red virtual con varias subredes
> * Asociación de una tabla de rutas a una subred
> * Creación de una aplicación virtual de red para enrutar el tráfico
> * Implementación de máquinas virtuales (VM) en subredes diferentes
> * Enrutamiento del tráfico desde una subred a otra a través de una aplicación virtual de red

Si lo prefiere, puede completar este tutorial con la [CLI de Azure](tutorial-create-route-table-cli.md) o [Azure PowerShell](tutorial-create-route-table-powershell.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en [Azure Portal](https://portal.azure.com).

## <a name="create-a-route-table"></a>Creación de una tabla de rutas

1. En el menú de Azure Portal, seleccione **Crear un recurso**.
2. En el cuadro de búsqueda, escriba *Tabla de enrutamiento*. Cuando **Tabla de enrutamiento** aparezca en los resultados de la búsqueda, selecciónelo.
3. En la página **Tabla de enrutamiento**, seleccione **Crear**.
4. En **Crear tabla de rutas**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba *myRouteTablePublic*. |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **Crear nuevo**, escriba *myResourceGroup* y seleccione *Aceptar*. |
    | Location | Seleccione **Este de EE. UU**.
    | Propagación de rutas de puerta de enlace de red virtual | Deje el valor predeterminado **Habilitado**. |
5. Seleccione **Crear**.

## <a name="create-a-route"></a>Creación de una ruta

1. En la barra de búsqueda del portal, escriba *myRouteTablePublic*.

1. Cuando aparezca **myRouteTablePublic** en los resultados de búsqueda, selecciónelo.

1. En **myRouteTablePublic** en **Configuración**, seleccione **Rutas** >  **+ Agregar**.

    ![Adición de una ruta](./media/tutorial-create-route-table-portal/add-route.png)

1. En **Agregar ruta**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre de ruta | Escriba *ToPrivateSubnet*. |
    | Prefijo de dirección | Escriba *10.0.1.0/24*. |
    | Tipo de próximo salto | Seleccione **Aplicación virtual**. |
    | Siguiente dirección de salto | Escriba *10.0.2.4*. |

1. Seleccione **Aceptar**.

## <a name="associate-a-route-table-to-a-subnet"></a>Asociación de una tabla de rutas a una subred

Para poder asociar una tabla de rutas a una subred, debe crear una red virtual y una subred.

### <a name="create-a-virtual-network"></a>Creación de una red virtual

1. En la parte superior izquierda de la pantalla, seleccione **Crear un recurso** > **Redes** > **Red virtual**.

1. En **Creación de una red virtual**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba *myVirtualNetwork*. |
    | Espacio de direcciones | Escriba *10.0.0.0/16*. |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione ***Seleccionar existente*** > **myResourceGroup**. |
    | Location | Deje el valor predeterminado **Este de EE. UU.** |
    | Subred: nombre | Escriba *Pública*. |
    | Subred: intervalo de direcciones | Escriba *10.0.0.0/24*. |

1. Deje el resto de valores predeterminados y seleccione **Crear**.

### <a name="add-subnets-to-the-virtual-network"></a>Adición de subredes a la red virtual

1. En la barra de búsqueda del portal, escriba *myVirtualNetwork*.

1. Cuando aparezca la opción **myVirtualNetwork** en los resultados de la búsqueda, selecciónela.

1. En **myVirtualNetwork**, en **Configuración**, seleccione **Subredes** >  **+ Subred**.

    ![Subred agregada](./media/tutorial-create-route-table-portal/add-subnet.png)

1. En **Agregar subred**, especifique esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba *Privada*. |
    | Espacio de direcciones | Escriba *10.0.1.0/24*. |

1. Deje el resto de valores predeterminados y seleccione **Aceptar**.

1. Seleccione de nuevo **+ Subred**. Esta vez, especifique esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba *DMZ*. |
    | Espacio de direcciones | Escriba *10.0.2.0/24*. |

1. Al igual que la última vez, deje el resto de valores predeterminados y seleccione **Aceptar**.

    Azure muestra las tres subredes: **Pública**, **Privada** y **DMZ**.

### <a name="associate-myroutetablepublic-to-your-public-subnet"></a>Asociación de myRouteTablePublic a la subred pública

1. Seleccione **Público**.

1. En **Pública**, seleccione **Tabla de rutas** > **MyRouteTablePublic** > **Guardar**.

    ![Asociación de una tabla de rutas](./media/tutorial-create-route-table-portal/associate-route-table.png)

## <a name="create-an-nva"></a>Creación de una aplicación virtual de red

Las aplicaciones virtuales de red son máquinas virtuales que ayudan con las funciones de red, como el enrutamiento y la optimización de firewall. Si lo desea puede seleccionar un sistema operativo diferente. En este tutorial se supone que está utilizando **Windows Server 2016 Datacenter**.

1. En la esquina superior izquierda de la pantalla, seleccione **Crear un recurso** > **Compute** > **Windows Server 2016 Datacenter**.

1. En **Creación de una máquina virtual: conceptos básicos**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | **DETALLES DEL PROYECTO** | |
    | Subscription | Seleccione su suscripción. |
    | Resource group | Seleccione **myResourceGroup**. |
    | **DETALLES DE INSTANCIA** |  |
    | Nombre de la máquina virtual | Escriba *myVmNva*. |
    | Region | Seleccione **Este de EE. UU**. |
    | Opciones de disponibilidad | Deje el valor predeterminado **No se requiere redundancia de la infraestructura**. |
    | Imagen | Deje el valor predeterminado **Windows Server 2016 Datacenter**. |
    | Size | Deje el valor predeterminado **Estándar DS1 v2**. |
    | **CUENTA DE ADMINISTRADOR** |  |
    | Nombre de usuario | Escriba un nombre de usuario de su elección. |
    | Contraseña | Escriba una contraseña de su elección. La contraseña debe tener al menos 12 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    | Confirm Password | Vuelva a escribir la contraseña. |
    | **REGLAS DE PUERTO DE ENTRADA** |  |
    | Puertos de entrada públicos | Seleccione **Ninguno**.
    | **AHORRE DINERO** |  |
    | ¿Ya tiene una licencia de Windows? | Deje el valor predeterminado **No**. |

1. Seleccione **Siguiente: Discos**.

1. En **Creación de una máquina virtual: discos**, seleccione la configuración adecuada para sus necesidades.

1. Seleccione **Siguiente: Redes**.

1. En **Creación de una máquina virtual: Redes**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Virtual network | Deje el valor predeterminado **myVirtualNetwork**. |
    | Subnet | Seleccione **DMZ (10.0.2.0/24)** . |
    | Dirección IP pública | Seleccione **Ninguno**. No necesita una dirección IP pública. La máquina virtual no se conectará a través de Internet.|

1. Deje el resto de valores predeterminados y seleccione **Siguiente: Administración**.

1. En **Creación de una máquina virtual: Administración**, para **Cuenta de almacenamiento de diagnóstico**, seleccione **Crear nuevo**.

1. En **Crear cuenta de almacenamiento**, escriba o seleccione esta información:

    | Configuración | Value |
    | ------- | ----- |
    | Nombre | Escriba *mynvastorageaccount*. |
    | Tipo de cuenta | Deje el valor predeterminado **Storage (uso general v1)** . |
    | Rendimiento | Deje el valor predeterminado **Estándar**. |
    | Replicación | Deje el valor predeterminado **Almacenamiento con redundancia local (LRS)** .

1. Seleccione **Aceptar**.

1. Seleccione **Revisar + crear**. Se le remite a la página **Revisar y crear** y Azure valida la configuración.

1. Cuando vea **Validación superada**, seleccione **Crear**.

    La máquina virtual tarda en crearse unos minutos. No continúe hasta que Azure termine de crear la máquina virtual. La página **La implementación está en curso** le mostrará los detalles de la implementación.

1. Cuando la máquina virtual esté lista, seleccione **Ir al recurso**.

## <a name="turn-on-ip-forwarding"></a>Habilitación del reenvío IP

Active el reenvío IP para *myVmNva*. Cuando Azure envía tráfico de red a *myVmNva*, si el tráfico se destina a una dirección IP diferente, el reenvío IP enviará el tráfico a la ubicación correcta.

1. En **myVmNva**, en **Configuración**, seleccione **Redes**.

1. Seleccione **myvmnva123**. Esta es la interfaz de red que Azure creó para la máquina virtual. Tendrá una cadena de números que será única para su caso.

    ![Redes de máquinas virtuales](./media/tutorial-create-route-table-portal/virtual-machine-networking.png)

1. En **Configuración**, seleccione **Configuraciones IP**.

1. En **myvmnva123: Configuraciones IP**, para **Reenvío IP**, seleccione **Habilitado** y, a continuación, seleccione **Guardar**.

    ![Habilitación del reenvío IP](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png)

## <a name="create-public-and-private-virtual-machines"></a>Creación de máquinas virtuales públicas y privadas

Cree una máquina virtual pública y una máquina virtual privada en la red virtual. Más adelante, las usará para ver que Azure enruta el tráfico de subred *Público* a la subred *Privada* mediante la aplicación virtual de red.

Realice los pasos 1 a 12 de [Creación de una aplicación virtual de red](#create-an-nva). Use la mayoría de los mismos valores. Estos valores son los que tienen que ser diferentes:

| Configuración | Value |
| ------- | ----- |
| **VM PÚBLICA** | |
| ASPECTOS BÁSICOS |  |
| Nombre de la máquina virtual | Escriba *myVmPublic*. |
| REDES | |
| Subnet | Seleccione **Pública (10.0.0.0/24)** . |
| Dirección IP pública | Acepte el valor predeterminado. |
| Puertos de entrada públicos | Seleccione **Permitir los puertos seleccionados**. |
| Selección de puertos de entrada | Seleccione **HTTP** y **RDP**. |
| ADMINISTRACIÓN | |
| Cuenta de almacenamiento de diagnóstico | Deje el valor predeterminado **mynvastorageaccount**. |
| **VM PRIVADA** | |
| ASPECTOS BÁSICOS |  |
| Nombre de la máquina virtual | Escriba *myVmPrivate*. |
| REDES | |
| Subnet | Seleccione **Privada (10.0.1.0/24)** . |
| Dirección IP pública | Acepte el valor predeterminado. |
| Puertos de entrada públicos | Seleccione **Permitir los puertos seleccionados**. |
| Selección de puertos de entrada | Seleccione **HTTP** y **RDP**. |
| ADMINISTRACIÓN | |
| Cuenta de almacenamiento de diagnóstico | Deje el valor predeterminado **mynvastorageaccount**. |

Puede crear la máquina virtual *myVmPrivate* mientras Azure crea la máquina virtual *myVmPublic*. No continúe con el resto de pasos hasta que Azure termine de crear ambas máquinas virtuales.

## <a name="route-traffic-through-an-nva"></a>Enrutamiento del tráfico a través de una aplicación virtual de red

### <a name="sign-in-to-myvmprivate-over-remote-desktop"></a>Inicio de sesión en myVmPrivate a través de Escritorio remoto

1. En la barra de búsqueda del portal, escriba *myVmPrivate*.

1. Cuando la máquina virtual **myVmPrivate** aparece en los resultados de búsqueda, selecciónela.

1. Seleccione **Conectar** para crear una conexión al Escritorio remoto para la máquina virtual *myVmPrivate*.

1. En **Conectar a máquina virtual**, seleccione **Descargar archivo RDP**. Azure crea un archivo de Protocolo de Escritorio remoto ( *.rdp*) y lo descarga en su equipo.

1. Abra el archivo *.rdp* descargado.

    1. Cuando se le pida, seleccione **Conectar**.

    1. Escriba el nombre de usuario y la contraseña que especificó al crear la máquina virtual privada.

    1. Es posible que deba seleccionar **Más opciones** > **Usar otra cuenta** para usar las credenciales de la VM privada.

1. Seleccione **Aceptar**.

    Puede recibir una advertencia de certificado durante el proceso de inicio de sesión.

1. Seleccione **Sí** para conectarse a la máquina virtual.

### <a name="enable-icmp-through-the-windows-firewall"></a>Habilite ICMP mediante el firewall de Windows

En un paso posterior, usará la herramienta trace route para probar el enrutamiento. Trace route usa el Protocolo de mensajes de control de Internet (ICMP), que el Firewall de Windows deniega de manera predeterminada. Habilite ICMP a través del Firewall de Windows.

1. En el escritorio remoto de *myVmPrivate*, abra PowerShell.

1. Escriba este comando:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

    Está usando el comando trace route para probar el enrutamiento en este tutorial. Para entornos de producción, no se recomienda permitir ICMP a través del Firewall de Windows.

### <a name="turn-on-ip-forwarding-within-myvmnva"></a>Activación del reenvío IP en myVmNva

Ha [activado el reenvío IP](#turn-on-ip-forwarding) para la interfaz de red de la máquina virtual con Azure. El sistema operativo de la máquina virtual también tiene que reenviar el tráfico de red. Active el reenvío IP para el sistema operativo de máquina virtual *myVmNva* con estos comandos.

1. Desde un símbolo del sistema en la máquina virtual *myVmPrivate*, abra un Escritorio remoto en la máquina virtual *myVmNva*:

    ```cmd
    mstsc /v:myvmnva
    ```

1. En PowerShell, en *myVmNva*, escriba este comando para activar el reenvío IP:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```

1. Reinicie la máquina virtual *myVmNva*. En la barra de tareas, seleccione el **botón Inicio** > **botón de encendido**, **Otros (planeado)**  > **Continuar**.

    Esto también desconecta la sesión de Escritorio remoto.

1. Una vez que se reinicie la máquina virtual *myVmNva*, cree una sesión de Escritorio remoto en la máquina virtual *myVmPublic*. Mientras sigue conectado a la máquina virtual *myVmPrivate*, abra un símbolo del sistema y ejecute este comando:

    ```cmd
    mstsc /v:myVmPublic
    ```
1. En el Escritorio remoto de *myVmPrivate*, abra PowerShell.

1. Habilite ICMP mediante el Firewall de Windows con el comando siguiente:

    ```powershell
    New-NetFirewallRule –DisplayName "Allow ICMPv4-In" –Protocol ICMPv4
    ```

## <a name="test-the-routing-of-network-traffic"></a>Prueba del enrutamiento del tráfico de red

En primer lugar, vamos a probar el enrutamiento del tráfico de red desde la máquina virtual *myVmPublic* a la máquina virtual *myVmPrivate*.

1. En PowerShell, en la máquina virtual *myVmPublic*, escriba este comando:

    ```powershell
    tracert myVmPrivate
    ```

    La respuesta es similar a este ejemplo:

    ```powershell
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:

    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4

    Trace complete.
    ```

    Puede ver que el primer salto es 10.0.2.4. Es la dirección IP privada de la aplicación virtual de red. El segundo salto es a la dirección IP privada de la máquina virtual *myVmPrivate*: 10.0.1.4. Anteriormente, agregó la ruta a la tabla de rutas *myRouteTablePublic* y la asoció a la subred *Pública*. Como resultado, Azure envió el tráfico a través de la aplicación virtual de red y no directamente a la subred *Privada*.

1. Cierre la sesión de Escritorio remoto a la máquina virtual *myVmPublic*, que aún le deja conectado a la máquina virtual *myVmPrivate*.

1. Desde un símbolo del sistema de la máquina virtual *myVmPrivate*, escriba este comando:

    ```cmd
    tracert myVmPublic
    ```

    Prueba el enrutamiento del tráfico de red desde la máquina virtual *myVmPrivate* a la máquina virtual *myVmPublic*. La respuesta es similar a este ejemplo:

    ```cmd
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:

    1     1 ms     1 ms     1 ms  10.0.0.4

    Trace complete.
    ```

    Puede ver si Azure enruta el tráfico directamente desde la máquina virtual *myVmPrivate* a la máquina virtual *myVmPublic*. De forma predeterminada, Azure enruta el tráfico directamente entre subredes.

1. Cierre la sesión de Escritorio remoto a la máquina virtual *myVmPrivate*.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sea necesario, elimine el grupo de recursos y todos los recursos que contiene:

1. En la barra de búsqueda del portal, escriba *myResourceGroup*.

1. Seleccione **myResourceGroup** cuando lo vea en los resultados de la búsqueda.

1. Seleccione **Eliminar grupo de recursos**.

1. Escriba *myResourceGroup* para **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado una tabla de rutas y la ha asociado a una subred. Creó una aplicación virtual de red sencilla que enrutó el tráfico desde una subred pública hasta una subred privada. Ahora que sabe cómo hacerlo, puede implementar diferentes aplicaciones virtuales de red configuradas previamente desde [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Llevan a cabo muchas funciones de red que le resultarán útiles. Para más información acerca del enrutamiento, consulte [Introducción al enrutamiento](virtual-networks-udr-overview.md) y [Administración de una tabla de rutas](manage-route-table.md).

Aunque puede implementar muchos recursos de Azure en una red virtual, Azure no puede implementar recursos para algunos servicios de PaaS en una red virtual. Es posible restringir el acceso a los recursos de algunos servicios de PaaS de Azure. Sin embargo, la restricción solo debe ser tráfico desde una subred de red virtual. Para aprender a restringir el acceso de red a los recursos de PaaS de Azure, pase al siguiente tutorial.

> [!div class="nextstepaction"]
> [Restringir el acceso de red a los recursos de PaaS](tutorial-restrict-network-access-to-resources.md)
