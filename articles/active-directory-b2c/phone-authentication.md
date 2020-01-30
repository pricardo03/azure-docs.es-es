---
title: Registro e inicio de sesión en el teléfono con directivas personalizadas
titleSuffix: Azure AD B2C
description: Aprenda a enviar contraseñas de un solo uso en mensajes de texto a los teléfonos de los usuarios de la aplicación con directivas personalizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/17/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 8cb0340d9e04db2bfbf088bce9505351d7588cd9
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840339"
---
# <a name="set-up-phone-sign-up-and-sign-in-with-custom-policies-in-azure-ad-b2c"></a>Configuración del registro y el inicio de sesión en el teléfono con directivas personalizadas en Azure AD B2C

El registro y el inicio de sesión en el teléfono en Azure Active Directory B2C (Azure AD B2C) permite a los usuarios registrarse e iniciar sesión en las aplicaciones mediante una contraseña de un solo uso (OTP) enviada en un mensaje de texto a su teléfono. Las contraseñas de un solo uso pueden ayudar a reducir el riesgo de que los usuarios olviden sus contraseñas o estas se vean comprometidas.

Siga los pasos de este artículo para usar las directivas personalizadas y permitir que los clientes se registren e inicien sesión en las aplicaciones mediante una contraseña de un solo uso enviada a su teléfono.

[!INCLUDE [b2c-public-preview-feature](../../includes/active-directory-b2c-public-preview.md)]

## <a name="prerequisites"></a>Prerequisites

* [Inquilino de Azure AD B2C](tutorial-create-tenant.md)
* [Aplicación web registrada](tutorial-register-applications.md) en el inquilino
* [Directivas personalizadas](custom-policy-get-started.md) cargadas en el inquilino

## <a name="get-the-phone-sign-up--sign-in-starter-pack"></a>Obtención del paquete de inicio de registro e inicio de sesión en el teléfono

Empiece por actualizar los archivos de directivas personalizadas de registro e inicio de sesión en el teléfono para que funcionen con el inquilino de Azure AD B2C.

En los siguientes pasos se da por supuesto que ha completado los [requisitos previos](#prerequisites) y ya ha clonado el repositorio del [paquete de inicio de directivas personalizadas][starter-pack] en la máquina local.

1. Busque los [archivos de directivas personalizadas de registro e inicio de sesión en el teléfono][starter-pack-phone] en el clon local del repositorio del paquete de inicio o descárguelos directamente. Los archivos XML de directivas están ubicados en el siguiente directorio:

    `active-directory-b2c-custom-policy-starterpack/scenarios/`**`phone-number-passwordless`**

1. En cada archivo, reemplace la cadena `yourtenant` por el nombre del inquilino de Azure AD B2C. Por ejemplo, si el nombre del inquilino de B2C es *contosob2c*, todas las instancias de `yourtenant.onmicrosoft.com` se convierten en `contosob2c.onmicrosoft.com`.

1. Complete los pasos de la sección [Agregar identificadores de aplicación a la directiva personalizada](custom-policy-get-started.md#add-application-ids-to-the-custom-policy) de [Introducción a las directivas personalizadas en Azure Active Directory B2C](custom-policy-get-started.md). En este caso, actualice `/phone-number-passwordless/` **`Phone_Email_Base.xml`** con los valores de **Id. de aplicación (cliente)** de las dos aplicaciones que registró al completar los requisitos previos, *IdentityExperienceFramework* y *ProxyIdentityExperienceFramework*.

## <a name="upload-the-policy-files"></a>Carga de los archivos de directivas

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya al inquilino de Azure AD B2C.
1. En **Directivas**, seleccione **Identity Experience Framework**.
1. Seleccione **Cargar directiva personalizada**.
1. Cargue los archivos de directivas en el siguiente orden:
    1. *Phone_Email_Base.xml*
    1. *SignUpOrSignInWithPhone.xml*
    1. *SignUpOrSignInWithPhoneOrEmail.xml*
    1. *ProfileEditPhoneOnly.xml*
    1. *ProfileEditPhoneEmail.xml*
    1. *ChangePhoneNumber.xml*
    1. *PasswordResetEmail.xml*

A medida que carga cada archivo, Azure agrega el prefijo `B2C_1A_`.

## <a name="test-the-custom-policy"></a>Prueba de la directiva personalizada

1. En **Directivas personalizadas**, seleccione **B2C_1A_SignUpOrSignInWithPhone**.
1. En **Seleccionar aplicación**, seleccione la aplicación *webapp1* que registró al completar los requisitos previos.
1. En **Seleccionar dirección URL de respuesta**, elija `https://jwt.ms`.
1. Seleccione **Ejecutar ahora** y regístrese con una dirección de correo electrónico o un número de teléfono.
1. Seleccione **Ejecutar ahora** de nuevo e inicie sesión con la misma cuenta para confirmar que tiene la configuración correcta.

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar el paquete de inicio de las directivas personalizadas de registro e inicio de sesión en el teléfono (y otros paquetes de inicio) en GitHub:

[Azure-Samples/active-directory-b2c-custom-policy-starterpack/scenarios/phone-number-passwordless][starter-pack-phone]

Los archivos de directivas del paquete de inicio usan perfiles técnicos de autenticación multifactor y transformaciones de notificaciones de números de teléfono:

* [Definición de un perfil técnico de Azure AD en una directiva personalizada de Azure AD B2C](multi-factor-auth-technical-profile.md)
* [Definición de transformaciones de notificaciones de número de teléfono en Azure AD B2C](phone-number-claims-transformations.md)

<!-- LINKS - External -->
[starter-pack]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack
[starter-pack-phone]: https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/phone-number-passwordless
