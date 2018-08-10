---
title: Habilitación del inicio de sesión único entre aplicaciones en Android mediante ADAL | Microsoft Docs
description: 'Cómo utilizar las características del SDK de ADAL para habilitar el inicio de sesión único entre las aplicaciones. '
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 40710225-05ab-40a3-9aec-8b4e96b6b5e7
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: android
ms.devlang: java
ms.topic: article
ms.date: 06/13/2018
ms.author: celested
ms.reviewer: dadobali
ms.custom: aaddev
ms.openlocfilehash: 4abf6bd2d82753e22d4fde92e219109274ce36be
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2018
ms.locfileid: "39601279"
---
# <a name="how-to-enable-cross-app-sso-on-android-using-adal"></a>Habilitación de SSO entre aplicaciones en Android mediante ADAL
El inicio de sesión único (SSO), de modo que los usuarios solo tengan que escribir sus credenciales una sola vez y dichas credenciales automáticamente funcionen en otras aplicaciones, es ahora un estándar del sector. La dificultad que supone escribir el nombre de usuario y la contraseña en una pantalla pequeña, a menudo combinada con un factor adicional (2FA) como una llamada de teléfono o un código de mensaje de texto, provoca insatisfacción en el usuario si este tiene que iniciar sesión más de una vez.

Además, si aplica una plataforma de identidad que puedan usar otras aplicaciones, como cuentas Microsoft o una cuenta profesional de Microsoft365, los clientes esperan que las credenciales estén disponibles para todas sus aplicaciones, independientemente de quién sea el publicador.

La plataforma Microsoft Identity, junto con los SDK de Microsoft Identity, le ofrecen la posibilidad de satisfacer a sus clientes con el inicio de sesión único, ya sea dentro de su propio conjunto de aplicaciones o, como sucede con la funcionalidad de agente y las aplicaciones de Authenticator, en todo el dispositivo.

Este tutorial le indicará cómo configurar el SDK dentro de la aplicación para proporcionar SSO a sus clientes.

