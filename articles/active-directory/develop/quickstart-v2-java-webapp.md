---
title: Inicio rápido de la aplicación web de Java para la Plataforma de identidad de Microsoft | Azure
description: Obtenga información sobre cómo implementar el inicio de sesión de Microsoft en una aplicación web de Java mediante OpenID Connect.
services: active-directory
documentationcenter: dev-center-name
author: sangonzal
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/11/2019
ms.author: sagonzal
ms.custom: aaddev
ms.openlocfilehash: 549b4457ee38504001e83c4b831cd321e1542068
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70125469"
---
# <a name="quickstart-add-sign-in-with-microsoft-to-a-java-web-app"></a>Inicio rápido: Adición de inicio de sesión con Microsoft a una aplicación web de Java

[!INCLUDE [active-directory-develop-applies-v2](../../../includes/active-directory-develop-applies-v2.md)]

En este inicio rápido, aprenderá a integrar una aplicación web de Java con la Plataforma de identidad de Microsoft. Su aplicación iniciará la sesión de un usuario, obtendrá un token de acceso para llamar a Microsoft Graph API y realizará una solicitud a Microsoft Graph API. 

Cuando haya completado la guía, la aplicación podrá aceptar inicios de sesión de cuentas Microsoft personales (como outlook.com, live.com y otras), y cuentas profesionales y educativas de cualquier empresa u organización que utilice Azure Active Directory.

![Muestra cómo funciona la aplicación de ejemplo generada por este inicio rápido.](media/quickstart-v2-java-webapp/java-quickstart.svg)

