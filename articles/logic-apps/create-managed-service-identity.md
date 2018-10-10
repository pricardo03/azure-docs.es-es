---
title: 'Acceso y autenticación sin iniciar sesión: Azure Logic Apps | Microsoft Docs'
description: Cree una identidad administrada para que la aplicación lógica pueda autenticarse y acceder a recursos de otros inquilinos de Azure Active Directory (Azure AD) sin sus credenciales
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: fb1c31e6e7c075e20191a4e51d7b1a9323f3b979
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973973"
---
# <a name="access-resources-and-authenticate-as-managed-identities-in-azure-logic-apps"></a>Acceso a recursos y autenticación como identidades administradas en Azure Logic Apps

Para obtener acceso a los recursos de otros inquilinos de Azure Active Directory (Azure AD) y autenticar la identidad sin iniciar sesión, puede crear una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) que la aplicación lógica usará en lugar de sus credenciales. Azure administra esta identidad y le ayuda a proteger las credenciales porque de esta forma no tiene que proporcionar secretos o cambiarlos. En este artículo se muestra cómo crear y usar una identidad administrada para la aplicación lógica. Para más información, consulte [Administrar identidades para los recursos de Azure](../app-service/app-service-managed-service-identity.md).

> [!NOTE]
> Managed Identities for Azure Resources es el nombre por el que ahora se conoce al servicio Managed Service Identity (MSI).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure, si no tiene una, <a href="https://azure.microsoft.com/free/" target="_blank">regístrese para obtener una cuenta gratuita de Azure</a>.

