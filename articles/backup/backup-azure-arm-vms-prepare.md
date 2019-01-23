---
title: Preparación para la realización de copias de seguridad de máquinas virtuales de Azure con Azure Backup
description: Describe cómo preparar las máquinas virtuales de Azure para la realización de copias de seguridad con el servicio Azure Backup
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: raynew
ms.openlocfilehash: 65e4c6d66e410e8cd761128028b7a47e21db86eb
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354508"
---
# <a name="prepare-to-back-up-azure-vms"></a>Preparación para copias de seguridad de máquinas virtuales de Azure

En este artículo se describe cómo preparar la realización de una copia de seguridad de una máquina virtual de Azure mediante un almacén de Recovery Services de [Azure Backup](backup-introduction-to-azure-backup.md). La preparación para la realización de una copia de seguridad incluye:


> [!div class="checklist"]
> * Antes de comenzar, examine los escenarios admitidos y las limitaciones.
> * Compruebe los requisitos previos, incluidos los requisitos de la máquina virtual de Azure y la conectividad de ka red.
> * Crear un almacén.
> * Seleccione cómo se replica el almacenamiento.
> * Detecte las máquinas virtuales, configure la directiva y los ajustes de la copia de seguridad.
> * Habilite la copia de seguridad de las máquinas virtuales seleccionadas.


> [!NOTE]
   > En este artículo se describe cómo realizar copias de seguridad de máquinas virtuales de Azure mediante la configuración de un almacén y la selección de las máquinas virtuales de las que se van a realizar copias de seguridad. Resulta útil si desea realizar copias de seguridad de varias máquinas virtuales. También puede hacer una copia de seguridad de una máquina virtual de Azure directamente desde la configuración de la misma. [Más información](backup-azure-vms-first-look-arm.md)

## <a name="before-you-start"></a>Antes de comenzar

