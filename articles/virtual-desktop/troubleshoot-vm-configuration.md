---
title: Creación del grupo host y el inquilino en el escritorio Virtual de Windows - Azure
description: Cómo resolver problemas al configurar una sesión y el inquilino host máquina virtual (VM) en un entorno de Escritorio Virtual de Windows.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: 38d59fb20776470cb683f2a2146838bb217addf7
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64928128"
---
# <a name="tenant-and-host-pool-creation"></a>Creación del grupo de inquilinos y host

Use este artículo para solucionar los problemas que tiene al configurar las máquinas de virtuales de host de sesión de Escritorio Virtual de Windows (VM).

## <a name="provide-feedback"></a>Envío de comentarios

En este momento no se aceptan casos de soporte técnico mientras Windows Virtual Desktop se encuentre en versión preliminar. Visite la [Comunidad técnica de Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) para hablar sobre Windows Virtual Desktop con el equipo de producto y los miembros activos de la comunidad.

## <a name="vms-are-not-joined-to-the-domain"></a>Las máquinas virtuales no están unidas al dominio

Siga estas instrucciones si tiene problemas para unir las máquinas virtuales al dominio.

- Unir la máquina virtual manualmente mediante el proceso en [unir una máquina virtual Windows Server a un dominio administrado](https://docs.microsoft.com/azure/active-directory-domain-services/Active-directory-ds-admin-guide-join-windows-vm-portal) o mediante el [plantilla de unión de dominio](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).
- Intente hacer ping en el nombre de dominio desde la línea de comandos en la máquina virtual.
- Revise la lista de mensajes de error de unión de dominio en [solución de problemas de mensajes de Error de unión de dominio](https://social.technet.microsoft.com/wiki/contents/articles/1935.troubleshooting-domain-join-error-messages.aspx).

### <a name="error-incorrect-credentials"></a>Error: Credenciales incorrectas

**Causa:** Se ha producido un error de escritura realizada cuando las credenciales se han introducido en correcciones de la interfaz de la plantilla Azure Resource Manager.

**Fix:** Siga estas instrucciones para corregir las credenciales.

1. Agregar manualmente las máquinas virtuales a un dominio.
2. Volver a implementar una vez que las credenciales que se hayan confirmado. Consulte [creación de un grupo host con PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).
3. Unir las máquinas virtuales a un dominio mediante una plantilla con [une una máquina virtual Windows existente al dominio de Active Directory](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/).

### <a name="error-timeout-waiting-for-user-input"></a>Error: Tiempo de espera para la entrada de usuario

**Causa:** La cuenta utilizada para completar la unión al dominio puede tener la autenticación multifactor (MFA).

**Fix:** Siga estas instrucciones para completar la unión al dominio.

1. Quitar temporalmente MFA para la cuenta.
2. Use una cuenta de servicio.

### <a name="error-the-account-used-during-provisioning-doesnt-have-permissions-to-complete-the-operation"></a>Error: La cuenta usada durante el aprovisionamiento no tiene permisos para completar la operación

**Causa:** La cuenta utilizada no tiene permisos para unir las máquinas virtuales al dominio debido al cumplimiento de normas y reglamentos.

**Fix:** Siga estas instrucciones.

1. Usar una cuenta que sea miembro del grupo de administradores.
2. Conceda los permisos necesarios para la cuenta utilizada.

### <a name="error-domain-name-doesnt-resolve"></a>Error: No se resuelve el nombre de dominio

**Causa 1:** Las máquinas virtuales están en un grupo de recursos que no está asociado con la red virtual (VNET) donde se encuentra el dominio.

**Corrección 1:** Crear el emparejamiento de VNET entre la red virtual donde se aprovisionan las máquinas virtuales y la red virtual donde se ejecuta el controlador de dominio (DC). Consulte [crear un emparejamiento de redes virtuales: Resource Manager, diferentes suscripciones](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions).

**Causa 2:** Cuando se usa AadService (AADS), no se han establecido las entradas de DNS.

**Corrección 2:** Para configurar servicios de dominio, consulte [habilitar Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns).

## <a name="windows-virtual-desktop-agent-and-windows-virtual-desktop-boot-loader-are-not-installed"></a>Agente de Escritorio Virtual de Windows y el cargador de arranque de Escritorio Virtual de Windows no están instalados

La manera recomendada para aprovisionar máquinas virtuales es mediante Azure Resource Manager **creación y aprovisionamiento de Escritorio Virtual de Windows de host grupo** plantilla. La plantilla instala automáticamente el agente de Escritorio Virtual de Windows y el cargador de arranque de Escritorio Virtual de Windows del agente.

Siga estas instrucciones para confirmar que se instalan los componentes y para comprobar los mensajes de error.

1. Confirme que los dos componentes se instalan mediante la comprobación **Panel de Control** > **programas** > **programas y características**. Si **Windows Virtual Desktop Agent** y **cargador de arranque de Escritorio Virtual de Windows del agente** no son visibles, no están instaladas en la máquina virtual.
2. Abra **Explorador de archivos** y vaya a **C:\Windows\Temp\scriptlogs.log**. Si falta el archivo, indica que el DSC de PowerShell que instala los dos componentes no pudo ejecutar en el contexto de seguridad proporcionado.
3. Si el archivo **C:\Windows\Temp\scriptlogs.log** está presente, ábralo y busque mensajes de error.

### <a name="error-windows-virtual-desktop-agent-and-windows-virtual-desktop-agent-boot-loader-are-missing-cwindowstempscriptlogslog-is-also-missing"></a>Error: Faltan Windows Virtual Desktop Agent y el cargador de arranque de Escritorio Virtual de Windows del agente. C:\Windows\Temp\scriptlogs.log is also missing

**Causa 1:** Credenciales proporcionadas durante la entrada de la plantilla de Azure Resource Manager son incorrectas o los permisos son insuficientes.

**Corrección 1:** Agregar manualmente los componentes que faltan para las máquinas virtuales con [creación de un grupo host con PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).

**Causa 2:** DSC de PowerShell se puede iniciar y ejecutar pero no se pudo completar, ya no se puede iniciar sesión en el escritorio Virtual de Windows y obtener la información necesaria.

**Corrección 2:** Confirme que los elementos de la lista siguiente.

- Asegúrese de que la cuenta no tiene MFA.
- Confirme que el nombre del inquilino es preciso y el inquilino existe en el escritorio Virtual de Windows.
- Confirme que la cuenta tiene al menos permisos de colaborador de RDS.

### <a name="error-authentication-failed-error-in-cwindowstempscriptlogslog"></a>Error: Error de autenticación, error en C:\Windows\Temp\scriptlogs.log

**Causa:** DSC de PowerShell era capaz de ejecutar pero no se pudo conectar al escritorio Virtual de Windows.

**Fix:** Confirme que los elementos de la lista siguiente.

- Registrar manualmente las máquinas virtuales con el servicio de Escritorio Virtual de Windows.
- Confirmar la cuenta usada para la conexión a Escritorio Virtual de Windows tiene permisos en el inquilino para crear grupos host.
- Confirmar la cuenta no tiene MFA.

## <a name="windows-virtual-desktop-agent-is-not-registering-with-the-windows-virtual-desktop-service"></a>Agente de Escritorio Virtual de Windows no está registrando con el servicio de Escritorio Virtual de Windows

Cuando el agente de Escritorio Virtual de Windows se instala por primera vez en la sesión de hospedar las máquinas virtuales (ya sea manualmente o a través de la plantilla de Azure Resource Manager y PowerShell DSC), proporciona un token de registro. La siguiente sección trata problemas aplicables para el agente de Escritorio Virtual de Windows y el token.

### <a name="error-the-status-filed-in-get-rdssessionhost-cmdlet-shows-status-as-unavailable"></a>Error: El campo en el cmdlet Get-RdsSessionHost estado muestra el estado como no disponible

![Cmdlet Get-RdsSessionHost muestra el estado como no disponible.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Causa:** El agente no puede actualizarse automáticamente a una nueva versión.

**Fix:** Siga estas instrucciones para actualizar manualmente el agente.

1. Descargue una versión nueva del agente en la máquina virtual del host de sesión.
2. Inicie el Administrador de tareas y, en la ficha servicio, detenga el servicio RDAgentBootLoader.
3. Ejecute al instalador para la nueva versión del agente de Escritorio Virtual de Windows.
4. Cuando se le pida el token de registro, quite la entrada INVALID_TOKEN y presione siguiente (un nuevo token no es necesario).
5. Complete el Asistente de instalación.
6. Abra el Administrador de tareas e iniciar el servicio RDAgentBootLoader.

## <a name="error--windows-virtual-desktop-agent-registry-entry-isregistered-shows-a-value-of-0"></a>Error:  Entrada del registro de Windows Virtual Desktop Agent IsRegistered muestra un valor de 0

**Causa:** Token de registro ha expirado o se ha generado con el valor de expiración de 999999.

**Fix:** Siga estas instrucciones para corregir al agente de registro error.

1. Si ya hay un token de registro, lo quita con Remove-RDSRegistrationInfo.
2. Generar nuevo token con Rds NewRegistrationInfo.
3. Confirme que el parámetro - ExpriationHours está configurado en 72 (valor máximo es 99999).

### <a name="error-windows-virtual-desktop-agent-isnt-reporting-a-heartbeat-when-running-get-rdssessionhost"></a>Error: Agente de Escritorio Virtual de Windows no está notificando un latido al ejecutar Get-RdsSessionHost

**Causa 1:** Se ha detenido el servicio RDAgentBootLoader.

**Corrección 1:** Inicie el Administrador de tareas y, si la ficha servicio notifica un estado detenido para RDAgentBootLoader servicio, inicie el servicio.

**Causa 2:** Se puede cerrar el puerto 443.

**Corrección 2:** Siga estas instrucciones para abrir el puerto 443.

1. Confirmar el puerto 443 esté abierto, descargue la herramienta PSPing desde [herramientas Sysinternal](https://docs.microsoft.com/sysinternals/downloads/psping).
2. Instale PSPing en el host de sesión que se ejecuta el agente de máquina virtual.
3. Abra el símbolo del sistema como administrador y emita el siguiente comando:

    ```cmd
    psping rdbroker.wvdselfhost.microsoft.com:443
    ```

4. Confirmar información PSPing recibido desde el RDBroker:

    ```
    PsPing v2.10 - PsPing - ping, latency, bandwidth measurement utility
    Copyright (C) 2012-2016 Mark Russinovich
    Sysinternals - www.sysinternals.com
    TCP connect to 13.77.160.237:443:
    5 iterations (warmup 1) ping test:
    Connecting to 13.77.160.237:443 (warmup): from 172.20.17.140:60649: 2.00ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60650: 3.83ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60652: 2.21ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60653: 2.14ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60654: 2.12ms
    TCP connect statistics for 13.77.160.237:443:
    Sent = 4, Received = 4, Lost = 0 (0% loss),
    Minimum = 2.12ms, Maximum = 3.83ms, Average = 2.58ms
    ```

## <a name="troubleshooting-issues-with-the-windows-virtual-desktop-side-by-side-stack"></a>Solución de problemas con la pila de Escritorio Virtual de Windows en paralelo

La pila en paralelo de Escritorio Virtual de Windows se instala automáticamente con Windows Server 2019. Usar Microsoft Installer (MSI) para instalar la pila en paralelo en Microsoft Windows Server 2016 o Windows Server 2012 R2. Microsoft Windows 10, la pila de Escritorio Virtual de Windows en paralelo está habilitada con **enablesxstackrs.ps1**.

Hay tres maneras principales de la pila side-by-side obtiene instalada o habilitada en las máquinas virtuales del grupo de host de sesión:

- Con Azure Resource Manager **creación y aprovisionamiento de grupo de host de Escritorio Virtual de Windows nueva** plantilla
- Por el que se incluye y habilitado en la imagen maestra
- Instalado o habilitado manualmente en cada máquina virtual (o con extensiones o PowerShell)

Si tiene problemas con la pila en paralelo de Escritorio Virtual de Windows, escriba el **qwinsta** ejecutado desde el símbolo del sistema para confirmar que la pila side-by-side está instalada o habilitada.

La salida de **qwinsta** enumerará **rdp sxs** en la salida si la pila side-by-side está instalada y habilitada.

![Pila Side-by-side instalado o habilitado con qwinsta aparece como sxs de rdp en la salida.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

Examine las entradas del registro que se enumeran a continuación y confirme que coinciden con sus valores. Si faltan las claves del registro o valores no coinciden, siga las instrucciones de [creación de un grupo host con PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) acerca de cómo volver a instalar la pila en paralelo.

```registry
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\WinStations\rds-sxs\"fEnableWinstation":DWORD=1

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\ClusterSettings\"SessionDirectoryListener":rdp-sxs
```

### <a name="error-oreverseconnectstackfailure"></a>Error: O_REVERSE_CONNECT_STACK_FAILURE

![Código de error O_REVERSE_CONNECT_STACK_FAILURE.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Causa:** La pila en paralelo no está instalada en el host de sesión de máquina virtual.

**Fix:** Siga estas instrucciones para instalar la pila en paralelo en el host de sesión de máquina virtual.

1. Use el protocolo de escritorio remoto (RDP) para obtener directamente en la máquina virtual del host de sesión como administrador local.
2. Descargue e importe [Windows Virtual Desktop PowerShell module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) usar en la sesión de PowerShell si no lo ha hecho ya.
3. Instalar la pila en paralelo usando [creación de un grupo host con PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).

## <a name="how-to-fix-a-windows-virtual-desktop-side-by-side-stack-that-malfunctions"></a>Cómo corregir una pila de side-by-side de Escritorio Virtual de Windows que no funciona correctamente

Existen conocidos circunstancias que pueden causar la pila en paralelo no funcione correctamente:

- No, siguiendo el orden correcto de los pasos para habilitar la pila en paralelo
- Actualización automática para Windows 10 mejorado versátil disco (EVD)
- Falta el rol de Host de sesión de escritorio remoto (RDSH)
- Ejecutar enablesxsstackrc.ps1 varias veces
- Ejecutando enablesxsstackrc.ps1 en una cuenta que no tiene privilegios de administrador local

Las instrucciones de esta sección pueden ayudarle a desinstalar la pila de Escritorio Virtual de Windows en paralelo. Una vez que se desinstale la pila en paralelo, vaya a "Registrar la máquina virtual con el grupo de host de Escritorio Virtual de Windows" en [creación de un grupo host con PowerShell](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) para volver a instalar la pila en paralelo.

La máquina virtual utilizada para ejecutar la corrección debe estar en la misma subred y el mismo dominio que la máquina virtual con la pila en paralelo que no funciona correctamente.

Siga estas instrucciones para ejecutar la corrección de la misma subred y dominio:

1. Conectar con protocolo estándar de escritorio remoto (RDP) a la máquina virtual desde donde se aplicará la revisión.
2. Descargar PsExec de https://docs.microsoft.com/sysinternals/downloads/psexec.
3. Descomprima el archivo descargado.
4. Inicie el símbolo del sistema como administrador local.
5. Navegue hasta la carpeta donde ha descomprimido PsExec.
6. Desde el símbolo del sistema, use el siguiente comando:

    ```cmd
            psexec.exe \\<VMname> cmd
    ```

    >[!Note]
    >VMname es el nombre del equipo de la máquina virtual con la pila en paralelo que no funciona correctamente.

7. Acepte el contrato de licencia de PsExec, haga clic en Aceptar.

    ![Captura de pantalla de contrato de licencia de software.](media/SoftwareLicenseTerms.png)

    >[!Note]
    >Este cuadro de diálogo se muestra solo la primera vez que se ejecuta PsExec.

8. Una vez abierta la sesión de símbolo del sistema en la máquina virtual con la pila en paralelo que no funciona correctamente, ejecute qwinsta y confirme que está disponible una entrada denominada rdp-sxs. Si no es así, una pila en paralelo no está presente en la máquina virtual para que el problema no está vinculado a la pila en paralelo.

    ![Símbolo del sistema de administrador](media/AdministratorCommandPrompt.png)

9. Ejecute el comando siguiente, que mostrará una lista de componentes de Microsoft instalados en la máquina virtual con la pila en paralelo que no funciona correctamente.

    ```cmd
        wmic product get name
    ```

10. Ejecute el comando siguiente con los nombres de producto del paso anterior.

    ```cmd
        wmic product where name="<Remote Desktop Services Infrastructure Agent>" call uninstall
    ```

11. Desinstalar todos los productos que empiecen por "Escritorio remoto".

12. Una vez se hayan desinstalado todos los componentes de Escritorio Virtual de Windows, siga las instrucciones para su sistema operativo:

13. Si el sistema operativo es Windows Server, reinicie la máquina virtual que tenía la pila en paralelo que no funciona correctamente (ya sea con Azure portal o desde la herramienta de PsExec).

Si el sistema operativo es Microsoft Windows 10, continúe con las instrucciones siguientes:

14. Desde la máquina virtual ejecuta PsExec, abra el Explorador de archivos y copie disablesxsstackrc.ps1 en la unidad del sistema de la máquina virtual con la pila side-by-side funcionamiento incorrecto.

    ```cmd
        \\<VMname>\c$\
    ```

    >[!NOTE]
    >VMname es el nombre del equipo de la máquina virtual con la pila en paralelo que no funciona correctamente.

15. El proceso recomendado: desde la herramienta de PsExec, inicie PowerShell y navegue hasta la carpeta en el paso anterior y ejecute disablesxsstackrc.ps1. Como alternativa, puede ejecutar los siguientes cmdlets:

    ```PowerShell
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\ClusterSettings" -Name "SessionDirectoryListener" -Force
    Remove-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" -Recurse -Force
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations" -Name "ReverseConnectionListener" -Force
    ```

16. Cuando se realizan los cmdlets en ejecución, reinicie la máquina virtual con la pila en paralelo que no funciona correctamente.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general sobre cómo solucionar problemas de Escritorio Virtual de Windows y las pistas de escalado, consulte [solución de problemas de introducción, comentarios y soporte técnico](troubleshoot-set-up-overview.md).
- Para solucionar problemas durante la creación de un grupo de inquilinos y host en un entorno de Escritorio Virtual de Windows, consulte [creación del grupo de inquilinos y host](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en el escritorio Virtual de Windows, consulte [configuración de máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas con conexiones de cliente de Escritorio Virtual de Windows, consulte [las conexiones de cliente de escritorio remoto](troubleshoot-client-connection.md).
- Para solucionar problemas al usar PowerShell con el escritorio Virtual de Windows, consulte [Windows PowerShell de Escritorio Virtual](troubleshoot-powershell.md).
- Para obtener más información sobre el servicio en versión preliminar, consulte [entorno de versión preliminar de Windows Desktop](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de implementaciones de plantilla de Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Para más información sobre las acciones de auditoría, consulte [Operaciones de auditoría con Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Si desea conocer más detalles sobre las acciones que permiten determinar los errores durante la implementación, consulte [Visualización de operaciones de implementación con el Portal de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).