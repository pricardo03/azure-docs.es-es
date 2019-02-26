---
title: ¿Qué es Azure Active Directory B2C? | Microsoft Docs
description: Aprenda a crear y a administrar experiencias de identidad como el registro, inicio de sesión y la administración de perfiles en la aplicación mediante Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 02/20/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 9e01ba8ae53dbcca686a9844600a5df416a685ae
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56455507"
---
# <a name="what-is-azure-active-directory-b2c"></a>¿Qué es Azure Active Directory B2C?

Azure Active Directory (Azure AD) B2C es un servicio de administración de identidad. Este servicio le permite personalizar y controlar el modo en que los usuarios interactúan de forma segura con aplicaciones web, de escritorio, móviles o de página única. Con Azure AD B2C, los usuarios pueden registrarse, iniciar sesión, restablecer contraseñas y editar perfiles. Azure AD B2C implementa un formulario de los protocolos OpenID Connect y OAuth 2.0. La clave importante en la implementación de estos protocolos son los tokens de seguridad y sus notificaciones que le permiten proporcionar acceso seguro a recursos.

Un *recorrido del usuario* es una solicitud que especifica una directiva, la cual controla el comportamiento de cómo interactúan el usuario y la aplicación con Azure AD B2C. Existen dos rutas de acceso para definir los recorridos del usuario en Azure AD B2C. 

Si es un desarrollador de aplicaciones con o sin experiencia en identidad, puede definir flujos de usuario de identidad comunes mediante Azure Portal. Si es un profesional de la identidad, un integrador de sistemas, un asesor o pertenece a un equipo de identidad local, o está familiarizado con los flujos de OpenID Connect y conoce los proveedores de identidades y la autenticación basada en notificaciones, podría elegir directivas personalizadas basadas en XML.

Antes de empezar a definir un recorrido del usuario, debe crear un inquilino de Azure AD B2C y registrar la aplicación y la API en él. Una vez que haya completado estas tareas, puede empezar a definir un recorrido del usuario con flujos de usuario o con directivas personalizadas. También puede, opcionalmente, agregar o cambiar proveedores de identidades o personalizar la forma en que el usuario experimenta el recorrido.

## <a name="protocols-and-tokens"></a>Protocolos y tokens

Azure AD B2C admite los protocolos [OpenID Connect y OAuth 2.0](active-directory-b2c-reference-protocols.md) para los recorridos del usuario. En la implementación de OpenID Connect de Azure AD B2C, su aplicación inicia este recorrido del usuario mediante la emisión de solicitudes de autenticación a Azure AD B2C. 

El resultado de una solicitud a Azure AD B2C es un token de seguridad como, por ejemplo, un [token de identificador o un token de acceso](active-directory-b2c-reference-tokens.md). Este token de seguridad define la identidad del usuario. Los tokens se reciben de los puntos de conexión de Azure AD B2C como un punto de conexión `/token` o `/authorize`. A partir de estos tokens, podrá acceder a notificaciones que se pueden usar para validar una identidad y permitir el acceso a recursos seguros.

## <a name="tenants-and-applications"></a>Inquilinos y aplicaciones

En Azure AD B2C, un *inquilino* representa a su organización y es un directorio de usuarios. Cada inquilino de Azure AD B2C es distinto e independiente del resto de inquilinos de Azure AD B2C. Es posible que tenga ya un inquilino de Azure Active Directory, pero el inquilino de Azure AD B2C es un inquilino diferente. Un inquilino contiene información acerca de los usuarios que se han registrado para utilizar la aplicación. Por ejemplo, contraseñas, datos de perfil y permisos. Para más información, consulte [Tutorial: Creación de un inquilino de Azure Active Directory B2C](tutorial-create-tenant.md).

Antes de configurar su aplicación para usar Azure AD B2C, primero debe registrarla en el inquilino mediante Azure Portal. El proceso de registro recopila y asigna valores a la aplicación. Estos valores incluyen un identificador de aplicación que identifica de forma exclusiva a esta y una URI de redirección que se utiliza para dirigir las respuestas hacia la aplicación.

La interacción de cada aplicación sigue un patrón de nivel alto similar:

1. La aplicación dirige al usuario para que ejecute una directiva.
2. El usuario completa la directiva según la definición de la misma.
3. La aplicación recibe un token.
4. La aplicación utiliza el token para intentar acceder a un recurso.
5. El servidor de recursos valida el token para asegurarse de que se puede conceder acceso.
6. La aplicación actualiza periódicamente el token.

