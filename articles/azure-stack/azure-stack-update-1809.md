---
title: Actualización de Azure Stack 1809 | Microsoft Docs
description: Obtenga información sobre las novedades de la actualización 1809 de los sistemas integrados de Azure Stack, incluidos los problemas conocidos y dónde se puede descargar la actualización.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/29/2018
ms.author: sethm
ms.reviewer: justini
ms.openlocfilehash: 38a8cd35d123f58d6379ade4d900e86b36b416ec
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240144"
---
# <a name="azure-stack-1809-update"></a>Actualización de Azure Stack 1809

*Se aplica a: sistemas integrados de Azure Stack*

En este artículo se describe el contenido del paquete de actualización 1809. El paquete de actualización incluye mejoras, correcciones y problemas conocidos de esta versión de Azure Stack. También se incluye un vínculo para que pueda descargar la actualización. Los problemas conocidos se dividen en aquellos que están directamente relacionados con el proceso de actualización y aquellos que están relacionados con la compilación (posteriores a la instalación).

> [!IMPORTANT]  
> Este paquete de actualización es únicamente para los sistemas integrados de Azure Stack. No la aplique al Kit de desarrollo de Azure Stack.

## <a name="build-reference"></a>Referencia de compilación

El número de compilación de la actualización 1809 de Azure Stack es **1.1809.0.90**.  

### <a name="new-features"></a>Nuevas características

Esta actualización incluye las siguientes correcciones para Azure Stack:

