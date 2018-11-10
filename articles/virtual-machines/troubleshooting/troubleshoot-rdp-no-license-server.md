---
title: El servidor de licencias de Escritorio remoto no está disponible cuando se conecta a una máquina virtual de Azure | Microsoft Docs
description: Obtenga información sobre cómo solucionar problemas de errores con el Protocolo de escritorio remoto porque no está disponible ningún servidor de licencias de Escritorio remoto | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 5e54579a35140ee7cfc06358d60cf7a63292e107
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50088303"
---
# <a name="remote-desktop-license-server-is-not-available-when-you-connect-to-azure-vm"></a>El servidor de licencias de Escritorio remoto no está disponible cuando se conecta a una máquina virtual de Azure

En este artículo se le ayuda a resolver el problema en el que no se puede conectar a una máquina virtual de Azure (VM) porque no hay ningún servidor de licencias de Escritorio remoto disponible para proporcionar una licencia.

## <a name="symptoms"></a>Síntomas

Cuando intenta conectarse a una máquina virtual, se encuentra con los siguientes escenarios:

- La captura de pantalla de la máquina virtual (VM) muestra que el sistema operativo está totalmente cargado y esperando las credenciales.
- Recibe los siguientes mensajes de error cuando intenta realizar una conexión del Protocolo de escritorio remoto de Microsoft (RDP):

  - **Se desconectó la sesión remota porque no hay servidores de licencias de Escritorio remoto disponibles para proporcionar una licencia.**

  - **No hay ningún servidor de licencias de Escritorio remoto disponible. Los Servicios de Escritorio remoto dejarán de funcionar porque el período de gracia de este equipo terminó y no ha establecido contacto con un servidor de licencias de Windows Server 2008 válido como mínimo. Haga clic en este mensaje para abrir la configuración del servidor host de sesión de Escritorio remoto a fin de usar el diagnóstico de licencias.**

Sin embargo, puede conectarse a la máquina virtual normalmente mediante una sesión administrativa:

```
mstsc /v:<Server>[:<Port>] /admin
```

## <a name="cause"></a>Causa

Este problema se produce si no hay ningún servidor de licencias de Escritorio remoto disponible para proporcionar una licencia que permita iniciar una sesión remota. Este problema puede deberse a varios escenarios incluso si en la máquina virtual se configuró un rol de host de sesión de Escritorio remoto:

- Nunca hubo ningún rol de administración de licencias de Escritorio remoto en el entorno y se terminó el período de gracia (180 días).
- Una licencia de Escritorio remoto se instaló en el entorno, pero nunca se activó.
- Una licencia de Escritorio remoto en el entorno no tiene insertadas licencias de acceso de cliente (CAL) para establecer la conexión.
- Se instaló una licencia de Escritorio remoto en el entorno. Hay CAL disponibles, pero no se han configurado correctamente.
- Una licencia de Escritorio remoto cuenta con CAL y se ha activado. Sin embargo, algunos otros problemas en el servidor de licencias de Escritorio remoto impiden que se proporcionen las licencias en el entorno.

## <a name="solution"></a>Solución

Para resolver este problema, [realice una copia de seguridad del disco del sistema operativo](../windows/snapshot-copy-managed-disk.md) y siga estos pasos:

1. Conéctese a la máquina virtual mediante una sesión administrativa:

   ```
   mstsc /v:<Server>[:<Port>] /admin
   ```

  Si no se puede conectar a la máquina virtual mediante una sesión administrativa, puede usar la [consola serie](serial-console-windows.md) para tener acceso a la máquina virtual con los siguientes pasos:

  1. Acceda a Serial Console seleccionando **Soporte técnico y solución de problemas** > **Serial Console (versión preliminar)**. Si la característica está habilitada en la máquina virtual, puede conectar la máquina virtual correctamente.

  2. Creación de un nuevo canal para una instancia CMD. Escriba **CMD** para iniciar el canal con el fin de obtener el nombre del canal.

  3. Cambie al canal que ejecuta la instancia CMD, en este caso debe ser el canal 1.

    ```
    ch -si 1
    ```

  4. Presione **Entrar** otra vez y escriba un nombre de usuario y una contraseña válidos (id. de dominio o local) para la máquina virtual.

2. Compruebe si la máquina virtual tiene habilitado un rol de host de sesión de Escritorio remoto. Si el rol está habilitado, asegúrese de que está funcionando correctamente. Abra una instancia CMD con privilegios elevados y siga estos pasos:

  1. Use el siguiente comando para comprobar el estado del rol de host de sesión de Escritorio remoto:

    ```
    reg query "HKLM\SOFTWARE\Microsoft\ServerManager\ServicingStorage\ServerComponentCache\RDS-RD-Server" /v InstallState
    ```

    Si este comando devuelve un valor de 0, significa que el rol está deshabilitado, y puede continuar en el paso 3.

  2. Use el siguiente comando para comprobar las directivas y volver a configurarlas según sea necesario:

    ```
    reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode reg query "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers
    ```

    Si el valor de **LicensingMode** es distinto de 4 (por usuario), establezca el valor en 4:

    ```
    reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\RCM\Licensing Core" /v LicensingMode /t REG_DWORD /d 4
    ```

    Si el valor de **SpecifiedLicenseServers** no existe o tiene información del servidor de licencias incorrecta, cámbielo como se muestra a continuación:

    ```
    reg add "HKLM\SYSTEM\CurrentControlSet\Services\TermService\Parameters" /v SpecifiedLicenseServers /t REG_MULTI_SZ /d "<FQDN / IP License server>"
    ```

  3. Después de realizar cualquier cambio en el registro, reinicie la máquina virtual.

  4. Si no dispone de ninguna CAL, quite el rol de host de sesión de Escritorio remoto. A continuación, el RDP vuelve a su estado normal y solo permite dos conexiones simultáneas de RDP a la máquina virtual.

    ```
    dism /ONLINE /Disable-feature /FeatureName:Remote-Desktop-Services
    ```

    Si la máquina virtual tiene el rol de administración de licencias de Escritorio remoto y no se usa, también puede quitar ese rol.

    ```
    dism /ONLINE /Disable-feature /FeatureName:Licensing
    ```

  5. Asegúrese de que la máquina virtual puede conectarse al servidor de licencias de Escritorio remoto. Puede probar la conectividad con el puerto 135 entre la máquina virtual y el servidor de licencias. 

    ```
    telnet <FQDN / IP License Server> 135
    ```

3. Si no hay ningún servidor de licencias de Escritorio remoto en el entorno y quiere uno, puede [instalar un servicio de rol de administración de licencias de Escritorio remoto ](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731765(v=ws.11))y, a continuación, [configurar las licencias de RDS](https://blogs.technet.microsoft.com/askperf/2013/09/20/rd-licensing-configuration-on-windows-server-2012/).

4. Si un servidor de licencias de Escritorio remoto está configurado y en buen estado, debe asegurarse de activar el servidor de licencias de escritorio remoto con CAL.

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