Para registrar una aplicación web, realice los pasos del [Tutorial: Registro de aplicaciones para habilitar el registro y el inicio de sesión con Azure AD B2C](tutorial-register-applications.md). También puede [agregar una aplicación de API web a su inquilino de Azure Active Directory B2C](add-web-application.md) o [agregar una aplicación cliente nativa a este](add-native-application.md).

## <a name="user-journeys"></a>Recorridos de usuario

La directiva en un recorrido del usuario se puede definir como un [flujo de usuario](active-directory-b2c-reference-policies.md) o como una [directiva personalizada](active-directory-b2c-overview-custom.md). Los flujos de usuario predefinidos para las tareas de identidad más comunes, como el registro, el inicio de sesión y la edición de perfiles, están disponibles en Azure Portal.

Los recorridos del usuario le permiten controlar comportamientos configurando los siguientes valores:

- Cuentas de redes sociales que el usuario emplea para registrarse en la aplicación
- Datos recopilados del usuario como el nombre o el código postal
- Multi-Factor Authentication
- Apariencia de las páginas
- Información que se devuelve a la aplicación

Las directivas personalizadas son archivos de configuración que definen el comportamiento de [Identity Experience Framework](trustframeworkpolicy.md) en el inquilino de Azure AD B2C. Identity Experience Framework es la plataforma subyacente que establece la confianza de varias partes y completa los pasos de un recorrido del usuario. 

Las directivas personalizadas se pueden modificar para realizar muchas tareas. Una directiva personalizada consta de uno o varios archivos con formato XML que se hacen referencia entre sí en una cadena jerárquica. Hay un [paquete de inicio](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) disponible para directivas personalizadas para que habiliten tareas comunes de identidad. 

Se usan directivas personalizadas o flujos de usuarios de diferentes tipos en el inquilino de Azure AD B2C según sea necesario y se pueden reutilizar en las aplicaciones. Esta flexibilidad le permite definir y modificar las experiencias de identidad del usuario con cambios mínimos o ningún cambio en el código. Se pueden utilizar las directivas si se agrega un parámetro de consulta especial en solicitudes de autenticación de HTTP. Para crear su propia directiva personalizada, consulte [Introducción a las directivas personalizadas en Azure Active Directory B2C](active-directory-b2c-get-started-custom.md).

## <a name="identity-providers"></a>Proveedores de identidades 

En las aplicaciones, es posible que quiera permitir que los usuarios inicien sesión con distintos proveedores de identidades. Un *proveedor de identidades* crea, mantiene y administra la información de identidad al tiempo que proporciona servicios de autenticación a las aplicaciones. Puede agregar proveedores de identidades compatibles con Azure AD B2C mediante Azure Portal. 

Por lo general, solo se usa un proveedor de identidades en una aplicación, pero existe la opción de agregar más. Para configurar un proveedor de identidades en su inquilino de Azure AD B2C, primero debe crear una aplicación en el sitio para desarrolladores del proveedor de identidades, después debe registrar el identificador de aplicación o identificador de cliente, así como la contraseña o secreto de cliente desde la aplicación del proveedor de identidades que cree. Este identificador y la contraseña se utilizan entonces para configurar la aplicación. 

Los artículos siguientes describen los pasos para agregar algunos de los proveedores de identidades más habituales a los flujos de usuarios:

- [Amazon](active-directory-b2c-setup-amzn-app.md)
- [Facebook](active-directory-b2c-setup-fb-app.md)
- [Cuenta Microsoft](active-directory-b2c-setup-msa-app.md)

Los artículos siguientes describen los pasos para agregar algunos de los proveedores de identidades más habituales a directivas personalizadas:
- [Amazon](setup-amazon-custom.md)
- [Google](active-directory-b2c-custom-setup-goog-idp.md)
- [Cuenta Microsoft](active-directory-b2c-custom-setup-msa-idp.md)

Para más información, consulte [Tutorial: Incorporación de proveedores de identidades a las aplicaciones en Azure Active Directory B2C](tutorial-add-identity-providers.md).


## <a name="page-customization"></a>Personalización de una página

Se puede controlar la mayor parte del contenido HTML y CSS que se presenta a los clientes en un recorrido del usuario. La característica de personalización de la página sirve para personalizar la apariencia y la experiencia de cualquier directiva personalizada o flujo de usuario. También puede mantener la coherencia visual y de la marca entre la aplicación y Azure AD B2C mediante esta característica de personalización. 

Azure AD B2C ejecuta código en el explorador del usuario y usa un enfoque moderno denominado Uso compartido de recursos entre orígenes (CORS). En primer lugar, especifique una dirección URL en una directiva con contenido HTML personalizado. Azure AD B2C combina elementos de la interfaz de usuario con el contenido HTML cargado desde la URL y, después, muestra la página al usuario.

