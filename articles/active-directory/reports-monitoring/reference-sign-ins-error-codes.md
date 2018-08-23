---
title: Códigos de error de informes de actividad de inicio de sesión en el portal de Azure Active Directory | Microsoft Docs
description: Referencia sobre los códigos de error de los informes de actividad de inicio de sesión.
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 05/31/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 85df34445622a13488df590f5cb53bedb7b6ea93
ms.sourcegitcommit: 1af4bceb45a0b4edcdb1079fc279f9f2f448140b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "42142580"
---
# <a name="sign-in-activity-report-error-codes-in-the-azure-active-directory-portal"></a>Códigos de error de informes de actividad de inicio de sesión en el portal de Azure Active Directory

Con la información proporcionada por el informe de inicio de sesión del usuario, puede encontrar respuestas a preguntas tales como:

- ¿Quién ha iniciado sesión mediante Azure Active Directory?
- ¿En qué aplicaciones se inició sesión?
- ¿En qué inicios de sesión se produjeron errores y cuál fue la causa?

Este artículo muestra los códigos de error y las descripciones relacionadas. 

## <a name="how-can-i-display-failed-sign-ins"></a>¿Cómo puedo mostrar los inicios de sesión en los que se produjeron errores? 

El primer punto de entrada a todos los datos de actividades de inicio de sesión es **[Inicios de sesión](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/SignIns)** en la sección **Actividad** de **Azure Active**.


![Actividad de inicio de sesión](./media/reference-sign-ins-error-codes/61.png "Actividad de inicio de sesión")

En los informes de inicios de sesión, puede mostrar todos los inicios de sesión en los que se produjeron errores seleccionando **Error** como **Estado de inicio de sesión**.

![Actividad de inicio de sesión](./media/reference-sign-ins-error-codes/06.png "Actividad de inicio de sesión")

Si hace clic en un elemento de la lista mostrada, se abre la hoja **Detalles de la actividad: Inicios de sesión**. Esta vista le proporciona todos los detalles sobre los inicios de sesión de los que Azure Active Directory realiza el seguimiento incluido el **código de error del inicio de sesión** y un **motivo de error**.

![Actividad de inicio de sesión](./media/reference-sign-ins-error-codes/05.png "Actividad de inicio de sesión")


Como alternativa al uso de Azure Portal para acceder a los datos de inicios de sesión, también puede utilizar la [API de informes](concept-reporting-api.md).


En la siguiente sección se proporciona una información general completa de todos los posibles errores y las descripciones relacionadas. 

## <a name="error-codes"></a>Códigos de error


