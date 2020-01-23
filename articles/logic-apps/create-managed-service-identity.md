---
title: Autenticación con identidades administradas
description: Acceder a los recursos de otros inquilinos de Azure Active Directory sin iniciar sesión con credenciales o secretos mediante el uso de una identidad administrada
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 10/21/2019
ms.openlocfilehash: 714faa43f34de965055ceba80de08972dd4192ac
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2020
ms.locfileid: "75861207"
---
# <a name="authenticate-access-to-azure-resources-by-using-managed-identities-in-azure-logic-apps"></a>Autenticar el acceso a los recursos de Azure mediante identidades administradas en Azure Logic Apps

Para obtener acceso a los recursos en otros inquilinos de Azure Active Directory (Azure AD) y autenticar su identidad sin iniciar sesión, su aplicación lógica puede usar una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) asignada por el sistema (anteriormente conocida como Managed Service Identity o MSI), en lugar de credenciales o secretos. Azure administra esta identidad y le ayuda a proteger las credenciales porque, de esta forma, no tiene que proporcionar secretos o cambiarlos. En este artículo se muestra cómo configurar y usar una identidad administrada asignada por el sistema para la aplicación lógica. Actualmente, las identidades administradas solo funcionan con [desencadenadores y acciones integrados específicos](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-to-outbound-calls), conectores o conexiones no administrados.

Para más información, consulte los temas siguientes:

