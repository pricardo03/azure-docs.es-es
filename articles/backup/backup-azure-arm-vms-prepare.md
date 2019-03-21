---
title: Copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services con Azure Backup
description: Describe cómo realizar una copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services con Azure Backup
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: raynew
ms.openlocfilehash: c6d6e380cded18a089f624f90d998477a89293be
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259048"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services

En este artículo se describe cómo realizar una copia de seguridad de máquinas virtuales de Azure mediante [Azure Backup](backup-overview.md) implementando y habilitando la copia de seguridad en un almacén de Recovery Services.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Comprobar los escenarios y requisitos previos admitidos.
> * Preparar las máquinas virtuales de Azure. Instalar el agente de máquina virtual de Azure si es necesario y comprobar el acceso de salida para las máquinas virtuales.
> * Crear un almacén.
> * Configuración del almacenamiento para el almacén
> * Detecte las máquinas virtuales, configure la directiva y los ajustes de la copia de seguridad.
> * Habilitación de la copia de seguridad de máquinas virtuales de Azure


> [!NOTE]
   > En este artículo se describe cómo realizar copias de seguridad de máquinas virtuales de Azure mediante la configuración de un almacén y la selección de las máquinas virtuales de las que se van a realizar copias de seguridad. Resulta útil si desea realizar copias de seguridad de varias máquinas virtuales. También puede [hacer una copia de seguridad de una máquina virtual de Azure](backup-azure-vms-first-look-arm.md) directamente desde la configuración de la misma.

## <a name="before-you-start"></a>Antes de comenzar

Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina.

