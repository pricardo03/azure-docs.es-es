---
title: Preguntas comunes acerca de VMware para la recuperación ante desastres de Azure con Azure Site Recovery | Microsoft Docs
description: Obtenga respuestas a preguntas habituales sobre la recuperación ante desastres de máquinas virtuales de VMware locales en Azure mediante Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 05/30/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: 59be8e0585f0bedcafc868ee42f5113509c9c4ef
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417779"
---
# <a name="common-questions-about-vmware-to-azure-replication"></a>Preguntas comunes acerca de VMware en replicación de Azure

En este artículo se responde preguntas comunes que pueden surgir al implementar la recuperación ante desastres de máquinas virtuales de VMware locales (VM) en Azure.

## <a name="general"></a>General

### <a name="what-do-i-need-for-vmware-vm-disaster-recovery"></a>¿Qué es necesario para la recuperación ante desastres de VM de VMware?

[Obtenga información sobre los componentes que intervienen](vmware-azure-architecture.md) en recuperación ante desastres de máquinas virtuales de VMware.

### <a name="can-i-use-site-recovery-to-migrate-vmware-vms-to-azure"></a>¿Puedo usar Site Recovery para migrar máquinas virtuales de VMware a Azure?

Sí. Además de utilizar Site Recovery para configurar la recuperación ante desastres completa para las máquinas virtuales de VMware, también puede usar Site Recovery para migrar máquinas virtuales de VMware locales a Azure. En este escenario, se replican máquinas virtuales de VMware locales a Azure Storage. Luego, conmuta por error desde el entorno local a Azure. Después de la conmutación por error, las aplicaciones y cargas de trabajo están disponibles y se ejecutan en máquinas virtuales de Azure. El proceso es similar a configurar la recuperación ante desastres completa, salvo que en una migración no se puede conmutar por recuperación desde Azure.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>¿La cuenta de Azure necesita permisos para crear máquinas virtuales?

Si es administrador de una suscripción, ya tiene los permisos de replicación que necesita. Si no es un administrador, necesita permisos para realizar las siguientes acciones:

- Crear una máquina virtual de Azure en el recurso virtual y de grupo de red que se especifique al configurar Site Recovery.
- Escribir en la cuenta de almacenamiento seleccionada o un disco administrado según la configuración.

[Obtenga más información](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) sobre los permisos necesarios.

### <a name="what-applications-can-i-replicate"></a>¿Qué aplicaciones se pueden replicar?

