---
title: Inicio de sesión sin contraseña de Azure Active Directory (versión preliminar)
description: Inicio de sesión sin contraseña en Azure AD con claves de seguridad FIDO2 o la aplicación Microsoft Authenticator (versión preliminar)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: librown
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2d80b359be0a6249327ba1ba1d51ffbc330bb073
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711969"
---
# <a name="what-is-passwordless"></a>¿Qué quiere decir sin contraseña?

La autenticación multifactor (MFA) es una excelente manera de proteger la organización, aunque los usuarios se frustran con el paso que se suma a tener que recordar sus contraseñas. Los métodos de autenticación sin contraseña resultan más cómodos, ya que la contraseña se quita y se reemplaza por algo que se tiene más algo que se es o se sabe.

|   | Algo que se tiene | Algo que se es o se sabe |
| --- | --- | --- |
| Inicio de sesión sin contraseña | Clave de seguridad o teléfono | PIN o biométrica |

Microsoft es consciente de que cada organización tiene diferentes necesidades en cuanto a la autenticación. Actualmente, ofrece Windows Hello, la experiencia sin contraseña Premier para equipos Windows. Se están agregando nuevas credenciales a la familia sin contraseña: Aplicación Microsoft Authenticator y claves de seguridad FIDO2.

## <a name="microsoft-authenticator-app"></a>Aplicación Microsoft Authenticator

Permita que el teléfono del empleado se convierta en un método de autenticación sin contraseña. Es posible que ya esté usando la aplicación Microsoft Authenticator como una opción de autenticación multifactor cómoda sumada a una contraseña. Pero ahora está disponible como una opción sin contraseña.

Convierte cualquier teléfono iOS o Android en una credencial sin contraseña segura al permitir a los usuarios iniciar sesión en cualquier plataforma o explorador mediante la recepción de una notificación en su teléfono, la coincidencia de un número que aparece en la pantalla con el de su teléfono y luego el uso de su biométrica (toque o cara) o PIN para confirmar.

## <a name="fido2-security-keys"></a>Claves de seguridad FIDO2

Las claves de seguridad FIDO2 son un método de autenticación sin contraseña basado en estándares que no permite la suplantación de identidad y que puede venir en cualquier factor de forma. Fast Identity Online (FIDO) es un estándar abierto para la autenticación sin contraseña. Permite a los usuarios y a las organizaciones aprovechar el estándar para iniciar sesión en sus recursos sin un nombre de usuario o una contraseña mediante una clave de seguridad externa o una clave de plataforma integrada en un dispositivo.

En la versión preliminar pública, los empleados pueden usar claves de seguridad externas para iniciar sesión en sus equipos Windows 10 unidos a Azure Active Directory (con la versión 1809 o superior) y lograr el inicio de sesión único en sus recursos de nube. También pueden iniciar sesión en exploradores compatibles.

Aunque hay muchas claves certificadas como FIDO2 por FIDO Alliance, Microsoft requiere la implementación de algunas extensiones opcionales de la especificación FIDO2 CTAP por parte del proveedor para garantizar la máxima seguridad y la mejor experiencia.

Una clave de seguridad **DEBE** implementar las siguientes extensiones y características del protocolo FIDO2 CTAP para ser compatible con Microsoft:

| # | Confianza de característica o extensión | ¿Por qué se requiere? |
| --- | --- | --- |
| 1 | Clave residente | Esta característica permite que la clave de seguridad sea portátil, ya que la credencial se almacena en la clave de seguridad. |
| 2 | PIN de cliente | Esta característica permite proteger las credenciales con un segundo factor y se aplica a las claves de seguridad que no tienen una interfaz de usuario. |
| 3 | hmac-secret | Esta extensión garantiza que pueda iniciar sesión en el dispositivo cuando está sin conexión o en modo avión. |
| 4 | Varias cuentas por RP | Esta característica garantiza que pueda usar la misma clave de seguridad en varios servicios, como Microsoft Account y Azure Active Directory. |

Los siguientes proveedores ofrecen claves de seguridad FIDO2 o diferentes factores de forma que se sabe que son compatibles con la experiencia sin contraseña. Microsoft anima a los clientes a evaluar las propiedades de seguridad de estas claves al ponerse en contacto con el proveedor y con FIDO Alliance.

| Proveedor | Contacto |
| --- | --- |
| Yubico | [https://www.yubico.com/support/contact/](https://www.yubico.com/support/contact/) |
| Feitian | [https://www.ftsafe.com/about/Contact_Us](https://www.ftsafe.com/about/Contact_Us) |
| HID | [https://www.hidglobal.com/contact-us](https://www.hidglobal.com/contact-us) |
| Ensurity | [https://ensurity.com/contact-us.html](https://ensurity.com/contact-us.html) |
| eWBM | [https://www.ewbm.com/page/sub1_5](https://www.ewbm.com/page/sub1_5) |

Si es proveedor y quiere que su dispositivo aparezca en esta lista, póngase en contacto con [Fido2Request@Microsoft.com](mailto:Fido2Request@Microsoft.com).

Las claves de seguridad FIDO2 son una excelente opción para las empresas que son muy conscientes de la seguridad o tienen escenarios o empleados que no quieren o no pueden usar su teléfono como un segundo factor.

## <a name="what-scenarios-work-with-the-preview"></a>¿Qué escenarios funcionan con la versión preliminar?

1. Los administradores pueden habilitar métodos de autenticación sin contraseña para su inquilino
1. Los administradores pueden establecer como destino a todos los usuarios o seleccionar usuarios o grupos dentro de su inquilino para cada método
1. Los usuarios finales pueden registrar y administrar estos métodos de autenticación sin contraseña en el portal de la cuenta
1. Los usuarios finales pueden iniciar sesión con estos métodos de autenticación sin contraseña
   1. Aplicación Microsoft Authenticator: funciona en todos los escenarios donde se usa la autenticación de Azure AD, lo que incluye todos los exploradores, durante la configuración rápida (OOBE) de Windows 10 y con aplicaciones móviles integradas en cualquier sistema operativo.
   1. Claves de seguridad: funcionan en la pantalla de bloqueo de Windows 10 versión 1809 o superior e Internet en exploradores compatibles como Microsoft Edge.

## <a name="next-steps"></a>Pasos siguientes

[Habilitar opciones sin contraseña en la organización](howto-authentication-passwordless-enable.md)

### <a name="external-links"></a>Vínculos externos

[FIDO Alliance](https://fidoalliance.org/)

[Especificación FIDO2 CTAP](https://fidoalliance.org/specs/fido-v2.0-id-20180227/fido-client-to-authenticator-protocol-v2.0-id-20180227.html)
