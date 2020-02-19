---
title: Autenticación sin contraseña con Azure AD
titleSuffix: Active Directory
description: Esta guía está pensada para ayudar a los consejeros delegados (CEO), los directores de información (CIO), los directores de seguridad de la información (CISO), los arquitectos jefe de identidad, los arquitectos de empresa y los responsables de la toma de decisiones de TI a elegir un método de autenticación sin contraseña para la implementación de Azure Active Directory.
keywords: passwordless, azuread
author: martincoetzer
ms.author: martinco
ms.date: 07/09/2019
ms.topic: article
ms.service: security
ms.subservice: security-fundamentals
ms.workload: identity
ms.openlocfilehash: 5fc6a4a23573995cf791a21ec7cf3a7d68d048e8
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77064485"
---
# <a name="a-world-without-passwords-with-azure-active-directory"></a>Un mundo sin contraseñas con Azure Active Directory

Ya es hora de romper su relación con las contraseñas. Las contraseñas han sido útiles en el pasado, pero en el lugar de trabajo digital de hoy en día se han convertido en un vector de ataque relativamente sencillo para los hackers. A los hackers les encantan las contraseñas y no es difícil adivinar por qué si se considera que la mayoría de las contraseñas rechazadas en Azure Active Directory (Azure AD) incluyen términos como el año, el mes, la estación o un equipo deportivo local. Además, la [investigación ha demostrado](https://aka.ms/passwordguidance) que las recomendaciones tradicionales para la administración de contraseñas, como los requisitos de longitud y complejidad y las frecuencias de cambio son contraproducentes por diversos motivos relacionados con la naturaleza humana.

Tres tipos de ataques que se usan normalmente para poner en peligro las cuentas de usuario son la difusión de contraseña, la suplantación de identidad (phishing) y la reproducción de infracciones. Características de Azure AD como [bloqueo inteligente](../../active-directory/authentication/howto-password-smart-lockout.md), [contraseñas prohibidas](../../active-directory/authentication/concept-password-ban-bad-on-premises.md) y [protección con contraseñas](../../active-directory/authentication/concept-password-ban-bad-on-premises.md) pueden contribuir a la protección frente a estos tipos de ataques. Igualmente, la implementación de la [autenticación multifactor](../../active-directory/authentication/concept-mfa-howitworks.md) (MFA) o de la verificación en dos pasos proporcionan seguridad adicional al exigir una segunda forma de autenticación. No obstante, a largo plazo, una solución sin contraseña es lo mejor para garantizar el método de autenticación más seguro.

Este artículo es el comienzo del camino para ayudarle a entender y a implementar soluciones sin contraseña de Microsoft y facilitarle la elección entre una o varias de las siguientes opciones:

* **Windows Hello para empresas**. En Windows 10, Windows Hello para empresas reemplaza las contraseñas por la autenticación en dos fases segura en los PC y dispositivos móviles. Esta autenticación consta de un nuevo tipo de credencial de usuario que está vinculado a un dispositivo y utiliza un reconocimiento biométrico o un PIN.

* **Inicio de sesión sin contraseña de Microsoft Authenticator**. La aplicación Microsoft Authenticator se puede usar para iniciar sesión en una cuenta de Azure AD sin utilizar una contraseña. Al igual que la tecnología de Windows Hello para empresas, Microsoft Authenticator usa la autenticación basada en claves para habilitar una credencial de usuario que está vinculada a un dispositivo y usa información biométrica o un PIN.

* **Llaves de seguridad FIDO2**. FIDO2 proporciona credenciales de inicio de sesión criptográficas que son únicas en cada sitio web y se almacenan en un dispositivo local, como Windows Hello o llaves de seguridad externas. Estas llaves de seguridad son resistentes a los riesgos de suplantación de identidad (phishing), robo de contraseñas y ataques de reproducción. En combinación con la verificación del usuario mediante información biométrica o PIN, la solución constituye una verificación en dos pasos que satisface las necesidades de seguridad modernas.

## <a name="the-future-of-passwordless-authentication"></a>El futuro de la autenticación sin contraseña

Hoy en día, los bancos, las empresas de tarjetas de crédito y otras organizaciones y servicios en línea con frecuencia le piden que compruebe su identidad dos veces para proteger su cuenta: una vez con la contraseña y luego con el teléfono, mediante texto o una aplicación. Aunque la autenticación multifactor soluciona el problema de seguridad de uso compartido, robo o adivinación de contraseñas, no resuelve la inconveniencia de intentar recordarlas. Lo que desean los usuarios y las organizaciones de la era actual de la nube son métodos de autenticación sin contraseña que sean prácticos *y* seguros.

![Comodidad frente a seguridad](./media/ad-passwordless/azure-ad-pwdless-image1.png)

Windows Hello para empresas, el inicio de sesión sin contraseña con Microsoft Authenticator y las llaves de seguridad FIDO2 comparten todos una arquitectura común simple que proporciona a los usuarios un método de autenticación que es práctico y seguro. Los tres métodos se basan en la tecnología de clave pública-privada, necesitan un gesto local, como información biométrica o un PIN, y que las claves privadas estén vinculadas a un solo dispositivo, se protejan de forma segura y nunca se compartan.

## <a name="windows-hello-for-business"></a>Windows Hello para empresas

En Windows 10, Windows Hello para empresas reemplaza las contraseñas por la autenticación en dos fases segura en equipos y dispositivos. La autenticación consta de un nuevo tipo de credencial de usuario que está vinculada a un dispositivo, y usa un gesto biométrico o un PIN que permite a los usuarios autenticarse en Azure AD, así como en Active Directory local. Iniciar sesión en un dispositivo con Windows Hello para empresas es fácil. Se usa un PIN o un gesto biométrico, como la huella digital o el reconocimiento facial.

### <a name="windows-hello-for-business-scenarios"></a>Escenarios de Windows Hello para empresas

Windows Hello para empresas resulta muy conveniente para los trabajadores de la información que tienen su propio PC con Windows designado. La información biométrica y las credenciales están vinculadas directamente al PC del usuario, lo que impide el acceso de cualquier persona que no sea el propietario. Con la integración de PKI y la compatibilidad integrada con el inicio de sesión único (SSO), Windows Hello para empresas ofrece un método sencillo y práctico de acceder completamente a los recursos corporativos del entorno local y la nube.

### <a name="windows-hello-for-business-deployment-considerations"></a>Cuestiones sobre la implementación de Windows Hello para empresas

Windows Hello para empresas es un sistema distribuido que usa varios componentes para realizar el registro, el aprovisionamiento y la autenticación de los dispositivos. Por lo tanto, la implementación requiere un planeamiento adecuado entre varios equipos humanos de la organización. La [guía de planeamiento](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) de Windows Hello para empresas se puede usar para ayudarle a tomar decisiones sobre el tipo de implementación y las opciones que es necesario tener en cuenta.

Hay muchas opciones para elegir al implementar Windows Hello para empresas. Al haber tantas opciones, se garantiza que casi todas las organizaciones pueden implementarlo. Tenga en cuenta los siguientes tipos de implementaciones que se admiten:

* [Implementación de confianza de la clave unida a Azure AD híbrido](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-key-trust)

* [Implementación de confianza del certificado unido a Azure AD híbrido](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-cert-trust)

* [Guías de implementación de inicio de sesión único unido a Azure AD](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-hybrid-aadj-sso)

* [Implementación de confianza de la clave local](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-key-trust)

* [Implementación de confianza del certificado local](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-cert-trust)

Puede que al haber tantas opciones parezca que la implementación es compleja. Sin embargo, la mayoría de las organizaciones probablemente determinarán que ya tienen implementada la mayor parte de la infraestructura de la que depende Windows Hello para empresas. No obstante, es importante entender que Windows Hello para empresas es un sistema distribuido y, por tanto, se recomienda un planeamiento adecuado.

Le recomendamos que lea [Planificar una implementación de Windows Hello para empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-planning-guide) para ayudarle a decidir qué modelo de implementación es el más adecuado para su organización en particular. Luego, en función del planeamiento realizado, consulte la [Guía de implementación de Windows Hello para empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-deployment-guide) para ayudar a garantizar la implementación satisfactoria de Windows Hello para empresas en su entorno existente.

### <a name="how-windows-hello-for-business-works"></a>Funcionamiento de Windows Hello para empresas

#### <a name="user-sets-up-windows-hello-for-business"></a>El usuario instala Windows Hello para empresas

Tras una verificación en dos pasos inicial del usuario durante la inscripción, Windows Hello se instala en el dispositivo del usuario y Windows le pide a este que establezca un gesto, que puede ser biométrico, como una huella digital o el reconocimiento facial, o un PIN. Una vez configurado, el usuario proporciona el gesto para comprobar su identidad. Windows usa entonces Windows Hello para autenticar a los usuarios.

Según las funcionalidades del dispositivo Windows 10, tendrá un enclave seguro integrado, conocido como módulo de plataforma segura (TPM) de hardware o software. El TPM almacena la clave privada, que requiere su rostro, huella digital o PIN para desbloquearla. La información biométrica no se mueve y nunca se envía a servidores o dispositivos externos. No hay ningún punto de recopilación que un atacante pueda poner en peligro para robar información biométrica, porque Windows Hello solo almacena los datos biométricos de identificación en el dispositivo.

> [!TIP]
> Aparentemente, un PIN es como una contraseña, pero es realmente más seguro. Una diferencia importante entre una contraseña y un PIN es que el PIN está vinculado con el dispositivo específico en el que se ha configurado. Alguien que robe su contraseña puede iniciar sesión en su cuenta desde cualquier parte. Pero si roban su PIN, tendrían que robarle también su dispositivo físico. Además, como el PIN es local para el dispositivo, no se transmite a ninguna parte por lo que no se puede interceptar durante la transmisión ni robarse desde un servidor.

#### <a name="user-using-windows-hello-for-business-for-sign-in"></a>El usuario usa Windows Hello para empresas para el inicio de sesión

La información biométrica y los números PIN de Windows Hello para empresas usan el cifrado asimétrico (clave pública-privada) para la autenticación. Durante la autenticación, el cifrado se vincula a la clave de sesión TLS, que protege el proceso de autenticación para que un ataque man-in-the-middle (MiTM) no pueda robar el token o el artefacto de seguridad resultante y reproducirlos desde otros lugares.

Windows Hello para empresas ofrece una práctica experiencia de inicio de sesión que autentica al usuario en los recursos de Azure AD y Active Directory. Los dispositivos unidos a Azure AD se autentican en Azure durante el inicio de sesión y, opcionalmente, pueden autenticarse en Active Directory. Los dispositivos unidos a Azure Active Directory híbrido se autentican en Active Directory durante el inicio de sesión y se autentican en Azure Active Directory en segundo plano.

![Consulte la imagen de origen](./media/ad-passwordless/azure-ad-pwdless-image2.jpeg)

Los siguientes pasos muestran la autenticación de inicio de sesión en Azure Active Directory.

![Pantalla de bloqueo de Windows 10](./media/ad-passwordless/azure-ad-pwdless-image3.png)

1. El usuario inicia sesión en Windows mediante un gesto biométrico o un PIN. El gesto desbloquea la clave privada de Windows Hello para empresas y se envía al proveedor de compatibilidad para seguridad de la autenticación en la nube, conocido como el proveedor de CloudAP.

2. El proveedor de CloudAP solicita un valor nonce a Azure Active Directory.

3. Azure AD devuelve un valor nonce que es válido durante cinco minutos.

4. El proveedor de CloudAP firma el valor nonce con la clave privada del usuario y devuelve el valor nonce firmado a Azure Active Directory.

5. Azure Active Directory valida el valor nonce firmado con la clave pública registrada de forma segura del usuario con la firma del valor nonce. Después de validar la firma, Azure AD valida el valor nonce firmado devuelto. Tras validar el valor nonce, Azure AD crea un PRT con la clave de sesión que se cifró con la clave de transporte del dispositivo y lo devuelve al proveedor de CloudAP.

6. El proveedor de CloudAP recibe el PRT cifrado con la clave de sesión. Mediante la clave de transporte privada del dispositivo, el proveedor de CloudAP descifra la clave de sesión y la protege con el TPM del dispositivo.

7. El proveedor de CloudAP devuelve una respuesta de autenticación correcta a Windows tras la cual el usuario puede tener acceso a Windows, así como a las aplicaciones locales y en la nube sin necesidad de volver a autenticarse (SSO).

Para examinar más detenidamente el proceso de autenticación en otros escenarios con Windows Hello para empresas, consulte [Windows Hello para empresas y autenticación](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-how-it-works-authentication#azure-ad-join-authentication-to-active-directory-using-a-key).

#### <a name="user-manages-their-windows-hello-for-business-credentials"></a>El usuario administra sus credenciales de Windows Hello para empresas

El [servicio de restablecimiento del PIN de Microsoft](/windows/security/identity-protection/hello-for-business/hello-feature-pin-reset) es una característica de Azure AD que permite a los usuarios restablecer su PIN si es necesario. Mediante la directiva de grupo, Microsoft Intune o una MDM compatible, un administrador puede configurar los dispositivos Windows 10 para usar de forma segura el servicio de restablecimiento de PIN de Microsoft que permite a los usuarios restablecer su PIN olvidado mediante configuración o encima de la pantalla de bloqueo sin necesidad de volver a inscribirse.

En ocasiones tienen que recurrir al uso de contraseñas. El [autoservicio de restablecimiento de contraseña](../../active-directory/authentication/howto-sspr-deployment.md) (SSPR) es otra característica de Azure AD que permite a los usuarios restablecer sus contraseñas sin necesidad de ponerse en contacto con el personal de TI. Los usuarios deben registrarse o ser registrados en este servicio para poder usarlo. Durante el registro, el usuario elige uno o varios métodos de autenticación habilitados por su organización. SSPR permite el desbloqueo rápido de los usuarios para que continúen trabajando con independencia de dónde se encuentren o la hora del día. Al permitir que los usuarios se desbloqueen a sí mismos, su organización puede reducir el tiempo no productivo y los altos costos de soporte técnico para los problemas más comunes relacionados con las contraseñas.

## <a name="passwordless-sign-in-with-microsoft-authenticator"></a>Inicio de sesión sin contraseña de Microsoft Authenticator

El inicio de sesión sin contraseña con Microsoft Authenticator es otra solución sin contraseña que se puede usar para iniciar sesión en las cuentas de Azure AD mediante el inicio de sesión en el teléfono. Para verificar su identidad debe especificar algo que sepa y algo que tenga, pero el inicio de sesión en el teléfono permite omitir el paso de escribir la contraseña y realiza toda la comprobación de la identidad en el dispositivo móvil con la huella digital, la cara o el PIN.

### <a name="microsoft-authenticator-passwordless-scenarios"></a>Escenarios sin contraseña de Microsoft Authenticator

La aplicación Microsoft Authenticator permite a los usuarios comprobar su identidad y autenticarse en su cuenta personal o profesional. También puede usarse para aumentar una contraseña con un código de acceso de un solo uso o una notificación push o reemplazar completamente la necesidad de una contraseña. En lugar de usar una contraseña, los usuarios confirman su identidad con su teléfono móvil mediante la huella digital, el reconocimiento facial o del iris o el número PIN. Esta herramienta, basada en tecnología segura similar a la que usa Windows Hello para empresas, se empaqueta en una aplicación sencilla en un dispositivo móvil, lo que la convierte en una opción práctica para los usuarios. La aplicación Microsoft Authenticator está disponible para Android e iOS.

### <a name="microsoft-authenticator-deployment-considerations"></a>Cuestiones sobre la implementación de Microsoft Authenticator

Los requisitos previos para usar la aplicación Microsoft Authenticator para realizar el inicio de sesión sin contraseña en Azure AD incluyen los siguientes:

* Los usuarios finales deben estar habilitados para Azure Multi-Factor Authentication.

* Se recomienda que los usuarios inscriban sus dispositivos mediante Microsoft Intune o una solución de administración de dispositivos móviles (MDM) de terceros para facilitar la implementación de la aplicación.

Suponiendo que se cumplen estos requisitos, los administradores habilitan el inicio de sesión en el teléfono sin contraseña con [Windows PowerShell.](../../active-directory/authentication/howto-authentication-passwordless-phone.md) Una vez que el inicio de sesión en el teléfono está habilitado en el inquilino, los usuarios finales pueden elegir iniciar sesión con su teléfono; para ello, seleccionan su cuenta profesional o educativa en la pantalla **Cuentas** de la aplicación y, luego, seleccionan **Habilitar inicio de sesión en el teléfono**.

Suponiendo que un administrador haya habilitado el inicio de sesión sin contraseña, los usuarios finales deberán cumplir los siguientes requisitos:

* Estar inscritos en Azure Multi-factor Authentication.

* Tener instalada la versión más reciente de Microsoft Authenticator en dispositivos con iOS 8.0 o superior o Android 6.0 o superior.

* Haber agregado una cuenta profesional o educativa con notificaciones push a la aplicación.

Para evitar la posibilidad de que se le bloquee su cuentas o tener que volver a crear las cuentas en un nuevo dispositivo, se recomienda usar Microsoft Authenticator para [hacer una copia de seguridad de las credenciales de la cuenta](../../active-directory/user-help/user-help-auth-app-backup-recovery.md) en la nube. Después de la copia de seguridad, también puede utilizar la aplicación para recuperar la información en un dispositivo nuevo y evitar así un posible bloqueo o tener que volver a crear las cuentas.

Dado que la mayoría de los usuarios están acostumbrados a usar solo las contraseñas para autenticarse, es importante que la organización instruya a los usuarios sobre este proceso. Conocerlo puede reducir la probabilidad de que los usuarios llamen al departamento de soporte técnico por cualquier [problema](../../active-directory/authentication/howto-authentication-passwordless-phone.md) relacionado con el inicio de sesión con la aplicación Microsoft Authenticator.

> [!NOTE]
> Un posible punto de error de esta solución es cuando un usuario itinerante se encuentra en una ubicación donde no hay conexión a Internet. Las llaves de seguridad FIDO2 y Windows Hello para empresas no están sujetos a la misma limitación.

### <a name="how-passwordless-sign-in-with-microsoft-authenticator-works"></a>Cómo funciona el inicio de sesión sin contraseña con Microsoft Authenticator

#### <a name="user-sets-up-passwordless-sign-in-with-microsoft-authenticator"></a>El usuario configura el inicio de sesión sin contraseña con Microsoft Authenticator

Para que la aplicación Microsoft Authenticator se pueda usar como una solución sin contraseña para iniciar sesión en una cuenta de Azure AD, tanto el administrador como los usuarios deben realizar una serie de pasos.

En primer lugar, un administrador deberá [habilitar el uso de la aplicación como una credencial](../../active-directory/authentication/howto-authentication-passwordless-phone.md) en el inquilino mediante Windows PowerShell. El administrador también deberá habilitar a los usuarios finales en Azure Multi-Factor Authentication (Azure MFA) y configurar la aplicación Microsoft Authenticator como uno de los [métodos de verificación](../../active-directory/authentication/howto-mfa-mfasettings.md#verification-methods).

Los usuarios finales deberán [descargar e instalar](../../active-directory/user-help/user-help-auth-app-download-install.md) la aplicación Microsoft Authenticator y [configurar su cuenta](../../active-directory/user-help/security-info-setup-auth-app.md) para usar esta aplicación como uno de los métodos de verificación siguientes.

> [!VIDEO https://www.youtube.com/embed/uWbkLuI4g30]

#### <a name="user-using-microsoft-authenticator-for-passwordless-sign-in"></a>El usuario usa Microsoft Authenticator para el inicio de sesión sin contraseña

La aplicación Microsoft Authenticator se puede utilizar para iniciar sesión en cualquier cuenta de Azure AD sin utilizar una contraseña. Si bien la pantalla de bloqueo de Windows 10 no incluye la aplicación Microsoft Authenticator como opción de inicio de sesión, los usuarios pueden escribir su nombre de usuario y recibir a continuación una notificación push en su dispositivo móvil para comprobar su presencia. Para confirmarla, los usuarios escriben un número que coincida en la pantalla de inicio de sesión y, luego, emplean el reconocimiento facial, la huella digital o un número PIN para desbloquear la clave privada y completar la autenticación. Este método de verificación multifactor es más seguro que una contraseña y más práctico que escribir una contraseña y un código.

![Inicio de sesión de Authenticator](./media/ad-passwordless/azure-ad-pwdless-image4.png)

La autenticación sin contraseña con Microsoft Authenticator sigue el mismo patrón básico que Windows Hello para empresas, pero es un poco más complicado, ya que el usuario debe identificarse para que Azure AD pueda encontrar la versión de la aplicación Microsoft Authenticator que se usa.

![Proceso de Authenticator](./media/ad-passwordless/azure-ad-pwdless-image5.png)

1. El usuario escribe su nombre de usuario.

2. Azure AD detecta que el usuario tiene una credencial segura e inicia el flujo de credencial segura.

3. Se envía una notificación a la aplicación mediante Apple Push Notification Service (APNS) en dispositivos iOS o por medio de Firebase Cloud Messaging (FCM) en dispositivos Android.

4. El usuario recibe la notificación push y abre la aplicación.

5. La aplicación llama a Azure AD y recibe un desafío de prueba de presencia y un valor nonce.

6. Para completar el desafío, el usuario escribe su información biométrica o su PIN para desbloquear la clave privada.

7. El valor nonce se firma con la clave privada y se envía a Azure AD.

8. Azure AD realiza la validación de la clave pública-privada y devuelve un token.

#### <a name="user-manages-their-passwordless-sign-in-with-microsoft-authenticator-credentials"></a>El usuario administra su inicio de sesión sin contraseña con las credenciales de Microsoft Authenticator

Con el [registro combinado](../../active-directory/authentication/concept-registration-mfa-sspr-combined.md), los usuarios pueden registrarse una vez y obtener las ventajas de Multi-Factor Authentication y del autoservicio de restablecimiento de contraseña. Los usuarios se registran y administran esta configuración en la [página Mi perfil](https://aka.ms/mysecurityinfo). Además de habilitar SSPR, el registro combinado admite varios métodos de autenticación y acciones.

## <a name="fido2-security-keys"></a>Claves de seguridad FIDO2

FIDO2 es la versión más reciente del estándar FIDO Alliance y tiene dos componentes: el estándar de autenticación web de W3C (WebAuthN) y el protocolo correspondiente de cliente a autenticador (CTAP2) de FIDO Alliance. Los estándares FIDO2 permiten a los usuarios aprovechar los autenticadores basados en hardware, dispositivos móviles e información biométrica para autenticarse fácilmente con muchas aplicaciones y sitios web en los entornos móviles y de escritorio.

Microsoft y asociados del sector han estado trabajando juntos en dispositivos de seguridad FIDO2 para Windows Hello a fin de permitir la autenticación sencilla y segura en dispositivos compartidos. Las claves de seguridad FIDO2 le permiten llevar encima sus credenciales y autenticarse de forma segura en un dispositivo Windows 10 unido a [Azure AD](https://aka.ms/azuread418) que forme parte de su organización.

WebAuthN define una API que permite el desarrollo y la implementación de una autenticación sin contraseña segura mediante aplicaciones y servicios web. El protocolo CTAP permite que dispositivos externos, como las claves de seguridad compatibles con FIDO, funcionen con WebAuthN y sirvan como autenticadores. Con la autenticación web, los usuarios pueden iniciar sesión en servicios en línea con su cara, huella digital, PIN o claves de seguridad FIDO2 portátiles y hacer uso de credenciales de clave pública seguras en lugar de contraseñas. Actualmente WebAuthN se admite en Microsoft Edge y la compatibilidad con Chrome y Firefox está en desarrollo.

Los dispositivos y los tokens que se adhieren a los protocolos FIDO2, WebAuthN y CTAP generan una solución multiplataforma de autenticación segura sin el uso de contraseñas. Los asociados de Microsoft trabajan en diversos factores de forma de llaves de seguridad, como las llaves de seguridad USB y las tarjetas inteligentes habilitadas para NFC.

### <a name="fido2-security-keys-scenarios"></a>Escenarios de las llaves de seguridad FIDO2

Las llaves de seguridad FIDO2 pueden usarse para iniciar sesión en Azure AD mediante la elección de la llave de seguridad como proveedor de credenciales en la pantalla de bloqueo de Windows 10. Al no necesitarse un nombre de usuario y una contraseña, es la solución perfecta para los trabajadores de primera línea con equipos compartidos entre varios usuarios. También es una opción de autenticación excelente cuando las directivas corporativas dictaminan que las credenciales del usuario deben estar físicamente separadas de su dispositivo. Los usuarios pueden optar por iniciar sesión en los sitios web con su llave de seguridad FIDO2 dentro del explorador Microsoft Edge en Windows 10 versión 1809 o posterior.

### <a name="fido2-security-keys-deployment-considerations"></a>Cuestiones sobre la implementación de las llaves de seguridad FIDO2

Los administradores pueden habilitar la compatibilidad con FIDO2 en Azure AD y asignar la funcionalidad a los usuarios o grupos. También se pueden crear directivas para el modo en que se aprovisionan las claves y configurar restricciones, como permitir o bloquear un conjunto específico de llaves de seguridad de hardware. Las llaves deben distribuirse físicamente a los usuarios finales.

**Los requisitos para habilitar el inicio de sesión único sin contraseña en Azure AD y en los sitios web con las llaves de seguridad FIDO2 son estos:**

* Azure AD

* Azure Multi-Factor Authentication

* Versión preliminar de registro combinado

* La versión preliminar de la llave de seguridad FIDO2 requiere una llave de seguridad FIDO2 compatible

* La autenticación web (WebAuthN) requiere Microsoft Edge en Windows 10 versión 1809 o superior

* El inicio de sesión en Windows basado en FIDO2 requiere Windows 10 versión 1809 o superior unido a Azure AD (la mejor experiencia es con Windows 10 versión 1903 o posterior)

Los factores de forma compatibles con FIDO2 incluyen dispositivos USB, NFC y Bluetooth. Se recomienda elegir el factor de forma que satisfaga sus necesidades específicas dado que algunas plataformas y exploradores no son aún compatibles con FIDO2.

También se recomienda que cada organización cree un protocolo que deben seguir los usuarios y los administradores en caso del robo o la pérdida de una llave de seguridad. Los usuarios deben informar del robo o la pérdida de una llave para que los administradores o el usuario puedan eliminar sus llaves de seguridad del perfil de usuario y proporcionarles una nueva.

### <a name="how-fido2-security-keys-works"></a>Funcionamiento de las llaves de seguridad FIDO2

#### <a name="user-sets-up-fido2-security-key"></a>El usuario configura la llave de seguridad FIDO2

Si bien los administradores pueden [proporcionar llaves manualmente](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless) y distribuirlas a los usuarios finales, para aprovisionar y habilitar el proveedor de credenciales FIDO2 en la pantalla de bloqueo de Windows 10 se usa [Intune](https://docs.microsoft.com/intune/windows-enrollment-methods). Los administradores también deberán usar [Azure Portal](https://portal.azure.com/) para habilitar dispositivos de token de hardware como método de autenticación sin contraseña.

La implementación de las llaves de seguridad FIDO2 también requiere que los usuarios registren sus llaves mediante [registro combinado](../../active-directory/authentication/concept-registration-mfa-sspr-combined.md). Con el registro combinado, los usuarios se registran una vez y obtienen las ventajas de Azure Multi-Factor Authentication y del autoservicio de restablecimiento de contraseña (SSPR).

Además de seleccionar el token de hardware como método de autenticación multifactor predeterminado, se recomienda que también seleccione una opción de verificación adicional.

* Microsoft Authenticator: notificación

* Aplicación Authenticator o token de hardware: código

* llamada de teléfono

* mensaje de texto

#### <a name="user-using-fido2-security-key-for-sign-in"></a>El usuario usa la clave de seguridad FIDO2 para el inicio de sesión

FIDO2 proporciona una capa de abstracción entre el factor de forma que se usa como autenticador y la criptografía de clave pública-privada para permitir a los autenticadores integrados de la plataforma, como Windows Hello y las llaves de seguridad, resolverse como una clave privada y entregar una clave pública que pueda usarse como identificador para acceder a recursos externos. Las llaves de seguridad FIDO2 incorporan su propio enclave seguro integrado que almacena la clave privada y solicita la información biométrica o el PIN para desbloquearla. Las credenciales no se puede reutilizar, reproducir ni compartir entre servicios, y no están sujetas a ataques de suplantación de identidad y MiTM ni a infracciones del servidor.

![Inicio de sesión de FIDO2](./media/ad-passwordless/azure-ad-pwdless-image6.png)

Las llaves de seguridad FIDO2 proporcionan una autenticación segura, con independencia del factor de forma. La llave de seguridad contiene la credencial y debe protegerse con un segundo factor adicional, como una huella digital (integrada en la clave de seguridad) o un PIN para escribirse en el inicio de sesión de Windows. Los asociados de Microsoft trabajan en una variedad de factores de forma de llave de seguridad. Algunos ejemplos incluyen llaves de seguridad USB y tarjetas inteligentes habilitadas para NFC.

> [!NOTE]
> Una llave de seguridad DEBE implementar determinadas características y extensiones del protocolo FIDO2 CTAP para ser [compatibles con Microsoft](https://aka.ms/fido2securitykeys). Microsoft ha probado la compatibilidad de estas soluciones con Windows 10 y Azure Active Directory.

![Proceso de inicio de sesión de FIDO2](./media/ad-passwordless/azure-ad-pwdless-image9.png)

1. El usuario conecta el dispositivo FIDO2 al equipo.

2. Windows detecta la llave de seguridad FIDO2.

3. Windows envía una solicitud de autenticación.

4. Azure AD devuelve un valor nonce.

5. El usuario realiza su gesto para desbloquear la clave privada almacenada en el enclave seguro de la llave de seguridad FIDO2.

6. La llave de seguridad FIDO2 firma el valor nonce con la clave privada.

7. La solicitud de token PRT con el valor nonce firmado se envía a Azure AD.

8. Azure AD comprueba el valor nonce firmado con la clave pública FIDO2.

9. Azure AD devuelve el PRT para permitir el acceso a los recursos locales.

#### <a name="user-manages-their-fido2-security-key-credentials"></a>El usuario administra sus credenciales de llave de seguridad FIDO2

Al igual que la aplicación Microsoft Authenticator, la administración de credenciales de las llaves de seguridad FIDO2 se basa en la experiencia de registro combinado de los usuarios finales.

## <a name="deciding-a-passwordless-method"></a>Elección de un método sin contraseña

La decisión entre estas tres opciones sin contraseña depende de los requisitos de seguridad, plataforma y aplicación de su empresa.

Estos son algunos de los factores que se deben tener en cuenta al elegir la tecnología de Microsoft sin contraseña:

||**Windows Hello para empresas**|**Inicio de sesión sin contraseña con la aplicación Microsoft Authenticator**|**Llaves de seguridad FIDO2**|
|:-|:-|:-|:-|
|**Requisito previo**| Windows 10, versión 1809 o posterior<br>Azure Active Directory| Aplicación Microsoft Authenticator<br>Teléfono (dispositivos iOS y Android que ejecutan Android 6.0 o posterior)|Windows 10, versión 1809 o posterior<br>Azure Active Directory|
|**Modo**|Plataforma|Software|Hardware|
|**Sistemas y dispositivos**|PC con un módulo de plataforma segura (TPM) integrado<br>Reconocimiento de PIN e información biométrica |PIN y reconocimiento biométrico en el teléfono|Dispositivos de seguridad FIDO2 que son compatibles con Microsoft|
|**Experiencia del usuario**|Inicie sesión con un PIN o mediante reconocimiento biométrico (facial, iris o huella digital) con dispositivos Windows.<br>La autenticación de Windows Hello está vinculada al dispositivo; el usuario necesita el dispositivo y un componente de inicio de sesión, como un PIN o un factor biométrico, para acceder a los recursos corporativos.|Inicio de sesión con un teléfono móvil con la huella digital, el reconocimiento facial o del iris, o bien con un PIN.<br>Los usuarios inician sesión en su cuenta profesional o personal desde su PC o teléfono móvil.|Inicio de sesión con el dispositivo de seguridad FIDO2 (información biométrica, PIN y NFC)<br>El usuario puede acceder al dispositivo según los controles de la organización y autenticarse con un PIN, información biométrica mediante dispositivos como llaves de seguridad USB, y por medio de tarjetas inteligentes, llaves o dispositivos ponibles habilitados para NFC.|
|**Escenarios habilitados**| Experiencia sin contraseña con dispositivos Windows.<br>Se aplica a PC de trabajo dedicados con posibilidad de inicio de sesión único en aplicaciones y dispositivos.|Solución en cualquier parte sin contraseña con el teléfono móvil.<br>Se aplica para el acceso a aplicaciones de trabajo o personales en la web desde cualquier dispositivo.|Experiencia sin contraseña para los trabajadores con NFC, información biométrica y PIN.<br>Aplicable a PC compartidos y cuando un teléfono móvil no es una opción viable (por ejemplo, personal del departamento de soporte técnico, quiosco multimedia público o equipo de hospital).|

Use la tabla siguiente para elegir qué método será más adecuado para sus requisitos y usuarios.

|Persona|Escenario|Entorno|Tecnología sin contraseña|
|:-|:-|:-|:-|
|**Administrador**|Acceso seguro a un dispositivo para tareas de administración|Dispositivo Windows 10 asignado|Windows Hello para empresas, llave de seguridad FIDO2 o ambos|
|**Administrador**|Tareas de administración en dispositivos que no son Windows| Dispositivo móvil o que no sean Windows|Inicio de sesión sin contraseña con la aplicación Microsoft Authenticator|
|**Trabajador de la información**|Trabajo de productividad|Dispositivo Windows 10 asignado|Windows Hello para empresas, llave de seguridad FIDO2 o ambos|
|**Trabajador de la información**|Trabajo de productividad| Dispositivo móvil o que no sean Windows|Inicio de sesión sin contraseña con la aplicación Microsoft Authenticator|
|**Trabajador de primera línea**|Quioscos multimedia de una fábrica, planta, comercio o entrada de datos|Dispositivos Windows 10 compartidos|Llaves de seguridad FIDO2|

## <a name="getting-started"></a>Introducción

La autenticación sin contraseña es el futuro y el camino a un entorno más seguro. Se recomienda a las organizaciones empezar a planear este cambio y reducir sus dependencias de las contraseñas. Para comenzar, considere los siguientes objetivos:

* Habilitar a los usuarios para MFA + aplicación Microsoft Authenticator + acceso condicional

* Lanzar la protección con contraseña de Azure AD + directivas de actualización

* Habilitar a los usuarios para SSPR con registro combinado

* Implementar la aplicación Microsoft Authenticator para la movilidad

* Implementar Windows Hello para empresas (1909: mantenerse al día)

* Implementar dispositivos FIDO2 para los usuarios que no puedan usar teléfonos

* Cuando sea posible, deshabilitar la autenticación basada en contraseña

Para lograr estos objetivos, se recomienda el siguiente enfoque:

1. Habilitar Azure Active Directory para aprovechar al máximo características como Azure MFA y acceso condicional

2. Habilitar la autenticación multifactor para proporcionar protección adicional

3. Habilitar el autoservicio de restablecimiento de contraseña en caso de que los usuarios necesiten recurrir al uso de contraseñas

4. Implementar el inicio de sesión en el teléfono de Microsoft Authenticator para una mayor movilidad

5. Implementar Windows Hello para empresas en todos los dispositivos Windows 10

6. Preparar las llaves de seguridad FIDO2

> [!NOTE]
> Para más información sobre los requisitos de licencia de los métodos sin contraseña, consulte la [página de licencia](https://azure.microsoft.com/pricing/details/active-directory/) de Azure Active Directory.

## <a name="conclusion"></a>Conclusión

En los últimos años, Microsoft ha continuado su compromiso de crear un mundo sin contraseñas. Con Windows 10, Microsoft introdujo Windows Hello para empresas, una credencial en dos fases segura protegida por hardware que permite el inicio de sesión único en Azure Active Directory y Active Directory. Al igual que la tecnología de Windows Hello para empresas, la aplicación Microsoft Authenticator usa autenticación basada en claves para habilitar una credencial de usuario que está vinculada a un dispositivo y utiliza información biométrica o un PIN. Ahora las llaves de seguridad FIDO2 le permiten llevar encima sus credenciales e iniciar sesión en Azure AD mediante la elección de la llave de seguridad como proveedor de credenciales en la pantalla de bloqueo de Windows 10. Estas tres soluciones sin contraseña reducen el riesgo de ataques de suplantación de identidad (phishing), difusión de contraseñas y reproducción y proporcionan a los usuarios una manera muy segura y práctica de iniciar sesión y acceder a los datos desde cualquier parte.

La adopción de tecnologías modernas de autenticación multifactor, como lecturas biométricas y criptografía de claves públicas en dispositivos ampliamente accesibles es uno de los pasos con más repercusión que puede reducir considerablemente el riesgo de la identidad en las empresas. La transición a un mundo sin contraseñas es un enfoque a largo plazo para la autenticación segura y se encuentra en evolución. Dados los requisitos emergentes, las organizaciones pueden crear un plan y prepararse para comenzar a moverse a las tecnologías sin contraseña.

## <a name="next-steps"></a>Pasos siguientes

* Información general sobre lo que [significa el inicio de sesión sin contraseña](../../active-directory/authentication/concept-authentication-passwordless.md)
* [Cómo se habilita el inicio de sesión sin contraseña en Azure AD](https://docs.microsoft.com/azure/active-directory/authentication/concept-authentication-passwordless)
