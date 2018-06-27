---
title: Notas de la versión del Kit de desarrollo de Microsoft Azure Stack | Microsoft Docs
description: Mejoras, correcciones y problemas conocidos del Kit de desarrollo de Azure Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2018
ms.author: brenduns
ms.reviewer: misainat
ms.openlocfilehash: bbd9bb0d56dd61fd0a32531ac425a1dbc1aa8923
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36295418"
---
# <a name="azure-stack-development-kit-release-notes"></a>Notas de la versión del Kit de desarrollo de Azure Stack  
Estas notas de la versión proporcionan información sobre las mejoras, correcciones y problemas conocidos en el Kit de desarrollo de Azure Stack. Si no está seguro de qué versión se está ejecutando, puede usar el [portal de administración](.\.\azure-stack-updates.md#determine-the-current-version).

> Para estar al día de las novedades del ASDK, suscríbase a esta [![fuente](./media/asdk-release-notes/feed-icon-14x14.png)](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#) [RSS](https://docs.microsoft.com/api/search/rss?search=Azure+Stack+Development+Kit+release+notes&locale=en-us#).

## <a name="build-11805147"></a>Compilación 1.1805.1.47

> [!TIP]  
> Según los comentarios recibidos, hay una actualización al esquema de versiones en uso para Microsoft Azure Stack. A partir de esta actualización, la 1805, el nuevo esquema representa mejor la versión actual de la nube.  
> 
> El esquema de la versión es ahora *Version.YearYearMonthMonth.MinorVersion.BuildNumber* donde en el segundo y tercer conjunto se indica la versión y la versión de lanzamiento. Por ejemplo, 1805.1 representa la *versión que se distribuye a los fabricantes* (RTM) de la actualización 1805.  


### <a name="new-features"></a>Nuevas características 
Esta compilación incluye las siguientes correcciones y mejoras para Azure Stack.  

- <!-- 2297790 - IS, ASDK --> **Azure Stack incluye ahora un *cliente de* Syslog** como una *característica de la versión preliminar*. Este cliente permite el reenvío de los registros de auditoría y seguridad relacionados con la infraestructura de Azure Stack a un servidor de Syslog o a un software de administración de eventos e información de seguridad (SIEM) que es externo a Azure Stack. Actualmente, el cliente de Syslog solo admite conexiones no autenticadas de UDP en el puerto predeterminado 514. La carga de cada mensaje de Syslog tiene el formato Common Event Format (CEF). 

  Para configurar el cliente de Syslog, use el cmdlet **Set-SyslogServer** que se expone en el punto de conexión con privilegios. 

  Con esta versión preliminar, puede que vea las siguientes tres alertas. Cuando las presenta Azure Stack, estas alertas incluyen *descripciones* e instrucciones de *corrección*. 
  - TITLE: Code Integrity Off (TÍTULO: se ha desactivado la integridad del código)  
  - TITLE: Code Integrity in Audit Mode (TÍTULO: integridad del código en modo auditoría) 
  - TITLE: User Account Created (TÍTULO: se ha creado la cuenta de usuario)

  Aunque esta característica está en su versión preliminar, no se debería usar en entornos de producción.   


### <a name="fixed-issues"></a>Problemas corregidos
- Hemos corregido el problema que bloqueaba [la apertura de una nueva solicitud de soporte técnico desde la lista desplegable](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) en el portal de administración. Esta opción funciona ahora según lo previsto. 

- **Varias revisiones** de rendimiento, estabilidad, seguridad y el sistema operativo que se usa en Azure Stack.


<!-- ### Changes  --> 


<!--   ### Additional releases timed with this update  -->


### <a name="known-issues"></a>Problemas conocidos
 
#### <a name="portal"></a>Portal
- <!-- 2551834 - IS, ASDK --> Cuando se selecciona **Información general** para una cuenta de almacenamiento en los portales de administrador o de usuario, la información del panel *Información esencial* no aparece.  Este panel muestra información sobre la cuenta como, por ejemplo, su *grupo de recursos*, *ubicación* y el *identificador de la suscripción*.  Se puede acceder a otras opciones de Información general como *Servicios* y *Supervisión*, así como a opciones para *Abrir en el Explorador* o para *Eliminar cuenta de almacenamiento*.  

  Para ver la información no disponible, use el cmdlet de PowerShell [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0). 

- <!-- 2551834 - IS, ASDK --> Cuando se selecciona **Etiquetas** para una cuenta de almacenamiento en los portales de administrador o de usuario, la información no se puede cargar y no aparece.  

  Para ver la información no disponible, use el cmdlet de PowerShell [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0).

- <!-- TBD - IS ASDK --> No use los nuevos tipos de suscripción administrativos de *suscripción de medición* y *suscripción de consumo*. Estos nuevos tipos de suscripción se introdujeron con la versión 1804, pero aún no están listos para su uso. Tendrá que seguir usando el tipo de suscripción del *proveedor predeterminado*.  

- <!-- 2403291 - IS ASDK --> No puede usar la barra de desplazamiento horizontal a lo largo de la parte inferior de los portales de administrador y de usuario. Si no puede acceder a la barra de desplazamiento horizontal, use las rutas de navegación para ir a una hoja anterior del portal seleccionando la hoja que desea ver en la lista de rutas que se encuentra en la parte superior izquierda del portal.
  ![Ruta de navegación](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> La eliminación de las suscripciones del usuario da como resultado recursos huérfanos. Como alternativa, elimine primero los recursos del usuario o todo el grupo de recursos y, a continuación, elimine las suscripciones del usuario.

- <!-- TBD -  IS ASDK --> No puede ver los permisos de la suscripción mediante los portales de Azure Stack. Como alternativa, use PowerShell para comprobar los permisos.


#### <a name="health-and-monitoring"></a>Estado y supervisión
- <!-- 1264761 - IS ASDK --> Es posible que vea alertas del componente *Controlador de mantenimiento* con los siguientes detalles:  

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

- <!-- 2392907 – ASDK --> Es posible que vea una alerta *crítica* de **Poca capacidad de memoria**. Esta alerta tiene la siguiente descripción: *la región ha consumido más del 95,00 % de la memoria disponible. La creación de máquinas virtuales con grandes cantidades de memoria puede producir un error.*

  Esta alerta se puede generar cuando Azure Stack tiene el cuenta incorrectamente el uso de la memoria en el kit de desarrollo de Azure Stack.  

  Esta alerta se puede ignorar y el problema no tiene ningún efecto sobre la ubicación de las máquinas virtuales. 

- <!-- 2368581 - IS. ASDK --> Operador de Azure Stack: si recibe una alerta de memoria insuficiente y no se pueden implementar las máquinas virtuales del inquilino debido a un *error de creación de máquina virtual de Fabric*, es posible que la marca de Azure Stack supere la memoria disponible. Use la [herramienta de planeamiento de capacidad de Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para comprender mejor la capacidad disponible para las cargas de trabajo. 


#### <a name="compute"></a>Compute
- <!-- TBD - IS, ASDK --> Al seleccionar un tamaño de máquina virtual para una implementación, algunos tamaños de la serie F no aparecen en el selector de tamaño al crear una máquina virtual. Los siguientes tamaños de máquinas virtuales no aparecen en el selector: *F8s_v2*, *F16s_v2*, *F32s_v2* y *F64s_v2*.  
  Como alternativa, utilice uno de los métodos siguientes para implementar una máquina virtual. En cada método, debe especificar el tamaño de máquina virtual que desea utilizar.

  - **Plantilla de Azure Resource Manager:** cuando utilice una plantilla, establezca *vmSize* en la plantilla de modo que sea igual al tamaño deseado de la máquina virtual. Por ejemplo, se utiliza la siguiente entrada para implementar una máquina virtual que utiliza el tamaño *F32s_v2*:  

    ```
        "properties": {
        "hardwareProfile": {
                "vmSize": "Standard_F32s_v2"
        },
    ```  
  - **CLI de Azure:** puede utilizar el comando [az vm create](https://docs.microsoft.com/cli/azure/vm?view=azure-cli-latest#az-vm-create) y especificar el tamaño de máquina virtual como parámetro, de forma similar a `--size "Standard_F32s_v2"`.

  - **PowerShell:** con PowerShell, puede usar [New-AzureRMVMConfig](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermvmconfig?view=azurermps-6.0.0) con el parámetro que especifica el tamaño de la máquina virtual, de forma similar a `-VMSize "Standard_F32s_v2"`.


- <!-- TBD -  IS ASDK --> Los valores de escalado de los conjuntos de escalado de máquinas virtuales no están disponibles en el portal. Como alternativa, puede usar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Debido a diferencias en la versión de PowerShell, debe usar el parámetro `-Name` en lugar de `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Al crear máquinas virtuales en el portal de usuario de Azure Stack, este muestra un número incorrecto de discos de datos que se pueden asociar a una máquina virtual de la serie D. Todas las máquinas virtuales de la serie D pueden albergar tantos discos de datos como la configuración de Azure.

- <!-- TBD -  IS ASDK --> Cuando no es posible crear una imagen de máquina virtual, los elementos con error que no se pueden eliminar se podrían agregar a la hoja de proceso de imágenes de máquina virtual.

  Como alternativa, cree una nueva imagen de máquina virtual con un disco duro virtual ficticio que se pueda crear mediante Hyper-V (nuevo VHD, ruta de acceso C:\dummy.vhd, fijo, bytes de tamaño 1 GB). Este proceso debería corregir el problema que impide que se elimine el elemento con error. A continuación, 15 minutos después de crear la imagen ficticia, puede eliminarlo correctamente.

  Luego puede volver a intentar cargar la imagen de máquina virtual que anteriormente produjo un error.

- <!-- TBD -  IS ASDK --> Si el aprovisionamiento de una extensión en la implementación de una máquina virtual tarda demasiado, los usuarios deben dejar que se agote el tiempo de espera del aprovisionamiento, en lugar de intentar detener el proceso para desasignar o eliminar la máquina virtual.  

- <!-- 1662991 - IS ASDK --> No se admite el diagnóstico de máquinas virtuales Linux en Azure Stack. Si implementa una máquina virtual Linux con diagnósticos de máquina virtual habilitado, se producirá un error en la implementación. Tampoco se podrá realizar la implementación si habilita las métricas básicas de máquina virtual Linux a través de la configuración de diagnóstico. 

#### <a name="networking"></a>Redes
- <!-- TBD - IS ASDK --> No se pueden crear rutas definidas por el usuario en el portal de administrador ni en el de usuario. Como alternativa, use [Azure PowerShell](https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell).

- <!-- 1766332 - IS, ASDK --> En **Redes**, si hace clic en **Create VPN Gateway** (Crear instancia de VPN Gateway) para configurar una conexión VPN, aparece **Policy Based** (Basada en directivas) como un tipo de VPN. No seleccione esta opción. En Azure Stack solo se admite la opción **Route Based** (Basada en rutas).

- <!-- 2388980 -  IS ASDK --> Después de crear una máquina virtual y asociarla a una dirección IP pública, no se puede desasociar de dicha dirección IP. Puede que parezca que se ha desasociado, pero la dirección IP pública anteriormente asignada permanece asociada a la máquina virtual original.

  Actualmente, solo debe usar nuevas direcciones IP públicas para las nuevas máquinas virtuales creadas.

  Este comportamiento se producirá incluso si vuelve a asignar la dirección IP a una nueva máquina virtual (lo que normalmente se conoce como *intercambio de VIP*). Todos los intentos futuros de conectarse a través de esta dirección IP tendrán como resultado una conexión a la máquina virtual original, y no a la nueva.


- <!-- 2292271 - IS ASDK --> Si genera un límite de cuota para un recurso de red que forma parte de una oferta o plan que está asociado a una suscripción de inquilino, el nuevo límite no se aplicará a esa suscripción. Sin embargo, el nuevo límite se aplicará a las nuevas suscripciones que se creen después de que se aumente la cuota. 

  Para solucionar este problema, use un plan complementario para aumentar una cuota de red cuando el plan ya esté asociado a una suscripción. Para obtener más información, consulte cómo [disponer de un plan complementario](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> No se puede eliminar una suscripción que disponga de recursos de zona DNS o recursos de tabla de rutas asociados a ella. Para eliminar correctamente la suscripción, primero debe eliminar los recursos de la zona DNS y de la tabla de rutas de la suscripción de inquilino. 


- <!-- 1902460 -  IS ASDK -->Azure Stack admite una única *puerta de enlace de red local* por dirección IP. Y esto se aplica a las suscripciones de todos los inquilinos. Tras la creación de la primera conexión a la puerta de enlace de red local, los sucesivos intentos para crear un recurso de puerta de enlace de red local con la misma dirección IP se bloquean.

- <!-- 16309153 -  IS ASDK --> En una red virtual que se creó con el valor *Automático* en Servidor DNS, se produce un error al cambiar a un servidor DNS personalizado. La configuración actualizada no se inserta en las máquinas virtuales de esa red virtual.
 
- <!-- TBD -  IS ASDK --> Azure Stack no admite la adición de interfaces de red adicionales a una instancia de máquina virtual una vez implementada la máquina virtual. Si la máquina virtual requiere más de una interfaz de red, se deben definir en el momento de la implementación.


#### <a name="sql-and-mysql"></a>SQL y MySQL 
- <!-- TBD - ASDK --> La base de datos que hospeda los servidores debe estar dedicada al uso por parte del proveedor de recursos y las cargas de trabajo del usuario. No puede usar una instancia de SQL que esté siendo utilizada por otro consumidor, incluido App Services.

- <!-- IS, ASDK --> Los caracteres especiales, entre los que se incluyen espacios y puntos, no se admiten en el nombre de **familia** al crear una SKU para los proveedores de recursos de SQL y MySQL. 

#### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Los usuarios deben registrar el proveedor de recursos de almacenamiento antes de crear su primera función de Azure en la suscripción.

- <!-- TBD - IS ASDK --> Para escalar horizontalmente la infraestructura (roles de trabajo, administración y front-end), debe usar PowerShell, tal como se describe en las notas de la versión de Compute.  

- <!-- TBD - IS ASDK --> App Service solo puede implementarse en la *suscripción del proveedor predeterminado* en este momento. En una próxima actualización, App Service se podrá implementar en la nueva *suscripción de medición* que se introdujo en Azure Stack 1804. Cuando se admita el uso de la suscripción de medición, todas las implementaciones existentes se migrarán a este nuevo tipo de suscripción.

#### <a name="usage"></a>Uso  
- <!-- TBD -  IS ASDK --> El uso de los datos del medidor de uso de la dirección IP pública muestran el mismo valor *EventDateTime* para cada registro en lugar de la marca *TimeDate* que muestra cuándo se creó el registro. Actualmente, no puede usar estos datos para realizar un recuento adecuado del uso de la dirección IP pública.

<!-- #### Identity -->



## <a name="build-201805131"></a>Compilación 20180513.1

### <a name="new-features"></a>Nuevas características 
Esta compilación incluye las siguientes correcciones y mejoras para Azure Stack.  

- <!-- 1759172 - IS, ASDK --> **Nuevas suscripciones administrativas**. Con 1804 hay dos nuevos tipos de suscripción disponibles en el portal. Estos nuevos tipos de suscripción se suman a la suscripción del proveedor predeterminado y se ven con las nuevas instalaciones de Azure Stack a partir de la versión 1804. *No use estos nuevos tipos de suscripción con esta versión de Azure Stack*. Anunciaremos la disponibilidad para usar estos tipos de suscripción con una actualización futura. 

  Estos nuevos tipos de suscripción se pueden ver, pero forman parte de un cambio de mayor envergadura para proteger la suscripción del proveedor predeterminado y para que resulte más fácil implementar recursos compartidos, como los servidores de hospedaje de SQL. 

  Los tres tipos de suscripción que están disponibles ahora son:  
  - Suscripción de proveedor predeterminado: continúe usando este tipo de suscripción. 
  - Suscripción de medición: *no use este tipo de suscripción*.
  - Suscripción de consumo: *no use este tipo de suscripción*.

### <a name="fixed-issues"></a>Problemas corregidos
- <!-- IS, ASDK --> En el portal de administración, ya no es preciso actualizar el icono de actualización para que muestre información. 

- <!-- 2050709 - IS, ASDK --> Ahora puede usar el portal de administración para editar las métricas de almacenamiento de Blob service, Table service y Queue service.

- <!-- IS, ASDK --> En **Redes**, al hacer clic en **Conexión** para configurar una conexión VPN, **De sitio a sitio (IPsec)** es ahora la única opción disponible. 

- **Varias revisiones** de rendimiento, estabilidad, seguridad y el sistema operativo que se usa en Azure Stack.

<!-- ### Changes  --> 
### <a name="additional-releases-timed-with-this-update"></a>Versiones adicionales en el momento de esta actualización  
Las siguientes están disponibles ahora, pero no requieren la actualización de Azure Stack 1804.
- **Actualización del pack de supervisión de System Center Operations Manager para Microsoft Azure Stack**. Una nueva versión (1.0.3.0) del paquete de supervisión de System Center Operations Manager para Microsoft Azure Stack está disponible para [descargar](https://www.microsoft.com/download/details.aspx?id=55184). Con esta versión, puede usar a las entidades de servicio al agregar una implementación de Azure Stack conectada. Esta versión también incluye una experiencia de Update Management que le permite realizar una acción de corrección directamente desde Operations Manager. También hay nuevos paneles que muestran los proveedores de recursos, las unidades de escalado y los nodos de las unidades de escalado.

- **Nueva versión 1.3.0 de PowerShell de administración para Azure Stack**.  Azure Stack PowerShell 1.3.0 ahora está disponible para instalar. Esta versión proporciona comandos para que todos los proveedores de recursos de administración administren Azure Stack.  Con esta versión, parte del contenido del [repositorio](https://github.com/Azure/AzureStack-Tools) de GitHub para herramientas de Azure Stack entrará en desuso. 

   Para obtener información de instalación, siga las [instrucciones](.\.\azure-stack-powershell-install.md) o el contenido de [ayuda](https://docs.microsoft.com/powershell/azure/azure-stack/overview?view=azurestackps-1.3.0) para el módulo 1.3.0 de Azure Stack. 

- **Versión inicial de la referencia de la API de REST para Azure Stack**. Se ha publicado la [referencia de la API para todos los proveedores de recursos de administración de Azure Stack](https://docs.microsoft.com/rest/api/azure-stack/). 

### <a name="known-issues"></a>Problemas conocidos
 
#### <a name="portal"></a>Portal
- <!-- TBD - IS ASDK --> No está disponible la posibilidad de [abrir una nueva solicitud de soporte técnico desde la lista desplegable](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) del portal de administración. En su lugar, use el siguiente vínculo:     
    - Para el Kit de desarrollo de Azure Stack, use https://aka.ms/azurestackforum.    

- <!-- 2403291 - IS ASDK --> No puede usar la barra de desplazamiento horizontal a lo largo de la parte inferior de los portales de administrador y de usuario. Si no puede acceder a la barra de desplazamiento horizontal, use las rutas de navegación para ir a una hoja anterior del portal seleccionando la hoja que desea ver en la lista de rutas que se encuentra en la parte superior izquierda del portal.
  ![Ruta de navegación](media/asdk-release-notes/breadcrumb.png)

- <!-- TBD -  IS ASDK --> La eliminación de las suscripciones del usuario da como resultado recursos huérfanos. Como alternativa, elimine primero los recursos del usuario o todo el grupo de recursos y, a continuación, elimine las suscripciones del usuario.

- <!-- TBD -  IS ASDK --> No puede ver los permisos de la suscripción mediante los portales de Azure Stack. Como alternativa, use PowerShell para comprobar los permisos.

-   <!-- TBD -  IS ASDK --> En el portal de administración, puede que vea una alerta crítica del componente Microsoft.Update.Admin. El nombre de la alerta, la descripción y la solución se muestran como:  
    - *ERROR - Template for FaultType ResourceProviderTimeout is missing.* (ERROR: falta la plantilla de FaultType ResourceProviderTimeout)

    Esta alerta puede omitirse sin problemas. 

#### <a name="health-and-monitoring"></a>Estado y supervisión
- <!-- 1264761 - IS ASDK --> Es posible que vea alertas del componente *Controlador de mantenimiento* con los siguientes detalles:  

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

  Ambas alertas pueden omitirse sin problemas. Se cerrarán automáticamente pasado un tiempo.  

#### <a name="marketplace"></a>Marketplace
- Los usuarios pueden examinar toda la plataforma Marketplace sin ninguna suscripción y ver elementos administrativos, como planes y ofertas. Estos elementos no funcionan para los usuarios.
 
#### <a name="compute"></a>Compute
- <!-- TBD -  IS ASDK --> Los valores de escalado de los conjuntos de escalado de máquinas virtuales no están disponibles en el portal. Como alternativa, puede usar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Debido a diferencias en la versión de PowerShell, debe usar el parámetro `-Name` en lugar de `-VMScaleSetName`.

- <!-- TBD -  IS ASDK --> Al crear máquinas virtuales en el portal de usuario de Azure Stack, el portal muestra un número incorrecto de discos de datos que se pueden asociar a una máquina virtual de la serie DS. Las máquinas virtuales de la serie DS pueden albergar tantos discos de datos como la configuración de Azure.

- <!-- TBD -  IS ASDK --> Cuando no es posible crear una imagen de máquina virtual, los elementos con error que no se pueden eliminar se podrían agregar a la hoja de proceso de imágenes de máquina virtual.

  Como alternativa, cree una nueva imagen de máquina virtual con un disco duro virtual ficticio que se pueda crear mediante Hyper-V (nuevo VHD, ruta de acceso C:\dummy.vhd, fijo, bytes de tamaño 1 GB). Este proceso debería corregir el problema que impide que se elimine el elemento con error. A continuación, 15 minutos después de crear la imagen ficticia, puede eliminarlo correctamente.

  Luego puede volver a intentar cargar la imagen de máquina virtual que anteriormente produjo un error.

- <!-- TBD -  IS ASDK --> Si aprovisionar una extensión en una implementación de máquina virtual tarda demasiado tiempo, los usuarios deberían dejar que se agote el tiempo de espera de aprovisionamiento en lugar de intentar detener el proceso para desasignar o eliminar la máquina virtual.  

- <!-- 1662991 - IS ASDK --> No se admite el diagnóstico de máquinas virtuales Linux en Azure Stack. Si implementa una máquina virtual Linux con diagnósticos de máquina virtual habilitado, se producirá un error en la implementación. Tampoco se podrá realizar la implementación si habilita las métricas básicas de máquina virtual Linux a través de la configuración de diagnóstico. 

#### <a name="networking"></a>Redes
- <!-- 1766332 - IS, ASDK --> En **Redes**, si hace clic en **Create VPN Gateway** (Crear instancia de VPN Gateway) para configurar una conexión VPN, aparece **Policy Based** (Basada en directivas) como un tipo de VPN. No seleccione esta opción. En Azure Stack solo se admite la opción **Route Based** (Basada en rutas).

- <!-- 2388980 -  IS ASDK --> Después de crear una máquina virtual y asociarla a una dirección IP pública, no puede desasociar esa máquina virtual de esa dirección IP. Puede que parezca que se ha desasociado, pero la dirección IP pública anteriormente asignada permanece asociada a la máquina virtual original.

  Actualmente, solo debe usar nuevas direcciones IP públicas para las nuevas máquinas virtuales creadas.

  Este comportamiento se producirá incluso si vuelve a asignar la dirección IP a una nueva máquina virtual (lo que normalmente se conoce como *intercambio de VIP*). Todos los intentos futuros de conectarse a través de esta dirección IP tendrán como resultado una conexión a la máquina virtual original, y no a la nueva.

- <!-- 2292271 - IS ASDK --> Si genera un límite de cuota para un recurso de red que forma parte de una oferta o plan que está asociado a una suscripción de inquilino, el nuevo límite no se aplicará a esa suscripción. Sin embargo, el nuevo límite se aplicará a las nuevas suscripciones que se creen después de que se aumente la cuota. 

  Para solucionar este problema, use un plan complementario para aumentar una cuota de red cuando el plan ya esté asociado a una suscripción. Para obtener más información, consulte cómo [disponer de un plan complementario](.\.\azure-stack-subscribe-plan-provision-vm.md#to-make-an-add-on-plan-available).

- <!-- 2304134 IS ASDK --> No se puede eliminar una suscripción que disponga de recursos de zona DNS o recursos de tabla de rutas asociados a ella. Para eliminar correctamente la suscripción, primero debe eliminar los recursos de la zona DNS y de la tabla de rutas de la suscripción de inquilino. 


- <!-- 1902460 -  IS ASDK -->Azure Stack admite una única *puerta de enlace de red local* por dirección IP. Y esto se aplica a las suscripciones de todos los inquilinos. Tras la creación de la primera conexión a la puerta de enlace de red local, los sucesivos intentos para crear un recurso de puerta de enlace de red local con la misma dirección IP se bloquean.

- <!-- 16309153 -  IS ASDK --> En una red virtual que se creó con el valor *Automático* en Servidor DNS, se produce un error al cambiar a un servidor DNS personalizado. La configuración actualizada no se inserta en las máquinas virtuales de esa red virtual.
 
- <!-- TBD -  IS ASDK --> Azure Stack no admite la adición de interfaces de red adicionales a una instancia de máquina virtual una vez implementada la máquina virtual. Si la máquina virtual requiere más de una interfaz de red, se deben definir en el momento de la implementación.


#### <a name="sql-and-mysql"></a>SQL y MySQL 
- <!-- TBD - ASDK --> La base de datos que hospeda los servidores debe estar dedicada al uso por parte del proveedor de recursos y las cargas de trabajo del usuario. No puede usar una instancia de SQL que esté siendo utilizada por otro consumidor, incluido App Services.

- <!-- IS, ASDK --> Los caracteres especiales, entre los que se incluyen espacios y puntos, no se admiten en el nombre de **familia** al crear una SKU para los proveedores de recursos de SQL y MySQL. 

#### <a name="app-service"></a>App Service
- <!-- TBD -  IS ASDK --> Los usuarios deben registrar el proveedor de recursos de almacenamiento antes de crear su primera instancia de Azure Function en la suscripción.

- <!-- TBD -  IS ASDK --> Para escalar horizontalmente la infraestructura (roles de trabajo, administración y front-end), debe usar PowerShell, tal como se describe en las notas de la versión de Compute.
 
#### <a name="usage"></a>Uso  
- <!-- TBD -  IS ASDK --> El uso de los datos del medidor de uso de la dirección IP pública muestran el mismo valor *EventDateTime* para cada registro en lugar de la marca *TimeDate* que muestra cuándo se creó el registro. Actualmente, no puede usar estos datos para realizar un recuento adecuado del uso de la dirección IP pública.

<!--
#### Identity
-->



#### <a name="downloading-azure-stack-tools-from-github"></a>Descarga de las herramientas de Azure Stack desde GitHub
- Al usar el cmdlet *invoke-webrequest* de PowerShell para descargar las herramientas de Azure Stack de Github, recibirá un error:     
    -  *invoke-webrequest : The request was aborted: Could not create SSL/TLS secure channel.* (invoke-webrequest: anulada la solicitud, no se pudo crear un canal seguro SSL/TLS)     

  Este error se produce debido a una degradación reciente de la compatibilidad con GitHub de los estándares criptográficos Tlsv1 y Tlsv1.1 (los predeterminados de PowerShell). Para más información, consulte [Weak cryptographic standards removal notice](https://githubengineering.com/crypto-removal-notice/) (Aviso de eliminación de estándares criptográficos débiles).

<!-- #### Identity -->



## <a name="build-201803021"></a>Compilación 20180302.1

### <a name="new-features-and-fixes"></a>Nuevas características y correcciones
Las nuevas características y revisiones publicadas para la versión 1803 de los sistemas integrados de Azure Stack se aplican al Kit de desarrollo de Azure Stack. Consulte las secciones de [nuevas características](.\.\azure-stack-update-1803.md#new-features) y [nuevas correcciones](.\.\azure-stack-update-1803.md#fixed-issues) de las notas de la versión de la actualización 1803 de Azure Stack para obtener más información.  
> [!IMPORTANT]    
> Algunos de los elementos que aparecen en las secciones de **nuevas características** y **nuevas correcciones** solo son importantes para los sistemas integrados de Azure Stack.

### <a name="changes"></a>Cambios
- La manera de cambiar el estado de una oferta recién creada de *privada* a *pública* o *retirada* ha cambiado. Para más información, consulte [Creación de una oferta](.\.\azure-stack-create-offer.md). 


### <a name="known-issues"></a>Problemas conocidos
 
#### <a name="portal"></a>Portal
- No está disponible la posibilidad de [abrir una nueva solicitud de soporte técnico desde la lista desplegable](.\.\azure-stack-manage-portals.md#quick-access-to-help-and-support) del portal de administración. En su lugar, use el siguiente vínculo:     
    - Para el Kit de desarrollo de Azure Stack, use https://aka.ms/azurestackforum.    

- <!-- 2050709 --> En el portal de administración no se pueden editar las métricas de almacenamiento de Blob service, Table service o Queue service. Si va a Storage y selecciona el icono de Blob service, Table service o Queue service, se abre una hoja nueva que muestra una tabla de métricas de dicho servicio. Si luego selecciona Editar en la parte superior del icono del gráfico de métricas, se abre la hoja Editar gráfico, pero no muestra las opciones para editar las métricas.  

- Verá una alerta de advertencia **Activación necesaria** que le informa de que debe registrar el Kit de desarrollo de Azure Stack. Este comportamiento es normal.

- La eliminación de las suscripciones del usuario da como resultado recursos huérfanos. Como alternativa, elimine primero los recursos del usuario o todo el grupo de recursos y, a continuación, elimine las suscripciones del usuario.

- No puede ver los permisos de la suscripción mediante los portales de Azure Stack. Como alternativa, use PowerShell para comprobar los permisos.

- En el panel del portal de administración, el icono de actualización no muestra información sobre las actualizaciones. Para resolver este problema, haga clic en el icono para actualizarlo.

-   En el portal de administración, puede que vea una alerta crítica del componente Microsoft.Update.Admin. El nombre de la alerta, la descripción y la solución se muestran como:  
    - *ERROR - Template for FaultType ResourceProviderTimeout is missing.* (ERROR: falta la plantilla de FaultType ResourceProviderTimeout)

    Esta alerta puede omitirse sin problemas. 

- En el portal de administración y el portal de usuarios, la hoja de información general no puede cargarse al seleccionarla para las cuentas de almacenamiento que se crearon con una versión anterior de la API (ejemplo: 15-06-2015). 

  Como alternativa, use PowerShell para ejecutar el script **Start-ResourceSynchronization.ps1** para restaurar el acceso a los detalles de la cuenta de almacenamiento. [El script está disponible en GitHub]( https://github.com/Azure/AzureStack-Tools/tree/master/Support/scripts) y debe ejecutarse con las credenciales de administrador del servicio en el host del kit de desarrollo si utiliza el ASDK.  

- La hoja de **Service Health** no se carga. Cuando se abre la hoja de Service Health en el portal del administrador o de usuario, Azure Stack muestra un error y no carga información. Este es el comportamiento esperado. Si bien es posible seleccionar y abrir Service Health, esta característica no está disponible aún, aunque se implementará en una versión futura de Azure Stack.


#### <a name="health-and-monitoring"></a>Estado y supervisión
- <!-- 1264761 - IS ASDK --> Es posible que vea alertas del componente *Controlador de mantenimiento* con los siguientes detalles:  

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

  Ambas alertas pueden omitirse sin problemas. Se cerrarán automáticamente pasado un tiempo.  

- En el portal de administración de Azure Stack, puede que vea una alerta crítica con el nombre **Pending external certificate expiration**(Pendiente de expiración del certificado externo).  Esta alerta puede omitirse sin problemas y afecta a las operaciones del Kit de desarrollo de Azure Stack. 


#### <a name="marketplace"></a>Marketplace
- Los usuarios pueden examinar toda la plataforma Marketplace sin ninguna suscripción y ver elementos administrativos, como planes y ofertas. Estos elementos no funcionan para los usuarios.
 
#### <a name="compute"></a>Proceso
- Los valores de escalado para conjuntos de escalas de máquina virtual no están disponibles en el portal. Como alternativa, puede usar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Debido a diferencias en la versión de PowerShell, debe usar el parámetro `-Name` en lugar de `-VMScaleSetName`.

- Al crear máquinas virtuales en el portal de usuario de Azure Stack, el portal muestra un número incorrecto de discos de datos que se pueden asociar a una máquina virtual de la serie DS. Las máquinas virtuales de la serie DS pueden albergar tantos discos de datos como la configuración de Azure.

- Cuando no es posible crear una imagen de máquina virtual, los elementos con error que no se pueden eliminar se podrían agregar a la hoja de proceso de imágenes de máquina virtual.

  Como alternativa, cree una nueva imagen de máquina virtual con un disco duro virtual ficticio que se pueda crear mediante Hyper-V (nuevo VHD, ruta de acceso C:\dummy.vhd, fijo, bytes de tamaño 1 GB). Este proceso debería corregir el problema que impide que se elimine el elemento con error. A continuación, 15 minutos después de crear la imagen ficticia, puede eliminarlo correctamente.

  Luego puede volver a intentar cargar la imagen de máquina virtual que anteriormente produjo un error.

-  Si aprovisionar una extensión en una implementación de máquina virtual tarda demasiado tiempo, los usuarios deberían dejar que se agote el tiempo de espera de aprovisionamiento en lugar de intentar detener el proceso para desasignar o eliminar la máquina virtual.  

- <!-- 1662991 --> No se admite el diagnóstico de máquinas virtuales Linux en Azure Stack. Si implementa una máquina virtual Linux con diagnósticos de máquina virtual habilitado, se producirá un error en la implementación. Tampoco se podrá realizar la implementación si habilita las métricas básicas de máquina virtual Linux a través de la configuración de diagnóstico. 


#### <a name="networking"></a>Redes
- En **Red**, si hace clic en **Conexión** para configurar una conexión VPN, **VNet a VNet** aparece como un tipo de conexión posible. No seleccione esta opción. Actualmente, solo se admite la opción **De sitio a sitio (IPsec)**.

- Después de crear una máquina virtual y asociarla a una dirección IP pública, no puede desasociar esa máquina virtual de esa dirección IP. Puede que parezca que se ha desasociado, pero la dirección IP pública anteriormente asignada permanece asociada a la máquina virtual original.

  Actualmente, solo debe usar nuevas direcciones IP públicas para las nuevas máquinas virtuales creadas.

  Este comportamiento se producirá incluso si vuelve a asignar la dirección IP a una nueva máquina virtual (lo que normalmente se conoce como *intercambio de VIP*). Todos los intentos futuros de conectarse a través de esta dirección IP tendrán como resultado una conexión a la máquina virtual original, y no a la nueva.



- Azure Stack admite una única *puerta de enlace de red local* por dirección IP. Y esto se aplica a las suscripciones de todos los inquilinos. Tras la creación de la primera conexión a la puerta de enlace de red local, los sucesivos intentos para crear un recurso de puerta de enlace de red local con la misma dirección IP se bloquean.

- En una red virtual que se creó con una configuración de servidor DNS de *Automática*, se produce un error al cambiar a un servidor DNS personalizado. La configuración actualizada no se inserta en las máquinas virtuales de esa red virtual.
 
- Azure Stack no admite la adición de interfaces de red adicionales a una instancia de máquina virtual una vez implementada la máquina virtual. Si la máquina virtual requiere más de una interfaz de red, se deben definir en el momento de la implementación.



#### <a name="sql-and-mysql"></a>SQL y MySQL 
- Puede pasar una hora antes de que los usuarios puedan crear bases de datos en una nueva SKU de SQL o MySQL.

- La base de datos que hospeda los servidores debe estar dedicada al uso por el proveedor de recursos y las cargas de trabajo de usuario. No puede usar una instancia de SQL que esté siendo utilizada por otro consumidor, incluido App Services.

- <!-- IS, ASDK -->No se admiten caracteres especiales, lo que incluye espacios y puntos, en los nombres de **familia** o **capa** al crear una SKU para los proveedores de recursos SQL y MySQL.

#### <a name="app-service"></a>App Service
- Los usuarios deben registrar el proveedor de recursos de almacenamiento antes de crear su primera función de Azure en la suscripción.

- Para escalar horizontalmente la infraestructura (roles de trabajo, administración y front-end), debe usar PowerShell, tal como se describe en las notas de la versión de proceso.
 
#### <a name="usage"></a>Uso  
- El uso de los datos del medidor de uso de la dirección IP pública muestran el mismo valor *EventDateTime* para cada registro en lugar de la marca *TimeDate* que muestra cuándo se creó el registro. Actualmente, no puede usar estos datos para realizar un recuento adecuado del uso de la dirección IP pública.
<!--
#### Identity
-->

#### <a name="downloading-azure-stack-tools-from-github"></a>Descarga de las herramientas de Azure Stack desde GitHub
- Al usar el cmdlet *invoke-webrequest* de PowerShell para descargar las herramientas de Azure Stack de Github, recibirá un error:     
    -  *invoke-webrequest : The request was aborted: Could not create SSL/TLS secure channel.* (invoke-webrequest: anulada la solicitud, no se pudo crear un canal seguro SSL/TLS)     

  Este error se produce debido a una degradación reciente de la compatibilidad con GitHub de los estándares criptográficos Tlsv1 y Tlsv1.1 (los predeterminados de PowerShell). Para más información, consulte [Weak cryptographic standards removal notice](https://githubengineering.com/crypto-removal-notice/) (Aviso de eliminación de estándares criptográficos débiles).

  Para resolver este problema, agregue `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12` a la parte superior del script para obligar a la consola de PowerShell a usar TLSv1.2 al realizar la descarga desde repositorios de GitHub.





