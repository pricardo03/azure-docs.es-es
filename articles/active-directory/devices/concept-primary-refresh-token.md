---
title: 'Token de actualización principal (PRT) y Azure AD: Azure Active Directory'
description: ¿Cuál es el rol de y cómo se administra la principal actualización de Token (PRT) en Azure Active Directory?
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: ravenn
ms.collection: M365-identity-device-management
ms.openlocfilehash: e9ecf6d04056a91f1f9dd62a5238f60177d2bf59
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66420594"
---
# <a name="what-is-a-primary-refresh-token"></a>¿Qué es un Token de actualización principal?

Un principal Refresh Token (PRT) es un artefacto de la clave de autenticación de Azure AD en dispositivos Android, iOS y Windows 10. Es un JSON Web Token (JWT) emitidos especialmente para Microsoft primera entidad token los agentes para habilitar el inicio de sesión único (SSO) a través de las aplicaciones usadas en esos dispositivos. En este artículo, nos ofrece detalles sobre cómo un PRT es emitido, usan y protegido en dispositivos Windows 10.

En este artículo se da por supuesto que ya conocen los Estados de dispositivo diferentes disponibles en Azure AD y cómo único inicio de sesión funciona en Windows 10. Para obtener más información acerca de los dispositivos de Azure AD, consulte el artículo [¿qué es la administración de dispositivos en Azure Active Directory?](overview.md)

## <a name="key-terminology-and-components"></a>Los componentes y la terminología clave

Los siguientes componentes de Windows desempeñan un papel clave en la solicitud y el uso de un PRT:

