---
title: Preparación de máquinas virtuales de VMware para su evaluación y migración a Azure con Azure Migrate | Microsoft Docs
description: Describe cómo prepararse para la evaluación y migración de máquinas virtuales de VMware locales a Azure mediante Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 16c0354466d5c2a1207873a1e83e209da9339705
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69509990"
---
# <a name="prepare-vmware-vms-for-assessment-and-migration-to-azure"></a>Preparación de máquinas virtuales de VMware para la evaluación y migración a Azure

En este artículo se indica cómo prepararse para la evaluación y migración de máquinas virtuales de VMware locales a Azure mediante [Azure Migrate](migrate-services-overview.md).

[Azure Migrate](migrate-overview.md) proporciona un centro de herramientas que le ayudan a detectar, evaluar y migrar aplicaciones, infraestructura y cargas de trabajo en Microsoft Azure. Dicho centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros. 


Este tutorial es el primero de una serie que muestra cómo evaluar y migrar máquinas virtuales de VMware. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Prepare Azure. Configure los permisos de la cuenta y los recursos de Azure para poder usar Azure Migrate.
> * Prepare los servidores y las máquinas virtuales de VMware para la evaluación de las máquinas virtuales.
> * Prepare los servidores y las máquinas virtuales de VMware para la migración de estas.

> [!NOTE]
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para obtener instrucciones detalladas, revise los procedimientos para la evaluación y la migración de VMware.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prepare-azure"></a>Preparación de Azure

Necesita estos permisos de Azure:

- La cuenta de Azure necesita permisos para crear un proyecto de Azure Migrate para su evaluación y migración. 
- Para la evaluación y migración sin agente de máquinas virtuales de VMware, Azure Migrate ejecuta un dispositivo ligero que detecta máquinas virtuales y envía los metadatos y los datos de rendimiento de estas a Azure Migrate. En Azure, necesita permisos para registrar el dispositivo de Azure Migrate.
- Cuando se migran máquinas virtuales de VMware con Azure Migrate Server Migration, Azure Migrate crea un almacén de claves en el grupo de recursos para administrar las claves de acceso a la cuenta de almacenamiento de replicación de la suscripción. Para crear el almacén, necesita permisos de asignación de roles en el grupo de recursos en el que reside el proyecto de Azure Migrate. 


### <a name="assign-permissions-to-create-project"></a>Asignación de permisos para crear un proyecto

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.

### <a name="assign-permissions-to-register-the-appliance"></a>Asignación de permisos para registrar el dispositivo

Si va a implementar el dispositivo de Azure Migrate para evaluar o ejecutar una migración sin agente de máquinas virtuales, deberá registrarlo.

- Durante el registro del dispositivo, Azure Migrate crea dos aplicaciones de Azure Active Directory (Azure AD) que identifican de forma única al dispositivo.
    - La primera aplicación se comunica con los puntos de conexión de servicio de Azure Migrate.
    - La segunda aplicación accede a un almacén de Azure Key Vault creado durante el registro para almacenar la información de la aplicación de Azure AD y los valores de configuración del dispositivo.
- Puede asignar permisos para Azure Migrate para crear estas aplicaciones de Azure AD mediante uno de los métodos siguientes:
    - Un administrador de inquilinos o administrador global puede conceder permisos a los usuarios del inquilino para crear y registrar aplicaciones de Azure AD.
    - Un administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.

Merece la pena mencionar que:

- Las aplicaciones no tienen otros permisos de acceso en la suscripción distintos de los descritos anteriormente.
- Solo necesita estos permisos al registrar un nuevo dispositivo. Puede quitar los permisos una vez configurado el dispositivo. 


#### <a name="grant-account-permissions"></a>Concesión de permisos de cuenta

El administrador de inquilinos o administrador global puede conceder permisos como se indica:

1. En Azure AD, el administrador de inquilinos o global debe ir a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
2. El administrador debe establecer **Registros de aplicaciones** en **Sí**.

    ![Permisos de Azure AD](./media/tutorial-prepare-vmware/aad.png)

