---
title: Códigos de error de informes en el portal de Azure Active Directory | Microsoft Docs
description: Referencia sobre los códigos de error de los informes de actividad de inicio de sesión.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 02/19/2020
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd37374ab6341356d84f205e92c9612d8481818f
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77468854"
---
# <a name="sign-in-activity-report-error-codes"></a>Códigos de error de los informes de actividad de inicio de sesión. 

Con la información que proporciona el [informe de inicios de sesión de usuario](concept-sign-ins.md), puede encontrar respuestas a preguntas tales como:

- ¿Quién inició sesión en mi aplicación?
- ¿En qué aplicaciones se ha iniciado sesión?
- ¿Qué inicios de sesión dieron error y por qué?

Cuando un inicio de sesión produce un error, verá un código de error correspondiente. En este artículo se enumeran los códigos de error y sus descripciones, junto con un curso de acción sugerido, cuando corresponda. 

## <a name="how-can-i-display-failed-sign-ins"></a>¿Cómo puedo mostrar los inicios de sesión en los que se produjeron errores? 

En el menú de [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory** o busque y seleccione **Azure Active Directory** desde cualquier página.

![Seleccionar Azure Active Directory](./media/reference-sign-ins-error-codes/select-azure-active-directory.png "Azure Active Directory")

En **Supervisión**, seleccione **Inicios de sesión** para abrir el [informe de inicios de sesión](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns).

![Actividad de inicio de sesión](./media/reference-sign-ins-error-codes/monitoring-sign-ins-in-azure-active-directory.png "Actividad de inicio de sesión")

Filtre el informe para mostrar todos los inicios de sesión con error; para ello, seleccione **Error** en el cuadro desplegable **Estado de inicio de sesión**.

![Actividad de inicio de sesión](./media/reference-sign-ins-error-codes/06.png "Actividad de inicio de sesión")

Al seleccionar un elemento de la lista filtrada, se abre la hoja **Detalles de actividad: Inicios de sesión**. Esta vista proporciona información adicional sobre el evento de inicio de sesión con error, incluido el **código de error de inicio de sesión** y el **motivo del error**.

![Actividad de inicio de sesión](./media/reference-sign-ins-error-codes/05.png "Actividad de inicio de sesión")

También puede acceder mediante programación a los datos de inicio de sesión con la [API de informes](concept-reporting-api.md).

## <a name="error-codes"></a>Códigos de error


|Error|Descripción|
|---|---|
|16000|Se trata de un detalle de implementación interno y no de una condición de error. Puede omitir esta referencia sin problemas.|
|20001|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|20012|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|20033|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|40008|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|40009|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|40014|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|50000|Hay un problema con nuestro servicio de inicio de sesión. Abra un [vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para resolver este problema.|
|50001|No se ha encontrado el nombre de entidad de seguridad de servicio en este inquilino. Este problema puede ocurrir si el administrador del inquilino no ha instalado la aplicación, o si la entidad de seguridad del recurso no se ha encontrado en el directorio o no es válida.|
|50002|No se pudo iniciar sesión debido a un acceso de proxy restringido en el inquilino. Si se trata de su propia directiva de inquilino, puede cambiar la configuración restringida del inquilino para corregir este problema.|
|50003|No se pudo iniciar sesión debido a que falta la clave de firma o el certificado. Esto se puede deber a que no había ninguna clave de firma configurada en la aplicación. Compruebe las soluciones detalladas en [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured). Si el problema persiste, póngase en contacto con el propietario o el administrador de la aplicación.|
|50005|El usuario intentó iniciar sesión en un dispositivo desde una plataforma que actualmente no es compatible mediante una directiva de acceso condicional.|
|50006| No se pudo comprobar la firma debido a una firma no válida. Compruebe la solución detallada en [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery). Si el problema persiste, póngase en contacto con el propietario o el administrador de la aplicación.|
|50007|No se encontró el certificado de cifrado del asociado para esta aplicación. [Abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md) en Microsoft para solucionar este problema.|
|50008|La aserción SAML falta o está mal configurada en el token. Póngase en contacto con el proveedor de federación.|
|50010|Se ha producido un error en la validación del identificador URI de audiencia de la aplicación debido a que no se configuraron audiencias de token. Póngase en contacto con el propietario de la aplicación para encontrar una solución.|
|50011|La dirección de respuesta falta, está mal configurada o no coincide con las direcciones de respuesta configuradas para la aplicación. Pruebe la solución que se indica en [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application). Si el problema persiste, póngase en contacto con el propietario o el administrador de la aplicación.|
|50012| Se trata de un mensaje de error genérico que indica que se produjo un error de autenticación. Esto puede ocurrir por razones como credenciales no válidas o ausentes, o por notificaciones en la solicitud. Asegúrese de que la solicitud se envía con las credenciales y notificaciones correctas. |
|50013|La aserción no es válida debido a varias razones. Por ejemplo, el emisor del token no coincide con la versión de la API en su intervalo de tiempo válido, el token ha expirado o su formato no es válido o el token de actualización de la aserción no es un token de actualización principal.|
|50017|No se pudo realizar la validación de la certificación por alguno de los siguientes motivos:<ul><li>No se pudo encontrar el certificado de emisión en la lista de certificados de confianza</li><li>No se pudo encontrar el CrlSegment esperado</li><li>No se pudo encontrar el certificado de emisión en la lista de certificados de confianza</li><li>El punto de distribución de la diferencia CRL se ha configurado sin el punto de distribución de CRL correspondiente</li><li>No se pueden recuperar segmentos CRL válidos debido a un problema de tiempo de espera</li><li>No se puede descargar el CRL</li></ul>Póngase en contacto con el administrador del inquilino.|
|50020|El usuario no está autorizado por uno de los motivos a continuación.<ul><li>El usuario está intentando iniciar sesión con una cuenta MSA con el punto de conexión v1</li><li>El usuario no existe en el inquilino.</li></ul> Póngase en contacto con el propietario de la aplicación.|
|50027|Token JWT no válido debido a los siguientes motivos:<ul><li>no contiene la notificación nonce, sub notificación</li><li>no coincide el identificador de asunto</li><li>notificación duplicada en las notificaciones de idToken</li><li>emisor inesperado</li><li>audiencia inesperada</li><li>fuera de su intervalo de tiempo válido </li><li>el formato del token no es correcto</li><li>El token del identificador externo del emisor no pudo comprobar la firma.</li></ul>Póngase en contacto con el propietario de la aplicación|
|50029|Identificador URI no válido: el nombre de dominio contiene caracteres no válidos. Póngase en contacto con el administrador del inquilino.|
|50034|El usuario no existe en el directorio. Póngase en contacto con el administrador del inquilino.|
|50042|En principio, falta la sal necesaria para generar un identificador en pares. Póngase en contacto con el administrador del inquilino.|
|50048|El asunto no coincide con la notificación del emisor en la aserción de cliente. Póngase en contacto con el administrador del inquilino.|
|50050|La solicitud tiene un formato incorrecto. Póngase en contacto con el propietario de la aplicación.|
|50053|La cuenta está bloqueada porque el usuario intentó iniciar sesión demasiadas veces con un identificador de usuario o una contraseña incorrectos.|
|50055|La contraseña no es válida o ha expirado.|
|50056|Contraseña no válida o nula: la contraseña no existe en el almacén de este usuario.|
|50057|La cuenta de usuario está deshabilitada. Un administrador ha deshabilitado la cuenta.|
|50058|La aplicación intentó realizar un inicio de sesión silencioso y el usuario no pudo iniciar sesión de forma silenciosa. La aplicación tiene que iniciar un flujo interactivo que proporcione a los usuarios una opción para iniciar sesión. Póngase en contacto con el propietario de la aplicación.|
|50059|El usuario no existe en el directorio. Póngase en contacto con el administrador del inquilino.|
|50061|La solicitud de cierre de sesión no es válida. Póngase en contacto con el propietario de la aplicación.|
|50072|El usuario debe inscribirse para la autenticación en dos fases (interactivo).|
|50074|El usuario no superó el desafío MFA.|
|50076|El usuario no superó el desafío MFA (no interactivo).|
|50078|La autenticación multifactor presentada ha expirado, debe actualizar la autenticación multifactor para acceder.|
|50079|El usuario debe inscribirse para la autenticación en dos fases (inicios de sesión no interactivos).|
|50085|El token de actualización necesita un inicio de sesión de IDP social. Indique al usuario que intente iniciar sesión de nuevo con su nombre de usuario y contraseña.|
|50089|El token de flujo ha expirado: error de autenticación. Indique al usuario que intente iniciar sesión de nuevo con su nombre de usuario y contraseña.|
|50097|Se requiere autenticación del dispositivo. Este error puede deberse a que las notificaciones DeviceId o DeviceAltSecId son **nulas** o a que no existe ningún dispositivo correspondiente al identificador de dispositivo.|
|50099|La firma JWT no es válida. Póngase en contacto con el propietario de la aplicación.|
|50105|El usuario con sesión iniciada no está asignado a un rol de la aplicación en la que inició sesión. Asigne el usuario a la aplicación. Para más información: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role)|
|50107|El objeto solicitado del dominio Kerberos de federación no existe. Póngase en contacto con el administrador del inquilino.|
|50120|Problema con el encabezado JWT. Póngase en contacto con el administrador del inquilino.|
|50124|La transformación de notificaciones contiene parámetros de entrada no válidos. Póngase en contacto con el administrador del inquilino para actualizar la directiva.|
|50125|El inicio de sesión se interrumpió debido a una entrada de registro de contraseña o restablecimiento de contraseña.|
|50126|Nombre de usuario o contraseña no válidos, o nombre de usuario o contraseña locales no válidos.|
|50127|El usuario debe instalar una aplicación de agente para obtener acceso a este contenido.|
|50128|Nombre de dominio no válido: No se ha encontrado ninguna información de identificación del inquilino en la solicitud ni implícita en ninguna credencial proporcionada.|
|50129|El dispositivo no está unido al área de trabajo: es necesario **unirse al área de trabajo** para registrar el dispositivo.|
|50130|El valor de notificación no se puede interpretar como método de autenticación conocido.|
|50131|Se usa en varios errores de acceso condicional. Por ejemplo, Estado de dispositivo incorrecto de Windows, solicitud bloqueada debido a actividades, directivas de acceso y decisiones sobre directivas de seguridad sospechosas.|
|50132|Las credenciales se han revocado por los siguientes motivos:<ul><li>El artefacto de SSO no es válido o ha expirado</li><li>La sesión no es lo suficientemente nueva para la aplicación</li><li>Se envió una solicitud silenciosa de inicio de sesión, pero la sesión del usuario en Azure AD no es válida o ha expirado.</li></ul>|
|50133|La sesión no es válida debido a que ha expirado o a un cambio reciente de contraseña.|
|50135|Se necesita un cambio de contraseña debido a un riesgo de la cuenta.|
|50136|Redirección de la sesión de MSA a una aplicación: se ha detectado una única sesión de MSA. |
|50140|Este error se produjo debido a una interrupción en "Mantener la sesión iniciada" cuando el usuario estaba iniciando sesión. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) con el identificador de correlación, el de solicitud y el código de error para conocer más detalles. |
|50143|Error de coincidencia de sesión: la sesión no es válida porque el inquilino del usuario no coincide con la sugerencia de dominio debido a un recurso diferente.  [Abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md) con el identificador de correlación, el identificador de solicitud y el código de error para más detalles.|
|50144|Ha expirado la contraseña de Active Directory del usuario. Genere una nueva contraseña para el usuario o solicite al usuario final que lo haga mediante la herramienta de autoservicio de restablecimiento.|
|50146|Es necesario que esta aplicación esté configurada con una clave de firma específica de la aplicación. En este momento no tiene configurada ninguna o ha expirado o ya no es válida. Póngase en contacto con el propietario de la aplicación.|
|50148|El code_verifier no coincide con el code_challenge proporcionado en la solicitud de autorización para PKCE. Póngase en contacto con el desarrollador de aplicaciones. |
|50155|Error de autenticación del dispositivo para este usuario.|
|50158|No se cumplió el desafío de seguridad externo.|
|50161|Las notificaciones enviadas por el proveedor externo no son suficientes o falta la notificación que se solicitó al proveedor externo.|
|50166|No se pudo enviar la solicitud al proveedor de notificaciones.|
|50169|El dominio Kerberos no es un dominio configurado del espacio de nombres de servicio actual.|
|50172|El proveedor de notificaciones externo no está aprobado. Póngase en contacto con el administrador del inquilino.|
|50173|Se necesita un nuevo token de seguridad. Indique el usuario que inicie sesión de nuevo con unas nuevas credenciales.|
|50177|No se admite el desafío externo para los usuarios con acceso directo.|
|50178|No se admite el control de sesión para los usuarios con acceso directo.|
|50180|Se necesita la autenticación integrada de Windows. Habilite el inquilino para un inicio de sesión único de conexión directa.|
|50181|Error relacionado con OTP durante el inicio de sesión. |
|50194|La aplicación '{appId}'({appName}) no está configurada como una aplicación multiinquilino. El uso del punto de conexión /common no se admite para las aplicaciones creadas después de '{time}'. Use un punto de conexión específico del inquilino o configure la aplicación para que sea multiinquilino.|
|50201|La interrupción de este mensaje se mostrará al usuario durante el inicio de sesión cuando se debe proporcionar información adicional al usuario.|
|51001|La sugerencia de dominio no está presente con el identificador de seguridad local: UPN local.|
|51004|La cuenta de usuario no existe en el directorio.|
|51006|Se necesita la autenticación integrada de Windows. El usuario ha iniciado sesión mediante un token de sesión que falta en la notificación. Solicite al usuario que vuelva a iniciar sesión.|
|52004|El usuario no ha proporcionado consentimiento para acceder a los recursos de LinkedIn. |
|53000|La directiva de acceso condicional requiere un dispositivo compatible, y el dispositivo no lo es. Indique al usuario que inscriba su dispositivo con un proveedor de administración de datos maestros aprobado como Intune.|
|53001|La directiva de acceso condicional requiere un dispositivo unido a un dominio y este no lo está. Haga que el usuario utilice un dispositivo unido a un dominio.|
|53002|La aplicación utilizada no está aprobada para el acceso condicional. El usuario tiene que usar una de las aplicaciones de la lista de aplicaciones aprobadas para poder acceder.|
|53003|Se ha bloqueado el acceso debido a las directivas de acceso condicional.|
|53004|El usuario tiene que completar el proceso de registro de la autenticación multifactor antes de acceder a este contenido. El usuario se debe registrar en la autenticación multifactor.|
|53032|La cuenta se ha bloqueado debido a las directivas de Azure AD Identity Protection.|
|65001|La aplicación X no tiene permiso para acceder a la aplicación Y o este se ha revocado. O bien, el usuario o administrador no ha dado su consentimiento para usar la aplicación con el identificador X. Envíe una solicitud de autorización interactiva para este usuario y recurso. O bien, el usuario o administrador no han dado su consentimiento para usar la aplicación con el identificador X. Envíe una solicitud de autorización al administrador del inquilino para que actúe en nombre de la aplicación: Y para el recurso: Z.|
|65004|El usuario no dio su consentimiento para acceder a la aplicación. Haga que el usuario intente iniciar sesión de nuevo y dé el consentimiento para acceder a la aplicación|
|65005|La lista de acceso a recursos requeridos de la aplicación no contiene aplicaciones reconocibles por el recurso, o la aplicación cliente ha solicitado el acceso a un recurso que no se especificó en su lista de acceso a recursos requeridos, o el servicio Graph devolvió una solicitud incorrecta o no se encontró el recurso. Si la aplicación es compatible con SAML, es posible que haya configurado la aplicación con el identificador incorrecto (entidad). Pruebe la solución indicada para SAML mediante el siguiente vínculo: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav)|
|70000|Concesión no válida debido a los siguientes motivos:<ul><li>La aserción de SAML 2.0 solicitada tiene un método confirmación de asunto no válido</li><li>No se admite el flujo OnBehalfOf de la aplicación en V2</li><li>El token de actualización principal no está firmado con la clave de sesión</li><li>El token de actualización externo no es válido</li><li>Se obtuvo la concesión de acceso para un inquilino diferente.</li></ul>|
|70001|No se encontró la aplicación denominada X en el inquilino denominado Y. Esto puede pasar si el administrador del inquilino no es el que ha instalado la aplicación con el identificador X o no ha recibido el consentimiento de ningún usuario del inquilino. Puede que haya configurado de forma incorrecta el valor del identificador de la aplicación o que haya enviado la solicitud de autenticación al inquilino incorrecto.|
|70002|La aplicación ha devuelto credenciales de cliente no válidas. Póngase en contacto con el propietario de la aplicación.|
|70003|La aplicación ha devuelto un tipo de concesión no admitido. Póngase en contacto con el propietario de la aplicación.|
|70004|La aplicación ha devuelto un identificador URI de redireccionamiento no válido. La dirección de redireccionamiento que ha especificado el cliente no coincide con ninguna de las direcciones configuradas ni con ninguna de las de la lista de direcciones aprobadas de OIDC. Póngase en contacto con el propietario de la aplicación.|
|70005|La aplicación ha devuelto un tipo de respuesta no admitido por alguno de los siguientes motivos:<ul><li>el tipo de respuesta "token" no está habilitado para la aplicación</li><li>el tipo de respuesta "id_token" requiere el ámbito "OpenID", o contiene un valor de parámetro de OAuth no admitido en el wctx codificado</li></ul>Póngase en contacto con el propietario de la aplicación.|
|70007|La aplicación ha devuelto un valor no admitido de "response_mode" al solicitar un token. Póngase en contacto con el propietario de la aplicación.|
|70008|El código de autorización o el token de actualización proporcionados han expirado o se han revocado. Indique al usuario que vuelva a iniciar sesión.|
|70011|El ámbito solicitado por la aplicación no es válido. Póngase en contacto con el propietario de la aplicación.|
|70012|Se produjo un error de servidor al autenticar un usuario (consumidor) de MSA. Intente iniciar sesión de nuevo y, si el problema persiste, [abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md) .|
|70018|El código de verificación no es válido debido a que el usuario ha escrito un código de usuario incorrecto en el flujo de códigos del dispositivo. No se ha aprobado la autorización.|
|70019|El código de verificación ha expirado. Pida al usuario que vuelva a iniciar sesión.|
|70037|Se ha proporcionado una respuesta incorrecta al desafío. Se ha rechazado la sesión de autenticación remota.|
|70043|La administración de sesiones de acceso condicional de Azure fuerza la expiración de la sesión.|
|70044|La administración de sesiones de acceso condicional de Azure fuerza la expiración de la sesión.|
|75001|Se ha producido un error durante el enlace de mensajes de SAML.|
|75003|La aplicación ha devuelto un error relacionado con un enlace no admitido (la respuesta del protocolo SAML no se puede enviar a través de enlaces que no sean HTTP POST). Póngase en contacto con el propietario de la aplicación.|
|75005|Azure AD no admite la solicitud SAML enviada por la aplicación para el inicio de sesión único. Póngase en contacto con el propietario de la aplicación.|
|75008|Se ha rechazado la solicitud de la aplicación ya que la solicitud SAML tenía un destino inesperado. Póngase en contacto con el propietario de la aplicación.|
|75011|El método de autenticación mediante el cual se autenticó el usuario en el servicio no coincide con el método de autenticación solicitado. Póngase en contacto con el propietario de la aplicación.|
|75016|La solicitud de autenticación de SAML2 tiene un valor de NameIdPolicy no válido. Póngase en contacto con el propietario de la aplicación.|
|80001|El agente de autenticación no puede conectarse a Active Directory. Asegúrese de que el agente de autenticación está instalado en una máquina unida a un dominio que tenga una línea de visión a un controlador de dominio que pueda atender la solicitud de inicio de sesión del usuario.|
|80002|Error interno. La solicitud de validación de la contraseña ha agotado el tiempo de espera. No se pudo enviar la solicitud de autenticación al servicio de identidad híbrido interno. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para más información sobre el error.|
|80003|El agente de autenticación recibió una respuesta no válida. Se ha producido un error desconocido al intentar realizar la autenticación con Active Directory local. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para más información sobre el error.|
|80005|Agente de autenticación: se ha producido un error desconocido al procesar la respuesta del agente de autenticación. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para más información sobre el error.|
|80007|El agente de autenticación no puede validar la contraseña del usuario.|
|80010|El agente de autenticación no puede descifrar la contraseña. |
|80011|El agente de autenticación no puede recuperar la clave de cifrado.|
|80012|Los usuarios han intentado iniciar sesión fuera de las horas permitidas (esto se especifica en AD).|
|80013|No se pudo completar el intento de autenticación debido al desfase horario entre la máquina que ejecuta el agente de autenticación y AD. Corrija los problemas de sincronización de horas|
|80014|El agente de autenticación agotó el tiempo de espera. [Abra una incidencia de soporte técnico](../fundamentals/active-directory-troubleshooting-support-howto.md) con el código de error, el identificador de correlación y los datos de fecha y hora para conocer más detalles sobre este error.|
|81001|El vale de Kerberos del usuario es demasiado grande. Esto puede ocurrir si el usuario pertenece a demasiados grupos y, por tanto, el vale de Kerberos contiene demasiadas pertenencias a grupos. Reduzca la pertenencia a grupos del usuario e inténtelo de nuevo.|
|81005|No se admite este paquete de autenticación.|
|81007|El inquilino no está habilitado para un inicio de sesión único de conexión directa.|
|81012|No se trata de una condición de error. Indica que el usuario que intenta iniciar sesión en Azure AD es distinto del que inició sesión en el dispositivo. Puede omitir este código en los registros sin problemas.|
|90010|La solicitud no se admite por diversos motivos. Por ejemplo, la solicitud se ha realizado utilizando un método de solicitud que no es compatible (se admite solo el método POST), o el algoritmo de firma de token que se solicitó no es compatible. Póngase en contacto con el desarrollador de aplicaciones.|
|90014| Falta un campo obligatorio para un mensaje de protocolo, póngase en contacto con el propietario de la aplicación. Si es usted el propietario de la aplicación, asegúrese de que tiene todos los parámetros necesarios para la solicitud de inicio de sesión. |
|90051| Token de delegación no válido. Se especifica un identificador de nube nacional ({cloudId}) no válido.|
|90072| La cuenta tiene que agregarse primero como un usuario externo en el inquilino. Cierre sesión y vuelva a iniciarla con otra cuenta de Azure AD.|
|90094| La aplicación ha solicitado permisos a los que el usuario con sesión iniciada no puede dar consentimiento y se bloqueó al usuario. |
|90095| La aplicación ha solicitado permisos a los que el usuario con sesión iniciada no puede dar consentimiento y se mostró al usuario el formulario de [solicitud de consentimiento del administrador](../manage-apps/configure-admin-consent-workflow.md). |
|130500|El inicio de sesión de teléfono se bloqueó debido a la directiva de credencial de usuario.|
|500011| No se encontró la entidad de seguridad de recurso denominada <site address> en el inquilino denominado <tenant ID>. Esto puede pasar si el administrador del inquilino no es el que ha instalado el administrador del inquilino o no ha recibido el consentimiento de ningún usuario del inquilino. Es posible que haya enviado la solicitud de autenticación al inquilino incorrecto.|
|500014|El elemento '{identifier}' del recurso está deshabilitado.|
|500021| El inquilino está restringido por el proxy de la empresa. Se deniega el acceso del recurso.|
|500121| Error de autenticación durante la solicitud de autenticación sólida.|
|500133| La aserción no está dentro de su intervalo de tiempo válido. Asegúrese de que el token de acceso no ha expirado antes de usarlo para la aserción de usuario o solicite un nuevo token.|
|500172|El elemento '{name}' del certificado emitido por '{issuer}' no es válido. Hora actual: '{curTime}'. Certificado NotBefore: '{startTime}'. Certificado NotAfter: '{endTime}'.|
|501291|La aplicación cliente es una aplicación MAM, el dispositivo no está registrado y la solicitud se envía mediante un agente. Se debe realizar la unión al área de trabajo para registrar el dispositivo antes de que se pueda tener acceso a la aplicación.|
|530003|Se requiere que su dispositivo sea administrado para acceder a este recurso.|
|530021|La aplicación no cumple los requisitos de aplicación aprobada para el acceso condicional.|
|530032|Bloqueado por una directiva de seguridad.| 
|700016|La aplicación con identificador "{appIdentifier}" no se encontró en el directorio "{tenantName}". Esto puede pasar si el administrador del inquilino no es el que ha instalado el administrador del inquilino o no ha recibido el consentimiento de ningún usuario del inquilino. Es posible que haya enviado la solicitud de autenticación al inquilino incorrecto.|
|700051|El elemento 'token' de response_type no está habilitado para la aplicación. La aplicación solicitó un tipo de respuesta no compatible debido a los siguientes motivos: el elemento 'token' de response_type no está habilitado para la aplicación. El propietario de la aplicación debe ir a Azure Portal o llamar a MS Graph para habilitar la concesión implícita del token de acceso.|
|900432|El cliente confidencial no se admite en una solicitud entre nubes.|
|5000811|No se puede comprobar la firma del token SAML. El identificador de la clave de firma no coincide con ninguna clave registrada válida.|
|7000215|Se ha proporcionado un secreto de cliente no válido.|
|7000218|El cuerpo de la solicitud debe contener el siguiente parámetro: "client_assertion" o "client_secret".|


## <a name="next-steps"></a>Pasos siguientes

* [Información general sobre los informes de inicios de sesión](concept-sign-ins.md)
* [Acceso mediante programación a los informes de Azure AD](concept-reporting-api.md)
