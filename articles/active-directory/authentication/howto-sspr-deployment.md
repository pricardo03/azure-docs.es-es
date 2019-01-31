---
title: 'Guía de implementación de restablecimiento de contraseña de autoservicio: Azure Active Directory'
description: Sugerencia para el lanzamiento del autoservicio de restablecimiento de contraseña de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/17/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.openlocfilehash: 0111cb077afe0412d0ab807955c666b42dd1ff31
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55079620"
---
# <a name="how-to-successfully-roll-out-self-service-password-reset"></a>Cómo implementar correctamente el lanzamiento del restablecimiento de contraseña de autoservicio

Para garantizar un lanzamiento fluido de la funcionalidad del restablecimiento de contraseña de autoservicio (SSPR) de Azure Active Directory (Azure AD), la mayoría de los clientes completan los pasos siguientes:

> [!VIDEO https://www.youtube.com/embed/OZn5btP6ZXw]

1. Realización de una implementación piloto con un pequeño subconjunto de su organización.
   * Se puede encontrar más información sobre el procedimiento en el [Tutorial: Realización de una implementación piloto del autoservicio de restablecimiento de contraseñas en Azure AD](tutorial-sspr-pilot.md).
1. Ofrezca cursos a su departamento de soporte técnico.
   * ¿Cómo ayudarán a los usuarios?
   * ¿Va a exigir que los usuarios usen SSPR y no permitirá que el departamento de soporte técnico les ayude?
   * ¿Les ha proporcionado direcciones URL con fines de registro y restablecimiento?
      * Registro: https://aka.ms/ssprsetup
      * Restablecimiento: https://aka.ms/sspr
1. Ofrezca cursos a sus usuarios.
   * En las siguientes secciones de este documento se repasan ejemplos de comunicación, portales de contraseñas, aplicación del registro y relleno de los datos de autenticación.
   * El grupo de producto de Azure Active Directory ha creado un [plan de implementación paso a paso](https://aka.ms/SSPRDeploymentPlan) que las organizaciones pueden usar en paralelo con la documentación que se encuentra en este sitio para crear un caso empresarial y un plan de implementación de restablecimiento de contraseña de autoservicio.
1. Habilite el autoservicio de restablecimiento de contraseña para toda la organización.
   * Cuando esté listo, habilite el restablecimiento de contraseña para todos los usuarios al establecer **Se habilitó el restablecimiento de contraseña del autoservicio** en **Todos**.

## <a name="sample-communication"></a>Ejemplo de comunicación

Muchos clientes opinan que una campaña de correo electrónico, con instrucciones sencillas, es la manera más fácil de hacer que los usuarios utilicen SSPR. [Hemos creado varios correos electrónicos sencillos y otra documentación y material adjunto que se pueden utilizar como plantillas para facilitar el lanzamiento](https://www.microsoft.com/download/details.aspx?id=56768):

* **Próximamente**: una plantilla de correo electrónico para usar las semanas o los días previos al lanzamiento con el fin de informar a los usuarios de que deben hacer algo.
* **Ya disponible**: una plantilla de correo electrónico para que use el día del lanzamiento del programa con el fin de dirigir a los usuarios al registro y a confirmar los datos de autenticación. Si los usuarios se registran ahora, tendrán SSPR disponible para cuando lo necesiten.
* **Recordatorio de registro**: una plantilla de correo electrónico para los días o semanas previos a la implementación para recordar a los usuarios que se registren y confirmen los datos de autenticación.
* **Pósteres de SSPR**: pósteres que puede personalizar y mostrar en los días y semanas anteriores y posteriores a la implementación.
* **Tiendas de mesa de SSPR**: tarjetas de mesa que se pueden colocar en el comedor, en salas de conferencias o en mesas para animar a los usuarios a completar el registro.
* **Adhesivos de SSPR**: plantillas de adhesivo que puede personalizar e imprimir para colocar en equipos portátiles, monitores, teclados o teléfonos móviles para recordar cómo se obtiene acceso a SSPR.

![Ejemplos de correo electrónico de SSPR][Email]

## <a name="create-your-own-password-portal"></a>Creación de su propio portal de contraseñas

Muchos clientes eligen hospedar una página web y crear una entrada DNS raíz, como https://passwords.contoso.com. Rellenan esta página con vínculos a la información siguiente:

* [Portal de restablecimiento de contraseña de Azure AD: https://aka.ms/sspr](https://aka.ms/sspr)
* [Portal de registro de restablecimiento de contraseña de Azure AD: https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)
* [Portal de cambio de contraseñas de Azure AD: https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* Otra información específica de la organización

En las comunicaciones por correo electrónico o los folletos que envíe, puede incluir una dirección URL de la marca fácil de recordar que los usuarios puedan visitar cuando necesiten usar los servicios. Para ayudarle, hemos creado una [página de ejemplo de restablecimiento de contraseña](https://github.com/ajamess/password-reset-page) que puede usar y personalizar para las necesidades de su organización.

## <a name="use-enforced-registration"></a>Uso del registro forzado

Si desea que los usuarios se registren para el restablecimiento de la contraseña, puede solicitar que se registren cuando inician sesión con Azure AD. Puede habilitar esta opción desde el panel **Restablecimiento de contraseña** del directorio; para ello, habilite la opción **Exigir a los usuarios que se registren al iniciar sesión** de la pestaña **Registro**.

Los administradores pueden solicitar a los usuarios que se vuelvan a registrar después de un período de tiempo específico. Pueden establecer la opción **Número de días que pasan hasta que se pide a los usuarios que vuelvan a confirmar su información de autenticación** de 0 a 730 días.

Después de habilitar esta opción, cuando los usuarios inicien sesión verán un mensaje que les informará de que el administrador les solicita que comprueben la información de autenticación.

## <a name="populate-authentication-data"></a>Rellenado de los datos de autenticación

Debe plantearse [rellenar previamente algunos datos de autenticación para los usuarios](howto-sspr-authenticationdata.md). De este modo los usuarios no necesitan registrarse para restablecer la contraseña antes de poder usar SSPR. Siempre que los datos de autenticación proporcionados por los usuarios se correspondan con la directiva de restablecimiento de contraseña definida, se podrán restablecer las contraseñas.

## <a name="disable-self-service-password-reset"></a>Deshabilitación del autoservicio de restablecimiento de contraseña

Si su organización decide deshabilitar el autoservicio de restablecimiento de contraseña, se trata de un proceso sencillo. Abra el inquilino de Azure AD y vaya a **Restablecimiento de contraseña** > **Propiedades** y seleccione **Ninguno** en **Se habilitó el restablecimiento de contraseña del autoservicio**. Los usuarios seguirán manteniendo sus métodos de autenticación registrados para su uso futuro.

## <a name="next-steps"></a>Pasos siguientes

* [Restablecimiento o modificación de la contraseña](../user-help/active-directory-passwords-update-your-own-password.md)
* [Registro para el autoservicio de restablecimiento de contraseñas](../user-help/active-directory-passwords-reset-register.md)
* [Habilitación del registro convergente para el restablecimiento de contraseña de autoservicio de Azure Multi-Factor Authentication y Azure AD](concept-registration-mfa-sspr-converged.md)
* [¿Tiene alguna pregunta acerca de las licencias?](concept-sspr-licensing.md)
* [¿Qué datos usa SSPR y cuáles se deben rellenar en lugar de los usuarios?](howto-sspr-authenticationdata.md)
* [¿Cuáles son las opciones de directiva con SSPR?](concept-sspr-policy.md)
* [¿Qué es la escritura diferida de contraseñas y por qué nos interesa?](howto-sspr-writeback.md)
* [¿Cómo se informa sobre la actividad de SSPR?](howto-sspr-reporting.md)
* [¿Cuáles son todas las opciones en SSPR y qué significan?](concept-sspr-howitworks.md)
* [Creo que algo se ha roto. ¿Cómo se solucionan problemas en SSPR?](active-directory-passwords-troubleshoot.md)
* [Tengo una pregunta que no se ha comentado en ningún otro sitio](active-directory-passwords-faq.md)

[Email]: ./media/howto-sspr-deployment/sspr-emailtemplates.png "Personalice estas plantillas de correo electrónico para ajustarlas a sus necesidades organizativas"