* [Desencadenadores y acciones que admiten identidades administradas](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Servicios de Azure que admiten la autenticación de Azure AD con las identidades administradas](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication)
* [Tipos de autenticación que se admiten en llamadas salientes](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)
* [Límites de identidad administrada para aplicaciones lógicas](../logic-apps/logic-apps-limits-and-config.md#managed-identity)

## <a name="prerequisites"></a>Prerequisites

* Una suscripción de Azure, si no tiene una, [regístrese para obtener una cuenta gratuita de Azure](https://azure.microsoft.com/free/). La identidad administrada y el recurso Azure de destino al que quiere tener acceso deben tener la misma suscripción a Azure.

* [Permisos de administrador de Azure AD ](../active-directory/users-groups-roles/directory-assign-admin-roles.md) que puedan asignar roles a identidades administradas en el mismo inquilino de Azure AD que el recurso de destino. Para conceder acceso a una identidad administrada a un recurso de Azure, debe agregar un rol para esa identidad en el recurso de destino.

* El recurso Azure de destino para el que quiere obtener acceso

* Una aplicación lógica que utilice [desencadenadores y acciones que admiten identidades administradas](../logic-apps/logic-apps-securing-a-logic-app.md#add-authentication-outbound)

<a name="system-assigned"></a>

## <a name="enable-system-assigned-identity"></a>Activar una identidad asignada por el sistema

A diferencia de las identidades asignadas por el usuario, no tiene que crear manualmente la identidad asignada por el sistema. Para configurar la identidad asignada por el sistema de la aplicación lógica, estas son las opciones que puede usar:

* [Azure Portal](#azure-portal-system-logic-app)
* [Plantillas del Administrador de recursos de Azure](#template-system-logic-app)

<a name="azure-portal-system-logic-app"></a>

### <a name="enable-system-assigned-identity-in-azure-portal"></a>Activar una identidad asignada por el sistema en Azure Portal

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Identidad** > **Asignada por el sistema**. En **Estado**, seleccione **Activar** > **Guardar** > **Sí**.

   ![Activar la identidad asignada por el sistema](./media/create-managed-service-identity/turn-on-system-assigned-identity.png)

   Ahora, la aplicación lógica puede usar la identidad asignada por el sistema, que se registra con Azure Active Directory y se representa mediante un identificador de objeto.

   ![Identificador de objeto para la identidad asignada por el sistema](./media/create-managed-service-identity/object-id.png)

   | Propiedad | Value | Descripción |
   |----------|-------|-------------|
   | **Id. de objeto** | <*identity-resource-ID*> | Un identificador único global (GUID) que representa la identidad asignada por el sistema para la aplicación lógica en un inquilino de Azure AD |
   ||||

1. Ahora siga los [pasos que proporcionan a la identidad acceso al recurso](#access-other-resources).

<a name="template-system-logic-app"></a>

### <a name="enable-system-assigned-identity-in-azure-resource-manager-template"></a>Habilitar la identidad asignada por el sistema en la plantilla de Azure Resource Manager

Para automatizar la creación e implementación de los recursos de Azure, como las aplicaciones lógicas, puede usar [plantillas de Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md). Para habilitar la identidad administrada asignada por el sistema para la aplicación lógica en la plantilla, agregue el objeto `identity` y la propiedad secundaria `type` a la definición de recursos de la aplicación lógica en la plantilla, por ejemplo:

```json
{
   "apiVersion": "2016-06-01",
   "type": "Microsoft.logic/workflows",
   "name": "[variables('logicappName')]",
   "location": "[resourceGroup().location]",
   "identity": {
      "type": "SystemAssigned"
   },
   "properties": {
      "definition": {
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
         "actions": {},
         "parameters": {},
         "triggers": {},
         "contentVersion": "1.0.0.0",
         "outputs": {}
   },
   "parameters": {},
   "dependsOn": []
}
```

Cuando Azure crea la definición de recurso de la aplicación lógica, el objeto `identity` incluye estas propiedades adicionales:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Propiedad (JSON) | Value | Descripción |
|-----------------|-------|-------------|
| `principalId` | <*principal-ID*> | El identificador único global (GUID) del objeto de entidad de servicio para la identidad administrada que representa la aplicación lógica en el inquilino de Azure AD. Este GUID aparece a veces como "Id. de objeto" o `objectID`. |
| `tenantId` | <*Azure-AD-tenant-ID*> | El identificador único global (GUID) que representa un inquilino de Azure AD del que la aplicación lógica es ahora miembro. En el inquilino de Azure AD, la entidad de servicio tiene el mismo nombre que la instancia de aplicación lógica. |
||||

<a name="access-other-resources"></a>

## <a name="give-identity-access-to-resources"></a>Conceder acceso de identidad a los recursos

Antes de poder usar la identidad administrada asignada por el sistema de la aplicación lógica para la autenticación, conceda a esa identidad acceso al recurso de Azure en el que planee usar la identidad. Para completar esta tarea, asigne el rol adecuado a esa identidad en el recurso de Azure de destino. Estas son las opciones que puede usar:

* [Azure Portal](#azure-portal-assign-access)
* [Plantilla de Azure Resource Manager](../role-based-access-control/role-assignments-template.md)
* Azure PowerShell ([New-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment)): para más información, vea [Incorporación o eliminación de asignaciones de roles con RBAC de Azure y Azure PowerShell](../role-based-access-control/role-assignments-powershell.md).
* CLI de Azure ([az role assignment create](https://docs.microsoft.com/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-create)): para más información, vea [Incorporación o eliminación de asignaciones de roles con RBAC de Azure y la CLI de Azure](../role-based-access-control/role-assignments-cli.md).
* [API de REST de Azure](../role-based-access-control/role-assignments-rest.md)

<a name="azure-portal-assign-access"></a>

### <a name="assign-access-in-the-azure-portal"></a>Asignación del acceso en Azure Portal

1. En el [Azure Portal](https://portal.azure.com), vaya al recurso de Azure en el que quiere asignar acceso para la identidad administrada.

1. En el menú del recurso, seleccione **Control de acceso (IAM)**  > **asignaciones de roles** donde pueda revisar las asignaciones de roles actuales para ese recurso. En la barra de herramientas, seleccione **Agregar** > **Agregar asignación de roles**.

   ![Seleccione "Agregar" > "Agregar asignación de roles"](./media/create-managed-service-identity/add-role-to-resource.png)

   > [!TIP]
   > Si la opción **Agregar asignación de roles** está deshabilitada, lo más probable es que no tenga permisos. Para más información acerca de los permisos que le permiten administrar roles para los recursos, consulte [Permisos de roles de administrador en Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

1. En **Agregar asignación de roles**, seleccione un **Rol** que proporcione a su identidad el acceso necesario al recurso de destino.

   En el ejemplo de este tema, la identidad necesita un [rol que pueda tener acceso al BLOB en un contenedor de Azure Storage](../storage/common/storage-auth-aad.md#assign-rbac-roles-for-access-rights):

   ![Seleccione rol "Colaborador de datos de blobs de almacenamiento"](./media/create-managed-service-identity/assign-role.png)

1. En el cuadro **Asignar acceso a**, seleccione **Usuario, grupo o entidad de servicio de Azure AD**.

   ![Seleccione el acceso para la identidad asignada por el sistema](./media/create-managed-service-identity/assign-access-system.png)

1. En el cuadro **Seleccionar**, busque y seleccione la aplicación lógica.

   ![Seleccione la aplicación lógica para la identidad asignada por el sistema](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

1. Cuando finalice, seleccione **Guardar**.

   La lista de asignaciones de roles del recurso de destino muestra ahora la identidad administrada y el rol administrados.

   ![Se han agregado las identidades y roles administrados al recurso de destino](./media/create-managed-service-identity/added-roles-for-identities.png)

1. Ahora siga los [pasos para autenticar el acceso con la identidad](#authenticate-access-with-identity) en un desencadenador o una acción que admita identidades administradas.

<a name="authenticate-access-with-identity"></a>

## <a name="authenticate-access-with-managed-identity"></a>Autenticación del acceso con una identidad administrada

Después de [habilitar la identidad administrada para la aplicación lógica](#azure-portal-system-logic-app) y [conceder a esa identidad acceso al recurso o a la entidad de destino](#access-other-resources), puede usar esa identidad en [desencadenadores y acciones que admiten identidades administradas](logic-apps-securing-a-logic-app.md#managed-identity-authentication).

> [!IMPORTANT]
> Antes de que una función de Azure pueda usar la identidad asignada por el sistema, primero [habilite la autenticación de Azure Functions](../logic-apps/logic-apps-azure-functions.md#enable-authentication-for-azure-functions).

En estos pasos se muestra cómo usar la identidad administrada con un desencadenador o una acción a través del Azure Portal. Para especificar la identidad administrada en la definición de JSON subyacente de un desencadenador o una acción, consulte [Autenticación de identidad administrada](../logic-apps/logic-apps-securing-a-logic-app.md#managed-identity-authentication).

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. Si todavía no lo ha hecho, agregue el [desencadenador o la acción que admita identidades administradas](logic-apps-securing-a-logic-app.md#managed-identity-authentication).

   Por ejemplo, el desencadenador o la acción HTTP pueden usar la identidad asignada por el sistema que habilitó para la aplicación lógica. En general, el desencadenador o la acción HTTP utiliza estas propiedades para especificar el recurso o la entidad a los que desea obtener acceso:

   | Propiedad | Obligatorio | Descripción |
   |----------|----------|-------------|
   | **Método** | Sí | El método HTTP usado por la operación que desea ejecutar |
   | **URI** | Sí | La dirección URL del punto de conexión para acceder a la entidad o recurso de Azure de destino. La sintaxis del URI normalmente incluye el [Id. de recurso](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication) para el recurso o servicio de Azure. |
   | **Encabezados** | No | Los valores de encabezado que necesita o desea incluir en la solicitud saliente, como el tipo de contenido |
   | **Consultas** | No | Los parámetros de consulta que necesita o desea incluir en la solicitud, como el parámetro para una operación específica o la versión de la API para la operación que desea ejecutar |
   | **Autenticación** | Sí | El tipo de autenticación que se va a usar para autenticar el acceso al recurso o entidad de destino |
   ||||

   Como ejemplo específico, supongamos que desea ejecutar la [Operación instantánea de blob](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob) en un BLOB de la cuenta Azure Storage en la que previamente configuró el acceso para su identidad. Sin embargo, el [conector de Azure Blob Storage](https://docs.microsoft.com/connectors/azureblob/) no ofrece actualmente esta operación. En su lugar, puede ejecutar esta operación mediante la [acción HTTP](../logic-apps/logic-apps-workflow-actions-triggers.md#http-action) u otra [operación de la API REST de servicios blob](https://docs.microsoft.com/rest/api/storageservices/operations-on-blobs).

   > [!IMPORTANT]
   > Para acceder a cuentas de Azure Storage detrás de firewalls mediante solicitudes HTTP e identidades administradas, asegúrese de que también configura la cuenta de almacenamiento con la [excepción que permite el acceso a los servicios de Microsoft de confianza](../connectors/connectors-create-api-azureblobstorage.md#access-trusted-service).

   Para ejecutar la [operación blob de instantáneas](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob), la acción HTTP especifica estas propiedades:

   | Propiedad | Obligatorio | Valor de ejemplo | Descripción |
   |----------|----------|---------------|-------------|
   | **Método** | Sí | `PUT`| El método HTTP que utiliza la operación blob de instantáneas |
   | **URI** | Sí | `https://{storage-account-name}.blob.core.windows.net/{blob-container-name}/{folder-name-if-any}/{blob-file-name-with-extension}` | El identificador de recurso de un archivo Azure Blob Storage en el entorno de Azure Global (público), que usa esta sintaxis |
   | **Encabezados** | Sí, para Azure Storage | `x-ms-blob-type` = `BlockBlob` <p>`x-ms-version` = `2019-02-02` | Los valores de encabezado `x-ms-blob-type` y `x-ms-version` necesarios para las operaciones de Azure Storage. <p><p>**Importante**: En el desencadenador HTTP saliente y en las solicitudes de acción de Azure Storage, el encabezado requiere la propiedad `x-ms-version` y la versión de la API para la operación que se desea ejecutar. <p>Para más información, consulte los temas siguientes: <p><p>- [Encabezados de solicitud - Blob de instantáneas](https://docs.microsoft.com/rest/api/storageservices/snapshot-blob#request) <br>- [Control de versiones para servicios de Azure Storage](https://docs.microsoft.com/rest/api/storageservices/versioning-for-the-azure-storage-services#specifying-service-versions-in-requests) |
   | **Consultas** | Sí, para esta operación | `comp` = `snapshot` | El nombre y el valor del parámetro de consulta para la operación del blob de instantánea. |
   | **Autenticación** | Sí | `Managed Identity` | El tipo de autenticación que se va a usar para autenticar el acceso al blob de Azure |
   |||||

   Este es un ejemplo de la acción HTTP que muestra todos estos valores de propiedad:

   ![Incorporación de una acción HTTP para acceder a un recurso de Azure](./media/create-managed-service-identity/http-action-example.png)

   Para más información sobre las operaciones de API de REST de Azure disponibles, consulte la [referencia de API de REST en Azure](https://docs.microsoft.com/rest/api/azure/).

1. Desde la lista **Autenticación**, seleccione **Identidad administrada**. Si se admite la propiedad [**Autenticación**](logic-apps-securing-a-logic-app.md#add-authentication-outbound) pero está oculta, abra la lista **Agregar nuevo parámetro** y seleccione **Autenticación**.

   > [!NOTE]
   > No todos los desencadenadores y las acciones permiten seleccionar un tipo de autenticación. Para obtener más información, consulte [Incorporación de la autenticación en las llamadas salientes](logic-apps-securing-a-logic-app.md#add-authentication-outbound).

   ![En la propiedad "Autenticación", seleccione "Identidad administrada"](./media/create-managed-service-identity/select-managed-identity.png)

1. Después de seleccionar **Identidad administrada**, aparece la propiedad **Audiencia** para algunos desencadenadores y acciones. Si se admite la propiedad **Audiencia** pero está oculta, abra la lista **Agregar nuevo parámetro** y seleccione **Audiencia**.

1. Asegúrese de establecer el valor **Audiencia** en el identificador de recurso para el recurso o servicio de destino. De lo contrario, de forma predeterminada, la propiedad **Audiencia** usa el identificador de recurso `https://management.azure.com/`, que es el identificador de recurso para Azure Resource Manager.

   > [!IMPORTANT]
   > Asegúrese de que el identificador de este recurso de destino *coincide exactamente* con el valor esperado en Azure Active Directory (AD), incluida toda barra diagonal necesaria al final. Por ejemplo, el Id. de recurso para todas las cuentas de Azure Blob Storage requiere una barra diagonal final. Sin embargo, el Id. de recurso de una cuenta de almacenamiento específica no requiere una barra diagonal final. Verifique los [identificadores de recursos para los servicios de Azure que admiten Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication).

   En este ejemplo se establece la propiedad **Audiencia** en `https://storage.azure.com/`, de modo que los tokens de acceso que se usan para la autenticación sean válidos para todas las cuentas de almacenamiento. Sin embargo, también puede especificar una dirección URL de servicio raíz, `https://fabrikamstorageaccount.blob.core.windows.net`, para una cuenta de almacenamiento específica.

   ![Especificar el identificador de recurso de destino en la propiedad "Audiencia"](./media/create-managed-service-identity/specify-audience-url-target-resource.png)

   Para obtener más información sobre cómo autorizar el acceso con Azure AD para Azure Storage, consulte estos temas:

   * [Autorización del acceso a blobs y colas de Azure con Azure Active Directory](../storage/common/storage-auth-aad.md)
   * [Autorización del acceso a Azure Storage con Azure Active Directory](https://docs.microsoft.com/rest/api/storageservices/authorize-with-azure-active-directory#use-oauth-access-tokens-for-authentication)

<a name="remove-identity"></a>

## <a name="remove-system-assigned-identity"></a>Quitar la identidad asignada por el sistema

Para dejar de usar la identidad asignada por el sistema para la aplicación lógica, tiene estas opciones:

* [Azure Portal](#azure-portal-disable)
* [Plantillas del Administrador de recursos de Azure](#template-disable)
* [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.resources/remove-azroleassignment)
* [CLI de Azure](https://docs.microsoft.com/cli/azure/role/assignment?view=azure-cli-latest#az-role-assignment-delete)

Si se elimina la aplicación lógica, Azure quita automáticamente de Azure AD la identidad administrada.

<a name="azure-portal-disable"></a>

### <a name="remove-system-assigned-identity-in-the-azure-portal"></a>Remover una identidad asignada por el sistema en Azure Portal

En el Azure Portal, quite la identidad asignada por el sistema [de la aplicación lógica](#disable-identity-logic-app) y el acceso de la identidad [desde el recurso de destino](#disable-identity-target-resource).

<a name="disable-identity-logic-app"></a>

#### <a name="remove-system-assigned-identity-from-logic-app"></a>Quitar una identidad asignada por el sistema de una aplicación lógica

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Identidad** > **Asignada por el sistema**. En **Estado**, seleccione **Desactivar** > **Guardar** > **Sí**.

   ![Dejar de usar una identidad asignada por el sistema](./media/create-managed-service-identity/turn-off-system-assigned-identity.png)

<a name="disable-identity-target-resource"></a>

#### <a name="remove-identity-access-from-resources"></a>Quitar acceso de identidad a los recursos

1. En [Azure Portal](https://portal.azure.com), vaya al recurso destino de Azure donde quiere quitar el acceso para una identidad administrada.

1. En el menú del recurso de destino, seleccione **Control de acceso (IAM)** . En la barra de herramientas, seleccione **Asignación de roles**.

1. En la lista de roles, seleccione las identidades administradas que desea quitar. En la barra de herramientas, seleccione **Eliminar**.

   > [!TIP]
   > Si está deshabilitada la opción **Eliminar**, lo más probable es que no tenga los permisos correspondientes. Para más información acerca de los permisos que le permiten administrar roles para los recursos, consulte [Permisos de roles de administrador en Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

La identidad administrada se ha quitado y ya no tiene acceso al recurso de destino.

<a name="template-disable"></a>

### <a name="disable-managed-identity-in-azure-resource-manager-template"></a>Deshabilitar la identidad administrada con una plantilla de Azure Resource Manager

Si ha habilitado la identidad administrada por el sistema de la aplicación lógica mediante una plantilla de Azure Resource Manager, establezca la propiedad secundaria `type` del objeto `identity` en `None`. Esta acción también elimina el Id. principal de la identidad administrada por el sistema de Azure AD.

```json
"identity": {
   "type": "None"
}
```

## <a name="next-steps"></a>Pasos siguientes

* [Proteger el acceso y los datos en Azure Logic Apps](../logic-apps/logic-apps-securing-a-logic-app.md)
