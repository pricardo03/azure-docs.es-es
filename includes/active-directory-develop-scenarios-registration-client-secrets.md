---
title: archivo de inclusión
description: archivo de inclusión para el escenario de cliente confidencial (demonio, aplicación Web, Web API) de páginas de aterrizaje
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/18/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 9ee7422b372993d60c629524eb036b9678e5776c
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074656"
---
## <a name="registration-of-secrets-or-certificates"></a>Registro de certificados o secretos

Al igual que para cualquier aplicación cliente confidencial, debe registrar un secreto o certificado. Puede registrar los secretos de aplicación a través de la experiencia interactiva en el [portal Azure](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredAppsPreview), o mediante herramientas de línea de comandos (por ejemplo, PowerShell)

### <a name="registering-client-secrets-using-the-application-registration-portal"></a>Registrar los secretos de cliente mediante el portal de registro de aplicación

La administración de credenciales de cliente se produce en el **certificados y secretos** página para una aplicación:

![image](../articles/active-directory/develop/media/quickstart-update-azure-ad-app-preview/credentials-certificates-secrets-expanded.png)

- el secreto de aplicación (también el secreto de cliente con nombre) se genera por Azure AD, durante el registro de la aplicación cliente confidencial. Esta generación ocurre cuando se selecciona **nuevo secreto de cliente**. En ese momento, debe copiar la cadena de secreto en el Portapapeles para utilizarlas en su aplicación, antes de seleccionar **guardar**. Esta cadena no aparecerá más largo.
- el certificado se carga en el registro de aplicación mediante el **cargar certificado** botón

Para obtener más información, consulte [inicio rápido: Configurar una aplicación cliente para tener acceso a las API web | Agregar credenciales a la aplicación](../articles/active-directory/develop/quickstart-configure-app-access-web-apis.md#add-credentials-to-your-web-application)

### <a name="registering-client-secrets-using-powershell"></a>Registrar los secretos de cliente mediante PowerShell

Como alternativa, puede registrar la aplicación con Azure AD mediante herramientas de línea de comandos. El [active directory dotnetcore daemon v2](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2) ejemplo muestra cómo registrar un secreto de aplicación o un certificado con una aplicación de Azure AD:

- Para obtener más información sobre cómo registrar un secreto de aplicación, consulte [AppCreationScripts/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts/Configure.ps1#L190)
- Para obtener más información sobre cómo registrar un certificado con la aplicación, consulte [AppCreationScripts-withCert/Configure.ps1](https://github.com/Azure-Samples/active-directory-dotnetcore-daemon-v2/blob/5199032b352a912e7cc0fce143f81664ba1a8c26/AppCreationScripts-withCert/Configure.ps1#L162-L178)