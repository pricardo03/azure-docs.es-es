---
title: Seguridad de Azure Key Vault | Microsoft Docs
description: Administración de los permisos de acceso para Azure Key Vault, claves y secretos. Trata sobre el modelo de autenticación y autorización de Key Vault y cómo proteger un almacén de claves.
services: key-vault
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/18/2019
ms.author: barclayn
Customer intent: As a key vault administrator, I want to learn the options available to secure my vaults
ms.openlocfilehash: 5b32e4897e718e0e411caf9ba76b036f1352bde0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64715284"
---
# <a name="azure-key-vault-security"></a>Seguridad de Azure Key Vault

Debe proteger las claves de cifrado y secretos, como certificados, las cadenas de conexión y contraseñas en la nube, de modo que usa Azure Key Vault. Ya que está almacenando confidenciales y datos empresariales fundamentales, deberá tomar medidas para maximizar la seguridad de los almacenes y los datos almacenados en ellas. En este artículo se tratan algunos de los conceptos que se deben considerar al diseñar la seguridad de Azure Key Vault.

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

Cuando se crea un almacén de claves en una suscripción de Azure, se asocia automáticamente al inquilino de Azure AD de dicha suscripción. Cualquier persona que intente administrar o recuperar el contenido desde un almacén debe autenticarse con Azure AD.

- La autenticación establece la identidad del llamador.
- La autorización determina las operaciones que puede llevar a cabo el llamador. Autorización en Key Vault usa una combinación de [control de acceso basado en roles](../role-based-access-control/overview.md) (RBAC) y las directivas de acceso de Azure Key Vault.

### <a name="access-model-overview"></a>Introducción al modelo acceso

Acceso a los almacenes realiza a través de dos interfaces o planos. Estos planos son el plano de administración y el plano de datos.

- El *plano de administración* es donde administrar Key Vault sí mismo y es la interfaz utilizada para crear y eliminar almacenes. También puede leer las propiedades del almacén de claves y administrar directivas de acceso.
- El *plano de datos* le permite trabajar con los datos almacenados en un almacén de claves. Puede agregar, eliminar y modificar claves, secretos y certificados.

Para obtener acceso a un almacén de claves en cualquier plano, todos los llamadores (usuarios o aplicaciones) deben autenticarse y autorizarse. Ambos planos usan Azure Active Directory (Azure AD) para la autenticación. Para la autorización, el plano de administración usa el control de acceso basado en rol (RBAC) y el plano de datos utiliza la directiva de acceso de Key Vault.

El modelo de un único mecanismo de autenticación para ambos planos tiene varias ventajas:

- Las organizaciones pueden controlar el acceso de forma centralizada a todos los almacenes de claves de su organización.
- Si un usuario abandona la organización, al instante pierde el acceso a todos los almacenes de claves de la organización.
- Las organizaciones pueden personalizar la autenticación mediante las opciones de Azure AD, por ejemplo, para habilitar la autenticación multifactor para una mayor seguridad

### <a name="managing-administrative-access-to-key-vault"></a>Administración del acceso administrativo al almacén de claves

Cuando se crea un almacén de claves en un grupo de recursos, administrar el acceso mediante el uso de Azure AD. Puede conceder a usuarios o grupos la capacidad de administrar los almacenes de claves en un grupo de recursos. Puede conceder acceso a un nivel de ámbito específico mediante la asignación de roles de RBAC apropiados. Para conceder acceso a un usuario para administrar almacén de claves, debe asignar un rol `key vault Contributor` predefinido al usuario en un ámbito específico. Los siguientes niveles de ámbitos se pueden asignar a un rol de RBAC:

- **Suscripción**: Un rol de RBAC asignado al nivel de suscripción se aplica a todos los recursos y grupos de recursos de esa suscripción.
- **Grupo de recursos**: Un rol de RBAC asignado al nivel de grupo de recursos se aplica a todos los recursos de dicho grupo de recursos.
- **Recurso específico**: un rol de RBAC asignado a un recurso concreto se aplica a dicho recurso. En este caso, el recurso es un almacén de claves específico.

Existen varios roles predefinidos. Si un rol predefinido no se ajusta a sus necesidades, puede definir uno propio. Para más información, vea [RBAC: roles integrados](../role-based-access-control/built-in-roles.md).

> [!IMPORTANT]
> Si un usuario tiene permisos `Contributor` en un plano de administración de Key Vault, se puede conceder a sí mismo acceso al plano de datos estableciendo una directiva de acceso de Key Vault. Debe controlar estrechamente quién tiene el rol de acceso `Contributor` a los almacenes de claves. Asegúrese de que solo las personas autorizadas pueden acceder a los almacenes de claves, las claves, los secretos y los certificados y administrarlos.

