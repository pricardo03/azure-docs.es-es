---
title: Consola serie de máquina virtual en Azure | Microsoft Docs
description: Consola serie bidireccional para máquinas virtuales de Azure.
services: virtual-machines-linux
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/07/2018
ms.author: harijay
ms.openlocfilehash: 20bd2d61671d89a5c2a13525ea119595cf0b7c93
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "40246642"
---
# <a name="virtual-machine-serial-console-preview"></a>Consola serie de máquina virtual (versión preliminar) 


La consola serie de máquina virtual en Azure ofrece acceso a una consola basada en texto para máquinas virtuales Linux y Windows. Esta conexión de serie es al puerto de serie COM1 de la máquina virtual y ofrece acceso a la misma, sin estar relacionada con el estado del sistema operativo o la red de la máquina virtual. En este momento, el acceso a la consola serie para una máquina virtual solo es posible mediante Azure Portal, y se permite únicamente a los usuarios que tengan acceso de colaborador o superior a la máquina virtual. 

Para obtener documentación de la consola serie para VM Windows, [haga clic aquí](../windows/serial-console.md).

> [!Note] 
> Las versiones preliminares están a su disposición con la condición de que acepte los términos de uso. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Actualmente este servicio está en **versión preliminar pública** y el acceso a la consola serie para las máquinas virtuales está disponible para las regiones globales de Azure. En este momento, la consola serie no está disponible en las nubes Azure Government, Azure Alemania y Azure China.


## <a name="prerequisites"></a>Requisitos previos 

