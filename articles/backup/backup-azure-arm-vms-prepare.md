---
title: Copia de seguridad de VM de Azure en un almacén de Recovery Services con Azure Backup
description: Aquí se describe cómo realizar una copia de seguridad de VM de Azure en un almacén de Recovery Services con Azure Backup
service: backup
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: dacurwin
ms.openlocfilehash: 1f8086580d60d13251052636d4d771855e9605a5
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68954951"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Copia de seguridad de máquinas virtuales de Azure en un almacén de Recovery Services

En este artículo se describe cómo realizar una copia de seguridad de VM de Azure en un almacén de Recovery Services con el servicio [Azure Backup](backup-overview.md).

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Prepare las máquinas virtuales de Azure.
> * Crear un almacén.
> * Detecte VM y configure una directiva de copia de seguridad.
> * Habilite la copia de seguridad de VM de Azure.
> * Realizar la copia de seguridad inicial.


> [!NOTE]
> EN Este artículo describe cómo configurar un almacén y seleccionar VM para hacer una copia de seguridad. Resulta útil si desea realizar copias de seguridad de varias máquinas virtuales. Como alternativa, puede [realizar una copia de seguridad de una VM de Azure](backup-azure-vms-first-look-arm.md) directamente desde la configuración de la VM.

## <a name="before-you-start"></a>Antes de comenzar


