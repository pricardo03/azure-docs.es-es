---
title: Conexión a una máquina virtual de Windows Server | Microsoft Docs
description: Aprenda a conectarse a una máquina virtual de Windows y a iniciar sesión en ella mediante Azure Portal y el modelo de implementación de Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 11/26/2018
ms.author: cynthn
ms.openlocfilehash: 14bbb77678a70b51c7f640b501827c5618cce606
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55978971"
---
# <a name="how-to-connect-and-sign-on-to-an-azure-virtual-machine-running-windows"></a>Conexión a una máquina virtual de Azure donde se ejecuta Windows e inicio de sesión en ella
Para iniciar una sesión de Escritorio remoto (RDP) desde un escritorio de Windows, usará el botón **Conectar** de Azure Portal. En primer lugar, conéctese a la máquina virtual e inicie sesión.

Para conectarse a una máquina virtual Windows desde un equipo Mac, debe instalar un cliente de RDP para Mac como el [Escritorio remoto de Microsoft](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417).

## <a name="connect-to-the-virtual-machine"></a>Conexión a la máquina virtual
1. Si aún no lo ha hecho, inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. En el menú de la izquierda, seleccione **Máquinas virtuales**.
3. Seleccione la máquina virtual en la lista.
4. En la parte superior de la página de la máquina virtual, haga clic en **Conectar**.
2. En la página **Conexión a la máquina virtual**, seleccione la dirección IP y el puerto adecuados. En la mayoría de los casos, se deben utilizar la dirección IP y el puerto predeterminados. Seleccione **Descargar archivo RDP**Descargar archivo RDP**. Si la máquina virtual tiene configurada una directiva Just-In-Time, primero deberá seleccionar el botón **Solicitar acceso** para solicitar acceso antes de descargar el archivo RDP. Para obtener más información sobre la directiva Just-in-Time, consulte [Administración del acceso a máquina virtual mediante Just-In-Time](../../security-center/security-center-just-in-time.md).
2. Abra el archivo RDP que descargó y haga clic en **Conectar** cuando se le solicite. 
2. Aparecerá una advertencia que indica que el archivo `.rdp` procede de un editor desconocido. Se espera que esto sea así. En la ventana **Conexión a Escritorio remoto**, haga clic en **Conectar**.
   
    ![Captura de pantalla de una advertencia sobre un editor desconocido.](./media/connect-logon/rdp-warn.png)
3. En la ventana **Seguridad de Windows**, seleccione **Más opciones** y, después, **Usar otra cuenta**. Escriba las credenciales de una cuenta en la máquina virtual y haga clic en **Aceptar**.
   
     **Cuenta local**: suele ser el nombre de usuario y la contraseña de la cuenta local que especificó al crear la máquina virtual. En este caso, el dominio es el nombre de la máquina virtual y se escribe como *nombreDeVm*&#92;*nombreDeUsuario*.  
   
    **Máquina virtual unida a dominio**: si la máquina virtual pertenece a un dominio, escriba el nombre de usuario con el formato *Dominio*&#92;*Nombre de usuario*. La cuenta también debe estar en el grupo Administradores o tener privilegios de acceso remoto a la máquina virtual.
   
    **Controlador de dominio**: si la máquina virtual es un controlador de dominio, escriba el nombre de usuario y la contraseña de una cuenta de administrador de dominio para ese dominio.
4. Haga clic en **Sí** para comprobar la identidad de la máquina virtual y finalizar el inicio de sesión.
   
   ![Captura de pantalla que muestra un mensaje sobre la comprobación de la identidad de la máquina virtual.](./media/connect-logon/cert-warning.png)


   > [!TIP]
   > Si el botón **Conectar** del portal está atenuado y no está conectado a Azure a través de una conexión [Express Route](../../expressroute/expressroute-introduction.md) o [VPN de sitio a sitio](../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), deberá crear y asignar a la máquina virtual una dirección IP pública antes de poder usar RDP. Para obtener más información, consulte [Public IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) (Direcciones IP públicas en Azure).
   > 
   > 

## <a name="connect-to-the-virtual-machine-using-powershell"></a>Conexión a la máquina virtual mediante PowerShell

Si usa PowerShell y tiene instalado el módulo de AzureRM, también puede conectarse con el cmdlet `Get-AzRemoteDesktopFile`, como se indica a continuación.

En este ejemplo, se iniciará inmediatamente la conexión RDP, con la aparición de mensajes similares a los anteriores.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -Launch
```

También puede guardar el archivo RDP para un uso futuro.

```powershell
Get-AzRemoteDesktopFile -ResourceGroupName "RgName" -Name "VmName" -LocalPath "C:\Path\to\folder"
```

## <a name="next-steps"></a>Pasos siguientes
Si tiene problemas con la conexión, consulte [Troubleshoot Remote Desktop connections](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Seleccionar problemas con las conexiones del Escritorio remoto). 

