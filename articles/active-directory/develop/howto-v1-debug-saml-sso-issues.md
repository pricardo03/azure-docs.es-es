---
title: 'Depuración del inicio de sesión único basado en SAML: Azure Active Directory | Microsoft Docs'
description: Depure el inicio de sesión único basado en SAML en aplicaciones de Azure Active Directory.
services: active-directory
author: CelesteDG
documentationcenter: na
manager: mtillman
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/15/2018
ms.author: celested
ms.custom: aaddev
ms.reviewer: hirsin, dastrock, smalser
ms.openlocfilehash: 4d536e530c1f119a33c4583a413143808a43d5e5
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55092006"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Depuración del inicio de sesión único basado en SAML en aplicaciones de Azure Active Directory

Obtenga información acerca de cómo buscar y corregir problemas del [inicio de sesión único](../manage-apps/what-is-single-sign-on.md) en aplicaciones de Azure Active Directory (Azure AD) compatibles con el [Lenguaje de marcado de aserción de seguridad (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language). 

## <a name="before-you-begin"></a>Antes de empezar
Se recomienda instalar la [Extensión de inicio de sesión seguro de mis aplicaciones](../user-help/active-directory-saas-access-panel-user-help.md#i-am-having-trouble-installing-the-my-apps-secure-sign-in-extension). Esta extensión de explorador facilita la reunión de la información de la solicitud SAML y de la respuesta SAML que necesita para resolver problemas con el inicio de sesión único. En caso de que no pueda instalar la extensión, este artículo muestra cómo resolver problemas con y sin la extensión instalada.

Para descargar e instalar la extensión de inicio de sesión seguro de mis aplicaciones, utilice uno de los siguientes vínculos.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)


## <a name="test-saml-based-single-sign-on"></a>Prueba del inicio de sesión único basado en SAML

Para probar el inicio de sesión único basado en SAML entre AAD y una aplicación de destino:

1.  Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador global u otro administrador que esté autorizado para administrar aplicaciones.
2.  En la hoja izquierda, haga clic en **Azure Active Directory** y, a continuación, haga clic en **Aplicaciones empresariales**. 
3.  En la lista de aplicaciones empresariales, haga clic en la aplicación para la que desea probar el inicio de sesión único y, a continuación, en las opciones de la izquierda haga clic en **Inicio de sesión único**.
4.  Para abrir la experiencia de prueba de inicio de sesión único basado en SAML, en la sección **Dominio y direcciones URL**, haga clic en **Probar la configuración de SAML**. Si el botón Probar la configuración de SAML aparece atenuado, en primer lugar necesitará rellenar y guardar los atributos necesarios.
5.  En la hoja **Probar el inicio de sesión único**, use sus credenciales corporativas para iniciar sesión en la aplicación de destino. Puede iniciar sesión como el usuario actual o como otro usuario. Si inicia sesión como otro usuario, un mensaje le pedirá que se autentique.

    ![Prueba de página SAML](./media/howto-v1-debug-saml-sso-issues/testing.png)


Si ha iniciado sesión correctamente, ha superado la prueba. En este caso, Azure AD ha emitido un token de respuesta de SAML a la aplicación. La aplicación ha usado el token de SAML para su inicio de sesión correcto.

Si tiene un error en la página de inicio de sesión de la compañía o la página de la aplicación, use una de las secciones siguientes para resolver el error.


## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Resolver un error de inicio de sesión en la página de inicio de sesión de compañía

Al intentar iniciar sesión puede ver un error en la página de inicio de sesión de la compañía. 

![Error de inicio de sesión](./media/howto-v1-debug-saml-sso-issues/error.png)

Para depurar este error, se necesita el mensaje de error y la solicitud SAML. La Extensión de inicio de sesión seguro de mis aplicaciones recopila automáticamente esta información y muestra una guía para la resolución en Azure AD. 

Para resolver el error de inicio de sesión con la extensión de inicio de sesión seguro de mis aplicaciones instalada:

1.  Cuando se produce un error, la extensión le redirige a la hoja **Probar el inicio de sesión único** de Azure AD. 
2.  En la hoja **Probar el inicio de sesión único**, haga clic en **Download the SAML request** (Descargar la solicitud SAML). 
3.  Debería ver instrucciones específicas para la resolución en función del error y los valores de la solicitud SAML. Revise las instrucciones.

Para resolver el error sin la instalación de la Extensión de inicio de sesión seguro de mis aplicaciones:

1. Copie el mensaje de error de la esquina inferior derecha de la página. El mensaje de error incluye:
    - Un elemento CorrelationID y una marca de tiempo. Estos valores son importantes cuando se crea un caso de soporte técnico con Microsoft ya que ayudan a los ingenieros a identificar el problema y a proporcionar una solución más precisa y rápida para el problema.
    - Una instrucción que identifica la causa principal del problema.
2.  Vuelva a Azure AD y busque la hoja **Probar el inicio de sesión único**.
3.  En el cuadro de texto situado encima de **Get resolution guidance** (Obtener instrucciones para la resolución), pegue el mensaje de error.
3.  Haga clic en **Get resolution guidance** (Obtener instrucciones para la resolución) para mostrar los pasos para resolver el problema. Las instrucciones pueden requerir información de la solicitud SAML o la respuesta de SAML. Si no usa la Extensión de inicio de sesión seguro de mis aplicaciones, puede que necesite una herramienta como [Fiddler](https://www.telerik.com/fiddler) para recuperar la solicitud y la respuesta de SAML.
4.  Compruebe que el destino de la solicitud SAML se corresponde con la dirección URL del servicio de inicio de sesión único de SAML obtenido en Azure Active Directory.
5.  Compruebe que el emisor de la solicitud SAML es el mismo identificador que ha configurado para la aplicación en Azure Active Directory. Azure AD usa al emisor para buscar una aplicación en el directorio.
6.  Compruebe que AssertionConsumerServiceURL es la dirección en la que la aplicación espera recibir el token SAML de Azure Active Directory. Puede configurar este valor en Azure Active Directory, pero no es obligatorio si forma parte de la solicitud SAML.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Resolución de un error de inicio de sesión único en la página de la aplicación

Puede iniciar sesión correctamente y luego ver un error en la página de la aplicación. Esto se produce cuando Azure AD ha emitido un token a la aplicación, pero la aplicación no acepta la respuesta.   

Para resolver el error:

1. Si la aplicación se encuentra en la galería de Azure AD, compruebe que ha seguido todos los pasos para la integración de la aplicación con Azure AD. Para buscar las instrucciones de integración de la aplicación, consulte la [lista de tutoriales sobre la integración de aplicaciones de SaaS](../saas-apps/tutorial-list.md).
2. Recupere la respuesta SAML.
    - Si se ha instalado la extensión de inicio de sesión seguro de mis aplicaciones, en la hoja **Probar el inicio de sesión único**, haga clic en **download the SAML response** (Descargar respuesta SAML).
    - Si no se ha instalado la extensión, use una herramienta como [Fiddler](https://www.telerik.com/fiddler) para recuperar la respuesta SAML. 
3. Observe estos elementos en el token de la respuesta SAML:
    - Identificador único del usuario del valor NameID y formato
    - Notificaciones emitidas en el token
    - Certificado usado en el token de inicio de sesión. Para obtener información sobre cómo revisar la respuesta SAML, vea [Protocolo SAML de inicio de sesión único](single-sign-on-saml-protocol.md).
4. Para más información sobre la respuesta SAML, vea [Protocolo SAML de inicio de sesión único](single-sign-on-saml-protocol.md).
5. Ahora que ha revisado la respuesta SAML, vea [Error en la página de aplicación después de iniciar sesión](../manage-apps/application-sign-in-problem-application-error.md) para obtener instrucciones sobre la resolución del problema. 
6. Si sigue sin poder iniciar sesión correctamente, puede preguntar al proveedor de la aplicación lo que falta en la respuesta SAML.


## <a name="next-steps"></a>Pasos siguientes
Ahora que el inicio de sesión único funciona para la aplicación, puede ir a la [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../manage-apps/user-provisioning.md) o a la [Introducción al acceso condicional en Azure Active Directory](../conditional-access/app-based-conditional-access.md).