En el documento anterior se supone que sabe integrar su aplicación con [Android SDK de Microsoft Identity](https://github.com/AzureAD/azure-activedirectory-library-for-android).

## <a name="sso-concepts-in-the-microsoft-identity-platform"></a>Conceptos de inicio de sesión único en la plataforma de Microsoft Identity
### <a name="microsoft-identity-brokers"></a>Agentes de Microsoft Identity
Microsoft proporciona aplicaciones para todas las plataformas móviles que permiten el traspaso de credenciales entre aplicaciones de distintos proveedores, y habilita características mejoradas especiales que requieren un único lugar seguro desde el que validar las credenciales. El SDK las llama **agentes**. En iOS y Android, los agentes se proporcionan a través de aplicaciones descargables que los clientes instalan de forma independiente o bien las inserta en el dispositivo una empresa que administra algunos o todos los dispositivos de sus empleados. Gracias a estos agentes, es posible administrar la seguridad de solo unas aplicaciones o bien del dispositivo en su totalidad, en función de lo que decidan los administradores de TI. En Windows, esta funcionalidad se proporciona mediante un selector de cuentas integrado en el sistema operativo, que se conoce técnicamente como agente de autenticación web.

#### <a name="broker-assisted-logins"></a>Inicios de sesión asistidos por agente
Los inicios de sesión asistidos por agente son experiencias de inicio de sesión que se producen dentro de la aplicación del agente y usan el almacenamiento y la seguridad del agente para compartir las credenciales entre todas las aplicaciones del dispositivo que se aplican a la plataforma Microsoft Identity. La consecuencia es que las aplicaciones usarán el agente para que los usuarios inicien sesión. En iOS y Android, estos agentes se proporcionan a través de aplicaciones descargables que los clientes instalan de forma independiente, o bien las inserta en el dispositivo una empresa que administra el dispositivo de su usuario. Un ejemplo de este tipo de aplicación es la aplicación Microsoft Authenticator en iOS. En Windows, esta funcionalidad se proporciona mediante un selector de cuentas integrado en el sistema operativo, que se conoce técnicamente como agente de autenticación web.
La experiencia varía según la plataforma y a veces puede ser problemática para los usuarios si no se administra correctamente. Quizá este modelo le resulte más familiar si tiene instalada la aplicación Facebook y usa Facebook Connect desde otra aplicación. La plataforma Microsoft Identity utiliza este mismo modelo.

En el caso de Android, el selector de cuentas se muestra encima de la aplicación, lo que interrumpe menos al usuario.

#### <a name="how-the-broker-gets-invoked"></a>¿Cómo se invoca el agente?
Si se instala un agente compatible en el dispositivo, como la aplicación Microsoft Authenticator, los SDK de Microsoft Identity se ocuparán automáticamente de la tarea de invocar al agente cuando un usuario indique que quiere iniciar sesión con alguna de las cuentas de la plataforma Microsoft Identity. 
 
 #### <a name="how-microsoft-ensures-the-application-is-valid"></a>Cómo garantiza Microsoft que la aplicación es válida
 
 La necesidad de asegurar la identidad de una llamada de la aplicación al agente es fundamental para la seguridad que se proporciona en los inicios de sesión asistidos por agente. Ni iOS ni Android exigen identificadores únicos que sean válidos solo para una aplicación determinada, por lo que aplicaciones malintencionadas pueden "suplantar" un identificador de una aplicación legítima y recibir los tokens destinados a ella. Para asegurarse de que Microsoft siempre se comunica con la aplicación correcta en el runtime, se solicita al desarrollador que proporcione un redirectURI personalizado al registrar su aplicación en el propio Microsoft. **A continuación se describe cómo los desarrolladores deben diseñar este URI de redirección.** Este URI de redirección personalizado contiene la huella digital del certificado de la aplicación y se asegura de que es único para la aplicación por Google Play Store. Cuando una aplicación llama el agente, este solicita al sistema de operativo Android que le proporcione la huella digital del certificado que llama el agente. El agente proporciona esta huella digital del certificado a Microsoft en la llamada al sistema de identidad. Si la huella digital del certificado de la aplicación no coincide con la huella digital del certificado proporcionada por el desarrollador durante el registro, se deniega el acceso a los tokens al recurso que solicita la aplicación. Esta comprobación asegura que solo la aplicación registrada por el desarrollador recibe los tokens.

Los inicios de sesión únicos con agente tienen las siguientes ventajas:

* El usuario experimenta el inicio de sesión único en todas sus aplicaciones, con independencia del proveedor.
* La aplicación puede usar características empresariales más avanzadas, como el acceso condicional, y admitir usuarios de Intune.
* La aplicación puede admitir la autenticación basada en certificados para los usuarios empresariales.
* Un inicio de sesión más seguro, ya que tanto la identidad de la aplicación como la del usuario se verifican por parte de la aplicación de agente con algoritmos de seguridad y sistemas de cifrado adicionales.

A continuación se representa el funcionamiento de los SDK de Microsoft Identity con las aplicaciones de agente para habilitar el inicio de sesión único:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
|  ADAL SDK  | |  ADAL SDK  |   |  ADAL SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```

Una vez que disponga de toda esta información, probablemente pueda entender e implementar mejor el inicio de sesión único en la aplicación mediante los SDK y la plataforma Microsoft Identity.

### <a name="turning-on-sso-for-broker-assisted-sso"></a>Activación del SSO para el SSO asistido por agente
La capacidad de una aplicación de usar cualquier agente que esté instalado en el dispositivo está **desactivada de forma predeterminada**. Para utilizar la aplicación con el agente debe realizar algunos pasos de configuración adicionales y agregar código a la aplicación.

Los pasos que debe seguir son los siguientes:

1. Habilitar el modo de agente en la llamada al código de la aplicación al SDK de Microsoft
2. Establecer un nuevo URI de redirección e indicarlo tanto en la aplicación como en el registro de la aplicación
3. Configurar los permisos correctos en el manifiesto de Android

#### <a name="step-1-enable-broker-mode-in-your-application"></a>Paso 1: Habilitar el modo de agente en la aplicación
La capacidad de la aplicación de utilizar el agente se activa al crear la configuración inicial de la instancia de autenticación. Para hacerlo en la aplicación:

```
AuthenticationSettings.Instance.setUseBroker(true);
```

#### <a name="step-2-establish-a-new-redirect-uri-with-your-url-scheme"></a>Paso 2: Establecer un nuevo URI de redirección con el esquema de dirección URL
Para garantizar que la aplicación correcta recibe los tokens de credencial devueltos, es preciso asegurarse de que se llama a la aplicación de forma que el sistema operativo Android pueda verificarla. El sistema operativo Android utiliza el hash del certificado en Google Play Store. Este hash del certificado no puede suplantarlo ninguna aplicación no autorizada. Junto con el identificador URI de la aplicación de agente, Microsoft garantiza que los tokens se devuelven a la aplicación correcta. Es preciso registrar un identificador URI de redirección único en la aplicación.

Para ser correcto, el URI de redirección debe presentar el formato siguiente:

`msauth://packagename/Base64UrlencodedSignature`

Por ejemplo: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Este identificador URI se puede registrar en la aplicación mediante [Azure Portal](https://portal.azure.com/). Para más información sobre el registro de aplicaciones de Azure AD, consulte [Integración con Azure Active Directory](active-directory-how-to-integrate.md).

#### <a name="step-3-set-up-the-correct-permissions-in-your-application"></a>Paso 3: Configurar los permisos correctos en la aplicación
La aplicación de agente en Android utiliza la característica de administrador de cuentas del sistema operativo Android para administrar las credenciales entre aplicaciones. Para utilizar el agente en Android el manifiesto de la aplicación debe tener permisos para usar cuentas del administrador de cuentas. Estos permisos se explican con todo detalle en la [documentación de Google para el administrador de cuentas que puede encontrar aquí](http://developer.android.com/reference/android/accounts/AccountManager.html)

En concreto, estos permisos son:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### <a name="youve-configured-sso"></a>Ya ha configurado el SSO.
Ahora, el SDK de Microsoft Identity compartirá automáticamente las credenciales entre las aplicaciones e invocará al agente si está presente en su dispositivo.