* Debe utilizar el modelo de implementación de Resource Manager. No se admiten implementaciones clásicas. 
* La máquina virtual TIENE QUE tener los [diagnósticos de arranque](boot-diagnostics.md) habilitados 
* La cuenta que utilice la consola serie tiene que tener el [rol Colaborador](../../role-based-access-control/built-in-roles.md) para la máquina virtual y la cuenta de almacenamiento de [diagnósticos de arranque](boot-diagnostics.md). 
* Para conocer valores específicos de distribución de Linux, consulte [Acceso a la consola serie para Linux](#access-serial-console-for-linux).


## <a name="open-the-serial-console"></a>Apertura de la consola serie
La consola serie para las máquinas virtuales solo es accesible mediante [Azure Portal](https://portal.azure.com). A continuación se muestran los pasos requeridos para acceder a la consola de serie para las máquinas virtuales a través del portal. 

  1. Abra Azure Portal.
  2. En el menú de la izquierda, seleccione Máquinas virtuales.
  3. Haga clic en la máquina virtual en la lista. Se abrirá la página de información general para la VM.
  4. Desplácese hacia abajo hasta la sección Soporte y solución de problemas y haga clic en la opción de la consola de serie (versión preliminar). Se abrirá un panel nuevo con la consola de serie y se iniciará la conexión.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> La consola de serie requiere un usuario local con una contraseña configurada. En este momento, las máquinas virtuales configuradas solo con la clave pública SSH no tendrán acceso a la consola de serie. Para crear un usuario local con contraseña, utilice la [extensión VMAccess](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) (también disponible en el portal al hacer clic en "Restablecer contraseña") y cree un usuario local con una contraseña.

## <a name="disable-serial-console"></a>Deshabilitación de la consola serie
De forma predeterminada, todas las suscripciones tienen el acceso a la consola serie habilitado para todas las VM. Puede deshabilitar la consola serie en el nivel de suscripción o el nivel de VM.

### <a name="subscription-level-disable"></a>Deshabilitación a nivel de supervisión
La consola serie puede deshabilitarse para toda una suscripción a través de la [llamada a la API de REST Disable Console](https://aka.ms/disableserialconsoleapi). Puede usar la funcionalidad "Pruébelo" disponible en la página de documentación de la API para deshabilitar y habilitar la consola serie para una suscripción. Escriba su `subscriptionId`, "default" en el campo `default` y haga clic en Ejecutar. Los comandos de la CLI de Azure todavía no están disponibles y llegarán en una fecha posterior. [Pruebe la llamada de API de REST aquí](https://aka.ms/disableserialconsoleapi).

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

Como alternativa, puede usar el conjunto de comandos siguiente en Cloud Shell (comandos de bash que se muestran) para deshabilitar, habilitar y ver el estado deshabilitado de la consola serie para una suscripción. 

* Para obtener el estado deshabilitado de la consola serie para una suscripción:
    ```
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* Para deshabilitar la consola serie para una suscripción:
    ```
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"')) 

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* Para habilitar la consola serie para una suscripción:
    ```
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
Si bien no se registra ninguna contraseña de acceso para la consola, si los comandos ejecutados en la consola contienen o producen contraseñas, secretos, nombres de usuario o cualquier otra forma de información de identificación personal, estos se escribirán en los registros de diagnósticos de arranque de la máquina virtual, junto con todo el demás texto visible, como parte de la aplicación de la funcionalidad de desplazamiento de la consola de serie. Estos registros son circulares y solo aquellas personas con permisos de lectura para la cuenta de almacenamiento de diagnósticos tienen acceso a ellos; sin embargo, se recomienda seguir la práctica recomendada de usar la consola SSH para cualquier elemento que pueda implicar secretos o información de identificación personal. 

### <a name="concurrent-usage"></a>Uso simultáneo
Si un usuario se conecta a la consola serie y otro usuario solicita correctamente el acceso a esa misma máquina virtual, se desconectará al primer usuario y se conectará al segundo de una forma similar a la acción del primer usuario levantándose y abandonando la consola física y el nuevo usuario sentándose a ocupar su sitio.

>[!CAUTION] 
Esto quiere decir que no se cerrará la sesión del usuario que se desconecta. La posibilidad de forzar el cierre de sesión con la desconexión (mediante SIGHUP u otro mecanismo similar) sigue en proceso de valoración. Para Windows hay un tiempo de espera automático habilitado en SAC, sin embargo para Linux puede configurar el parámetro de tiempo de espera terminal. Basta con agregar `export TMOUT=600` en .bash_profile o .profile para el usuario con el que inicia sesión en la consola para que el tiempo de espera de la sesión finalice una vez transcurridos 10 minutos.

### <a name="disable-feature"></a>Deshabilitación de la característica
La funcionalidad de la consola de serie puede desactivarse para determinadas máquinas virtuales con la deshabilitación de ese parámetro de diagnósticos de arranque de la VM.

## <a name="common-scenarios-for-accessing-serial-console"></a>Escenarios comunes para acceder a la consola de serie 
Escenario          | Acciones en la consola de serie                |  Aplicabilidad de SO 
:------------------|:-----------------------------------------|:------------------
Archivo FSTAB roto | `Enter` la clave para continuar y corrija el archivo fstab con un editor de texto. Es posible que deba estar en modo de usuario único para esto. Para empezar a trabajar, consulte la [solución de problemas de fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) y [Using Serial Console to access GRUB and Single User Mode](serial-console-grub-single-user-mode.md) (Uso de la consola serie para acceder a GRUB y el modo de usuario único). | Linux 
Reglas de firewall incorrectas | Acceda a la consola de serie y corrija iptables o las reglas de firewall de Windows. | Linux y Windows 
Comprobación o daños en el sistema de archivos | Acceda a la consola de serie y recupere el sistema de archivos. | Linux y Windows 
Problemas de configuración de SSH/RDP | Acceda a la consola de serie y cambie la configuración. | Linux y Windows 
Sistema de bloqueo de red| Acceda a la consola de serie a través del portal para administrar el sistema. | Linux y Windows 
Interacción con el cargador de arranque | Acceda a GRUB/BCD mediante la consola de serie. Para empezar a trabajar, vaya a [Using Serial Console to access GRUB and Single User Mode](serial-console-grub-single-user-mode.md) (Uso de la consola serie para acceder a GRUB y el modo de usuario único). | Linux y Windows 

## <a name="access-serial-console-for-linux"></a>Acceso a la consola serie para Linux
Para que la consola de serie funcione correctamente, el sistema operativo invitado debe configurarse para leer y escribir mensajes de la consola en el puerto de serie. La mayoría de [distribuciones de Linux aprobadas por Azure](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) tienen la consola de serie configurada de manera predeterminada. Para acceder a la consola, tan solo haga clic en la sección Consola serie de Azure Portal. 

Distribuciones      | Acceso a la consola serie
:-----------|:---------------------
Red Hat Enterprise Linux    | Las imágenes de Red Hat Enterprise Linux disponibles en Azure tienen habilitado el acceso a la consola de forma predeterminada. 
CentOS      | Las imágenes de CentOS disponibles en Azure tienen habilitado el acceso a la consola de forma predeterminada. 
Ubuntu      | Las imágenes de Ubuntu disponibles en Azure tienen habilitado el acceso a la consola de forma predeterminada.
CoreOS      | Las imágenes de CoreOS disponibles en Azure tienen habilitado el acceso a la consola de forma predeterminada.
SUSE        | Las imágenes de SLES más nuevas disponibles en Azure tienen habilitado el acceso a la consola de forma predeterminada. Si está utilizando versiones anteriores (10 o inferiores) de SLES en Azure, siga el [artículo de KB](https://www.novell.com/support/kb/doc.php?id=3456486) para habilitar la consola de serie. 
Oracle Linux        | Las imágenes de Oracle Linux disponibles en Azure tienen habilitado el acceso a la consola de forma predeterminada.
Imágenes personalizadas de Linux     | Para habilitar la consola de serie para la imagen de VM de Linux personalizada, habilite el acceso de la consola en /etc/inittab para ejecutar un terminal en ttyS0. A continuación se muestra un ejemplo de cómo se agrega esto al archivo inittab: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Para obtener más información sobre la creación de imágenes personalizadas correctamente, consulte [Creación y carga de un VHD de Linux en Azure](https://aka.ms/createuploadvhd).

## <a name="errors"></a>Errors
La mayoría de los errores son transitorios por naturaleza, y basta con reintentar establecer a menudo la conexión a la consola serie para solucionarlos. La tabla siguiente muestra una lista de errores y su solución 

Error                            |   Mitigación 
:---------------------------------|:--------------------------------------------|
No se puede recuperar la configuración de diagnóstico de arranque para "<VMNAME>". Para usar la consola serie, asegúrese de que el diagnóstico de arranque está habilitado para esta máquina virtual. | Asegúrese de que la máquina virtual tiene los [diagnósticos de arranque](boot-diagnostics.md) habilitados. 
La máquina virtual se encuentra en un estado desasignado detenido. Inicie la máquina virtual y vuelva a intentar la conexión de la consola serie. | La máquina virtual debe estar en un estado iniciado para tener acceso a la consola de serie.
No tiene los permisos necesarios para usar esta máquina virtual como consola de serie. Asegúrese de tener al menos permisos del rol de colaborador de la VM.| El acceso a la consola serie requiere cierto nivel de permiso. Consulte los [requisitos de acceso](#prerequisites) para más información
No se puede determinar el grupo de recursos para la cuenta de almacenamiento de diagnósticos de arranque "<STORAGEACCOUNTNAME>". Compruebe que el diagnóstico de arranque está habilitado para esta máquina virtual y que tiene acceso a esta cuenta de almacenamiento. | El acceso a la consola serie requiere cierto nivel de permiso. Consulte los [requisitos de acceso](#prerequisites) para más información
Socket web está cerrado o no se pudo abrir. | Puede que necesite incluir `*.console.azure.com` en la lista blanca. Un enfoque más detallado, pero más largo, es incluir en la lista blanca los [rangos de IP del centro de datos de Microsoft Azure](https://www.microsoft.com/en-us/download/details.aspx?id=41653), que cambian con bastante frecuencia.
## <a name="known-issues"></a>Problemas conocidos 
Como nos encontramos aún en las etapas de versión preliminar para el acceso a la consola serie, estamos trabajando en algunos problemas conocidos. Presentamos aquí la lista con algunas posibles soluciones:

Problema                           |   Mitigación 
:---------------------------------|:--------------------------------------------|
No hay opción para la consola serie con la instancia de conjunto de escalado de máquinas virtuales |  Con esta versión preliminar, no se admite el acceso a la consola serie para las instancias del conjunto de escalado de máquinas virtuales.
Al pulsar Entrar tras un banner de conexión no aparece la solicitud de inicio de sesión | [Pulsar Entrar no hace nada](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)
Se encontró una respuesta "Prohibido" al obtener acceso a la cuenta de almacenamiento de diagnóstico de arranque de la VM. | Asegúrese de que el diagnóstico de arranque no tenga un firewall de cuentas. Se necesita una cuenta de almacenamiento de diagnóstico de arranque accesible para que la consola serie funcione.


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes 
**P. ¿Cómo puedo enviar comentarios?**

A. Envíe comentarios como problemas que tenga acudiendo a https://aka.ms/serialconsolefeedback. También tiene la posibilidad (aunque se prefiere menos) de enviar comentarios a azserialhelp@microsoft.com, o en la categoría de máquinas virtuales en http://feedback.azure.com

**P. No puedo acceder a la consola serie, ¿dónde puedo presentar un caso de soporte técnico?**

A. Esta característica de versión preliminar viene recogida en los términos de las versiones preliminares de Azure. La mejor manera de obtener soporte técnico es recurrir a los canales mencionados anteriormente. 

## <a name="next-steps"></a>Pasos siguientes
* Uso de la consola serie para [arrancar en GRUB y entrar en modo de usuario único](serial-console-grub-single-user-mode.md)
* Uso de la consola serie para [llamadas NMI y SysRq](serial-console-nmi-sysrq.md)
* La consola serie también está disponible para VM [Windows](../windows/serial-console.md)
* Obtenga más información sobre [Diagnósticos de arranque](boot-diagnostics.md)