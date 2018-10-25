---
title: Protección de Azure Key Vault | Microsoft Docs
description: Administración de los permisos de acceso del almacén de claves para la administración de Azure Key Vault, así como de claves y secretos. Modelo de autenticación y autorización de Key Vault y cómo proteger un almacén de claves
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/09/2018
ms.author: ambapat
ms.openlocfilehash: 4a1de3c011f1f8cfa1ea9246efad4ebb7f9e3a76
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364530"
---
# <a name="secure-your-key-vault"></a>Protección de un almacén de claves
Azure Key Vault es un servicio en la nube que protege las claves de cifrado y los secretos (como certificados, cadenas de conexión y contraseñas). Dado que estos datos son confidenciales y críticos para la empresa, el acceso a los almacenes de claves debe estar protegido, de modo que solo puedan obtener acceso las aplicaciones y los usuarios autorizados. 

En este artículo se proporciona información general del modelo de acceso del almacén de claves. Se explican la autenticación y la autorización, y se describe cómo proteger el acceso al almacén de claves.

## <a name="overview"></a>Información general
El acceso a un almacén de claves se controla a través de dos interfaces diferentes: plano de administración y plano de datos. Para ambos planos se requieren una autorización y autenticación correctas antes de que cualquier autor de una llamada (usuario o aplicación) pueda obtener acceso a un almacén de claves. La autenticación establece la identidad del autor de la llamada, mientras que la autorización determina las operaciones que este puede realizar.

Para la autenticación tanto del plano de administración como del plano de datos se usa Azure Active Directory. Para la autorización, el plano de administración usa el control de acceso basado en rol (RBAC), mientras que el plano de datos utiliza la directiva de acceso de Key Vault.

Esta es una breve descripción general de los temas tratados:

[Autenticación mediante Azure Active Directory](#authentication-using-azure-active-directory): esta sección explica la forma en que el autor de una llamada se autentica con Azure Active Directory para acceder a un almacén de claves a través del plano de administración y del plano de datos. 

[Plano de administración y plano de datos](#management-plane-and-data-plane): el plano de administración y el plano de datos son dos planos de acceso que se usan para acceder a un almacén de claves. Cada plano de acceso admite operaciones concretas. En esta sección se describen los puntos de conexión de acceso, las operaciones compatibles y el método de control de acceso que utiliza cada plano. 

[Control de acceso del plano de administración](#management-plane-access-control): en esta sección examinaremos cómo se permite el acceso a las operaciones del plano de administración mediante un control de acceso basado en roles.

[Control de acceso al plano de datos](#data-plane-access-control): en esta sección se describe cómo usar la directiva de acceso de Key Vault para controlar el acceso al plano de datos.

[Ejemplo](#example): en este ejemplo se describe cómo configurar el control de acceso en un almacén de claves para permitir tres equipos distintos (el equipo de seguridad, los desarrolladores y operadores, y los auditores) para realizar tareas específicas para desarrollar, administrar y supervisar una aplicación en Azure.

## <a name="authentication-using-azure-active-directory"></a>Autenticación mediante Azure Active Directory
Cuando se crea un almacén de claves en una suscripción de Azure, se asocia automáticamente al inquilino de Azure Active Directory de dicha suscripción. Todos los autores de llamada (usuarios y aplicaciones) deben estar registrados en este inquilino y deben autenticarse para acceder al almacén de claves. Este requisito se aplica al acceso tanto al plano de administración como al plano de datos. En ambos casos, una aplicación puede acceder al almacén de claves de dos maneras:

* **acceso a usuario + aplicación**: se utiliza con aplicaciones que acceden al almacén de claves en nombre del usuario que ha iniciado sesión. Algunos ejemplos de este tipo de acceso son Azure PowerShell y Azure Portal. Existen dos maneras de conceder acceso a los usuarios: 
- Conceder acceso a los usuarios para que puedan acceder al almacén de claves desde cualquier aplicación.
- Conceder a los usuarios acceso al almacén de claves solo cuando usen una aplicación específica (se conoce como identidad compuesta).

* **acceso solo a aplicación**: se utiliza con aplicaciones que ejecutan servicios de demonio o trabajos en segundo plano. A la identidad de la aplicación se le concede acceso al almacén de claves.

En ambos tipos de aplicaciones, la aplicación se autentica con Azure Active Directory mediante cualquiera de los [métodos de autenticación admitidos](../active-directory/develop/authentication-scenarios.md) y adquiere un token. El método de autenticación utilizado depende del tipo de aplicación. Después, la aplicación usa este token y envía la solicitud de la API de REST a Key Vault. Las solicitudes del plano de administración se enrutan a través de un punto de conexión de Azure Resource Manager. Al acceder al plano de datos, la aplicación se comunica directamente con un punto de conexión del almacén de claves. Consulte más detalles acerca de [todo el flujo de autenticación](../active-directory/develop/v1-protocols-oauth-code.md). 

El nombre del recurso para el que la aplicación solicita un token es diferente en función de si la aplicación accede a un plano de administración o a un plano de datos. De ahí que el nombre del recurso sea el punto de conexión del plano de administración o del plano de datos descrito en la tabla de una sección posterior, según el entorno de Azure.

Tener un solo mecanismo para la autenticación tanto del plano de administración como del de datos tiene sus propias ventajas:

* Las organizaciones pueden controlar de forma centralizada el acceso a todos sus almacenes de claves
* Si un usuario abandona la organización, al instante pierde el acceso a todos los almacenes de claves de la organización
* Las organizaciones pueden personalizar la autenticación mediante las opciones de Azure Active Directory (por ejemplo, la habilitación de Multi-Factor Authentication para aumentar la seguridad)

## <a name="management-plane-and-data-plane"></a>Plano de administración y plano de datos
Azure Key Vault es un servicio de Azure disponible a través del modelo de implementación de Azure Resource Manager. Al crear un almacén de claves, se obtiene un contenedor virtual en el que se pueden almacenar objetos confidenciales, como certificados, claves y secretos. Las operaciones específicas se realizan en un almacén de claves mediante las interfaces del plano de administración y el plano de datos. El plano de administración se utiliza para administrar el propio almacén de claves. Incluye operaciones tales como administrar atributos y establecer las directivas de acceso del plano de datos. La interfaz del plano de datos se utiliza para agregar, eliminar, modificar y utilizar las claves, los secretos y los certificados almacenados en el almacén de claves.

A las interfaces del plano de administración y del plano de datos se accede a través de los distintos puntos de conexión que se indican a continuación. En la segunda columna de la tabla se describen los nombres DNS de estos puntos de conexión en diferentes entornos de Azure. La tercera columna describe las operaciones que se pueden realizar desde cada plano de acceso. Cada plano de acceso también tiene su propio mecanismo de control de acceso. El control de acceso del plano de administración se establece mediante el control de acceso basado en rol (RBAC) de Azure Resource Manager. El control de acceso del plano de datos se establece mediante la directiva de acceso del almacén de claves.

| Plano de acceso | Puntos de conexión de acceso | Operaciones | Mecanismo de control de acceso |
| --- | --- | --- | --- |
| Plano de administración |**Global:**<br> management.azure.com:443<br><br> **Azure China 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure Gobierno de EE. UU.:**<br> management.usgovcloudapi.net:443<br><br> **Azure Alemania:**<br> management.microsoftazure.de:443 |Crear, leer, actualizar o eliminar un almacén de claves <br> Establecer directivas de acceso para un almacén de claves<br>Establecer etiquetas para un almacén de claves |Control de acceso basado en rol (RBAC) de Azure Resource Manager |
| Plano de datos |**Global:**<br> &lt;vault-name&gt;.vault.azure.net:443<br><br> **Azure China 21Vianet:**<br> &lt;vault-name&gt;.vault.azure.cn:443<br><br> **Azure Gobierno de EE. UU.:**<br> &lt;vault-name&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Alemania:**<br> &lt;vault-name&gt;.vault.microsoftazure.de:443 |Para claves: Decrypt, Encrypt, UnwrapKey, WrapKey, Verify, Sign, Get, List, Update, Create, Import, Delete, Backup, Restore<br><br> Para secretos: Get, List, Set, Delete |Directiva de acceso de Key Vault |

Los controles de acceso del plano de administración y del plano de datos funcionan de forma independiente. Por ejemplo, si quiere conceder acceso a una aplicación para usar las claves de un almacén de claves, solo tiene que conceder acceso al plano de datos. El acceso se concede a través de las directivas de acceso del almacén de claves. Por el contrario, un usuario que necesita leer propiedades y etiquetas del almacén, pero no acceder a los datos (claves, secretos o certificados), solo necesita acceso al plano de control. El acceso se concede asignando acceso de “lectura” al usuario a través de RBAC.

## <a name="management-plane-access-control"></a>Control de acceso del plano de administración
El plano de la administración se compone de las operaciones que afectan al propio almacén de claves, tales como:

- Creación o eliminación de un almacén de claves.
- Obtención de una lista de los almacenes de una suscripción.
- Recuperación de las propiedades del almacén de claves (por ejemplo, SKU y etiquetas).
- Definición de las directivas de acceso del almacén de claves que controlan el acceso de usuarios y aplicaciones a claves y secretos.

El control de acceso del plano de administración utiliza RBAC. Vea la lista completa de operaciones del almacén de claves que se pueden realizar a través del plano de administración en la tabla de la sección anterior. 

### <a name="role-based-access-control-rbac"></a>Control de acceso basado en rol (RBAC)
Cada una de las suscripciones de Azure está asociada a una instancia de Azure Active Directory. Es posible conceder acceso a los usuarios, los grupos y las aplicaciones de ese directorio para que administren los de la suscripción de Azure que usan el modelo de implementación de Azure Resource Manager. Este tipo de control de acceso se conoce como control de acceso basado en rol (RBAC). Para administrar este acceso, se puede utilizar [Azure Portal](https://portal.azure.com/), [las herramientas de la CLI de Azure](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) o las [API de REST de Azure Resource Manager](https://msdn.microsoft.com/library/azure/dn906885.aspx).

Un almacén de claves se crea en un grupo de recursos y controla el acceso al plano de administración mediante Azure Active Directory. Por ejemplo, puede conceder a los usuarios o a un grupo la capacidad de administrar los almacenes de claves en un grupo de recursos.

Puede conceder acceso a usuarios, grupos y aplicaciones de un ámbito concreto mediante la asignación de los roles de RBAC apropiados. Por ejemplo, para conceder acceso a un usuario para administrar los almacenes de claves, debe asignar el rol predefinido “Colaborador de almacén de claves” a este usuario en un ámbito específico. En este caso, el ámbito sería una suscripción, un grupo de recursos o un almacén de claves específico. Un rol asignado al nivel de suscripción se aplica a todos los recursos y grupos de recursos de esa suscripción. Un rol asignado al nivel de grupo de recursos se aplica a todos los recursos de dicho grupo de recursos. Un rol asignado a un recurso concreto se aplica a dicho recurso. Existen varios roles predefinidos (consulte [RBAC: roles integrados](../role-based-access-control/built-in-roles.md)). Si un rol predefinido no se ajusta a sus necesidades, puede definir uno propio.

> [!IMPORTANT]
> Tenga en cuenta que si un usuario tiene permisos de Colaborador (RBAC) en un plano de administración de un almacén de claves, se puede conceder el mismo acceso al plano de datos estableciendo la directiva de acceso al almacén de claves, que controla el acceso al plano de datos. Por consiguiente, se recomienda controlar de forma estricta quién tiene acceso de 'colaborador' a los almacenes de claves, con el fin de garantizar que las personas autorizadas son las únicas que pueden acceder y administrar los almacenes de claves, las claves, los secretos y los certificados.
> 
> 

## <a name="data-plane-access-control"></a>Control de acceso al plano de datos
Las operaciones del plano de datos del almacén de claves se aplican a los objetos almacenados, tales como claves, secretos y certificados. Las operaciones principales incluyen las de creación, importación, actualización, enumeración, copia de seguridad y restauración de claves. Las operaciones criptográficas incluyen las de firma, comprobación, cifrado, descifrado, encapsulado, desencapsulado y definición de etiquetas y otros atributos de claves. De forma similar, las operaciones de secretos incluyen las de obtención, definición, enumeración y eliminación.

El acceso al plano de datos se concede mediante el establecimiento de directivas de acceso para un almacén de claves. Un usuario, grupo o aplicación deben tener permisos de colaborador (RBAC) para el plano de administración de una instancia de un almacén de claves para poder establecer directivas de acceso para dicho almacén. Se puede conceder a un usuario, grupo o aplicación acceso para realizar operaciones concretas en las claves o secretos de un almacén de claves. Key Vault admite hasta 1024 entradas de directivas de acceso para un almacén de claves. Cree un grupo de seguridad de Azure Active Directory y agregue usuarios a dicho grupo para conceder acceso al plano de datos a varios usuarios a un almacén de claves.

### <a name="key-vault-access-policies"></a>Directivas de acceso de almacén de claves
Las directivas de acceso a los almacenes de claves conceden permisos a las claves, los secretos y los certificados por separado. Por ejemplo, se puede conceder a un usuario acceso solo a las claves, sin permisos para los secretos. Los permisos para acceder a las claves, los secretos o los certificados se encuentran en el nivel del almacén. La directiva de acceso del almacén de claves no admite permisos de nivel de objeto pormenorizado, como una clave, un secreto o un certificado específico. Para establecer las directivas de acceso de un almacén de claves, puede usar [Azure Portal](https://portal.azure.com/), las [herramientas de la CLI de Azure](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) o las [API de REST de administración de un almacén de claves](https://msdn.microsoft.com/library/azure/mt620024.aspx).

> [!IMPORTANT]
> Tenga en cuenta que las directivas de acceso de un almacén de claves se aplican en el nivel de almacén. Por ejemplo, cuando se concede a un usuario permiso para crear y eliminar claves, puede realizar dichas operaciones en todas las claves de dicho almacén de claves.

Además de las directivas de acceso, el acceso al plano de datos también puede restringirse mediante [puntos de conexión de servicio de Virtual Network para Azure Key Vault](key-vault-overview-vnet-service-endpoints.md) configurando los [firewalls y las reglas de red virtual](key-vault-network-security.md) para crear un nivel de seguridad adicional.

## <a name="example"></a>Ejemplo
Supongamos que está desarrollando una aplicación que utiliza un certificado para SSL, Azure Storage para almacenar los datos y una clave RSA de 2048 bits para las operaciones de firma. Supongamos que esta aplicación se ejecuta en una máquina virtual de Azure (o un conjunto de escalado de máquinas virtuales). Puede usar un almacén de claves para almacenar todos los secretos de la aplicación y almacenar el certificado de arranque que usa la aplicación para realizar la autenticación con Azure Active Directory.

Este es un resumen de todos los tipos de claves y secretos almacenados en un almacén de claves:

* **Certificado SSL**: se usa para SSL.
* **Clave de almacenamiento**: se usa para obtener acceso a una cuenta de Storage.
* **Clave RSA de 2048 bits**: se usa para las operaciones de firma.
* **Certificado de arranque** : se usa para la autenticación con Azure Active Directory. Una vez que se concede acceso, puede recuperar la clave de almacenamiento y usar la clave RSA para la firma.

Ahora vamos a conocer a las personas que administran, implementan y auditan esta aplicación. En este ejemplo, vamos a usar tres roles.

* **Equipo de seguridad**: por lo general, personal de TI de la oficina del CSO (director de seguridad) o equivalente. Este equipo es responsable de la protección adecuada de los secretos. Por ejemplo, los certificados SSL, las claves RSA utilizadas para la firma, las cadenas de conexión y las claves de cuenta de almacenamiento.
* **Desarrolladores u operadores**: personal que desarrollan la aplicación y, después, la implementa en Azure. Por lo general, no forman parte del equipo de seguridad y, por tanto, no deben tener acceso a información confidencial, como los certificados SSL y las claves RSA. Solo la aplicación que implementan debe tener acceso a esos objetos.
* **Auditores**: suele ser un conjunto diferente de personas, aisladas de los desarrolladores y del personal de TI en general. Su responsabilidad es revisar el uso y mantenimiento de los certificados, las claves y los secretos para garantizar el cumplimiento de los estándares de seguridad. 

Existe más un rol más que está fuera del ámbito de esta aplicación, pero que vale la pena mencionar aquí. Ese rol es el de administrador de la suscripción (o el grupo de recursos). El administrador de la suscripción configura los permisos de acceso iniciales del equipo de seguridad. El administrador de la suscripción concede acceso al equipo de seguridad, para lo cual usa un grupo de recursos que contiene los recursos necesarios para esta aplicación.

Ahora veamos qué acciones realiza cada rol en el contexto de esta aplicación.

* **Equipo de seguridad**
  * Crear instancias de Key Vault
  * Activar el registro de Key Vault
  * Agregar claves y secretos
  * Crear una copia de seguridad de las claves para la recuperación ante desastres
  * Establecer una directiva de acceso a un almacén de claves para conceder permisos a usuarios y aplicaciones para realizar operaciones concretas
  * Rotación periódica las claves y los secretos
* **Desarrolladores y operadores**
  * Obtener referencias al arranque y certificados SSL (huellas digitales), la clave de almacenamiento (identificador URI de secreto) y la clave de firma (identificador URI de clave) del equipo de seguridad
  * Desarrollar e implementar una aplicación que acceda a las claves y los secretos por programación
* **Auditores**
  * Examinar los registros de uso para confirmar que las claves y los secretos se usan correctamente y cumplen los estándares de seguridad de datos

Ahora veamos qué permisos de acceso necesita cada rol y aplicación para realizar las tareas que tiene asignadas. 

| Rol de usuario | Permisos del plano de administración | Permisos del plano de datos |
| --- | --- | --- |
| Equipo de seguridad |Colaborador de almacén de claves |Claves: copia de seguridad, creación, eliminación, obtención, importación, lista y restauración <br> Secretos: todos |
| Desarrolladores u operador |Permiso de implementación en un almacén de claves para que las máquinas virtuales que se implementen puedan capturar secretos desde el almacén de claves |None |
| Auditores |None |Claves: enumeración<br>Secretos: enumeración |
| Application |None |Claves: firma<br>Secretos: obtención |

> [!NOTE]
> Los auditores necesitan un permiso de enumeración de claves y secretos para poder inspeccionar los atributos de las claves y los secretos que no se emiten en los registros, como las etiquetas y las fechas de activación y expiración.
> 
> 

Además de los permisos del almacén de claves, los tres roles también necesitan tener acceso a otros recursos. Por ejemplo, para poder implementar máquinas virtuales (o Web Apps, etc.) Los desarrolladores y operadores también necesitan acceso de 'Colaborador' a dichos tipos de recursos. Los auditores necesitan acceso de “lectura” a la cuenta de almacenamiento donde se almacenan los registros del almacén de claves.

Puesto que el enfoque de este artículo es proteger el acceso al almacén de claves, solo se ilustran los conceptos relacionados con este tema. Los detalles relativos a la implementación de certificados, y el acceso a claves y secretos mediante programación, entre otros, se tratan en otro artículo. Por ejemplo:

- La implementación de certificados almacenados en el almacén de claves en máquinas virtuales se trata en una [entrada de blog: Deploy Certificates to VMs from customer-managed Key Vault](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) (Implementar certificados en máquinas virtuales desde la instancia de Key Vault administrada por el cliente)
- La [descarga de ejemplos de cliente de Azure Key Vault](https://www.microsoft.com/download/details.aspx?id=45343) ilustra cómo usar un certificado de arranque para autenticarse en Azure AD con la finalidad de acceder a un almacén de claves.

La mayoría de los permisos de acceso se pueden conceder mediante Azure Portal. Para conceder permisos pormenorizados, debe usar Azure PowerShell o la CLI de Azure para lograr el resultado deseado. 

En los siguientes fragmentos de código de PowerShell se asume que:

* El administrador de Azure Active Directory ha creado grupos de seguridad que representan los tres roles (Contoso Security Team, Contoso App Devops y Contoso App Auditors). El administrador también ha agregado usuarios a los grupos que pertenecen.
* **ContosoAppRG** es el grupo de recursos en el que residen todos los recursos. **contosologstorage** es el lugar en que se almacenan los registros. 
* La instancia de Key Vault **ContosoKeyVault** y la cuenta de Storage que se utiliza para los registros del almacén de claves **contosologstorage** deben estar en la misma ubicación de Azure

En primer lugar el administrador de la suscripción asigna los roles de ' Colaborador de almacén de claves' y 'Administrador de acceso del usuario' al equipo de seguridad. Estos roles permiten al equipo de seguridad administrar no solo el acceso a otros recursos sino también los almacenes de claves en el grupo de recursos ContosoAppRG.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

El siguiente script muestra cómo el equipo de seguridad puede crear un almacén de claves y configurar los permisos de acceso y registro. Consulte [Información acerca de claves, secretos y certificados de Azure Key Vault](about-keys-secrets-and-certificates.md) para obtener más información sobre los permisos de directiva de acceso de Key Vault.

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -Name ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

El rol personalizado definido solo se puede asignar a la aplicación en la que se crea el grupo de recursos ContosoAppRG. Si se van a usar los mismos roles personalizados para otros proyectos de otras suscripciones, se podrán agregar más suscripciones a su ámbito.

El ámbito de la asignación de roles personalizados a los desarrolladores u operadores para el permiso de "implementación o acción" es el grupo de recursos. De este modo los secretos (certificado SSL y certificado de arranque) solo los obtendrán las máquinas virtuales creadas en el grupo de recursos “ContosoAppRG”. Las máquinas virtuales creadas en otro grupo de recursos por un miembro del equipo de desarrollo y operaciones no tendrán acceso a estos secretos, aunque tengan identificadores URI de secreto.

En este ejemplo se muestra un escenario simple. Los escenarios de la vida real pueden ser más complejos y puede que necesite ajustar los permisos del almacén de claves en función de sus necesidades. En nuestro ejemplo, suponemos que el equipo de seguridad proporcionará las referencias de las claves y los secretos (identificadores URI y huellas digitales) a los que los desarrolladores y operadores deben hacer referencia en sus aplicaciones. Los desarrolladores y operadores no requieren ningún acceso al plano de datos. Este ejemplo se centra en la protección de su almacén de claves. Debe darse una consideración parecida a la protección de [las máquinas virtuales](https://azure.microsoft.com/services/virtual-machines/security/), [las cuentas de almacenamiento](../storage/common/storage-security-guide.md) y otros recursos de Azure.

> [!NOTE]
> Nota: En este ejemplo se muestra cómo se bloqueará el acceso a un almacén de claves en producción. Los desarrolladores deben tener su propia suscripción o grupo de recursos, en el que tengan permisos completos para administrar sus almacenes, máquinas virtuales y la cuenta de Storage donde desarrollan la aplicación.

Se recomienda encarecidamente proteger el acceso al almacén de claves aún más [configurando los firewalls y las redes virtuales de Key Vault](key-vault-network-security.md).

## <a name="resources"></a>Recursos
* [Control de acceso basado en roles de Azure Active Directory](../role-based-access-control/role-assignments-portal.md)
  
  En este artículo se explica el control de acceso basado en rol de Azure Active Directory y su funcionamiento.
* [RBAC: Roles integrados](../role-based-access-control/built-in-roles.md)
  
  En este artículo se proporciona información detallada de todos los roles integrados disponibles en RBAC.
* [Descripción de la implementación del Administrador de recursos y la implementación clásica](../azure-resource-manager/resource-manager-deployment-model.md)
  
  En este artículo se explica el modelo de implementación clásica y de Resource Manager, y se detallan las ventajas de utilizar Resource Manager y los grupos de recursos.
* [Administración del control de acceso basado en rol con Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)
  
  En este artículo se explica cómo administrar el control de acceso basado en rol con Azure PowerShell
* [Administración del control de acceso basado en rol con la API de REST](../role-based-access-control/role-assignments-rest.md)
  
  En este artículo se muestra cómo utilizar la API de REST para administrar el control de acceso basado en rol.
* [Role-Based Access Control for Microsoft Azure from Ignite (Control de acceso basado en rol para Microsoft Azure de Ignite)](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  En este vídeo de la conferencia Microsoft Ignite de 2015 se tratan las funcionalidades de administración del acceso y la generación de informes en Azure. También se exploran los procedimientos recomendados para proteger el acceso a suscripciones de Azure con Azure Active Directory.
* [Autorización del acceso a aplicaciones web mediante OAuth 2.0 y Azure Active Directory](../active-directory/develop/v1-protocols-oauth-code.md)
  
  En este artículo se todo el flujo de OAuth 2.0 para realizar la autenticación con Azure Active Directory.
* [API de REST de administración de almacén de claves](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Este documento es la referencia para que las API de REST administren un almacén de claves mediante programación, donde se incluye el establecimiento de la directiva de acceso del almacén de claves.
* [API de REST de almacén de claves](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Vínculo a la documentación de referencia de la API de REST de almacén de claves.
* [Control de acceso de claves](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Vínculo a la documentación de referencia del control de acceso de secretos.
* [Control de acceso de secretos](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Vínculo a la documentación de referencia del control de acceso de claves.
* [Establecer](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) y [quitar](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy) una directiva de acceso de Key Vault mediante PowerShell
  
  Vínculos a documentación de referencia de los cmdlets de PowerShell para administrar la directiva de acceso de un almacén de claves.

## <a name="next-steps"></a>Pasos siguientes
[Configuración de firewalls y redes virtuales de Key Vault](key-vault-network-security.md)

Para ver un tutorial de introducción para administradores, consulte [Introducción a Azure Key Vault](key-vault-get-started.md).

Para más información acerca del registro de uso de Key Vault, consulte [Registro de Azure Key Vault](key-vault-logging.md).

Para más información acerca del uso de claves y secretos con Azure Key Vault, consulte [About keys, secrets, and certificates](https://msdn.microsoft.com/library/azure/dn903623.aspx) (Acerca de claves, secretos y certificados).

Si le queda alguna duda acerca de Key Vault, visite los [foros de Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault)

