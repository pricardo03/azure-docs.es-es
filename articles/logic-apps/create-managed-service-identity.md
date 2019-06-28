---
title: 'Autenticación con identidades administradas: Azure Logic Apps | Microsoft Docs'
description: Para autenticarse sin iniciar sesión, puede crear una identidad administrada (anteriormente denominada Managed Service Identity o MSI) para que su aplicación lógica pueda acceder a los recursos de los otros inquilinos de Azure Active Directory (Azure AD) sin usar credenciales ni secretos.
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 03/29/2019
ms.openlocfilehash: 65fe89bf775a649d5654ce739d8d18e05d3048ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65416114"
---
# <a name="authenticate-and-access-resources-with-managed-identities-in-azure-logic-apps"></a>Autenticación y acceso a los recursos con identidades administradas en Azure Logic Apps

Para obtener acceso a los recursos en otros inquilinos de Azure Active Directory (Azure AD) y autenticar su identidad sin iniciar sesión, su aplicación lógica puede usar una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) (anteriormente conocida como Managed Service Identity o MSI), en lugar de credenciales o secretos. Azure administra esta identidad y le ayuda a proteger las credenciales porque, de esta forma, no tiene que proporcionar secretos o cambiarlos. En este artículo se muestra cómo configurar y usar una identidad administrada asignada por el sistema para la aplicación lógica. Para obtener más información sobre las identidades administradas, consulte [¿Qué es Managed Identities for Azure Resources?](../active-directory/managed-identities-azure-resources/overview.md)

