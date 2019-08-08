---
title: 'Conexiones de cliente de Escritorio remoto en Windows Virtual Desktop: Azure'
description: Cómo resolver problemas al configurar conexiones de cliente en un entorno de inquilinos de Windows Virtual Desktop.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/08/2019
ms.author: helohr
ms.openlocfilehash: 9cd754b1810595c3ae82a7e4edfd9a3abe145b3f
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68816419"
---
# <a name="remote-desktop-client-connections"></a>Conexiones de cliente de Escritorio remoto

Use este artículo para resolver problemas con conexiones de cliente de Windows Virtual Desktop.

## <a name="provide-feedback"></a>Envío de comentarios

En este momento no se aceptan casos de soporte técnico mientras Windows Virtual Desktop se encuentre en versión preliminar. Visite la [Comunidad técnica de Windows Virtual Desktop](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) para hablar sobre Windows Virtual Desktop con el equipo de producto y los miembros activos de la comunidad.

## <a name="you-cant-open-a-web-client"></a>No se puede abrir un cliente web.

Abra otro sitio web, por ejemplo, [www.Bing.com](https://www.bing.com), para confirmar que hay conectividad a Internet.

Use **nslookup** para confirmar que DNS puede resolver el FQDN:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Intente conectarse con otro cliente, como el cliente de Escritorio remoto para Windows 7 o Windows 10, y compruebe si puede abrir el cliente web.

### <a name="error-opening-another-site-fails"></a>Error: error al abrir otro sitio

**Causa:** Interrupciones o problemas de red.

**Solución:** Póngase en contacto con el soporte técnico para redes.

### <a name="error-nslookup-cannot-resolve-the-name"></a>Error: Nslookup no puede resolver el nombre

**Causa:** Interrupciones o problemas de red.

**Solución:** Póngase en contacto con el soporte técnico para redes.

### <a name="error-you-cant-connect-but-other-clients-can-connect"></a>Error: No puede conectarse, pero otros clientes sí

**Causa:** El explorador no se comporta según lo esperado y ha dejado de funcionar.

**Solución:** Siga estas instrucciones para solucionar problemas del explorador.

1. Reinicie el explorador.
2. Borre las cookies del explorador. Consulte [Cómo eliminar archivos de cookies en Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Borre la memoria caché del explorador Consulte [Borrar la memoria caché del explorador](https://binged.it/2RKyfdU).
4. Abra el explorador en modo privado.

## <a name="web-client-stops-responding-or-disconnects"></a>El cliente web deja de responder o se desconecta

Intente conectarse con otro explorador o cliente.

### <a name="error-other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Error: Otros exploradores y clientes también funcionan incorrectamente o no se abren

**Causa:** Interrupciones o problemas de red o del sistema operativo.

**Solución:** Póngase en contacto con los equipos de soporte técnico.

## <a name="web-client-keeps-prompting-for-credentials"></a>El cliente web sigue solicitando credenciales

Si el cliente web sigue solicitando credenciales, siga estas instrucciones.

1. Confirme que la dirección URL del cliente web es correcta.
2. Confirme que las credenciales son para el entorno de Windows Virtual Desktop asociado a la dirección URL.
3. Borre las cookies del explorador. Consulte [Cómo eliminar archivos de cookies en Internet Explorer](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Borre la memoria caché del explorador Consulte [Borrar la memoria caché del explorador](https://binged.it/2RKyfdU).
5. Abra el explorador en modo privado.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>El cliente de Escritorio remoto para Windows 7 o Windows 10 deja de responder o no se puede abrir

Use los siguientes cmdlets de PowerShell para limpiar los registros de cliente fuera de banda (OOB).

```PowerShell
Remove-ItemProperty 'HKCU:\Software\Microsoft\Terminal Server Client\Default' - Name FeedURLs

#Remove RdClientRadc registry key
Remove-Item 'HKCU:\Software\Microsoft\RdClientRadc' -Recurse

#Remove all files under %appdata%\RdClientRadc
Remove-Item C:\Users\pavithir\AppData\Roaming\RdClientRadc\* -Recurse
```

Vaya a **%AppData%\RdClientRadc** y elimine todo el contenido.

Desinstale y vuelva a instalar el cliente de Escritorio remoto para Windows 7 y Windows 10.

## <a name="troubleshooting-end-user-connectivity"></a>Solución de problemas de conectividad del usuario final

A veces, los usuarios pueden tener acceso a los recursos locales y a la fuente, pero, aún así, tener problemas de rendimiento, disponibilidad o configuración que les impiden tener acceso a recursos remotos. En estos casos, el usuario obtiene mensajes similares a los siguientes:

![Mensaje de error de conexión a Escritorio remoto.](media/eb76b666808bddb611448dfb621152ce.png)

![Mensaje de error que indica que no se puede conectar a la puerta de enlace.](media/a8fbb9910d4672147335550affe58481.png)

Siga estas instrucciones para solucionar problemas generales para los códigos de error de conexión de cliente.

1. Confirme el nombre de usuario y la hora del momento en que se produjo el problema.
2. Abra **PowerShell** y establezca conexión con el inquilino de Windows Virtual Desktop donde se notificó el problema.
3. Confirme la conexión al inquilino correcto con **Get-RdsTenant**.
4. Use los cmdlets **Get-RdsHostPool** y **Get-RdsSessionHost** para confirmar que la solución de problemas se realiza en el grupo de hosts correcto.
5. Ejecute el comando siguiente para obtener una lista de todas las actividades con errores de conexión para el período de tiempo especificado:

    ```PowerShell
     Get-RdsDiagnosticActivities -TenantName <TenantName> -username <UPN> -StartTime
     "11/21/2018 1:07:03 PM" -EndTime "11/21/2018 1:27:03 PM" -Outcome Failure -ActivityType Connection
    ```

6. Mediante el **ActivityId** de la anterior salida del cmdlet, ejecute el siguiente comando:

    ```PowerShell
    (Get-RdsDiagnosticActivities -TenantName $tenant -ActivityId <ActivityId> -Detailed).Errors
    ```

7. El comando genera una salida similar a la que se muestra a continuación. Use **ErrorCodeSymbolic** y **ErrorMessage** para solucionar la causa principal.

    ```PowerShell
    ErrorSource       : <Source>
    ErrorOperation    : <Operation>
    ErrorCode         : <Error code>
    ErrorCodeSymbolic : <Error code string>
    ErrorMessage      : <Error code message>
    ErrorInternal     : <Internal for the OS>
    ReportedBy        : <Reported by component>
    Time              : <Timestampt>
    ```

### <a name="error-o_add_user_to_group_failed--failed-to-add-user--username-to-group--remote-desktop-users-reason-win32error_no_such_member"></a>Error: O_ADD_USER_TO_GROUP_FAILED / No se pudo agregar el usuario = ≤username≥ al grupo = Usuarios de Escritorio remoto. Motivo: Win32.ERROR_NO_SUCH_MEMBER

**Causa:** La VM no se ha unido al dominio donde está el objeto de usuario.

**Solución:** Agregue la VM al dominio correcto. Consulte [Unión de una máquina virtual de Windows Server a un dominio administrado](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal).

### <a name="error-nslookup-cannot-resolve-the-name"></a>Error: Nslookup no puede resolver el nombre

**Causa:** Interrupciones o problemas de red.

**Solución:** Póngase en contacto con el soporte técnico para redes.

### <a name="error-connectionfailedclientprotocolerror"></a>Error: ConnectionFailedClientProtocolError

**Causa:** Las VM a las que el usuario intenta conectarse no están unidas a un dominio.

**Solución:** Una todas las VM que forman parte de un grupo de hosts al controlador de dominio.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>El usuario se conecta, pero no se muestra nada (ninguna fuente)

Un usuario puede iniciar clientes de Escritorio remoto y es capaz de realizar la autenticación, pero no ve ningún icono en la fuente de detección de web.

Confirme que el usuario que notificó los problemas se ha asignado a grupos de aplicaciones mediante el uso de esta línea de comandos:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Confirme que el usuario ha iniciado sesión con las credenciales correctas.

Si se usa el cliente web, confirme que no hay ningún problema de credenciales almacenadas en caché.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general sobre cómo solucionar problemas de Windows Virtual Desktop y las pistas de escalación, consulte [Introducción, comentarios y soporte técnico para solucionar problemas](troubleshoot-set-up-overview.md).
- Para solucionar problemas durante la creación de un grupo de inquilinos y de hosts en un entorno de Windows Virtual Desktop, consulte [Creación de los grupos de inquilinos y de host](troubleshoot-set-up-issues.md).
- Para solucionar problemas al configurar una máquina virtual (VM) en Windows Virtual Desktop, consulte [Configuración de la máquina virtual del host de sesión](troubleshoot-vm-configuration.md).
- Para solucionar problemas al usar PowerShell con Windows Virtual Desktop, consulte [PowerShell para Windows Virtual Desktop](troubleshoot-powershell.md).
- Para más información acerca del servicio en versión preliminar, consulte [Entorno de versión preliminar de Windows Virtual Desktop](https://docs.microsoft.com/azure/virtual-desktop/environment-setup?).
- Para realizar un tutorial de solución de problemas, consulte [Tutorial: Solución de problemas de las implementaciones de plantillas de Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Para más información sobre las acciones de auditoría, consulte [Operaciones de auditoría con Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Si desea conocer más detalles sobre las acciones que permiten determinar los errores durante la implementación, consulte [Visualización de operaciones de implementación con el Portal de Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).
