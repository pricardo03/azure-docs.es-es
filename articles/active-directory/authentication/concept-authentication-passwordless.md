---
title: Inicio de sesión sin contraseña de Azure Active Directory (versión preliminar)
description: Inicio de sesión sin contraseña en Azure AD con claves de seguridad FIDO2 o la aplicación Microsoft Authenticator (versión preliminar)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 10/08/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: e073b4ff366c05cdf429f81d46647cd330604057
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74081574"
---
# <a name="passwordless-authentication-options"></a>Opciones de autenticación sin contraseña

La autenticación multifactor (MFA) es una excelente manera de proteger la organización, aunque los usuarios se frustran con el paso que se suma a tener que recordar sus contraseñas. Los métodos de autenticación sin contraseña resultan más cómodos, ya que la contraseña se quita y se reemplaza por algo que se tiene más algo que se es o se sabe.

|   | Algo que se tiene | Algo que se es o se sabe |
| --- | --- | --- |
| Inicio de sesión sin contraseña | Dispositivo, teléfono o clave de seguridad con Windows 10 | PIN o biométrica |

Cada organización tiene diferentes necesidades en cuanto a la autenticación. Microsoft ofrece tres opciones de autenticación sin contraseña:

- Windows Hello para empresas
- Aplicación Microsoft Authenticator
- Claves de seguridad FIDO2

