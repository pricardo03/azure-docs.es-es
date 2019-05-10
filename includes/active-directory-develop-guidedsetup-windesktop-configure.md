---
title: archivo de inclusión
description: archivo de inclusión
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
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: c4dc3d422e18d9ee41bf16ac3e6f22c3d7e466d7
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65199477"
---
## <a name="register-your-application"></a>Registrar su aplicación

Puede registrar la aplicación de dos maneras.

### <a name="option-1-express-mode"></a>Opción 1: Modo rápido

Puede registrar rápidamente la aplicación mediante estos pasos:
1. Vaya a [Azure Portal: registro de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_RegisteredApps/applicationsListBlade/quickStartType/WinDesktopQuickstartPage/sourceType/docs).
1. Escriba un nombre para la aplicación y seleccione **Registrar**.
1. Siga las instrucciones para descargar y configurar automáticamente la nueva aplicación con un solo clic.

### <a name="option-2-advanced-mode"></a>Opción 2: Modo avanzado

Para registrar la aplicación y agregar la información de registro de aplicación a la solución, siga estos pasos:
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
1. Vaya a la página [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de la plataforma de identidad de Microsoft para desarrolladores.
1. Seleccione **Nuevo registro**.
   - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `Win-App-calling-MsGraph`.
   - En **Tipos de cuenta admitidos**, seleccione **Cuentas en cualquier directorio de organización y cuentas personales de Microsoft (por ejemplo, Skype, Xbox o Outlook.com)**.
   - Seleccione **Registrar** para crear la aplicación.
1. En la lista de páginas de la aplicación, seleccione **Autenticación**.
   1. En la sección **URI de redirección**, en la lista de identificadores URI de redirección:
   1. En la columna **TIPO**, seleccione **Cliente público (móvil y escritorio)**.
   1. Escriba `urn:ietf:wg:oauth:2.0:oob` en la columna **URI DE REDIRECCIÓN**.
1. Seleccione **Guardar**.
1. Vaya a Visual Studio, abra el archivo *App.xaml.cs* y, luego, reemplace `Enter_the_Application_Id_here` por el identificador de aplicación que acaba de registrar y copiar.

    ```csharp
    private static string ClientId = "Enter_the_Application_Id_here";
    ```
