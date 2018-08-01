---
title: Actualización de Azure Stack 1807 | Microsoft Docs
description: Conozca las novedades de la actualización 1807 de los sistemas integrados de Azure Stack, incluidos los problemas conocidos y la ubicación donde debe descargarse la actualización.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2018
ms.author: brenduns
ms.reviewer: justini
ms.openlocfilehash: fe37a62d364d53d03a232b6fdc41e38a9ae2e30f
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39227691"
---
# <a name="azure-stack-1807-update"></a>Actualización de Azure Stack 1807

*Se aplica a: sistemas integrados de Azure Stack*

En este artículo se describe el contenido de la actualización 1807. Esta actualización incluye mejoras, correcciones y problemas conocidos de esta versión de Azure Stack y dónde debe descargarse la actualización. Los problemas conocidos se dividen en aquellos que están directamente relacionados con el proceso de actualización y aquellos que están relacionados con la compilación (posteriores a la instalación).

> [!IMPORTANT]        
> Este paquete de actualización es únicamente para los sistemas integrados de Azure Stack. No la aplique al Kit de desarrollo de Azure Stack.

## <a name="build-reference"></a>Referencia de compilación    
El número de compilación de la actualización de Azure Stack 1807 es **1.1807.XX.X**.  


### <a name="new-features"></a>Nuevas características
Esta actualización incluye las siguientes correcciones para Azure Stack.

- <!-- 1658937 | ASDK, IS --> **Iniciar copias de seguridad según una programación predefinida**: como un dispositivo, Azure Stack ahora puede desencadenar automáticamente las copias de seguridad de infraestructura periódicamente para eliminar la intervención humana. Azure Stack también limpiará automáticamente el recurso compartido externo para las copias de seguridad anteriores al período de retención definido. 

- <!-- 2496385 | ASDK, IS --> **Tiempo de transferencia de datos agregado al tiempo total de copia de seguridad.**

-   <!-- 1702130 | ASDK, IS --> **La capacidad externa de copia de seguridad ahora muestra la capacidad correcta del recurso compartido externo.** (Anteriormente esto se codificada de forma rígida a 10 GB).

- <!-- 2508488 |  IS   -->   Expanda la capacidad [agregando nodos de unidad de escalado adicionales](azure-stack-add-scale-node.md).



### <a name="fixed-issues"></a>Problemas corregidos

- <!-- 448955 | IS ASDK --> Ahora puede consultar correctamente los registros de actividad para los sistemas que se implementan en una zona de horaria UTC+N.    

- <!-- 2319627 |  ASDK, IS --> La comprobación previa de los parámetros de configuración de copia de seguridad (Ruta de acceso, nombre de usuario, contraseña y clave de cifrado) ya no establece una configuración incorrecta en la configuración de copia de seguridad. (Anteriormente, se establecía una configuración incorrecta en la copia de seguridad y esta produciría después un error al desencadenarse).

- <!-- 2215948 |  ASDK, IS --> La lista de copias de seguridad ahora se actualiza cuando elimina manualmente la copia de seguridad desde el recurso compartido externo.

- <!-- 2332636 - IS -->  La actualización a esta versión ya no restablece el propietario predeterminado de la suscripción del proveedor predeterminado al usuario CloudAdmin integrado cuando se implementa con AD FS.