![Autenticación: Seguridad frente a comodidad](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

## <a name="windows-hello-for-business"></a>Windows Hello para empresas

Windows Hello para empresas resulta muy conveniente para los trabajadores de la información que tienen su propio PC con Windows designado. La información biométrica y el PIN están directamente asociados al equipo del usuario, lo que impide el acceso a cualquier otra persona que no sea el propietario. Con la integración de PKI y la compatibilidad integrada con el inicio de sesión único (SSO), Windows Hello para empresas ofrece un método sencillo y práctico de acceder completamente a los recursos corporativos del entorno local y la nube.

La [guía de planeamiento](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) de Windows Hello para empresas se puede usar para ayudarle a tomar decisiones sobre el tipo de implementación y las opciones que es necesario tener en cuenta.

## <a name="microsoft-authenticator-app"></a>Aplicación Microsoft Authenticator

Permita que el teléfono del empleado se convierta en un método de autenticación sin contraseña. Es posible que ya esté usando la aplicación Microsoft Authenticator como una opción de autenticación multifactor cómoda sumada a una contraseña. Pero ahora está disponible como una opción sin contraseña.

![Inicio de sesión en Microsoft Edge con la aplicación Microsoft Authenticator](./media/concept-authentication-passwordless/concept-web-sign-in-microsoft-authenticator-app.png)

Convierte cualquier teléfono iOS o Android en una credencial sin contraseña segura al permitir a los usuarios iniciar sesión en cualquier plataforma o explorador mediante la recepción de una notificación en su teléfono, la coincidencia de un número que aparece en la pantalla con el de su teléfono y luego el uso de su biométrica (toque o cara) o PIN para confirmar.

## <a name="fido2-security-keys"></a>Claves de seguridad FIDO2

Las claves de seguridad FIDO2 son un método de autenticación sin contraseña basado en estándares que no permite la suplantación de identidad y que puede venir en cualquier factor de forma. Fast Identity Online (FIDO) es un estándar abierto para la autenticación sin contraseña. Permite a los usuarios y a las organizaciones aprovechar el estándar para iniciar sesión en sus recursos sin un nombre de usuario o una contraseña mediante una clave de seguridad externa o una clave de plataforma integrada en un dispositivo.

En la versión preliminar pública, los empleados pueden usar claves de seguridad para iniciar sesión en sus dispositivos Windows 10 unidos a Azure AD y lograr el inicio de sesión único en sus recursos de nube y locales. También pueden iniciar sesión en exploradores compatibles.

![Inicio de sesión en Microsoft Edge con una clave de seguridad](./media/concept-authentication-passwordless/concept-web-sign-in-security-key.png)

Aunque hay muchas claves certificadas como FIDO2 por FIDO Alliance, Microsoft necesita que el proveedor implemente algunas extensiones opcionales de la especificación Client-to-Authenticator Protocol (CTAP) FIDO2 para garantizar la máxima seguridad y la mejor experiencia.

Una clave de seguridad **DEBE** implementar las siguientes extensiones y características del protocolo FIDO2 CTAP para ser compatible con Microsoft:

| # | Confianza de característica o extensión | ¿Por qué se requiere esta característica o extensión? |
| --- | --- | --- |
| 1 | Clave residente | Esta característica permite que la clave de seguridad sea portátil, ya que la credencial se almacena en la clave de seguridad. |
| 2 | PIN de cliente | Esta característica permite proteger las credenciales con un segundo factor y se aplica a las claves de seguridad que no tienen una interfaz de usuario. |
| 3 | hmac-secret | Esta extensión garantiza que pueda iniciar sesión en el dispositivo cuando está sin conexión o en modo avión. |
| 4 | Varias cuentas por RP | Esta característica garantiza que pueda usar la misma clave de seguridad en varios servicios, como Microsoft Account y Azure Active Directory. |

Los siguientes proveedores ofrecen claves de seguridad FIDO2 o diferentes factores de forma que permiten iniciar sesión sin contraseña. Microsoft anima a los clientes a evaluar las propiedades de seguridad de estas claves al ponerse en contacto con el proveedor y con FIDO Alliance.

| Proveedor | Contacto |
| --- | --- |
| Yubico | [https://www.yubico.com/support/contact/](https://www.yubico.com/support/contact/) |
| Feitian | [https://www.ftsafe.com/about/Contact_Us](https://www.ftsafe.com/about/Contact_Us) |
| HID | [https://www.hidglobal.com/contact-us](https://www.hidglobal.com/contact-us) |
| Ensurity | [https://www.ensurity.com/contact](https://www.ensurity.com/contact) |
| eWBM | [https://www.ewbm.com/support](https://www.ewbm.com/support) |
| AuthenTrend | [https://authentrend.com/about-us/#pg-35-3](https://authentrend.com/about-us/#pg-35-3) |

> [!NOTE]
> Si adquiere y planea usar claves de seguridad basadas en NFC, necesita un lector NFC compatible.

Si es proveedor y quiere que su dispositivo aparezca en esta lista, póngase en contacto con [Fido2Request@Microsoft.com](mailto:Fido2Request@Microsoft.com).

Las claves de seguridad FIDO2 son una excelente opción para las empresas que son muy conscientes de la seguridad o tienen escenarios o empleados que no quieren o no pueden usar su teléfono como un segundo factor.

## <a name="what-scenarios-work-with-the-preview"></a>¿Qué escenarios funcionan con la versión preliminar?

- Los administradores pueden habilitar métodos de autenticación sin contraseña para su inquilino
- Los administradores pueden establecer como destino a todos los usuarios o seleccionar usuarios o grupos dentro de su inquilino para cada método
- Los usuarios finales pueden registrar y administrar estos métodos de autenticación sin contraseña en el portal de la cuenta
- Los usuarios finales pueden iniciar sesión con estos métodos de autenticación sin contraseña
   - Aplicación Microsoft Authenticator: funciona en los escenarios donde se usa la autenticación de Azure AD, lo que incluye todos los exploradores, durante la configuración rápida (OOBE) de Windows 10 y con aplicaciones móviles integradas en cualquier sistema operativo.
   - Claves de seguridad: funcionan en la pantalla de bloqueo de Windows 10 e Internet en exploradores compatibles como Microsoft Edge.

## <a name="next-steps"></a>Pasos siguientes

[Habilitar opciones sin contraseña de clave de seguridad FIDO2 en la organización](howto-authentication-passwordless-security-key.md)

[Habilitar opciones sin contraseña basadas en teléfono en la organización](howto-authentication-passwordless-phone.md)

### <a name="external-links"></a>Vínculos externos

[FIDO Alliance](https://fidoalliance.org/)

[Especificación FIDO2 CTAP](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
