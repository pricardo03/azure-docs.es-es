---
title: Preparación de la evaluación de servidores físicos con Azure Migrate Server Assessment
description: Describe cómo prepararse para la evaluación y la migración de servidores virtuales a Azure mediante Azure Migrate Server Assessment.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 11/07/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 176dfdabeee6299bba0eb17085be25ced3e97993
ms.sourcegitcommit: a170b69b592e6e7e5cc816dabc0246f97897cb0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74091914"
---
# <a name="prepare-for-assessment-and-migration-of-physical-servers-to-azure"></a>Preparación de la evaluación y migración de servidores físicos a Azure

En este artículo se indica cómo prepararse para la evaluación de servidores físicos en el entorno local con [Azure Migrate](migrate-services-overview.md).


> [!NOTE]
> Si aún no ve algunas de estas características en el portal de Azure Migrate, espere. Aparecerán aproximadamente la semana siguiente.

[Azure Migrate](migrate-overview.md) proporciona un centro de herramientas que le ayuda a detecta las aplicaciones, la infraestructura y las cargas de trabajo, a evaluarlas y a migrarlas a Microsoft Azure. Este centro incluye herramientas de Azure Migrate y ofertas de fabricantes de software independientes (ISV) de terceros. 

Este tutorial es el primero de una serie que muestra cómo evaluar servidores físicos con Azure Migrate. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Prepare Azure. Configure los permisos de la cuenta y los recursos de Azure para poder usar Azure Migrate.
> * Preparar los servidores físicos en el entorno local para la evaluación de servidores.


> [!NOTE]
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para instrucciones detalladas, consulte los procedimientos para la evaluación de servidores físicos.


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/) antes de empezar.


## <a name="prepare-azure"></a>Preparación de Azure

### <a name="azure-permissions"></a>Permisos de Azure

Necesita configurar permisos para la implementación de Azure Migrate.

- Permisos para crear un proyecto de Azure Migrate en la cuenta de Azure. 
- Permisos en la cuenta de Azure para registrar el dispositivo de Azure Migrate. El dispositivo se usa para la detección y la migración de Hyper-V. Durante el registro del dispositivo, Azure Migrate crea dos aplicaciones Azure Active Directory (Azure AD) que identifican de forma única al dispositivo:
    - La primera aplicación se comunica con los puntos de conexión de servicio de Azure Migrate.
    - La segunda aplicación accede a un almacén de Azure Key Vault creado durante el registro para almacenar la información de la aplicación de Azure AD y los valores de configuración del dispositivo.



### <a name="assign-permissions-to-create-project"></a>Asignación de permisos para crear un proyecto

Compruebe que tenga permiso para crear un proyecto de Azure Migrate.

1. En Azure Portal, abra la suscripción y seleccione **Control de acceso (IAM)** .
2. En **Comprobar acceso**, busque la cuenta correspondiente y haga clic en ella para ver los permisos.
3. Debe tener permisos de **Colaborador** o **Propietario**.
    - Si acaba de crear una cuenta de Azure gratuita, es el propietario de la suscripción.
    - Si no es el propietario, trabaje con él para asignar el rol.


### <a name="assign-permissions-to-register-the-appliance"></a>Asignación de permisos para registrar el dispositivo

Puede asignar permisos para Azure Migrate para crear estas aplicaciones de Azure AD durante el registro del dispositivo mediante uno de los métodos siguientes:

- Un administrador de inquilinos o administrador global puede conceder permisos a los usuarios del inquilino para crear y registrar aplicaciones de Azure AD.
- Un administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.

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

El administrador de inquilinos o administrador global puede asignar el rol de desarrollador de aplicaciones a una cuenta. [Más información](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-for-physical-server-assessment"></a>Preparación de la valoración de servidores físicos

Para preparar la valoración de los servidores físicos, tiene que comprobar su configuración así como la de la implementación de los dispositivos.

### <a name="verify-physical-server-settings"></a>Comprobación de la configuración de los servidores físicos

1. Compruebe los [requisitos de los servidores físicos](migrate-support-matrix-physical.md#assessment-physical-server-requirements) para la valoración de los servidores.
2. Asegúrese de que los [puertos necesarios](migrate-support-matrix-physical.md#assessment-port-requirements) están abiertos en los servidores físicos.


### <a name="verify-appliance-settings"></a>Comprobación de la configuración del dispositivo

Antes de configurar el dispositivo de Azure Migrate y comenzar la evaluación en el siguiente tutorial, prepare la implementación del dispositivo.

1. [Compruebe](migrate-support-matrix-physical.md#assessment-appliance-requirements) los requisitos del dispositivo.
2. [Revise](migrate-support-matrix-physical.md#assessment-appliance-url-access) las direcciones URL de Azure a las que tendrá acceso el dispositivo.
3. Revise los datos que el dispositivo va a recopilar durante la detección y la evaluación.
4. [Tenga en cuenta](migrate-support-matrix-physical.md#assessment-port-requirements) los requisitos de acceso a puertos del dispositivo.


### <a name="set-up-an-account-for-physical-server-discovery"></a>Configuración de una cuenta para la detección de servidores físicos

Azure Migrate necesita permisos para detectar los servidores en el entorno local.

- **Windows:** configure una cuenta de usuario local en todos los servidores de Windows que desee incluir en la detección. Es necesario agregar la cuenta de usuario a los grupos siguientes: - Usuarios de administración remota       - Usuarios de Monitor de rendimiento       - Usuarios de Registro de rendimiento
- **Linux:** necesita una cuenta raíz en los servidores Linux que desee detectar.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, hizo lo siguiente:
 
> [!div class="checklist"] 
> * Configurar los permisos de la cuenta de Azure.
> * Preparar los servidores físicos para la evaluación.

Continúe con el siguiente tutorial para crear un proyecto de Azure Migrate y evaluar los servidores físicos para la migración a Azure

> [!div class="nextstepaction"] 
> [Evaluación de servidores físicos](./tutorial-assess-physical.md) 