- [Revise](backup-architecture.md#architecture-direct-backup-of-azure-vms) la arquitectura de copia de seguridad de máquinas virtuales de Azure.
- [Más información sobre](backup-azure-vms-introduction.md) la copia de seguridad de máquinas virtuales de Azure y la extensión de reserva.
- [Revise la matriz de compatibilidad](backup-support-matrix-iaas.md) antes de configurar la copia de seguridad.

Además, hay un par de cosas que puede que deba hacer en algunas circunstancias:

- **Instalar el agente de máquina virtual en la máquina virtual**: Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina. Si la máquina virtual se creó a partir de una imagen de Azure Marketplace, el agente se instala y se ejecuta. Si crea una máquina virtual personalizada o migra una máquina local, es posible que deba [instalar el agente manualmente](#install-the-vm-agent).
- **Permitir explícitamente el acceso saliente**: por lo general, no es necesario permitir explícitamente el acceso de red saliente para que una máquina virtual de Azure pueda comunicarse con Azure Backup. Sin embargo, algunas máquinas virtuales podrían experimentar problemas de conexión, que muestran el error **ExtensionSnapshotFailedNoNetwork** al intentar conectarse. Si esto sucede, debe [permitir explícitamente el acceso saliente](#explicitly-allow-outbound-access) para que la extensión Azure Backup pueda comunicarse con las direcciones IP públicas de Azure y gestionar el tráfico de copia de seguridad.


## <a name="create-a-vault"></a>Creación de un almacén

 Un almacén almacena las copias de seguridad y los puntos de recuperación creados con el tiempo y almacena las directivas de copia de seguridad asociadas a máquinas de las que se han realizado copias de seguridad. Cree un almacén como se indica a continuación:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. En la búsqueda, escriba **Recovery Services**. En **Servicios**, haga clic en **Almacenes de Recovery Services**.

     ![Busque los almacenes de Recovery Services.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

3. En el menú **Almacenes de Recovery Services**, haga clic en **+Agregar**.

     ![Creación del almacén de Recovery Services, paso 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

4. En el **Almacén de Recovery Services**, escriba un nombre descriptivo para identificar el almacén.
    - El nombre debe ser único para la suscripción de Azure.
    - Puede contener entre 2 y 50 caracteres.
    - Debe comenzar por una letra y solo puede contener letras, números y guiones.
5. Seleccione la suscripción de Azure, el grupo de recursos y la región geográfica en la que se debe crear el almacén. A continuación, haga clic en **Crear**.
    - La creación del almacén puede tardar un tiempo.
    - Supervise las notificaciones del estado en la parte superior derecha del portal.


 Una vez que se crea el almacén, aparece en la lista de almacenes de Recovery Services. Si no lo ve, haga clic en **Actualizar**.

![Lista de copias de seguridad](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

> [!NOTE]
> El servicio Azure Backup crea un grupo de recursos independiente (que no sea el grupo de recursos de VM) para almacenar la instantánea con el formato de nomenclatura **AzureBackupRG_geography_number** (ejemplo: AzureBackupRG_northeurope_1). Los datos de este grupo de recursos se conservarán durante el intervalo de días especificado en la sección *Conservar las instantáneas de recuperación instantánea* de la directiva de copia de seguridad de la máquina virtual de Azure.  Si se aplica un bloqueo a este grupo de recursos, pueden producirse errores de copia de seguridad.<br>
Este grupo de recursos también debe excluirse de todas las restricciones de nombre o etiqueta, ya que una directiva de restricción podría bloquear la creación de colecciones de puntos de recursos en el grupo, lo que provocaría errores de copia de seguridad.


### <a name="modify-storage-replication"></a>Modificar la replicación de almacenamiento

De forma predeterminada, los almacenes usan el [almacenamiento con redundancia geográfica (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).

- Si el almacén es su mecanismo principal de copia de seguridad, le recomendamos que use GRS.
- Puede usar el [almacenamiento con redundancia local (LRS) ](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) si busca una opción más barata.

Modifique el tipo de replicación del almacenamiento tal como se indica a continuación:

1. En el nuevo almacén, haga clic en **Propiedades** en la sección **Configuración**.
2. En **Propiedades**, en **Configuración de copia de seguridad**, haga clic en **Actualizar**.
3. Seleccione el tipo de replicación de almacenamiento y haga clic en **Guardar**.

      ![Establecimiento de la configuración de almacenamiento del nuevo almacén](./media/backup-try-azure-backup-in-10-mins/full-blade.png)
> [!NOTE]
   > No puede modificar el tipo de replicación de almacenamiento después de configurar el almacén y si este contiene elementos de copia de seguridad. Si quiere hacer esto, debe volver a crear el almacén.

## <a name="apply-a-backup-policy"></a>Aplicar una directiva de copia de seguridad

Configurar una directiva de copia de seguridad para el almacén.

1. En el almacén, haga clic en **+ Copia de seguridad** en la sección **Introducción**.

   ![Botón Backup](./media/backup-azure-arm-vms-prepare/backup-button.png)


2. En **Objetivo de copia de seguridad** >  **¿Dónde se ejecuta su carga de trabajo?** , seleccione **Azure**. En **What do you want to back up?** (¿De qué desea realizar una copia de seguridad?), seleccione **Máquina virtual** >  **Aceptar**. Esto registra la extensión de la máquina virtual en el almacén.

   ![Paneles Backup y Objetivo de Backup](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. En **Directiva de copia de seguridad**, seleccione la directiva que desea asociar al almacén.
    - La directiva predeterminada hace una copia de seguridad de la VM una vez al día. Asimismo, las copias de seguridad diarias se conservan durante 30 días. Las instantáneas de recuperación instantánea se conservan durante dos días.
    - Si no quiere usar la directiva predeterminada, seleccione **Crear nueva** y cree una directiva personalizada tal como se describe en el siguiente procedimiento.

      ![Directiva de copia de seguridad predeterminada](./media/backup-azure-arm-vms-prepare/default-policy.png)

4. En **Seleccionar máquinas virtuales**, seleccione las VM de las que quiere hacer una copia de seguridad usando la directiva. A continuación, haga clic en **Aceptar**.

   - Las VM seleccionadas son validadas.
   - Solo se puede seleccionar máquinas virtuales de la región en que se encuentre el almacén.
   - Las copias de seguridad de las máquinas virtuales solo se pueden realizar en un almacén individual.

     ![Panel "Seleccionar máquinas virtuales"](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. En **Copia de seguridad**, haga clic en **Habilitar copia de seguridad**. Esto implementa la directiva en el almacén y las máquinas virtuales, e instala la extensión de copia de seguridad en el agente de máquina virtual que se ejecuta en la máquina virtual de Azure.

     ![Botón "Habilitar copia de seguridad"](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Después de habilitar la copia de seguridad:

- El servicio Backup instala la extensión de copia de seguridad tanto si la máquina virtual está en ejecución como si no lo está.
- Una copia de seguridad inicial se ejecutará según la programación de copia de seguridad.
- Cuando se ejecuten las copias de seguridad, tenga en cuenta que:
    - Una VM que se está ejecutando tiene una mayor oportunidad de capturar un punto de recuperación coherente con la aplicación.
    - Sin embargo, incluso si la VM está desactivada, se hace una copia de seguridad. Esa VM se conoce como una VM sin conexión. En este caso, el punto de recuperación será coherente frente a bloqueos.


### <a name="create-a-custom-policy"></a>Creación de una directiva personalizada

Si creó una nueva directiva de copia de seguridad, complete la configuración de la misma.

1. En **Nombre de directiva**, especifique un nombre descriptivo.
2. En **Programación de copia de seguridad**, especifique cuándo se deben realizar las copias de seguridad. Puede realizar copias de seguridad diarias o semanales para VM de Azure.
2. En **Restauración instantánea**, especifique cuánto tiempo quiere conservar las instantáneas localmente para la restauración instantánea.
    - Cuando realice la restauración, los discos de la VM con copia de seguridad se copian desde el almacén a través de la red, hasta la ubicación de almacenamiento de recuperación. Con la restauración instantánea, puede aprovechar las instantáneas almacenadas localmente y que se tomaron durante un trabajo de copia de seguridad, sin esperar a que los datos de la copia de seguridad se transfieran al almacén.
    - Puede conservar las instantáneas para la restauración instantánea durante uno y cinco días. Dos días es la configuración predeterminada.
3. En el **rango de retención** , especifique cuánto tiempo quiere conservar sus puntos de copia de seguridad diarios o semanales.
4. En **Retención del punto de copia de seguridad mensual**, especifique si quiere mantener una copia de seguridad mensual de sus copias de seguridad diarias o semanales.
5. Haga clic en **Aceptar** para guardar la directiva.

    ![Nueva directiva de copia de seguridad](./media/backup-azure-arm-vms-prepare/new-policy.png)

> [!NOTE]
   > Azure Backup no admite el ajuste automático del reloj para los cambios de horario de verano para realizar copias de seguridad de máquinas virtuales de Azure. A medida que se produzcan cambios horarios, modifique las directivas de copia de seguridad manualmente según sea necesario.

## <a name="trigger-the-initial-backup"></a>Desencadenar la copia de seguridad inicial

La copia de seguridad inicial se ejecutará según la programación, peor puede ejecutarla inmediatamente de la manera siguiente:

1. En el menú Almacén, haga clic en **Elementos de copia de seguridad**.
2. En **Elementos de copia de seguridad**, haga clic en **Máquina virtual de Azure**.
3. En la lista **Elementos de copia de seguridad**, haga clic en el botón de puntos suspensivos (...).
4. Haga clic en **Realizar copia de seguridad ahora**.
5. En **Realizar copia de seguridad ahora**, use el control del calendario para seleccionar el último día que debería retenerse el punto de recuperación. A continuación, haga clic en **Aceptar**.
6. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **Trabajos de copia de seguridad** > **En curso**. Según el tamaño de la máquina virtual, la creación de la copia de seguridad inicial puede tardar un tiempo.

## <a name="verify-backup-job-status"></a>Comprobar el estado del trabajo de copia de seguridad

Los detalles del trabajo de la copia de seguridad de cada una de las máquinas virtuales constan de dos fases: la fase **Instantánea** y, después, la fase **Transferir datos al almacén**.<br/>
La primera de estas fases garantiza la disponibilidad de un punto de recuperación almacenado junto con los discos para realizar **Restauraciones instantáneas** y está disponibles un máximo de cinco días, en función del valor de conservación de instantáneas que haya configurado el usuario. Transferir datos al almacén crea un punto de recuperación en el almacén para la retención a largo plazo. La transferencia de datos al almacén solo comienza una vez que se completa la fase de instantánea.

  ![Estado del trabajo de copia de seguridad](./media/backup-azure-arm-vms-prepare/backup-job-status.png)

Existen dos **tareas secundarias** que se ejecutan en el back-end; una de ellas se usa en el trabajo de copia de seguridad del front-end que se puede comprobar desde la hoja de detalles de la **Tarea de copia de seguridad** como se indica a continuación:

  ![Estado del trabajo de copia de seguridad](./media/backup-azure-arm-vms-prepare/backup-job-phase.png)

La fase de **Transferencia de datos al almacén** puede tardar varios días en completarse, según el tamaño de los discos, la actividad de cada disco y otros factores.

El estado del trabajo puede variar según los siguientes escenarios:

**Instantánea** | **Transferir los datos al almacén** | **Estado del trabajo**
--- | --- | ---
Completed | En curso | En curso
Completed | Skipped | Completed
Completed | Completed | Completed
Completed | Con error | Completado con advertencia
Con error | Con error | Con error


Con esta capacidad y para la misma VM, se pueden ejecutar dos copias de seguridad en paralelo, pero en cualquier fase (instantánea, transferir datos al almacén) solo se puede ejecutar una subtarea. Por lo tanto, si un trabajo de copia de seguridad en curso produce un error en la copia de seguridad del día siguiente, este se evitará con esta funcionalidad de desacoplamiento. Las copias de seguridad del día siguiente pueden tener una instantánea completada mientras que la opción **Transferir datos al almacén** se omite si el trabajo de copia de seguridad de un día anterior está en curso.
El punto de recuperación incremental creado en el almacén capturará toda la rotación desde el último punto de recuperación creado en ese almacén. Esto no costará nada al usuario.


## <a name="optional-steps-install-agentallow-outbound"></a>Pasos opcionales (instalar agente / permitir salida)
### <a name="install-the-vm-agent"></a>Instalar el agente de máquina virtual

Azure Backup realiza una copia de seguridad de máquinas virtuales de Azure instalando una extensión en el agente de máquina virtual de Azure que se ejecuta en la máquina. Si la VM se creó a partir de una imagen de Azure Marketplace, el agente se instala y se ejecuta. Si creó una VM personalizada o migra una máquina local, es posible que deba instalar el agente manualmente.

**VM** | **Detalles**
--- | ---
**Windows** | 1. [Descargue e instale](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) el archivo MSI del agente.<br/><br/> 2. Realice la instalación con permisos de administrador en el equipo.<br/><br/> 3. Compruebe la instalación. En *C:\WindowsAzure\Packages* en la VM, haga clic con el botón derecho en **WaAppAgent.exe** > **Propiedades**. En la pestaña **Detalles**, la **versión del producto** debe ser 2.6.1198.718 o superior.<br/><br/> Si va a actualizar el agente, asegúrese de que no se ejecuta ninguna operación de copia de seguridad y [vuelva a instalar el agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux** | Realice la instalación con un paquete de RPM o DEB del repositorio de paquetes de su distribución. Este es el método preferido para instalar y actualizar el agente Linux de Azure. Todos los [proveedores de distribución aprobada](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) integran el paquete de agente Linux de Azure en sus imágenes y repositorios. El agente está disponible en [GitHub](https://github.com/Azure/WALinuxAgent), pero no se recomienda instalarlo desde allí.<br/><br/> Si va a actualizar el agente, asegúrese de que no se ejecuta ninguna operación de copia de seguridad y actualice los archivos binarios.

### <a name="explicitly-allow-outbound-access"></a>Permitir explícitamente el acceso saliente

Asegúrese de que la extensión de la copia de seguridad que se ejecuta en la VM tiene acceso saliente a direcciones IP públicas de Azure.

- Por lo general, no es necesario permitir explícitamente el acceso de red saliente para que una VM de Azure pueda comunicarse con Azure Backup.
- Si tiene dificultades para conectar las VM o si ve el error **ExtensionSnapshotFailedNoNetwork** cuando intenta conectarse, debe permitir el acceso explícitamente para que la extensión de copia de seguridad pueda comunicarse con las direcciones IP públicas de Azure para el tráfico de copia de seguridad. Los métodos de acceso se resumen en la siguiente tabla.


**Opción** | **Acción** | **Detalles**
--- | --- | ---
**Configuración de reglas de NSG** | Permita los [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653).<br/><br/> En lugar de permitir y administrar cada rango de direcciones, puede agregar una regla que permita el acceso al servicio Azure Backup mediante una [etiqueta de servicio](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure). | [Más información](../virtual-network/security-overview.md#service-tags) sobre las etiquetas de servicio.<br/><br/> Las etiquetas de servicio simplifican la administración del acceso y no tienen costo adicional.
**Implementación de un proxy** | Implementación de un servidor proxy HTTP para enrutar el tráfico. | Proporciona acceso a la totalidad de Azure, no solo al almacenamiento.<br/><br/> Se permite un control detallado de las direcciones URL de almacenamiento.<br/><br/> Punto individual de acceso a Internet para las máquinas virtuales.<br/><br/> Costos adicionales del proxy.
**Configuración de Azure Firewall** | Permita que el tráfico pase por Azure Firewall en la máquina virtual y utilice una etiqueta de nombre de dominio completo para el servicio Azure Backup. | Esto es fácil de usar si Azure Firewall está instalado en una subred de la red virtual.<br/><br/> No puede crear sus propias etiquetas de nombre de dominio completo ni modificar los nombres de dominio completo de una etiqueta.<br/><br/> Si las VM de Azure tienen discos administrados, es posible que deba abrir un puerto adicional (8443) en los firewalls.

#### <a name="establish-network-connectivity"></a>Establecimiento de conectividad de red

Establezca conectividad con NSG, mediante el proxy o a través del firewall.

##### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Configuración de una regla de grupo de seguridad de red para permitir el acceso saliente a Azure

Si un NSG administra el acceso a la VM, permita el acceso de salida para el almacenamiento de copia de seguridad a los rangos y puertos necesarios.

1. En las propiedades de la VM > **Redes**, seleccione **Agregar regla de puerto de salida**.
2. En **Agregar regla de seguridad de salida**, seleccione **Opciones avanzadas**.
3. En **Origen**, seleccione **VirtualNetwork**.
4. En **Rangos de puertos de origen**, escriba un asterisco (*) para permitir el acceso de salida desde cualquier puerto.
5. En **Destino**, seleccione **Etiqueta de servicio**. En la lista, seleccione **Storage.region**. La región es donde se encuentran el almacén y las VM de las que quiere realizar una copia de seguridad.
6. En **Intervalos de puertos de destino** , seleccione el puerto.
    - VM con discos no administrados con una cuenta de almacenamiento sin cifrar: 80
    - VM con discos no administrados con una cuenta de almacenamiento cifrada: 443 (configuración predeterminada)
    - VM con discos administrados: 8443.
7. En **Protocolo**: seleccione **TCP**.
8. En **Prioridad**, especifique un valor de prioridad inferior a las reglas de denegación elevadas.

   Si tiene una regla que deniegue el acceso, la nueva regla que lo permita debe tener un valor mayor. Por ejemplo, si tiene un conjunto de reglas **Deny_All** en prioridad 1000, el valor de la nueva regla deben ser menor de 1000.
9. Especifique el nombre y la descripción de la regla y haga clic en **Aceptar**.

Puede aplicar la regla de NSG a varias máquinas virtuales para permitir el acceso de salida. Este vídeo le guía por el proceso.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]


##### <a name="route-backup-traffic-through-a-proxy"></a>Enrutamiento del tráfico de la copia de seguridad a través de un proxy

El tráfico de la copia de seguridad se puede enrutar a través de un proxy y, después, dé al proxy acceso a los intervalos de Azure necesarios. Configure la VM proxy para permitir lo siguiente:

- La máquina virtual de Azure debe enrutar todo el tráfico HTTP enlazado dirigido a la red pública de Internet a través del proxy.
- El proxy debería permitir el tráfico de entrada desde las VM de la red virtual aplicable.
- El grupo de seguridad de red (NSG) **NSF-lockdown** necesita una regla de seguridad que permita el tráfico de Internet de salida desde la máquina virtual proxy.

###### <a name="set-up-the-proxy"></a>Configuración del proxy

Si no tiene un proxy de la cuenta del sistema, configure uno como se indica a continuación:

1. Descargue [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Ejecute **PsExec.exe -i -s cmd.exe** para ejecutar el símbolo del sistema en una cuenta del sistema.
3. Ejecute el explorador en el contexto del sistema. Por ejemplo, use: **%PROGRAMFILES%\Internet Explorer\iexplore.exe** en el caso de Internet Explorer.  
4. Defina la configuración del proxy.
   - En equipos Linux:
     - Agregue esta línea al archivo **/etc/environment**:
       - **http_proxy=http:\//proxy IP address:proxy port**
     - Agregue estas líneas al archivo **/etc/waagent.conf**:
         - **HttpProxy.Host=proxy IP address**
         - **HttpProxy.Port=proxy port**
   - En los equipos Windows, en la configuración del explorador, especifique que se debe usar un proxy. Si actualmente usa a un proxy en una cuenta de usuario, puede utilizar este script para aplicar el valor en el nivel de cuenta del sistema.
       ```powershell
      $obj = Get-ItemProperty -Path Registry::"HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
      Set-ItemProperty -Path Registry::"HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
      Set-ItemProperty -Path Registry::"HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
      $obj = Get-ItemProperty -Path Registry::"HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
      Set-ItemProperty -Path Registry::"HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
      Set-ItemProperty -Path Registry::"HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

       ```

###### <a name="allow-incoming-connections-on-the-proxy"></a>Aceptación de conexiones de entrada en el proxy

Permita las conexiones de entrada en la configuración del proxy.

1. En el firewall de Windows, abra **Firewall de Windows con seguridad avanzada**.
2. Haga clic con el botón derecho en **Reglas de entrada** > **Nueva regla**.
3. En **Tipo de regla** seleccione **Personalizado** > **Siguiente**.
4. En **Programa**, seleccione **Todos los programas** > **Siguiente**.
5. En **Puertos y protocolos**:
   - Establezca el tipo en **TCP**.
   - Establezca los **puertos locales** en **Puertos específicos**.
   - Establezca el **puerto remoto** en **Todos los puertos**.

6. Finalice el asistente y especifique el nombre de la regla.

###### <a name="add-an-exception-rule-to-the-nsg-for-the-proxy"></a>Adición de una regla de excepción al grupo de seguridad de red para el proxy

En el grupo de seguridad de red **NSF-lockdown**, permita el tráfico desde puerto en la dirección IP 10.0.0.5 a cualquier dirección de Internet en los puertos 80 (HTTP) o 443 (HTTPS).

El siguiente script de PowerShell proporciona un ejemplo en el que se permite el tráfico.
En lugar de permitir el tráfico de salida a todas las direcciones de Internet públicas, puede especificar un intervalo de direcciones IP (`-DestinationPortRange`), o bien usar la etiqueta de servicio storage.region.   

```powershell
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

##### <a name="allow-firewall-access-with-an-fqdn-tag"></a>Permitir el acceso de firewall con una etiqueta de nombre de dominio completo

Azure Firewall se puede configurar para permitir el acceso de salida para el tráfico de red a Azure Backup.

- [Más información](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) acerca de cómo implementar Azure Firewall.
- [Más información acerca de](https://docs.microsoft.com/azure/firewall/fqdn-tags) las etiquetas FQDN.



## <a name="next-steps"></a>Pasos siguientes

- Solucione todos los problemas que se produzcan con los [agentes de VM de Azure](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) o con la [copia de seguridad de VM de Azure](backup-azure-vms-troubleshoot.md).
- [Restaure](backup-azure-arm-restore-vms.md) las VM de Azure.