1. [Obtenga información general](backup-azure-vms-introduction.md) acerca de Azure Backup para máquinas virtuales de Azure.
2. A continuación encontrará información acerca de la compatibilidad y de las limitaciones.

   **Compatibilidad/limitación** | **Detalles**
   --- | ---
   **SO Windows** | Windows Server 2008 R2 de 64 bits, o cualquier versión posterior.<br/><br/> Cliente de Windows 7 de 64 bits, o cualquier versión posterior.
   **SO Linux** | Puede realizar copias de seguridad de las distribuciones de Linux de 64 bits [compatibles con Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), a excepción de CoreOS Linux.<br/><br/> Examine los sistemas operativos Linux que [admiten la restauración de archivos](backup-azure-restore-files-from-vm.md#for-linux-os).<br/><br/> Otras distribuciones de Linux podrían funcionar, siempre que el agente de máquina virtual esté disponible en la máquina virtual y haya compatibilidad con Python. Sin embargo, estas distribuciones no se admiten.
   **Región** | Se pueden realizar copias de seguridad de las máquinas virtuales de Azure en todas las [regiones admitidas](https://azure.microsoft.com/regions/#services). Si una región no admite, no podrá seleccionarla al crear el almacén.<br/><br/> No es posible realizar copias de seguridad y restauraciones entre regiones de Azure. Estas operaciones solo se pueden realizar dentro de una sola región.
   **Límite de discos de datos** | No se pueden realizar copias de seguridad de máquinas virtuales con más de 16 discos de datos.
   **Almacenamiento compartido** | No es aconsejable realizar copias de seguridad de máquinas virtuales mediante CSV o un servidor de archivos de escalabilidad horizontal. Es probable que los escritores de CSV cometan errores.
   **Cifrado de Linux** | No se admite la copia de seguridad de máquinas virtuales Linux cifradas con LUKS (Linux Unified Key Setup).
   **Coherencia de máquina virtual** | Azure Backup no es compatible con la coherencia entre varias VM.
   **Redes** | Los datos de la copia de seguridad no incluyen unidades montadas de red conectadas a una máquina virtual.<br/><br/>
   **Instantáneas** | No se admite la toma de instantáneas en un disco habilitado para el acelerador de escritura. Bloquea a Azure Backup y no le permite tomar una instantánea coherente con la aplicación de todos los discos de la máquina virtual.
   **PowerShell** | Hay varias acciones que solo están disponibles con PowerShell:<br/><br/> - La restauración de máquinas virtuales administradas por equilibradores de carga internos y externos, o con varias direcciones IP reservadas o adaptadores. [Más información](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations)<br/><br/> -La restauración de una máquina virtual del controlador de dominio en una configuración de controlador de varios dominios. [Más información](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
   **Hora del sistema** | Azure Backup no admite el ajuste automático del reloj para los cambios de horario de verano para realizar copias de seguridad de máquinas virtuales de Azure. Modifique las directivas de copia de seguridad manualmente según sea necesario.
   **Cuentas de almacenamiento** | Si usa una cuenta de almacenamiento restringido de red, asegúrese de que habilita **Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento** para que el servicio Azure Backup pueda acceder a la cuenta. No se admite la recuperación a nivel de elemento para cuentas de almacenamiento con la red restringida.<br/><br/> En una cuenta de almacenamiento, asegúrese de que la configuración de **Firewalls y redes virtuales** permite el acceso desde **todas las redes**.


## <a name="prerequisites"></a>Requisitos previos

- El almacén se debe crear en la misma región que las máquinas virtuales de las que se desean realizar copias de seguridad.
- Antes de empezar, compruebe las regiones de las máquinas virtuales de Azure.
    - Si tiene máquinas virtuales en varias regiones, cree un almacén en cada una de ellas.
    - No es preciso especificar cuentas de almacenamiento para almacenar los datos de la copia de seguridad. Tanto el almacén como el servicio Azure Backup lo controlan automáticamente.
- Compruebe que el agente de VM está instalado en las máquinas virtuales de Azure de las que desea realizar copias de seguridad.

### <a name="install-the-vm-agent"></a>Instalar el agente de máquina virtual

Para habilitar la copia de seguridad, Azure Backup instala una extensión de copia de seguridad (instantánea de máquina virtual o instantánea de máquina virtual Linux) en el agente de máquina virtual que se ejecuta en la máquina virtual de Azure.
    -  El agente de máquina virtual de Azure se instala de forma predeterminada en cualquier máquina virtual de Windows implementada a partir de una imagen de Azure Marketplace. Al implementar una imagen de Azure Marketplace desde el portal, PowerShell, la CLI o una plantilla de Azure Resource Manager, también se instala el agente de máquina virtual de Azure.
    - Si ha migrado una máquina virtual desde un entorno local, el agente no está instalado, y es preciso instalarlo para poder habilitar la copia de seguridad de la máquina virtual.

Si es preciso, instale el agente como se indica a continuación.

**VM** | **Detalles**
--- | ---
**Máquinas virtuales Windows** | [Descargue e instale](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) el agente con permisos de administrador en el equipo.<br/><br/> Para comprobar la instalación, en *C:\WindowsAzure\Packages* en la máquina virtual, haga clic con el botón derecho en la pestaña WaAppAgent.exe > **Propiedades**, > **Detalles**. El valor de **Versión del producto** debe ser 2.6.1198.718 o superior.
**Máquinas virtuales Linux** | La instalación con RPM o un paquete de DEB del repositorio de paquetes de su distribución es el método preferido para instalar y actualizar el agente Linux de Azure. Todos los [proveedores de distribución aprobada](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) integran el paquete de agente Linux de Azure en sus imágenes y repositorios. El agente está disponible en [GitHub](https://github.com/Azure/WALinuxAgent), pero no se recomienda instalarlo desde allí.
Si tiene problemas para realizar una copia de seguridad de la máquina virtual de Azure, use la siguiente tabla para comprobar que el agente de máquina virtual de Azure está instalado correctamente en la máquina virtual. La tabla proporciona información adicional acerca del agente de máquina virtual para las máquinas virtuales Windows y Linux.

### <a name="establish-network-connectivity"></a>Establecimiento de conectividad de red

La extensión de Backup que se ejecuta en la máquina virtual debe tener acceso saliente a direcciones IP públicas de Azure.

> [!NOTE]
> No es necesario ningún acceso de red saliente explícita para que la máquina virtual de Azure se comunique con el servicio Azure Backup. Sin embargo, es posible que algunas máquinas virtuales antiguas se encuentren con problemas y se produzca el error **ExtensionSnapshotFailedNoNetwork**; con el fin de solucionar este error, elija una de las siguientes opciones para permitir que la extensión de copia de seguridad se comunique con las direcciones IP públicas de Azure para proporcionar una ruta de acceso clara para el tráfico de copia de seguridad.

- **Reglas de grupo de seguridad de red**: Permita los [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653). Puede agregar una regla que permita el acceso al servicio Azure Backup mediante un [etiqueta de servicio](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure), en lugar de permitir cada intervalo de direcciones individualmente y administrarlo con el paso del tiempo. Para más información sobre la etiqueta de servicio, consulte este [artículo](../virtual-network/security-overview.md#service-tags).
- **Proxy**: Implementación de un servidor proxy HTTP para enrutar el tráfico.
- **Azure Firewall**: Permita que el tráfico pase por Azure Firewall en la máquina virtual y utilice una etiqueta de nombre de dominio completo para el servicio Azure Backup.

A la hora de decidir una de las opciones, tenga en cuenta los inconvenientes.

**Opción** | **Ventajas** | **Desventajas**
--- | --- | ---
**NSG** | Sin costos adicionales. Fácil de administrar con etiquetas de servicio | Proporciona acceso a la totalidad de Azure, no solo al almacenamiento. |
**Proxy HTTP** | Se permite un control detallado de las direcciones URL de almacenamiento.<br/><br/> Punto individual de acceso a Internet para las máquinas virtuales.<br/><br/> Costos adicionales del proxy.
**Etiquetas de nombre de dominio completo** | Fáciles de usar si Azure Firewall está instalado en una subred de la red virtual | No puede crear sus propias etiquetas de nombre de dominio completo ni modificar los nombres de dominio completo de una etiqueta.

Si usa Azure Managed Disks, necesitará abrir otro puerto (8443) en los firewalls.

### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Configuración de una regla de grupo de seguridad de red para permitir el acceso saliente a Azure

Si el acceso a la máquina virtual de Azure lo administra un grupo de seguridad de red, permita el acceso de salida para el almacenamiento de las copias de seguridad a los puertos e intervalos necesarios.

1. En la máquina virtual > **Redes**, haga clic en **Agregar regla de puerto de salida**.

  - Si tiene una regla que deniegue el acceso, la nueva regla que lo permita debe ser mayor. Por ejemplo, si tiene un conjunto de reglas **Deny_All** en prioridad 1000, el valor de la nueva regla deben ser menor de 1000.
2. En **Agregar regla de seguridad de salida**, haga clic en **Avanzado**.
3. En **Origen**, seleccione **VirtualNetwork**.
4. En **Intervalos de puertos de origen** , escriba un asterisco (*) para permitir el acceso de salida desde cualquier puerto.
5. En **Destino**, seleccione **Etiqueta de servicio**. En la lista, seleccione Almacenamiento.<region>. La región es la región en la que se encuentran el almacén y las máquinas virtuales de las que desea realizar copias de seguridad.
6. En **Intervalos de puertos de destino** , seleccione el puerto.

    - Máquina virtual con discos no administrados y una cuenta de almacenamiento sin cifrar: 80
    - Máquina virtual con discos no administrados y una cuenta de almacenamiento cifrada: 443 (configuración predeterminada)
    - Máquina virtual administrada: 8443.
7. En **Protocolo**: seleccione **TCP**.
8. En **Prioridad**, asígnele un valor de prioridad inferior a las reglas de denegación elevadas.
9. Especifique el nombre y la descripción de la regla y haga clic en **Aceptar**.

Puede aplicar la regla de NSG a varias máquinas virtuales para permitir el acceso de salida a Azure para Azure Backup.

Este vídeo le guía por el proceso.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]

> [!WARNING]
> Las etiquetas de servicio de almacenamiento se encuentran en versión preliminar. Solo están disponibles en determinadas regiones. Para ver la lista de regiones, consulte el apartado [Etiquetas de servicio para almacenamiento](../virtual-network/security-overview.md#service-tags).

### <a name="route-backup-traffic-through-a-proxy"></a>Enrutamiento del tráfico de la copia de seguridad a través de un proxy

El tráfico de la copia de seguridad se puede enrutar a través de un proxy y, después, dé al proxy acceso a los intervalos de Azure necesarios.
Debería configurar la máquina virtual proxy para permitir lo siguiente:

- La máquina virtual de Azure debe enrutar todo el tráfico HTTP enlazado dirigido a la red pública de Internet a través del proxy.
- El proxy debería permitir el tráfico de entrada desde las máquinas virtuales de la red virtual (VNet) aplicable.
- El grupo de seguridad de red (NSG) **NSF-lockdown** necesita una regla de seguridad que permita el tráfico de Internet de salida desde la máquina virtual proxy.

Así es cómo se debe configurar el proxy. Usamos los valores del ejemplo. Debería reemplazarlos por los suyos propios.

#### <a name="set-up-a-system-account-proxy"></a>Configuración de un proxy de la cuenta del sistema
Si no tiene un proxy de la cuenta del sistema, configure uno como se indica a continuación:

1. Descargue [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Ejecute **PsExec.exe -i -s cmd.exe** para ejecutar el símbolo del sistema en una cuenta del sistema.
3. Ejecute el explorador en el contexto del sistema. Por ejemplo:  **PROGRAMFILES%\Internet Explorer\iexplore.exe** en el caso de Internet Explorer.  
4. Defina la configuración del proxy.
    - En equipos Linux:
        - Agregue esta línea al archivo **/etc/environment**:
            - **http_proxy=http://proxy IP address:proxy port**
        - Agregue estas líneas al archivo **/etc/waagent.conf**:
            - **HttpProxy.Host=proxy IP address**
            - **HttpProxy.Port=proxy port**
    - En los equipos Windows, en la configuración del explorador, especifique que se debe usar un proxy. Si actualmente usa a un proxy en una cuenta de usuario, puede utilizar este script para aplicar el valor en el nivel de cuenta del sistema.
        ```
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

        ```

#### <a name="allow-incoming-connections-on-the-proxy"></a>Aceptación de conexiones de entrada en el proxy

Permita las conexiones de entrada en la configuración del proxy.

- Por ejemplo, abra **Firewall de Windows con seguridad avanzada**.
    - Haga clic con el botón derecho en **Reglas de entrada** > **Nueva regla**.
    - En **Tipo de regla** seleccione **Personalizado** > **Siguiente**.
    - En **Programa**, seleccione **Todos los programas** > **Siguiente**.
    - En **Protocolos y puertos** establezca el tipo en **TCP**, en **Puertos locales** elija **Puertos específicos** y en **Puerto remoto** elija **Todos los puertos**.
    - Finalice el asistente y especifique el nombre de la regla.

#### <a name="add-an-exception-rule-to-the-nsg"></a>Adición de una regla de excepción al grupo de seguridad de red

En el grupo de seguridad de red **NSF-lockdown**, permita el tráfico desde puerto en la dirección IP 10.0.0.5 a cualquier dirección de Internet en los puertos 80 (HTTP) o 443 (HTTPS).

- El siguiente script de PowerShell proporciona un ejemplo en el que se permite el tráfico.
- En lugar de permitir el tráfico de salida a todas las direcciones de Internet públicas, puede especificar un intervalo de direcciones IP (-DestinationPortRange), o bien usar la etiqueta de servicio storage.region.   

    ```
    Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
    Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
    ```

### <a name="allow-firewall-access-with-fqdn-tag"></a>Permitir el acceso de firewall con la etiqueta FQDN

Azure Firewall se puede configurar para permitir el acceso de salida para el tráfico de red a Azure Backup.

- [Más información](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) acerca de cómo implementar Azure Firewall.
- [Más información acerca de](https://docs.microsoft.com/azure/firewall/fqdn-tags) las etiquetas FQDN.

## <a name="create-a-vault"></a>Creación de un almacén

Un almacén de Recovery Services para Backup almacena las copias de seguridad y los puntos de recuperación creados con el tiempo y almacena las directivas de copia de seguridad asociadas a máquinas de las que se han realizado copias de seguridad. Cree un almacén como se indica a continuación:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. En el menú de la **izquierda**, seleccione **Más servicios**y escriba **Recovery Services**. En cuanto empiece a escribirlo, la entrada filtra la lista de recursos. Seleccione **Almacenes de Recovery Services**.

    ![Escribir en el cuadro y seleccionar "Almacenes de Recovery Services" en los resultados](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Aparece la lista de almacenes de Recovery Services.
3. En el menú **Almacenes de Recovery Services**, seleccione **Agregar**.

    ![Creación del almacén de Recovery Services, paso 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    Se abre el panel **Almacenes de Recovery Services**. En él se le solicita que rellene los campos **Nombre**, **Suscripción**, **Grupo de recursos** y **Ubicación**.

    ![Panel "Almacenes de Recovery Services"](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. En **Nombre**, escriba un nombre descriptivo para identificar el almacén.
    - El nombre debe ser único para la suscripción de Azure.
    - Puede contener entre 2 y 50 caracteres.
    - Debe comenzar por una letra y solo puede contener letras, números y guiones.
5. Seleccione **Suscripción** para ver la lista de suscripciones disponibles. Si no está seguro de la suscripción que va a utilizar, use la predeterminada (o sugerida). Solo hay varias opciones si la cuenta profesional o educativa está asociada a varias suscripciones de Azure.
6. Seleccione **Grupo de recursos** para ver la lista de grupos de recursos disponibles, o bien seleccione **Nuevo** para crear uno. [Más información](../azure-resource-manager/resource-group-overview.md) acerca de los grupos de recursos.
7. Haga clic en **Ubicación** para seleccionar la región geográfica del almacén. El almacén *debe* estar en la misma región que las máquinas virtuales de las que desea realizar copias de seguridad.
8. Seleccione **Crear**.
    - La creación del almacén puede tardar un tiempo.
    - Supervise las notificaciones del estado en la parte superior derecha del portal.
    ![Lista de almacenes de copia de seguridad](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Tras crear el almacén, este ya aparece en la lista de almacenes de Recovery Services. Si no lo ve, haga clic en **Actualizar**.

## <a name="set-up-storage-replication"></a>Configuración de la replicación del almacenamiento

De manera predeterminada, los almacenes tienen [almacenamiento con redundancia geográfica (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). Es aconsejable usar almacenamiento con redundancia geográfica para la copia de seguridad principal, pero también se puede usar [almacenamiento localmente redundante](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json), que es una opción más barata.

Modifique la replicación del almacenamiento como se indica a continuación:

1. En el almacén > **Infraestructura de Backup**, haga clic en **Configuración de copia de seguridad** 

   ![Lista de copias de seguridad](./media/backup-azure-arm-vms-prepare/full-blade.png)

2. En **Configuración de copia de seguridad**, modifique el método de redundancia de almacenamiento como se necesario y seleccione **Guardar**.


## <a name="configure-backup"></a>Configuración de la copia de seguridad

Detecte las máquinas virtuales de la suscripción y configure la copia de seguridad.

1. En el almacén > **Información general**, haga clic en **+ Copia de seguridad**

   ![Botón Backup](./media/backup-azure-arm-vms-prepare/backup-button.png)

   Se abren los paneles **Backup** y **Objetivo de Backup**.

2. En **Objetivo de Backup**> **¿Dónde se ejecuta su carga de trabajo?**, seleccione **Azure**. En **¿De qué quiere realizar una copia de seguridad?**, seleccione **Máquina virtual** >  **Aceptar**. Esto registra la extensión de la máquina virtual en el almacén.

   ![Paneles Backup y Objetivo de Backup](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   En este paso se registra la extensión de la máquina virtual en el almacén. El panel **Objetivo de Backup** se cierra y se abre el panel **Directiva de copia de seguridad**.

3. En **Directiva de copia de seguridad**, seleccione la directiva que desea asociar al almacén. A continuación, haga clic en **Aceptar**.
    - Los detalles de la directiva predeterminada se muestran en el menú desplegable.
    - Para crear una directiva, seleccione **Crear nueva**. [Más información](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) acerca de cómo definir una directiva.

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

- La directiva de copia de seguridad se ejecuta según la programación de copia de seguridad.
- El servicio Backup instala la extensión de copia de seguridad tanto si la máquina virtual está en ejecución como si no lo está.
    - Una máquina virtual en ejecución ofrece más probabilidad de obtener un punto de recuperación coherente con la aplicación.
    -  Sin embargo, se realiza una copia de seguridad de la máquina virtual aunque está desactivada y no se pueda instalar la extensión. Esto se conoce como *máquina virtual sin conexión*. En este caso, el punto de recuperación será *coherente frente a bloqueos*.
- Si desea generar una copia de seguridad a petición para la máquina virtual de inmediato, en **Elementos de copia de seguridad** , haga clic en el botón de puntos suspensivos (...) situado junto a la máquina virtual > **Realizar copia de seguridad ahora**.


## <a name="next-steps"></a>Pasos siguientes

- Solucione todos los problemas que se producen con los [agentes de máquina virtual de Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) o la [copia de seguridad de máquina virtual de Azure](backup-azure-vms-troubleshoot.md).
- [Copia de seguridad de máquinas virtuales de Azure](backup-azure-vms-first-look-arm.md)
