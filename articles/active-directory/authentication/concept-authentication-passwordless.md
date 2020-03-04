---
title: Inicio de sesión sin contraseña de Azure Active Directory (versión preliminar)
description: Conozca las opciones para el inicio de sesión sin contraseña en Azure Active Directory con las claves de seguridad FIDO2 o la aplicación Microsoft Authenticator.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: ba579d6da8c759a4653b729f1a471efdedc2baa7
ms.sourcegitcommit: 934776a860e4944f1a0e5e24763bfe3855bc6b60
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2020
ms.locfileid: "77505755"
---
# <a name="passwordless-authentication-options-for-azure-active-directory"></a>Opciones de autenticación sin contraseña de Azure Active Directory

La autenticación multifactor (MFA) es una excelente manera de proteger la organización, aunque los usuarios se frustran a menudo con el paso de seguridad que se suma a tener que recordar sus contraseñas. Los métodos de autenticación sin contraseña resultan más cómodos, ya que la contraseña se quita y se reemplaza por algo que se tiene más algo que se es o se sabe.

|   | Algo que se tiene | Algo que se es o se sabe |
| --- | --- | --- |
| Inicio de sesión sin contraseña | Dispositivo, teléfono o clave de seguridad con Windows 10 | PIN o biométrica |

Cada organización tiene diferentes necesidades en cuanto a la autenticación. Microsoft ofrece las siguientes tres opciones de autenticación sin contraseña:

- Windows Hello para empresas
- Aplicación Microsoft Authenticator
- Claves de seguridad FIDO2

![Autenticación: Seguridad frente a comodidad](./media/concept-authentication-passwordless/passwordless-convenience-security.png)

## <a name="windows-hello-for-business"></a>Windows Hello para empresas

Windows Hello para empresas resulta muy conveniente para los trabajadores de la información que tienen su propio PC con Windows designado. La información biométrica y el PIN están directamente asociados al equipo del usuario, lo que impide el acceso a cualquier otra persona que no sea el propietario. Con la integración de la infraestructura de clave pública (PKI) y la compatibilidad integrada con el inicio de sesión único (SSO), Windows Hello para empresas ofrece un método sencillo y práctico de acceder completamente a los recursos corporativos del entorno local y la nube.

La [guía de planeamiento](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) de Windows Hello para empresas se puede usar para ayudarle a tomar decisiones sobre el tipo de implementación y las opciones que es necesario tener en cuenta.

## <a name="microsoft-authenticator-app"></a>Aplicación Microsoft Authenticator

Permita que el teléfono del empleado se convierta en un método de autenticación sin contraseña. Es posible que ya esté usando la aplicación Microsoft Authenticator como una opción de autenticación multifactor cómoda sumada a una contraseña. También puede usar la aplicación Authenticator como una opción sin contraseña.

![Inicio de sesión en Microsoft Edge con la aplicación Microsoft Authenticator](./media/concept-authentication-passwordless/concept-web-sign-in-microsoft-authenticator-app.png)

La aplicación Authenticator convierte cualquier teléfono Android o iOS en una credencial segura sin contraseña. Los usuarios pueden iniciar sesión en cualquier plataforma o explorador con este proceso: reciben una notificación en su teléfono, comprueban que el número mostrado en la pantalla coincide con el de su teléfono y, a continuación, usan datos biométricos (reconocimiento táctil o facial) o el PIN para confirmarlo.

## <a name="fido2-security-keys"></a>Claves de seguridad FIDO2

Las claves de seguridad FIDO2 son un método de autenticación sin contraseña basado en estándares que no permite la suplantación de identidad y que puede venir en cualquier factor de forma. Fast Identity Online (FIDO) es un estándar abierto para la autenticación sin contraseña. FIDO permite a los usuarios y a las organizaciones aprovechar el estándar para iniciar sesión en sus recursos sin un nombre de usuario o una contraseña mediante una clave de seguridad externa o una clave de plataforma integrada en un dispositivo.

En la versión preliminar pública, los empleados pueden usar claves de seguridad para iniciar sesión en sus dispositivos Windows 10 unidos a Azure AD o Azure AD híbrido y lograr el inicio de sesión único en sus recursos de nube y locales. Los usuarios también pueden iniciar sesión en exploradores compatibles. Las claves de seguridad FIDO2 son una excelente opción para las empresas que son muy conscientes de la seguridad o tienen escenarios o empleados que no quieren o no pueden usar su teléfono como un segundo factor.

