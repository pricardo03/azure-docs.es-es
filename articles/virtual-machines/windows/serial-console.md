---
title: Consola serie de máquina virtual de Azure | Microsoft Docs
description: Consola serie bidireccional para máquinas virtuales de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 08/07/2018
ms.author: harijay
ms.openlocfilehash: 4e93e455e309771ed3e33382ee49cdc144036fb1
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782420"
---
# <a name="virtual-machine-serial-console-preview"></a>Consola serie de máquina virtual (versión preliminar) 


La consola serie de máquina virtual en Azure ofrece acceso a una consola basada en texto para máquinas virtuales de Windows. Esta es una conexión de serie al puerto de serie COM1 de la máquina virtual y ofrece acceso a la misma, sin estar relacionada con el estado del sistema operativo o de la red de la máquina virtual. En este momento, el acceso a la consola serie para una máquina virtual solo es posible mediante Azure Portal, y se permite únicamente a los usuarios que tengan acceso de colaborador o superior a la máquina virtual. 

Para obtener documentación de la consola serie para VM Linux, [haga clic aquí](../linux/serial-console.md).

> [!Note] 
> Las versiones preliminares están a su disposición con la condición de que acepte los términos de uso. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Actualmente este servicio está en **versión preliminar pública** y el acceso a la consola serie para las máquinas virtuales está disponible para las regiones globales de Azure. En este momento, la consola serie no está disponible en las nubes Azure Government, Azure Alemania y Azure China.

 

## <a name="prerequisites"></a>Requisitos previos 

* Debe utilizar el modelo de implementación de Resource Manager. No se admiten implementaciones clásicas. 
* La máquina virtual TIENE QUE tener los [diagnósticos de arranque](boot-diagnostics.md) habilitados 

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* La cuenta que utilice la consola serie tiene que tener el [rol Colaborador](../../role-based-access-control/built-in-roles.md) para la máquina virtual y la cuenta de almacenamiento de [diagnósticos de arranque](boot-diagnostics.md). 
* La máquina virtual para la que está accediendo a la consola serie también debe tener una cuenta basada en contraseñas. Puede crear una con la funcionalidad [restablecer contraseña](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) de la extensión de acceso de máquina virtual. Consulte la captura de pantalla siguiente.

    ![](../media/virtual-machines-serial-console/virtual-machine-serial-console-reset-password.png)

