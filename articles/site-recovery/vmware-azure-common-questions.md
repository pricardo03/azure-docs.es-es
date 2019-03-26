---
title: 'Preguntas comunes: recuperación ante desastres de VMware a Azure con Azure Site Recovery | Microsoft Docs'
description: En este artículo se resumen las preguntas comunes al configurar la recuperación ante desastres de máquinas virtuales de VMware locales en Azure mediante Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
services: site-recovery
ms.date: 03/21/2019
ms.topic: conceptual
ms.author: raynew
ms.openlocfilehash: cdb8fe5deb71c014f7e0af01d070e5004d8c9994
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418801"
---
# <a name="common-questions---vmware-to-azure-replication"></a>Preguntas frecuentes: replicación de VMware en Azure

En este artículo se proporcionan respuestas a preguntas comunes que podemos observar al implementar la recuperación ante desastres de máquinas virtuales de VMware locales en Azure. Si tiene alguna pregunta después de leer el artículo, publíquela en el [Foro de Azure Recovery Services](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr).


## <a name="general"></a>General
### <a name="how-is-site-recovery-priced"></a>¿Cómo se fija el precio de Site Recovery?
Consulte los detalles de los [precios de Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).

### <a name="how-do-i-pay-for-azure-vms"></a>¿Cómo se pagan las máquinas virtuales de Azure?
Durante la replicación, los datos se replican en Azure Storage y no se paga ningún cambio en las máquinas virtuales. Cuando se ejecuta una conmutación por error en Azure, Site Recovery crea automáticamente máquinas virtuales IaaS de Azure. Después de eso, se le facturan los recursos de proceso que consume en Azure.

### <a name="what-can-i-do-with-vmware-to-azure-replication"></a>¿Qué puedo hacer con la replicación de VMware en Azure?
- **Recuperación ante desastres**: Puede configurar la recuperación ante desastres completa. En este escenario, replica máquinas virtuales de VMware locales en Azure Storage. Luego, si la infraestructura local no está disponible, puede conmutar por error en Azure. Cuando conmuta por error, las máquinas virtuales de Azure se crean con los datos replicados. Puede acceder a aplicaciones y cargas de trabajo en las máquinas virtuales de Azure hasta que el centro de datos local vuelva a estar disponible. Luego, puede conmutar por recuperación desde Azure al sitio local.
- **Migración**: Puede usar Site Recovery para migrar VM de VMware locales a Azure. En este escenario, replica máquinas virtuales de VMware locales en Azure Storage. Luego, conmuta por error desde el entorno local a Azure. Después de la conmutación por error, las aplicaciones y cargas de trabajo están disponibles y se ejecutan en máquinas virtuales de Azure.

## <a name="azure"></a>Azure
### <a name="what-do-i-need-in-azure"></a>¿Qué necesito tener en Azure?
Necesita una suscripción de Azure, un almacén de Recovery Services, una cuenta de almacenamiento en caché, una red virtual y discos administrados. El almacén, la cuenta de almacenamiento en caché, discos administrado y la red debe estar en la misma región.

### <a name="does-my-azure-account-need-permissions-to-create-vms"></a>¿La cuenta de Azure necesita permisos para crear máquinas virtuales?
Si es administrador de una suscripción, ya tiene los permisos de replicación que necesita. Si no es así, necesita permisos para crear una máquina virtual de Azure en el grupo de recursos y especifique al configurar Site Recovery y los permisos para escribir en la cuenta de almacenamiento seleccionada o administradas basadas en disco en la configuración de red virtual. [Más información](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines).

### <a name="can-i-use-guest-os-server-license-on-azure"></a>¿Es posible usar la licencia de servidor de SO invitado en Azure?
Sí, los clientes de Microsoft Software Assurance pueden usar la [Ventaja híbrida de Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) para ahorrar en los costos de licencia de **máquinas Windows Server** que se migran a Azure o usar Azure para la recuperación ante desastres.

## <a name="pricing"></a>Precios

### <a name="how-are-licensing-charges-handled-during-replication-after-failover"></a>¿Cómo se controlan los cargos de licencia durante la replicación después de la conmutación por error?