> [!NOTE]
> La aplicación lógica puede usar las identidades administradas solo con los conectores que admiten identidades administradas. Actualmente, solo el conector HTTP admite las identidades administradas.
>
> Actualmente, puede tener hasta 10 flujos de trabajo de la aplicación lógica con las identidades administradas asignadas por el sistema en cada suscripción a Azure.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure, si no tiene una, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* La aplicación lógica en la que quiere usar la identidad administrada asignada por el sistema. Si no tiene una aplicación lógica, consulte [Creación del primer flujo de trabajo de aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="enable-identity"></a>

## <a name="enable-managed-identity"></a>Habilitación de una entidad administrada

Para las identidades administradas asignadas por el sistema, no tiene que crear esa identidad manualmente. Para configurar una identidad administrada asignada por el sistema para la aplicación lógica, puede usar las siguientes maneras: 

* [Azure Portal](#azure-portal) 
* [Plantillas del Administrador de recursos de Azure](#template) 
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/howto-assign-access-powershell.md) 

<a name="azure-portal"></a>

### <a name="azure-portal"></a>Portal de Azure

Para habilitar una identidad administrada asignada por el sistema para la aplicación lógica mediante Azure Portal, active el valor **Asignado por el sistema** en la configuración de la identidad de la aplicación lógica.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Identidad**. 

1. En **Asignado por el sistema** > **Estado**, elija **Activado**. Después, elija **Guardar** > **Sí**.

   ![Activación del valor de identidad administrada](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

   La aplicación lógica ahora tiene una identidad administrada asignada por el sistema registrada en Azure Active Directory:

   ![GUID para el Id. de objeto](./media/create-managed-service-identity/object-id.png)

   | Propiedad | Valor | DESCRIPCIÓN | 
   |----------|-------|-------------| 
   | **Id. de objeto** | <*identity-resource-ID*> | Un identificador único global (GUID) que representa la identidad administrada asignada por el sistema para la aplicación lógica en un inquilino de Azure AD | 
   ||| 

<a name="template"></a>

### <a name="azure-resource-manager-template"></a>Plantilla del Administrador de recursos de Azure

Si quiere automatizar la creación e implementación de los recursos de Azure, como las aplicaciones lógicas, puede usar [plantillas de Azure Resource Manager](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md). Para crear una identidad administrada asignada por el sistema para su aplicación lógica mediante una plantilla, agregue el elemento `"identity"` y la propiedad `"type"` a la definición del flujo de trabajo de la aplicación lógica en la plantilla de implementación: 

```json
"identity": {
   "type": "SystemAssigned"
}
```

Por ejemplo:

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

Cuando Azure crea la aplicación lógica, la definición de flujo de trabajo de la aplicación lógica incluye estas propiedades adicionales:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Propiedad | Valor | DESCRIPCIÓN | 
|----------|-------|-------------|
| **principalId** | <*principal-ID*> | Un identificador único global (GUID) que representa la aplicación lógica en el inquilino de Azure AD y, a veces, aparece como un "Id. de objeto" o `objectID` | 
| **tenantId** | <*Azure-AD-tenant-ID*> | Un identificador único global (GUID) que representa un inquilino de Azure AD del que la aplicación lógica es ahora miembro. En el inquilino de Azure AD, la entidad de servicio tiene el mismo nombre que la instancia de aplicación lógica. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Acceso a recursos con la identidad administrada

Después de crear una identidad administrada asignada por el sistema para la aplicación lógica, puede [proporcionar a dicha identidad acceso a otros recursos de Azure](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). A continuación, puede usar esa identidad para la autenticación, como cualquier otra [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md). 

> [!NOTE]
> La identidad administrada asignada por el sistema y el recurso en el que quiere asignar acceso deben tener la misma suscripción a Azure.

### <a name="assign-access-to-managed-identity"></a>Asignación de acceso a la identidad administrada

Para proporcionar acceso a otro recurso de Azure para la identidad administrada asignada por el sistema de la aplicación lógica, siga estos pasos:

1. En Azure Portal, vaya al recurso de Azure en el que quiere asignar acceso para la identidad administrada. 

1. En el menú del recurso, seleccione **Control de acceso (IAM)** . En la barra de herramientas, seleccione **Agregar** > **Agregar asignación de roles**.

   ![Agregar asignación de roles](./media/create-managed-service-identity/add-permissions-logic-app.png)

1. En **Add role assignment** (Agregar asignación de roles), seleccione el **Rol** que quiera para la identidad. 

1. En la propiedad **Asignar acceso a**, seleccione la opción **Usuario, grupo o entidad de servicio de Azure AD**, si aún no está seleccionada.

1. En el cuadro **Seleccionar**, escriba el nombre de la aplicación lógica empezando por el primer carácter del nombre de la aplicación lógica. Cuando aparezca la aplicación lógica, selecciónela.

   ![Selección de la aplicación lógica con la identidad administrada](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

1. Cuando termine, seleccione **Guardar**.

### <a name="authenticate-with-managed-identity-in-logic-app"></a>Autenticación con una identidad administrada en la aplicación lógica

Después de configurar la aplicación lógica con una identidad administrada asignada por el sistema y de asignar acceso al recurso que quiere para esa identidad, puede usar dicha identidad para la autenticación. Por ejemplo, puede usar una acción HTTP para que su aplicación lógica pueda enviar una solicitud HTTP o una llamada a ese recurso. 

1. Agregue la acción **HTTP** a la aplicación lógica.

1. Proporcione la información necesaria para dicha acción, por ejemplo, el **método** de la solicitud y la ubicación del **URI** para el recurso que desea llamar.

   Por ejemplo, supongamos que usa la autenticación de Azure Active Directory (Azure AD) con [uno de estos servicios de Azure que admiten Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). 
   En el cuadro **URI**, escriba la dirección URL del punto de conexión para ese servicio de Azure. 
   Por lo tanto, si usa Azure Resource Manager, especifique este valor en la propiedad **URI**:

   `https://management.azure.com/subscriptions/<Azure-subscription-ID>?api-version=2016-06-01`

1. En la acción HTTP, elija **Mostrar opciones avanzadas**.

1. Desde la lista **Autenticación**, seleccione **Identidad administrada**. Después de seleccionar esta autenticación, la propiedad **Audiencia** aparece con el valor de Id. de recurso predeterminado:

   ![Seleccione "Identidad administrada".](./media/create-managed-service-identity/select-managed-service-identity.png)

   > [!IMPORTANT]
   > 
   > En la propiedad **Audiencia**, el valor de Id. de recurso debe coincidir exactamente con lo que espera Azure AD, incluida toda barra diagonal necesaria al final. 
   > Puede encontrar estos valores de Id. de recurso en esta [tabla que describe servicios de Azure que admiten Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). 
   > Por ejemplo, si usa el Id. de recurso de Azure Resource Manager, asegúrese de que el URI tenga una barra diagonal al final.

1. Siga creando la aplicación lógica de la forma que desee.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Eliminación de una identidad administrada

Para deshabilitar una identidad administrada asignada por el sistema en la aplicación lógica, puede seguir unos pasos similares a los que realizó al configurar la identidad mediante Azure Portal, las plantillas de implementación de Azure Resource Manager o Azure PowerShell.

Cuando se elimina la aplicación lógica, Azure quita automáticamente de Azure AD la identidad asignada por el sistema a la aplicación lógica.

### <a name="azure-portal"></a>Portal de Azure

Para quitar una identidad administrada asignada por el sistema para la aplicación lógica mediante Azure Portal, desactive el valor **Asignado por el sistema** en la configuración de la identidad de la aplicación lógica.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Identidad**. 

1. En **Asignado por el sistema** > **Estado**, elija **Desactivado**. Después, elija **Guardar** > **Sí**.

   ![Desactivación del valor de identidad administrada](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Plantilla de implementación

Si ha creado la identidad administrada asignada por el sistema de la aplicación lógica con una plantilla de implementación de Azure Resource Manager, establezca la propiedad `"type"` del elemento `"identity"` en `"None"`. Esta acción también elimina el identificador de entidad de seguridad de Azure AD.

```json
"identity": {
   "type": "None"
}
```

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](https://aka.ms/logicapps-wish).
