---
title: 'Registro de una aplicación cliente pública en Azure Active Directory: Azure API for FHIR'
description: En este artículo se explica cómo registrar una aplicación cliente pública en Azure Active Directory.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 69504bc9ba0420b47a26519a0b112ff102171254
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033704"
---
# <a name="register-a-public-client-application-in-azure-active-directory"></a>Registro de una aplicación cliente pública en Azure Active Directory

En este artículo aprenderá a registrar una aplicación pública en Azure Active Directory. Los registros de aplicaciones cliente son representaciones de Azure Active Directory de aplicaciones que pueden autenticar y solicitar permisos de API en nombre de un usuario. Los clientes públicos son aplicaciones como las aplicaciones móviles y las aplicaciones Javascript de una sola página que no pueden mantener la confidencialidad de los secretos. El procedimiento es similar a [registrar un cliente confidencial](register-confidential-azure-ad-client-app.md), pero dado que no se puede confiar en los clientes públicos para almacenar un secreto de aplicación, no es necesario agregar uno.

## <a name="app-registrations-in-azure-portal"></a>Registros de aplicaciones en Azure Portal

1. En el panel de navegación izquierdo de [Azure Portal](https://portal.azure.com), haga clic en **Azure Active Directory**.

2. En la hoja **Azure Active Directory**, haga clic en **Registros de aplicaciones (versión preliminar)** :

    ![Azure Portal. Nuevo registro de aplicaciones.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Haga clic en **Nuevo registro**.

## <a name="application-registration-overview"></a>Introducción al registro de aplicaciones

1. Asigne a la aplicación un nombre para mostrar.

2. Proporcione una dirección URL de respuesta. La dirección URL de respuesta es donde los códigos de autenticación se devolverán a la aplicación cliente. Puede agregar más direcciones URL de respuesta y editar las existentes más adelante.

    ![Azure Portal. Nuevo registro de aplicaciones pública.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-NAME.png)

## <a name="api-permissions"></a>Permisos de API

De forma similar a la [aplicación cliente confidencial](register-confidential-azure-ad-client-app.md), deberá seleccionar los permisos de API que esta aplicación debería poder solicitar en nombre de los usuarios:

1. Abra los **permisos de API** y seleccione el [registro de aplicaciones de recursos de FHIR API](register-resource-azure-ad-client-app.md):

    ![Azure Portal. Nuevos permisos de API públicos.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-API-PERMISSIONS.png)

2. Seleccione los ámbitos que desea que la aplicación pueda solicitar.

## <a name="next-steps"></a>Pasos siguientes

En este artículo ha aprendido cómo registrar una aplicación cliente pública en Azure Active Directory. A continuación, implemente una FHIR API en Azure.
 
>[!div class="nextstepaction"]
>[Implementación de FHIR Server de código abierto](fhir-oss-powershell-quickstart.md)