Puede enviar parámetros a Azure AD B2C en una cadena de consulta. Al pasar este parámetro al punto de conexión HTML, el contenido de la página se cambia de forma dinámica. Por ejemplo, puede cambiar la imagen de fondo en la página de inicio de sesión o de registro en función de un parámetro que se pasa desde la aplicación web o dispositivo móvil.

Para personalizar las páginas de un flujo de usuario, consulte [Tutorial: Personalización de la interfaz de experiencias de usuario en Azure Active Directory B2C](tutorial-customize-ui.md). Para personalizar las páginas de una directiva personalizada, consulte [Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada en Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md) o [Configuración de la interfaz de usuario con contenido dinámico usando directivas personalizadas](active-directory-b2c-ui-customization-custom-dynamic.md).

## <a name="developer-resources"></a>Recursos para el desarrollador

### <a name="client-applications"></a>Aplicaciones cliente

Esto incluye las aplicaciones desarrolladas para [iOS](active-directory-b2c-devquickstarts-ios.md), [Android](active-directory-b2c-devquickstarts-android.md) y .NET, entre otros. Azure AD B2C permite estas acciones y, al mismo tiempo, protege las identidades de los usuarios.

Si es un desarrollador de aplicaciones web ASP.NET, configure la aplicación para autenticar las cuentas mediante los pasos descritos en el [Tutorial: Habilitación de una aplicación web para autenticarse con cuentas mediante Azure AD B2C](active-directory-b2c-tutorials-web-app.md).

Si es un desarrollador de aplicaciones de escritorio, configure la aplicación para autenticar las cuentas mediante los pasos descritos en el [Tutorial: Habilitación de la autenticación de una aplicación de escritorio con cuentas mediante Azure AD B2C](active-directory-b2c-tutorials-desktop-app.md).

Si es un desarrollador de aplicaciones de página única con Node.js, configure la aplicación para autenticar las cuentas mediante los pasos descritos en el [Tutorial: Habilitación de la autenticación de una aplicación de una sola página con cuentas mediante Azure AD B2C](active-directory-b2c-tutorials-spa.md).

### <a name="apis"></a>API existentes
Si las aplicaciones cliente o las aplicaciones web necesitan llamar a las API, puede configurar un acceso seguro a esos recursos en Azure AD B2C.

Si es un desarrollador de aplicaciones web ASP.NET, configure la aplicación para llamar a una API protegida mediante los pasos descritos en el [Tutorial: Concesión de acceso a una API web de ASP.NET mediante Azure Active Directory B2C](active-directory-b2c-tutorials-web-api.md).

Si es un desarrollador de aplicaciones de escritorio, configure la aplicación para llamar a una API protegida mediante los pasos descritos en el [Tutorial: Concesión de acceso a una API web de Node.js desde una aplicación de escritorio mediante Azure Active Directory B2C](active-directory-b2c-tutorials-desktop-app-webapi.md).

Si es desarrollador de aplicaciones de una sola página con Node.js, configure la aplicación para autenticar las cuentas mediante los pasos descritos en el [Tutorial: Concesión de acceso a una API web de ASP.NET Core desde una aplicación de página única mediante Azure Active Directory B2C](active-directory-b2c-tutorials-spa-webapi.md).

### <a name="javascript"></a>JavaScript

Puede agregar su propio código del lado cliente de JavaScript a las aplicaciones de Azure AD B2C. Para configurar JavaScript en la aplicación, puede definir un [contrato de página](page-contract.md) y habilitar [JavaScript](javascript-samples.md) en sus flujos de usuarios o directivas personalizadas.

### <a name="user-accounts"></a>Cuentas de usuario

Muchas tareas comunes de administración de inquilinos necesitan realizarse mediante programación. Un ejemplo importante es la administración de usuarios. Quizá necesite migrar un almacén de usuario existente a un inquilino de Azure AD B2C. Puede que desee hospedar un registro de usuarios en su propia página y crear cuentas de usuario en su directorio de Azure AD B2C en segundo plano. Estos tipos de tareas requieren la capacidad de crear, leer, actualizar y eliminar cuentas de usuario. [Puede realizar estas tareas mediante Graph API de Azure AD](active-directory-b2c-devquickstarts-graph-dotnet.md).

## <a name="next-steps"></a>Pasos siguientes

Comience a configurar su aplicación para la experiencia de registro e inicio de sesión al continuar el tutorial.

> [!div class="nextstepaction"]
> [Tutorial: Creación de un inquilino de Azure Active Directory B2C](tutorial-create-tenant.md)