1. [Revise](backup-architecture.md#architecture-direct-backup-of-azure-vms) la arquitectura de copia de seguridad de máquinas virtuales de Azure.
[Más información sobre](backup-azure-vms-introduction.md) la copia de seguridad de máquinas virtuales de Azure y la extensión de reserva.
2. [Revise la matriz de compatibilidad](backup-support-matrix-iaas.md) para la copia de seguridad de máquinas virtuales de Azure.
3. Prepare las máquinas virtuales de Azure. Instale el agente de máquina virtual si no está instalado y compruebe el acceso de salida para las máquinas virtuales de las que desee realizar una copia de seguridad.


## <a name="prepare-azure-vms"></a>Preparación de las máquinas virtuales de Azure

Instale el agente de máquina virtual si es necesario y compruebe el acceso de salida desde las máquinas virtuales.

### <a name="install-the-vm-agent"></a>Instalar el agente de máquina virtual
Si es preciso, instale el agente como se indica a continuación.

**VM** | **Detalles**
--- | ---
**Máquinas virtuales Windows** | [Descargue e instale](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) el archivo MSI del agente. Realice la instalación con permisos de administrador en el equipo.<br/><br/> Para comprobar la instalación, en *C:\WindowsAzure\Packages* en la máquina virtual, haga clic con el botón derecho en la pestaña WaAppAgent.exe > **Propiedades**, > **Detalles**. El valor de **Versión del producto** debe ser 2.6.1198.718 o superior.<br/><br/> Si va a actualizar el agente, asegúrese de que no se ejecuta ninguna operación de copia de seguridad y [vuelva a instalar el agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Máquinas virtuales Linux** | La instalación con RPM o un paquete de DEB del repositorio de paquetes de su distribución es el método preferido para instalar y actualizar el agente Linux de Azure. Todos los [proveedores de distribución aprobada](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) integran el paquete de agente Linux de Azure en sus imágenes y repositorios. El agente está disponible en [GitHub](https://github.com/Azure/WALinuxAgent), pero no se recomienda instalarlo desde allí.<br/><br/> Si va a actualizar el agente, asegúrese de que no se ejecuta ninguna operación de copia de seguridad y actualice los archivos binarios.


### <a name="establish-network-connectivity"></a>Establecimiento de conectividad de red

La extensión de Backup que se ejecuta en la máquina virtual debe tener acceso saliente a direcciones IP públicas de Azure.

- No es necesario ningún acceso de red saliente explícita para que la máquina virtual de Azure se comunique con el servicio Azure Backup.
- Sin embargo, determinadas máquinas virtuales más antiguas pueden tener problemas y fallar con el error **ExtensionSnapshotFailedNoNetwork** al intentar conectarse. En este caso, use una de las opciones siguientes para que la extensión de reserva pueda comunicarse con direcciones IP públicas de Azure para el tráfico de copia de seguridad.

   **Opción** | **Acción** | **Ventajas** | **Desventajas**
   --- | --- | --- | ---
   **Configuración de reglas de NSG** | Permita los [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653).<br/><br/>  Puede agregar una regla que permita el acceso al servicio Azure Backup mediante una [etiqueta de servicio](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure), en lugar de permitir y administrar cada intervalo de direcciones individualmente. [Más información](../virtual-network/security-overview.md#service-tags) sobre las etiquetas de servicio. | Sin costos adicionales. Fácil de administrar con etiquetas de servicio
   **Implementación de un proxy** | Implementación de un servidor proxy HTTP para enrutar el tráfico. | Proporciona acceso a la totalidad de Azure, no solo al almacenamiento. Se permite un control detallado de las direcciones URL de almacenamiento.<br/><br/> Punto individual de acceso a Internet para las máquinas virtuales.<br/><br/> Costos adicionales del proxy.<br/><br/>
   **Configuración de Azure Firewall** | Permita que el tráfico atraviese Azure Firewall en la máquina virtual y utilice una etiqueta de nombre de dominio completo para el servicio Azure Backup.|  Fáciles de usar si Azure Firewall está instalado en una subred de la red virtual | No puede crear sus propias etiquetas de nombre de dominio completo ni modificar los nombres de dominio completo de una etiqueta.<br/><br/> Si usa Azure Managed Disks, necesitará abrir otro puerto (8443) en los firewalls.

#### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Configuración de una regla de grupo de seguridad de red para permitir el acceso saliente a Azure

Si el acceso a la máquina virtual de Azure lo administra un grupo de seguridad de red, permita el acceso de salida para el almacenamiento de las copias de seguridad a los puertos e intervalos necesarios.

1. En la máquina virtual > **Redes**, haga clic en **Agregar regla de puerto de salida**.
2. En **Agregar regla de seguridad de salida**, haga clic en **Avanzado**.
3. En **Origen**, seleccione **VirtualNetwork**.
4. En **Intervalos de puertos de origen** , escriba un asterisco (*) para permitir el acceso de salida desde cualquier puerto.
5. En **Destino**, seleccione **Etiqueta de servicio**. En la lista, seleccione **Storage.region**. La región es la región en la que se encuentran el almacén y las máquinas virtuales de las que desea realizar copias de seguridad.
6. En **Intervalos de puertos de destino** , seleccione el puerto.
    - Máquina virtual no administrada con una cuenta de almacenamiento sin cifrar: 80
    - Máquina virtual no administrada con una cuenta de almacenamiento cifrada: 443 (configuración predeterminada)
    - Máquina virtual administrada: 8443.
7. En **Protocolo**: seleccione **TCP**.
8. En **Prioridad**, especifique un valor de prioridad inferior a las reglas de denegación elevadas. Si tiene una regla que deniegue el acceso, la nueva regla que lo permita debe ser mayor. Por ejemplo, si tiene un conjunto de reglas **Deny_All** en prioridad 1000, el valor de la nueva regla deben ser menor de 1000.
9. Especifique el nombre y la descripción de la regla y haga clic en **Aceptar**.

Puede aplicar la regla de NSG a varias máquinas virtuales para permitir el acceso de salida.

Este vídeo le guía por el proceso.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]


#### <a name="route-backup-traffic-through-a-proxy"></a>Enrutamiento del tráfico de la copia de seguridad a través de un proxy

El tráfico de la copia de seguridad se puede enrutar a través de un proxy y, después, dé al proxy acceso a los intervalos de Azure necesarios.
Debería configurar la máquina virtual proxy para permitir lo siguiente:

- La máquina virtual de Azure debe enrutar todo el tráfico HTTP enlazado dirigido a la red pública de Internet a través del proxy.
- El proxy debería permitir el tráfico de entrada desde las máquinas virtuales de la red virtual (VNet) aplicable.
- El grupo de seguridad de red (NSG) **NSF-lockdown** necesita una regla de seguridad que permita el tráfico de Internet de salida desde la máquina virtual proxy.

##### <a name="set-up-the-proxy"></a>Configuración del proxy
Si no tiene un proxy de la cuenta del sistema, configure uno como se indica a continuación:

1. Descargue [PsExec](https://technet.microsoft.com/sysinternals/bb897553).

2. Ejecute **PsExec.exe -i -s cmd.exe** para ejecutar el símbolo del sistema en una cuenta del sistema.
3. Ejecute el explorador en el contexto del sistema. Por ejemplo: **%PROGRAMFILES%\Internet Explorer\iexplore.exe** en el caso de Internet Explorer.  
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

##### <a name="allow-incoming-connections-on-the-proxy"></a>Aceptación de conexiones de entrada en el proxy

Permita las conexiones de entrada en la configuración del proxy.

- Por ejemplo, abra **Firewall de Windows con seguridad avanzada**.
    - Haga clic con el botón derecho en **Reglas de entrada** > **Nueva regla**.
    - En **Tipo de regla** seleccione **Personalizado** > **Siguiente**.
    - En **Programa**, seleccione **Todos los programas** > **Siguiente**.
    - En **Protocolos y puertos** establezca el tipo en **TCP**, en **Puertos locales** elija **Puertos específicos** y en **Puerto remoto** elija **Todos los puertos**.
    - Finalice el asistente y especifique el nombre de la regla.

##### <a name="add-an-exception-rule-to-the-nsg-for-the-proxy"></a>Adición de una regla de excepción al grupo de seguridad de red para el proxy

En el grupo de seguridad de red **NSF-lockdown**, permita el tráfico desde puerto en la dirección IP 10.0.0.5 a cualquier dirección de Internet en los puertos 80 (HTTP) o 443 (HTTPS).

- El siguiente script de PowerShell proporciona un ejemplo en el que se permite el tráfico.
- En lugar de permitir el tráfico de salida a todas las direcciones de Internet públicas, puede especificar un intervalo de direcciones IP (-DestinationPortRange), o bien usar la etiqueta de servicio storage.region.   

    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
    Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
    ```

### <a name="allow-firewall-access-with-fqdn-tag"></a>Permitir el acceso de firewall con la etiqueta FQDN

Azure Firewall se puede configurar para permitir el acceso de salida para el tráfico de red a Azure Backup.

- [Más información](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) acerca de cómo implementar Azure Firewall.
- [Más información acerca de](https://docs.microsoft.com/azure/firewall/fqdn-tags) las etiquetas FQDN.

## <a name="set-up-storage-replication"></a>Configuración de la replicación del almacenamiento

De manera predeterminada, los almacenes tienen [almacenamiento con redundancia geográfica (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). Es aconsejable usar almacenamiento con redundancia geográfica para la copia de seguridad principal, pero también se puede usar [almacenamiento localmente redundante](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), que es una opción más barata.

Copia de seguridad de Azure controla automáticamente el almacenamiento para el almacén. Debe especificar cómo se replica los que el almacenamiento.
Modifique la replicación del almacenamiento como se indica a continuación:

1. En la hoja **Almacenes de Recovery Services**, haga clic en el almacén nuevo. En el **configuración** sección, haga clic en **propiedades**.
2. En **propiedades**, en **configuración de copia de seguridad**, haga clic en **actualización**.

3. Seleccione el tipo de replicación de almacenamiento y haga clic en **guardar**.

      ![Establecimiento de la configuración de almacenamiento del nuevo almacén](./media/backup-try-azure-backup-in-10-mins/full-blade.png)


## <a name="configure-a-backup-policy"></a>Configuración de una directiva de copia de seguridad

Detecte las máquinas virtuales de la suscripción y configure la copia de seguridad.

1. En el almacén > **Información general**, haga clic en **+ Copia de seguridad**

   ![Botón Backup](./media/backup-azure-arm-vms-prepare/backup-button.png)

   Se abren los paneles **Backup** y **Objetivo de Backup**.

2. En **Objetivo de Backup**> **¿Dónde se ejecuta su carga de trabajo?**, seleccione **Azure**. En **¿De qué quiere realizar una copia de seguridad?**, seleccione **Máquina virtual** >  **Aceptar**. Esto registra la extensión de la máquina virtual en el almacén.

   ![Paneles Backup y Objetivo de Backup](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   En este paso se registra la extensión de la máquina virtual en el almacén. El panel **Objetivo de Backup** se cierra y se abre el panel **Directiva de copia de seguridad**.

3. En **Directiva de copia de seguridad**, seleccione la directiva que desea asociar al almacén. A continuación, haga clic en **Aceptar**.
    - Los detalles de la directiva predeterminada se muestran en el menú desplegable.
    - Para crear una directiva, seleccione **Crear nueva**. [Más información](backup-azure-arm-vms-prepare.md#configure-a-backup-policy) acerca de cómo definir una directiva.

      ![Paneles "Backup" y "Directiva de copia de seguridad"](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. En el panel **Seleccionar máquinas virtuales**, seleccione las máquinas virtuales que va a usar la directiva de copia de seguridad especificada > **Aceptar**.

   - Se valida la máquina virtual seleccionada.
   - Solo se puede seleccionar máquinas virtuales de la región en que se encuentre el almacén. Las copias de seguridad de las máquinas virtuales solo se pueden realizar en un almacén individual.

     ![Panel "Seleccionar máquinas virtuales"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. En **Copia de seguridad**, seleccione **Habilitar copia de seguridad** .

   - Esto implementa la directiva en el almacén y las máquinas virtuales, e instala la extensión de copia de seguridad en el agente de máquina virtual que se ejecuta en la máquina virtual de Azure.
   - Este paso no crea el punto de recuperación inicial de la máquina virtual.

     ![Botón "Habilitar copia de seguridad"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Después de habilitar la copia de seguridad:

- Una copia de seguridad inicial se ejecuta según la programación de copia de seguridad.
- El servicio Backup instala la extensión de copia de seguridad tanto si la máquina virtual está en ejecución como si no lo está.
    - Una máquina virtual en ejecución ofrece más probabilidad de obtener un punto de recuperación coherente con la aplicación.
    -  Sin embargo, se realiza una copia de seguridad de la máquina virtual aunque está desactivada y no se pueda instalar la extensión. Esto se conoce como *máquina virtual sin conexión*. En este caso, el punto de recuperación será *coherente frente a bloqueos*.
    Tenga en cuenta que Azure Backup no admite el ajuste automático del reloj para los cambios de horario de verano para realizar copias de seguridad de máquinas virtuales de Azure. Modifique las directivas de copia de seguridad manualmente según sea necesario.

## <a name="run-the-initial-backup"></a>Ejecución de la copia de seguridad inicial

La copia de seguridad inicial se ejecutará según la programación a menos que la ejecute manualmente de forma inmediata. Ejecútela manualmente de la siguiente manera:

1. En el menú Almacén, haga clic en **Elementos de copia de seguridad**.
2. En **Elementos de copia de seguridad**, haga clic en **Máquina virtual de Azure**.
3. En la lista **Elementos de copia de seguridad**, haga clic en el botón de puntos suspensivos **...**.
4. Haga clic en **Realizar copia de seguridad ahora**.
5. En **Realizar copia de seguridad ahora**, use el control del calendario para seleccionar el último día que debería retenerse el punto de recuperación >  **Aceptar**.
6. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **Trabajos de copia de seguridad** > **En curso**. Según el tamaño de la máquina virtual, la creación de la copia de seguridad inicial puede tardar un tiempo.



## <a name="next-steps"></a>Pasos siguientes

- Solucione todos los problemas que se producen con los [agentes de máquina virtual de Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) o la [copia de seguridad de máquina virtual de Azure](backup-azure-vms-troubleshoot.md).
- [Copia de seguridad de máquinas virtuales de Azure](backup-azure-vms-first-look-arm.md)
