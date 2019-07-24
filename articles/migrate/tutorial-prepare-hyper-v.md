---
title: Preparación de máquinas virtuales de Hyper-V para su evaluación y migración a Azure con Azure Migrate | Microsoft Docs
description: Describe cómo preparar para la evaluación y la migración de máquinas virtuales de Hyper-V a Azure mediante Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 9e0d29770aa36f8e79bf08b7c5435ea2dbc4ae38
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840379"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Preparación de la evaluación y migración de máquinas virtuales de Hyper-V a Azure

En este artículo se indica cómo prepararse para la evaluación y migración de máquinas virtuales de Hyper-V locales a Azure mediante [Azure Migrate](migrate-services-overview.md).

[Azure Migrate](migrate-overview.md) proporciona un centro de herramientas que le ayudan a detectar, evaluar y migrar aplicaciones, infraestructura y cargas de trabajo en Microsoft Azure. Dicho centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros. 

Este tutorial es el primero de una serie que muestra cómo evaluar máquinas virtuales de Hyper-V y migrarlas a Azure. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Prepare Azure. Configure los permisos de la cuenta y los recursos de Azure para trabajar con Azure Migrate.
> * Prepare los hosts y las máquinas virtuales de Hyper-V locales para la evaluación de los servidores.
> * Prepare los hosts y las máquinas virtuales de Hyper-V locales para la migración de los servidores.


> [!NOTE]
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para obtener instrucciones detalladas, revise los procedimientos para la evaluación y la migración de Hyper-V.


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prepare-azure"></a>Preparación de Azure

### <a name="azure-permissions"></a>Permisos de Azure

Necesita un par de permisos para implementar Azure Migrate:

- La cuenta de Azure necesita permisos para crear un proyecto de Azure Migrate para su evaluación y migración. 
- La cuenta de Azure necesita permisos para registrar el dispositivo de Azure Migrate.
    - Para la evaluación, Azure Migrate ejecuta un dispositivo ligero que detecta máquinas virtuales de Hyper-V y envía los metadatos y los datos de rendimiento de estas a Azure Migrate.
    - Durante el registro del dispositivo, Azure Migrate crea dos aplicaciones Azure Active Directory (Azure AD) que identifican de forma única al dispositivo:
        - La primera aplicación se comunica con los puntos de conexión de servicio de Azure Migrate.
        - La segunda aplicación accede a un almacén de claves de Azure creado durante el registro para almacenar la información de la aplicación de Azure AD y los valores de configuración del dispositivo.
    - Puede dar permiso a Azure Migrate para que cree estas aplicaciones de Azure AD con uno de los métodos siguientes:
        - Un administrador de inquilinos o global puede conceder permisos a los usuarios del inquilino para crear y registrar aplicaciones de Azure AD.
        - Un administrador de inquilinos o global puede asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.
    - Merece la pena mencionar que:
        - Las aplicaciones no tienen otros permisos de acceso en la suscripción distintos de los descritos anteriormente.
        - Solo necesita estos permisos al registrar un nuevo dispositivo. Puede quitar los permisos una vez configurado el dispositivo. 


### <a name="assign-permissions-to-create-project"></a>Asignación de permisos para crear un proyecto

Compruebe que tenga permiso para crear un proyecto de Azure Migrate.

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.


### <a name="assign-permissions-to-register-the-appliance"></a>Asignación de permisos para registrar el dispositivo

Si va a implementar el dispositivo de Azure Migrate para evaluar máquinas virtuales, deberá registrarlo.

- Durante el registro del dispositivo, Azure Migrate crea dos aplicaciones de Azure Active Directory (Azure AD) que identifican de forma única al dispositivo.
    - La primera aplicación se comunica con los puntos de conexión de servicio de Azure Migrate.
    - La segunda aplicación accede a un almacén de claves de Azure creado durante el registro para almacenar la información de la aplicación de Azure AD y los valores de configuración del dispositivo.
- Puede dar permiso a Azure Migrate para que cree estas aplicaciones de Azure AD con uno de los métodos siguientes:
    - Un administrador de inquilinos o global puede conceder permisos a los usuarios del inquilino para crear y registrar aplicaciones de Azure AD.
    - Un administrador de inquilinos o global puede asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.

Merece la pena mencionar que:

- Las aplicaciones no tienen otros permisos de acceso en la suscripción distintos de los descritos anteriormente.
- Solo necesita estos permisos al registrar un nuevo dispositivo. Puede quitar los permisos una vez configurado el dispositivo. 


#### <a name="grant-account-permissions"></a>Concesión de permisos de cuenta

El administrador de inquilinos o global puede conceder permisos como se indica:

1. En Azure AD, el administrador de inquilinos o global debe ir a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
2. El administrador debe establecer **Registros de aplicaciones** en **Sí**.

    ![Permisos de Azure AD](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Se trata de una configuración predeterminada que no es confidencial. [Más información](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Asignación del rol de desarrollador de aplicaciones 

El administrador de inquilinos o global puede asignar el rol de desarrollador de aplicaciones a una cuenta. [Más información](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-for-hyper-v-assessment"></a>Preparación de la evaluación de Hyper-V

Para preparar la evaluación de Hyper-V, compruebe la configuración de la máquina virtual y el host de Hyper-V y compruebe la configuración de la implementación del dispositivo.

### <a name="verify-hyper-v-host-settings"></a>Comprobación de la configuración del host de Hyper-V

1. Compruebe los [requisitos del host de Hyper-V](migrate-support-matrix-hyper-v.md#assessment-hyper-v-host-requirements) para la evaluación de los servidores.
2. Asegúrese de que los [puertos necesarios](migrate-support-matrix-hyper-v.md#assessment-port-requirements) están abiertos en los hosts de Hyper-V.

### <a name="enable-powershell-remoting-on-hosts"></a>Habilitación de la comunicación remota con PowerShell en los hosts

Configure la comunicación remota de PowerShell en cada host, como se indica a continuación:

1. En cada uno de los hosts, abra una consola de PowerShell como administrador.
2. Ejecute este comando:

    ```
    Enable-PSRemoting -force
    ```

### <a name="enable-credssp-on-hosts"></a>Habilitación de CredSSP en los hosts

Si los discos de las máquinas virtuales se encuentran en recursos compartidos SMB, complete este paso en cada host de Hyper-V correspondiente. Este paso se usa para detectar la información de configuración de las máquinas virtuales de Hyper-V que tienen discos en recursos compartidos SMB. Si no tiene discos de máquina virtual en recursos compartidos SMB, puede omitir el paso.

1. Identifique los hosts de Hyper-V que ejecutan máquinas virtuales de Hyper-V con discos en recursos compartidos SMB.
2. Ejecute el siguiente comando en cada host de Hyper-V identificado:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

- La autenticación CredSSP permite al host de Hyper-V delegar las credenciales en nombre del cliente de Azure Migrate.
- Puede ejecutar este comando de forma remota en todos los hosts de Hyper-V.
- Si agrega nuevos nodos de host en un clúster, se agregan automáticamente para la detección, pero debe habilitar CredSSP manualmente en los nuevos nodos si es necesario.

### <a name="verify-appliance-settings"></a>Comprobación de la configuración del dispositivo

Antes de configurar el dispositivo de Azure Migrate y comenzar la evaluación en el siguiente tutorial, prepare la implementación del dispositivo.

1. [Compruebe](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) los requisitos del dispositivo.
2. [Revise](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) las direcciones URL de Azure a las que tendrá acceso el dispositivo.
3. Revise los datos que el dispositivo va a recopilar durante la detección y la evaluación.
4. [Tenga en cuenta](migrate-support-matrix-hyper-v.md#assessment-port-requirements) los puertos a los que el dispositivo necesita acceder.


### <a name="set-up-an-account-for-vm-discovery"></a>Configuración de una cuenta para la detección de máquinas virtuales

Azure Migrate necesita permisos para detectar máquinas virtuales locales.

- Configure una cuenta de usuario local o de dominio con permisos de administrador en el clúster o los hosts de Hyper-V.

    - Necesita una sola cuenta para todos los hosts y clústeres que desee incluir en la detección.
    - La cuenta puede ser una cuenta local o de dominio. Se recomienda que tenga permisos de administrador en los hosts o clústeres de Hyper-V.
    - O bien, si no desea asignar permisos de administrador, se necesitan los siguientes permisos:
        - Usuarios de Administración remota
        - Administradores de Hyper-V
        - Usuarios de Monitor de rendimiento

### <a name="enable-integration-services-on-vms"></a>Habilitación de Integration Services en máquinas virtuales

Integration Services debe estar habilitado en todas las máquinas virtuales para que Azure Migrate pueda capturar la información del sistema operativo de la máquina virtual.

- En las máquinas virtuales que quiera detectar y evaluar, habilite [Hyper-V Integration Services](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) en cada máquina virtual. 

## <a name="prepare-for-hyper-v-migration"></a>Preparación de la migración de Hyper-V

1. [Consulte](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-requirements) los requisitos del host de Hyper-V para la migración.
2. [Consulte](migrate-support-matrix-hyper-v.md#migration-hyper-v-vm-requirements) los requisitos de las máquinas virtuales de Hyper-V que desea migrar a Azure.
3. [Tenga en cuenta](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-url-access) las direcciones URL de Azure a las que los hosts y clústeres de Hyper-V necesitan acceder durante la migración de las máquinas virtuales.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:
 
> [!div class="checklist"] 
> * Configurar los permisos de la cuenta de Azure.
> * Preparar las máquinas virtuales y los hosts de Hyper-V para la evaluación y migración.

Continúe con el siguiente tutorial para crear un proyecto de Azure Migrate y evaluar las máquinas virtuales de Hyper-V para la migración a Azure.

> [!div class="nextstepaction"] 
> [Evaluación de máquinas virtuales de Hyper-V](./tutorial-assess-hyper-v.md) 