> ## <a name="prerequisites"></a>Requisitos previos
> Para ejecutar este ejemplo, necesitará lo siguiente: 
> - Kit de desarrollo de Java (JDK) 8 o una versión posterior y Maven.
> - Un inquilino de Azure Active Directory (Azure AD). Para más información sobre cómo obtener una suscripción de Azure AD, consulte [Obtención de un inquilino de Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-create-new-tenant).
>
> [!div renderon="docs"]
> ## <a name="register-and-download-your-quickstart-app"></a>Registro y descarga de la aplicación de inicio rápido
> Tiene dos opciones para comenzar con la aplicación de inicio rápido:
> * Express: [Opción 1: Registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo.](#option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample)
> * Manual: [Opción 2: Registrar y configurar manualmente la aplicación y el código de ejemplo.](#option-2-register-and-manually-configure-your-application-and-code-sample)
> 
> ### <a name="option-1-register-and-auto-configure-your-app-and-then-download-your-code-sample"></a>Opción 1: registrar y configurar de modo automático la aplicación y, a continuación, descargar el código de ejemplo
>
> 1. Vaya a [Azure Portal: registros de aplicaciones](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps).
> 1. Escriba un nombre para la aplicación y seleccione **Registrar**.
> 1. Siga las instrucciones para descargar y configurar automáticamente la nueva aplicación.
>
> ### <a name="option-2-register-and-manually-configure-your-application-and-code-sample"></a>Opción 2: registrar y configurar manualmente la aplicación y el código de ejemplo
> 
>
> #### <a name="step-1-download-the-code-sample"></a>Paso 1: Descargar el código de ejemplo
> 
> - [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)
>
> #### <a name="step-2-open-applicationproperties"></a>Paso 2: Abrir application.properties
>
> 1. Extraiga el archivo zip en una carpeta local.
> 1. (Opcional) Si usa un entorno de desarrollo integrado, abra el ejemplo en su IDE favorito.
> 1. Abra el archivo *application.properties*. Insertará valores para `aad.clientId`, `aad.authority` y `aad.secretKey` al registrar la aplicación en el paso siguiente.


> #### <a name="step-3-register-your-application"></a>Paso 3: Registrar su aplicación
> Para registrar la aplicación y agregar la información de registro de la aplicación a la solución de forma manual, siga estos pasos:
>
> 1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
> 1. Si la cuenta proporciona acceso a más de un inquilino, haga clic en la cuenta en la esquina superior derecha y establezca la sesión del portal en el inquilino de Azure AD deseado.
> 1. Vaya a la página [Registros de aplicaciones](https://go.microsoft.com/fwlink/?linkid=2083908) de la plataforma de identidad de Microsoft para desarrolladores.
> 1. Seleccione **Nuevo registro**.
> 1. Cuando aparece la página **Registrar una aplicación**, escriba la información de registro de la aplicación:
>    - En la sección **Nombre**, escriba un nombre significativo para la aplicación, que se mostrará a los usuarios de la aplicación, por ejemplo, `java-webapp`.
>    - Deje **URI de redirección** en blanco por ahora y seleccione **Registrar**.
> 1. Encuentre el valor del **identificador de aplicación (cliente)** de la aplicación. Actualice el valor de `Enter_the_Application_Id_here` en el archivo *application.properties*.
> 1. Busque el valor de **Id. de directorio (inquilino)** de la aplicación. Actualice el valor de `Enter_the_Tenant_Info_Here` en el archivo *application.properties*. 
> 1. Seleccione el menú **Autenticación** y, a continuación, agregue la siguiente información:
>    - En **URI de redirección**, agregue `http://localhost:8080/msal4jsamples/secure/aad` y `https://localhost:8080/msal4jsamples/graph/users`.
>    - Seleccione **Guardar**.
> 1. En el menú de la izquierda, elija **Certificates & secrets** (Certificados y secretos) y haga clic en **Nuevo secreto de cliente** en la sección **Client Secrets** (Secretos de cliente):
>     
>    - Escriba una descripción de la clave (del secreto de la aplicación de instancia).
>    - Seleccione una duración de la clave de **En 1 año**.
>    - Al hacer clic en **Agregar**, se mostrará el valor de la clave. 
>    - Copie el valor de la clave. Abra el archivo*application.properties* que descargó anteriormente y actualice el valor de `Enter_the_Client_Secret_Here` con el valor de la clave. 
>
> [!div class="sxs-lookup" renderon="portal"]
> #### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Paso 1: Configuración de la aplicación en Azure Portal
> Para que el código de ejemplo de este inicio rápido funcione, es preciso:
> 1. Agregue las direcciones URL de respuesta como `http://localhost:8080/msal4jsamples/secure/aad` y `https://localhost:8080/msal4jsamples/graph/users`.
> 1. Cree un secreto de cliente.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Hacer este cambio por mí]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Ya configurada](media/quickstart-v2-aspnet-webapp/green-check.png) La aplicación está configurada con estos atributos.
> 
> #### <a name="step-2-download-the-code-sample"></a>Paso 2: Descargar el código de ejemplo
> 
> - [Descargar el código de ejemplo](https://github.com/Azure-Samples/ms-identity-java-webapp/archive/master.zip)
> 
> #### <a name="step-3-configure-the-code-sample"></a>Paso 3: Configurar el ejemplo de código 
> 
> 1. Extraiga el archivo zip en una carpeta local.
> 1. Si usa un entorno de desarrollo integrado, abra el ejemplo en su IDE favorito (opcional).
> 1. Abra el archivo **application.properties**, que puede encontrarse en *src/main/resources/* .
> 1. Reemplace las propiedades de la aplicación.
>   1. Busque `aad.clientId` y actualice el valor de `Enter_the_Application_Id_here` con el valor de **Identificador de aplicación (cliente)** de la aplicación que ha registrado. 
>   1. Busque `aad.authority` y actualice el valor de `Enter_the_Tenant_Name_Here` con el valor de **Id. de directorio (inquilino)** de la aplicación que ha registrado.
>   1. Busque `aad.secretKey` y actualice el valor de `Enter_the_Client_Secret_Here` con el **secreto de cliente** que creó en **Certificates & secrets** (Certificados y secretos) de la aplicación que registró.

#### <a name="step-4-run-the-code-sample"></a>Paso 4: Ejecución del ejemplo de código
1. Ejecute el ejemplo de código y abra un explorador y vaya a *http://localhost:8080* .
1. La página principal contiene un botón de **inicio de sesión**. Haga clic en el botón de **inicio de sesión** para redirigir a Azure Active Directory. Se le pedirá al usuario sus credenciales.  
1. Después de autenticarse correctamente en Azure Active Directory, se le redirigirá a *http://localhost:8080/msal4jsamples/secure/aad* . Han iniciado sesión oficialmente en la aplicación y la página debe mostrar información de la cuenta con la sesión iniciada. También contendrá los siguientes botones: 
    - *Sign Out* (Cerrar sesión): cerrará la sesión del usuario actual de la aplicación y le redirigirá a la página principal.
    - *Show Users* (Mostrar usuarios): adquirirá un token para Microsoft Graph y, a continuación, llamará a Microsoft Graph con el token asociado a la solicitud para obtener todos los usuarios del inquilino.


## <a name="more-information"></a>Más información

### <a name="getting-msal"></a>Obtención de MSAL
MSAL4J es la biblioteca que se usa para iniciar la sesión de los usuarios y solicitar los tokens que se usan para acceder a una API protegida por la Plataforma de identidad de Microsoft. Puede agregar MSAL4J a la aplicación mediante Maven o Gradle para administrar las dependencias realizando los siguientes cambios en el archivo pom.xml o build.gradle de la aplicación. 

```
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>msal4j</artifactId>
    <version>0.5.0-preview</version>
</dependency>
```

```$xslt
compile group: 'com.microsoft.azure', name: 'msal4j', version: '0.5.0-preview'
```


### <a name="msal-initialization"></a>Inicialización de MSAL
Para agregar la referencia a MSAL4J, agregue el código siguiente en la parte superior del archivo en el que va a usar MSAL4J: 

```
import com.microsoft.aad.msal4j.*;
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre los permisos y el consentimiento:

> [!div class="nextstepaction"]
> [Permisos y consentimiento](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-permissions-and-consent)

Para más información sobre el flujo de autenticación en este escenario, consulte el flujo del código de autorización de OAuth 2.0:

> [!div class="nextstepaction"]
> [Flujo del código de autorización de OAuth](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-auth-code-flow)

Ayúdenos a mejorar la Plataforma de identidad de Microsoft. Rellene una breve encuesta de dos preguntas y háganos saber su opinión.

> [!div class="nextstepaction"]
> [Encuesta sobre la Plataforma de identidad de Microsoft](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyKrNDMV_xBIiPGgSvnbQZdUQjFIUUFGUE1SMEVFTkdaVU5YT0EyOEtJVi4u)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
