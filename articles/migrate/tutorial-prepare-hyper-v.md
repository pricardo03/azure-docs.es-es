---
title: Preparación de máquinas virtuales de Hyper-V para su evaluación y migración a Azure con Azure Migrate | Microsoft Docs
description: Describe cómo preparar para la evaluación y la migración de máquinas virtuales de Hyper-V a Azure mediante Azure Migrate.
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/24/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 87df37cc6baa863bb0b068bdfeb9cde873e38836
ms.sourcegitcommit: acffa72239413c62662febd4e39ebcb6c6c0dd00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952078"
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
> En los tutoriales se muestra la ruta de implementación más sencilla para un escenario, de modo que pueda configurar rápidamente una prueba de concepto. En ellos se usan las opciones predeterminadas siempre que es posible y no muestran todos los valores y rutas de acceso posibles. Para instrucciones detalladas, revise los procedimientos para la evaluación y la migración de Hyper-V.


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


## <a name="prepare-for-hyper-v-assessment"></a>Preparación de la evaluación de Hyper-V

Para prepararse para la evaluación de Hyper-V haga lo siguiente:

1. Compruebe la configuración del host de Hyper-V.
2. Configure la comunicación remota de PowerShell en cada host de modo que el dispositivo de Azure Migrate pueda ejecutar comandos de PowerShell en el host mediante una conexión de WinRM.
3. Si los discos de máquina virtual se encuentran en almacenamiento SMB remoto, se necesita la delegación de credenciales. 
    - Habilite la delegación CredSSP para que el dispositivo de Azure Migrate pueda actuar como cliente y delegar las credenciales en un host. T
    - Permita que cada host actúe como delegado para el dispositivo, tal y como se describe a continuación.
    - Más adelante, cuando configure el dispositivo, podrá habilitar en él la delegación.
4. Revise los requisitos del dispositivo y el acceso a la dirección URL y el puerto necesarios para el dispositivo.
5. Configure una cuenta que el dispositivo usará para detectar máquinas virtuales.
6. Configure Hyper-V Integration Services en las máquinas virtuales que desee detectar y evaluar.


Puede configurar estas opciones manualmente mediante los procedimientos siguientes. Como alternativa, ejecute el script de configuración de requisitos previos de Hyper-V.

### <a name="hyper-v-prerequisites-configuration-script"></a>Script de configuración de requisitos previos de Hyper-V

El script valida los hosts de Hyper-V y configura las opciones necesarias para detectar y evaluar las máquinas virtuales de Hyper-V. Así es cómo funciona:

- Comprueba que se está ejecutando el script en una versión compatible de PowerShell.
- Comprueba que usted (el usuario que ejecuta el script) tiene privilegios administrativos en el host de Hyper-V.
- Permite crear una cuenta de usuario local (no administrador) que se usa para que el servicio Azure Migrate se comunique con el host de Hyper-V. Esta cuenta de usuario se agrega a estos grupos en el host:
    - Usuarios de Administración remota
    - Administradores de Hyper-V
    - Usuarios de Monitor de rendimiento
- Comprueba que el host está ejecutando una versión compatible de Hyper-V y el rol de Hyper-V.
- Habilita el servicio WinRM y abre los puertos 5985 (HTTP) y 5986 (HTTPS) en el host (necesario para la recopilación de metadatos).
- Habilita la comunicación remota con PowerShell en el host.
- Comprueba que el servicio de integración de Hyper-V está habilitado en todas las máquinas virtuales administradas por el host. 
- Habilita CredSSP en el host si es necesario.

Ejecute el script como se indica a continuación:

1. Asegúrese de que tiene instalada la versión de PowerShell 4.0 o posterior en el host de Hyper-V.
2. Descargue el script del [Centro de descarga de Microsoft](https://aka.ms/migrate/script/hyperv). Microsoft firma criptográficamente el script.
3. Valide la integridad del script mediante archivos hash MD5 o SHA256. Ejecute este comando para generar el código hash para el script:
    ```
    C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]
    ```
    Ejemplo de uso: 
    ```
    C:\>CertUtil -HashFile C:\Users\Administrators\Desktop\ MicrosoftAzureMigrate-Hyper-V.ps1
    SHA256
    ```
    
    Los valores de hash son:
    Hash | Valor
    --- | ---
    **MD5** | 0ef418f31915d01f896ac42a80dc414e
    **SHA256** | 0ad60e7299925eff4d1ae9f1c7db485dc9316ef45b0964148a3c07c80761ade2


4.  Después de validar la integridad del script, ejecútelo en cada host de Hyper-V con este comando de PowerShell:
    ```
    PS C:\Users\Administrators\Desktop> MicrosoftAzureMigrate-Hyper-V.ps1
    ```


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

Si el host tiene discos de las máquinas virtuales en recursos compartidos SMB, complete este paso en cada host.

- Puede ejecutar este comando de forma remota en todos los hosts de Hyper-V.
- Si agrega nuevos nodos de host en un clúster, se agregan automáticamente para la detección, pero debe habilitar CredSSP manualmente en los nuevos nodos si es necesario.

Habilite de la siguiente manera:

1. Identifique los hosts de Hyper-V que ejecutan máquinas virtuales de Hyper-V con discos en recursos compartidos SMB.
2. Ejecute el siguiente comando en cada host de Hyper-V identificado:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

Al configurar el dispositivo, termine de configurar CredSSP mediante su [habilitación en el dispositivo](tutorial-assess-hyper-v.md#delegate-credentials-for-smb-vhds). Esto se describe en el siguiente tutorial de la serie.


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

En las máquinas virtuales que quiera detectar y evaluar, habilite [Hyper-V Integration Services](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) en cada máquina virtual. 

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