|Error|DESCRIPCIÓN|
|---|---|
|16000|Se trata de un detalle de implementación interno y no de una condición de error. Puede omitir esta referencia sin problemas.|
|20001|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|20012|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|20033|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|40008|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|40009|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|40014|Hay un problema con el proveedor de identidades federado. Póngase en contacto con el IDP para resolver este problema.|
|50000|Hay un problema con nuestro servicio de inicio de sesión. Abra un [vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para resolver este problema.|
|50001|No se ha encontrado el nombre de entidad de seguridad de servicio en este inquilino. Esto puede pasar si el administrador del inquilino no es el que ha instalado la aplicación. O bien no se encontró la entidad de servicio del recurso en el directorio o no es válida.|
|50002|No se pudo iniciar sesión debido a un acceso de proxy restringido en el inquilino. Si se trata de su propia directiva de inquilino, puede cambiar la configuración restringida del inquilino para corregir este problema|
|50003|No se pudo iniciar sesión debido a que falta la clave de firma o el certificado. Esto se puede deber a que no había ninguna clave de firma configurada en la aplicación. Compruebe las soluciones detalladas en [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#certificate-or-key-not-configured). Si siguen los problemas, póngase en contacto con el propietario de la aplicación o el administrador de esta|
|50005|El usuario intentó iniciar sesión en un dispositivo desde una plataforma que no es compatible a través de una directiva de acceso condicional|
|50006| No se pudo comprobar la firma debido a una firma no válida. Compruebe la solución detallada en [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery). Si siguen los problemas, póngase en contacto con el propietario de la aplicación o el administrador de esta|
|50007|No se encontró el certificado de cifrado del asociado para esta aplicación. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) dirigido a Microsoft para solucionar este problema|
|50008|La aserción SAML falta o está mal configurada en el token. Póngase en contacto con el proveedor de federación.|
|50010|Se ha producido un error en la validación del identificador URI de audiencia de la aplicación debido a que no se configuraron audiencias de token. Póngase en contacto con el propietario de la aplicación|
|50011|La dirección de respuesta falta, está mal configurada o no coincide con las direcciones de respuesta configuradas para la aplicación. Pruebe la solución descrita en [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application). Si siguen los problemas, póngase en contacto con el propietario de la aplicación o el administrador de esta|
|50012| Se trata de un mensaje de error genérico que indica que se produjo un error de autenticación. Esto puede ocurrir por razones como credenciales no válidas o ausentes, o por notificaciones en la solicitud. Asegúrese de que la solicitud se envía con las credenciales y notificaciones correctas. |
|50013|La aserción no es válida debido a varias razones: el emisor del token no coincide con la versión de la API en su intervalo de tiempo válido, la aserción ha expirado o tiene un formato incorrecto, o el token de actualización de la aserción no es un token de actualización principal.|
|50017|No se pudo realizar la validación de la certificación por alguno de los siguientes motivos:<ul><li>No se pudo encontrar el certificado de emisión en la lista de certificados de confianza</li><li>No se pudo encontrar el CrlSegment esperado</li><li>No se pudo encontrar el certificado de emisión en la lista de certificados de confianza</li><li>El punto de distribución de la diferencia CRL se ha configurado sin el punto de distribución de CRL correspondiente</li><li>No se pueden recuperar segmentos CRL válidos debido a un problema de tiempo de espera</li><li>No se puede descargar el CRL</li></ul>Póngase en contacto con el administrador del inquilino.|
|50020|El usuario no está autorizado por uno de los motivos a continuación.<ul><li>El usuario está intentando iniciar sesión con una cuenta MSA con el punto de conexión v1</li><li>El usuario no existe en el inquilino.</li></ul> Póngase en contacto con el propietario de la aplicación.|
|50027|Token JWT no válido debido a los siguientes motivos:<ul><li>no contiene la notificación nonce, sub notificación</li><li>no coincide el identificador de asunto</li><li>notificación duplicada en las notificaciones de idToken</li><li>emisor inesperado</li><li>audiencia inesperada</li><li>fuera de su intervalo de tiempo válido </li><li>el formato del token no es correcto</li><li>El token del identificador externo del emisor no pudo comprobar la firma.</li></ul>Póngase en contacto con el propietario de la aplicación|
|50029|Identificador URI no válido: el nombre de dominio contiene caracteres no válidos. Póngase en contacto con el administrador del inquilino.|
|50034|El usuario no existe en el directorio. Póngase en contacto con el administrador del inquilino.|
|50042|Falta la sal necesaria para generar un identificador en pares en la entidad de seguridad. Póngase en contacto con el administrador del inquilino.|
|50048|El asunto no coincide con la notificación del emisor en la aserción de cliente. Póngase en contacto con el administrador del inquilino.|
|50050|La solicitud tiene un formato incorrecto. Póngase en contacto con el propietario de la aplicación.|
|50053|La cuenta está bloqueada porque el usuario intentó iniciar sesión demasiadas veces con un Id. de usuario o contraseña incorrectos.|
|50055|La contraseña no es válida o ha expirado.|
|50056|Contraseña no válida o nula: la contraseña no existe en el almacén de este usuario|
|50057|La cuenta de usuario está deshabilitada. Un administrador ha deshabilitado la cuenta.|
|50058|La aplicación intentó realizar un inicio de sesión silencioso y el usuario no pudo iniciar sesión de forma silenciosa. La aplicación tiene que iniciar un flujo interactivo que proporcione a los usuarios una opción para iniciar sesión. Póngase en contacto con el propietario de la aplicación.|
|50059|El usuario no existe en el directorio. Póngase en contacto con el administrador del inquilino|
|50061|La solicitud de cierre de sesión no es válida. Póngase en contacto con el propietario de la aplicación|
|50072|El usuario debe inscribirse para el segundo factor de autenticación (interactivo).|
|50074|El usuario no superó el desafío MFA.|
|50076|El usuario no superó el desafío MFA (no interactivo)|
|50079|El usuario debe inscribirse para el segundo factor de autenticación (inicios de sesión no interactivos).|
|50085|El token de actualización necesita un inicio de sesión de IDP social. Haga que el usuario intente iniciar sesión de nuevo con el nombre de usuario y la contraseña|
|50089|El token de flujo ha expirado: error de autenticación. Haga que el usuario intente iniciar sesión de nuevo con el nombre de usuario y la contraseña|
|50097|Se requiere autenticación de dispositivo: las notificaciones DeviceId y DeviceAltSecId son nulas O no existe ningún dispositivo que se corresponda con el identificador|
|50099|La firma JWT no es válida. Póngase en contacto con el propietario de la aplicación.|
|50105|El usuario con sesión iniciada no está asignado a un rol de la aplicación en la que inició sesión. Asigne el usuario a la aplicación. Para más información: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery#user-not-assigned-a-role)|
|50107|El objeto solicitado del dominio Kerberos de federación no existe. Póngase en contacto con el administrador del inquilino.|
|50120|Problema con el encabezado JWT. Póngase en contacto con el administrador del inquilino.|
|50124|La transformación de notificaciones contiene parámetros de entrada no válidos. Póngase en contacto con el administrador del inquilino para actualizar la directiva.|
|50125|El inicio de sesión se interrumpió debido a una entrada de registro de contraseña o restablecimiento de contraseña|
|50126|Nombre de usuario o contraseña no válidos, o nombre de usuario o contraseña locales no válidos.|
|50127|El usuario necesita instalar una aplicación de agente para obtener acceso a este contenido.|
|50128|Nombre de dominio no válido: No se ha encontrado ninguna información de identificación del inquilino en la solicitud ni implícita en ninguna credencial proporcionada.|
|50129|El dispositivo no está unido al área de trabajo: es necesario unirse al área de trabajo para registrar el dispositivo.|
|50130|El valor de notificación no se puede interpretar como método de autenticación conocido|
|50131|Se usa en varios errores de acceso condicional. Por ejemplo, Estado de dispositivo incorrecto de Windows, solicitud bloqueada debido a actividades, directivas de acceso y decisiones sobre directivas de seguridad sospechosas.|
|50132|Las credenciales se han revocado por los siguientes motivos:<ul><li>El artefacto de SSO no es válido o ha expirado</li><li>La sesión no es lo suficientemente nueva para la aplicación</li><li>Se envió una solicitud silenciosa de inicio de sesión, pero la sesión del usuario en Azure AD no es válida o ha expirado.</li></ul>|
|50133|La sesión no es válida debido a que ha expirado o a un cambio reciente de contraseña.|
|50135|Se necesita un cambio de contraseña debido a un riesgo de la cuenta|
|50136|Redirección de la sesión de MSA a una aplicación: se ha detectado una única sesión de MSA |
|50140|Este error se produjo debido a una interrupción en "Mantener la sesión iniciada" cuando el usuario estaba iniciando sesión. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) con el identificador de correlación, el de solicitud y el código de error para conocer más detalles. |
|50143|Error de coincidencia de sesión: la sesión no es válida porque el inquilino de usuario no coincide con la sugerencia de dominio debido a que el recurso es diferente. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) con el identificador de correlación, el de solicitud y el código de error para conocer más detalles.|
|50144|Ha expirado la contraseña de Active Directory del usuario. Genere una nueva contraseña para el usuario o solicite al usuario final que lo haga mediante la herramienta de restablecimiento de autoservicio|
|50146|Es necesario que esta aplicación esté configurada con una clave de firma específica de la aplicación. En este momento no tiene configurada ninguna o ha expirado o ya no es válida. Póngase en contacto con el propietario de la aplicación|
|50148|El code_verifier no coincide con el code_challenge proporcionado en la solicitud de autorización para PKCE. Póngase en contacto con el desarrollador de aplicaciones. |
|50155|Error de autenticación del dispositivo para este usuario|
|50158|No se cumplió el desafío de seguridad externo|
|50161|Las notificaciones enviadas por el proveedor externo no son suficientes o falta la notificación que se solicitó al proveedor externo|
|50166|No se pudo enviar la solicitud al proveedor de notificaciones|
|50169|El dominio Kerberos no es un dominio configurado del espacio de nombres de servicio actual.|
|50172|El proveedor de notificaciones externo no está aprobado. Póngase en contacto con el administrador del inquilino|
|50173|Se necesita un nuevo token de seguridad. Haga que el usuario vuelva a iniciar sesión con credenciales nuevas|
|50177|No se admite el desafío externo para los usuarios con acceso directo|
|50178|No se admite el control de sesión para los usuarios con acceso directo|
|50180|Se necesita la autenticación integrada de Windows. Habilite el inquilino para un inicio de sesión único de conexión directa.|
|51001|La sugerencia de dominio no está presente con el identificador de seguridad local: UPN local|
|51004|La cuenta de usuario no existe en el directorio.|
|51006|Se necesita la autenticación integrada de Windows. El usuario ha iniciado sesión mediante un token de sesión que falta en la notificación. Solicite al usuario que vuelva a iniciar sesión.|
|52004|El usuario no ha proporcionado consentimiento para acceder a los recursos de LinkedIn. |
|53000|La directiva de acceso condicional requiere un dispositivo compatible, y el dispositivo no lo es. Haga que el usuario inscriba su dispositivo con un proveedor de administración de datos maestros aprobado como Intune|
|53001|La directiva de acceso condicional requiere un dispositivo unido a un dominio y este no lo está. Haga que el usuario use un dispositivo unido a un dominio|
|53002|La aplicación utilizada no es una aplicación aprobada para el acceso condicional. El usuario tiene que usar una de las aplicaciones de la lista de aplicaciones aprobadas para poder acceder.|
|53003|Se ha bloqueado el acceso debido a las directivas de acceso condicional.|
|53004|El usuario tiene que completar el proceso de registro de la autenticación multifactor antes de acceder a este contenido. El usuario se debe registrar en la autenticación multifactor.|
|65001|La aplicación X no tiene permiso para acceder a la aplicación Y o este se ha revocado. O bien, el usuario o administrador no ha dado su consentimiento para usar la aplicación con el identificador X. Envíe una solicitud de autorización interactiva para este usuario y recurso. O bien, el usuario o administrador no ha dado su consentimiento para usar la aplicación con el identificador X. Envíe una solicitud de autorización al administrador del inquilino para que actúe en nombre de la aplicación: Y para el recurso: Z.|
|65004|El usuario no dio su consentimiento para acceder a la aplicación. Haga que el usuario intente iniciar sesión de nuevo y dé el consentimiento para acceder a la aplicación|
|65005|La lista de acceso a recursos requeridos de la aplicación no contiene aplicaciones reconocibles por el recurso, o la aplicación cliente ha solicitado el acceso a un recurso que no se especificó en su lista de acceso a recursos requeridos, o el servicio Graph devolvió una solicitud incorrecta o no se encontró el recurso. Si la aplicación es compatible con SAML, es posible que haya configurado la aplicación con el identificador incorrecto (entidad). Pruebe la solución indicada para SAML mediante el siguiente vínculo: [https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav#no-resource-in-requiredresourceaccess-list)|
|70000|Concesión no válida debido a los siguientes motivos:<ul><li>La aserción de SAML 2.0 solicitada tiene un método confirmación de asunto no válido</li><li>No se admite el flujo OnBehalfOf de la aplicación en V2</li><li>El token de actualización principal no está firmado con la clave de sesión</li><li>El token de actualización externo no es válido</li><li>Se obtuvo la concesión de acceso para un inquilino diferente.</li></ul>|
|70001|No se encontró la aplicación denominada X en el inquilino denominado Y. Esto puede pasar si el administrador del inquilino no es el que ha instalado la aplicación con el identificador X o no ha recibido el consentimiento de ningún usuario del inquilino. Puede que haya configurado de forma incorrecta el valor del identificador de la aplicación o que haya enviado la solicitud de autenticación al inquilino incorrecto|
|70002|La aplicación ha devuelto credenciales de cliente no válidas. Póngase en contacto con el propietario de la aplicación|
|70003|La aplicación ha devuelto un tipo de concesión no admitido. Póngase en contacto con el propietario de la aplicación|
|70004|La aplicación ha devuelto un identificador URI de redireccionamiento no válido. La dirección de redireccionamiento que ha especificado el cliente no coincide con ninguna de las direcciones configuradas ni con ninguna de las de la lista de direcciones aprobadas de OIDC. Póngase en contacto con el propietario de la aplicación|
|70005|La aplicación ha devuelto un tipo de respuesta no admitido por alguno de los siguientes motivos:<ul><li>el tipo de respuesta "token" no está habilitado para la aplicación</li><li>el tipo de respuesta "id_token" requiere el ámbito "OpenID", o contiene un valor de parámetro de OAuth no admitido en el wctx codificado</li></ul>Póngase en contacto con el propietario de la aplicación|
|70007|La aplicación ha devuelto un valor no admitido de "response_mode" al solicitar un token. Póngase en contacto con el propietario de la aplicación|
|70008|El código de autorización o el token de actualización proporcionados han expirado o se han revocado. Haga que el usuario vuelva a iniciar sesión|
|70011|El ámbito solicitado por la aplicación no es válido. Póngase en contacto con el propietario de la aplicación|
|70012|Se produjo un error de servidor al autenticar un usuario (consumidor) de MSA. Inténtelo de nuevo. Si el error persiste, [abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) |
|70018|El código de verificación no es válido debido a que el usuario ha escrito un código de usuario incorrecto en el flujo de códigos del dispositivo. No se ha aprobado la autorización|
|70019|El código de verificación ha expirado. Haga que el usuario vuelva a iniciar sesión|
|70037|Se ha proporcionado una respuesta incorrecta al desafío. Se ha rechazado la sesión de autenticación remota.|
|75001|Se ha producido un error durante el enlace de mensajes de SAML.|
|75003|La aplicación ha devuelto un error relacionado con un enlace no admitido (la respuesta del protocolo SAML no se puede enviar a través de enlaces que no sean HTTP POST). Póngase en contacto con el propietario de la aplicación|
|75005|Azure AD no admite la solicitud SAML enviada por la aplicación para el inicio de sesión único. Póngase en contacto con el propietario de la aplicación|
|75008|Se ha rechazado la solicitud de la aplicación ya que la solicitud SAML tenía un destino inesperado. Póngase en contacto con el propietario de la aplicación|
|75011|El método de autenticación mediante el cual se autenticó el usuario en el servicio no coincide con el método de autenticación solicitado. Póngase en contacto con el propietario de la aplicación|
|75016|La solicitud de autenticación de SAML2 tiene un valor de NameIdPolicy no válido. Póngase en contacto con el propietario de la aplicación|
|80001|El agente de autenticación no puede conectarse a Active Directory. Asegúrese de que el agente de autenticación está instalado en una máquina unida a un dominio que tenga una línea de visión a un controlador de dominio que pueda atender la solicitud de inicio de sesión del usuario.|
|80002|Error interno. La solicitud de validación de la contraseña ha agotado el tiempo de espera. No se pudo enviar la solicitud de autenticación al servicio de identidad híbrido interno. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para más información sobre el error|
|80003|El agente de autenticación recibió una respuesta no válida. Se ha producido un error desconocido al intentar realizar la autenticación con Active Directory local. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para más información sobre el error.|
|80005|Agente de autenticación: se ha producido un error desconocido al procesar la respuesta del agente de autenticación. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) para más información sobre el error.|
|80007|El agente de autenticación no puede validar la contraseña del usuario.|
|80010|El agente de autenticación no puede descifrar la contraseña. |
|80011|El agente de autenticación no puede recuperar la clave de cifrado.|
|80012|Los usuarios han intentado iniciar sesión fuera de las horas permitidas (esto se especifica en Active Directory)|
|80013|No se pudo completar el intento de autenticación debido al desfase horario entre la máquina que ejecuta el agente de autenticación y AD. Corrija los problemas de sincronización de horas|
|80014|El agente de autenticación agotó el tiempo de espera. [Abra un vale de soporte](../fundamentals/active-directory-troubleshooting-support-howto.md) con el código de error, el identificador de correlación y datos de fecha y hora para conocer más detalles sobre este error|
|81001|El vale de Kerberos del usuario es demasiado grande. Esto puede ocurrir si el usuario pertenece a demasiados grupos y, por tanto, el vale de Kerberos contiene demasiadas pertenencias a grupos. Reduzca la pertenencia a grupos del usuario e inténtelo de nuevo.|
|81005|No se admite este paquete de autenticación|
|81007|El inquilino no está habilitado para un inicio de sesión único de conexión directa|
|81012|No se trata de una condición de error. Indica que el usuario que intenta iniciar sesión en Azure AD es distinto del que inició sesión en el dispositivo. Puede omitir este código en los registros sin problemas.|
|90010|La solicitud no se admite por diversos motivos. Por ejemplo, la solicitud se ha realizado utilizando un método de solicitud que no es compatible (se admite solo el método POST), o el algoritmo de firma de token que se solicitó no es compatible. Póngase en contacto con el desarrollador de aplicaciones.|
|90014| Falta un campo obligatorio para un mensaje de protocolo, póngase en contacto con el propietario de la aplicación. Si es usted el propietario de la aplicación, asegúrese de que tiene todos los parámetros necesarios para la solicitud de inicio de sesión. |
|90072| La cuenta tiene que agregarse primero como un usuario externo en el inquilino. Cierre sesión y vuelva a iniciarla con otra cuenta de Azure AD.|
|90094| La concesión requiere permisos de administrador. Pida al administrador de inquilino que dé su consentimiento para esta aplicación.|

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte [Introducción a los informes de actividad de inicio de sesión en el portal de Azure Active Directory](concept-sign-ins.md).
