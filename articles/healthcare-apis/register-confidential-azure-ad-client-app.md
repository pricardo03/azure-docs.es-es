---
title: 'Registro de una aplicación de cliente confidencial en Azure Active Directory: Azure API for FHIR'
description: En este artículo se explica cómo registrar una aplicación cliente confidencial en Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 53ebce4151bfc9ef2324f0e75eb7d199d9479e83
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033722"
---
# <a name="register-a-confidential-client-application-in-azure-active-directory"></a>Registro de una aplicación cliente confidencial en Azure Active Directory

En este artículo aprenderá a registrar una aplicación cliente confidencial en Azure Active Directory. Un registro de aplicación cliente es una representación de Azure Active Directory de una aplicación que se puede usar para autenticar en nombre de un usuario y solicitar acceso a las [aplicaciones de recursos](register-resource-azure-ad-client-app.md). Una aplicación cliente confidencial es una aplicación en la que se puede confiar para almacenar un secreto y presentar dicho secreto cuando se solicitan tokens de acceso. Ejemplos de aplicaciones confidenciales son las aplicaciones de servidor.

Para registrar una nueva aplicación confidencial en el portal, siga estos pasos.

## <a name="app-registrations-in-azure-portal"></a>Registros de aplicaciones en Azure Portal

1. En el panel de navegación izquierdo de [Azure Portal](https://portal.azure.com), haga clic en **Azure Active Directory**.

2. En la hoja **Azure Active Directory**, haga clic en **Registros de aplicaciones (versión preliminar)** :

    ![Azure Portal. Nuevo registro de aplicaciones.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Haga clic en **Nuevo registro**.

## <a name="register-a-new-application"></a>Registro de una nueva aplicación

1. Asigne un nombre para mostrar a la aplicación.

2. Proporcione una dirección URL de respuesta. Estos detalles se pueden cambiar más adelante, pero si conoce la dirección URL de respuesta de la aplicación, indíquelo ahora.

    ![Nuevo registro de la aplicación cliente confidencial.](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT.png)

## <a name="api-permissions"></a>Permisos de API

A continuación, agregue los permisos de API:

1. Abra los **permisos de API**:

    ![Cliente confidencial. Permisos de API](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-Permissions.png)

2. Haga clic en **Agregar un permiso**.

3. Seleccione la API de recursos adecuada:

    Para Azure API for FHIR (servicio administrado), haga clic en **API usadas en mi organización** y busque "Azure Healthcare API". Para el servidor FHIR de código abierto para Azure, seleccione el [registro de la aplicación de recursos de la API de FHIR](register-resource-azure-ad-client-app.md):

    ![Cliente confidencial. Mis API](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-MyApis.png)

4. Seleccione los ámbitos (permisos) que la aplicación confidencial debería poder solicitar en nombre del usuario:

    ![Cliente confidencial. Permisos delegados](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-API-DelegatedPermissions.png)

## <a name="application-secret"></a>Secreto de aplicación

1. Cree un secreto de aplicación (secreto de cliente):

    ![Cliente confidencial. Secreto de aplicación](media/how-to-aad/portal-aad-register-new-app-registration-CONF-CLIENT-SECRET.png)

2. Proporcione una descripción y duración del secreto.

3. Una vez generado, se mostrará en el portal una sola vez. Tome nota del mismo y guárdelo de forma segura.

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido cómo registrar una aplicación cliente confidencial en Azure Active Directory. A continuación, implemente una API FHIR en Azure.
 
>[!div class="nextstepaction"]
>[Implementación de FHIR Server de código abierto](fhir-oss-powershell-quickstart.md)