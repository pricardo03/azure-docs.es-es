---
title: 'Registro de una aplicación de recursos en Azure Active Directory: Azure API for FHIR'
description: En este artículo se explica cómo registrar una aplicación de recursos en Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: e8305c5a69fa3fda29f4f1292b7faa59f8ec3608
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033712"
---
# <a name="register-a-resource-application-in-azure-active-directory"></a>Registro de una aplicación de recursos en Azure Active Directory

En este artículo aprenderá registrar una aplicación de recursos (o API) en Azure Active Directory. Una aplicación de recursos es una representación de Azure Active Directory de la misma API de FHIR Server y las aplicaciones cliente pueden solicitar acceso al recurso al autenticar. La aplicación de recursos también se conoce como la *audiencia* en términos de OAuth.

## <a name="app-registrations-in-azure-portal"></a>Registros de aplicaciones en Azure Portal

1. En el panel de navegación izquierdo de [Azure Portal](https://portal.azure.com), haga clic en **Azure Active Directory**.

2. En la hoja **Azure Active Directory**, haga clic en **Registros de aplicaciones (versión preliminar)** :

    ![Azure Portal. Nuevo registro de aplicaciones.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Haga clic en **Nuevo registro**.

## <a name="add-a-new-application-registration"></a>Adición de un nuevo registro de aplicaciones

Rellene los detalles de la nueva aplicación. No hay requisitos específicos para el nombre para mostrar, pero establece en el URI de FHIR Server facilita su búsqueda:

![Nuevo registro de aplicaciones](media/how-to-aad/portal-aad-register-new-app-registration-NAME.png)

## <a name="set-identifier-uri-and-define-scopes"></a>Establecimiento del URI de identificador y definición de ámbitos

Una aplicación de recursos tiene un URI de identificador (URI de identificador de aplicación), que los clientes pueden utilizar cuando solicitan acceso al recurso. Este valor se rellenará la notificación `aud` del token de acceso. Se recomienda establecer este URI para que sea el URI de FHIR Server. Para aplicaciones SMART en FHIR, se supone que la *audiencia* es el URI de FHIR Server.

1. Haga clic en **Exponer una API**.

2. Haga clic en **Establecer** junto a *URI de Id. de aplicación*.

3. Escriba el URI de identificador y haga clic en **Guardar**. Un identificador URI adecuado sería el URI de FHIR Server.

4. Haga clic en **Agregar un ámbito** y agregue todos los ámbitos que desea definir para la API. Azure AD actualmente no admite las barras diagonales (`/`) en los nombres de ámbito. Se recomienda usar `$` en su lugar. Un ámbito como `patient/*.read` sería `patient$*.read`.

    ![Audiencia y ámbito](media/how-to-aad/portal-aad-register-new-app-registration-AUD-SCOPE.png)

## <a name="define-application-roles"></a>Definición de los roles de aplicación

Azure API for FHIR y FHIR Server de código abierto utilizan los [roles de aplicación de Azure Active Directory](https://docs.microsoft.com/azure/architecture/multitenant-identity/app-roles) para el control de acceso basado en rol. Para definir los roles que deben estar disponibles para la API de FHIR Server, abra el [manifiesto](https://docs.microsoft.com/azure/active-directory/active-directory-application-manifest/) de la aplicación de recursos:

1. Haga clic en **Manifiesto**:

    ![Roles de la aplicación](media/how-to-aad/portal-aad-register-new-app-registration-APP-ROLES.png)

2. En la propiedad `appRoles`, agregue los roles que desea que tengan los usuarios o las aplicaciones:

    ```json
    "appRoles": [
      {
        "allowedMemberTypes": [
          "User",
          "Application"
        ],
        "description": "FHIR Server Administrators",
        "displayName": "admin",
        "id": "1b4f816e-5eaf-48b9-8613-7923830595ad",
        "isEnabled": true,
        "value": "admin"
      },
      {
        "allowedMemberTypes": [
          "User"
        ],
        "description": "Users who can read",
        "displayName": "reader",
        "id": "c20e145e-5459-4a6c-a074-b942bbd4cfe1",
        "isEnabled": true,
        "value": "reader"
      }
    ],
    ```

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido cómo registrar una aplicación de recursos en Azure Active Directory. A continuación, implemente una API FHIR en Azure.
 
>[!div class="nextstepaction"]
>[Implementación de FHIR Server de código abierto](fhir-oss-powershell-quickstart.md)