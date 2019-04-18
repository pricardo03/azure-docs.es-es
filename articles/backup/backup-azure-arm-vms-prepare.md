---
title: Realizar una copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services mediante Azure Backup
description: Describe cómo realizar una copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services mediante la copia de seguridad de Azure
service: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: raynew
ms.openlocfilehash: 142ffdadf4adb1ee07f3592624cbdddfb310b580
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59264563"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services

En este artículo se describe cómo realizar una copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services mediante el [Azure Backup](backup-overview.md) service. 

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Prepare las máquinas virtuales de Azure.
> * Crear un almacén.
> * Detectar las máquinas virtuales y configurar una directiva de copia de seguridad.
> * Habilitar copia de seguridad para máquinas virtuales de Azure.
> * Realizar la copia de seguridad inicial.


> [!NOTE]
> En este artículo se describe cómo configurar un almacén y seleccione las máquinas virtuales para realizar copias de seguridad. Resulta útil si desea realizar copias de seguridad de varias máquinas virtuales. Como alternativa, puede [copia de seguridad de una sola máquina virtual de Azure](backup-azure-vms-first-look-arm.md) directamente desde la configuración de máquina virtual.

## <a name="before-you-start"></a>Antes de comenzar


- [Revisión](backup-architecture.md#architecture-direct-backup-of-azure-vms) la arquitectura de copia de seguridad de máquina virtual de Azure.
- [Más información sobre](backup-azure-vms-introduction.md) la copia de seguridad de máquinas virtuales de Azure y la extensión de reserva.
- [Revise la matriz de compatibilidad](backup-support-matrix-iaas.md) antes de configurar la copia de seguridad.

Además, hay un par de cosas que debe hacer en algunas circunstancias:

- **Instale el agente de máquina virtual en la máquina virtual**: Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina. Si se creó la máquina virtual desde una imagen de marketplace de Azure, el agente está instalado y ejecutándose. Si crea una máquina virtual personalizada, o migrar una máquina local, es posible que deba [instalar el agente manualmente](#install-the-vm-agent).
- **Permitir explícitamente el acceso de salida**: Por lo general, no es necesario permitir explícitamente el acceso de red saliente para que una máquina virtual de Azure para poder comunicarse con Azure Backup. Sin embargo, algunas máquinas virtuales podrían experimentar problemas de conexión, que muestra la **ExtensionSnapshotFailedNoNetwork** error al intentar conectarse. Si esto sucede, debería [permitir explícitamente el acceso de salida](#explicitly-allow-outbound-access), por lo que la extensión de copia de seguridad de Azure puede comunicarse con Azure direcciones IP públicas para el tráfico de copia de seguridad.


## <a name="create-a-vault"></a>Creación de un almacén

 Un almacén almacena las copias de seguridad y los puntos de recuperación creados con el tiempo y almacena las directivas de copia de seguridad asociadas a máquinas de las que se han realizado copias de seguridad. Cree un almacén como se indica a continuación:    

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).    
2. En la búsqueda, escriba **Recovery Services**. En **servicios**, haga clic en **almacenes de Recovery Services**.   

     ![Buscar almacenes de Recovery Services](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/> 

3. En **almacenes de Recovery Services** menú, haga clic en **+ agregar**.    

     ![Creación del almacén de Recovery Services, paso 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)   

4. En **almacén de Recovery Services**, escriba un nombre descriptivo para identificar el almacén.   
    - El nombre debe ser único para la suscripción de Azure.   
    - Puede contener entre 2 y 50 caracteres.    
    - Debe comenzar por una letra y solo puede contener letras, números y guiones.   
5. Seleccione la suscripción de Azure, el grupo de recursos y la región geográfica en la que se debe crear el almacén. A continuación, haga clic en **Crear**.    
    - La creación del almacén puede tardar un tiempo.  
    - Supervise las notificaciones del estado en la parte superior derecha del portal.   


 Una vez creado el almacén, aparece en la lista de almacenes de Recovery Services. Si no lo ve, haga clic en **Actualizar**.
 
![Lista de copias de seguridad](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)    

### <a name="modify-storage-replication"></a>Modificar la replicación de almacenamiento

De forma predeterminada, los almacenes de uso [almacenamiento con redundancia geográfica (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).

- Si el almacén es el principal mecanismo de copia de seguridad, se recomienda que usar GRS.
- Puede usar [almacenamiento con redundancia local (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para una opción más económica.

Modifique el tipo de replicación de almacenamiento como sigue:

1. En el nuevo almacén, haga clic en **propiedades** en el **configuración** sección.
2. En **propiedades**, en **configuración de copia de seguridad**, haga clic en **actualización**.
3. Seleccione el tipo de replicación de almacenamiento y haga clic en **guardar**.

      ![Establecimiento de la configuración de almacenamiento del nuevo almacén](./media/backup-try-azure-backup-in-10-mins/full-blade.png)
> [!NOTE]
   > No se puede modificar el tipo de replicación de almacenamiento después de que el almacén se ha configurado y contiene elementos de copia de seguridad. Si desea hacer esto se que debe volver a crear el almacén. 

## <a name="apply-a-backup-policy"></a>Aplicar una directiva de copia de seguridad

Configurar una directiva de copia de seguridad para el almacén.

1. En el almacén, haga clic en **+ Backup** en el **Introducción** sección.

   ![Botón Backup](./media/backup-azure-arm-vms-prepare/backup-button.png)


2. En **objetivo de Backup** > **donde se ejecuta la carga de trabajo?** seleccione **Azure**. En **¿en qué se desea hacer una copia de seguridad?** seleccione **Máquina Virtual** >  **Aceptar**. Esto registra la extensión de la máquina virtual en el almacén.

   ![Paneles Backup y Objetivo de Backup](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. En **Directiva de copia de seguridad**, seleccione la directiva que desea asociar al almacén. 
    - La directiva predeterminada realiza una copia de seguridad de la máquina virtual una vez al día. Las copias de seguridad diarias se conservan durante 30 días. Se conservan las instantáneas de recuperación instantánea durante dos días.
    - Si no desea usar la directiva predeterminada, seleccione **crear nuevo**y crear una directiva personalizada, como se describe en el procedimiento siguiente.

      ![Directiva de copia de seguridad predeterminada](./media/backup-azure-arm-vms-prepare/default-policy.png)

4. En **seleccionar máquinas virtuales**, seleccione las máquinas virtuales que desea realizar copias de seguridad mediante la directiva. A continuación, haga clic en **Aceptar**.

   - Se validan las máquinas virtuales seleccionadas.
   - Solo se puede seleccionar máquinas virtuales de la región en que se encuentre el almacén.
   - Las copias de seguridad de las máquinas virtuales solo se pueden realizar en un almacén individual.

     ![Panel "Seleccionar máquinas virtuales"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. En **copia de seguridad**, haga clic en **habilitar copia de seguridad**. Esto implementa la directiva en el almacén y las máquinas virtuales, e instala la extensión de copia de seguridad en el agente de máquina virtual que se ejecuta en la máquina virtual de Azure.
     
     ![Botón "Habilitar copia de seguridad"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Después de habilitar la copia de seguridad:

- El servicio Backup instala la extensión de copia de seguridad tanto si la máquina virtual está en ejecución como si no lo está.
- Se ejecutará una copia de seguridad inicial con arreglo a la programación de copia de seguridad.
- Al ejecutan las copias de seguridad, tenga en cuenta:
    - Una máquina virtual que se está ejecutando ofrecen la mejor oportunidad para capturar un punto de recuperación coherentes con la aplicación.
    - Sin embargo, incluso si se desactiva la máquina virtual es una copia de seguridad. Dicha máquina virtual se conoce como una máquina virtual sin conexión. En este caso, el punto de recuperación será coherente con el bloqueo.
    

### <a name="create-a-custom-policy"></a>Creación de una directiva personalizada

Si ha seleccionado para crear una nueva directiva de copia de seguridad, rellene la configuración de directiva.

1. En **nombre de la directiva**, especifique un nombre descriptivo.
2. En **programación de copia de seguridad** especifique cuándo deben realizarse copias de seguridad. Puede realizar copias de seguridad diarias o semanales para máquinas virtuales de Azure.
2. En **la restauración instantánea**, especifique cuánto tiempo desea conservar las instantáneas localmente para la restauración instantánea.
    - Cuando se restaura, copia de seguridad de VM se copiarán los discos de almacenamiento, a través de la red en la ubicación de almacenamiento de recuperación. Con la restauración instantánea, puede aprovechar realizadas durante un trabajo de copia de seguridad, sin tener que esperar para que se transfiere al almacén de los datos de copia de seguridad de instantáneas almacenadas localmente.
    - Puede conservar las instantáneas para la restauración instantánea para entre uno y cinco días. Dos días es el valor predeterminado.
3. En **duración de retención**, especifique cuánto tiempo desea conservar los puntos de copia de seguridad diarios o semanales.
4. En **retención de punto de copia de seguridad mensual**, especifique si desea mantener una cuota mensual copia de seguridad de las copias de seguridad diarias o semanales. 
5. Haga clic en **Aceptar** para guardar la directiva.

    ![Nueva directiva de copia de seguridad](./media/backup-azure-arm-vms-prepare/new-policy.png)

> [!NOTE]
   > Azure Backup no admite el ajuste automático del reloj para los cambios de horario de verano para realizar copias de seguridad de máquinas virtuales de Azure. Cuando se producen cambios de tiempo, modificar las directivas de copia de seguridad manualmente según sea necesario.

## <a name="trigger-the-initial-backup"></a>Desencadenar la copia de seguridad inicial

La copia de seguridad inicial se ejecutará según la programación, pero puede ejecutarlo inmediatamente la manera siguiente:

1. En el menú Almacén, haga clic en **Elementos de copia de seguridad**.
2. En **Elementos de copia de seguridad**, haga clic en **Máquina virtual de Azure**.
3. En el **elementos de copia de seguridad** lista, haga clic en el botón de puntos suspensivos (...).
4. Haga clic en **Realizar copia de seguridad ahora**.
5. En **copia de seguridad ahora**, use el control de calendario para seleccionar el último día que se debe conservar el punto de recuperación. A continuación, haga clic en **Aceptar**.
6. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **Trabajos de copia de seguridad** > **En curso**. Según el tamaño de la máquina virtual, la creación de la copia de seguridad inicial puede tardar un tiempo.

## <a name="optional-steps-install-agentallow-outbound"></a>Pasos opcionales (instalar el agente o permitir el tráfico saliente)
### <a name="install-the-vm-agent"></a>Instalar el agente de máquina virtual

Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina. Si se creó la máquina virtual desde una imagen de Marketplace de Azure, el agente está instalado y ejecutándose. Si crea una máquina virtual personalizada, o migrar una máquina local, es posible que deba instalar al agente manualmente, como se resume en la tabla.

**VM** | **Detalles**
--- | ---
**Windows** | 1. [Descargue e instale](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) el archivo MSI del agente.<br/><br/> 2. Realice la instalación con permisos de administrador en el equipo.<br/><br/> 3. Comprobar la instalación. En *C:\WindowsAzure\Packages* en la máquina virtual, haga clic en **WaAppAgent.exe** > **propiedades**. En el **detalles** ficha, **versión del producto** debe ser el valor 2.6.1198.718 o superior.<br/><br/> Si va a actualizar el agente, asegúrese de que no se está ejecutando ninguna operación de copia de seguridad, y [volver a instalar el agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux** | Instalar mediante el uso de un paquete de DEB o RPM del repositorio de paquetes de su distribución. Este es el método preferido para instalar y actualizar al agente Linux de Azure. Todos los [proveedores de distribución aprobada](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) integran el paquete de agente Linux de Azure en sus imágenes y repositorios. El agente está disponible en [GitHub](https://github.com/Azure/WALinuxAgent), pero no se recomienda instalarlo desde allí.<br/><br/> Si va a actualizar al agente, asegúrese de que está ejecutando ninguna operación de copia de seguridad y actualizar los archivos binarios.

### <a name="explicitly-allow-outbound-access"></a>Permitir explícitamente el acceso de salida

La extensión de copia de seguridad que se ejecuta en la máquina virtual necesita acceso saliente a direcciones IP públicas de Azure.

- Por lo general no es necesario permitir explícitamente el acceso de red saliente para que una máquina virtual de Azure para poder comunicarse con Azure Backup.
- Si tiene dificultades con las máquinas virtuales conectarse o si ve el error **ExtensionSnapshotFailedNoNetwork** al intentar conectarse, se debe permitir expresamente el acceso para la extensión de copia de seguridad puede comunicarse con la dirección IP pública de Azure direcciones para el tráfico de copia de seguridad. En la tabla siguiente se resumen los métodos de acceso.


**Opción** | **Acción** | **Detalles** 
--- | --- | --- 
**Configuración de reglas de NSG** | Permita los [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653).<br/><br/> En lugar de permitir y administrar cada intervalo de direcciones, puede agregar una regla que permita el acceso al servicio Azure Backup mediante un [etiqueta de servicio](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure). | [Más información](../virtual-network/security-overview.md#service-tags) sobre las etiquetas de servicio.<br/><br/> Las etiquetas de servicios simplifican la administración de acceso y no incurrir en costos adicionales.
**Implementación de un proxy** | Implementación de un servidor proxy HTTP para enrutar el tráfico. | Proporciona acceso a la totalidad de Azure, no solo al almacenamiento.<br/><br/> Se permite un control detallado de las direcciones URL de almacenamiento.<br/><br/> Punto individual de acceso a Internet para las máquinas virtuales.<br/><br/> Costos adicionales del proxy.
**Configuración de Azure Firewall** | Permita que el tráfico pase por Azure Firewall en la máquina virtual y utilice una etiqueta de nombre de dominio completo para el servicio Azure Backup. | Fácil de usar si tiene configurado en una subred de red virtual de Firewall de Azure.<br/><br/> No se puede crear sus propias etiquetas FQDN, o modificar el FQDN en una etiqueta.<br/><br/> Si las máquinas virtuales de Azure tienen discos administrados, deberá abrir otro puerto (8443) en los firewalls.

#### <a name="establish-network-connectivity"></a>Establecimiento de conectividad de red

Establecer la conectividad con NSG, proxy, o a través del firewall

##### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Configuración de una regla de grupo de seguridad de red para permitir el acceso saliente a Azure

Si un NSG administra el acceso de máquina virtual, permitir el acceso de salida para el almacenamiento de copia de seguridad a los puertos y los intervalos necesarios.

1. En las propiedades de la máquina virtual > **redes**, seleccione **Agregar regla de puerto de salida**.
2. En **Agregar regla de seguridad de salida**, seleccione **avanzadas**.
3. En **Origen**, seleccione **VirtualNetwork**.
4. En **intervalos de puertos de origen**, escriba un asterisco (*) para permitir el acceso desde cualquier puerto de salida.
5. En **Destino**, seleccione **Etiqueta de servicio**. En la lista, seleccione **Storage.region**. La región es donde se encuentran el almacén y las máquinas virtuales que desea realizar copias de seguridad.
6. En **Intervalos de puertos de destino** , seleccione el puerto.
    - Máquina virtual no administrada con una cuenta de almacenamiento sin cifrar: 80
    - Máquina virtual no administrada con una cuenta de almacenamiento cifrada: 443 (configuración predeterminada)
    - Máquina virtual administrada: 8443.
7. En **Protocolo**: seleccione **TCP**.
8. En **Prioridad**, especifique un valor de prioridad inferior a las reglas de denegación elevadas.
   
   Si tiene una regla que deniegue el acceso, el nuevo permite regla debe ser mayor. Por ejemplo, si tiene un conjunto de reglas **Deny_All** en prioridad 1000, el valor de la nueva regla deben ser menor de 1000.
9. Proporcione un nombre y una descripción para la regla y seleccione **Aceptar**.

Puede aplicar la regla de NSG a varias máquinas virtuales para permitir el acceso de salida. Este vídeo le guía por el proceso.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]


##### <a name="route-backup-traffic-through-a-proxy"></a>Enrutamiento del tráfico de la copia de seguridad a través de un proxy

El tráfico de la copia de seguridad se puede enrutar a través de un proxy y, después, dé al proxy acceso a los intervalos de Azure necesarios. Configurar al proxy de la máquina virtual para permitir lo siguiente:

- La máquina virtual de Azure debe enrutar todo el tráfico HTTP enlazado dirigido a la red pública de Internet a través del proxy.
- El servidor proxy debe permitir el tráfico entrante desde las máquinas virtuales en la red virtual aplicable.
- El grupo de seguridad de red (NSG) **NSF-lockdown** necesita una regla de seguridad que permita el tráfico de Internet de salida desde la máquina virtual proxy.

###### <a name="set-up-the-proxy"></a>Configuración del proxy

Si no tiene un proxy de la cuenta del sistema, configure uno como se indica a continuación:

1. Descargue [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Ejecute **PsExec.exe -i -s cmd.exe** para ejecutar el símbolo del sistema en una cuenta del sistema.
3. Ejecute el explorador en el contexto del sistema. Por ejemplo, usar **%PROGRAMFILES%\Internet Explorer\iexplore.exe** para Internet Explorer.  
4. Defina la configuración del proxy.
   - En equipos Linux:
     - Agregue esta línea al archivo **/etc/environment**:
       - **http_proxy = http:\/puerto de proxy: dirección IP/proxy**
     - Agregue estas líneas al archivo **/etc/waagent.conf**:
         - **HttpProxy.Host=proxy IP address**
         - **HttpProxy.Port=proxy port**
   - En los equipos Windows, en la configuración del explorador, especifique que se debe usar un proxy. Si actualmente usa a un proxy en una cuenta de usuario, puede utilizar este script para aplicar el valor en el nivel de cuenta del sistema.
       ```powershell
      $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
      $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

       ```

###### <a name="allow-incoming-connections-on-the-proxy"></a>Aceptación de conexiones de entrada en el proxy

Permita las conexiones de entrada en la configuración del proxy.

1. En el Firewall de Windows, abra **Firewall de Windows con seguridad avanzada**.
2. Haga clic con el botón derecho en **Reglas de entrada** > **Nueva regla**.
3. En **tipo de regla**, seleccione **personalizado** > **siguiente**.
4. En **Programa**, seleccione **Todos los programas** > **Siguiente**.
5. En **protocolos y puertos**:
   - Establezca el tipo en **TCP**.
   - Establecer **puertos locales** a **puertos específicos**.
   - Establecer **puerto remoto** a **todos los puertos**.
  
6. Finalice el asistente y especifique el nombre de la regla.

###### <a name="add-an-exception-rule-to-the-nsg-for-the-proxy"></a>Adición de una regla de excepción al grupo de seguridad de red para el proxy

En el grupo de seguridad de red **NSF-lockdown**, permita el tráfico desde puerto en la dirección IP 10.0.0.5 a cualquier dirección de Internet en los puertos 80 (HTTP) o 443 (HTTPS).

El siguiente script de PowerShell proporciona un ejemplo en el que se permite el tráfico.
En lugar de permitir salientes a todas las direcciones de internet pública, puede especificar un intervalo de direcciones IP (`-DestinationPortRange`), o bien usar la etiqueta de servicio storage.region.   

```powershell
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

##### <a name="allow-firewall-access-with-an-fqdn-tag"></a>Permitir el acceso de firewall con una etiqueta de nombre de dominio completo

Puede configurar Firewall de Azure para permitir el acceso de salida para tráfico de red a Azure Backup.

- [Más información](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) acerca de cómo implementar Azure Firewall.
- [Más información acerca de](https://docs.microsoft.com/azure/firewall/fqdn-tags) las etiquetas FQDN.



## <a name="next-steps"></a>Pasos siguientes

- Solucionar los problemas con [agentes de máquina virtual de Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) o [copia de seguridad de máquina virtual de Azure](backup-azure-vms-troubleshoot.md).
- [Restaurar](backup-azure-arm-restore-vms.md) máquinas virtuales de Azure.