Puede replicar cualquier aplicación o carga de trabajo que se ejecutan en una VM de VMware que cumpla el [los requisitos de replicación](vmware-physical-azure-support-matrix.md#replicated-machines).

- Site Recovery admite replicación compatible con la aplicación, para que las aplicaciones puedan conmutación por error y conmuta por recuperación a un estado inteligente.
- Site Recovery se integra con aplicaciones de Microsoft, como SharePoint, Exchange, Dynamics, SQL Server y Active Directory. También colabora estrechamente con los principales proveedores, como Oracle, SAP, IBM y Red Hat.

[Obtenga más información](site-recovery-workload.md) acerca de la protección de la carga de trabajo.

### <a name="can-i-use-a-guest-os-server-license-on-azure"></a>¿Puedo usar una licencia de servidor de sistema operativo invitado en Azure?

Sí, pueden usar los clientes de Microsoft Software Assurance [ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) para ahorrar en costos para máquinas Windows Server que se migran a Azure de licencias, o usar Azure para recuperación ante desastres.

## <a name="security"></a>Seguridad

### <a name="what-access-to-vmware-servers-does-site-recovery-need"></a>¿Qué acceso a los servidores de VMware necesita Site Recovery?

Site Recovery necesita acceso a los servidores de VMware para:

- Configurar una VM de VMware que ejecuta el servidor de configuración de Site Recovery.
- Detectar automáticamente máquinas virtuales para su replicación.

### <a name="what-access-to-vmware-vms-does-site-recovery-need"></a>¿Qué acceso a máquinas virtuales de VMware necesita Site Recovery?

- Para replicar, una VM de VMware debe tener el servicio Site Recovery Mobility instalada y en ejecución. Puede implementar la herramienta manualmente, o puede especificar que Site Recovery realice una instalación de inserción del servicio cuando se habilita la replicación para una máquina virtual.
- Durante la replicación, las máquinas virtuales se comunican con Site Recovery como se indica a continuación:
    - Las máquinas virtuales se comunican con el servidor de configuración en el puerto HTTPS 443 para la administración de replicación.
    - Las máquinas virtuales envían datos de replicación al servidor de procesos en el puerto HTTPS 9443. (Esta configuración se puede modificar.)
    - Si habilita la coherencia entre varias máquinas virtuales, estas se comunican entre sí a través del puerto 20004.

### <a name="is-replication-data-sent-to-site-recovery"></a>¿Los datos de replicación se envían a Site Recovery?

No, Site Recovery no intercepta los datos replicados y no tiene ninguna información sobre lo que se ejecuta en las máquinas virtuales. Datos de replicación se intercambian entre los hipervisores de VMware y Azure Storage. Site Recovery no tiene capacidad para interceptar los datos. Únicamente se envían los metadatos necesarios para coordinar la replicación y la conmutación por error al servicio Site Recovery.  

Site Recovery está certificado para ISO 27001: 2013 y 27018, HIPAA y DPA. Está en proceso de evaluaciones de SOC2 y FedRAMP JAB.

## <a name="pricing"></a>Precios

### <a name="how-do-i-calculate-approximate-charges-for-vmware-disaster-recovery"></a>¿Cómo se pueden calcular los cargos aproximados de VMware y recuperación ante desastres?

Use la [Calculadora de precios](https://aka.ms/asr_pricing_calculator) para estimar los costos mientras usa Site Recovery.

Para obtener una estimación detallada de los costos, ejecute la herramienta deployment planner para [VMware](https://aka.ms/siterecovery_deployment_planner) y usar el [informe de estimación de costos](https://aka.ms/asr_DP_costreport).

### <a name="is-there-any-difference-in-cost-between-replicating-to-storage-or-directly-to-managed-disks"></a>¿Hay alguna diferencia en el costo entre replicar al almacenamiento o directamente a discos administrados?

Discos administrados se cobran ligeramente diferente de las cuentas de almacenamiento. [Obtenga más información](https://azure.microsoft.com/pricing/details/managed-disks/) sobre los precios de managed Disks.

## <a name="mobility-service"></a>Mobility Service

### <a name="where-can-i-find-the-mobility-service-installers"></a>¿Dónde puedo encontrar los instaladores de Mobility Service?

Los instaladores se encuentran en la carpeta %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository en el servidor de configuración.

## <a name="how-do-i-install-the-mobility-service"></a>¿Cómo instalo Mobility Service?

En cada máquina virtual que desea replicar, instale el servicio de varios métodos:

- [Instalación de inserción](vmware-physical-mobility-service-overview.md#push-installation)
- [Instalación manual](vmware-physical-mobility-service-overview.md#install-mobility-agent-through-ui) desde la interfaz de usuario o PowerShell
- Implementación mediante una herramienta de implementación, como [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md)

## <a name="managed-disks"></a>Discos administrados

### <a name="where-does-site-recovery-replicate-data-to"></a>¿En Site Recovery replicar datos a?

Site Recovery replica servidores físicos y máquinas virtuales de VMware locales a managed disks en Azure.

- El servidor de procesos de Site Recovery escribe los registros de replicación en una cuenta de almacenamiento de caché en la región de destino.
- Estos registros se utilizan para crear puntos de recuperación en discos administrados de Azure que tienen el prefijo de **asrseeddisk**.
- Cuando se produce conmutación por error, el punto de recuperación que seleccione se utiliza para crear un nuevo disco administrado de destino. Este disco administrado está conectado a la máquina virtual en Azure.
- Las máquinas virtuales que anteriormente se han replicado en una cuenta de almacenamiento (antes de marzo de 2019) no se ven afectadas.

### <a name="can-i-replicate-new-machines-to-storage-accounts"></a>¿Puedo replicar nuevas máquinas en cuentas de almacenamiento?

No. A partir de marzo de 2019, en el portal de Azure, puede replicar únicamente a Azure managed disks.

Replicación de nuevas máquinas virtuales a una cuenta de almacenamiento está disponible solo mediante PowerShell o la API de REST (versión 2018-01-10 ó 10-08-2016).

### <a name="what-are-the-benefits-of-replicating-to-managed-disks"></a>¿Cuáles son las ventajas de la replicación en discos administrados?

[Obtenga información sobre cómo](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/) Site Recovery simplifica la recuperación ante desastres con discos administrados.

### <a name="can-i-change-the-managed-disk-type-after-a-machine-is-protected"></a>¿Puedo cambiar el tipo de disco administrado después de proteger un equipo?

Sí, le resultará muy fácil [cambiar el tipo de disco administrado](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage) para replicaciones en curso. Antes de cambiar el tipo, no Asegúrese de que no hay ninguna firma de acceso compartido que se genera una dirección URL en el disco administrado:

1. Vaya a la **disco administrado** recursos en el portal de Azure y compruebe si tiene un banner de dirección URL de firma de acceso compartido el **Introducción** hoja.
1. Si el encabezado está presente, selecciónelo para cancelar la exportación en curso.
1. Cambiar el tipo del disco en los próximos minutos. Si cambia el tipo de disco administrado, espere de puntos de recuperación nuevo para generarse mediante Azure Site Recovery.
1. Use los nuevos puntos de recuperación para cualquier prueba de conmutación por error o conmutación por error en el futuro.

### <a name="can-i-switch-replication-from-managed-disks-to-unmanaged-disks"></a>¿Puedo cambiar la replicación de discos administrados a discos no administrados?

No. Cambio de administrado a no administrado no se admite.

## <a name="replication"></a>Replicación

### <a name="what-are-the-replicated-vm-requirements"></a>¿Cuáles son los requisitos de las máquinas virtuales replicadas?

[Obtenga más información](vmware-physical-azure-support-matrix.md#replicated-machines) sobre los requisitos de soporte técnico para máquinas virtuales de VMware y servidores físicos.

### <a name="how-often-can-i-replicate-to-azure"></a>¿Con qué frecuencia se puede replicar en Azure?

La replicación es continua cuando se replican máquinas virtuales de VMware en Azure.

### <a name="can-i-extend-replication"></a>¿Se puede extender la replicación?

No se admite la replicación extendida o encadenada. Solicite esta característica en el [foro de comentarios](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>¿Se puede realizar una replicación inicial sin conexión?

No se admite la replicación sin conexión. Solicite esta característica en el [foro de comentarios](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="what-is-asrseeddisk"></a>¿Qué es asrseeddisk?

Para cada disco de origen, los datos se replican en un disco administrado en Azure. Este disco tiene el prefijo **asrseeddisk**. Almacena la copia de los discos de origen y todas las instantáneas de punto de recuperación.

### <a name="can-i-exclude-disks-from-replication"></a>¿Se puede excluir discos de la replicación?

Sí, puede excluir discos.

### <a name="can-i-replicate-vms-that-have-dynamic-disks"></a>¿Puedo replicar las máquinas virtuales con discos dinámicos?

Los discos dinámicos se pueden replicar. El disco del sistema operativo debe ser un disco básico.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Si utilizo grupos de replicación para mantener la coherencia con varias máquinas virtuales, ¿puedo agregar una nueva máquina virtual a un grupo de replicación existente?

Sí, puede agregar nuevas máquinas virtuales a un grupo de replicación existente al habilitar la replicación para ellas. Sin embargo:

- No se puede agregar una máquina virtual a un grupo de replicación existente una vez iniciada la replicación.
- No se puede crear un grupo de replicación para máquinas virtuales existentes.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>¿Puedo modificar máquinas virtuales que se replican mediante la adición o el cambio de tamaño de discos?

Para la replicación de VMware en Azure, puede modificar el tamaño del disco. Si desea agregar discos nuevos, debe agregar el disco y volver a habilitar la protección de la máquina virtual.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-server-without-impacting-ongoing-replication"></a>¿Puedo migrar máquinas locales a un nuevo servidor de vCenter sin afectar a la replicación en curso?

No. Un cambio de VMware Vcenter o migración afectará a la replicación en curso. Configurar la recuperación de sitio con el nuevo vCenter Server y habilitar la replicación de máquinas de nuevo.

### <a name="can-i-replicate-to-a-cache-or-target-storage-account-that-has-a-virtual-network-with-azure-firewalls-configured-on-it"></a>¿Puedo replicar en una cuenta de almacenamiento de caché o de destino que tiene una red virtual (con Firewalls de Azure) configurada en él?

No, Site Recovery no admite la replicación en Azure Storage en las redes virtuales.

## <a name="component-upgrade"></a>Actualización del componente

### <a name="my-version-of-the-mobility-services-agent-or-configuration-server-is-old-and-my-upgrade-failed-what-do-i-do"></a>Mi versión de que el servidor de agente o la configuración de servicios de movilidad es antigua y la actualización no se pudo. ¿Qué puedo hacer?

Site Recovery sigue el modelo de soporte técnico de n-4. [Obtenga más información](https://aka.ms/asr_support_statement) sobre cómo actualizar de versiones muy anteriores.

### <a name="where-can-i-find-the-release-notes-and-update-rollups-for-azure-site-recovery"></a>¿Donde se puede encontrar las notas de la versión y paquetes acumulativos de actualizaciones para Azure Site Recovery?

[Obtenga información sobre las nuevas actualizaciones](site-recovery-whats-new.md), y [obtener información de consolidado](service-updates-how-to.md).

### <a name="where-can-i-find-upgrade-information-for-disaster-recovery-to-azure"></a>¿Dónde puedo encontrar información de actualización para la recuperación ante desastres en Azure?

[Obtenga información acerca de cómo actualizar](https://aka.ms/asr_vmware_upgrades).

## <a name="do-i-need-to-reboot-source-machines-for-each-upgrade"></a>¿Es necesario reiniciar las máquinas de origen para cada actualización?

Un reinicio se recomienda pero no es obligatorio para cada actualización. [Más información](https://aka.ms/asr_vmware_upgrades).

## <a name="configuration-server"></a>Servidor de configuración

### <a name="what-does-the-configuration-server-do"></a>¿Qué hace el servidor de configuración?

El servidor de configuración ejecuta los componentes locales de Site Recovery, incluidos:

- El propio servidor de configuración. El servidor coordina las comunicaciones entre los componentes locales y Azure y administra la replicación de datos.
- El servidor de procesos, que actúa como una puerta de enlace de replicación. Este servidor:
    1. Recibe datos de replicación.
    2. Optimiza los datos con el almacenamiento en caché, la compresión y cifrado.
    3. Envía los datos a Azure Storage.
  El servidor de procesos también una instalación de inserción de Mobility Service en máquinas virtuales realiza y la detección automática de máquinas virtuales de VMware locales.
- El servidor de destino maestro, que controla los datos de replicación durante la conmutación por recuperación desde Azure.

[Más información](vmware-azure-architecture.md) acerca de los componentes y procesos del servidor de configuración.

### <a name="where-do-i-set-up-the-configuration-server"></a>¿Dónde configuro el servidor de configuración?

Necesita una máquina virtual de VMware único, de alta disponibilidad local del servidor de configuración. Recuperación ante desastres de servidores físicos, instale al servidor de configuración de una máquina física.

### <a name="what-do-i-need-for-the-configuration-server"></a>¿Qué es necesario para el servidor de configuración?

Revise los [requisitos previos](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>¿Puedo configurar manualmente el servidor de configuración en lugar de usar una plantilla?

Se recomienda [crear la máquina virtual del servidor de configuración](vmware-azure-deploy-configuration-server.md) mediante el uso de la versión más reciente de la plantilla de máquina Virtual Open Format (OVF). Si no se puede usar la plantilla (por ejemplo, si no tiene acceso al servidor de VMware), [descargar](physical-azure-set-up-source.md) el archivo de instalación del portal y configurar el servidor de configuración.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>¿Un servidor de configuración se puede replicar en más de una región?

No. Para replicar en más de una región, necesita un servidor de configuración en cada región.

### <a name="can-i-host-a-configuration-server-in-azure"></a>¿Puedo hospedar un servidor de configuración en Azure?

Aunque es posible, la máquina virtual de Azure que ejecuta el servidor de configuración necesitaría comunicarse con su infraestructura de VMware locales y máquinas virtuales. Esta comunicación agrega latencia y afecta a la replicación en curso.

### <a name="how-do-i-update-the-configuration-server"></a>¿Cómo actualizo el servidor de configuración?

[Obtenga información sobre](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) cómo actualizar el servidor de configuración.

- Puede encontrar la información de actualización más reciente en el [página actualizaciones de Azure](https://azure.microsoft.com/updates/?product=site-recovery).
- Puede descargar la versión más reciente desde el portal. O bien, puede descargar la versión más reciente del servidor de configuración directamente desde el [Microsoft Download Center](https://aka.ms/asrconfigurationserver).
- Si su versión es anterior a la versión actual de más de cuatro versiones, vea el [admite instrucción](https://aka.ms/asr_support_statement) para obtener orientación.

### <a name="should-i-back-up-the-configuration-server"></a>¿Debo hacer copias de seguridad del servidor de configuración?

Se recomienda programar copias de seguridad periódicas del servidor de configuración.

- Para la conmutación por recuperación correcta, la máquina virtual que se ha conmutado debe existir en la base de datos del servidor de configuración.
- El servidor de configuración debe ser en ejecución y en un estado conectado.
- [Obtenga más información](vmware-azure-manage-configuration-server.md) sobre tareas comunes de administración de servidor de configuración.

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Cuando estoy configurando el servidor de configuración, ¿puedo descargar e instalar MySQL manualmente?

Sí. Descargar MySQL y lo coloca en la carpeta C:\Temp\ASRSetup. A continuación, instálelo manualmente. Cuando configure la máquina virtual del servidor de configuración y acepte los términos, MySQL aparecerá como **Ya instalado**en **Descargar e instalar**.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>¿Se puedo evitar descargar MySQL pero dejar que Site Recovery lo instale?

Sí. Descargue al instalador de MySQL y lo coloca en la carpeta C:\Temp\ASRSetup. Al configurar la máquina virtual del servidor de configuración, acepte los términos y seleccione **descargue e instale**. El portal usa el programa de instalación que ha agregado para instalar MySQL.

### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>¿Se puede usar la VM del servidor de configuración para cualquier otra cosa?

No. Use la máquina virtual solo para el servidor de configuración.

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>¿Puedo clonar un servidor de configuración y usarlo para orquestación?

No. Configurar un servidor de configuración actualizados para evitar problemas de registro.

### <a name="can-i-change-the-vault-in-which-the-configuration-server-is-registered"></a>¿Puedo cambiar el almacén en el que está registrado el servidor de configuración?

No. Después de un almacén está asociado con el servidor de configuración, no se puede cambiar. [Obtenga información sobre](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) acerca de cómo registrar un servidor de configuración con un almacén diferente.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>¿Puedo usar el mismo servidor de configuración para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos?

Sí, pero tenga en cuenta esa máquina física puede ser error únicamente en una VM de VMware.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>¿Dónde puedo descargar la frase de contraseña para el servidor de configuración?

[Obtenga información sobre](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) cómo descargar la frase de contraseña.

### <a name="where-can-i-download-vault-registration-keys"></a>¿Dónde puedo descargar las claves de registro del almacén?

En el almacén de Recovery Services, seleccione **servidores de configuración** en **infraestructura de Site Recovery** > **administrar**. A continuación, en **servidores**, seleccione **Descargar clave de registro** para descargar el archivo de credenciales de almacén.

## <a name="process-server"></a>Servidor de proceso

### <a name="why-am-i-unable-to-select-the-process-server-when-i-enable-replication"></a>¿Por qué no puedo seleccionar el servidor de procesos al habilitar la replicación?

Actualizaciones en las versiones 9,24 y posterior muestra ahora el [estado del servidor de procesos al habilitar la replicación](vmware-azure-enable-replication.md#enable-replication). Esta característica ayuda a evitar la limitación del servidor de procesos y a minimizar el uso de servidores de procesos en mal estado.

### <a name="how-do-i-update-the-process-server-to-version-924-or-later-for-accurate-health-information"></a>¿Cómo se puede actualizar el servidor de procesos a la versión 9.24 o posterior para obtener información de estado precisa?

A partir [versión 9,24](service-updates-how-to.md#links-to-currently-supported-update-rollups), se han agregado más alertas para indicar el estado del servidor de procesos. [Actualizar los componentes de Site Recovery a la versión 9.24 o versiones posterior] (service-updates-how-to.md#links-to-currently-supported-update-rollups) para que todas las alertas se generan.

## <a name="failover-and-failback"></a>Conmutación por error y conmutación por recuperación

### <a name="can-i-use-the-on-premises-process-server-for-failback"></a>¿Puedo usar el servidor de procesos local para la conmutación por recuperación?

Se recomienda encarecidamente crear un servidor de procesos en Azure con fines de conmutación por recuperación, para evitar las latencias de transferencia de datos. Además, en caso de que separan la red de máquinas virtuales de origen con la red accesible desde Azure en el servidor de configuración, es esencial para usar el servidor de procesos creado en Azure para la conmutación por recuperación.

### <a name="can-i-keep-the-ip-address-on-failover"></a>¿Puedo conservar la dirección IP de conmutación por error?

Sí, puede conservar la dirección IP de conmutación por error. Asegúrese de especificar la dirección IP de destino en el **proceso y red** configuración de la máquina virtual antes de la conmutación por error. Además, apagar las máquinas en el momento de la conmutación por error para evitar conflictos de direcciones IP durante la conmutación por recuperación.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>¿Puedo cambiar el tamaño de máquina virtual de destino o el tipo de máquina virtual antes de la conmutación por error?

Sí, puede cambiar el tipo o tamaño de la máquina virtual en cualquier momento antes de la conmutación por error. En el portal, use el **proceso y red** configuración para la máquina virtual replicada.

### <a name="how-far-back-can-i-recover"></a>¿Hasta cuánto tiempo atrás puedo recuperar?

Para VMware en Azure, el punto de recuperación más antiguo que puede usar es 72 horas.

### <a name="how-do-i-access-azure-vms-after-failover"></a>¿Cómo accedo a las máquinas virtuales de Azure después de la conmutación por error?

Después de la conmutación por error, puede tener acceso a máquinas virtuales de Azure a través de una conexión segura a internet, a través de una VPN de sitio a sitio o a través de ExpressRoute de Azure. Para conectarse, debe preparar varias cosas. [Más información](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).

### <a name="is-failed-over-data-resilient"></a>¿Es resistente datos conmutada por error?

Azure está diseñado para la resistencia. Site Recovery está diseñado para la conmutación por error a un centro de datos Azure secundaria, según sea necesario por el acuerdo de nivel de servicio (SLA de) Azure. Cuando se produce conmutación por error, nos aseguramos de sus metadatos y los almacenes permanecen en la misma región geográfica que eligió para su almacén.

### <a name="is-failover-automatic"></a>¿La conmutación por error es automática?

La [conmutación por error](site-recovery-failover.md) no es automática. Iniciar una conmutación por error mediante la realización de una sola selección en el portal, o puede usar [PowerShell](/powershell/module/az.recoveryservices) para desencadenar una conmutación por error.

### <a name="can-i-fail-back-to-a-different-location"></a>¿Es posible conmutar por recuperación en otra ubicación?

Sí. Si ha conmutado por error a Azure, puede producir un error a una ubicación diferente si original no está disponible. [Más información](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-with-private-peering-to-fail-back"></a>¿Por qué necesito una VPN o ExpressRoute con emparejamiento privado para conmutar por recuperación?

Cuando se conmuta por recuperación desde Azure, se copian datos desde Azure a la máquina virtual local y se requiere acceso privado.

### <a name="can-i-resize-the-azure-vm-after-failover"></a>¿Puedo cambiar el tamaño de la máquina virtual de Azure después de la conmutación por error?

No, no se puede cambiar el tamaño o tipo de la máquina virtual de destino después de la conmutación por error.

## <a name="automation-and-scripting"></a>Automatización y scripts

### <a name="can-i-set-up-replication-with-scripting"></a>¿Puedo configurar la replicación con scripts?

Sí. Puede automatizar los flujos de trabajo de Site Recovery mediante el uso de la API de Rest, PowerShell o el SDK de Azure. [Más información](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Rendimiento y capacidad

### <a name="can-i-throttle-replication-bandwidth"></a>¿Puedo limitar el ancho de banda de replicación?

Sí. [Más información](site-recovery-plan-capacity-vmware.md).

## <a name="next-steps"></a>Pasos siguientes

- [Revise](vmware-physical-azure-support-matrix.md) los requisitos de compatibilidad.
- [Configure](vmware-azure-tutorial.md) la replicación de VMware en Azure.