![Inicio de sesión en Microsoft Edge con una clave de seguridad](./media/concept-authentication-passwordless/concept-web-sign-in-security-key.png)

Aunque hay muchas claves certificadas como FIDO2 por FIDO Alliance, Microsoft necesita que el proveedor implemente algunas extensiones opcionales de la especificación Client-to-Authenticator Protocol (CTAP) FIDO2 para garantizar la máxima seguridad y la mejor experiencia.

Una clave de seguridad **DEBE** implementar las siguientes extensiones y características del protocolo FIDO2 CTAP para ser compatible con Microsoft:

| # | Confianza de característica o extensión | ¿Por qué se requiere esta característica o extensión? |
| --- | --- | --- |
| 1 | Clave residente | Esta característica permite que la clave de seguridad sea portátil, ya que la credencial se almacena en la clave de seguridad. |
| 2 | PIN de cliente | Esta característica permite proteger las credenciales con un segundo factor y se aplica a las claves de seguridad que no tienen una interfaz de usuario. |
| 3 | hmac-secret | Esta extensión garantiza que pueda iniciar sesión en el dispositivo cuando está sin conexión o en modo avión. |
| 4 | Varias cuentas por RP | Esta característica garantiza que pueda usar la misma clave de seguridad en varios servicios, como Microsoft Account y Azure Active Directory. |

Los siguientes proveedores ofrecen claves de seguridad FIDO2 o diferentes factores de forma que permiten iniciar sesión sin contraseña. Animamos a los clientes a evaluar las propiedades de seguridad de estas claves poniéndose en contacto con el proveedor y con FIDO Alliance.

| Proveedor | Contacto |
| --- | --- |
| Yubico | [https://www.yubico.com/support/contact/](https://www.yubico.com/support/contact/) |
| Feitian | [https://www.ftsafe.com/about/Contact_Us](https://www.ftsafe.com/about/Contact_Us) |
| HID | [https://www.hidglobal.com/contact-us](https://www.hidglobal.com/contact-us) |
| Ensurity | [https://www.ensurity.com/contact](https://www.ensurity.com/contact) |
| eWBM | [https://www.ewbm.com/support](https://www.ewbm.com/support) |
| AuthenTrend | [https://authentrend.com/about-us/#pg-35-3](https://authentrend.com/about-us/#pg-35-3) |

> [!NOTE]
> Si adquiere y planea usar claves de seguridad basadas en NFC, necesita un lector NFC compatible para la clave de seguridad. El lector NFC no es un requisito o limitación de Azure. Consulte al proveedor de la clave de seguridad basada en NFC para obtener una lista de lectores de NFC admitidos.

Si es proveedor y quiere que su dispositivo aparezca en esta lista de dispositivos compatibles, póngase en contacto con [Fido2Request@Microsoft.com](mailto:Fido2Request@Microsoft.com).

## <a name="what-scenarios-work-with-the-preview"></a>¿Qué escenarios funcionan con la versión preliminar?

- Los administradores pueden habilitar métodos de autenticación sin contraseña para su inquilino
- Los administradores pueden establecer como destino a todos los usuarios o seleccionar usuarios o grupos dentro de su inquilino para cada método
- Los usuarios finales pueden registrar y administrar estos métodos de autenticación sin contraseña en el portal de la cuenta
- Los usuarios finales pueden iniciar sesión con estos métodos de autenticación sin contraseña
   - Aplicación Microsoft Authenticator: Funciona en los escenarios donde se usa la autenticación de Azure AD, lo que incluye todos los exploradores, durante la configuración rápida (OOBE) de Windows 10 y con aplicaciones móviles integradas en cualquier sistema operativo.
   - Claves de seguridad: Funcionan en la pantalla de bloqueo de Windows 10 e Internet en exploradores compatibles como Microsoft Edge.

## <a name="next-steps"></a>Pasos siguientes

[Habilitar opciones sin contraseña de clave de seguridad FIDO2 en la organización](howto-authentication-passwordless-security-key.md)

[Habilitar opciones sin contraseña basadas en teléfono en la organización](howto-authentication-passwordless-phone.md)

### <a name="external-links"></a>Vínculos externos

[FIDO Alliance](https://fidoalliance.org/)

[Especificación FIDO2 CTAP](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
