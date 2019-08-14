---
title: Solución de problemas de Azure Migrate | Microsoft Docs
description: Se proporciona información general sobre los problemas conocidos del servicio Azure Migrate, además de sugerencias para solucionar los errores comunes.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 08/06/2019
ms.author: raynew
ms.openlocfilehash: fa1e7fcf89ccc06e429831191ba5dfce3cf33797
ms.sourcegitcommit: 3073581d81253558f89ef560ffdf71db7e0b592b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68828320"
---
# <a name="troubleshoot-azure-migrate"></a>Solución de problemas de Azure Migrate

[Azure Migrate](migrate-services-overview.md) proporciona un centro de herramientas para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV). Este artículo le ayuda a solucionar errores con Azure Migrate, Azure Migrate Server Assessment y Azure Migrate Server Migration.

## <a name="azure-migrate-project-issues"></a>Problemas de los proyectos de Azure Migrate

### <a name="i-cant-find-my-existing-azure-migrate-project"></a>No encuentro el proyecto de Azure Migrate existente.

Existen [dos versiones](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) de Azure Migrate. Use uno de los métodos siguientes en función de la versión en la que se haya creado el proyecto:

* Si busca un proyecto creado con la versión anterior (experiencia antigua) de Azure Migrate, siga estos pasos para buscar el proyecto:

    1. Vaya a [Azure Portal](https://portal.azure.com) y busque **Azure Migrate**.
    2. En el panel de Azure Migrate, verá un banner sobre el acceso a proyectos antiguos. Solo verá este banner si ha creado un proyecto con la experiencia anterior. Seleccione el banner.

       ![Acceso a proyectos existentes](./media/troubleshooting-general/access-existing-projects.png)

    3. Ahora verá la lista de los proyectos existentes que fueron creados con la versión anterior de Azure Migrate.

* Si busca un proyecto creado con la versión actual (experiencia nueva), siga estos pasos para buscar el proyecto:

    1. Vaya a [Azure Portal](https://portal.azure.com) y busque **Azure Migrate**.
    2. En el panel de Azure Migrate, vaya a la página **Servidores** en el panel izquierdo y seleccione **Cambiar** en la esquina superior derecha.

       ![Cambio a un proyecto de Azure Migrate existente](./media/troubleshooting-general/switch-project.png)

    3. Seleccione la suscripción y el proyecto de Azure Migrate adecuados.

### <a name="how-do-i-create-a-second-azure-migrate-project"></a>¿Cómo puedo crear un segundo proyecto de Azure Migrate?

Siga estos pasos para crear un nuevo proyecto de Azure Migrate:

1. Vaya a [Azure Portal](https://portal.azure.com) y busque **Azure Migrate**.
2. En el panel de Azure Migrate, vaya a la página **Servidores** en el panel izquierdo y seleccione **Cambiar** en la esquina superior derecha.

   ![Cambio de un proyecto de Azure Migrate](./media/troubleshooting-general/switch-project.png)

3. Para crear un nuevo proyecto, seleccione **Haga clic aquí**.

   ![Creación de un segundo proyecto de Azure Migrate](./media/troubleshooting-general/create-new-project.png)

### <a name="which-azure-geographies-does-azure-migrate-support"></a>¿Qué geografías de Azure admite Azure Migrate?

Consulte las listas para [VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) y para [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

### <a name="how-do-i-delete-azure-migrate-projects-and-associated-log-analytics-workspaces"></a>¿Cómo puedo eliminar proyectos de Azure Migrate y las áreas de trabajo de Log Analytics asociadas?

Cuando se elimina un proyecto de Azure Migrate, se eliminan el proyecto de migración *y* los metadatos sobre las máquinas detectadas. Sin embargo, si ha vinculado un área de trabajo de Log Analytics a la herramienta Server Assessment, el área de trabajo de Log Analytics no se elimina automáticamente. (La misma área de trabajo de Log Analytics se puede usar para varios casos de uso). Si también desea eliminar el área de trabajo Log Analytics, debe hacerlo manualmente:

1. Vaya al área de trabajo de Log Analytics vinculada al proyecto.
     * Si aún no ha eliminado el proyecto de migración, puede encontrar el vínculo al área de trabajo desde la página de información general de Server Assessment en la sección Essentials.

       ![Área de trabajo de LA](./media/troubleshooting-general/loganalytics-workspace.png)

     * Si ya ha eliminado el proyecto de migración, seleccione **Grupos de recursos** en el panel izquierdo de Azure Portal. Vaya al grupo de recursos en el que se creó el área de trabajo y, a continuación, desplácese hasta ella.
2. Siga las instrucciones de [Eliminación de una área de trabajo de Azure Log Analytics con Azure Portal](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace).

### <a name="migration-project-creation-failed-with-a-requests-must-contain-user-identity-headers-error"></a>Se produjo un error al crear el proyecto de migración con el mensaje de error "Las solicitudes deben contener encabezados de identidad del usuario".

Pueden experimentar este problema aquellos usuarios que no tienen acceso al inquilino de Azure Active Directory (Azure AD) de la organización. Cuando se agrega un usuario a un inquilino de Azure AD por primera vez, el usuario recibe una invitación por correo electrónico para unirse al inquilino. Los usuarios deben aceptar la invitación para agregarse correctamente al inquilino. Si no puede ver el correo electrónico, póngase en contacto con un usuario que ya tenga acceso al inquilino y pídale que vuelva a enviarle la invitación mediante los pasos que se especifican en [Reenvío de invitaciones a usuarios invitados](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Cuando reciba el correo electrónico de invitación, debe abrirlo y seleccionar el vínculo para aceptar la invitación. A continuación, debe cerrar sesión en Azure Portal e iniciar sesión de nuevo. (La actualización del explorador no funcionará). A continuación, puede empezar a crear el proyecto de migración.

## <a name="appliance-issues"></a>Problemas del dispositivo

### <a name="deployment-of-an-azure-migrate-appliance-for-vmware-failed-with-a-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry-error"></a>Se produjo un error en la implementación de un dispositivo de Azure Migrate para VMware con el mensaje de error "El archivo de manifiesto proporcionado no es válido: entrada de manifiesto de OVF no válida".

1. Compruebe si el archivo OVA del dispositivo de Azure Migrate se descargó correctamente; para ello, compruebe el valor hash. Para obtener instrucciones, consulte [Preparación de máquinas virtuales de VMware para la evaluación y migración a Azure](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware). Si el valor hash no coincide, vuelva a descargar el archivo OVA e intente de nuevo la implementación.
2. Si la implementación sigue sin funcionar y utiliza el cliente de VMware vSphere para implementar el archivo OVF, intente implementarlo mediante el cliente web de vSphere. Si la implementación sigue sin funcionar, intente usar otro explorador web.
3. Si usa el cliente web de vSphere e intenta implementarlo en vCenter Server 6.5 o 6.7, pruebe a implementar el archivo OVA directamente en el host ESXi siguiendo estos pasos:
   - Conéctese directamente al host ESXi (en lugar de a vCenter Server) mediante el cliente web (https://<*dirección IP del host*>/ui).
   - Vaya a **Home** (Inicio)  > **Inventory** (Inventario).
   - Seleccione **File** (Archivo)  > **Deploy OVF Template** (Implementar plantilla OVF). Vaya al archivo OVA y complete la implementación.
4. Si la implementación sigue sin funcionar, póngase en contacto con el equipo de soporte técnico de Azure Migrate.

### <a name="the-appliance-cant-connect-to-the-internet"></a>El dispositivo no se puede conectar a Internet.

Este comportamiento puede ocurrir cuando la máquina que utiliza está detrás de un servidor proxy. Asegúrese de proporcionar las credenciales de autorización en caso de que el servidor proxy las necesite.
Si va a usar un servidor proxy de firewall basado en direcciones URL para controlar la conectividad saliente, asegúrese de agregar estas direcciones URL necesarias a la lista de direcciones permitidas:

- [Server Assessment para VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-url-access-requirements)
- [Server Assessment para Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#assessment-appliance-url-access)
- [Server Migration para VMware (sin agente)](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-url-access-requirements)
- [Server Migration para VMware (basado en agente)](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#replication-appliance-url-access)
- [Server Migration para Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#migration-hyper-v-host-url-access)

Si usa un servidor proxy de interceptación para conectarse a Internet, debe importar el certificado del servidor proxy en la máquina virtual del dispositivo. Para importar el certificado del servidor proxy, siga los pasos descritos en [Dispositivo de Azure Migrate](https://docs.microsoft.com/azure/migrate/concepts-collector).

### <a name="error-802-date-and-time-synchronization-error"></a>Error 802: error de sincronización de fecha y hora.

El reloj del servidor podría estar desincronizado con la hora actual en más de cinco minutos. Cambie la hora del reloj de la máquina virtual del recopilador para que coincida con la hora actual; para ello, siga estos pasos:

1. Abra un símbolo del sistema de administrador en la máquina virtual.
2. Para comprobar la zona horaria, ejecute **w32tm /tz**.
3. Para sincronizar la hora, ejecute **w32tm /resync**.


###  <a name="connection-failed-error-unabletoconnecttoserver"></a>Error en la conexión. Error: UnableToConnectToServer.

Es posible que no pueda conectarse al servidor de vCenter Server *NombreDelServidor*.com:9443. Los detalles del error indican que no hay ningún punto de conexión en escucha en https://*NombreDelServidor*.com:9443/sdk que pudiera aceptar el mensaje.

En esta situación, compruebe si tiene en ejecución la versión más reciente del dispositivo recopilador. Si no es así, actualice el dispositivo a la [versión más reciente](https://docs.microsoft.com/azure/migrate/concepts-collector).

Si el problema persiste con la última versión, es posible que el equipo recopilador no pueda resolver el nombre del servidor de vCenter Server especificado o que el puerto especificado sea incorrecto. De forma predeterminada, si no se especifica el puerto, el recopilador intentará conectar con el número de puerto 443.

1. Haga ping a *NombreDelServidor*.com desde el equipo recopilador.
2. Si se produce un error en el paso 1, intente conectarse al Servidor vCenter mediante la dirección IP.
3. Identifique el número de puerto correcto para conectarse a vCenter.
4. Compruebe que el servidor vCenter está en funcionamiento.


### <a name="the-appliance-might-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60052"></a>Es posible que el dispositivo no esté registrado correctamente en el proyecto de Azure Migrate (Identificador de error: 60052).

Este error se produce si la cuenta de Azure utilizada para registrar el dispositivo no tiene permisos suficientes. Asegúrese de que la cuenta de usuario de Azure que se ha usado para registrar el dispositivo tiene al menos el permiso de colaborador de la suscripción. [Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) sobre los roles y permisos de Azure necesarios.

### <a name="the-appliance-might-not-be-registered-successfully-to-the-azure-migrate-project-error-id-60039"></a>Es posible que el dispositivo no esté registrado correctamente en el proyecto de Azure Migrate (Identificador de error: 60039).

Es posible que no se encuentre el proyecto de Azure Migrate seleccionado para registrar el dispositivo. En esta situación, se produce un error en el registro. Vaya a Azure Portal y compruebe si el proyecto existe en el grupo de recursos. Si el proyecto no existe, cree un nuevo proyecto de Azure Migrate en el grupo de recursos y vuelva a registrar el dispositivo. [Más información](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool) sobre la creación de un nuevo proyecto de Azure Migrate.

### <a name="an-azure-key-vault-management-operation-failed-error-id-60030-60031"></a>Se produjo un error en una operación de administración de Azure Key Vault (Identificador de error: 60030, 60031).

Asegúrese de que la cuenta de usuario de Azure que se ha usado para registrar el dispositivo tiene al menos el permiso de colaborador de la suscripción. Además, asegúrese de que la cuenta tiene acceso al almacén de claves especificado en el mensaje de error y vuelva a intentar la operación. Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft. [Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) sobre los roles y permisos de Azure necesarios.

### <a name="discovery-couldnt-be-initiated-because-of-an-error-the-operation-failed-for-the-specified-list-of-hosts-or-clusters-error-id-60028"></a>No se pudo iniciar la detección debido a un error. No se pudo realizar la operación para la lista especificada de hosts o clústeres (Identificador de error: 60028).

No se pudo iniciar la detección en los hosts enumerados en el error debido a un problema al acceder o recuperar la información de la máquina virtual. El resto de los hosts se agregaron correctamente. Vuelva a agregar los hosts enumerados en el error mediante la opción **Agregar host**. Si se produce un error de validación, revise las instrucciones de solución para corregir los errores e intente la opción **Guardar e iniciar la detección** de nuevo.

### <a name="an-azure-ad-operation-failed-the-error-occurred-while-creating-or-updating-the-azure-ad-application-error-id-60025"></a>Error en una operación de Azure AD. El error se produjo al crear o actualizar la aplicación de Azure AD (Identificador de error: 60025).

Este error aparece cuando la cuenta de usuario de Azure que se ha utilizado para iniciar la detección es diferente de la que se ha usado para registrar el dispositivo. Puede realizar una de las operaciones siguientes:
1. Asegúrese de que la cuenta de usuario que inicia la detección es la misma que se ha usado para registrar el dispositivo.
1. Proporcione permisos de acceso de la aplicación de AAD a la otra cuenta de usuario para la que se produce el error en la operación de detección.
1. Elimine el grupo de recursos creado anteriormente para el proyecto de Azure Migrate y cree otro para volver a empezar.

[Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) sobre los permisos de la aplicación de AAD.

## <a name="discovery-issues"></a>Problemas de detección

### <a name="i-started-discovery-but-dont-see-the-discovered-vms-on-the-azure-portal-the-server-assessment-and-server-migration-tiles-show-a-status-of-discovery-in-progress"></a>He iniciado la detección, pero no veo las máquinas virtuales detectadas en Azure Portal. Los iconos de **Server Assessment** y **Server Migration** muestran el estado "Detección en curso".
Después de iniciar la detección desde el dispositivo, deje transcurrir un tiempo para que las máquinas detectadas se muestren en Azure Portal. Se tarda aproximadamente 15 minutos en la detección de VMware y unos 2 minutos por cada host agregado para una detección de Hyper-V. Si sigue viendo "Detección en curso" incluso después de estos tiempos de espera, seleccione **Actualizar** en la pestaña **Servidores**. Se debería mostrar el recuento de los servidores detectados en los iconos de Server Assessment y Server Migration.


### <a name="im-using-an-appliance-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-shown-in-the-portal"></a>Utilizo un dispositivo de detección continua en el entorno local, pero las máquinas virtuales que se eliminan del entorno local todavía aparecen en el portal.

Los datos de detección recopilados por el dispositivo tardan hasta 30 minutos en verse reflejados en el portal. Si no ve la información actualizada incluso después de 30 minutos, siga estos pasos para actualizar los datos:

1. Vaya a **Servidores** > **Azure Migrate Server Assessment** y seleccione **Información general**.
2. En **Administrar**, seleccione **Agent Health**.
3. Seleccione **Actualizar agente**. Verá esta opción debajo de la lista de agentes.
4. Espere a que se complete la operación de actualización. Ahora debería ver la información actualizada sobre las máquinas virtuales.

### <a name="i-dont-see-the-latest-information-on-the-on-premise-vms-on-the-portal"></a>No veo la información más reciente sobre las máquinas virtuales locales en el portal.

Los datos de detección recopilados por el dispositivo tardan hasta 30 minutos en verse reflejados en el portal. Si no ve la información actualizada incluso después de 30 minutos, siga estos pasos para actualizar los datos:

1. Vaya a **Servidores** > **Azure Migrate Server Assessment** y seleccione **Información general**.
2. En **Administrar**, seleccione **Agent Health**.
3. Seleccione **Actualizar agente**. Verá esta opción debajo de la lista de agentes.
4. Espere a que se complete la operación de actualización. Ahora debería ver la información actualizada sobre las máquinas virtuales.

### <a name="cant-connect-to-a-host-or-cluster-because-the-server-name-cant-be-resolved-winrm-error-code-0x803381b9-error-id-50004"></a>No se puede conectar a un host o un clúster porque no se puede resolver el nombre del servidor. Código de error WinRM: 0x803381B9 (identificador de error: 50004).
Este error se produce si el servicio Azure DNS del dispositivo no puede resolver el nombre de host o el clúster que ha proporcionado. Si ve este error en el clúster, intente proporcionar el nombre de dominio completo (FQDN) del clúster.

También puede ver este error en los hosts de un clúster. En este caso, el dispositivo puede conectarse al clúster, pero el clúster devuelve nombres de host que no son nombres de dominio completos.

Para resolver este error, actualice el archivo de hosts del dispositivo; para ello, agregue una asignación de la dirección IP y los nombres de host:
1. Abra el Bloc de notas como usuario administrador. A continuación, abra el archivo C:\Windows\System32\Drivers\etc\hosts.
2. Agregue la dirección IP y el nombre de host en una fila. Repita el procedimiento para cada host o clúster en el que observe este error.
3. Guarde el archivo de hosts y ciérrelo.
4. Puede comprobar si el dispositivo puede conectarse a los hosts mediante la aplicación de administración del dispositivo. Después de 30 minutos, debería poder ver la información más reciente sobre estos hosts en Azure Portal.


## <a name="assessment-issues"></a>Problemas de evaluación

### <a name="azure-readiness-issues"></a>Problemas de preparación para Azure

Problema | Corrección
--- | ---
Tipo de arranque no compatible | Azure no admite máquinas virtuales con el tipo de arranque EFI. Se recomienda convertir el tipo de arranque a BIOS antes de ejecutar una migración. <br/><br/>Puede usar Azure Migrate Server Migration para controlar la migración de estas máquinas virtuales. Durante la migración, el tipo de arranque de la máquina virtual se convertirá a BIOS.
Sistema operativo Windows admitido con condiciones | El sistema operativo alcanzó la fecha de finalización del soporte técnico y necesita un contrato de soporte técnico personalizado (CSA) para [recibir soporte técnico en Azure](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar el sistema operativo antes de migrar a Azure.
Sistema operativo Windows no admitido | Azure solo admite [versiones del sistema operativo Windows seleccionadas](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar el sistema operativo de la máquina antes de migrar a Azure.
Sistema operativo Linux aprobado con condiciones | Azure aprueba solo [versiones del sistema operativo Linux seleccionadas](../virtual-machines/linux/endorsed-distros.md). Considere la posibilidad de actualizar el sistema operativo de la máquina antes de migrar a Azure.
Sistema operativo Linux no aprobado | La máquina puede iniciarse en Azure, pero Azure no proporciona compatibilidad con el sistema operativo. Considere la posibilidad de actualizar el sistema operativo a una [versión de Linux aprobada](../virtual-machines/linux/endorsed-distros.md) antes de migrar a Azure.
Sistema operativo desconocido | El sistema operativo de la máquina virtual se especificó como "Otros" en vCenter Server. Este comportamiento impide que Azure Migrate compruebe la preparación para Azure de la máquina virtual. Asegúrese de que Azure [admite](https://aka.ms/azureoslist) el sistema operativo de la máquina antes de migrarla.
Valor de bits de sistema operativo no aprobado | Las máquinas virtuales con un sistema operativo de 32 bits pueden arrancar en Azure, pero se recomienda que actualice el sistema operativo de la máquina virtual a 64 bits antes de migrar a Azure.
Requiere una suscripción de Microsoft Visual Studio | En la máquina se ejecuta un sistema operativo cliente Windows, que solo se admite mediante una suscripción de Visual Studio.
No se encontró ninguna máquina virtual para el rendimiento de almacenamiento requerido | El rendimiento del almacenamiento (operaciones de entrada/salida por segundo [IOPS] y rendimiento) necesario para la máquina supera la compatibilidad con máquinas virtuales de Azure. Reduzca los requisitos de almacenamiento de la máquina antes de realizar la migración.
No se encontró ninguna máquina virtual para el rendimiento de red requerido | El rendimiento de red (entrada/salida) requerido para la máquina excede el soporte técnico de máquina virtual de Azure. Reduzca los requisitos de red de la máquina.
No se encontró ninguna máquina virtual en la ubicación especificada | Utilice una ubicación de destino diferente antes de la migración.
Uno o varios discos no son adecuados | Uno o varios discos conectados a la máquina virtual no cumplen los requisitos de Azure. Azure Migrate: Server Assessment no admite actualmente discos Ultra SSD y evalúa los discos en función de los límites de disco de los discos administrados Premium (32 TB).  Para cada disco conectado a la máquina virtual, asegúrese de que el tamaño del disco es menor que 64 TB (admitido en discos Ultra SSD). Si no es así, reduzca el tamaño del disco antes de migrar a Azure o use varios discos en Azure y [colóquelos juntos](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) para obtener límites de almacenamiento superiores. Asegúrese de que el rendimiento (IOPS y rendimiento) necesario para cada disco sea compatible con los [discos de máquinas virtuales administradas](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) de Azure.
Uno o varios adaptadores de red no son adecuados | Quite los adaptadores de red no utilizados de la máquina antes de realizar la migración.
El recuento del disco supera el límite | Quite los discos no utilizados del equipo antes de la migración.
El tamaño del disco supera el límite | Azure Migrate: Server Assessment no admite actualmente discos Ultra SSD y evalúa los discos en función de los límites de los discos Premium (32 TB). Sin embargo, Azure admite discos con un tamaño de hasta 64 TB (compatible con discos Ultra SSD). Reduzca los discos a menos de 64 TB antes de la migración o use varios discos en Azure y [colóquelos juntos](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) para obtener mayores límites de almacenamiento.
El disco no está disponible en la ubicación especificada | Asegúrese de que el disco está en la ubicación de destino antes de realizar la migración.
El disco no está disponible para la redundancia especificada | El disco debe usar el tipo de almacenamiento de redundancia definido en la configuración de evaluación (LRS de forma predeterminada).
No se pudo determinar la idoneidad del disco debido a un error interno | Intente crear una evaluación para el grupo.
No se encontró ninguna máquina virtual con los núcleos y memoria requeridos | Azure no pudo encontrar un tipo de máquina virtual adecuado. Reduzca la memoria y el número de núcleos de la máquina local antes de realizar la migración.
No se pudo determinar la idoneidad de la máquina virtual debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios discos debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios adaptadores de red debido a un error interno | Intente crear una evaluación para el grupo.

### <a name="i-cant-specify-enterprise-agreement-ea-as-an-azure-offer-in-the-assessment-properties"></a>No puedo especificar el Contrato Enterprise (EA) como una oferta de Azure en las propiedades de la evaluación.
Azure Migrate Server Assessment no admite actualmente precios basados en el Contrato Enterprise (EA). Para solucionar esta limitación, use la opción de **Pago por uso** como la oferta de Azure y use la propiedad **Descuento** para especificar los descuentos personalizados que reciba. [Más información sobre cómo puede personalizar una evaluación](https://aka.ms/migrate/selfhelp/eapricing).

### <a name="why-does-server-assessment-mark-my-linux-vms-conditionally-ready"></a>¿Por qué Server Assessment marca las máquinas virtuales Linux como "Lista con condiciones"?
Hay un problema conocido en Server Assessment que le impide detectar la versión secundaria del sistema operativo Linux instalado en las máquinas virtuales locales (por ejemplo, en el caso de RHEL 6.10, actualmente Server Assessment solo detecta RHEL 6 como la versión del sistema operativo). Puesto que Azure solo aprueba versiones específicas de Linux, las máquinas virtuales Linux actualmente se marcan como listas con condiciones en Server Assessment. Puede determinar si el sistema operativo Linux en ejecución en la máquina virtual local está aprobado en Azure; para ello, consulte la [documentación de soporte técnico de Linux en Azure](https://aka.ms/migrate/selfhost/azureendorseddistros). Una vez haya comprobado que la distribución está aprobada, puede omitir esta advertencia.

### <a name="why-does-the-vm-sku-recommended-by-server-assessment-have-more-cores-and-more-memory-than-whats-allocated-on-premises"></a>¿Por qué la SKU de máquina virtual recomendada por Server Assessment tiene más núcleos y más memoria que la asignación en el entorno local?
La recomendación de SKU de máquina virtual de Server Assessment depende de las propiedades de la evaluación. Puede crear dos tipos de evaluaciones en Server Assessment: *Basada en el rendimiento* y *Como local*. Para obtener evaluaciones en función del rendimiento, Server Assessment tiene en cuenta los datos de uso de las máquinas virtuales en el entorno local (uso de CPU, memoria, disco y red) para determinar la SKU de máquina virtual de destino correcta para las máquinas virtuales en el entorno local. Además, para el ajuste de tamaño basado en el rendimiento, se tiene en cuenta el factor de confort para determinar el uso eficaz. Para el ajuste de tamaño en la opción como en el entorno local, no se tienen en cuenta los datos de rendimiento y se recomienda una SKU de destino en función de lo que hay asignado en el entorno local.

Por ejemplo, supongamos que hay una máquina virtual en el entorno local con 4 núcleos y 8 GB de memoria con un uso de CPU del 50 % y un uso de memoria del 50 % y que se especifica un factor de confort de 1,3 en la evaluación. Si el criterio de ajuste de tamaño de la evaluación es **Como local**, se recomienda una SKU de máquina virtual de Azure con 4 núcleos y 8 gigabytes (GB) de memoria.

Sin embargo, se supone que los criterios de ajuste de tamaño se basan en el rendimiento. En función de la utilización de memoria y CPU efectiva (el 50 % de 4 núcleos * 1,3 = 2,6 núcleos y el 50% de 8 GB de memoria * 1,3 = 5,3 GB de memoria), se recomienda la SKU de máquina virtual más barata de cuatro núcleos (número de núcleos admitidos más cercano) y 8 GB de memoria (tamaño de memoria admitido más cercano). [Más información sobre el ajuste de tamaño de Server Assessment](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing).

### <a name="why-is-the-disk-sku-recommended-by-server-assessment-bigger-than-whats-allocated-on-premises"></a>¿Por qué la SKU de disco recomendada por Server Assessment tiene un tamaño mayor que la asignación en el entorno local?
El ajuste de tamaño del disco en Server Assessment depende de dos propiedades de la evaluación: el criterio de ajuste de tamaño y el tipo de almacenamiento. Si el criterio de ajuste de tamaño es **En función del rendimiento** y el tipo de almacenamiento es **Automático**, se tendrán en cuenta los valores de rendimiento y de IOPS del disco para identificar el tipo de disco de destino (discos HDD estándar, SSD estándar o Premium). Se recomienda una SKU de disco del tipo de disco y esta recomendación tiene en cuenta los requisitos de tamaño del disco en el entorno local. Si el criterio de ajuste de tamaño es **En función del rendimiento** y el tipo de almacenamiento es **Premium**, se recomienda una SKU de disco Premium de Azure en función de los requisitos de IOPS, rendimiento y tamaño del disco en el entorno local. Se usa la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es **Como local** y el tipo de almacenamiento es **HDD estándar**, **SSD estándar** o **Premium**.

Por ejemplo, si tiene un disco en el entorno local con 32 GB de memoria, pero las IOPS de lectura y escritura agregadas son 800 IOPS, Server Assessment recomendará un tipo de disco Premium (debido a los mayores requisitos de IOPS) y, a continuación, una SKU de disco que pueda admitir el tamaño y las IOPS necesarias. La coincidencia más cercana en este ejemplo sería P15 (256 GB, 1100 IOPS). Aunque el tamaño requerido por el disco en el entorno local fue de 32 GB, Server Assessment recomienda un disco con un tamaño mayor debido a los altos requisitos de IOPS del disco en el entorno local.

### <a name="why-does-my-assessment-report-indicate-percentageofcoresutilizedmissing-or-percentageofmemoryutilizedmissing-for-some-vms"></a>¿Por qué mi informe de evaluación indica "PercentageOfCoresUtilizedMissing" o "PercentageOfMemoryUtilizedMissing" para algunas máquinas virtuales?
Estos problemas se muestran cuando el dispositivo de Azure Migrate no puede recopilar los datos de rendimiento de las máquinas virtuales locales. Este comportamiento se puede producir si las máquinas virtuales están apagadas mientras se crea la evaluación (último día/una semana/mes). El dispositivo no puede recopilar datos de rendimiento de una máquina virtual cuando está desactivada. Si solo faltan los contadores de memoria e intenta evaluar máquinas virtuales de Hyper-V, compruebe si tiene habilitada la memoria dinámica en estas máquinas virtuales. Existe un problema conocido en el que un dispositivo de Azure Migrate no puede recopilar los datos de uso de memoria de las máquinas virtuales que no tienen habilitada la memoria dinámica. Este problema solo afecta a las máquinas virtuales de Hyper-V, no a las máquinas virtuales de VMware. Si falta alguno de los contadores de rendimiento, Azure Migrate Server Assessment recurre a la memoria y los núcleos asignados y recomienda un tamaño de máquina virtual correspondiente.

### <a name="is-the-os-license-cost-of-the-vm-included-in-the-compute-cost-estimated-by-server-assessment"></a>¿El costo de la licencia del sistema operativo de la máquina virtual se incluye en el costo de proceso estimado por Server Assessment?
Actualmente, Server Assessment solo tiene en cuenta el costo de la licencia del sistema operativo para las máquinas Windows. Los costos de las licencias del sistema operativo para las máquinas Linux no se tienen en cuenta actualmente.

### <a name="what-impact-do-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>¿Qué impacto tienen el historial de rendimiento y el percentil de uso en las recomendaciones de tamaño?
Estas propiedades solo se aplican al ajuste de tamaño basado en el rendimiento. Server Assessment recopila continuamente datos de rendimiento de las máquinas en el entorno local y los usa para recomendar la SKU de máquina virtual y la SKU de disco de Azure. A continuación se muestra cómo recopila Server Assessment los datos de rendimiento:
- El dispositivo de Azure Migrate genera perfiles del entorno local continuamente con el fin de recopilar datos de uso en tiempo real cada 20 segundos, en el caso de máquinas virtuales VMware, y cada 30 segundos, en el caso de máquinas virtuales Hyper-V.
- El dispositivo acumula muestras cada 20 segundos y cada 30 segundos y crea un único punto de datos cada 10 minutos. Para crear el punto de datos único, el dispositivo selecciona el valor máximo de todas las muestras de cada 20 segundos y cada 30 segundos y lo envía a Azure.
- Cuando se crea una evaluación en Server Assessment en función de la duración del rendimiento y del valor del percentil del historial de rendimiento, se identifica el valor de uso representativo. Por ejemplo, si el historial de rendimiento es de una semana y el percentil de uso es 95, Azure Migrate ordena todos los puntos de muestra de 10 minutos de la última semana en orden ascendente y, a continuación, seleccionará el percentil 95 como valor representativo.
El valor del percentil 95 garantiza que se van a omitir los valores atípicos que se pueden incluir si elige el percentil 99. Si quiere elegir el uso máximo para el período y no desea omitir ningún valor atípico, debe seleccionar el percentil 99 como percentil de uso.

## <a name="dependency-visualization-issues"></a>Problemas de visualización de dependencia

### <a name="i-cant-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>No puedo encontrar la funcionalidad de visualización de dependencias para proyectos de Azure Government.

Azure Migrate depende de Service Map para la funcionalidad de visualización de dependencias. Como Service Map actualmente no está disponible en Azure Government, esta funcionalidad no está disponible en Azure Government.

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>He instalado Microsoft Monitoring Agent (MMA) y el agente de dependencias en las máquinas virtuales locales, pero las dependencias no se muestran en el portal de Azure Migrate.

Después de instalar los agentes, Azure Migrate suele tardar entre 15 y 30 minutos en mostrar las dependencias en el portal. Si ha esperado más de 30 minutos, asegúrese de que MMA puede comunicarse con el área de trabajo de OMS; para ello, siga estos pasos.

Para máquinas virtuales con Windows 10:
1. Vaya al Panel de control e inicie Microsoft Monitoring Agent.
2. En la pestaña **Azure Log Analytics (OMS)** del cuadro de diálogo **Propiedades de Microsoft Monitoring Agent**, asegúrese de que el **Estado** del área de trabajo sea verde.
3. Si el estado no está en verde, intente quitar el área de trabajo y vuelva a agregarla al MMA.

      ![Cuadro de diálogo Propiedades de MMA](./media/troubleshooting-general/mma-status.png)

Para máquinas virtuales con Linux, asegúrese de que se hayan completado correctamente los comandos de instalación de MMA y el agente de dependencias.

### <a name="what-operating-systems-does-mma-support"></a>¿Qué sistemas operativos admite MMA?

 [Esta es una lista de los sistemas operativos Windows compatibles con MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems).
Y [esta es una lista de los sistemas operativos Linux compatibles con MMA](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-operating-systems-does-the-dependency-agent-support"></a>¿Qué sistemas operativos admite el agente de dependencias?

[Esta es una lista de los sistemas operativos Windows compatibles con el agente de dependencias](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems). [Esta es una lista de los sistemas operativos Linux compatibles con el agente de dependencias](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-cant-visualize-dependencies-in-azure-migrate-for-more-than-a-one-hour-duration"></a>No puedo visualizar las dependencias en Azure Migrate durante más de una hora.
En Azure Migrate, puede visualizar las dependencias durante un máximo de una hora. Aunque Azure Migrate le permite volver atrás a una fecha determinada en el último mes, el tiempo máximo durante el que se pueden visualizar las dependencias es de una hora.

Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Sin embargo, puede usar los registros de Azure Monitor para [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

### <a name="i-cant-visualize-dependencies-for-groups-that-have-more-than-10-vms"></a>No puedo visualizar las dependencias de los grupos que tienen más de 10 máquinas virtuales.
Puede [visualizar las dependencias de grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) que tengan un máximo de 10 máquinas virtuales. Si tiene un grupo con más de 10, se recomienda dividirlo en grupos más pequeños y visualizar las dependencias.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>He instalado agentes y he usado la visualización de dependencias para crear grupos. Ahora, después de la conmutación por error, las máquinas muestran la acción "Instalar agente" en lugar de "Ver dependencias".
* Tras una conmutación por error planeada o no planeada, las máquinas locales se desactivan y se activan máquinas equivalentes en Azure. Estos equipos adquieren una dirección MAC diferente. Pueden adquirir una dirección IP distinta en función de si el usuario elige conservar o no una dirección IP local.

  Si las direcciones IP y MAC difieren, Azure Migrate no asocia los equipos locales con ningún dato de dependencias de Service Map. Pide al usuario que instale los agentes en lugar de ver las dependencias.
* Después de la conmutación por error de prueba, las máquinas locales permanecen encendidas según lo previsto. Las máquinas equivalentes que se activan en Azure adquieren una dirección MAC distinta y pueden adquirir una dirección IP diferente. A menos que el usuario bloquee el tráfico saliente del registro de Azure Monitor procedente de estas máquinas, Azure Migrate no asocia las máquinas locales con ningún dato de dependencias de Service Map y solicita al usuario que instale los agentes en lugar de visualizar las dependencias.

## <a name="collect-azure-portal-logs"></a>Recopilación de registros de Azure Portal

**¿Cómo puedo recopilar registros de tráfico de red de Azure Portal?**

1. Abra el explorador, vaya al [portal](https://portal.azure.com) e inicie sesión en él.
2. Presione F12 para iniciar las Herramientas de desarrollo. Si es necesario, desactive la opción **Borrar entradas de navegación**.
3. Seleccione la pestaña **Red** para empezar a capturar el tráfico de red:
   - En Chrome, seleccione **Preserve log** (Conservar registro). La grabación debe iniciarse automáticamente. Un círculo rojo indica que la captura del tráfico está en curso. Si no aparece el círculo rojo, seleccione el círculo negro para comenzar.
   - En Microsoft Edge e Internet Explorer, la grabación debe iniciarse automáticamente. Si no es así, seleccione el botón de reproducción de color verde.
4. Pruebe a reproducir el error.
5. Una vez haya encontrado el error durante la grabación, detenga la grabación y guarde una copia de la actividad grabada:
   - En Chrome, haga clic con el botón derecho y seleccione **Guardar como HAR con contenido**. Esta acción comprime y exporta los registros como un archivo .har.
   - En Microsoft Edge o Internet Explorer, seleccione la opción **Exportar tráfico capturado**. Esta acción comprime y exporta el registro.
6. Seleccione la pestaña **Consola** para ver si hay alguna advertencia o error. Para guardar el registro de la consola:
   - En Chrome, haga clic con el botón derecho en cualquier lugar en el registro de la consola. Seleccione **Save as** (Guardar como) para exportar y comprimir el registro.
   - En Microsoft Edge o Internet Explorer, haga clic con el botón derecho sobre los errores y seleccione **Copiar todo**.
7. Cierre las Herramientas de desarrollo.