* **Proveedor de autenticación en la nube** (CloudAP): CloudAP es el proveedor de autenticación moderna para la sesión de Windows, que comprueba que los usuarios conectados a un dispositivo Windows 10. CloudAP proporciona un marco de complemento esa identidad pueden crear proveedores en para habilitar la autenticación de Windows con credenciales de dicho proveedor de identidades.
* **Administrador de cuentas Web** (WAM): WAM es el agente de token predeterminado en dispositivos Windows 10. WAM también proporciona un marco de complemento esa identidad proveedores pueden ampliar y habilitar SSO para sus aplicaciones en dicho proveedor de identidades de usuario de confianza.
* **Complemento de Azure AD CloudAP**: Un complemento específico de Azure AD basado en el marco de trabajo CloudAP, que comprueba las credenciales de usuario con Azure AD durante el inicio de sesión de Windows.
* **Complemento de Azure AD WAM**: Un complemento específico de Azure AD basado en el marco WAM, que permite el inicio de sesión único a las aplicaciones que se basan en Azure AD para la autenticación.
* **Dsreg**: Un componente específico de Azure AD en Windows 10, que controla el proceso de registro de dispositivo para todos los Estados de dispositivo.
* **Módulo de plataforma segura** (TPM): Un TPM es un componente de hardware integrado en un dispositivo, que proporciona funciones de seguridad basada en hardware para los secretos de usuario y dispositivo. Se pueden encontrar más detalles en el artículo [Trusted Platform Module Technology Overview](https://docs.microsoft.com/windows/security/information-protection/tpm/trusted-platform-module-overview).

## <a name="what-does-the-prt-contain"></a>¿Qué contiene el PRT?

Un PRT contiene notificaciones normalmente contenidas en cualquier token de actualización de Azure AD. Además, hay algunas notificaciones específicas del dispositivo incluidos en el PRT. Los pasos son los siguientes:

* **Id. de dispositivo**: Se emite un PRT para un usuario en un dispositivo específico. La notificación de identificador de dispositivo `deviceID` determina el dispositivo se ha emitido el PRT al usuario en. Más adelante se emite esta notificación a los tokens obtenidos mediante el PRT. La notificación de identificador de dispositivo se usa para determinar la autorización de acceso condicional basado en el estado del dispositivo o de cumplimiento.
* **Clave de sesión**: La clave de sesión es una clave simétrica cifrada, generado por el servicio de autenticación de Azure AD emitido como parte de la PRT. La clave de sesión actúa como la prueba de posesión cuando se usa un PRT para obtener tokens para otras aplicaciones.

### <a name="can-i-see-whats-in-a-prt"></a>¿Puedo ver lo que está en un PRT?

Un PRT es un objeto binario opaco que se envían desde Azure AD cuyo contenido no se sabe que los componentes de cliente. No puede ver lo que está dentro de un PRT.

## <a name="how-is-a-prt-issued"></a>¿Cómo se emite un PRT?

Registro de dispositivos es un requisito previo para la autenticación de dispositivo en función de Azure AD. Se emite un PRT a los usuarios solo en los dispositivos registrados. Para información más detallada en el registro de dispositivos, consulte el artículo [Windows Hello para empresas y el registro de dispositivos](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-how-it-works-device-registration). Durante el registro de dispositivos, el componente dsreg genera dos conjuntos de pares de claves criptográficas:

* Clave de dispositivo (dkpub/dkpriv)
* Clave de transporte (tkpub/tkpriv)

Las claves privadas se enlazan a TPM del dispositivo si el dispositivo tiene un TPM válido y en funcionamiento, mientras que las claves públicas se envían a Azure AD durante el proceso de registro del dispositivo. Estas claves se utilizan para validar el estado del dispositivo durante las solicitudes PRT.

Se emite el PRT durante la autenticación de usuario en un dispositivo Windows 10 en dos escenarios:

* **Unidos a Azure AD** o **Azure AD híbrido unido**: Cuando un usuario inicia sesión con sus credenciales de la organización, se emite un PRT durante el inicio de sesión de Windows. Se emite un PRT con todas las credenciales de Windows 10 admite, por ejemplo, contraseña y Windows Hello para empresas. En este escenario, el complemento de Azure AD CloudAP es la autoridad principal para el PRT.
* **Dispositivos registrados en Azure AD**: Un PRT se emite cuando un usuario agrega una cuenta de trabajo secundario a su dispositivo Windows 10. Los usuarios pueden agregar una cuenta a Windows 10 de dos maneras diferentes:  
   * Agregar una cuenta a través de la **usar esta cuenta en todas partes de este dispositivo** símbolo del sistema después de iniciar sesión una aplicación (por ejemplo, Outlook)
   * Agregar una cuenta de **configuración** > **cuentas** > **acceso profesional o educativo** > **Connect**

En estos casos, el complemento Administrador de aplicaciones Web de Azure AD es la autoridad principal para el PRT puesto que no se produce el inicio de sesión de Windows con esta cuenta de Azure AD.

## <a name="what-is-the-lifetime-of-a-prt"></a>¿Qué es la duración de un PRT?

Una vez emitido, un PRT es válido durante 14 días y se renueva continuamente siempre y cuando el usuario usa activamente el dispositivo.  

## <a name="how-is-a-prt-used"></a>¿Cómo se utiliza un PRT?

Se usa un PRT por dos componentes clave en Windows:

* **Complemento de Azure AD CloudAP**: Durante el inicio de sesión de Windows en, el complemento de Azure AD CloudAP solicita un PRT desde Azure AD con las credenciales proporcionadas por el usuario. También almacena en caché el PRT para habilitar el inicio de sesión almacenado en caché cuando el usuario no tiene acceso a una conexión a internet.
* **Complemento de Azure AD WAM**: Cuando los usuarios intentan tener acceso a aplicaciones, usa en el complemento de Azure AD WAM el PRT para habilitar el inicio de sesión único en Windows 10. Complemento de Azure AD WAM usa el PRT para solicitar tokens de acceso y actualización para las aplicaciones que dependen de WAM las solicitudes de tokens. También permite inicio de sesión único en exploradores insertando el PRT en las solicitudes del explorador. Explorador de inicio de sesión único en Windows 10 es compatible con Microsoft Edge (de forma nativa) y Chrome (a través de la extensión de las cuentas de Windows 10 o en Office Online).

## <a name="how-is-a-prt-renewed"></a>¿Cómo se renueva un PRT?

Se renueva un PRT en dos métodos diferentes:

* **Complemento de Azure AD CloudAP cada 4 horas**: El complemento CloudAP renueva el PRT cada 4 horas durante el inicio de sesión de Windows. Si el usuario no tiene conexión a internet durante ese tiempo, complemento CloudAP renovará la PRT después de que el dispositivo está conectado a internet.
* **Complemento de Azure AD WAM durante las solicitudes de tokens de aplicación**: El complemento WAM permite inicio de sesión único en dispositivos Windows 10 mediante la habilitación de solicitudes de token silenciosas para las aplicaciones. El complemento de WAM puede renovar el PRT durante estas solicitudes de token de dos maneras diferentes:
   * Una aplicación solicita WAM para un token de acceso en modo silencioso, pero no hay ningún token de actualización disponible para esa aplicación. En este caso, WAM usa el PRT para solicitar un token para la aplicación y recibe un nuevo PRT en la respuesta.
   * Una aplicación solicita WAM para un token de acceso no es válido el PRT o Azure AD necesita autorización adicional (por ejemplo, Azure Multi-factor Authentication). En este escenario, WAM inicia una sesión interactiva que pedir al usuario que se vuelve a autenticar o proporcionar comprobación adicional y se emite un nuevo PRT en la autenticación correcta.

### <a name="key-considerations"></a>Consideraciones clave

* Un PRT solo se emite y se renueva durante la autenticación de la aplicación nativa. Un PRT no se renueva o emitido durante una sesión del explorador.
* En Azure AD Unido y los dispositivos Unidos a Azure AD híbrido, el complemento CloudAP es la autoridad principal para un PRT. Si se renueva un PRT durante una solicitud de token WAM, se envía el PRT al complemento de CloudAP, que comprueba la validez de la PRT con Azure AD antes de aceptarla.

## <a name="how-is-the-prt-protected"></a>¿Cómo se protege la PRT?

Un PRT está protegida por el dispositivo del usuario que ha iniciado sesión en el enlace. Azure AD y Windows 10 habilitar la protección de PRT a través de los métodos siguientes:

* **Durante el primer inicio de sesión**: Durante el primer inicio de sesión, se emite un PRT firmando solicitudes utilizando la clave del dispositivo generada criptográficamente durante el registro del dispositivo. En un dispositivo con un TPM válido y en funcionamiento, la clave del dispositivo está protegida por el TPM impide cualquier acceso malintencionado. No se emite un PRT si no se puede validar la firma de clave de dispositivo correspondiente.
* **Durante las solicitudes de token y renovación**: Cuando se emite un PRT, Azure AD también emite una clave de sesión cifrada al dispositivo. Se cifra con la clave pública de transporte (tkpub) generados y enviados a Azure AD como parte del registro de dispositivos. Esta clave de sesión solo se puede descifrar la clave privada de transporte (tkpriv) protegida por el TPM. La clave de sesión es la clave de prueba de posesión (POP) para las solicitudes enviadas a Azure AD.  La clave de sesión también está protegida por el TPM y ningún otro componente del sistema operativo puede acceder a él. Las solicitudes de token o las solicitudes de renovación PRT segura están firmadas por esta clave de sesión a través de TPM y por lo tanto, no se podrá cambiar. Azure AD, invalidará las solicitudes desde el dispositivo que no están firmadas por la clave de sesión correspondiente.

Al proteger estas claves con el TPM, actores malintencionados no pueden robar las claves ni reproducir el PRT en otro lugar como el TPM es inaccesible, incluso si un atacante tiene el dispositivo físicamente.  Por lo tanto, uso de un TPM en gran medida mejora la seguridad de unido a Azure AD, híbrido unido Azure AD, y los dispositivos contra el robo de credenciales registrados en Azure AD. Rendimiento y confiabilidad, TPM 2.0 es la versión recomendada para todos los escenarios de registro de dispositivos de Azure AD en Windows 10.

### <a name="how-are-app-tokens-and-browser-cookies-protected"></a>¿Cómo son tokens de aplicación y las cookies del explorador protegidas?

**Tokens de aplicación**: Cuando una aplicación solicita un token a través de WAM, Azure AD emite un token de actualización y un token de acceso. Sin embargo, WAM solo devuelve el token de acceso a la aplicación y protege el token de actualización en su caché cifrándolo con la clave del usuario data protection aplicación programación interfaz (DPAPI). WAM segura usa el token de actualización por solicitudes de firma con la clave de sesión más emitir tokens de acceso. La clave DPAPI está protegida mediante una clave simétrica de Azure AD en función de Azure AD. Cuando el dispositivo debe descifrar el perfil de usuario con la clave DPAPI, Azure AD proporciona la clave DPAPI cifrada la clave de sesión, qué complemento CloudAP solicita TPM para descifrar. Esta funcionalidad garantiza la coherencia en la protección de tokens de actualización y evita la implementación de sus propios mecanismos de protección de aplicaciones.  

**Las cookies del explorador**: En Windows 10, Azure AD admite el inicio de sesión único en Internet Explorer y Microsoft Edge browser en Google Chrome o de forma nativa a través de la extensión de las cuentas de Windows 10. La seguridad se ha creado no solo para proteger las cookies, sino también los puntos de conexión al que se envían las cookies. Las cookies del explorador están protegidas de la misma manera es un PRT, mediante el uso de la clave de sesión para iniciar sesión y proteger las cookies.

Cuando un usuario inicia una interacción con el explorador, el explorador (o la extensión) invoca un host de cliente nativo de COM. El host del cliente nativo garantiza que la página procede de uno de los dominios permitidos. El explorador podría enviar que otros parámetros para el cliente nativo de host, incluido un nonce, sin embargo, el host del cliente nativo garantiza la validación del nombre de host. El host del cliente nativo solicita una cookie PRT de complemento de CloudAP, que crea y lo firma con la clave de sesión protegida por TPM. Como la cookie PRT está firmado por la clave de sesión, no se puedan alterar. Esta cookie de PRT se incluye en el encabezado de solicitud para Azure AD validar el dispositivo que es originaria. Si usa el explorador Chrome, solo la extensión que se definen explícitamente en el manifiesto del host de cliente nativo puede invocarla impide que se realizan estas solicitudes extensiones arbitrarias. Una vez que Azure AD valida la cookie PRT, emite una cookie de sesión en el explorador. Esta cookie de sesión también contiene la misma clave de sesión emitida con un PRT. Durante las solicitudes posteriores, la clave de sesión se valida la cookie de enlace en el dispositivo de forma eficaz y evitar las reproducciones desde cualquier parte.

## <a name="when-does-a-prt-get-an-mfa-claim"></a>¿Cuando un PRT Obtiene una notificación de MFA?

Un PRT puede obtener una notificación de autenticación multifactor (MFA) en escenarios concretos. Cuando se usa un PRT basado en MFA para solicitar tokens para las aplicaciones, la notificación de MFA se transfiere a los tokens de aplicación. Esta funcionalidad proporciona una experiencia sin problemas a los usuarios evitando el desafío MFA para todas las aplicaciones que lo requiera. Un PRT puede obtener una notificación de MFA de las maneras siguientes:

* **Inicie sesión con Windows Hello para empresas**: Windows Hello para empresas reemplaza las contraseñas y claves criptográficas se utiliza para proporcionar autenticación segura en dos fases. Windows Hello para empresas es un usuario en un dispositivo específico y, sí requiere MFA para aprovisionar. Cuando un usuario inicia sesión con Windows Hello para empresas, PRT del usuario obtiene una notificación de MFA. Este escenario también se aplica a los usuarios que inician sesión con tarjetas inteligentes si la autenticación de tarjeta inteligente genera una notificación de MFA de ADFS.
* **MFA durante el inicio de sesión interactivo WAM**: Durante una solicitud de token a través de WAM, si un usuario se requiere para realizar la MFA para tener acceso a la aplicación, el PRT que se renueva durante esta interacción es impreso con una notificación de MFA.
* **MFA durante el registro de dispositivo**: Si un administrador ha configurado la configuración del dispositivo en Azure AD para [exija MFA para registrar dispositivos](device-management-azure-portal.md#configure-device-settings), el usuario necesita para realizar la MFA para completar el registro. Durante este proceso, el PRT emitido para el usuario tiene la notificación de MFA obtenida durante el registro. Esta funcionalidad solo se aplica al usuario que realizó la operación de combinación, no a otros usuarios que inician sesión en el dispositivo.

Windows 10 mantiene una lista con particiones de prt por cada credencial. Por lo tanto, hay un PRT para cada uno de Windows Hello para la empresa, contraseña o tarjeta inteligente. Este particionamiento asegura que las notificaciones MFA están aisladas en función de la credencial que usa y no se mezclen durante las solicitudes de token.

## <a name="how-is-a-prt-invalidated"></a>¿Cómo se invalida un PRT?

Se invalida un PRT en los escenarios siguientes:

* **Usuario no válido**: Si un usuario se elimina o deshabilita en Azure AD, su PRT se invalida y no se puede usar para obtener tokens para las aplicaciones. Si un usuario ha eliminado o deshabilitado ha iniciado sesión en un dispositivo que antes, el inicio de sesión almacenado en caché registraría en, hasta que CloudAP es consciente de su estado no válido. Una vez que CloudAP determina que el usuario no es válido, bloquea inicios de sesión posteriores. Un usuario no válido se bloquea automáticamente desde el inicio de sesión para los nuevos dispositivos que no tienen sus credenciales almacenadas en caché.
* **Dispositivo no válido**: Si un dispositivo se elimina o deshabilita en Azure AD, el PRT obtenido en el dispositivo se invalida y no se puede usar para obtener tokens para otras aplicaciones. Si un usuario ya ha iniciado sesión en un dispositivo no válido, podrán seguir hacerlo. Pero se invalidan todos los tokens en el dispositivo y el usuario no tiene inicio de sesión único a todos los recursos de dicho dispositivo.
* **Cambio de contraseña**: Después de que un usuario cambie su contraseña, se invalida el PRT obtenido con la contraseña anterior de Azure AD. Los resultados de los usuarios obtengan un nuevo PRT de cambio de contraseña. Esta invalidación puede suceder de dos maneras diferentes:
   * Si el usuario inicia sesión Windows con su nueva contraseña, CloudAP descarta el PRT antiguo y las solicitudes de Azure AD para emitir un nuevo PRT con su nueva contraseña. Si el usuario no tiene una conexión a internet, no se puede validar la nueva contraseña, Windows pueden requerir al usuario que escriba su contraseña anterior.
   * Si un usuario ha iniciado sesión con su contraseña anterior o cambiado su contraseña después de iniciar sesión en Windows, se usa el antiguo PRT para las solicitudes de token WAM. En este escenario, se solicita al usuario autenticarse de nuevo durante la solicitud de token de WAM y se emite un nuevo PRT.
* **Problemas de TPM**: A veces, TPM de un dispositivo puede error o se producirá un error, dando lugar a la inaccesibilidad de claves protegidas por el TPM. En este caso, el dispositivo no es capaz de obtener un PRT o solicitar tokens mediante una PRT existente como que no puede demostrar la posesión de las claves criptográficas. Como resultado, se invalida cualquier PRT existente de Azure AD. Cuando Windows 10 se detecta un error, inicia un flujo de recuperación para volver a registrar el dispositivo con nuevas claves criptográficas. Con la combinación híbrida de Azure Ad, al igual que el registro inicial, la recuperación se produce en modo silencioso sin intervención del usuario. Para los dispositivos registrados en Azure unido a AD o Azure AD, la recuperación debe llevarse a cabo por un usuario que tenga privilegios de administrador en el dispositivo. En este escenario, se inicia el flujo de recuperación mediante un símbolo del sistema de Windows que guía al usuario para recuperar correctamente el dispositivo.

## <a name="detailed-flows"></a>Flujos detallados

Los siguientes diagramas ilustran los detalles subyacentes en emitir, renovar y utilizar un PRT para solicitar un token de acceso para una aplicación. Además, estos pasos también describen cómo se aplican los mecanismos de seguridad mencionados anteriormente durante estas interacciones.

### <a name="prt-issuance-during-first-sign-in"></a>Emisión de PRT durante el primer inicio de sesión

![Emisión de PRT durante el primer inicio de sesión en el flujo detallado](./media/concept-primary-refresh-token/prt-initial-sign-in.png)

> [!NOTE]
> En Azure AD de dispositivos Unidos a, este cambio se produce sincrónicamente para emitir un PRT antes de que el usuario puede iniciar sesión en Windows. En entornos híbridos Unidos a Azure AD los dispositivos, en el entorno local de Active Directory es la autoridad principal. Por lo tanto, el usuario sólo espera hasta que puedan adquirir un TGT para el inicio de sesión, mientras que la emisión de PRT ocurre de manera asincrónica. Este escenario no se aplica a los dispositivos registrados en Azure AD como el inicio de sesión no usa credenciales de Azure AD.

| Paso | DESCRIPCIÓN |
| :---: | --- |
| Una | Usuario escribe su contraseña en la interfaz de usuario de inicio de sesión. LogonUI pasa las credenciales en un búfer de autenticación para LSA, lo pasa internamente a CloudAP. CloudAP reenvía esta solicitud para el complemento CloudAP. |
| b | Complemento CloudAP inicia una solicitud de detección del dominio para identificar el proveedor de identidades para el usuario. Si el inquilino del usuario tiene un programa de instalación del proveedor de federación, Azure AD devuelve el intercambio de metadatos del proveedor de federación punto de conexión de punto de conexión (MEX). Si no, Azure AD devuelve que el usuario está administrado que indica que el usuario puede autenticarse con Azure AD. |
| C | Si el usuario está administrado, CloudAP obtendrá el valor de seguridad de Azure AD. Si el usuario está federado, complemento CloudAP solicita un token SAML del proveedor de federación con las credenciales del usuario. Una vez que recibe el token SAML, solicita un valor nonce de Azure AD. |
| D | Complemento CloudAP construye la solicitud de autenticación con las credenciales del usuario, valor nonce y un ámbito de agente, firma la solicitud con la clave del dispositivo (dkpriv) y lo envía a Azure AD. En un entorno federado, CloudAP complemento usa el token SAML devuelto por el proveedor de federación en lugar del usuario ' credenciales. |
| E | Azure AD valida las credenciales de usuario, el valor nonce, y firma del dispositivo, comprueba que el dispositivo es válido en el inquilino y emite el PRT cifrado. Junto con el PRT, Azure AD también emite una clave simétrica, llama a la clave de sesión cifrada en Azure AD utilizando la clave de transporte (tkpub). Además, la clave de sesión también se inserta en la PRT. Esta clave de sesión actúa como la clave de prueba de posesión (PoP) para las solicitudes posteriores con el PRT. |
| F | Complemento CloudAP pasa la clave de sesión y PRT cifrada a CloudAP. CloudAP solicitar el TPM para descifrar la clave de sesión mediante la clave de transporte (tkpriv) y volver a cifrar utilizando la clave de TPM. CloudAP almacena la clave de sesión cifrada en su caché junto con el PRT. |

### <a name="prt-renewal-in-subsequent-logons"></a>Renovación PRT en inicios de sesión posteriores

![Renovación PRT en inicios de sesión posteriores](./media/concept-primary-refresh-token/prt-renewal-subsequent-logons.png)

| Paso | DESCRIPCIÓN |
| :---: | --- |
| Una | Usuario escribe su contraseña en la interfaz de usuario de inicio de sesión. LogonUI pasa las credenciales en un búfer de autenticación para LSA, lo pasa internamente a CloudAP. CloudAP reenvía esta solicitud para el complemento CloudAP. |
| b | Si anteriormente, el usuario ha iniciado sesión al usuario, inicia Windows almacena en caché inicie sesión y valida las credenciales para el usuario inicie sesión. Cada 4 horas, el complemento CloudAP inicia asincrónicamente la renovación PRT. |
| C | Complemento CloudAP inicia una solicitud de detección del dominio para identificar el proveedor de identidades para el usuario. Si el inquilino del usuario tiene un programa de instalación del proveedor de federación, Azure AD devuelve el intercambio de metadatos del proveedor de federación punto de conexión de punto de conexión (MEX). Si no, Azure AD devuelve que el usuario está administrado que indica que el usuario puede autenticarse con Azure AD. |
| D | Si el usuario está federado, complemento CloudAP solicita un token SAML del proveedor de federación con las credenciales del usuario. Una vez que recibe el token SAML, solicita un valor nonce de Azure AD. Si el usuario está administrado, CloudAP obtener directamente el valor de seguridad de Azure AD. |
| E | Complemento CloudAP construye la solicitud de autenticación con las credenciales del usuario, valor nonce y la existente PRT, firma la solicitud con la clave de sesión y lo envía a Azure AD. En un entorno federado, CloudAP complemento usa el token SAML devuelto por el proveedor de federación en lugar del usuario ' credenciales. |
| F | Azure AD valida la firma de clave de sesión, comparándolos con la clave de sesión incrustada en el PRT, valida el valor nonce y comprueba que el dispositivo es válido en el inquilino y emite un nuevo PRT. Como vimos antes, el PRT viene acompañado de nuevo con la clave de sesión cifrada mediante clave de transporte (tkpub). |
| G | Complemento CloudAP pasa la clave de sesión y PRT cifrada a CloudAP. CloudAP solicita el TPM para descifrar la clave de sesión mediante la clave de transporte (tkpriv) y volver a cifrar utilizando la clave de TPM. CloudAP almacena la clave de sesión cifrada en su caché junto con el PRT. |

### <a name="prt-usage-during-app-token-requests"></a>Uso de PRT durante las solicitudes de tokens de aplicación

![Uso de PRT durante las solicitudes de tokens de aplicación](./media/concept-primary-refresh-token/prt-usage-app-token-requests.png)

| Paso | DESCRIPCIÓN |
| :---: | --- |
| Una | Una aplicación (por ejemplo, Outlook, OneNote etc.) inicia una solicitud de token de WAM. WAM, pide a su vez, el complemento de Azure AD WAM para atender la solicitud de token. |
| b | Si un token de actualización de la aplicación ya está disponible, el complemento de Azure AD WAM usa para solicitar un token de acceso. Para proporcionar una prueba de enlace de dispositivos, el complemento WAM firma la solicitud con la clave de sesión. Azure AD valida la clave de sesión y emite un token de acceso y un nuevo token de actualización para la aplicación, se cifra la clave de sesión. Complemento WAM solicita complemento AP de nube para descifrar los tokens, que, a su vez, solicita al TPM descifrar utilizando la clave de sesión, lo que resulta en el complemento WAM obtener los tokens de ambos. A continuación, el complemento WAM proporciona el token de acceso a la aplicación, mientras se vuelve a cifra el token de actualización con DPAPI y lo almacena en su propia memoria caché  |
| C |  Si un token de actualización de la aplicación no está disponible, el complemento de Azure AD WAM usa el PRT para solicitar un token de acceso. Para proporcionar la prueba de posesión, complemento WAM firma la solicitud que contiene el PRT con la clave de sesión. Azure AD valida la firma de clave de sesión, comparándolos con la clave de sesión incrustada en el PRT, comprueba que el dispositivo es válido y emite un token de acceso y un token de actualización de la aplicación. Además, Azure AD puede emitir un nuevo PRT (basado en el ciclo de actualización), todos ellos cifran con la clave de sesión. |
| D | Complemento WAM solicita complemento AP de nube para descifrar los tokens, que, a su vez, solicita al TPM descifrar utilizando la clave de sesión, lo que resulta en el complemento WAM obtener los tokens de ambos. A continuación, el complemento WAM proporciona el token de acceso a la aplicación, mientras se vuelve a cifra el token de actualización con DPAPI y lo almacena en su propia memoria caché. Complemento WAM usará el token de actualización en el futuro para esta aplicación. WAM complemento también envía el nuevo PRT al complemento AP de nube, que valida la PRT con Azure AD antes de actualizarlo en su propia memoria caché. Complemento AP de nube usará el PRT nueva a partir de ahora. |
| E | WAM proporciona el token de acceso recién emitido a WAM, que a su vez, se proporciona a la aplicación que realiza la llamada|

### <a name="browser-sso-using-prt"></a>Explorador SSO mediante PRT

![Explorador SSO mediante PRT](./media/concept-primary-refresh-token/browser-sso-using-prt.png)

| Paso | DESCRIPCIÓN |
| :---: | --- |
| Una | Usuario inicia sesión en Windows con sus credenciales para obtener un PRT. Una vez que el usuario abre el explorador, el explorador (o extensión) carga las direcciones URL desde el registro. |
| b | Cuando un usuario abre una dirección URL de inicio de sesión de Azure AD, el explorador o la extensión valida la dirección URL con los que obtener del registro. Si coinciden, el explorador invoca el host del cliente nativo para obtener un token. |
| C | El host del cliente nativo valida que las direcciones URL pertenecen a los proveedores de identidades de Microsoft (cuenta de Microsoft o Azure AD), extrae un valor nonce enviado desde la dirección URL y realiza una llamada al complemento CloudAP para obtener una cookie PRT. |
| D | El complemento CloudAP creará la cookie PRT, inicie sesión con la clave de sesión de TPM enlazados y enviarlo al host de cliente nativo. Como la cookie está firmada por la clave de sesión, no se puedan alterar. |
| E | El host del cliente nativo devolverá esta cookie PRT al explorador, donde se incluye como parte del encabezado de solicitud denominado x-ms-RefreshTokenCredential y solicitud de tokens de Azure AD. |
| F | Azure AD valida la firma de clave de sesión de la cookie PRT, valida el nonce, comprueba que el dispositivo es válido en el inquilino y emite un token de identificador de la página web y una cookie de sesión cifrada para el explorador. |

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo solucionar problemas relacionados con PRT, vea el artículo [híbridos de solución de problemas Azure Active Directory Unidos a dispositivos Windows 10 y Windows Server 2016](troubleshoot-hybrid-join-windows-current.md).