Consulte [aquí](https://aka.ms/asr_pricing_FAQ) nuestras preguntas frecuentes sobre licencias para más información.

### <a name="how-can-i-calculate-approximate-charges-during-the-use-of-site-recovery"></a>¿Cómo puedo calcular los cargos aproximados durante el uso de Site Recovery?

Puede usar la [calculadora de precios](https://aka.ms/asr_pricing_calculator) para calcular los costos al usar Azure Site Recovery. Para la estimación detallada en los costos, ejecute la herramienta deployment planner (https://aka.ms/siterecovery_deployment_planner) y analizar el [informe de estimación de costos](https://aka.ms/asr_DP_costreport).

### <a name="is-there-any-difference-in-cost-when-i-replicate-directly-to-managed-disk"></a>¿Hay alguna diferencia en el costo al replicar directamente en el disco administrado?

Discos administrados se cobran ligeramente diferentes de las cuentas de almacenamiento. Consulte el ejemplo siguiente de un disco de origen de 100 GB de tamaño. El ejemplo es diferencial costo de almacenamiento específico. Este costo no incluye el costo de las instantáneas, almacenamiento en caché y las transacciones.

* Cuenta de almacenamiento estándar de Vs. Disco administrado de unidad de disco duro estándar

    - **Disco de almacenamiento aprovisionado, Azure Site Recovery**: S10
    - **Cuenta de almacenamiento estándar que se cobran por consume volumen**: 5 USD al mes
    - **Disco administrado estándar que se cobran por volumen aprovisionado**: 5.89 $ al mes

* Cuenta de Premium storage Vs. Disco administrado de SSD Premium 
    - **Disco de almacenamiento aprovisionado, Azure Site Recovery**: P10
    - **Cuenta de almacenamiento Premium se cobra por volumen aprovisionado**: 17.92 $ al mes
    - **Disco administrado Premium cobran por volumen aprovisionado**: 17.92 $ al mes

Obtenga más información en [información detallada de precios de discos administrados](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="do-i-incur-additional-charges-for-cache-storage-account-with-managed-disks"></a>¿Hay cargos adicionales para la cuenta de almacenamiento en caché con discos administrados?

No, no incurre en cargos adicionales por la memoria caché. Memoria caché siempre forma parte de VMware a la arquitectura de Azure. Cuando se replica en la cuenta de almacenamiento estándar, este almacenamiento en caché es parte de la misma cuenta de almacenamiento de destino.

### <a name="i-have-been-an-azure-site-recovery-user-for-over-a-month-do-i-still-get-the-first-31-days-free-for-every-protected-instance"></a>He sido usuario de Azure Site Recovery durante más de un mes. ¿Los treinta y un primeros días serán gratuitos para cada instancia protegida?

Sí, no importa el tiempo que haya estado usando Azure Site Recovery. Ninguna instancia protegida incurre en cargos por Azure Site Recovery durante los primeros 31 días. Por ejemplo, si ha tenido protegidas diez instancias durante los últimos seis meses y conecta una undécima instancia a Azure Site Recovery, esta undécima instancia no incurrirá en cargos por Azure Site Recovery durante los treinta y un primeros días. Las 10 primeras instancias continúan incurriendo en cargos por Azure Site Recovery porque han estado protegidas durante más de 31 días.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>Durante los primeros 31 días, ¿puedo incurrir en otros cargos de Azure?

Sí, aunque Azure Site Recovery sea gratuito los primeros 31 días de una instancia protegida, puede incurrir en cargos por Azure Storage, transacciones de almacenamiento y transferencia de datos. Una máquina virtual recuperada también puede incurrir en cargos por proceso de Azure.

### <a name="what-charges-do-i-incur-while-using-azure-site-recovery"></a>¿En qué cargos se incurre al utilizar Azure Site Recovery?

Consulte nuestras [preguntas frecuentes sobre los costos incurridos](https://aka.ms/asr_pricing_FAQ) para más información.

### <a name="is-there-a-cost-associated-to-perform-dr-drillstest-failover"></a>¿Hay un costo asociado a exploraciones de recuperación ente desastres y pruebas de conmutación por error?

No hay ningún costo independiente para exploraciones de recuperación ante desastres. Habrá cargos de proceso una vez creada la máquina virtual después de la conmutación por error de prueba.

## <a name="azure-site-recovery-components-upgrade"></a>Actualización de componentes de Azure Site Recovery

### <a name="my-mobility-agentconfiguration-serverprocess-server-version-is-very-old-and-my-upgrade-has-failed-how-should-i-upgrade-to-latest-version"></a>La versión de mi agente de movilidad/servidor de configuración/servidor de procesos es muy antigua y no he podido actualizar. ¿Cómo debo actualizar a la versión más reciente?

Azure Site Recovery sigue el modelo de soporte técnico de N-4. Consulte nuestra [instrucción de compatibilidad](https://aka.ms/asr_support_statement) para entender los detalles sobre cómo actualizar de versiones muy antiguas.

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-azure-site-recovery"></a>¿Dónde puedo encontrar las notas de la versión o los paquetes acumulativos de actualización de Azure Site Recovery?

Consulte el [documento](https://aka.ms/asr_update_rollups) para información sobre las notas de la versión. Puede encontrar vínculos de instalación de los componentes correspondientes en cada paquete acumulativo de actualizaciones.

### <a name="how-should-i-upgrade-site-recovery-components-for-on-premises-vmware-or-physical-site-to-azure"></a>¿Cómo debo actualizar los componentes de Site Recovery de VMware local o un sitio físico a Azure?

Consulte nuestra guía [aquí](https://aka.ms/asr_vmware_upgrades) para actualizar los componentes.

## <a name="is-reboot-of-source-machine-mandatory-for-each-upgrade"></a>¿Es obligatorio reiniciar la máquina de origen para cada actualización?

Aunque se recomienda, no es obligatorio para cada actualización. Consulte [aquí](https://aka.ms/asr_vmware_upgrades) para instrucciones claras.

## <a name="on-premises"></a>Local

### <a name="what-do-i-need-on-premises"></a>¿Qué necesito tener en el entorno local?

En un entorno local, es preciso tener:
- Los componentes de Site Recovery, instalados en una sola máquina virtual de VMware.
- Una infraestructura de VMware con al menos un host ESXi; además, se recomienda que tenga un servidor vCenter.
- Una o varias máquinas virtuales de VMware para realizar la replicación.

[Más información](vmware-azure-architecture.md) sobre la arquitectura de VMware en Azure.

El servidor de configuración local se puede implementar de la siguiente manera:

- Se recomienda que implemente el servidor de configuración como una máquina virtual de VMware mediante una plantilla de OVA con el servidor de configuración preinstalado.
- Si por algún motivo no puede usar una plantilla, puede configurar el servidor de configuración manualmente. [Más información](physical-azure-disaster-recovery.md#set-up-the-source-environment).



### <a name="where-do-on-premises-vms-replicate-to"></a>¿A dónde se replican las máquinas virtuales locales?
Los datos se replican a Azure Storage. Al ejecutar una conmutación por error, Site Recovery crea máquinas virtuales de Azure desde la cuenta de almacenamiento o según la configuración de disco administrado de automáticamente.

## <a name="replication"></a>Replicación

### <a name="what-applications-can-i-replicate"></a>¿Qué aplicaciones se pueden replicar?
Puede replicar cualquier aplicación o carga de trabajo en ejecución en una máquina virtual de VMware que cumpla con los [requisitos de replicación](vmware-physical-azure-support-matrix.md##replicated-machines). Site Recovery proporciona compatibilidad para la replicación compatible con aplicaciones, para que estas se puedan conmutar por error y por recuperación a un estado inteligente. Site Recovery se integra con aplicaciones de Microsoft como SharePoint, Exchange, Dynamics, SQL Server y Active Directory; además, colabora estrechamente con los principales proveedores, como Oracle, SAP, IBM y Red Hat. [Obtenga más información](site-recovery-workload.md) acerca de la protección de la carga de trabajo.

### <a name="can-i-protect-a-virtual-machine-that-has-docker-disk-configuration"></a>¿Puedo proteger una máquina virtual con configuración de disco Docker?

No, se trata de un escenario no admitido.

### <a name="can-i-replicate-to-azure-with-a-site-to-site-vpn"></a>¿Puedo replicar en Azure con una VPN de sitio a sitio?
Site Recovery replica los datos desde el entorno local a una instancia de Azure Storage a través de un punto de conexión público o mediante el uso del emparejamiento público de ExpressRoute. No se admite la replicación a través de una red VPN de sitio a sitio.

### <a name="can-i-replicate-to-azure-with-expressroute"></a>¿Puedo replicar en Azure con ExpressRoute?
Sí, puede usar ExpressRoute para replicar máquinas virtuales en Azure. Site Recovery replica los datos en Azure Storage a través de un punto de conexión público. Es necesario configurar el [emparejamiento público](../expressroute/expressroute-circuit-peerings.md#publicpeering) o el [emparejamiento de Microsoft](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) si se va a usar ExpressRoute para la replicación de Site Recovery. El emparejamiento de Microsoft es el dominio de enrutamiento recomendado para la replicación. Asegúrese de que los [requisitos de red](vmware-azure-configuration-server-requirements.md#network-requirements) también se cumplen en la replicación. Una vez que las máquinas virtuales conmutan por error en una red virtual de Azure, puede acceder a ellas a través del [emparejamiento privado](../expressroute/expressroute-circuit-peerings.md#privatepeering).

### <a name="how-can-i-change-storage-account-after-machine-is-protected"></a>¿Cómo se puede cambiar la cuenta de almacenamiento después de proteger la máquina?

Debe deshabilitar y habilitar la replicación actualizar o cambiar el tipo de cuenta de almacenamiento.

### <a name="can-i-replicate-to-storage-accounts-for-new-machine"></a>¿Puedo replicar en cuentas de almacenamiento para la nueva máquina?

No, a partir de ' 19 de marzo, puede replicar a discos administrados en Azure desde el portal. Replicación en cuentas de almacenamiento para una nueva máquina solo está disponible a través de API de REST y Powershell. Use la API versión 2016-08-10 o 2018-01-10 para la replicación en cuentas de almacenamiento.

### <a name="what-are-the-benefits-in-replicating-to-managed-disks"></a>¿Cuáles son las ventajas en la replicación de discos administrados?

Lea el artículo sobre cómo [Azure Site Recovery simplifica la recuperación ante desastres con discos administrados](https://azure.microsoft.com/blog/simplify-disaster-recovery-with-managed-disks-for-vmware-and-physical-servers/).

### <a name="how-can-i-change-managed-disk-type-after-machine-is-protected"></a>¿Cómo se puede cambiar el tipo de disco administrado después de proteger el equipo?

Sí, puede cambiar fácilmente el tipo de disco administrado. [Más información](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage). Sin embargo, una vez que cambie el tipo de disco administrado, asegúrese de que esperar para que puntos de recuperación nuevo que se generará si necesita probar conmutación por error o conmutación por error se registra esta actividad.

### <a name="can-i-switch-the-replication-from-managed-disks-to-unmanaged-disks"></a>¿Puedo cambiar la replicación de discos administrados a discos no administrados?

No, cambia de administrado a no administrado no se admite.

### <a name="why-cant-i-replicate-over-vpn"></a>¿Por qué no puedo replicar a través de VPN?

Cuando se replica en Azure, el tráfico de replicación alcanza los extremos públicos de Azure Storage, por tanto, solo puede replicar a través de internet con ExpressRoute (emparejamiento público) y VPN no funciona.

### <a name="can-i-use-riverbed-steelheads-for-replication"></a>¿Puedo usar Riverbed SteelHeads para la replicación?

Nuestro socio, Riverbed, proporciona instrucciones detalladas sobre cómo trabajar con Azure Site Recovery. Consulte sus [Guía de solución](https://community.riverbed.com/s/article/DOC-4627).

### <a name="what-are-the-replicated-vm-requirements"></a>¿Cuáles son los requisitos de las máquinas virtuales replicadas?

Para realizar la replicación, una máquina virtual de VMware debe ejecutar un sistema operativo compatible. Además, la máquina virtual debe cumplir con los requisitos de las máquinas virtuales de Azure. [Más información](vmware-physical-azure-support-matrix.md##replicated-machines) en la matriz de compatibilidad.

### <a name="how-often-can-i-replicate-to-azure"></a>¿Con qué frecuencia se puede replicar en Azure?
La replicación es continua cuando se replican máquinas virtuales de VMware en Azure.

### <a name="can-i-retain-the-ip-address-on-failover"></a>¿Se puede conservar la dirección IP en la conmutación por error?
Sí, se puede conservar la dirección IP en la conmutación por error. Asegúrese de mencionar la dirección IP de destino en la hoja "Proceso y red" antes de la conmutación por error. Además, asegúrese de apagar las máquinas en el momento de la conmutación por error para evitar conflictos de dirección IP en el momento de la conmutación por recuperación.

### <a name="can-i-extend-replication"></a>¿Se puede extender la replicación?
No se admite la replicación extendida o encadenada. Solicite esta característica en el [foro de comentarios](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-initial-replication"></a>¿Se puede realizar una replicación inicial sin conexión?
No es una opción admitida. Solicite esta característica en el [foro de comentarios](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-disks"></a>¿Se pueden excluir discos?
Sí, puede excluir discos de la replicación.

### <a name="can-i-change-the-target-vm-size-or-vm-type-before-failover"></a>¿Puedo cambiar el tamaño de máquina virtual de destino o el tipo de máquina virtual antes de la conmutación por error?
Sí, puede cambiar el tipo o tamaño de la máquina virtual cualquier momento antes de la conmutación por error, vaya a proceso y red de la configuración del elemento de la replicación desde el portal.

### <a name="can-i-replicate-vms-with-dynamic-disks"></a>¿Se pueden replicar máquinas virtuales con discos dinámicos?
Los discos dinámicos se pueden replicar. El disco del sistema operativo debe ser un disco básico.

### <a name="if-i-use-replication-groups-for-multi-vm-consistency-can-i-add-a-new-vm-to-an-existing-replication-group"></a>Si utilizo grupos de replicación para mantener la coherencia con varias máquinas virtuales, ¿puedo agregar una nueva máquina virtual a un grupo de replicación existente?
Sí, puede agregar nuevas máquinas virtuales a un grupo de replicación existente al habilitar la replicación para ellas. No se puede agregar una máquina virtual a un grupo de replicación existente después de que se inicia la replicación y tampoco se puede crear un grupo de replicación para máquinas virtuales existentes.

### <a name="can-i-modify-vms-that-are-replicating-by-adding-or-resizing-disks"></a>¿Puedo modificar máquinas virtuales que se replican mediante la adición o el cambio de tamaño de discos?

Para la replicación de VMware en Azure, puede modificar el tamaño del disco. Si desea agregar discos nuevos, debe agregar el disco y volver a habilitar la protección de la máquina virtual.

### <a name="can-i-migrate-on-premises-machines-to-a-new-vcenter-without-impacting-ongoing-replication"></a>¿Puedo migrar en los equipos locales a un nuevo Vcenter sin afectar a la replicación en curso?
No, el cambio de instancia de Vcenter o migración afectará a la replicación en curso. Debe configurar Azure Site Recovery con el nuevo Vcenter y habilitar la replicación de máquinas.

### <a name="can-i-replicate-to-cachetarget-storage-account-which-has-a-vnet-with-azure-storage-firewalls-configured-on-it"></a>¿Puedo replicar a una cuenta de almacenamiento en caché o destino que tenga una red virtual (con firewalls de Azure Storage) configurada?
No, Azure Site Recovery no admite la replicación ni en el almacenamiento ni en la red virtual.

## <a name="configuration-server"></a>Servidor de configuración

### <a name="what-does-the-configuration-server-do"></a>¿Qué hace el servidor de configuración?
El servidor de configuración ejecuta los componentes locales de Site Recovery, incluidos:
- El servidor de configuración que coordina las comunicaciones entre el entorno local y Azure, además de administrar la replicación de datos.
- El servidor de procesos que actúa como puerta de enlace de replicación. Recibe los datos de la replicación, los optimiza mediante almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage. El servidor de procesos también instala Mobility Service en las máquinas virtuales que desea replicar y realiza la detección automática de las máquinas virtuales de VMware locales.
- El servidor de destino maestro que controla los datos de replicación durante la conmutación por recuperación desde Azure.

[Más información](vmware-azure-architecture.md) acerca de los componentes y procesos del servidor de configuración.

### <a name="where-do-i-set-up-the-configuration-server"></a>¿Dónde configuro el servidor de configuración?
Necesita una sola máquina virtual de VMware local de alta disponibilidad para el servidor de configuración.

### <a name="what-are-the-requirements-for-the-configuration-server"></a>¿Cuáles son los requisitos para el servidor de configuración?

Revise los [requisitos previos](vmware-azure-deploy-configuration-server.md#prerequisites).

### <a name="can-i-manually-set-up-the-configuration-server-instead-of-using-a-template"></a>¿Puedo configurar manualmente el servidor de configuración en lugar de usar una plantilla?
Se recomienda usar la versión más reciente de la plantilla de OVF para [crear la máquina virtual del servidor de configuración](vmware-azure-deploy-configuration-server.md). Si por algún motivo no puede hacerlo, por ejemplo, si no tiene acceso al servidor de VMware, puede [descargar el archivo de instalación unificada](physical-azure-set-up-source.md) del portal y ejecutarlo en una máquina virtual.

### <a name="can-a-configuration-server-replicate-to-more-than-one-region"></a>¿Un servidor de configuración se puede replicar en más de una región?
 No. Para hacerlo, necesita configurar un servidor de configuración en cada región.

### <a name="can-i-host-a-configuration-server-in-azure"></a>¿Puedo hospedar un servidor de configuración en Azure?
Si bien es posible, la máquina virtual de Azure que ejecuta el servidor de configuración necesitaría comunicarse con las máquinas virtuales y la infraestructura de VMware locales. Esto puede agregar latencias y afectar a la replicación en curso.

### <a name="how-do-i-update-the-configuration-server"></a>¿Cómo actualizo el servidor de configuración?
[Más información](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server) acerca de la actualización de los valores de configuración. Puede encontrar la información sobre las actualizaciones más recientes en la [página de actualizaciones de Azure](https://azure.microsoft.com/updates/?product=site-recovery). También puede descargar directamente la versión más reciente del servidor de configuración desde el [Centro de descarga de Microsoft](https://aka.ms/asrconfigurationserver). Si su versión es anterior a 4 versiones de la versión actual, consulte nuestra [instrucción de compatibilidad](https://aka.ms/asr_support_statement) para orientación sobre la actualización.

### <a name="should-i-backup-the-deployed-configuration-server"></a>¿Debo realizar copias de seguridad del servidor de configuración implementado?
Se recomienda programar copias de seguridad periódicas del servidor de configuración. Para obtener una conmutación por recuperación correcta, la máquina virtual que se ha conmutado debe existir en la base de datos del servidor de configuración, y este último debe estar en ejecución y conectado. Puede obtener más información sobre las tareas comunes de administración del servidor de configuración [aquí](vmware-azure-manage-configuration-server.md).

### <a name="when-im-setting-up-the-configuration-server-can-i-download-and-install-mysql-manually"></a>Cuando estoy configurando el servidor de configuración, ¿puedo descargar e instalar MySQL manualmente?

Sí. Descargue MySQL y colóquelo en la carpeta **C:\Temp\ASRSetup**. Después, instálelo manualmente. Cuando configure la máquina virtual del servidor de configuración y acepte los términos, MySQL aparecerá como **Ya instalado**en **Descargar e instalar**.

### <a name="can-i-avoid-downloading-mysql-but-let-site-recovery-install-it"></a>¿Se puedo evitar descargar MySQL pero dejar que Site Recovery lo instale?

Sí. Descargue el instalador de MySQL y colóquelo en la carpeta **C:\Temp\ASRSetup**.  Al configurar la máquina virtual del servidor de configuración, acepte los términos y haga clic en **Descargar e instalar**; el portal usará el instalador que ha agregado para instalar MySQL.
 
### <a name="can-i-use-the-configuration-server-vm-for-anything-else"></a>¿Se puede usar la VM del servidor de configuración para cualquier otra cosa?
No, solo se debe usar la máquina virtual para el servidor de configuración. 

### <a name="can-i-clone-a-configuration-server-and-use-it-for-orchestration"></a>¿Puedo clonar un servidor de configuración y usarlo para orquestación?
No, debe configurar un nuevo servidor de configuración para evitar problemas de registro.

### <a name="can-i-change-the-vault-registered-in-the-configuration-server"></a>¿Se puede cambiar el almacén registrado en el servidor de configuración?
 No. Una vez que un almacén está registrado con el servidor de configuración, no se puede cambiar. Revise [este artículo](vmware-azure-manage-configuration-server.md#register-a-configuration-server-with-a-different-vault) para conocer los pasos para un nuevo registro.

### <a name="can-i-use-the-same-configuration-server-for-disaster-recovery-of-both-vmware-vms-and-physical-servers"></a>¿Se puede usar el mismo servidor de configuración para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos?
Sí, pero tenga en cuenta que solo se puede realizar la conmutación por recuperación de una máquina física a una máquina virtual de VMware.

### <a name="where-can-i-download-the-passphrase-for-the-configuration-server"></a>¿Dónde puedo descargar la frase de contraseña para el servidor de configuración?
[Revise este artículo](vmware-azure-manage-configuration-server.md#generate-configuration-server-passphrase) para obtener información sobre cómo descargar la frase de contraseña.

### <a name="where-can-i-download-vault-registration-keys"></a>¿Dónde puedo descargar las claves de registro del almacén?

En **Almacén de Recovery Services**,**Administrar** > **Infraestructura de Site Recovery** > **Servidores de configuración**. En **Servers** (Servidores), seleccione **Download registration key** (Descargar clave de registro) para descargar el archivo de credenciales de almacén.



## <a name="mobility-service"></a>Mobility Service

### <a name="where-can-i-find-the-mobility-service-installers"></a>¿Dónde puedo encontrar los instaladores de Mobility Service?
Los instaladores se encuentran en la carpeta **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository** del servidor de configuración.

## <a name="how-do-i-install-the-mobility-service"></a>¿Cómo instalo Mobility Service?
Si desea instalarlo en cada máquina virtual que desea replicar, use una [instalación de inserción](vmware-physical-mobility-service-overview.md#push-installation) o una [instalación manual](vmware-physical-mobility-service-overview.md#install-mobility-agent-through-ui) desde la interfaz de usuario o PowerShell. También puede usar una herramienta de implementación, como [System Center Configuration Manager](vmware-azure-mobility-install-configuration-mgr.md).



## <a name="security"></a>Seguridad

### <a name="what-access-does-site-recovery-need-to-vmware-servers"></a>¿Qué acceso a los servidores de VMware necesita Site Recovery?
Site Recovery necesita acceso a los servidores de VMware para:

- Configurar una máquina virtual de VMware que ejecute el servidor de configuración y otros componentes locales de Site Recovery. [Más información](vmware-azure-deploy-configuration-server.md)
- Detectar automáticamente máquinas virtuales para su replicación. Más información sobre cómo preparar una cuenta para la detección automática. [Más información](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery)


### <a name="what-access-does-site-recovery-need-to-vmware-vms"></a>¿Qué acceso a las máquinas virtuales de VMware necesita Site Recovery?

- Para poder realizar la replicación, una máquina virtual de VMware debe tener Site Recovery Mobility Service instalado y en ejecución. Puede implementar manualmente la herramienta o especificar que Site Recovery debe realizar una instalación de inserción del servicio cuando habilite la replicación de una máquina virtual. Para realizar la instalación de inserción, Site Recovery necesita una cuenta que pueda usar para instalar el componente del servicio. [Más información](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation). El servidor de proceso (que se ejecuta de manera predeterminada en el servidor de configuración) realiza esta instalación. [Más información](vmware-azure-install-mobility-service.md) sobre la instalación de Mobility Service.
- Durante la replicación, las máquinas virtuales se comunican con Site Recovery como se indica a continuación:
    - Las máquinas virtuales se comunican con el servidor de configuración en el puerto HTTPS 443 para la administración de la replicación.
    - Las máquinas virtuales envían los datos de replicación al servidor de procesos en el puerto HTTPS 9443 (se puede modificar).
    - Si habilita la coherencia entre varias máquinas virtuales, estas se comunican entre sí a través del puerto 20004.


### <a name="is-replication-data-sent-to-site-recovery"></a>¿Los datos de replicación se envían a Site Recovery?
No, Site Recovery no intercepta los datos replicados ni tiene información sobre lo que se ejecuta en las máquinas virtuales. Los datos de replicación se intercambian entre los hipervisores de VMware y Azure Storage. Site Recovery no tiene capacidad para interceptar los datos. Únicamente se envían los metadatos necesarios para coordinar la replicación y la conmutación por error al servicio Site Recovery.  

Site Recovery está certificado según la norma ISO 27001:2013, 27018, además de HIPAA y DPA, y está completando sus evaluaciones de SOC2 y FedRAMP JAB.

### <a name="can-we-keep-on-premises-metadata-within-a-geographic-regions"></a>¿Podemos conservar los metadatos locales dentro de las regiones geográficas?
Sí. Cuando crea un almacén en una región, nos aseguramos de que todos los datos que Site Recovery usa permanezcan dentro del límite geográfico de esa región.

### <a name="does-site-recovery-encrypt-replication"></a>¿Site Recovery cifra la replicación?
Sí, se admite tanto el cifrado en tránsito como el [cifrado en Azure](https://docs.microsoft.com/azure/storage/storage-service-encryption).


## <a name="failover-and-failback"></a>Conmutación por error y conmutación por recuperación
### <a name="can-i-use-the-process-server-at-on-premises-for-failback"></a>¿Puedo usar el servidor de procesos en un entorno local para la conmutación por recuperación?
Se recomienda crear un servidor de procesos en Azure con fines de conmutación por recuperación para evitar las latencias de transferencia de datos. Además, en caso de que separan la red de máquinas virtuales de origen con la red accesible desde Azure al servidor de configuración, es esencial para usar el servidor de procesos creada en Azure para la conmutación por recuperación.

### <a name="how-far-back-can-i-recover"></a>¿Hasta cuánto tiempo atrás puedo recuperar?
Para VMware en Azure, el punto de recuperación más antiguo que puede usar es de 72 horas.

### <a name="how-do-i-access-azure-vms-after-failover"></a>¿Cómo accedo a las máquinas virtuales de Azure después de la conmutación por error?
Después de la conmutación por error, puede tener acceso a las máquinas virtuales de Azure a través de una conexión segura a Internet o a través de una VPN de sitio a sitio o mediante Azure ExpressRoute. Debe preparar algunas cosas para la conexión. [Más información](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover)

### <a name="is-failed-over-data-resilient"></a>¿Son resistentes los datos conmutados por error?
Azure está diseñado para la resistencia. Site Recovery está diseñado para la conmutación por error en un centro de datos de Azure secundario según el Acuerdo de Nivel de Servicio de Azure. Cuando se produce una conmutación por error, nos aseguramos de que los metadatos y los almacenes permanecen en la misma región geográfica que eligió para el almacén.

### <a name="is-failover-automatic"></a>¿La conmutación por error es automática?
La [conmutación por error](site-recovery-failover.md) no es automática. Puede iniciar las conmutaciones por error con solo un clic en el portal, o puede usar [PowerShell](/powershell/module/azurerm.siterecovery) para desencadenar una conmutación por error.

### <a name="can-i-fail-back-to-a-different-location"></a>¿Es posible conmutar por recuperación en otra ubicación?
Sí, si conmutó por error en Azure, puede conmutar por recuperación en otra ubicación si la ubicación original no está disponible. [Más información](concepts-types-of-failback.md#alternate-location-recovery-alr).

### <a name="why-do-i-need-a-vpn-or-expressroute-to-fail-back"></a>¿Por qué se necesita una VPN o ExpressRoute para conmutar por recuperación?
Cuando conmuta por recuperación desde Azure, los datos provenientes de Azure se copia nuevamente en la máquina virtual local y se requiere acceso privado.

### <a name="can-i-resize-the-azure-vm-after-failover"></a>¿Puedo cambiar el tamaño de la máquina virtual de Azure después de la conmutación por error?
No, no se puede cambiar el tamaño ni el tipo de la máquina virtual de destino después de la conmutación por error.


## <a name="automation-and-scripting"></a>Automatización y scripts

### <a name="can-i-set-up-replication-with-scripting"></a>¿Puedo configurar la replicación con scripts?
Sí. Puede automatizar los flujos de trabajo de Site Recovery mediante la API de Rest, PowerShell o el SDK de Azure.[Más información](vmware-azure-disaster-recovery-powershell.md).

## <a name="performance-and-capacity"></a>Rendimiento y capacidad
### <a name="can-i-throttle-replication-bandwidth"></a>¿Puedo limitar el ancho de banda de replicación?
Sí. [Más información](site-recovery-plan-capacity-vmware.md).


## <a name="next-steps"></a>Pasos siguientes
* [Revise](vmware-physical-azure-support-matrix.md) los requisitos de compatibilidad.
* [Configure](vmware-azure-tutorial.md) la replicación de VMware en Azure.