* La aplicación lógica en la que desea usar la identidad administrada. Si no tiene una aplicación lógica, consulte [Creación del primer flujo de trabajo de aplicación lógica](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="create-identity"></a>

## <a name="create-managed-identity"></a>Creación de una identidad administrada

Puede crear o habilitar una identidad administrada para la aplicación lógica mediante Azure Portal, las plantillas de Azure Resource Manager o Azure PowerShell. 

### <a name="azure-portal"></a>Azure Portal

Para crear una identidad administrada para la aplicación lógica mediante Azure Portal, active el valor **Registrar en Azure Active Directory** en la configuración del flujo de trabajo de la aplicación lógica.

1. En [Azure Portal](https://portal.azure.com), abra la aplicación lógica en Diseñador de aplicación lógica.

1. Siga estos pasos: 

   1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración del flujo de trabajo**. 

   1. En **Identidad de servicio administrada** > 
   **Registrar en Azure Active Directory**, elija **Activar**.

   1. Cuando haya terminado, seleccione **Guardar** en la barra de herramientas.

      ![Activación del valor de identidad administrada](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

      Azure muestra ahora estas propiedades y valores para la identidad administrada de la aplicación lógica:

      ![GUID del identificador de entidad y del identificador del inquilino](./media/create-managed-service-identity/principal-tenant-id.png)

      | Propiedad | Valor | DESCRIPCIÓN | 
      |----------|-------|-------------| 
      | **Identificador de entidad de seguridad** | <*principal-ID-GUID*> | Un identificador único global (GUID) que representa la aplicación lógica en un inquilino de Azure AD | 
      | **Id. de inquilino** | <*Azure-AD-tenant--ID-GUID*> | Un identificador único global (GUID) que representa un inquilino de Azure AD del que la aplicación lógica es ahora miembro. En el inquilino de Azure AD, la entidad de servicio tiene el mismo nombre que la instancia de aplicación lógica. | 
      ||| 

### <a name="deployment-template"></a>Plantilla de implementación

Para automatizar la creación e implementación de los recursos de Azure como las aplicaciones lógicas, puede configurar plantillas de Azure Resource Manager. Para más información consulte [Creación e implementación de aplicaciones lógicas con plantillas de Azure Resource Manager](../logic-apps/logic-apps-create-deploy-azure-resource-manager-templates.md). 

Para crear una identidad administrada para la aplicación lógica mediante una plantilla, agregue el elemento **identity** y la propiedad **type** a la definición del flujo de trabajo de la aplicación lógica en la plantilla de implementación. Estos valores indican que Azure crea y administra esta identidad para la aplicación lógica:

```json
"identity": {
    "type": "SystemAssigned"
}
```

Para ver un ejemplo, la aplicación lógica podría parecerse a esta versión:

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
    "principalId": "<principal-ID-GUID>",
    "tenantId": "<Azure-AD-tenant-ID>-GUID"
}
```

| Propiedad | Valor | DESCRIPCIÓN | 
|----------|-------|-------------|
| **principalId** | <*principal-ID-GUID*> | Un identificador único global (GUID) que representa la aplicación lógica en el inquilino de Azure AD | 
| **tenantId** | <*Azure-AD-tenant--ID-GUID*> | Un identificador único global (GUID) que representa un inquilino de Azure AD del que la aplicación lógica es ahora miembro. En el inquilino de Azure AD, la entidad de servicio tiene el mismo nombre que la instancia de aplicación lógica. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Acceso a recursos con la identidad administrada

Después de crear una identidad administrada para la aplicación lógica, puede [proporcionar a dicha identidad acceso a otros recursos](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). A continuación, puede usar esa identidad administrada para la autenticación, como cualquier otra [entidad de servicio](../active-directory/develop/app-objects-and-service-principals.md). 

Por ejemplo, supongamos que ya ha configurado una aplicación lógica con una identidad administrada que tenga acceso a otro recurso. Ahora puede agregar una acción HTTP para que la aplicación lógica pueda enviar una solicitud HTTP o llamada a ese recurso. 

1. Agregue la acción **HTTP** a la aplicación lógica. 

1. Proporcione la información necesaria para dicha acción, por ejemplo, el **método** de la solicitud y la ubicación del **URI** para el recurso que desea llamar.

1. En la acción HTTP, elija **Mostrar opciones avanzadas**. 

1. En la lista **Autenticación**, seleccione **Managed Service Identity**, que a continuación muestra la propiedad **Audiencia** para que establezca:

   ![Seleccione "Managed Service Identity"](./media/create-managed-service-identity/select-managed-service-identity.png)

1. Siga creando la aplicación lógica de la forma que desee.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Eliminación de una identidad administrada

Para deshabilitar una identidad administrada en la aplicación lógica, puede seguir unos pasos similares a los que realizó al crear la identidad mediante Azure Portal, las plantillas de implementación de Azure Resource Manager o Azure PowerShell. 

Cuando se elimina la aplicación lógica, Azure quita automáticamente de Azure AD la identidad asignada por el sistema a la aplicación lógica.

### <a name="azure-portal"></a>Azure Portal

1. Abra la aplicación lógica en el Diseñador de aplicación lógica.

1. Siga estos pasos: 

   1. En el menú de la aplicación lógica, en **Configuración**, seleccione **Configuración del flujo de trabajo**. 
   
   1. En **Identidad de servicio administrada**, elija **Desactivar** para la propiedad **Registrar en Azure Active Directory**.

   1. Cuando haya terminado, seleccione **Guardar** en la barra de herramientas.

      ![Desactivación del valor de identidad administrada](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Plantilla de implementación

Si ha creado la identidad administrada de la aplicación lógica con una plantilla de implementación de Azure Resource Manager, establezca la propiedad `"type"` elemento `"identity"` en `"None"`. Esta acción también elimina el identificador de entidad de seguridad de Azure AD. 

```json
"identity": {
    "type": "None"
}
```

## <a name="get-support"></a>Obtención de soporte técnico

* Si tiene alguna duda, visite el [foro de Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Para enviar ideas sobre características o votar sobre ellas, visite el [sitio de comentarios de los usuarios de Logic Apps](http://aka.ms/logicapps-wish).
