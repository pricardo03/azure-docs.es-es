---
title: Solución de problemas de Azure Migrate | Microsoft Docs
description: Se proporciona información general sobre los problemas conocidos del servicio Azure Migrate, además de sugerencias para solucionar los errores comunes.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: raynew
ms.openlocfilehash: 15d3809b9a028fd2495c504e9bf19251dd051520
ms.sourcegitcommit: 57a7d4f67635212f5bf0c56e58fd87c8ec366f2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68372588"
---
# <a name="troubleshoot-azure-migrate"></a>Solución de problemas de Azure Migrate

[Azure Migrate](migrate-services-overview.md) proporciona un centro de herramientas de Microsoft para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV). En este documento, se ofrece ayuda para solucionar errores con Azure Migrate, Azure Migrate: Server Assessment y Azure Migrate: Server Migration.

## <a name="azure-migrate-project-issues"></a>Problemas de los proyectos de Azure Migrate

### <a name="i-am-unable-to-find-my-existing-azure-migrate-project"></a>No puedo encontrar mi proyecto de Azure Migrate.

Existen [dos versiones](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) de Azure Migrate. En función de la versión en la que se haya creado el proyecto, siga los pasos que se indican a continuación para buscarlo:

1. Si busca un proyecto creado con la versión anterior (experiencia antigua) de Azure Migrate, siga los pasos que se indican a continuación para buscar el proyecto.

    1. Vaya a [Azure Portal](https://portal.azure.com) y busque **Azure Migrate**.
    2. En el panel de Azure Migrate, verá un banner sobre el acceso a proyectos antiguos. Solo verá este banner si ha creado un proyecto con la experiencia anterior. Haga clic en el banner.

    ![Acceso a proyectos existentes](./media/troubleshooting-general/access-existing-projects.png)

    3. Ahora verá la lista de los proyectos existentes creados con la versión anterior de Azure Migrate.

2. Si busca un proyecto creado con la versión actual (experiencia nueva), siga los pasos que se indican a continuación.

    1. Vaya a [Azure Portal](https://portal.azure.com) y busque **Azure Migrate**.
    2. En el panel de Azure Migrate, vaya a la página **Servidores** en el panel izquierdo y seleccione **Cambiar** en la esquina superior derecha:

    ![Cambio a un proyecto de Azure Migrate existente](./media/troubleshooting-general/switch-project.png)

    3. Seleccione la **Suscripción** adecuada y, después, **Migrar proyecto**.

### <a name="i-am-unable-to-create-a-second-azure-migrate-project"></a>No puedo crear un segundo proyecto de Azure Migrate.

Siga los pasos que se indican a continuación para crear un nuevo proyecto de Azure Migrate.

1. Vaya a [Azure Portal](https://portal.azure.com) y busque **Azure Migrate**.
2. En el panel de Azure Migrate, vaya a la página **Servidores** en el panel izquierdo y seleccione **Cambiar** en la esquina superior derecha:

   ![Cambio de un proyecto de Azure Migrate](./media/troubleshooting-general/switch-project.png)

3. Para crear un nuevo proyecto, seleccione **haga clic aquí**, tal como se muestra en la siguiente captura de pantalla:

   ![Creación de un segundo proyecto de Azure Migrate](./media/troubleshooting-general/create-new-project.png)

### <a name="which-azure-geographies-are-supported-by-azure-migrate"></a>¿Qué geografías de Azure con compatibles con Azure Migrate?

Puede encontrar la lista de [VMware aquí](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) y la de [Hyper-V aquí](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="deletion-of-azure-migrate-projects-and-associated-log-analytics-workspace"></a>Eliminación de proyectos de Azure Migrate y el área de trabajo de Log Analytics asociada

Cuando se elimina un proyecto de Azure Migrate, se elimina el proyecto de migración junto con los metadatos acerca de las máquinas detectadas. Sin embargo, si ha vinculado un área de trabajo de Log Analytics a la herramienta Server Assessment, no elimina automáticamente el área de trabajo de Log Analytics. Esto es porque la misma área de trabajo de Log Analytics se puede usar para varios casos de uso. Si también quiere eliminar el área de trabajo de Log Analytics, deberá hacerlo manualmente.

1. Vaya al área de trabajo de Log Analytics vinculada al proyecto.
     1. Si aún no ha eliminado el proyecto de migración, puede encontrar el vínculo al área de trabajo desde la página de información general de Server Assessment en la sección Essentials.

     ![Área de trabajo de LA](./media/troubleshooting-general/loganalytics-workspace.png)

     2. Si ya ha eliminado el proyecto de migración, seleccione **Grupos de recursos** en el panel izquierdo de Azure Portal. Vaya al grupo de recursos en el que se creó el área de trabajo y, a continuación, desplácese hasta él.
2. Siga las instrucciones de [este artículo](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace) para eliminar el área de trabajo.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Error al crear el proyecto de migración, que indica que *las solicitudes deben contener encabezados de identidad del usuario*

Pueden experimentar este problema aquellos usuarios que no tienen acceso al inquilino de Azure Active Directory (Azure AD) de la organización. Cuando un usuario se agrega a un inquilino de Azure AD por primera vez, recibe una invitación por correo electrónico para unirse al inquilino. Los usuarios deben ir al correo electrónico y aceptar la invitación para agregarse correctamente al inquilino. Si no puede ver el correo electrónico, póngase con un usuario que ya tenga acceso al inquilino y pídale que vuelva a enviarle la invitación mediante los pasos que se indican [aquí](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Cuando reciba el correo electrónico de invitación, debe abrirlo y hacer clic en el vínculo que contiene para aceptar la invitación. Una vez hecho esto, debe cerrar la sesión de Azure Portal e iniciarla de nuevo. La actualización del explorador no funcionará. A continuación, puede intentar crear el proyecto de migración.

## <a name="appliance-issues"></a>Problemas del dispositivo

### <a name="deployment-of-azure-migrate-appliance-for-vmware-failed-with-the-error-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry"></a>La implementación del dispositivo de Azure Migrate para VMware produjo el siguiente error: The provided manifest file is invalid: Invalid OVF manifest entry (El archivo de manifiesto proporcionado no es válido: entrada de manifiesto OVF no válida).

1. Compruebe si el archivo OVA del dispositivo de Azure Migrate se descargó correctamente; para ello, compruebe el valor hash. Consulte este [artículo](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware) para comprobar el valor hash. Si el valor de hash no coincide, vuelva a descargar el archivo OVA e intente de nuevo la implementación.
2. Si sigue sin funcionar y utiliza el cliente de VMware vSphere para implementar el OVF, intente implementarlo mediante el cliente web de vSphere. Si sigue sin funcionar, intente usar otro explorador web.
3. Si está usando el cliente web de vSphere e intenta implementarlo en vCenter Server 6.5 o 6.7, pruebe a implementar el archivo OVA directamente en el host ESXi siguiendo estos pasos:
   - Conéctese directamente al host ESXi (en lugar de vCenter Server) mediante el cliente web (https://<*dirección IP del host*>/ui).
   - Vaya a **Home** (Inicio)  > **Inventory** (Inventario).
   - Haga clic en **File** (Archivo)  > **Deploy OVF Template** (Implementar plantilla de OVF). Vaya al archivo OVA y complete la implementación.
4. Si la implementación sigue sin funcionar, póngase en contacto con el soporte técnico de Azure Migrate.

### <a name="appliance-is-not-able-to-connect-to-the-internet"></a>El dispositivo no es capaz de conectarse a Internet

Esto puede ocurrir cuando la máquina que está utilizando está detrás de un proxy. Asegúrese de proporcionar las credenciales de autorización en caso de que el proxy las necesite.
Si va a usar un proxy de firewall basado en direcciones URL para controlar la conectividad saliente, asegúrese de agregar estas direcciones URL necesarias a la lista de direcciones permitidas:

Escenario | Lista de direcciones URL
--- | ---
Server Assessment para VMware | [Aquí](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-url-access-requirements)
Server Assessment para Hyper-V | [Aquí](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#assessment-appliance-url-access)
Server Migration para VMware (sin agente) | [Aquí](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-url-access-requirements)
Server Migration para VMware (basado en agente) | [Aquí](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#replication-appliance-url-access)
Server Migration para Hyper-V | [Aquí](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#migration-hyper-v-host-url-access)

Si usa un proxy de interceptación para conectarse a Internet, no tiene que importar el certificado de proxy en la máquina virtual del dispositivo. Puede importar el certificado de proxy mediante los pasos detallados [aquí](https://docs.microsoft.com/azure/migrate/concepts-collector).

### <a name="error-802-date-and-time-synchronization-error"></a>Error 802: error de sincronización de fecha y hora

El reloj del servidor podría estar desincronizado con la hora actual en más de cinco minutos. Cambie la hora del reloj de la máquina virtual del recopilador para que coincida con la hora actual; para ello, siga estos pasos:

1. Abra un símbolo del sistema de administrador en la máquina virtual.
2. Para comprobar la zona horaria, ejecute w32tm /tz.
3. Para sincronizar la hora, ejecute w32tm /resync.


### <a name="error-unabletoconnecttoserver"></a>Error UnableToConnectToServer

No se puede conectar con vCenter Server "Servername.com:9443" por el siguiente error: There was no endpoint listening at https://Servername.com:9443/sdk that could accept the message (No había ningún punto de conexión escuchando en https://Servername.com:9443/sdk que pudiera aceptar el mensaje).

Compruebe si está ejecutando la versión más reciente del dispositivo recopilador; de lo contrario, actualice el dispositivo a la [última versión](https://docs.microsoft.com/azure/migrate/concepts-collector).

Si el problema persiste con la última versión, es posible que el equipo recopilador no pueda resolver el nombre de vCenter Server especificado o que el puerto especificado sea incorrecto. De forma predeterminada, si no se especifica el puerto, el recopilador intentará conectarse el puerto número 443.

1. Intente hacer ping a Servername.com desde el equipo recopilador.
2. Si se produce un error en el paso 1, intente conectarse al servidor de vCenter a través de la dirección IP.
3. Identifique el número de puerto correcto para conectarse a vCenter.
4. Por último, compruebe si el servidor de vCenter está en funcionamiento.


### <a name="the-appliance-could-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60052"></a>No se pudo registrar correctamente el dispositivo en el proyecto de Azure Migrate (identificador de error: 60052)

Este error se debe a que no hay permisos suficientes en la cuenta de Azure usada para registrar el dispositivo. Asegúrese de que la cuenta de usuario de Azure que se ha usado para registrar el dispositivo tiene al menos el rol de colaborador en la suscripción. [Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) sobre los roles y permisos de Azure necesarios.

### <a name="the-appliance-could-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60039"></a>No se pudo registrar correctamente el dispositivo en el proyecto de Azure Migrate (identificador de error: 60039)

No se encuentra el proyecto de Azure Migrate que seleccionó para registrar el dispositivo, lo cual ha provocado un error en el registro. Vaya a Azure Portal y compruebe si el proyecto existe en el grupo de recursos. Si el proyecto no existe, cree un nuevo proyecto de Azure Migrate en el grupo de recursos y vuelva a registrar el dispositivo. [Más información](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool) sobre la creación de un nuevo proyecto de Azure Migrate.

### <a name="azure-key-vault-management-operation-failed-error-id-60030-60031"></a>Error durante la operación de administración del almacén de claves de Azure (identificador de error: 60030, 60031)

Asegúrese de que la cuenta de usuario de Azure que se ha usado para registrar el dispositivo tiene al menos el rol de colaborador en la suscripción. Además, compruebe si la cuenta tiene acceso al almacén de claves especificado en el mensaje de error y vuelva a intentar la operación. Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft. [Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) sobre los roles y permisos de Azure necesarios.

### <a name="discovery-could-not-be-initiated-due-to-the-error-the-operation-failed-for-the-given-list-of-hosts-or-clusters-error-id-60028"></a>No se pudo iniciar la detección debido al error. No se pudo realizar la operación para la lista especificada de hosts o clústeres (identificador de error: 60028)

No se pudo iniciar la detección en los hosts enumerados en el error debido a un problema al acceder a la información de la máquina virtual o recuperarla. El resto de los hosts que ha agregado se han agregado correctamente. Vuelva a agregar los hosts del error mediante la opción **Agregar host**. Si se produce un error de validación, revise las instrucciones de solución de los errores y seleccione **Guardar e iniciar la detección** de nuevo.

### <a name="azure-active-directory-aad-operation-failed-the-error-occurred-while-creating-or-updating-the-aad-application-error-id-60025"></a>Error de operación de Azure Active Directory (AAD). El error se produjo al crear o actualizar la aplicación de AAD (identificador de error: 60025)

La cuenta de usuario de Azure usada para registrar el dispositivo no tiene acceso a la aplicación de AAD especificada en el mensaje de error. Compruebe si es el propietario de la aplicación de AAD. [Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) sobre los permisos de la aplicación de AAD.


## <a name="discovery-issues"></a>Problemas de detección

### <a name="i-started-discovery-but-i-dont-see-the-discovered-vms-on-azure-portal-server-assessment-and-server-migration-tiles-show-a-status-of-discovery-in-progress"></a>He iniciado la detección, pero no veo las máquinas virtuales detectadas en Azure Portal. Los iconos de Server Assessment y Server Migration muestran el estado "Detección en curso"
Después de iniciar la detección desde el dispositivo, deje tiempo para que las máquinas detectadas se muestren en Azure Portal. Se tarda aproximadamente 15 minutos en la detección de VMware y unos 2 minutos por cada host agregado para una detección de Hyper-V. Si sigue viendo "Detección en curso" incluso después de este tiempo, haga clic en **Actualizar** en la pestaña **Servidores**. Se debería mostrar el recuento de los servidores detectados en los iconos de Server Assessment y Server Migration.


### <a name="i-am-using-the-appliance-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-being-shown-in-the-portal"></a>Utilizo el dispositivo de detección continua en el entorno local, pero las máquinas virtuales que se eliminan de mi entorno local se siguen mostrando en el portal.

Los datos de detección recopilados por el dispositivo tardan hasta 30 minutos en reflejarse en el portal. Si no ve información actualizada ni siquiera después de 30 minutos, siga estos pasos para emitir una actualización de los datos:

1. En Servidores > Azure Migrate: Server Assessment, haga clic en **Información general**.
2. En **Administrar**, haga clic en **Agent Health**
3. Haga clic en la opción **Actualizar agente**. Verá esta opción debajo de la lista de agentes.
4. Espere a que se complete la operación de actualización. Compruebe que es capaz de ver la información actualizada sobre las máquinas virtuales.

### <a name="i-dont-the-latest-information-on-the-on-premise-vms-on-the-portal"></a>No veo la información más reciente sobre las máquinas virtuales locales en el portal

Los datos de detección recopilados por el dispositivo tardan hasta 30 minutos en reflejarse en el portal. Si no ve la información actualizada ni siquiera después de 30 minutos, siga estos pasos para emitir una actualización de los datos:

1. En Servidores > Azure Migrate: Server Assessment, haga clic en **Información general**.
2. En **Administrar**, haga clic en **Agent Health**
3. Haga clic en la opción **Actualizar agente**. Verá esta opción debajo de la lista de agentes.
4. Espere a que se complete la operación de actualización. Ahora debería ver la información actualizada sobre las máquinas virtuales.

### <a name="unable-to-connect-to-hosts-or-cluster-as-the-server-name-cannot-be-resolved-winrm-error-code-0x803381b9-error-id-50004"></a>No se puede conectar con los hosts o el clúster porque no se puede resolver el nombre del servidor. Código de error WinRM: 0x803381B9 (identificador de error: 50004)
Este error se produce si el servidor DNS que atiende el dispositivo no puede resolver el nombre de host o clúster proporcionado. Si ve este error en el clúster, intente proporcionar el nombre de dominio completo del clúster.

Este error también se puede ver para los hosts de un clúster. En este caso, el dispositivo pudo conectarse al clúster. Pero el clúster ha devuelto los nombres de host que no son nombres de dominio completos.

Para resolver este error, actualice el archivo de hosts en el dispositivo; para ello, agregue una asignación de la dirección IP y los nombres de host.
1. Abra el Bloc de notas como usuario administrador. Abra el archivo C:\Windows\System32\Drivers\etc\hosts.
2. Agregue la dirección IP y el nombre de host en una fila. Repita el procedimiento para cada host o clúster en el que observe este error.
3. Guarde el archivo de hosts y ciérrelo.
4. Puede comprobar si el dispositivo puede conectarse a los hosts mediante la aplicación de administración del dispositivo. Después de 30 minutos, debería poder ver la información más reciente sobre estos hosts en Azure Portal.


## <a name="assessment-issues"></a>Problemas de evaluación

### <a name="azure-readiness-issues"></a>Problemas de preparación para Azure

Problema | Corrección
--- | ---
Tipo de arranque no compatible | Azure no admite máquinas virtuales con el tipo de arranque EFI. Se recomienda convertir el tipo de arranque a BIOS antes de ejecutar una migración. <br/><br/>Puede usar Azure Migrate Server Migration para realizar la migración de tales máquinas virtuales, dado que se convertirá el tipo de arranque de la máquina virtual a BIOS durante la migración.
Sistema operativo Windows admitido con condiciones | El sistema operativo alcanzó la fecha de finalización del soporte técnico y necesita un contrato de soporte técnico personalizado (CSA) para [recibir soporte técnico en Azure](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar el sistema operativo antes de migrar a Azure.
Sistema operativo Windows no admitido | Azure solo admite [algunas versiones del sistema operativo Windows](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar el sistema operativo de la máquina antes de migrar a Azure.
Sistema operativo Linux aprobado con condiciones | Azure solo aprueba [algunas versiones del sistema operativo Linux](../virtual-machines/linux/endorsed-distros.md). Considere la posibilidad de actualizar el sistema operativo de la máquina antes de migrar a Azure.
Sistema operativo Linux no aprobado | La máquina se puede arrancar en Azure, pero Azure no proporciona compatibilidad con ningún sistema operativo. Considere la posibilidad de actualizar el sistema operativo a una [versión Linux aprobada](../virtual-machines/linux/endorsed-distros.md) antes de migrar a Azure
Sistema operativo desconocido | El sistema operativo de la máquina virtual se especificó como "Otro" en vCenter Server, debido a que Azure Migrate no puede identificar la preparación para Azure de la máquina virtual. Asegúrese de que el sistema operativo que se ejecuta dentro de la máquina se [admite](https://aka.ms/azureoslist) en Azure antes de migrar la máquina.
Valor de bits de sistema operativo no aprobado | Las máquinas virtuales con sistemas operativos de 32 bits pueden arrancar en Azure; sin embargo, se recomienda actualizar el sistema operativo de la máquina virtual a 64 bits antes de migrar a Azure.
Requiere una suscripción a Visual Studio | En la máquina se ejecuta un sistema operativo cliente Windows, que solo se admite en Visual Studio Subscription.
No se encontró ninguna máquina virtual para el rendimiento de almacenamiento requerido | El rendimiento de almacenamiento (IOPS/rendimiento) requerido para la máquina excede el soporte técnico de máquina virtual de Azure. Reduzca los requisitos de almacenamiento de la máquina antes de realizar la migración.
No se encontró ninguna máquina virtual para el rendimiento de red requerido | El rendimiento de red (entrada/salida) requerido para la máquina excede el soporte técnico de máquina virtual de Azure. Reduzca los requisitos de red de la máquina.
No se encontró la máquina virtual en la ubicación especificada | Utilice una ubicación de destino diferente antes de la migración.
Uno o varios discos no son adecuados | Uno o varios discos conectados a la VM no cumplen los requisitos de Azure. Para cada disco conectado a la VM, compruebe que el tamaño del disco sea inferior a 4 TB. De lo contrario, redúzcalo antes de migrar a Azure. Asegúrese de que el rendimiento (IOPS/rendimiento) necesario para cada disco sea compatible con los [discos de máquinas virtuales administradas](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) de Azure.   
Uno o varios adaptadores de red no son adecuados | Quite los adaptadores de red no utilizados de la máquina antes de realizar la migración.
El recuento del disco supera el límite | Quite los discos no utilizados del equipo antes de la migración.
El tamaño del disco supera el límite | Azure admite discos con un tamaño de hasta 4 TB. Reduzca los discos a un tamaño inferior a 4 TB antes de la migración.
El disco no está disponible en la ubicación especificada | Asegúrese de que el disco está en la ubicación de destino antes de realizar la migración.
El disco no está disponible para la redundancia especificada | El disco debe usar el tipo de almacenamiento de redundancia definido en la configuración de evaluación (LRS de forma predeterminada).
No se pudo determinar la idoneidad del disco debido a un error interno | Intente crear una evaluación para el grupo.
No se encontró ninguna máquina virtual con los núcleos y memoria requeridos | Azure no pudo ajustar un tipo de máquina virtual adecuado. Reduzca la memoria y el número de núcleos de la máquina local antes de realizar la migración.
No se pudo determinar la idoneidad de la máquina virtual debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios discos debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios adaptadores de red debido a un error interno | Intente crear una evaluación para el grupo.

### <a name="i-am-unable-to-specify-enterprise-agreement-ea-as-an-azure-offer-in-the-assessment-properties"></a>No puedo especificar el Contrato Enterprise (EA) como una oferta de Azure en las propiedades de la evaluación.
Azure Migrate: Server Assessment no admite actualmente precios basados en el Contrato Enterprise (EA). La solución alternativa es usar la opción de "Pago por uso" como la oferta de Azure y usar la propiedad "Descuento" para especificar los descuentos personalizados que reciba. [Más información sobre cómo puede personalizar una evaluación](https://aka.ms/migrate/selfhelp/eapricing).

### <a name="why-does-server-assessment-mark-my-linux-vms-conditionally-ready-is-there-anything-i-need-to-do-to-fix-this"></a>¿Por qué Server Assessment marca las máquinas virtuales Linux como "Lista con condiciones"? ¿Qué se puede hacer para solucionar este problema?
Hay un problema conocido en Server Assessment por el que no es capaz de detectar la versión secundaria del sistema operativo Linux instalado en las máquinas virtuales en el entorno local (por ejemplo, en el caso de RHEL 6.10, actualmente Server Assessment solo detecta RHEL 6 como la versión del sistema operativo). Puesto que Azure solo aprueba versiones específicas de Linux, las máquinas virtuales Linux actualmente se marcan como preparadas con condiciones en Server Assessment. Puede comprobar manualmente si el sistema operativo Linux en ejecución en la máquina virtual en el entorno local está aprobado en Azure; para ello, consulte la [documentación de soporte técnico de Linux en Azure](https://aka.ms/migrate/selfhost/azureendorseddistros). Una vez haya comprobado que la distribución está aprobada, puede omitir esta advertencia.

### <a name="the-vm-sku-recommended-by-server-assessment-has-more-number-of-cores-and-a-larger-memory-size-than-what-is-allocated-on-premises-why-is-that-so"></a>La SKU de máquina virtual recomendada por Server Assessment tiene más núcleos y un tamaño de memoria mayor que la asignación en el entorno local. ¿Por qué ocurre esto?
La recomendación de SKU de máquina virtual de Server Assessment depende de las propiedades de la evaluación. Puede crear dos tipos de evaluaciones en Server Assessment, "en función del rendimiento" y "como en el entorno local". Para obtener evaluaciones en función del rendimiento, Server Assessment tiene en cuenta los datos de uso de las máquinas virtuales en el entorno local (uso de CPU, memoria, disco y red) para determinar la SKU de máquina virtual de destino correcta para las máquinas virtuales en el entorno local. Además, para ajustar el tamaño según el rendimiento, se tiene en cuenta el factor de confort para identificar el uso eficaz. Para el ajuste de tamaño en la opción como en el entorno local, no se tienen en cuenta los datos de rendimiento y se recomienda una SKU de destino en función de lo que hay asignado en el entorno local.

Por ejemplo, supongamos que hay una máquina virtual en el entorno local con 4 núcleos y 8 GB de memoria con un uso de CPU del 50 % y un uso de memoria del 50 %, y que se especifica un factor de confort de 1,3 en la evaluación. Si el criterio de ajuste de tamaño de la evaluación es "como en el entorno local", se recomienda una SKU de máquina virtual de Azure con 4 núcleos y 8 GB de memoria; sin embargo, si el criterio de tamaño es la opción "en función del rendimiento", según el uso eficaz de CPU y memoria (el 50 % de 4 núcleos * 1,3 = 2,6 núcleos y el 50 % de 8 GB de memoria * 1,3 = 5,3 GB de memoria), se recomendaría la SKU de máquina virtual más barata de 4 núcleos (el número de núcleos admitidos más próximo) y un tamaño de memoria de 8 GB (el tamaño de memoria admitido más próximo). [Más información sobre el ajuste de tamaño de Server Assessment](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

### <a name="the-disk-sku-recommended-by-server-assessment-has-a-bigger-size-than-what-is-allocated-on-premises-why-is-that-so"></a>La SKU de disco recomendada por Server Assessment tiene un tamaño mayor que la asignación en el entorno local. ¿Por qué ocurre esto?
El ajuste de tamaño del disco en Server Assessment depende de dos propiedades de la valoración: el criterio de ajuste de tamaño y el tipo de almacenamiento. Si el criterio de ajuste de tamaño es "en función del rendimiento" y el tipo de almacenamiento es "automático", se tendrán en cuenta los valores de rendimiento y de IOPS del disco para identificar el tipo de disco de destino (discos HDD estándar, SSD estándar o Premium). Se recomienda una SKU de disco de dicho tipo de disco en función de los requisitos de tamaño del disco en el entorno local. Si el criterio de ajuste de tamaño es "en función del rendimiento" y el tipo de almacenamiento es Premium, se recomienda una SKU de disco Premium de Azure en función de los requisitos de IOPS, rendimiento y tamaño del disco en el entorno local. Se usa la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es "como en el entorno local" y el tipo de almacenamiento es "HDD estándar", "SSD estándar" o "Premium".

Por ejemplo, si tiene un disco en el entorno local con 32 GB de memoria, pero las IOPS de lectura y escritura agregadas son 800 IOPS, Server Assessment recomendará un tipo de disco Prémium (debido a los mayores requisitos de IOPS) y, a continuación, una SKU de disco que pueda admitir el tamaño y las IOPS necesarias. La coincidencia más cercana en este ejemplo sería P15 (256 GB, 1100 IOPS). Por lo tanto, aunque el tamaño requerido por el disco en el entorno local fue de 32 GB, Server Assessment recomendó un disco con un tamaño mayor debido a los altos requisitos de IOPS del disco en el entorno local.

### <a name="why-does-my-assessment-report-say-percentageofcoresutilizedmissing-or-percentageofmemoryutilizedmissing-for-some-vms"></a>¿Por qué mi informe de evaluación indica "PercentageOfCoresUtilizedMissing" o "PercentageOfMemoryUtilizedMissing" para algunas máquinas virtuales?
Los problemas anteriores se muestran cuando el dispositivo de Azure Migrate no puede recopilar los datos de rendimiento de las máquinas virtuales locales. Esto puede ocurrir si las máquinas virtuales están apagadas en el momento en que se realiza la evaluación (último día/una semana/mes), ya que el dispositivo no puede recopilar los datos de rendimiento de una máquina virtual cuando está apagada. Si solo faltan contadores de memoria y está intentando evaluar las máquinas virtuales de Hyper-V, compruebe si tiene habilitada la memoria dinámica en estas máquinas virtuales. Existe un problema conocido debido a que el dispositivo de Azure Migrate no puede recopilar el uso de memoria de las máquinas virtuales que no tienen habilitada la memoria dinámica. Tenga en cuenta que el problema solo se produce en las máquinas virtuales de Hyper-V y no en las máquinas virtuales de VMware. Si falta alguno de los contadores de rendimiento, Azure Migrate: Server Assessment vuelve a la memoria y los núcleos asignados, y recomienda un tamaño de máquina virtual acorde.

### <a name="is-the-os-license-cost-of-the-vm-included-in-the-compute-cost-estimated-by-server-assessment"></a>¿El costo de la licencia del sistema operativo de la máquina virtual se incluye en el costo de proceso estimado por Server Assessment?
Actualmente, Server Assessment solo tiene en cuenta el costo de la licencia del sistema operativo para las máquinas Windows; no se incluye el costo de la licencia del sistema operativo para las máquinas Linux.

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>¿Qué impacto tiene el historial de rendimiento y uso de percentil en las recomendaciones de tamaño?
Estas propiedades solo son aplicables para el ajuste de tamaño basado en el rendimiento. Server Assessment recopila continuamente datos de rendimiento de las máquinas en el entorno local y los usa para recomendar la SKU de máquina virtual y la SKU de disco de Azure. A continuación se muestra cómo recopila Server Assessment los datos de rendimiento:
- El dispositivo de Azure Migrate genera perfiles del entorno local continuamente con el fin de recopilar datos de uso en tiempo real cada 20 segundos, en el caso de máquinas virtuales VMware, y cada 30 segundos, en el caso de máquinas virtuales Hyper-V.
- El dispositivo acumula muestras cada 20/30 segundos y crea un único punto de datos cada 10 minutos. Para crear el punto de datos único, el dispositivo selecciona el valor máximo de todas las muestras cada 20/30 segundos y lo envía a Azure.
- Cuando se crea una evaluación en Server Assessment en función de la duración del rendimiento y del valor del percentil del historial de rendimiento, se identifica el valor de uso representativo. Por ejemplo, si el historial de rendimiento es de una semana y el percentil de uso es 95, Azure Migrate ordenará todos los puntos de muestra de 10 minutos de la última semana en orden ascendente y, a continuación, seleccionará el percentil 95 como valor representativo.
El valor del percentil 95 garantiza que se van a omitir los valores atípicos que se pueden incluir si elige el percentil 99. Si quiere elegir el uso máximo para el período y no desea omitir ningún valor atípico, debe seleccionar el percentil 99 como percentil de uso.

## <a name="dependency-visualization-issues"></a>Problemas de visualización de dependencia

### <a name="i-am-unable-to-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>No puedo encontrar la funcionalidad de visualización de dependencia para proyectos de Azure Government.

Azure Migrate depende de Service Map para la funcionalidad de visualización de dependencia y puesto que Service Map no está disponible actualmente en Azure Government, esta funcionalidad no está disponible en Azure Government.

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>He instalado Microsoft Monitoring Agent (MMA) y el agente de dependencias en las máquinas virtuales locales, pero las dependencias no se muestran en el portal de Azure Migrate.

Después de instalar los agentes, Azure Migrate suele tardar entre 15 y 30 minutos en mostrar las dependencias en el portal. Si ha esperado durante más de 30 minutos, asegúrese de que el agente de MMA es capaz de comunicarse con el área de trabajo de OMS. Para ello, siga estos pasos:

Para máquinas virtuales con Windows 10:
1. En el **Panel de Control**, abra **Microsoft Monitoring Agent**.
2. Vaya a la pestaña **Azure Log Analytics (OMS)** en la ventana emergente de propiedades de MMA.
3. Asegúrese de que el **Estado** del área de trabajo está en verde.
4. Si el estado no está en verde, intente quitar el área de trabajo y vuelva a agregarla al MMA.
        ![Estado del MMA](./media/troubleshooting-general/mma-status.png)

Para máquinas virtuales con Linux, asegúrese de que se hayan completado correctamente los comandos de instalación de MMA y el agente de dependencias.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>¿Cuáles son los sistemas operativos compatibles con MMA?

[Aquí](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) tiene una lista de los sistemas operativos Windows compatibles con MMA.
La lista de los sistemas operativos de Linux compatibles con MMA la encontrará [aquí](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>¿Cuáles son los sistemas operativos compatibles con el agente de dependencias?

[Aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) tiene una lista de los sistemas operativos Windows compatibles con el agente de dependencias.
La lista de los sistemas operativos de Linux compatibles con el agente de dependencias la encontrará [aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-am-unable-to-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>No puedo visualizar las dependencias de Azure Migrate durante más de una hora.
Azure Migrate le permite visualizar las dependencias durante un máximo de una hora. Aunque Azure Migrate le permite volver atrás a una fecha concreta en el historial de hasta el último mes, el tiempo máximo durante el que se pueden visualizar las dependencias es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Sin embargo, puede usar los registros de Azure Monitor para [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>No puedo visualizar las dependencias para grupos con más de 10 máquinas virtuales.
Puede [visualizar las dependencias de grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) que tengan un máximo de diez máquinas virtuales. Si tiene un grupo con más de diez, recomendamos dividirlo en grupos más pequeños y visualizar las dependencias.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>He instalado agentes y he usado la visualización de dependencias para crear grupos. Ahora, después de la conmutación por error, los equipos muestran la acción "Instalar agente" en lugar de "Ver dependencias"
* Tras una conmutación por error planeada o no planeada, los equipos locales se desactivan y los equipos equivalentes se activan en Azure. Estos equipos adquieren una dirección MAC diferente. Pueden adquirir una dirección IP distinta en función de si el usuario elige conservar o no la dirección IP local. Si las direcciones IP y MAC difieren, Azure Migrate no asocia los equipos locales con ningún dato de dependencia de Service Map y solicita al usuario que instale los agentes en lugar de visualizar las dependencias.
* Después de la conmutación por error de prueba, los equipos locales permanecen encendidos según lo previsto. Los equipos equivalentes que se activan en Azure adquieren una dirección MAC distinta y pueden adquirir una dirección IP diferente. A menos que el usuario bloquee el tráfico saliente de los registros de Azure Monitor procedente de estas máquinas, Azure Migrate no asocia las máquinas locales con ningún dato de dependencia de Service Map y solicita al usuario que instale los agentes en lugar de visualizar las dependencias.

## <a name="collect-azure-portal-logs"></a>Recopilación de registros de Azure Portal

**¿Cómo puedo recopilar registros de tráfico de red de Azure Portal?**

1. Abra el explorador, vaya al [portal](https://portal.azure.com) e inicie sesión en él.
2. Presione F12 para iniciar las Herramientas de desarrollo. Si es necesario, desactive la configuración **Clear entries on navigation** (Borrar entradas de navegación).
3. Haga clic en la pestaña **Network** (Red) para empezar a capturar el tráfico de red:
   - En Chrome, seleccione **Preserve log** (Conservar registro). La grabación debe iniciarse automáticamente. Un círculo rojo indica que la captura del tráfico está en curso. Si no aparece, haga clic en el círculo negro para iniciar la captura.
   - En Microsoft Edge/IE, la grabación debe iniciarse automáticamente. Si no es así, haga clic en el botón de reproducción de color verde.
4. Pruebe a reproducir el error.
5. Una vez haya encontrado el error durante la grabación, detenga la grabación y guarde una copia de la actividad grabada:
   - En Chrome, haga clic con el botón derecho y haga clic en **Save as HAR with content** (Guardar como HAR con contenido). Se ese modo, los registros se comprimen y exportan como un archivo .har.
   - En Microsoft Edge/IE, haga clic en el icono **Exportar el tráfico capturado**. Así se comprime y exporta el registro.
6. Navegue a la pestaña **Console** (Consola) para ver si hay alguna advertencia o error. Para guardar el registro de la consola:
   - En Chrome, haga clic con el botón derecho en cualquier lugar en el registro de la consola. Seleccione **Save as** (Guardar como) para exportar y comprimir el registro.
   - En Microsoft Edge/IE, haga clic con el botón derecho sobre los errores y seleccione **Copy all** (Copiar todo).
7. Cierre las Herramientas de desarrollo.
