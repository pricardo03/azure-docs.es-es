---
title: Realizar una copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services mediante Azure Backup
description: Describe cómo realizar una copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services mediante Azure Backup
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: raynew
ms.openlocfilehash: 3342b15511305ab337d9b5032080e205e36150d3
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59049820"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services

En este artículo se describe cómo realizar una copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services mediante el uso de la [Azure Backup](backup-overview.md) service. 

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Compruebe la compatibilidad y requisitos previos para una copia de seguridad.
> * Prepare las máquinas virtuales de Azure. Instalar el agente de máquina virtual de Azure si es necesario y comprobar el acceso de salida para las máquinas virtuales.
> * Crear un almacén.
> * Detectar las máquinas virtuales y configurar una directiva de copia de seguridad.
> * Habilitar copia de seguridad para máquinas virtuales de Azure.


> [!NOTE]
> En este artículo se describe cómo configurar un almacén y seleccione las máquinas virtuales para realizar copias de seguridad. Resulta útil si desea realizar copias de seguridad de varias máquinas virtuales. Como alternativa, puede [copia de seguridad de una sola máquina virtual de Azure](backup-azure-vms-first-look-arm.md) directamente desde la configuración de máquina virtual.

## <a name="before-you-start"></a>Antes de comenzar


- [Revisión](backup-architecture.md#architecture-direct-backup-of-azure-vms) la arquitectura de copia de seguridad de máquina virtual de Azure.
- [Más información sobre](backup-azure-vms-introduction.md) la copia de seguridad de máquinas virtuales de Azure y la extensión de reserva.
- [Revise la matriz de compatibilidad](backup-support-matrix-iaas.md) para la copia de seguridad de máquinas virtuales de Azure.


## <a name="prepare-azure-vms"></a>Preparación de las máquinas virtuales de Azure

En algunas circunstancias, es posible que deba configurar el agente de máquina virtual de Azure en máquinas virtuales de Azure, o permitir explícitamente el acceso de salida en una máquina virtual.

### <a name="install-the-vm-agent"></a>Instalar el agente de máquina virtual 

Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina. Si se creó la máquina virtual desde una imagen de Marketplace de Azure, el agente está instalado y ejecutándose. Si crea una máquina virtual personalizada, o migrar una máquina local, es posible que deba instalar al agente manualmente, como se resume en la tabla.

**máquina virtual** | **Detalles**
--- | ---
** Windows** | 1. [Descargue e instale](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) el archivo MSI del agente.<br/><br/> 2. Realice la instalación con permisos de administrador en el equipo.<br/><br/> 3. Comprobar la instalación. En *C:\WindowsAzure\Packages* en la máquina virtual, haga clic en **WaAppAgent.exe** > **propiedades**. En el **detalles** ficha, **versión del producto** debe ser el valor 2.6.1198.718 o superior.<br/><br/> Si va a actualizar el agente, asegúrese de que no se está ejecutando ninguna operación de copia de seguridad, y [volver a instalar el agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux** | Instalar mediante el uso de un paquete de DEB o RPM del repositorio de paquetes de su distribución. Este es el método preferido para instalar y actualizar al agente Linux de Azure. Todos los [proveedores de distribución aprobada](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) integran el paquete de agente Linux de Azure en sus imágenes y repositorios. El agente está disponible en [GitHub](https://github.com/Azure/WALinuxAgent), pero no se recomienda instalarlo desde allí.<br/><br/> Si va a actualizar al agente, asegúrese de que está ejecutando ninguna operación de copia de seguridad y actualizar los archivos binarios.


### <a name="establish-network-connectivity"></a>Establecimiento de conectividad de red

La extensión de copia de seguridad que se ejecuta en la máquina virtual necesita acceso saliente a direcciones IP públicas de Azure.

Por lo general, no deberá permitir explícitamente el acceso de red saliente para una máquina virtual de Azure para que pueda comunicarse con Azure Backup.
Si no se pueden conectar las máquinas virtuales, y si ve el error **ExtensionSnapshotFailedNoNetwork**, debe permitir expresamente el acceso. La extensión de copia de seguridad, a continuación, puede comunicarse con Azure direcciones IP públicas para el tráfico de copia de seguridad.


#### <a name="explicitly-allow-outbound-access"></a>Permitir explícitamente el acceso de salida

Si una máquina virtual no se puede conectar al servicio de copia de seguridad, debe permitir explícitamente el acceso de salida mediante el uso de uno de los métodos que se resumen en la tabla.

**Opción** | **.** | **Detalles** 
--- | --- | --- 
**Configurar las reglas NSG** | Permita los [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653). | En lugar de permitir y administrar cada intervalo de direcciones, puede agregar una regla de grupo (NSG) de seguridad de red que permita el acceso al servicio Azure Backup mediante el uso de un [etiqueta de servicio](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure). [Más información](../virtual-network/security-overview.md#service-tags).<br/><br/> No hay ningún costo adicional.<br/><br/> Las reglas son fáciles de administrar con las etiquetas de servicio.
**Implementar un servidor proxy** | Implementación de un servidor proxy HTTP para enrutar el tráfico. | Este método proporciona acceso a la totalidad de Azure y no solo al almacenamiento.<br/><br/> Se permite un control detallado de las direcciones URL de almacenamiento.<br/><br/> Hay un único punto de acceso a internet para máquinas virtuales.<br/><br/> Hay costos adicionales para un servidor proxy.
**Configurar el Firewall de Azure** | Permitir el tráfico a través del Firewall de Azure en la máquina virtual, mediante el uso de una etiqueta de nombre de dominio completo para el servicio de copia de seguridad de Azure. |  Este método es fácil de usar si tiene Firewall de Azure configurado en la subred de una red virtual.<br/><br/> No se puede crear sus propias etiquetas FQDN, o modificar el FQDN en una etiqueta.<br/><br/> Si usa Azure Managed Disks, necesitará abrir otro puerto (8443) en los firewalls.

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
         - **Dirección IP de HttpProxy.Host=proxy**
         - **Puerto HttpProxy.Port=proxy**
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

### <a name="allow-firewall-access-by-using-an-fqdn-tag"></a>Permitir el acceso de firewall mediante el uso de una etiqueta de nombre de dominio completo

Puede configurar Firewall de Azure para permitir el acceso de salida para tráfico de red a Azure Backup.

- [Más información](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) acerca de cómo implementar Azure Firewall.
- [Más información acerca de](https://docs.microsoft.com/azure/firewall/fqdn-tags) las etiquetas FQDN.

## <a name="modify-storage-replication-settings"></a>Modificar la configuración de replicación de almacenamiento

De manera predeterminada, los almacenes tienen [almacenamiento con redundancia geográfica (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). Se recomienda GRS para la copia de seguridad principal. Puede usar [almacenamiento con redundancia local (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para una opción más económica.

Modifique el tipo de replicación de almacenamiento como sigue:

1. En el portal, seleccione el nuevo almacén. En **configuración**, seleccione **propiedades**.
2. En **propiedades**, en **configuración de copia de seguridad**, seleccione **actualización**.
3. Seleccione el tipo de replicación de almacenamiento y seleccione **guardar**.

![Establecimiento de la configuración de almacenamiento del nuevo almacén](./media/backup-try-azure-backup-in-10-mins/full-blade.png)


## <a name="configure-a-backup-policy"></a>Configuración de una directiva de copia de seguridad

Detecte las máquinas virtuales de la suscripción y configure la copia de seguridad.

1. En el almacén > **Introducción**, seleccione **+ Backup**.

   ![Botón Backup](./media/backup-azure-arm-vms-prepare/backup-button.png)

   Se abren los paneles **Backup** y **Objetivo de Backup**.

2. En **Objetivo de Backup** > **¿Dónde se ejecuta su carga de trabajo?**, seleccione **Azure**. En **¿De qué quiere realizar una copia de seguridad?**, seleccione **Máquina virtual** >  **Aceptar**.

   ![Paneles Backup y Objetivo de Backup](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   En este paso se registra la extensión de la máquina virtual en el almacén. El panel **Objetivo de Backup** se cierra y se abre el panel **Directiva de copia de seguridad**.

3. En **Directiva de copia de seguridad**, seleccione la directiva que desea asociar al almacén. Después seleccione **Aceptar**.
    - Los detalles de la directiva predeterminada se muestran en el menú desplegable.
    - Seleccione **crear nuevo** para crear una directiva. [Más información](backup-azure-arm-vms-prepare.md#configure-a-backup-policy) acerca de cómo definir una directiva.

    ![Paneles "Backup" y "Directiva de copia de seguridad"](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. En el **seleccionar máquinas virtuales** panel, seleccione las máquinas virtuales que va a usar la directiva de copia de seguridad especificada > **Aceptar**.

   Se valida la máquina virtual seleccionada. Solo se puede seleccionar máquinas virtuales de la región en que se encuentre el almacén. Las copias de seguridad de las máquinas virtuales solo se pueden realizar en un almacén individual.

   ![Panel "Seleccionar máquinas virtuales"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. En **Copia de seguridad**, seleccione **Habilitar copia de seguridad** .

   Este paso implementa la directiva en el almacén y a las máquinas virtuales. También se instala la extensión de copia de seguridad en el agente de máquina virtual que se ejecuta en la máquina virtual de Azure.
   
   Este paso no crea el punto de recuperación inicial de la máquina virtual.

   ![Botón "Habilitar copia de seguridad"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Después de habilitar la copia de seguridad:

- Una copia de seguridad inicial se ejecuta según la programación de copia de seguridad.
- El servicio Backup instala la extensión de copia de seguridad tanto si la máquina virtual está en ejecución como si no lo está.

Una máquina virtual en ejecución ofrece más probabilidad de obtener un punto de recuperación coherente con la aplicación. Sin embargo, se realiza una copia de seguridad de la máquina virtual aunque está desactivada y no se pueda instalar la extensión. Se le conoce como una máquina virtual sin conexión. En este caso, el punto de recuperación será coherente con el bloqueo.
    
> [!NOTE]
> Azure Backup no admite el ajuste automático del reloj para los cambios de horario de verano para realizar copias de seguridad de máquinas virtuales de Azure. Modifique las directivas de copia de seguridad manualmente según sea necesario.

## <a name="run-the-initial-backup"></a>Ejecución de la copia de seguridad inicial

La copia de seguridad inicial se ejecutará según la programación a menos que la ejecute manualmente de forma inmediata. Ejecútela manualmente de la siguiente manera:

1. En el menú del almacén, seleccione **elementos de copia de seguridad**.
2. En **elementos de copia de seguridad**, seleccione **Máquina Virtual de Azure**.
3. En el **elementos de copia de seguridad** lista, seleccione los puntos suspensivos (**...** ).
4. Seleccione **de copia de seguridad ahora**.
5. En **copia de seguridad ahora**, use el control de calendario para seleccionar el último día que se debe conservar el punto de recuperación > **Aceptar**.
6. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **Trabajos de copia de seguridad** > **En curso**. Según el tamaño de la máquina virtual, la creación de la copia de seguridad inicial puede tardar un rato.



## <a name="next-steps"></a>Pasos siguientes

- Solucionar los problemas con [agentes de máquina virtual de Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) o [copia de seguridad de máquina virtual de Azure](backup-azure-vms-troubleshoot.md).
- [Restaurar](backup-azure-arm-restore-vms.md) máquinas virtuales de Azure.

