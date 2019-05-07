---
title: 'Aplicación móvil que llama a web API: configuración de la aplicación de código | Plataforma de identidad de Microsoft'
description: Aprenda a crear una aplicación móvil que llama a web API (configuración de código de la aplicación)
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: dadobali
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9eac9d1dfce79ac4ad9d49a6cfe6b7dee7f6681a
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075166"
---
# <a name="mobile-app-that-calls-web-apis---app-registration"></a>Aplicación móvil que llama a web API: registro de la aplicación

En este artículo contiene las instrucciones de registro de aplicación para crear una aplicación móvil.

## <a name="supported-account-types"></a>Tipos de cuenta admitidos

Los tipos de cuenta que se admiten en aplicaciones móviles dependen de la experiencia que desea habilitar y los usuarios que se destina la aplicación.

## <a name="platform-configuration-and-redirect-uris"></a>Configuración de la plataforma y el URI de redireccionamiento  

Al compilar una aplicación móvil, el paso de registro más importante es el URI de redireccionamiento. Esto se puede establecer a través de la [configuración de la plataforma en la hoja autenticación](https://aka.ms/MobileAppReg).

Esta experiencia permitirá a la aplicación para obtener el inicio de sesión único (SSO) a través de Microsoft Authenticator (y el Portal de empresa de Intune en Android), así como compatibilidad con las directivas de administración de dispositivos.

Si prefiere configurar manualmente el URI de redirección, puede hacerlo mediante el manifiesto de aplicación. El formato recomendado es el siguiente:

- ***iOS***: `msauth.<BUNDLE_ID>://auth`
- ***Android***: `msauth://<PACKAGE_NAME>/<SIGNATURE_HASH>`
  - El hash de firma de Android puede generarse utilizando las teclas de lanzamiento o depuración a través del comando KeyTool.

## <a name="api-permissions"></a>Permisos de API

Aplicaciones móviles, llamar a las API en nombre del usuario con sesión iniciada. La aplicación debe solicitar permisos delegados, que también se denomina ámbitos. Dependiendo de la experiencia deseada, esto puede hacerse estáticamente a través del portal de Azure o dinámicamente en tiempo de ejecución. Registrar estáticamente permisos permite a los administradores aprobar fácilmente su aplicación y se recomienda.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adquirir un token](scenario-mobile-acquire-token.md)