- Con esta versión, Sistemas integrados de Azure Stack admite configuraciones de entre 4 y 16 nodos. Puede usar la [herramienta de planeamiento de capacidad de Azure Stack](http://aka.ms/azstackcapacityplanner) para facilitar la configuración y planeación de Azure Stack.

- <!--  2712869   | IS  ASDK -->  **Cliente de Syslog de Azure Stack (disponibilidad general)**  Este cliente permite el reenvío de auditorías, alertas y registros de seguridad relacionados con la infraestructura de Azure Stack a un servidor de Syslog o a un software de información de seguridad y administración de eventos (SIEM) que es externo a Azure Stack. El cliente de Syslog ahora permite especificar el puerto en el que escucha el servidor de Syslog.

   Con esta versión, el cliente de Syslog está disponible con carácter general y se puede usar en entornos de producción.

   Para más información, consulte [Reenvío de syslog de Azure Stack](azure-stack-integrate-security.md).

- Ahora puede [mover el recurso de registro](azure-stack-registration.md#move-a-registration-resource) en Azure entre grupos de recursos sin tener que volver a realizar el registro. Los proveedores de soluciones en la nube (CSP) también pueden mover el recurso de registro entre suscripciones, siempre y cuando se asignen las suscripciones nuevas y antiguas en el mismo identificador de asociado de CSP. Esto no afecta a las asignaciones de inquilinos de cliente existentes. 

### <a name="fixed-issues"></a>Problemas corregidos

<!-- TBD - IS ASDK -->
- En el portal, el gráfico de memoria que notifica la capacidad libre y usada ahora es preciso. Ya puede predecir de manera más confiable cuántas máquinas virtuales puede crear.

<!-- TBD - IS ASDK --> 
- Se ha corregido un problema en el que se creaban máquinas virtuales en el portal de usuarios de Azure Stack y el portal mostraba un número incorrecto de discos de datos que se pueden asociar a una máquina virtual de la serie DS. Las máquinas virtuales de la serie DS pueden albergar tantos discos de datos como la configuración de Azure.

- Los siguientes problemas de disco administrado se han corregido en la 1809 y también en la 1808 [Revisión 1.1808.5.110 de Azure Stack](https://support.microsoft.com/help/4468920/): 

   <!--  2966665 – IS, ASDK --> 
   - Se ha corregido un problema en el que al asociar discos de datos SSD a máquinas virtuales con discos administrados de tamaño premium (DS, DSv2, Fs, Fs_V2), se produce el error *Failed to update disks for the virtual machine 'vmname' Error: Requested operation cannot be performed because storage account type 'Premium_LRS' is not supported for VM size 'Standard_DS/Ds_V2/FS/Fs_v2'* (No se pudieron actualizar los discos para la máquina virtual "nombre_de_máquina_virtual" Error: No se puede realizar la operación solicitada porque el tipo de cuenta de almacenamiento "Premium_LRS" no es compatible con el tamaño de máquina virtual (Standard_DS/Ds_V2/FS/Fs_v2)). 
   
   - Al crear una máquina virtual de disco administrado mediante **createOption**: la opción para **asociar** genera el siguiente error: *Long running operation failed with status 'Failed' (No se pudo realizar la operación de ejecución prolongada: estado "Error"). Información adicional:'An internal execution error occurred'* (Se produjo un error de ejecución interno).
   ErrorCode: InternalExecutionError ErrorMessage: An internal execution error occurred (Se produjo un error de ejecución interno).
   
   Este problema se ha corregido ahora.

- <!-- 2702741 -  IS, ASDK --> Se ha corregido el problema por el cual no se garantizaba que las direcciones IP públicas que se implementaban mediante el método de asignación dinámica se conservasen después de emitirse una detención o desasignación. Ahora ya se conservan.

- <!-- 3078022 - IS, ASDK --> Si una máquina virtual se detenía o desasignaba con una actualización anterior a la 1808, no se podía reasignar después de la actualización 1808.  Este problema se ha corregido en la actualización 1809. Con esta corrección, las instancias que se encontraban en este estado y no se podían iniciar ya se pueden iniciar en la actualización 1809. La corrección también impide que este problema vuelva a ocurrir.

### <a name="changes"></a>Cambios

Ninguno.

### <a name="common-vulnerabilities-and-exposures"></a>Puntos vulnerables y exposiciones comunes

Esta actualización instala las actualizaciones de seguridad siguientes:  

- [ADV180022](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180022)
- [CVE-2018-0965](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-0965)
- [CVE-2018-8271](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8271)
- [CVE-2018-8320](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8320)
- [CVE-2018-8330](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8330)
- [CVE-2018-8332](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8332)
- [CVE-2018-8333](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8333)
- [CVE-2018-8335](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8335)
- [CVE-2018-8392](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8392)
- [CVE-2018-8393](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8393)
- [CVE-2018-8410](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8410)
- [CVE-2018-8411](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8411)
- [CVE-2018-8413](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8413)
- [CVE-2018-8419](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8419)
- [CVE-2018-8420](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8420)
- [CVE-2018-8423](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8423)
- [CVE-2018-8424](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8424)
- [CVE-2018-8433](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8433)
- [CVE-2018-8434](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8434)
- [CVE-2018-8435](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8435)
- [CVE-2018-8438](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8438)
- [CVE-2018-8439](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8439)
- [CVE-2018-8440](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8440)
- [CVE-2018-8442](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8442)
- [CVE-2018-8443](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8443)
- [CVE-2018-8446](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8446)
- [CVE-2018-8449](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8449)
- [CVE-2018-8453](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8453)
- [CVE-2018-8455](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8455)
- [CVE-2018-8462](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8462)
- [CVE-2018-8468](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8468)
- [CVE-2018-8472](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8472)
- [CVE-2018-8475](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8475)
- [CVE-2018-8481](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8481)
- [CVE-2018-8482](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8482)
- [CVE-2018-8484](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8484)
- [CVE-2018-8486](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8486)
- [CVE-2018-8489](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8489)
- [CVE-2018-8490](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8490)
- [CVE-2018-8492](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8492)
- [CVE-2018-8493](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8493)
- [CVE-2018-8494](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8494)
- [CVE-2018-8495](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8495)
- [CVE-2018-8497](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2018-8497)

Para obtener más información acerca de estos puntos vulnerables, haga clic en los vínculos anteriores o consulte los artículos de Microsoft Knowledge Base [4457131](https://support.microsoft.com/help/4457131) y [4462917](https://support.microsoft.com/help/4462917).

### <a name="prerequisites"></a>Requisitos previos

- Instale la revisión más reciente de Azure Stack para la actualización 1808 antes de aplicar la 1809. Para obtener más información, consulte [KB 4468920: Revisión de Azure Stack 1.1808.5.110](https://support.microsoft.com/en-us/help/4468920).

  > [!TIP]  
  > Suscríbase a las siguientes fuentes *RRS* o *Atom* para mantenerse al día con las revisiones de Azure Stack:
  > - RRS: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/rss … 
  > - Atom: https://support.microsoft.com/app/content/api/content/feeds/sap/en-us/32d322a8-acae-202d-e9a9-7371dccf381b/atom …


- Antes de iniciar la instalación de esta actualización, ejecute [Test-AzureStack](azure-stack-diagnostic-test.md) con los parámetros siguientes para validar el estado de Azure Stack y resolver todos los problemas operativos detectados, incluidas todas las advertencias y errores. Repase también las alertas activas y resuelva las que requieran alguna acción.  

  ```PowerShell
  Test-AzureStack -Include AzsControlPlane, AzsDefenderSummary, AzsHostingInfraSummary, AzsHostingInfraUtilization, AzsInfraCapacity, AzsInfraRoleSummary, AzsPortalAPISummary, AzsSFRoleSummary, AzsStampBMCSummary
  ``` 

### <a name="known-issues-with-the-update-process"></a>Problemas conocidos relacionados con el proceso de actualización

- Al ejecutar [Test-AzureStack](azure-stack-diagnostic-test.md) después de la actualización 1809, se muestra un mensaje de advertencia desde el Controlador de administración de placa base (BMC). Puede omitir esta advertencia sin problemas.

- <!-- 2468613 - IS --> Durante la instalación de esta actualización, puede que vea alertas con el título *Error – Template for FaultType UserAccounts.New is missing (Error: Falta la plantilla para FaultType UserAccounts.New).*  Puede omitir estas alertas con seguridad. Estas alertas se cerrarán automáticamente cuando se complete la instalación de esta actualización.

- <!-- 2489559 - IS --> No intente crear máquinas virtuales durante la instalación de esta actualización. Para más información sobre cómo administrar las actualizaciones, consulte [Introducción a la administración de actualizaciones en Azure Stack](azure-stack-updates.md#plan-for-updates).

- <!-- 3139614 | IS --> Si ha aplicado una actualización a Azure Stack de su OEM, la notificación de **actualización disponible** no puede aparecer en el portal de administración de Azure Stack. Para instalar la actualización de Microsoft, descárguela e impórtela manualmente siguiendo las instrucciones que encontrará aquí: [Aplicación de actualizaciones en Azure Stack](azure-stack-apply-updates.md).

### <a name="post-update-steps"></a>Pasos posteriores a la actualización

*No hay ningún paso posterior para la actualización 1809.*

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - [Link to KB]()  
 -->

## <a name="known-issues-post-installation"></a>Problemas conocidos (posteriores a la instalación)

Los siguientes son problemas conocidos posteriores a la instalación de esta compilación.

### <a name="portal"></a>Portal

- La documentación técnica de Azure Stack se centra en la versión más reciente. Debido a los cambios del portal entre las versiones, es posible que lo que vea al usar los portales de Azure Stack varíe con respecto a lo que ve en la documentación.

<!-- 2930718 - IS ASDK --> 
- En el portal de administrador, al acceder a los detalles de cualquier suscripción de usuario después de cerrar la hoja y hacer clic en **Recientes**, no aparece el nombre de la suscripción de usuario.

<!-- 3060156 - IS ASDK --> 
- En los portales de administrador y usuario, hacer clic en la configuración del portal y seleccionar **Eliminar todas las opciones de configuración y los paneles privados** no funciona según lo previsto. Se muestra una notificación de error. 

<!-- 2930799 - IS ASDK --> 
- En los portales de administrador y de usuario, en **Todos los servicios**, el recurso **Plan de DDoS Protection** se muestra de forma incorrecta. No está disponible en Azure Stack. Si se intenta crearlo, se muestra un error que indica que el portal no pudo crear el elemento de Marketplace. 

<!-- 2930820 - IS ASDK --> 
- En los portales de administrador y de usuario, si se busca "Docker", el elemento se devuelve de forma incorrecta. No está disponible en Azure Stack. Si se intenta crearlo, se muestra una hoja con una indicación de error. 

<!-- 2967387 – IS, ASDK --> 
- La cuenta que se usa para iniciar sesión en el portal de administrador o usuario de Azure Stack se muestra como **Usuario no identificado**. Este mensaje se muestra cuando en la cuenta no se ha especificado un *nombre* o un *apellido*. Para solucionar este problema, modifique la cuenta de usuario para proporcionar el nombre o el apellido. Después, tendrá que cerrar sesión y volver a iniciar sesión en el portal.  

<!--  2873083 - IS ASDK --> 
-  Cuando se usa el portal para crear un conjunto de escalado de máquinas virtuales (VMSS), la lista desplegable *Tamaño de instancia* no se carga correctamente cuando se usa Internet Explorer. Para solucionar este problema, use otro explorador cuando utilice el portal para crear un VMSS.  

<!-- 2931230 – IS  ASDK --> 
- No se pueden eliminar los planes que se agregan a una suscripción de usuario como planes complementarios, aunque se quite el plan de la suscripción de usuario. El plan permanecerá hasta que también se eliminen las suscripciones a las que haga referencia el plan complementario. 

<!--2760466 – IS  ASDK --> 
- Cuando se instala un nuevo entorno de Azure Stack que ejecuta esta versión, la alerta que indica *Activación necesaria* podría no mostrarse. La [activación](azure-stack-registration.md) se requiere para poder usar la redifusión de marketplace.  

<!-- TBD - IS ASDK --> 
- Los dos tipos de suscripción administrativa que se incluyeron con la versión 1804 no deberían usarse. Los tipos de suscripción son **suscripción de medición** y **suscripción de consumo**. Estos tipos de suscripción están visibles en los nuevos entornos de Azure Stack a partir de la versión 1804 pero aún no están listos para su uso. Tendrá que seguir usando el tipo de suscripción del **proveedor predeterminado**.

<!-- TBD - IS ASDK --> 
- La eliminación de las suscripciones del usuario da como resultado recursos huérfanos. Como alternativa, elimine primero los recursos del usuario o todo el grupo de recursos y, a continuación, elimine las suscripciones del usuario.

<!-- TBD - IS ASDK --> 
- No puede ver los permisos de la suscripción mediante los portales de Azure Stack. Como alternativa, use PowerShell para comprobar los permisos.


### <a name="health-and-monitoring"></a>Estado y supervisión

<!-- TBD - IS -->
- Puede que vea que las siguientes alertas aparecen repetidamente y después, desaparecen en el sistema de Azure Stack:
   - *Instancia del rol de infraestructura no disponible*
   - *Nodo de la unidad de escalado desconectado*
   
  Vuelva a ejecutar el cmdlet [Test-AzureStack](azure-stack-diagnostic-test.md) para comprobar el estado de las instancias de rol de infraestructura y los nodos de la unidad de escalado. Si [Test-AzureStack](azure-stack-diagnostic-test.md) no detecta ningún problema, puede ignorar estas alertas. Si se detecta un problema, puede intentar iniciar la instancia de rol de infraestructura o un nodo mediante el portal de administración o PowerShell.

<!-- 1264761 - IS ASDK -->  
- Es posible que vea alertas del componente **Controlador de mantenimiento** con los siguientes detalles:  

   Alerta 1:
   - NOMBRE: rol de infraestructura incorrecto
   - GRAVEDAD: advertencia
   - COMPONENTE: controlador de mantenimiento
   - DESCRIPCIÓN: el controlador de mantenimiento Heartbeat Scanner no está disponible. Esto puede afectar a los informes y a las métricas de mantenimiento.  

  Alerta 2:
   - NOMBRE: rol de infraestructura incorrecto
   - GRAVEDAD: advertencia
   - COMPONENTE: controlador de mantenimiento
   - DESCRIPCIÓN: el controlador de mantenimiento Fault Scanner no está disponible. Esto puede afectar a los informes y a las métricas de mantenimiento.

  Se pueden omitir ambas alertas con seguridad y se cerrarán automáticamente con el tiempo.  


<!-- 2812138 | IS --> 
- Es posible que vea una alerta para el componente **Storage** con los siguientes detalles:

   - NOMBRE: error de comunicación interna del servicio Storage  
   - GRAVEDAD: crítica  
   - COMPONENTE: Storage  
   - DESCRIPCIÓN: se produjo un error de comunicación interna del servicio Storage al enviar solicitudes a los nodos siguientes.  

    La alerta puede omitirse sin riesgos, pero deberá cerrarla manualmente.

<!-- 2368581 - IS, ASDK --> 
- Operador de Azure Stack: si recibe una alerta de memoria insuficiente y no se pueden implementar las máquinas virtuales del inquilino debido a un **error de creación de máquina virtual de Fabric**, es posible que la marca de Azure Stack supere la memoria disponible. Use la [herramienta de planeamiento de capacidad de Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para comprender mejor la capacidad disponible para las cargas de trabajo.

### <a name="compute"></a>Compute

<!-- 3235634 – IS, ASDK -->
- Para implementar máquinas virtuales con tamaños que contiene un sufijo **v2**; por ejemplo, **Standard_A2_v2**, especifique el sufijo como **Standard_A2_v2** (v minúscula). No use **Standard_A2_V2** (v mayúscula). Esto funciona en Azure global y es una incoherencia en Azure Stack.

<!-- 3099544 – IS, ASDK --> 
- Cuando cree una máquina virtual mediante el portal de Azure Stack y seleccione el tamaño de la máquina, se mostrará la columna USD/mes con el mensaje **No disponible**. Esta columna no debería aparecer, ya que en Azure Stack no se permite mostrar la columna de precios de la máquina virtual.

<!-- 2869209 – IS, ASDK --> 
- Cuando se usa el [cmdlet **Add-AzsPlatformImage**](https://docs.microsoft.com/powershell/module/azs.compute.admin/add-azsplatformimage?view=azurestackps-1.4.0), se debe usar el parámetro **-OsUri** como el URI de la cuenta de almacenamiento donde se ha cargado el disco. Si usa la ruta de acceso local del disco, se produce un error en el cmdlet con el error siguiente: *Error en la operación de larga ejecución con el estado "Error"*. 

<!--  2795678 – IS, ASDK --> 
- Cuando usa el portal para crear máquinas virtuales (VM) en un tamaño de máquina virtual prémium (DS, Ds_v2, FS, FSv2), la máquina virtual se crea en una cuenta de almacenamiento estándar. La creación en una cuenta de almacenamiento estándar no afecta al funcionamiento, E/S por segundo ni la facturación. 

   Puede ignorar la advertencia que dice: *Ha elegido usar un disco estándar en un tamaño que admite discos premium. Esto no se recomienda ya que podría afectar al rendimiento del sistema operativo. Considere la opción de usar Premium Storage (SSD).*

<!-- 2967447 - IS, ASDK --> 
- La experiencia de creación de conjuntos de escalado de máquinas virtuales (VMSS) proporciona la versión 7.2 basada en CentOS como una opción para la implementación. Como esa imagen no está disponible en Azure Stack, seleccione otro sistema operativo para la implementación o use una plantilla de Azure Resource Manager en la que se especifique otra imagen de CentOS que el operador haya descargado de Marketplace antes de la implementación.  

<!-- 2724873 - IS --> 
- Al usar los cmdlets de PowerShell **Start-AzsScaleUnitNode** o **Stop-AzsScaleunitNode** para administrar las unidades de escalado, el primer intento para iniciar o detener la unidad de escalado podría producir un error. Si se produce un error en el cmdlet en la primera ejecución, ejecute el cmdlet una segunda vez. La segunda ejecución debe ser correcta para completar la operación. 

<!-- TBD - IS ASDK --> 
- Si aprovisionar una extensión en una implementación de máquina virtual tarda demasiado tiempo, los usuarios deberían dejar que se agote el tiempo de espera de aprovisionamiento en lugar de intentar detener el proceso para desasignar o eliminar la máquina virtual.  

<!-- 1662991 IS ASDK --> 
- No se admite el diagnóstico de máquinas virtuales Linux en Azure Stack. Si implementa una máquina virtual Linux con diagnósticos de máquina virtual habilitado, se producirá un error en la implementación. Tampoco se podrá realizar la implementación si habilita las métricas básicas de máquina virtual Linux a través de la configuración de diagnóstico.  

<!-- 2724961- IS ASDK --> 
- Al registrar el proveedor de recursos **Microsoft.Insight** en la configuración de la suscripción y crear una máquina virtual Windows con el diagnóstico del sistema operativo invitado habilitado, el gráfico de porcentaje de CPU de la página de información general de la máquina virtual no muestra los datos de métricas.

   Para buscar datos de métricas, como el gráfico de porcentaje de CPU para la máquina virtual, vaya a la ventana Métrica y muestre todas las métricas de invitado de las máquinas virtuales Windows admitidas.



### <a name="networking"></a>Redes  

<!-- 1766332 - IS ASDK --> 
- En **Redes**, si hace clic en **Create VPN Gateway** (Crear instancia de VPN Gateway) para configurar una conexión VPN, aparece **Basada en directivas** como un tipo de VPN. No seleccione esta opción. En Azure Stack solo se admite la opción **Route Based** (Basada en rutas).

<!-- 1902460 - IS ASDK --> 
- Azure Stack admite una única *puerta de enlace de red local* por dirección IP. Y esto se aplica a las suscripciones de todos los inquilinos. Tras la creación de la primera conexión a la puerta de enlace de red local, los sucesivos intentos para crear un recurso de puerta de enlace de red local con la misma dirección IP se bloquean.

<!-- 16309153 - IS ASDK --> 
- En una red virtual que se creó con una configuración de servidor DNS de *Automática*, se produce un error al cambiar a un servidor DNS personalizado. La configuración actualizada no se inserta en las máquinas virtuales de esa red virtual.

<!-- 2529607 - IS ASDK --> 
- Durante la *rotación secreta* de Azure Stack, hay un período en el que las direcciones IP públicas a nivel de instancia no son accesibles entre dos y cinco minutos.

<!-- 2664148 - IS ASDK --> 
-   En los escenarios en los que el inquilino tenga acceso a sus máquinas virtuales mediante un túnel VPN S2S, podría encontrarse con que los intentos de conexión producen un error si la subred local se ha agregado a la puerta de enlace de red local una vez que la puerta de enlace ya está creada. 


<!-- ### SQL and MySQL-->


### <a name="app-service"></a>App Service

<!-- 2352906 - IS ASDK --> 
- Los usuarios deben registrar el proveedor de recursos de almacenamiento antes de crear su primera función de Azure en la suscripción.


### <a name="usage"></a>Uso  

<!-- TBD - IS ASDK --> 
- Los datos del medidor de uso de la dirección IP pública muestran el mismo valor *EventDateTime* para cada registro en lugar de la marca *TimeDate* que muestra cuándo se creó el registro. Actualmente, no puede usar estos datos para realizar un recuento adecuado del uso de la dirección IP pública.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Descarga de la actualización

Puede descargar el paquete de actualización de Azure Stack 1809 desde [aquí](https://aka.ms/azurestackupdatedownload).
  

## <a name="next-steps"></a>Pasos siguientes

- Para revisar la directiva de servicio de los sistemas integrados de Azure Stack y lo que debe hacer para mantener el sistema en un estado admitido, consulte [Directiva de servicio de Azure Stack](azure-stack-servicing-policy.md).  
- Para usar el punto de conexión con privilegios (PEP) para supervisar y reanudar las actualizaciones, consulte [Supervisión de las actualizaciones en Azure Stack mediante el uso del punto de conexión con privilegios](azure-stack-monitor-update.md).  
- Para información general sobre la administración de actualizaciones en Azure Stack, consulte [Introducción a la administración de actualizaciones en Azure Stack](azure-stack-updates.md).  
- Para más información sobre cómo aplicar actualizaciones con Azure Stack, consulte [Aplicación de actualizaciones en Azure Stack](azure-stack-apply-updates.md).  
