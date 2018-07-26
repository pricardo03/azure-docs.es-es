---
title: Solución de problemas de Azure Cloud Shell | Microsoft Docs
description: Solución de problemas de Azure Cloud Shell
services: azure
documentationcenter: ''
author: maertendMSFT
manager: angelc
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/03/2018
ms.author: damaerte
ms.openlocfilehash: 21bc0633a9cc607325b48998791cb12631ecd0d7
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37856494"
---
# <a name="troubleshooting--limitations-of-azure-cloud-shell"></a>Solución de problemas y limitaciones de Azure Cloud Shell

Entre las resoluciones conocidas de problemas en Azure Cloud Shell se incluyen las siguientes:

## <a name="general-troubleshooting"></a>Solución general de problemas

### <a name="early-timeouts-in-firefox"></a>Tiempos de expiración tempranos en Firefox
- **Detalles**: Cloud Shell usa una conexión Websocket abierta para pasar la entrada/salida al explorador. Firefox tiene directivas preestablecidas que pueden cerrar la conexión WebSocket prematuramente, lo que genera tiempos de expiración tempranos en Cloud Shell.
- **Resolución**: abra FireFox y escriba "about:config" en el cuadro de dirección URL. Busque "network.websocket.timeout.ping.request" y cambie el valor de 0 a 10.

### <a name="storage-dialog---error-403-requestdisallowedbypolicy"></a>Cuadro de diálogo de Storage. Error: 403 RequestDisallowedByPolicy
- **Detalles**: al crear una cuenta de almacenamiento a través de Cloud Shell se produce un error debido a una directiva de Azure creada por el administrador. El mensaje de error incluirá: `The resource action 'Microsoft.Storage/storageAccounts/write' is disallowed by one or more policies.`
- **Resolución**: póngase en contacto con el administrador de Azure para quitar o actualizar la directiva que deniega la creación del almacenamiento.

### <a name="storage-dialog---error-400-disallowedoperation"></a>Cuadro de diálogo de Storage. Error: 400 DisallowedOperation
 - **Detalles**: cuando se usa una suscripción de Azure Active Directory, no puede crear almacenamiento.
 - **Resolución**: use una suscripción de Azure capaz de crear recursos de almacenamiento. Las suscripciones de Azure AD no pueden crear recursos de Azure.

### <a name="terminal-output---error-failed-to-connect-terminal-websocket-cannot-be-established-press-enter-to-reconnect"></a>Salida de terminal. Error: Failed to connect terminal: websocket cannot be established (Error al conectar con el terminal: no se puede establecer el websocket). Presione `Enter` para volver a conectarse.
 - **Detalles**: Cloud Shell requiere la capacidad de establecer una conexión de websocket con la infraestructura de Cloud Shell.
 - **Resolución**: Compruebe que ha configurado las opciones de red para permitir el envío de solicitudes https y websocket para dominios en *.console.azure.com.

## <a name="bash-troubleshooting"></a>Solución de problemas de Bash

### <a name="cannot-run-the-docker-daemon"></a>No se puede ejecutar el demonio de Docker

