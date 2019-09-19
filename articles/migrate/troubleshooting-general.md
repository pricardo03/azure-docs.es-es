---
title: Solución de problemas de Azure Migrate | Microsoft Docs
description: Se proporciona información general sobre los problemas conocidos del servicio Azure Migrate, además de sugerencias para solucionar los errores comunes.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/17/2019
ms.author: raynew
ms.openlocfilehash: 49c43d393ef0722424088e0073942b56787f1bc7
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71067804"
---
# <a name="troubleshoot-azure-migrate"></a>Solución de problemas de Azure Migrate

[Azure Migrate](migrate-services-overview.md) proporciona un centro de herramientas para la evaluación y la migración, así como ofertas de proveedores de software independientes (ISV). Este artículo le ayuda a solucionar problemas con Azure Migrate, Azure Migrate Server Assessment y Azure Migrate Server Migration.


## <a name="find-a-project"></a>Búsqueda de un programa

Existen [dos versiones](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) de Azure Migrate.


Si ha creado el proyecto de Azure Migrate en la versión actual de Azure Migrate, haga lo siguiente:

1. En [Azure Portal](https://portal.azure.com), busque **Azure Migrate**.
2. En el panel de Azure Migrate, vaya a **Servidores** y seleccione **Cambiar** en la esquina superior derecha.

    ![Cambio a un proyecto de Azure Migrate existente](./media/troubleshooting-general/switch-project.png)

3. Seleccione la suscripción y el proyecto de Azure Migrate adecuados.


Si creó el proyecto en la versión anterior de Azure Migrate, haga lo siguiente:

1. En [Azure Portal](https://portal.azure.com), busque **Azure Migrate**.
2. En el panel de Azure Migrate, si ha creado un proyecto en la versión anterior, aparece un banner que hace referencia a los proyectos anteriores. Seleccione el banner.

    ![Acceso a proyectos existentes](./media/troubleshooting-general/access-existing-projects.png)

3. Revise la lista de proyectos anteriores.


## <a name="create-additional-projects"></a>Creación de proyectos adicionales

Para crear un proyecto de Azure Migrate, siga los pasos que se indican a continuación.

1. En [Azure Portal](https://portal.azure.com), busque **Azure Migrate**.
2. En el panel de Azure Migrate, vaya a **Servidores** y seleccione **Cambiar** en la esquina superior derecha.

   ![Cambio de un proyecto de Azure Migrate](./media/troubleshooting-general/switch-project.png)

3. Para crear un nuevo proyecto, seleccione **Haga clic aquí**.

   ![Creación de un segundo proyecto de Azure Migrate](./media/troubleshooting-general/create-new-project.png)

## <a name="review-supported-geographies"></a>Revisión de las ubicaciones geográficas admitidas

Revise las ubicaciones geográficas admitidas para [VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#azure-migrate-projects) e [Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#azure-migrate-projects).

## <a name="delete-projectsworkspaces"></a>Eliminación de proyectos o áreas de trabajo

Al eliminar proyectos de Azure Migrate y áreas de trabajo de Log Analytics, tenga en cuenta que:

- Cuando se elimina un proyecto de Azure Migrate, se eliminan el proyecto de migración *y* los metadatos sobre las máquinas detectadas.
- Si ha vinculado un área de trabajo de Log Analytics a la herramienta Server Assessment, el área de trabajo no se elimina automáticamente.
- La misma área de trabajo de Log Analytics se puede usar para varios escenarios.
- Si desea eliminar el área de trabajo Log Analytics, debe hacerlo manualmente.


### <a name="delete-a-project"></a>Eliminación de un proyecto

Para eliminar un proyecto en la versión actual de Azure Migrate:

1. Abra el grupo de recursos de Azure en el que se ha creado el proyecto.
2. En la página del grupo de recursos, seleccione **Mostrar tipos ocultos**.
3. Seleccione el proyecto de migración que desea eliminar. El tipo de recurso es Microsoft.Migrate/migrateprojects y lo elimina.

Para eliminar un proyecto en la versión anterior de Azure Migrate:

1. Abra el grupo de recursos de Azure en el que se ha creado el proyecto.
2. Seleccione el proyecto de migración que desea eliminar. El tipo de recurso es un proyecto Migration y lo elimina.


### <a name="delete-a-workspace"></a>Eliminar un área de trabajo

Vaya al área de trabajo de Log Analytics vinculada al proyecto.
* Si no ha eliminado el proyecto de Azure Migrate, puede encontrar el vínculo al área de trabajo en **Essentials** > **Server Assessment** (Información esencial > Evaluación del servidor).
       ![Área de trabajo de Azure Migrate](./media/troubleshooting-general/loganalytics-workspace.png)

     * If you've already deleted the Azure Migrate project, select **Resource Groups** in the left pane of the Azure portal. Locate the workspace in the relevant resources group, and [follow the instructions](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace) to delete it.


## <a name="error-requests-must-contain-user-identity-headers"></a>Error "Las solicitudes deben incluir encabezados de identidad de usuario"

Al crear un proyecto, este error podría indicar que no tiene acceso al inquilino de Azure Active Directory (Azure AD) de la organización.

- Cuando agrega un usuario a un inquilino de Azure AD por primera vez, recibe una invitación por correo electrónico para unirse al inquilino.
- Acepte la invitación para agregarse correctamente al inquilino.
    - Si no puede ver el correo electrónico, póngase en contacto con un usuario con acceso al inquilino y pídale que le [vuelva a enviar la invitación](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).
    - Cuando reciba el correo electrónico de invitación, ábralo y seleccione el vínculo para aceptar la invitación. A continuación, cierre la sesión en Azure Portal e inicie sesión de nuevo. (La actualización del explorador no funcionará). A continuación, puede empezar a crear el proyecto de migración.


## <a name="error-invalid-ovf-manifest-entry"></a>Error de entrada de manifiesto de OVF no válida

Si recibe el error "The provided manifest file is invalid: Invalid OVF manifest entry" (El archivo de manifiesto proporcionado no es válido: entrada de manifiesto de OVF no válida), haga lo siguiente:

1. Compruebe si el archivo OVA del dispositivo de Azure Migrate se descargó correctamente; para ello, compruebe el valor hash. [Más información](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware). Si el valor hash no coincide, vuelva a descargar el archivo OVA e intente de nuevo la implementación.
2. Si la implementación sigue sin funcionar y utiliza el cliente de VMware vSphere para implementar el archivo OVF, intente implementarlo mediante el cliente web de vSphere. Si la implementación sigue sin funcionar, intente usar otro explorador web.
3. Si usa el cliente web de vSphere e intenta implementarlo en vCenter Server 6.5 o 6.7, pruebe a implementar el archivo OVA directamente en el host ESXi:
   - Conéctese directamente al host ESXi (en lugar de vCenter Server) con el cliente web (https://<*dirección IP del host*>/ui).
   - En **Home** > **Inventory** (Inicio > Inventario), seleccione **File** > **Deploy OVF template** (Archivo > Implementar plantilla OVF). Vaya al archivo OVA y complete la implementación.
4. Si la implementación sigue sin funcionar, póngase en contacto con el soporte técnico de Azure Migrate.

## <a name="appliance-cant-connect-to-the-internet"></a>El dispositivo no se puede conectar a Internet.

Esto puede ocurrir si la máquina del dispositivo está detrás de un proxy.

- Asegúrese de proporcionar las credenciales de autorización en caso de que el servidor proxy las necesite.
- Si usa un proxy de firewall basado en dirección URL para controlar la conectividad de salida, agregue estas direcciones URL a una lista de permitidos.

    - [Direcciones URL para la evaluación de VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-url-access-requirements)
    - [Direcciones URL para la evaluación de Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#assessment-appliance-url-access)
    - [Direcciones URL para la migración sin agente de VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-url-access-requirements)
    - [Direcciones URL para la migración basada en agente de VMware](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#replication-appliance-url-access)
    - [Direcciones URL para la migración de Hyper-V](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#migration-hyper-v-host-url-access)

- Si usa un servidor proxy de interceptación para conectarse a Internet, importe el certificado del servidor proxy en la máquina virtual del dispositivo mediante [estos pasos](https://docs.microsoft.com/azure/migrate/concepts-collector).

## <a name="errordatetime-synchronization"></a>Error de sincronización de fecha y hora

Un error sobre la sincronización de fecha y hora (802) indica que el reloj del servidor podría estar desincronizado con la hora actual en más de cinco minutos. Cambie la hora del reloj de la máquina virtual del recopilador para que coincida con la hora actual:

1. Abra un símbolo del sistema de administrador en la máquina virtual.
2. Para comprobar la zona horaria, ejecute **w32tm /tz**.
3. Para sincronizar la hora, ejecute **w32tm /resync**.


## <a name="error-unabletoconnecttoserver"></a>Error: UnableToConnectToServer

Si obtiene este error de conexión, es posible que no pueda conectarse a vCenter Server *NombreDelServidor*.com:9443. Los detalles del error indican que no hay ningún punto de conexión en escucha en https://*NombreDelServidor*.com:9443/sdk que puede aceptar el mensaje.

- Compruebe si tiene en ejecución la versión más reciente del dispositivo. Si no es así, actualice el dispositivo a la [versión más reciente](https://docs.microsoft.com/azure/migrate/concepts-collector).
- Si el problema persiste con la última versión, es posible que el dispositivo no pueda resolver el nombre de vCenter Server especificado o que el puerto especificado sea incorrecto. De forma predeterminada, si no se especifica el puerto, el recopilador intentará conectar con el número de puerto 443.

    1. Haga ping a *NombreDelServidor*.com desde el dispositivo.
    2. Si se produce un error en el paso 1, intente conectarse a vCenter Server mediante la dirección IP.
    3. Identifique el número de puerto correcto para conectarse a vCenter Server.
    4. Compruebe que vCenter Server está en funcionamiento.


## <a name="error-appliance-might-not-be-registered"></a>Error: Es posible que el dispositivo no esté registrado

- Se produce el error 60052: "The appliance might not be registered successfully to the Azure Migrate project" (El dispositivo podría no estar registrado correctamente en el proyecto de Azure Migrate) si la cuenta de Azure usada para registrar el dispositivo no tiene permisos suficientes.
    - Asegúrese de que la cuenta de usuario de Azure que se ha usado para registrar el dispositivo tiene al menos el permiso de colaborador de la suscripción.
    - [Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) sobre los roles y permisos de Azure necesarios.
- Puede producirse el error 60039, "The appliance might not be registered successfully to the Azure Migrate project" (El dispositivo podría no estar registrado correctamente en el proyecto de Azure Migrate) si se produce un error de registro porque no se puede encontrar el proyecto de Azure Migrate utilizado para registrar el dispositivo.
    - En Azure Portal, compruebe si el proyecto existe en el grupo de recursos.
    - Si el proyecto no existe, cree un nuevo proyecto de Azure Migrate en el grupo de recursos y vuelva a registrar el dispositivo. [Obtenga información sobre](https://docs.microsoft.com/azure/migrate/how-to-add-tool-first-time#create-a-project-and-add-a-tool) cómo crear un nuevo proyecto.

## <a name="error-key-vault-management-operation-failed"></a>Error: Error durante la operación de administración de Key Vault

Si recibe el error 60030 o 60031, "An Azure Key Vault management operation failed" (Se ha producido un error en la operación de administración de Azure Key Vault), haga lo siguiente:
- Asegúrese de que la cuenta de usuario de Azure que se ha usado para registrar el dispositivo tiene al menos el permiso de colaborador de la suscripción.
- Asegúrese de que la cuenta tiene acceso al almacén de claves especificado en el mensaje de error y vuelva a intentar la operación.
- Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft.
- [Más información](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) sobre los roles y permisos de Azure necesarios.

## <a name="fix-discovery-couldnt-be-initiated"></a>Solución: No se pudo iniciar la detección.

Error 60028: "Discovery couldn't be initiated because of an error. The operation failed for the specified list of hosts or clusters" (No se pudo iniciar la detección debido a un error. Se produjo un error en la operación para la lista especificada de hosts o clústeres) indica que no se pudo iniciar la detección en los hosts listados en el error debido a un problema de acceso o recuperación de la información de la máquina virtual. El resto de los hosts se agregaron correctamente.

- Vuelva a agregar los hosts enumerados en el error mediante la opción **Agregar host**.
- Si se produce un error de validación, revise las instrucciones de solución para corregir los errores e intente la opción **Guardar e iniciar la detección** de nuevo.

## <a name="fix-azure-ad-operation-failed-60025"></a>Solución: Error en una operación de Azure AD (60025).

El error 60025: "An Azure AD operation failed. The error occurred while creating or updating the Azure AD application" (Error en una operación de Azure AD. El error ha ocurrido al crear o actualizar la aplicación de Azure AD) aparece cuando la cuenta de usuario de Azure que se ha utilizado para iniciar la detección es diferente de la que se ha usado para registrar el dispositivo. Realice una de las operaciones siguientes:

- Asegúrese de que la cuenta de usuario que inicia la detección es la misma que se ha usado para registrar el dispositivo.
- Proporcione permisos de acceso de la aplicación de Azure Active Directory a la otra cuenta de usuario para la que se produce el error en la operación de detección.
- Elimine el grupo de recursos creado previamente para el proyecto de Azure Migrate. Cree otro grupo de recursos para empezar de nuevo.
- [Aprenda más](https://docs.microsoft.com/azure/migrate/migrate-appliance#appliance-deployment-requirements) sobre los permisos de aplicación de Azure Active Directory.


## <a name="discovered-vms-not-in-portal"></a>Máquinas virtuales detectadas no en el portal

Si inicia la detección para que **Server Assessment** y **Server Migration** muestren la **detección en curso**, pero aún no ve las máquinas virtuales en el portal, tenga en cuenta lo siguiente:

- Después de iniciar la detección desde el dispositivo, la detección de las máquinas virtuales de VMware tarda unos 15 minutos y aproximadamente dos minutos para cada host agregado para la detección de máquinas virtuales de Hyper-V.
- Si sigue viendo **Detección en curso** incluso después de estos períodos de espera, seleccione **Actualizar** en la pestaña **Servidores**. Se debería mostrar el recuento de los servidores detectados en **Server Assessment** y **Server Migration**.


## <a name="deleted-vms-in-the-portal"></a>Máquinas virtuales eliminadas en el portal

Si ha implementado un dispositivo que detecta continuamente su entorno local, pero las máquinas virtuales eliminadas siguen mostrándose en el portal, tenga en cuenta lo siguiente:  

- Los datos de detección recopilados por el dispositivo tardan hasta 30 minutos en verse reflejados en el portal.
- Si no ve la información actualizada después de 30 minutos, siga estos pasos para actualizar los datos:

    1. En **Servidores** > **Azure Migrate Server Assessment**, seleccione **Información general**.
    2. En **Administrar**, seleccione **Agent Health**.
    3. Seleccione **Actualizar agente**.
    1. Espere a que se complete la operación de actualización. Ahora debería ver la información actualizada.

## <a name="vm-information-isnt-in-the-portal"></a>La información de la máquina virtual no está en el portal

- Los datos de detección recopilados por el dispositivo tardan hasta 30 minutos en verse reflejados en el portal.
- Si no ve la información actualizada después de 30 minutos, siga estos pasos para actualizar los datos:

    1. En **Servidores** > **Azure Migrate Server Assessment**, seleccione **Información general**.
    2. En **Administrar**, seleccione **Agent Health**.
    3. Seleccione **Actualizar agente**.
    1. Espere a que se complete la operación de actualización. Ahora debería ver la información actualizada.


## <a name="fix-cant-connect-to-host-or-cluster"></a>Solución: No se puede conectar al host o clúster

Se produce el error 50004: No se puede conectar a un host o un clúster porque no se puede resolver el nombre del servidor. Código de error WinRM: 0x803381B9" si el servicio Azure DNS del dispositivo no puede resolver el nombre de host o el clúster que ha proporcionado.

- Si ve este error en el clúster, añada el nombre de dominio completo en el clúster.
- También puede ver este error en los hosts de un clúster. Esto indica que el dispositivo puede conectarse al clúster, pero el clúster devuelve nombres de host que no son nombres de dominio completos. Para resolver este error, actualice el archivo de hosts del dispositivo; para ello, agregue una asignación de la dirección IP y los nombres de host:
    1. Abra el Bloc de notas como administrador.
    2. Abra el archivo C:\Windows\System32\Drivers\etc\hosts.
    3. Agregue la dirección IP y el nombre de host en una fila. Repita el procedimiento para cada host o clúster en el que observe este error.
    4. Guarde el archivo de hosts y ciérrelo.
    5. Compruebe si el dispositivo puede conectarse a los hosts mediante la aplicación de administración del dispositivo. Después de 30 minutos, debería ver la información más reciente sobre estos hosts en Azure Portal.



## <a name="fix-assessment-readiness"></a>Revisión de la preparación de la evaluación

Revise los problemas de preparación de la evaluación como se indica a continuación:

**Problema** | **Revisión**
--- | ---
Tipo de arranque no compatible | Azure no admite máquinas virtuales con el tipo de arranque EFI. Se recomienda convertir el tipo de arranque a BIOS antes de ejecutar una migración. <br/><br/>Puede usar Azure Migrate Server Migration para controlar la migración de estas máquinas virtuales. Durante la migración, el tipo de arranque de la máquina virtual se convertirá a BIOS.
Sistemas operativos Windows condicionalmente compatibles | El sistema operativo alcanzó la fecha de finalización del soporte técnico y necesita un contrato de soporte técnico personalizado (CSA) para [recibir soporte técnico en Azure](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar antes de migrar a Azure.
Sistemas operativos Windows no compatibles | Azure solo admite [versiones del sistema operativo Windows seleccionadas](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar la máquina antes de migrar a Azure.
Sistema operativo Linux aprobado con condiciones | Azure aprueba solo [versiones del sistema operativo Linux seleccionadas](../virtual-machines/linux/endorsed-distros.md). Considere la posibilidad de actualizar la máquina antes de migrar a Azure.
Sistema operativo Linux no aprobado | La máquina puede iniciarse en Azure, pero Azure no proporciona compatibilidad con el sistema operativo. Considere la actualización a una [versión de Linux aprobada](../virtual-machines/linux/endorsed-distros.md) antes de migrar a Azure.
Sistema operativo desconocido | El sistema operativo de la máquina virtual se especificó como "Otros" en vCenter Server. Este comportamiento impide que Azure Migrate compruebe la preparación para Azure de la máquina virtual. Asegúrese de que Azure [admite](https://aka.ms/azureoslist) el sistema operativo antes de migrarla.
Versión de bits no admitida | Las máquinas virtuales con un sistema operativo de 32 bits pueden arrancar en Azure, pero se recomienda que actualice a 64 bits antes de migrar a Azure.
Requiere una suscripción de Microsoft Visual Studio | En la máquina se ejecuta un sistema operativo cliente Windows, que solo se admite mediante una suscripción de Visual Studio.
No se encontró ninguna máquina virtual para el rendimiento de almacenamiento requerido | El rendimiento del almacenamiento (operaciones de entrada/salida por segundo [IOPS] y rendimiento) necesario para la máquina supera la compatibilidad con máquinas virtuales de Azure. Reduzca los requisitos de almacenamiento de la máquina antes de realizar la migración.
No se encontró ninguna máquina virtual para el rendimiento de red requerido | El rendimiento de red (entrada/salida) requerido para la máquina excede el soporte técnico de máquina virtual de Azure. Reduzca los requisitos de red de la máquina.
No se encontró ninguna máquina virtual en la ubicación especificada | Utilice una ubicación de destino diferente antes de la migración.
Uno o varios discos no son adecuados | Uno o varios discos conectados a la máquina virtual no cumplen los requisitos de Azure.<br/><br/> Azure Migrate: Server Assessment no admite actualmente discos Ultra SSD y evalúa los discos en función de los límites de disco de los discos administrados Premium (32 TB).<br/><br/> Para cada disco conectado a la máquina virtual, asegúrese de que el tamaño del disco es menor que 64 TB (compatible con discos SSD Ultra).<br/><br/> Si no lo es, reduzca el tamaño de los discos antes de la migración o bien use varios discos en Azure y [colóquelos juntos](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) para obtener mayores límites de almacenamiento. Asegúrese de que el rendimiento (IOPS y rendimiento) necesario para cada disco sea compatible con los [discos de máquinas virtuales administradas](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) de Azure.
Uno o varios adaptadores de red no son adecuados | Quite los adaptadores de red no utilizados de la máquina antes de realizar la migración.
El recuento del disco supera el límite | Quite los discos no utilizados del equipo antes de la migración.
El tamaño del disco supera el límite | Azure Migrate: Server Assessment no admite actualmente discos Ultra SSD y evalúa los discos en función de los límites de los discos Premium (32 TB).<br/><br/> Sin embargo, Azure admite discos con un tamaño de hasta 64 TB (compatible con discos Ultra SSD). Reduzca los discos a menos de 64 TB antes de la migración o use varios discos en Azure y [colóquelos juntos](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#disk-striping) para obtener mayores límites de almacenamiento.
El disco no está disponible en la ubicación especificada | Asegúrese de que el disco está en la ubicación de destino antes de realizar la migración.
El disco no está disponible para la redundancia especificada | El disco debe usar el tipo de almacenamiento de redundancia definido en la configuración de evaluación (LRS de forma predeterminada).
No se pudo determinar la idoneidad del disco debido a un error interno | Intente crear una evaluación para el grupo.
No se encontró ninguna máquina virtual con los núcleos y memoria requeridos | Azure no pudo encontrar un tipo de máquina virtual adecuado. Reduzca la memoria y el número de núcleos de la máquina local antes de realizar la migración.
No se pudo determinar la idoneidad de la máquina virtual debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios discos debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios adaptadores de red debido a un error interno | Intente crear una evaluación para el grupo.

## <a name="linux-vms-are-conditionally-ready"></a>Las máquinas virtuales Linux están "condicionalmente preparadas"

Server Assessment marca las máquinas virtuales Linux como "condicionalmente preparadas" debido a una brecha conocida en Server Assessment.

- La brecha evita que detecte la versión secundaria del sistema operativo Linux instalada en las máquinas virtuales locales.
- Por ejemplo, para RHEL 6.10, Server Assessment actualmente solo detecta RHEL 6 como la versión del sistema operativo.
-  Puesto que Azure solo aprueba versiones específicas de Linux, las máquinas virtuales Linux actualmente se marcan como listas con condiciones en Server Assessment.
- Puede determinar si el sistema operativo Linux en ejecución en la máquina virtual local está aprobado en Azure; para ello, consulte [Soporte técnico de Linux en Azure](https://aka.ms/migrate/selfhost/azureendorseddistros).
-  Una vez haya comprobado que la distribución está aprobada, puede omitir esta advertencia.

## <a name="azure-skus-exceed-on-premises-sizing"></a>Las SKU de Azure superan el tamaño local

Azure Migrate Server Assessment podría recomendar las SKU de máquinas virtuales de Azure con más núcleos y memoria que la asignación local actual según el tipo de evaluación:


- La recomendación de las SKU de máquinas virtuales depende de las propiedades de la evaluación.
- Esto se ve afectado por el tipo de evaluación que realiza en Server Assessment: *Basado en el rendimiento* y *Como local*.
- Para obtener evaluaciones en función del rendimiento, Server Assessment tiene en cuenta los datos de uso de las máquinas virtuales en el entorno local (uso de CPU, memoria, disco y red) para determinar la SKU de máquina virtual de destino correcta para las máquinas virtuales en el entorno local. También agrega un factor de confort al determinar el uso efectivo.
- Para el ajuste de tamaño en la opción como en el entorno local, no se tienen en cuenta los datos de rendimiento y se recomienda una SKU de destino en función de la asignación local.

Para mostrar cómo esto puede afectar a las recomendaciones, veamos un ejemplo:

Tenemos una máquina virtual local con cuatro núcleos y 8 GB de memoria, con un uso de CPU del 50 % y un uso de memoria del 50 % y un factor de confort específico de 1,3.

-  Si la evaluación es **Como local**, se recomienda una SKU de máquina virtual de Azure con 4 núcleos y 8 GB de memoria.
- Si la evaluación está basada en el rendimiento, en función de la utilización de memoria y CPU efectiva (el 50 % de 4 núcleos * 1,3 = 2,6 núcleos y el 50 % de 8 GB de memoria * 1,3 = 5,3 GB de memoria), se recomienda la SKU de máquina virtual más barata de cuatro núcleos (número de núcleos admitidos más cercano) y 8 GB de memoria (tamaño de memoria admitido más cercano).
- [Más información](concepts-assessment-calculation.md#sizing) sobre el ajuste de tamaño de evaluación.

## <a name="azure-disk-skus-bigger-than-on-premises"></a>SKU de discos de Azure de mayor tamaño que el local

Azure Migrate Server Assessment podría recomendar un disco más grande según el tipo de evaluación.
- El ajuste de tamaño del disco en Server Assessment depende de dos propiedades de la evaluación: el criterio de ajuste de tamaño y el tipo de almacenamiento.
- Si el criterio de ajuste de tamaño es **En función del rendimiento** y el tipo de almacenamiento es **Automático**, se tendrán en cuenta los valores de rendimiento y de IOPS del disco para identificar el tipo de disco de destino (discos HDD estándar, SSD estándar o Premium). Se recomienda una SKU de disco del tipo de disco y esta recomendación tiene en cuenta los requisitos de tamaño del disco en el entorno local.
- Si el criterio de ajuste de tamaño es **En función del rendimiento** y el tipo de almacenamiento es **Premium**, se recomienda una SKU de disco Premium de Azure en función de los requisitos de IOPS, rendimiento y tamaño del disco en el entorno local. Se usa la misma lógica para ajustar el tamaño del disco cuando el criterio de ajuste de tamaño es **Como local** y el tipo de almacenamiento es **HDD estándar**, **SSD estándar** o **Premium**.

Por ejemplo, si tiene un disco en el entorno local con 32 GB de memoria, pero las IOPS de lectura y escritura agregadas son 800 IOPS, Server Assessment recomienda un tipo de disco Premium (debido a los mayores requisitos de IOPS) y, a continuación, una SKU de disco que pueda admitir el tamaño y las IOPS necesarias. La coincidencia más cercana en este ejemplo sería P15 (256 GB, 1100 IOPS). Aunque el tamaño requerido por el disco en el entorno local fue de 32 GB, Server Assessment recomienda un disco con un tamaño mayor debido a los altos requisitos de IOPS del disco en el entorno local.

## <a name="fix-percentage-of-utilized-core-or-memory-missing"></a>Solución: Porcentaje de núcleos usados o memoria que falta

Los informes de Server Assessment "PercentageOfCoresUtilizedMissing" o "PercentageOfMemoryUtilizedMissing" cuando el dispositivo de Azure Migrate no puede recopilar datos de rendimiento de las máquinas virtuales locales pertinentes.

- Esto puede ocurrir si las máquinas virtuales están desactivadas durante la duración de la evaluación. El dispositivo no puede recopilar datos de rendimiento de una máquina virtual cuando está desactivada.
- Si solo faltan los contadores de memoria e intenta evaluar máquinas virtuales de Hyper-V, compruebe si tiene habilitada la memoria dinámica en estas máquinas virtuales. Existe un problema conocido solo para las máquinas virtuales de Hyper-V, en el que un dispositivo de Azure Migrate no puede recopilar los datos de uso de memoria de las máquinas virtuales que no tienen habilitada la memoria dinámica.
- Si falta alguno de los contadores de rendimiento, Azure Migrate Server Assessment recurre a la memoria y los núcleos asignados y recomienda un tamaño de máquina virtual correspondiente.

## <a name="is-the-vm-os-license-cost-included-in-cost-assessment"></a>¿El costo de la licencia del sistema operativo de la máquina virtual se incluye en la evaluación de costos?

Azure Migrate Server Assessment solo tiene en cuenta el costo de la licencia del sistema operativo para las máquinas Windows. Los costos de las licencias para las máquinas Linux no se tienen en cuenta actualmente.

## <a name="performance-history-and-percentile-use"></a>Historial de rendimiento y uso de percentil

Estas propiedades solo se aplican al ajuste de tamaño basado en el rendimiento en Azure Migrate Server Assessment.

Server Assessment recopila continuamente datos de rendimiento de las máquinas en el entorno local y los usa para recomendar la SKU de máquina virtual y la SKU de disco de Azure. A continuación se muestra cómo recopila Server Assessment los datos de rendimiento:
- El dispositivo de Azure Migrate genera perfiles del entorno local continuamente con el fin de recopilar datos de uso en tiempo real cada 20 segundos, en el caso de máquinas virtuales VMware, y cada 30 segundos, en el caso de máquinas virtuales Hyper-V.
- El dispositivo acumula muestras cada 20 o 30 segundos y crea un único punto de datos cada 10 minutos. Para crear el punto de datos único, el dispositivo selecciona el valor máximo de todas las muestras de cada 20 segundos y cada 30 segundos y lo envía a Azure.
- Cuando se crea una evaluación en Server Assessment en función de la duración del rendimiento y del valor del percentil del historial de rendimiento, se identifica el valor de uso representativo. Por ejemplo, si el historial de rendimiento es de una semana y el percentil de uso es 95, Azure Migrate ordena todos los puntos de muestra de 10 minutos de la última semana en orden ascendente y, a continuación, seleccionará el percentil 95 como valor representativo.
- El valor del percentil 95 garantiza que se van a omitir los valores atípicos que se pueden incluir si elige el percentil 99.
- Si quiere elegir el uso máximo para el período y no desea omitir ningún valor atípico, debe seleccionar el percentil 99 como percentil de uso.



## <a name="i-cant-find-dependency-visualization-for-azure-government"></a>No puedo encontrar la visualización de dependencias en Azure Government.

Azure Migrate depende de Service Map para la funcionalidad de visualización de dependencias. Como Service Map actualmente no está disponible en Azure Government, esta funcionalidad no está disponible en Azure Government.

## <a name="dependencies-dont-show-after-installing-agents"></a>Las dependencias no se muestran después de instalar los agentes


Después de instalar los agentes de visualización de dependencias en las máquinas virtuales locales, Azure Migrate suele tardar entre 15 y 30 minutos en mostrar las dependencias en el portal. Si ha esperado durante más de 30 minutos, asegúrese de que Microsoft Monitoring Agent (MMA) puede conectarse al área de trabajo de Log Analytics.

Para las máquinas virtuales de Windows:
1. En el Panel de control, inicie MMA.
2. En **Propiedades de Microsoft Monitoring Agent** > **Azure Log Analytics (OMS)** , asegúrese de que el **estado** del área de trabajo esté en verde.
3. Si el estado no está en verde, intente quitar el área de trabajo y vuelva a agregarla a MMA.

      ![Cuadro de diálogo Propiedades de MMA](./media/troubleshooting-general/mma-status.png)

Para las máquinas virtuales Linux, asegúrese de que se hayan completado correctamente los comandos de instalación de MMA y el agente de dependencias.

## <a name="supported-mma-os"></a>Sistema operativo compatible con MMA

 Revise los sistemas operativos [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) y [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems) admitidos.

## <a name="supported-dependency-agent-os"></a>Sistema operativo del agente de dependencia admitido

Revise los sistemas operativos [Windows y Linux](../azure-monitor/insights/vminsights-enable-overview.md#supported-operating-systems) admitidos.

## <a name="dependencies-for-more-than-an-hour"></a>Dependencias de más de una hora

Aunque Azure Migrate le permite volver atrás a una fecha determinada en el último mes, el tiempo máximo durante el que se pueden visualizar las dependencias es de una hora.

Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora.

Sin embargo, puede usar los registros de Azure Monitor para [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

## <a name="i-cant-visualize-dependencies-for-groups-with-more-than-10-vms"></a>No puedo visualizar las dependencias para grupos con más de 10 máquinas virtuales.

En Azure Migrate Server Assessment, puede [visualizar las dependencias para grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) que tengan un máximo de diez máquinas virtuales. En el caso de los grupos más grandes, se recomienda dividir las máquinas virtuales en grupos más pequeños para visualizar las dependencias.

## <a name="machines-show-install-agent-not-view-dependencies"></a>Las máquinas muestran "Instalar agente" no "Ver dependencias".

Después de migrar las máquinas con la visualización de dependencias habilitada en Azure, es posible que las máquinas muestren la acción "Instalar agente" en lugar de "Ver dependencias" debido al comportamiento siguiente:


- Después de la migración a Azure, las máquinas locales están desconectadas y las máquinas virtuales equivalentes trabajan en Azure. Estos equipos adquieren una dirección MAC diferente.
- Las máquinas también pueden adquirir una dirección IP distinta en función de si el se elige conservar o no la dirección IP local.
- Si las direcciones IP y MAC difieren de las locales, Azure Migrate no asocia las máquinas locales con ningún dato de dependencias de Service Map. En este caso, se mostrará la opción para instalar el agente en lugar de ver las dependencias.
- Después de una migración de prueba a Azure, las máquinas locales permanecen encendidas según lo previsto. Las máquinas equivalentes que se activan en Azure adquieren una dirección MAC distinta y pueden adquirir una dirección IP diferente. A menos que bloquee el tráfico saliente del registro de Azure Monitor procedente de estas máquinas, Azure Migrate no asociará las máquinas locales con ningún dato de dependencias de Service Map y, por tanto, mostrará la opción de instalar los agentes en lugar de visualizar las dependencias.


## <a name="collect-network-traffic-logs-in-portal"></a>Recopilación de registros de tráfico de red en el portal

Recopile los registros como se indica a continuación:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
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