## <a name="get-started-with-serial-console"></a>Introducción al uso de la consola serie
La consola serie para las máquinas virtuales solo es accesible mediante [Azure Portal](https://portal.azure.com). A continuación se muestran los pasos para acceder a la consola serie de las máquinas virtuales a través del portal.

  1. Abra Azure Portal.
  2. En el menú de la izquierda, seleccione Máquinas virtuales.
  3. Haga clic en la máquina virtual en la lista. Se abrirá la página de información general de la máquina virtual.
  4. Desplácese hacia abajo hasta la sección de Soporte técnico y solución de problemas y haga clic en la opción de la consola serie (versión preliminar). Se abrirá un panel nuevo con la consola serie y se iniciará la conexión.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

## <a name="enable-serial-console-in-custom-or-older-images"></a>Habilitación de la consola serie en imágenes personalizadas o antiguas
Las imágenes más nuevas de Windows Server en Azure tendrán la [consola de administración especial](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) habilitada de forma predeterminada. SAC se admite en versiones de servidor de Windows, pero no está disponible en versiones de cliente (por ejemplo, Windows 10, Windows 8 o Windows 7). Para habilitar la consola serie en máquinas virtuales de Windows creadas con imágenes anteriores a febrero de 2018, siga estos pasos: 

1. Conéctese a la máquina virtual Windows mediante Escritorio remoto
2. Ejecute los siguientes comandos como administrador en un símbolo del sistema 
* `bcdedit /ems {current} on`
* `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
3. Reinicie el sistema para que la consola de SAC se habilite

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Si es necesario SAC también puede habilitarse sin conexión, 

1. Conecte el disco de Windows para el que desee Configurar SAC como disco de datos a la máquina virtual existente. 
2. Ejecute los siguientes comandos como administrador en un símbolo del sistema 
* `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
* `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled"></a>¿Cómo se puede saber si SAC está habilitada o no?

Si [SAC] (https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) no está habilitada la consola serie no mostrará el símbolo del sistema de SAC. En algunos casos, se mostrará la información de estado de la máquina virtual y, en otros casos, se quedará en blanco.  

## <a name="enable-the-windows-boot-menu-in-serial-console"></a>Habilitación del menú de arranque de Windows en la consola serie 

Si tiene que habilitar los mensajes del cargador de arranque de Windows para mostrar la consola serie, puede agregar las siguientes opciones adicionales a los datos de la configuración de arranque. Consulte [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set) para obtener más detalles

1. Conéctese a la máquina virtual Windows mediante Escritorio remoto
2. Ejecute los siguientes comandos como administrador en un símbolo del sistema 
* `bcdedit /set {bootmgr} displaybootmenu yes`
* `bcdedit /set {bootmgr} timeout 5`
* `bcdedit /set {bootmgr} bootems yes`
3. Reinicie el sistema para que el menú de arranque se habilite

## <a name="use-serial-console-for-nmi-calls-in-windows-vms"></a>Uso de la consola serie para llamadas NMI en máquinas virtuales de Windows
Una interrupción no enmascarable (NMI) está diseñada para crear una señal que el software de una máquina virtual no pasará por alto. Históricamente, las NMI se han usado para supervisar problemas de hardware en sistemas que necesitaban tiempos de respuesta específicos.  En la actualidad, los administradores del sistema y programadores suelen usar NMI como mecanismo para depurar o solucionar los problemas de sistemas que se han bloqueado.

La consola serie se puede usar para enviar una NMI a una máquina virtual de Azure mediante el icono de teclado de la barra de comandos que se muestra a continuación. Una vez que se entrega la NMI, la configuración de la máquina virtual controla cómo responde el sistema. Windows puede configurarse para bloquearse y crear un volcado de memoria cuando recibe una NMI.

![](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Para obtener información sobre cómo configurar Windows para crear un archivo de volcado cuando recibe una NMI, consulte: [Cómo generar un archivo de volcado completo o un archivo de volcado del núcleo utilizando un NMI en un sistema basado en Windows](https://support.microsoft.com/en-us/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file)

## <a name="disable-serial-console"></a>Deshabilitación de la consola serie
De forma predeterminada, todas las suscripciones tienen el acceso a la consola serie habilitado para todas las VM. Puede deshabilitar la consola serie en el nivel de suscripción o el nivel de VM.

### <a name="subscription-level-disable"></a>Deshabilitación a nivel de supervisión
La consola serie puede deshabilitarse para toda una suscripción a través de la [llamada a la API de REST Disable Console](https://aka.ms/disableserialconsoleapi). Puede usar la funcionalidad "Pruébelo" disponible en la página de documentación de la API para deshabilitar y habilitar la consola serie para una suscripción. Escriba su `subscriptionId`, "default" en el campo `default` y haga clic en Ejecutar. Los comandos de la CLI de Azure todavía no están disponibles y llegarán en una fecha posterior. [Pruebe la llamada de API de REST aquí](https://aka.ms/disableserialconsoleapi).

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Como alternativa, puede usar el conjunto de comandos siguiente en Cloud Shell (comandos de bash que se muestran) para deshabilitar, habilitar y ver el estado deshabilitado de la consola serie para una suscripción. 

* Para obtener el estado deshabilitado de la consola serie para una suscripción:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* Para deshabilitar la consola serie para una suscripción:
    ```azurecli-interactive 
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* Para habilitar la consola serie para una suscripción:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/enableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```

### <a name="vm-level-disable"></a>Deshabilitación a nivel de VM
La consola serie puede deshabilitarse para determinadas VM al deshabilitar la configuración de diagnóstico de arranque de la VM. Simplemente desactive el diagnóstico de arranque desde Azure Portal, y se deshabilitará la consola serie para la VM.

## <a name="serial-console-security"></a>Seguridad de la consola serie 

### <a name="access-security"></a>Seguridad de acceso 
El acceso a la consola serie está limitado a los usuarios que tienen un acceso de [colaborador de la máquina virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) o superior en relación con la máquina virtual. Si su inquilino de AAD requiere Multi-Factor Authentication, el acceso a la consola serie también necesitará MFA para acceder mediante [Azure Portal](https://portal.azure.com).

### <a name="channel-security"></a>Seguridad del canal
Todos los datos enviados y recibidos se cifran en la conexión.

### <a name="audit-logs"></a>Registros de auditoría
Todo el acceso a la consola serie queda registrado en los registros de los [diagnósticos de arranque](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) de la máquina virtual. El administrador de la máquina virtual de Azure es el propietario y el que controla el acceso a estos registros.  

>[!CAUTION] 
Si bien no se registra ninguna contraseña de acceso para la consola, si los comandos ejecutados en la consola contienen o producen contraseñas, secretos, nombres de usuario o cualquier otra forma de información de identificación personal (PII), estos se escribirán en los registros de los diagnósticos de arranque de la máquina virtual, junto con todo el demás texto visible, como parte de la implementación de la funcionalidad de desplazamiento de la consola serie. Estos registros son circulares y solo aquellas personas con permisos de lectura para la cuenta de almacenamiento de diagnósticos tienen acceso a ellos; de todas formas, se recomienda seguir la práctica recomendada de usar el Escritorio remoto para cualquier elemento que pueda implicar secretos o información de identificación personal. 

### <a name="concurrent-usage"></a>Uso simultáneo
Si un usuario se conecta a la consola serie y otro usuario solicita correctamente el acceso a esa misma máquina virtual, se desconectará al primer usuario y se conectará al segundo de una forma similar a la acción del primer usuario levantándose y abandonando la consola física y el nuevo usuario sentándose a ocupar su sitio.

>[!CAUTION] 
Esto quiere decir que no se cerrará la sesión del usuario que se desconecta. La posibilidad de forzar el cierre de sesión con la desconexión (mediante SIGHUP u otro mecanismo similar) sigue en proceso de valoración. En Windows hay un tiempo de espera automático habilitado en SAC; sin embargo, en Linux puede configurar el valor de tiempo de espera del terminal. 

## <a name="common-scenarios-for-accessing-serial-console"></a>Escenarios comunes para acceder a la consola de serie 
Escenario          | Acciones en la consola de serie                
:------------------|:-----------------------------------------
Reglas de firewall incorrectas | Accede a la consola de serie y corrige las reglas de firewall de Windows. 
Comprobación o daños en el sistema de archivos | Acceda a la consola de serie y recupere el sistema de archivos. 
Problemas de configuración de RDP | Acceda a la consola de serie y cambie la configuración. Vaya a la [documentación de RDP](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access) para empezar a trabajar.
Sistema de bloqueo de red| Acceda a la consola de serie a través del portal para administrar el sistema. Algunos comandos de red se muestran en la [documentación de CMD y PowerShell de la consola serie](./serial-console-cmd-ps-commands.md). 
Interacción con el cargador de arranque | Acceda a BCD mediante la consola serie. Vaya a [Habilitación del menú de arranque para mostrarlo en la consola serie](#enabling-boot-menu-to-show-in-the-serial-console) para empezar a trabajar. 

## <a name="accessibility"></a>Accesibilidad
La accesibilidad es un factor clave de la consola serie de Azure. Para ello, nos hemos asegurado de que la consola serie es accesible para quienes tienen dificultades auditivas y visuales, así como para las personas que no pueden utilizar un mouse.

### <a name="keyboard-navigation"></a>Navegación con el teclado
Use la tecla `tab` del teclado para navegar por la interfaz de la consola serie en Azure Portal. La ubicación se resaltará en pantalla. Para salir del foco de la hoja de la consola serie, presione `Ctrl + F6` en el teclado.

### <a name="use-serial-console-with-a-screen-reader"></a>Uso de la consola serie con un lector de pantalla
La consola serie tiene integrada la compatibilidad con lectores de pantalla. Si navega con el lector de pantalla activado, el lector de pantalla podrá leer en voz alta el texto alternativo del botón seleccionado actualmente.

## <a name="errors"></a>Errors
La mayoría de errores son transitorios por naturaleza, y basta con intentar establecer la conexión de nuevo para solucionarlos. La tabla siguiente muestra una lista de errores y mitigaciones

Error                            |   Mitigación 
:---------------------------------|:--------------------------------------------|
No se puede recuperar la configuración de diagnóstico de arranque para "<VMNAME>". Para usar la consola serie, asegúrese de que el diagnóstico de arranque está habilitado para esta máquina virtual. | Asegúrese de que la máquina virtual tiene los [diagnósticos de arranque](boot-diagnostics.md) habilitados. 
La máquina virtual se encuentra en un estado desasignado detenido. Inicie la máquina virtual y vuelva a intentar la conexión de la consola serie. | La máquina virtual tiene que estar en un estado iniciado para tener acceso a la consola serie
No tiene los permisos necesarios para usar la consola serie de esta máquina virtual. Asegúrese de tener al menos los permisos del rol de colaborador de máquina virtual.| El acceso a la consola serie requiere cierto nivel de permiso. Consulte los [requisitos de acceso](#prerequisites) para más información
No se puede determinar el grupo de recursos para la cuenta de almacenamiento de diagnósticos de arranque "<STORAGEACCOUNTNAME>". Compruebe que el diagnóstico de arranque está habilitado para esta máquina virtual y que tiene acceso a esta cuenta de almacenamiento. | El acceso a la consola serie requiere cierto nivel de permiso. Consulte los [requisitos de acceso](#prerequisites) para más información
Se encontró una respuesta "Prohibido" al obtener acceso a la cuenta de almacenamiento de diagnóstico de arranque de la VM. | Asegúrese de que el diagnóstico de arranque no tenga un firewall de cuentas. Se necesita una cuenta de almacenamiento de diagnóstico de arranque accesible para que la consola serie funcione.
Socket web está cerrado o no se pudo abrir. | Puede que necesite incluir `*.console.azure.com` en la lista blanca. Un enfoque más detallado, pero más largo, es incluir en la lista blanca los [rangos de IP del centro de datos de Microsoft Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653), que cambian con bastante frecuencia.

## <a name="known-issues"></a>Problemas conocidos 
Como nos encontramos aún en las etapas de versión preliminar para el acceso a la consola serie, estamos trabajando en algunos problemas conocidos. Presentamos aquí la lista con algunas posibles soluciones 

Problema                             |   Mitigación 
:---------------------------------|:--------------------------------------------|
No hay opción para la consola serie con la instancia de conjunto de escalado de máquinas virtuales | Con esta versión preliminar, no se admite el acceso a la consola serie para las instancias del conjunto de escalado de máquinas virtuales.
Al pulsar Entrar tras un banner de conexión no aparece la solicitud de inicio de sesión | Consulte esta página: [Pulsar Entrar no hace nada](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Esto puede ocurrir si está ejecutando una máquina virtual personalizada, un dispositivo reforzado o una configuración de GRUB que hace que Windows no pueda conectarse correctamente al puerto serie.
Se muestra solamente información de mantenimiento cuando se conecta a una máquina virtual Windows| Esto aparecerá si no se ha habilitado la consola de administración especial para la imagen de Windows. Consulte [Acceso a la consola serie para Windows](#access-serial-console-for-windows) para obtener instrucciones sobre cómo habilitar manualmente SAC en la máquina virtual Windows. Se pueden encontrar más detalles en [Señales de mantenimiento de Windows](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).
No se puede escribir en el símbolo del sistema de SAC si la depuración del kernel está habilitada | Conéctese mediante RDP a la máquina virtual y ejecute `bcdedit /debug {current} off` desde un símbolo del sistema con privilegios elevados. Si no puede conectarse mediante RDP, en su lugar puede asociar el disco del sistema operativo a otra máquina virtual de Azure y modificarlo mientras está asociado como un disco de datos mediante `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`. Luego, puede volver a intercambiar el disco.
Pegar en PowerShell en SAC da como resultado un tercer carácter si el contenido original tenía un carácter repetido | Una solución alternativa es quitar el módulo PSReadLine. `Remove-Module PSReadLine` quitará el módulo PSReadLine de la sesión actual.
Algunas entradas de teclado generan resultados extraños de SAC (por ejemplo: `[A`, `[3~`) | Las secuencias de escape [VT100](https://aka.ms/vtsequences) no son compatibles con el símbolo del sistema de SAC.
Se encontró una respuesta "Prohibido" al obtener acceso a la cuenta de almacenamiento de diagnóstico de arranque de la VM. | Asegúrese de que el diagnóstico de arranque no tenga un firewall de cuentas. Se necesita una cuenta de almacenamiento de diagnóstico de arranque accesible para que la consola serie funcione.

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes 
**P. ¿Cómo puedo enviar comentarios?**

A. Envíe comentarios como problemas que tenga acudiendo a https://aka.ms/serialconsolefeedback. También tiene la posibilidad (aunque se prefiere menos) de enviar comentarios a azserialhelp@microsoft.com, o en la categoría de máquinas virtuales en http://feedback.azure.com

**P. No puedo acceder a la consola serie, ¿dónde puedo presentar un caso de soporte técnico?**

A. Esta característica de versión preliminar viene recogida en los términos de las versiones preliminares de Azure. La mejor manera de obtener soporte técnico es recurrir a los canales mencionados anteriormente. 

## <a name="next-steps"></a>Pasos siguientes
* Para obtener una guía detallada para los comandos de CMD y PowerShell que puede usar en SAC de Windows, haga clic en [aquí](serial-console-cmd-ps-commands.md).
* La consola serie también está disponible para VM [Linux](../linux/serial-console.md).
* Obtenga más información sobre [diagnósticos de arranque](boot-diagnostics.md).