- **Detalles**: Cloud Shell utiliza un contenedor para hospedar el entorno de shell, lo que hace que no se permita la ejecución del demonio.
- **Resolución**: utilice la [máquina de Docker](https://docs.docker.com/machine/overview/), que se instala de forma predeterminada para administrar contenedores de Docker desde un host de Docker remoto.

## <a name="powershell-troubleshooting"></a>Solución de problemas de PowerShell

### <a name="gui-applications-are-not-supported"></a>No se admiten las aplicaciones con GUI

- **Detalles**: si un usuario inicia una aplicación con GUI, no se devuelve el símbolo del sistema. Por ejemplo, cuando un usuario clona un repositorio GitHub privado que tiene habilitada la autenticación en dos fases, aparece un cuadro de diálogo para completar esta autenticación.  
- **Resolución**: cierre y vuelva a abrir el shell.

### <a name="get-help--online-does-not-open-the-help-page"></a>Get-Help -online no abre la página de ayuda

- **Detalles**: si un usuario escriba `Get-Help Find-Module -online`, aparece un mensaje de error similar a: `Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
- **Resolución**: copie la dirección URL y ábrala manualmente en el explorador.

### <a name="troubleshooting-remote-management-of-azure-vms"></a>Solución de problemas de la administración remota de máquinas virtuales de Azure

- **Detalles**: debido a la configuración predeterminada del Firewall de Windows para WinRM, es posible que el usuario vea el error siguiente: `Ensure the WinRM service is running. Remote Desktop into the VM for the first time and ensure it can be discovered.`
- **Resolución**: ejecute `Enable-AzureRmVMPSRemoting` para habilitar todos los aspectos de la comunicación remota de PowerShell en la máquina de destino.
 

### <a name="dir-caches-the-result-in-azure-drive"></a>`dir` almacena en caché el resultado en la unidad de Azure.

- **Detalles**: el resultado de `dir` se almacena en caché en la unidad de Azure.
- **Resolución**: después de crear o quitar un recurso en la vista de la unidad de Azure, ejecute `dir -force` para actualizar.

## <a name="general-limitations"></a>Limitaciones generales
Azure Cloud Shell tiene las limitaciones conocidas siguientes:

### <a name="system-state-and-persistence"></a>Persistencia y estado del sistema

La máquina que proporciona la sesión de Cloud Shell es temporal y se recicla después de que la sesión esté inactiva durante 20 minutos. Cloud Shell requiere montar un recurso compartido de archivos de Azure. Como resultado, la suscripción debe poder configurar los recursos de almacenamiento para tener acceso a Cloud Shell. Otras consideraciones:

* Con el almacenamiento montado, solo se conservan las modificaciones dentro del directorio `clouddrive`. En Bash, también se conserva el directorio `$Home`.
* Solo se pueden montar recursos compartidos de archivos de Azure desde la [región asignada](persisting-shell-storage.md#mount-a-new-clouddrive).
  * En Bash, ejecute `env` para buscar la región establecida como `ACC_LOCATION`.
* Azure Files solo admite cuentas de almacenamiento con redundancia local o de almacenamiento con redundancia geográfica.

### <a name="browser-support"></a>Compatibilidad con exploradores

Cloud Shell es compatible con las versiones más recientes de Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox y Safari de Apple. Safari en modo privado no es compatible.

### <a name="copy-and-paste"></a>Copiar y pegar

[!include [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Solo puede haber un shell activo para un usuario determinado

Los usuarios solo pueden iniciar un tipo de shell a la vez, ya sea **Bash** o **PowerShell**. Pero puede haber varias instancias de Bash o PowerShell en ejecución al mismo tiempo. Los cambios entre Bash y PowerShell reinician Cloud Shell, lo que finaliza las sesiones existentes.

### <a name="usage-limits"></a>Límites de uso

Cloud Shell está pensado para casos de uso interactivos. Por tanto, todas las sesiones que no sean de este tipo y que se prolonguen durante mucho tiempo se finalizarán sin previo aviso.

## <a name="bash-limitations"></a>Limitaciones de Bash

### <a name="user-permissions"></a>Permisos de usuario

Los permisos se establecen como usuarios normales sin acceso a sudo. No se conserva cualquier instalación fuera del directorio `$Home`.

### <a name="editing-bashrc"></a>Editar .bashrc

Tenga cuidado al editar .bashrc, ya que puede producir errores inesperados en Cloud Shell.

## <a name="powershell-limitations"></a>Limitaciones de PowerShell

### <a name="azuread-module-name"></a>Nombre del módulo `AzureAD`

El nombre del módulo `AzureAD` es actualmente `AzureAD.Standard.Preview`y proporciona la misma funcionalidad.

### <a name="sqlserver-module-functionality"></a>Funcionalidad del módulo `SqlServer`

El módulo `SqlServer` incluido en Cloud Shell solo es compatible con versiones preliminares de PowerShell Core. En concreto, `Invoke-SqlCmd` aún no está disponible.

### <a name="default-file-location-when-created-from-azure-drive"></a>Ubicación del archivo predeterminada cuando se crea a partir de la unidad de Azure:

Con los cmdlets de PowerShell, los usuarios no pueden crear archivos en la unidad de Azure. Cuando los usuarios crean nuevos archivos con otras herramientas, como vim o nano, los archivos se guardan de forma predeterminada en `$HOME`. 

### <a name="gui-applications-are-not-supported"></a>No se admiten las aplicaciones con GUI

Si el usuario ejecuta un comando que crearía un cuadro de diálogo de Windows, como `Connect-AzureAD` o `Connect-AzureRmAccount`, se ve un mensaje de error como el siguiente: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="tab-completion-crashes-psreadline"></a>La finalización con tabulación bloquea PSReadline

Si el modo EditMode del usuario en PSReadline se establece en Emacs, el usuario intentará mostrar todas las posibilidades mediante la finalización con tabulación y el tamaño de la ventana será demasiado pequeño para mostrarlas todas y PSReadline se bloqueará.

### <a name="large-gap-after-displaying-progress-bar"></a>Espacio grande después de mostrar la barra de progreso

Si el usuario realiza una acción que muestra una barra de progreso, como una finalización con tabulación mientras está en la unidad `Azure:`, puede que el cursor no se establezca correctamente y que aparezca un espacio donde antes aparecía la barra de progreso.

### <a name="random-characters-appear-inline"></a>Aparecen caracteres aleatorios insertados

Los códigos de secuencia de posición del cursor como, por ejemplo `5;13R`, pueden aparecer en la entrada del usuario.  Los caracteres se pueden quitar manualmente.

## <a name="personal-data-in-cloud-shell"></a>Datos personales en Cloud Shell

Azure Cloud Shell se toma en serio sus datos, los datos capturados y almacenados por el servicio Azure Cloud Shell se usan para proporcionar los valores predeterminados de su experiencia, tales como el shell que ha utilizado más recientemente, el tamaño y el tipo de fuente que prefiere y los detalles de recursos compartidos de archivos que respaldan la unidad en la nube. En caso de que quiera exportar o eliminar estos datos, hemos incluido las siguientes instrucciones.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

### <a name="export"></a>Exportación
Para poder **exportar** la configuración de usuario, Cloud Shell guarda automáticamente datos tales como el shell, el tamaño y el tipo de fuente preferidos ejecutando los comandos siguientes.

1. Inicio de Bash en Cloud Shell
2. Ejecute los comandos siguientes:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token" -s | jq
```

### <a name="delete"></a>Eliminar
Para poder **eliminar** la configuración de usuario, Cloud Shell guarda automáticamente datos tales como el shell, el tamaño y el tipo de fuente preferidos ejecutando los comandos siguientes. La próxima vez que inicie Cloud Shell se le pedirá que vuelva a incorporar un recurso compartido de archivos. 

El recurso compartido de archivos real de Azure Files no se eliminará si elimina la configuración de usuario. Vaya a Azure Files para completar esa acción.

1. Inicio de Bash en Cloud Shell
2. Ejecute los comandos siguientes:
```
user@Azure:~$ token="Bearer $(curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true -s | jq -r ".access_token")"
user@Azure:~$ curl -X DELETE https://management.azure.com/providers/Microsoft.Portal/usersettings/cloudconsole?api-version=2017-12-01-preview -H Authorization:"$token"
```