> [!NOTE]
> Se trata de una configuración predeterminada que no es confidencial. [Más información](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Asignación del rol de desarrollador de aplicaciones 

El administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones a una cuenta. [Más información](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

## <a name="assign-role-assignment-permissions"></a>Asignación de permisos de asignación de roles

Asigne permisos de asignación de roles en el grupo de recursos en el que reside el proyecto de Azure Migrate, como se indica a continuación:

1. En el grupo de recursos de Azure Portal, seleccione **Control de acceso (IAM**).
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.

    - Para ejecutar Server Assessment, son suficientes los permisos de **colaborador**.
    - Para ejecutar la migración de un servidor sin agente, debe disponer de permisos de **propietario** (o **colaborador** y **administrador de acceso de usuario**).

3. Si no tiene los permisos necesarios, pídaselos al propietario del grupo de recursos. 



## <a name="prepare-for-vmware-vm-assessment"></a>Preparación para la evaluación de máquinas virtuales de VMware

Para preparar la evaluación de máquinas virtuales de VMware, compruebe la configuración de la máquina virtual y el host de VMware y compruebe la configuración de la implementación del dispositivo.

### <a name="verify-vmware-settings"></a>Comprobación de la configuración de VMware

1. [Compruebe](migrate-support-matrix-vmware.md#assessment-vcenter-server-requirements) los requisitos del servidor de VMware para la evaluación de las máquinas virtuales.
2. [Asegúrese](migrate-support-matrix-vmware.md#assessment-port-requirements) de que están abiertos los puertos necesarios en los servidores vCenter.


### <a name="set-up-an-account-for-assessment"></a>Configuración de una cuenta para la evaluación

Azure Migrate necesita acceder a vCenter Server para detectar máquinas virtuales para su evaluación y migración sin agente. Con fines solo de evaluación, necesita una cuenta de solo lectura para acceder a vCenter Server.

Si va a usar un proxy de firewall basado en direcciones URL, permita el acceso a las [URL de Azure](migrate-support-matrix-vmware.md#assessment-url-access-requirements) necesarias.

Asegúrese de que el proxy resuelve todos los registros CNAME recibidos al buscar las direcciones URL.


### <a name="verify-appliance-settings-for-assessment"></a>Comprobación de la configuración del dispositivo para su evaluación

Antes de configurar el dispositivo de Azure Migrate y comenzar la evaluación en el siguiente tutorial, prepare la implementación del dispositivo.

1. [Compruebe](migrate-support-matrix-vmware.md#assessment-appliance-requirements) los requisitos para configurar el dispositivo de Azure Migrate en VMware.
2. [Revise](migrate-support-matrix-vmware.md#assessment-url-access-requirements) las direcciones URL de Azure a las que tendrá acceso el dispositivo.
3. Revise los datos que el dispositivo va a recopilar durante la detección y la evaluación.
4. [Tenga en cuenta](migrate-support-matrix-vmware.md#assessment-port-requirements) los requisitos de acceso a puertos del dispositivo.
5. Puede implementar el dispositivo de Azure Migrate como una máquina virtual de VMware mediante un archivo OVA. En vCenter Server, asegúrese de que la cuenta tiene permisos para crear una máquina virtual mediante un archivo OVA.


## <a name="prepare-for-agentless-vmware-migration"></a>Preparación para la migración de VMware sin agente

Revise los requisitos para la migración sin agente de máquinas virtuales de VMware.

1. [Revise](migrate-support-matrix-vmware.md#agentless-migration-vmware-server-requirements) los requisitos del servidor de VMware para la migración sin agente.
2. Configure una cuenta para acceder a vCenter Server con los [permisos necesarios](migrate-support-matrix-vmware.md#agentless-migration-vcenter-server-permissions) para la migración sin agente.
3. [Tenga en cuenta](migrate-support-matrix-vmware.md#agentless-migration-vmware-vm-requirements) los requisitos de las máquinas virtuales de VMware que desea migrar a Azure mediante la migración sin agente.
4. [Revise](migrate-support-matrix-vmware.md#agentless-migration-appliance-requirements) los requisitos del dispositivo para la migración sin agente.
5. Tenga en cuenta los requisitos de [acceso a direcciones URL](migrate-support-matrix-vmware.md#agentless-migration-url-access-requirements) y [acceso a puertos](migrate-support-matrix-vmware.md#agentless-migration-port-requirements) del dispositivo para la migración sin agente.


## <a name="prepare-for-agent-based-vmware-migration"></a>Preparación para la migración de VMware basada en agente

Revise los requisitos para la migración basada en agente de máquinas virtuales de VMware.

1. [Revise](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) los requisitos del servidor de VMware para la migración sin agente. 
2. Configure una cuenta para acceder a vCenter Server con los [permisos necesarios](migrate-support-matrix-vmware.md#agent-based-migration-vcenter-server-permissions) para la migración sin agente.
3. [Tenga en cuenta](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) los requisitos de las máquinas virtuales de VMware que desea migrar a Azure mediante la migración basada en agente, incluida la instalación de Mobility Service en cada máquina virtual que desee migrar.
4. Tenga en cuenta el [acceso a direcciones URL](migrate-support-matrix-vmware.md#agent-based-migration-url-access-requirements).
5. Revise los requisitos de [acceso a puertos](migrate-support-matrix-vmware.md#agent-based-migration-port-requirements) de las instancias de Mobility Service que se ejecutan en cada máquina virtual y del servidor de configuración de Azure Migrate.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:
 
> [!div class="checklist"] 
> * Configuró permisos de Azure.
> * Preparó VMware para la migración y evaluación.


Continúe con el segundo tutorial para configurar un proyecto de Azure Migrate y evaluar las máquinas virtuales de VMware para la migración a Azure.

> [!div class="nextstepaction"] 
> [Evaluación de máquinas virtuales de VMware](./tutorial-migrate-vmware.md) 

