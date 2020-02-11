---
title: Preparación de máquinas virtuales de VMware para la evaluación y migración con Azure Migrate
description: Aprenda a prepararse para la evaluación y migración de máquinas virtuales de VMware con Azure Migrate.
ms.topic: tutorial
ms.date: 11/19/2019
ms.custom: mvc
ms.openlocfilehash: f00d5ba4841427098b0ab79ad1930e357008b6e0
ms.sourcegitcommit: f0f73c51441aeb04a5c21a6e3205b7f520f8b0e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77030802"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>Preparación de máquinas virtuales de VMware para la evaluación y migración a Azure

Este artículo ayuda a preparar la valoración y migración de máquinas virtuales de VMware locales a Azure mediante [Azure Migrate](migrate-services-overview.md).

[Azure Migrate](migrate-overview.md) proporciona un centro de herramientas que le ayuda a detecta las aplicaciones, la infraestructura y las cargas de trabajo, a evaluarlas y a migrarlas a Microsoft Azure. Este centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros.


Este tutorial es el primero de una serie que muestra cómo evaluar y migrar máquinas virtuales de VMware. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Preparar Azure para trabajar con Azure Migrate.
> * Preparar VMware para la valoración de máquinas virtuales.
> * Preparar VMware para la migración de máquinas virtuales.

> [!NOTE]
> Los tutoriales muestran la ruta de implementación más sencilla para un escenario. Resultan útiles cuando aprende a configurar una implementación y como una prueba de concepto rápida. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para instrucciones detalladas, revise los procedimientos para la valoración y la migración de VMware.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prepare-azure"></a>Preparación de Azure

Necesita estos permisos.

**Task** | **Permisos**
--- | ---
**Crear un proyecto de Azure Migrate** | La cuenta de Azure necesita permisos para crear un proyecto.
**Registrar el dispositivo de Azure Migrate** | Azure Migrate usa un dispositivo de Azure Migrate ligero para evaluar las máquinas virtuales de VMware con Azure Migrate Server Assessment y para ejecutar [migración sin agente](server-migrate-overview.md) de máquinas virtuales de VMware con la migración de Azure Migrate Server. Este dispositivo realiza la detección de máquinas virtuales y envía sus metadatos y sus datos de rendimiento a Azure Migrate.<br/><br/>Durante el registro del dispositivo, los siguientes proveedores de recursos se registran con la suscripción elegida en el dispositivo: Microsoft.OffAzure, Microsoft.Migrate y Microsoft.KeyVault. Al registrar un proveedor de recursos se configura la suscripción para que funcione con este. Para registrar los proveedores de recursos debe tener el rol colaborador o propietario de la suscripción.<br/><br/> Como parte del proceso de incorporación, Azure Migrate crea dos aplicaciones de Azure Active Directory (Azure AD):<br/> - La primera aplicación se usa para la comunicación (autenticación y autorización) entre los agentes que se ejecutan en el dispositivo con sus servicios respectivos que se ejecutan en Azure. Esta aplicación no tiene privilegios para realizar llamadas a ARM ni acceso RBAC en ningún recurso.<br/> - La segunda aplicación se usa exclusivamente para acceder a la instancia de KeyVault creada en la suscripción del usuario para la migración sin agente. Se proporciona con un acceso RBAC a Azure Key Vault (la instancia creada en el inquilino del cliente) cuando se inicia la detección desde el dispositivo.
**Crear un almacén de claves** | Cuando se migran máquinas virtuales de VMware con Azure Migrate Server Migration, Azure Migrate crea un almacén de claves para administrar las claves de acceso a la cuenta de almacenamiento de replicación de la suscripción. Para crear el almacén, necesita permisos de asignación de roles en el grupo de recursos en el que reside el proyecto de Azure Migrate.






### <a name="assign-permissions-to-create-project"></a>Asignación de permisos para crear un proyecto

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.

### <a name="assign-permissions-to-register-the-appliance"></a>Asignación de permisos para registrar el dispositivo

Para registrar el dispositivo, asigne los permisos para Azure Migrate de modo que se creen las aplicaciones de Azure AD durante el registro del dispositivo. Los permisos se pueden asignar mediante uno de los métodos siguientes:

- Un administrador de inquilinos o administrador global puede conceder permisos a los usuarios del inquilino para crear y registrar aplicaciones de Azure AD.
- Un administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.

> [!NOTE]
> - Las aplicaciones no tienen otros permisos de acceso en la suscripción distintos de los descritos anteriormente.
> - Solo necesita estos permisos al registrar un nuevo dispositivo. Puede quitar los permisos una vez configurado el dispositivo.


#### <a name="grant-account-permissions"></a>Concesión de permisos de cuenta

El administrador de inquilinos o administrador global puede conceder permisos como se indica:

1. En Azure AD, el administrador de inquilinos o global debe ir a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
2. El administrador debe establecer **Registros de aplicaciones** en **Sí**. Se trata de una configuración predeterminada que no es confidencial. [Más información](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

    ![Permisos de Azure AD](./media/tutorial-prepare-vmware/aad.png)



#### <a name="assign-application-developer-role"></a>Asignación del rol de desarrollador de aplicaciones

El administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones a una cuenta. [Más información](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

### <a name="assign-permissions-to-create-a-key-vault"></a>Asignación de permisos para crear un almacén de claves

Para habilitar Azure Migrate para crear un almacén de claves, asigne los permisos de la siguiente manera:

1. En el grupo de recursos de Azure Portal, seleccione **Control de acceso (IAM**).
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.

    - Para ejecutar Server Assessment, son suficientes los permisos de **colaborador**.
    - Para ejecutar la migración de un servidor sin agente, debe disponer de permisos de **propietario** (o **colaborador** y **administrador de acceso de usuario**).

3. Si no tiene los permisos necesarios, pídaselos al propietario del grupo de recursos.



## <a name="prepare-for-vmware-vm-assessment"></a>Preparación para la evaluación de máquinas virtuales de VMware

Para preparar la valoración de máquinas virtuales de VMware, tiene que:

- **Comprobar la configuración de VMware**. Asegúrese de que las máquinas virtuales y vCenter Server que desea migrar cumplan los requisitos.
- **Configurar una cuenta de valoración**. Azure Migrate tiene que acceder a vCenter Server para detectar máquinas virtuales para la valoración.
- **Comprobar los requisitos de los dispositivos**. Compruebe los requisitos de implementación para el dispositivo de Azure Migrate utilizado para la valoración.

### <a name="verify-vmware-settings"></a>Comprobación de la configuración de VMware

1. [Compruebe](migrate-support-matrix-vmware.md#vmware-requirements) los requisitos del servidor de VMware para la valoración.
2. [Asegúrese](migrate-support-matrix-vmware.md#port-access) de que los puertos necesarios están abiertos en vCenter Server.
3. En vCenter Server, asegúrese de que la cuenta tiene permisos para crear una máquina virtual mediante un archivo OVA. Puede implementar el dispositivo de Azure Migrate como una máquina virtual de VMware mediante un archivo OVA.


### <a name="set-up-an-account-for-assessment"></a>Configuración de una cuenta para la evaluación

Azure Migrate necesita acceder a vCenter Server para detectar máquinas virtuales para su evaluación y migración sin agente.

- Si tiene previsto detectar aplicaciones o visualizar dependencias sin agente, cree una cuenta de vCenter Server con acceso de solo lectura junto con los privilegios habilitados para **Virtual machines** > **Guest Operations** (Máquinas virtuales > Operaciones de invitado).

  ![Privilegios de cuenta de vCenter Server](./media/tutorial-prepare-vmware/vcenter-server-permissions.png)

- Si no tiene previsto realizar la detección de aplicaciones y la visualización de dependencias sin agente, configure una cuenta de solo lectura para vCenter Server.

### <a name="verify-appliance-settings-for-assessment"></a>Comprobación de la configuración del dispositivo para su evaluación

Antes de configurar el dispositivo de Azure Migrate y comenzar la evaluación en el siguiente tutorial, prepare la implementación del dispositivo.

1. [Compruebe](migrate-appliance.md#appliance---vmware) los requisitos del dispositivo para las máquinas virtuales de VMware.
2. [Revise](migrate-appliance.md#url-access) las direcciones URL de Azure a las que tendrá acceso el dispositivo. Si usa un proxy de firewall basado en direcciones URL, asegúrese de que permiten el acceso a las URL necesarias.
3. [Revise](migrate-appliance.md#collected-data---vmware) lo que el dispositivo va a recopilar durante la detección y la evaluación.
4. [Tenga en cuenta](migrate-support-matrix-vmware.md#port-access) los requisitos de acceso a puertos del dispositivo.




## <a name="prepare-for-agentless-vmware-migration"></a>Preparación para la migración de VMware sin agente

Revise los requisitos para la migración sin agente de máquinas virtuales de VMware.

1. [Revise](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) los requisitos del servidor de VMware y los [permisos](migrate-support-matrix-vmware-migration.md#agentless-vmware-servers) que necesita Azure Migrate para acceder a vCenter Server para la migración sin agente mediante Server Migration de Azure Migrate.
2. [Revise](migrate-support-matrix-vmware-migration.md#agentless-vmware-vms) los requisitos de las máquinas virtuales de VMware que desee migrar a Azure mediante la migración sin agente.
4. [Revise](migrate-support-matrix-vmware-migration.md#agentless-azure-migrate-appliance) los requisitos para usar el dispositivo de Azure Migrate para la migración sin agente.
5. Tenga en cuenta el [acceso a direcciones URL](migrate-appliance.md#url-access) y el [acceso a puertos](migrate-support-matrix-vmware-migration.md#agentless-ports) necesarios para la migración sin agente.


## <a name="prepare-for-agent-based-vmware-migration"></a>Preparación para la migración de VMware basada en agente

Revise los requisitos para la [migración basada en agente](server-migrate-overview.md) de máquinas virtuales de VMware.

1. [Revise](migrate-support-matrix-vmware-migration.md#agent-based-vmware-servers) los requisitos del servidor de VMware y los permisos que necesita Azure Migrate para acceder a vCenter Server para la migración con agente mediante Server Migration de Azure Migrate.
2. [Revise](migrate-support-matrix-vmware-migration.md#agent-based-vmware-vms) los requisitos de las máquinas virtuales de VMware que desea migrar a Azure mediante la migración basada en agente, incluida la instalación de Mobility Service en cada máquina virtual que desee migrar.
3. La migración basada en agente usa un dispositivo de replicación:
    - [Revise](migrate-replication-appliance.md#appliance-requirements) los requisitos de implementación para el dispositivo de replicación y las [opciones](migrate-replication-appliance.md#mysql-installation) para instalar MySQL en el dispositivo.
    - Revise los requisitos de acceso a [direcciones URL](migrate-replication-appliance.md#url-access) y [puertos](migrate-replication-appliance.md#port-access) para el dispositivo de replicación.
    
## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:

> [!div class="checklist"]
> * Configuró permisos de Azure.
> * Preparó VMware para la migración y evaluación.


Continúe con el segundo tutorial para configurar un proyecto de Azure Migrate y evaluar las máquinas virtuales de VMware para la migración a Azure.

> [!div class="nextstepaction"]
> [Evaluación de máquinas virtuales de VMware](./tutorial-assess-vmware.md)