- <!-- 2360715 |  ASDK, IS -->  Al configurar la integración del centro de datos, ya no se accede al archivo de metadatos de AD FS desde un recurso compartido. Para más información, consulte [Configuración de la integración de AD FS mediante el archivo de metadatos de federación](azure-stack-integrate-identity.md#setting-up-ad-fs-integration-by-providing-federation-metadata-file). 

- <!-- 2388980 | ASDK, IS --> Se ha corregido un problema que impedía a los usuarios asignar una dirección IP pública existente, que se había asignado previamente a una interfaz de red o a un equilibrador de carga, a una nueva interfaz de red o equilibrador de carga.  

-   <!--2292271 | ASDK, IS --> Se ha corregido un problema donde un límite de cuota modificado no se aplicaba a las suscripciones existentes.  Ahora, si genera un límite de cuota para un recurso de red que forma parte de una oferta o plan asociado a una suscripción de inquilino, el nuevo límite se aplica a las suscripciones previamente existentes.

- **Varias revisiones** de rendimiento, estabilidad, seguridad y el sistema operativo que se usa en Azure Stack.


<!-- ### Additional releases timed with this update    -->



## <a name="before-you-begin"></a>Antes de empezar    

### <a name="prerequisites"></a>Requisitos previos
- Antes de aplicar la actualización de Azure Stack 1807, instale la [actualización 1805](azure-stack-update-1805.md).  No había actualización 1806.  

- Antes de iniciar la instalación de la actualización 1807, ejecute [Test-AzureStack](azure-stack-diagnostic-test.md) para validar el estado de Azure Stack y resolver todos los problemas operativos que se encuentren. Repase también las alertas activas y resuelva las que requieran alguna acción.

### <a name="known-issues-with-the-update-process"></a>Problemas conocidos relacionados con el proceso de actualización   
- <!-- 2468613 - IS --> Durante la instalación de esta actualización, puede que vea alertas con el título *Error – Template for FaultType UserAccounts.New is missing (Error: Falta la plantilla para FaultType UserAccounts.New).*  Puede omitir estas alertas con seguridad. Estas alertas se cerrarán automáticamente cuando se complete la instalación de esta actualización.   

- <!-- 2489559 - IS --> No intente crear máquinas virtuales durante la instalación de esta actualización. Para más información sobre cómo administrar las actualizaciones, consulte [Introducción a la administración de actualizaciones en Azure Stack](azure-stack-updates.md#plan-for-updates).


### <a name="post-update-steps"></a>Pasos posteriores a la actualización
*No hay ningún paso posterior para la actualización 1807.*

<!-- After the installation of this update, install any applicable Hotfixes. For more information view the following knowledge base articles, as well as our [Servicing Policy](azure-stack-servicing-policy.md).  
 - Link to KB  
 -->

## <a name="known-issues-post-installation"></a>Problemas conocidos (posteriores a la instalación)
Los siguientes son problemas conocidos posteriores a la instalación de esta compilación.

### <a name="portal"></a>Portal  
- <!-- 2551834 - IS, ASDK --> Cuando se selecciona **Información general** para una cuenta de almacenamiento en los portales de administrador o de usuario, la información del panel *Información esencial* no aparece.  Este panel muestra información sobre la cuenta como, por ejemplo, su *grupo de recursos*, *ubicación* y el *identificador de la suscripción*.  Se puede acceder a otras opciones de Información general como *Servicios* y *Supervisión*, así como a opciones para *Abrir en el Explorador* o para *Eliminar cuenta de almacenamiento*.

  Para ver la información no disponible, use el cmdlet de PowerShell [Get-azureRMstorageaccount](https://docs.microsoft.com/powershell/module/azurerm.storage/get-azurermstorageaccount?view=azurermps-6.2.0).

- <!-- 2551834 - IS, ASDK --> Cuando se selecciona **Etiquetas** para una cuenta de almacenamiento en los portales de administrador o de usuario, la información no se puede cargar y no aparece.  

  Para ver la información no disponible, use el cmdlet de PowerShell [Get-AzureRmTag](https://docs.microsoft.com/powershell/module/azurerm.tags/get-azurermtag?view=azurermps-6.2.0). 

- <!-- TBD - IS ASDK --> Algunos tipos de suscripción administrativa no están disponibles.  Cuando actualiza Azure Stack a esta versión, los dos tipos de suscripciones que se [introdujeron con la versión 1804](azure-stack-update-1804.md#new-features) no aparecen en la consola. Se espera que esto sea así. Los tipos de suscripción no disponibles son *suscripción de medición* y *suscripción de consumo*. Estos tipos de suscripción están visibles en los nuevos entornos de Azure Stack a partir de la versión 1804 pero aún no están listos para su uso. Tendrá que seguir usando el tipo de suscripción del *proveedor predeterminado*.  

- <!-- 2403291 - IS ASDK --> No puede usar la barra de desplazamiento horizontal a lo largo de la parte inferior de los portales de administrador y de usuario. Si no puede acceder a la barra de desplazamiento horizontal, use las rutas de navegación para ir a una hoja anterior del portal seleccionando la hoja que desea ver en la lista de rutas que se encuentra en la parte superior izquierda del portal.
  ![Ruta de navegación](media/azure-stack-update-1804/breadcrumb.png)

- <!-- TBD - IS --> No es posible ver los recursos de proceso o almacenamiento en el portal de administración. La causa de este problema es un error durante la instalación de la actualización que hace que se notifique como correcta erróneamente. Si se produce este problema, póngase en contacto con los servicios de soporte técnico de Microsoft para obtener ayuda.

- <!-- TBD - IS --> Puede que vea un panel en blanco en el portal. Para recuperar el panel, seleccione el icono de engranaje en la esquina superior derecha del portal y, a continuación, seleccione **Restaurar configuración predeterminada**.

- <!-- TBD - IS ASDK --> La eliminación de las suscripciones del usuario da como resultado recursos huérfanos. Como alternativa, elimine primero los recursos del usuario o todo el grupo de recursos y, a continuación, elimine las suscripciones del usuario.

- <!-- TBD - IS ASDK --> No puede ver los permisos de la suscripción mediante los portales de Azure Stack. Como alternativa, use PowerShell para comprobar los permisos.


### <a name="health-and-monitoring"></a>Estado y supervisión
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

- <!-- 2368581 - IS. ASDK --> Operador de Azure Stack: si recibe una alerta de memoria insuficiente y no se pueden implementar las máquinas virtuales del inquilino debido a un *error de creación de máquina virtual de Fabric*, es posible que la marca de Azure Stack supere la memoria disponible. Use la [herramienta de planeamiento de capacidad de Azure Stack](https://gallery.technet.microsoft.com/Azure-Stack-Capacity-24ccd822) para comprender mejor la capacidad disponible para las cargas de trabajo.


### <a name="compute"></a>Compute
- <!-- 2724873 - IS --> Al usar los cmdlets de PowerShell **Start-AzsScaleUnitNode** o **Stop-AzsScaleunitNode** para administrar las unidades de escalado, el primer intento para iniciar o detener la unidad de escalado podría producir un error. Si se produce un error en el cmdlet en la primera ejecución, ejecute el cmdlet una segunda vez. La segunda ejecución debe ser correcta para completar la operación. 

- <!-- 2494144 - IS, ASDK --> Al seleccionar un tamaño de máquina virtual para una implementación, algunos tamaños de la serie F no aparecen en el selector de tamaño al crear una máquina virtual. Los siguientes tamaños de máquinas virtuales no aparecen en el selector: *F8s_v2*, *F16s_v2*, *F32s_v2* y *F64s_v2*.  
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


- <!-- TBD - IS ASDK --> Los valores de escalado de los conjuntos de escalado de máquinas virtuales no están disponibles en el portal. Como alternativa, puede usar [Azure PowerShell](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-manage-powershell#change-the-capacity-of-a-scale-set). Debido a diferencias en la versión de PowerShell, debe usar el parámetro `-Name` en lugar de `-VMScaleSetName`.

- <!-- TBD - IS --> Cuando crea un conjunto de disponibilidad en el portal en **Nuevo** > **Compute** > **Conjunto de disponibilidad**, solo puede crear uno con un dominio de error y un dominio de actualización de 1. Como alternativa, al crear una nueva máquina virtual, cree el conjunto de disponibilidad mediante PowerShell, la CLI o el portal.

- <!-- TBD - IS ASDK --> Al crear máquinas virtuales en el portal de usuario de Azure Stack, el portal muestra un número incorrecto de discos de datos que se pueden asociar a una máquina virtual de la serie DS. Las máquinas virtuales de la serie DS pueden albergar tantos discos de datos como la configuración de Azure.

- <!-- TBD - IS ASDK --> Cuando no es posible crear una imagen de máquina virtual, los elementos con error que no se pueden eliminar se podrían agregar a la hoja de proceso de imágenes de máquina virtual.

  Como alternativa, cree una nueva imagen de máquina virtual con un disco duro virtual ficticio que se pueda crear mediante Hyper-V (nuevo VHD, ruta de acceso C:\dummy.vhd, fijo, bytes de tamaño 1 GB). Este proceso debería corregir el problema que impide que se elimine el elemento con error. A continuación, 15 minutos después de crear la imagen ficticia, puede eliminarlo correctamente.

  Luego puede volver a intentar cargar la imagen de máquina virtual que anteriormente produjo un error.

- <!-- TBD - IS ASDK --> Si aprovisionar una extensión en una implementación de máquina virtual tarda demasiado tiempo, los usuarios deberían dejar que se agote el tiempo de espera de aprovisionamiento en lugar de intentar detener el proceso para desasignar o eliminar la máquina virtual.  

- <!-- 1662991 IS ASDK --> No se admite el diagnóstico de máquinas virtuales Linux en Azure Stack. Si implementa una máquina virtual Linux con diagnósticos de máquina virtual habilitado, se producirá un error en la implementación. Tampoco se podrá realizar la implementación si habilita las métricas básicas de máquina virtual Linux a través de la configuración de diagnóstico.  


### <a name="networking"></a>Redes  

- <!-- 1766332 - IS ASDK --> En **Redes**, si hace clic en **Create VPN Gateway** (Crear instancia de VPN Gateway) para configurar una conexión VPN, aparece **Policy Based** (Basada en directivas) como un tipo de VPN. No seleccione esta opción. En Azure Stack solo se admite la opción **Route Based** (Basada en rutas).

- <!-- 2304134 IS ASDK --> No se puede eliminar una suscripción que disponga de recursos de zona DNS o recursos de tabla de rutas asociados a ella. Para eliminar correctamente la suscripción, primero debe eliminar los recursos de la zona DNS y de la tabla de rutas de la suscripción de inquilino.


- <!-- 1902460 - IS ASDK -->Azure Stack admite una única *puerta de enlace de red local* por dirección IP. Y esto se aplica a las suscripciones de todos los inquilinos. Tras la creación de la primera conexión a la puerta de enlace de red local, los sucesivos intentos para crear un recurso de puerta de enlace de red local con la misma dirección IP se bloquean.

- <!-- 16309153 - IS ASDK --> En una red virtual que se creó con una configuración de servidor DNS de *Automática*, se produce un error al cambiar a un servidor DNS personalizado. La configuración actualizada no se inserta en las máquinas virtuales de esa red virtual.

- <!-- TBD - IS ASDK --> Azure Stack no admite la adición de interfaces de red adicionales a una instancia de máquina virtual una vez implementada la máquina virtual. Si la máquina virtual requiere más de una interfaz de red, se deben definir en el momento de la implementación.

- <!-- 2096388 IS --> No se puede usar el portal de administración para actualizar las reglas de un grupo de seguridad de red.

    Solución alternativa para App Service: si tiene que establecer una conexión de Escritorio remoto a las instancias del controlador, modifique las reglas de seguridad de los grupos de seguridad de red con PowerShell.  Estos son algunos ejemplos de cómo *permitir* y, luego, restaurar la configuración a *denegar*:  

    - *Permitir:*

      ```powershell    
      Connect-AzureRmAccount -EnvironmentName AzureStackAdmin

      $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

      $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

      ##This doesn’t work. Need to set properties again even in case of edit

      #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  

      Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
        -Name $RuleConfig_Inbound_Rdp_3389.Name `
        -Description "Inbound_Rdp_3389" `
        -Access Allow `
        -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
        -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
        -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
        -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
        -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
        -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
        -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

      # Commit the changes back to NSG
      Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
      ```

    - *Denegar:*

        ```powershell

        Connect-AzureRmAccount -EnvironmentName AzureStackAdmin

        $nsg = Get-AzureRmNetworkSecurityGroup -Name "ControllersNsg" -ResourceGroupName "AppService.local"

        $RuleConfig_Inbound_Rdp_3389 =  $nsg | Get-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389"

        ##This doesn’t work. Need to set properties again even in case of edit

        #Set-AzureRmNetworkSecurityRuleConfig -Name "Inbound_Rdp_3389" -NetworkSecurityGroup $nsg -Access Allow  

        Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
          -Name $RuleConfig_Inbound_Rdp_3389.Name `
          -Description "Inbound_Rdp_3389" `
          -Access Deny `
          -Protocol $RuleConfig_Inbound_Rdp_3389.Protocol `
          -Direction $RuleConfig_Inbound_Rdp_3389.Direction `
          -Priority $RuleConfig_Inbound_Rdp_3389.Priority `
          -SourceAddressPrefix $RuleConfig_Inbound_Rdp_3389.SourceAddressPrefix `
          -SourcePortRange $RuleConfig_Inbound_Rdp_3389.SourcePortRange `
          -DestinationAddressPrefix $RuleConfig_Inbound_Rdp_3389.DestinationAddressPrefix `
          -DestinationPortRange $RuleConfig_Inbound_Rdp_3389.DestinationPortRange

        # Commit the changes back to NSG
        Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
        ```


### <a name="sql-and-mysql"></a>SQL y MySQL

- <!-- TBD - IS --> Solo el proveedor de recursos puede crear elementos en servidores que hospedan SQL o MySQL. Los elementos creados en un servidor host que no se crean con el proveedor de recursos podrían dar lugar a un error de coincidencia de estado.  

- <!-- IS, ASDK -->No se admiten caracteres especiales, lo que incluye espacios y puntos, en los nombres de **familia** o **capa** al crear una SKU para los proveedores de recursos SQL y MySQL.


> [!NOTE]  
> <!-- TBD - IS --> Después de actualizar a esta versión de Azure Stack, puede seguir usando los proveedores de recursos SQL y MySQL que implementó anteriormente.  Le recomendamos que actualice SQL y MySQL cuando haya disponible una nueva versión. Al igual que Azure Stack, aplique las actualizaciones a los proveedores de recursos SQL y MySQL de manera secuencial. Por ejemplo, si usa la versión 1804, aplique primero la versión 1805 y luego actualice a la 1807.      
>   
> La instalación de esta actualización no afecta al uso actual de los proveedores de recursos SQL o MySQL por parte de los usuarios.
> Con independencia de la versión que use de los proveedores de recursos, los datos de los usuarios de sus bases de datos no se tocan, y permanecen accesibles.    



### <a name="app-service"></a>App Service
- <!-- 2352906 - IS ASDK --> Los usuarios deben registrar el proveedor de recursos de almacenamiento antes de crear su primera función de Azure en la suscripción.

- <!-- 2489178 - IS ASDK --> Para escalar horizontalmente la infraestructura (roles de trabajo, administración y front-end), debe usar PowerShell, tal como se describe en las notas de la versión de Compute.

- <!-- TBD - IS ASDK --> App Service solo puede implementarse en la *suscripción del proveedor predeterminado* en este momento. En una próxima actualización, App Service se podrá implementar en la nueva *suscripción de medición* que se introdujo en Azure Stack 1804. Cuando se admita el uso de la suscripción de medición, todas las implementaciones existentes se migrarán a este nuevo tipo de suscripción.


### <a name="usage"></a>Uso  
- <!-- TBD - IS ASDK --> El uso de los datos del medidor de uso de la dirección IP pública muestran el mismo valor *EventDateTime* para cada registro en lugar de la marca *TimeDate* que muestra cuándo se creó el registro. Actualmente, no puede usar estos datos para realizar un recuento adecuado del uso de la dirección IP pública.


<!-- #### Identity -->
<!-- #### Marketplace -->


## <a name="download-the-update"></a>Descarga de la actualización
Puede descargar el paquete de actualización de Azure Stack 1807 desde [aquí](https://aka.ms/azurestackupdatedownload).


## <a name="see-also"></a>Otras referencias
- Para usar el punto de conexión con privilegios (PEP) para supervisar y reanudar las actualizaciones, consulte [Supervisión de las actualizaciones en Azure Stack mediante el uso del punto de conexión con privilegios](azure-stack-monitor-update.md).
- Para información general sobre la administración de actualizaciones en Azure Stack, consulte [Introducción a la administración de actualizaciones en Azure Stack](azure-stack-updates.md).
- Para más información sobre cómo aplicar actualizaciones con Azure Stack, consulte [Aplicación de actualizaciones en Azure Stack](azure-stack-apply-updates.md).