<a id="data-plane-access-control"></a>
### <a name="controlling-access-to-key-vault-data"></a>Controlar el acceso a datos de Key Vault

Las directivas de acceso de Key Vault conceden permisos por separado a las claves, secretos o certificados. Puede conceder acceso a un usuario solo a las claves y no a los secretos. Permisos de acceso para claves, secretos y certificados se administran en el nivel de almacén.

> [!IMPORTANT]
> Las directivas de acceso de Key Vault no admiten permisos de nivel de objeto pormenorizados, como una clave, un secreto o un certificado específicos. Cuando se concede a un usuario permiso para crear y eliminar claves, este puede realizar dichas operaciones en todas las claves de dicho almacén de claves.

Para establecer directivas de acceso de un almacén de claves, use [Azure Portal](https://portal.azure.com/), la [CLI de Azure](../cli-install-nodejs.md), [Azure PowerShell](/powershell/azureps-cmdlets-docs) o las [API de REST de administración de Key Vault](https://msdn.microsoft.com/library/azure/mt620024.aspx).

Puede restringir el acceso al plano de datos mediante el uso de [puntos de conexión de servicio de red virtual para Azure Key Vault](key-vault-overview-vnet-service-endpoints.md). Puede configurar [firewall y reglas de red virtual](key-vault-network-security.md) para una capa adicional de seguridad.

## <a name="network-access"></a>Acceso de red

Puede reducir la exposición de los almacenes especificando las direcciones IP que tienen acceso a ellos. Los puntos de conexión de servicio de red virtual para Azure Key Vault permiten restringir el acceso a una red virtual especificada. También permiten restringir el acceso a una lista de intervalos de direcciones IPv4 (protocolo de Internet, versión 4). A todos los usuarios que se conecten a su almacén de claves desde fuera de esos orígenes se les negará el acceso.

Después de firewall de las reglas son en efecto, los usuarios solo pueden leer datos de Key Vault cuando las solicitudes proceden de redes virtuales permitidas o intervalos de direcciones IPv4. Esto también se aplica al acceso de Key Vault desde Azure Portal. Aunque los usuarios pueden ir a un almacén de claves desde Azure Portal, es posible que no pueda enumerar las claves, los secretos o los certificados si su equipo cliente no está en la lista de dispositivos permitidos. Esto también afecta al selector de Key Vault de otros servicios de Azure. Los usuarios podrían ver la lista de almacenes de claves, pero no enumerar las claves si las reglas del firewall limitan su equipo cliente.

Para obtener más información sobre Azure Key Vault de revisión de la dirección de red [extremos de servicio de red Virtual para Azure Key Vault](key-vault-overview-vnet-service-endpoints.md)

## <a name="monitoring"></a>Supervisión

Registro de Key Vault guarda información acerca de las actividades realizadas en el almacén. Registros de Key Vault:

- Todas las solicitudes de API de REST, incluidas las solicitudes con error de autenticarse
  - Las operaciones en la clave del almacén de sí mismo. Estas operaciones incluyen la creación, eliminación, establecimiento de directivas de acceso y actualizar los atributos del almacén de claves como las etiquetas.
  - Operaciones en claves y secretos del almacén de claves, incluidos:
    - Crear, modificar o eliminar estas claves o secretos.
    - Firma, comprobar, cifrar, descifrar, encapsular y desencapsular claves, obtener secretos y lista de claves y secretos (y sus versiones).
- Solicitudes no autenticadas que dan como resultado una respuesta 401. Algunos ejemplos son las solicitudes que no tienen un token de portador, que son un formato incorrecto o ha expirado o que tienen un token no válido.

Puede tener acceso a la información de registro en 10 minutos después de la operación de almacén de claves. Depende de administrar los registros en la cuenta de almacenamiento. 

- Utilice los métodos de control de acceso de Azure estándar para proteger los registros mediante la restricción de quién puede tener acceso a ellos.
- Elimine los registros que ya no desee mantener en la cuenta de almacenamiento.

Para la recomendación sobre la administración segura de almacenamiento revisión cuentas el [Guía de seguridad de Azure Storage](../storage/common/storage-security-guide.md)

## <a name="next-steps"></a>Pasos siguientes

- [Puntos de conexión de servicio de red virtual para Azure Key Vault](key-vault-overview-vnet-service-endpoints.md)
- [RBAC: roles integrados](../role-based-access-control/built-in-roles.md)
- [Puntos de conexión de servicio de red virtual para Azure Key Vault](key-vault-overview-vnet-service-endpoints.md)
