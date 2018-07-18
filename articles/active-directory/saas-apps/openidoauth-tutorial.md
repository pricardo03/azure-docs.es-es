---
title: Pasos para configurar una aplicación OpenID/OAuth desde la galería de aplicaciones de Azure AD | Microsoft Docs
description: Pasos para configurar una aplicación OpenID/OAuth desde la galería de aplicaciones de Azure AD.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: eedebb76-e78c-428f-9cf0-5891852e79fb
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: jeedes
ms.openlocfilehash: 69e9d66458409bbc744416a58ceb508349418a76
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37019560"
---
# <a name="steps-to-configure-an-openidoauth-application-from-azure-ad-app-gallery"></a>Pasos para configurar una aplicación OpenID/OAuth desde la galería de aplicaciones de Azure AD

## <a name="process-of-open-id-application-addition-from-gallery"></a>Proceso de adición de una aplicación OpenID de galería

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory](./media/openidoauth-tutorial/tutorial_general_01.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](./media/openidoauth-tutorial/tutorial_general_02.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](./media/openidoauth-tutorial/tutorial_general_03.png)

4. En el cuadro de búsqueda, escriba el **nombre de la aplicación**, seleccione la **aplicación que desee** en el panel de resultados e inicie de sesión en la aplicación.

    ![Incorporación de una aplicación](./media/openidoauth-tutorial/addfromgallery.png)

    > [!NOTE]
    > Para las aplicaciones OpenID Connect y OAuth, el botón Agregar está deshabilitado de forma predeterminada. Aquí, el administrador de inquilinos debe hacer clic en el botón de **suscripción** y proporcionar el consentimiento a la aplicación. Una vez hecho esto, la aplicación se agregará al inquilino de cliente (no es necesario agregarla de forma explícita), y realizará las configuraciones.

    ![Botón Agregar](./media/openidoauth-tutorial/addbutton.png)

5. Al hacer clic en el vínculo de suscripción, se le redirigirá a la página de Azure AD para las credenciales de inicio de sesión.

6. Una vez realizada la autenticación correctamente, el usuario debe aceptar el consentimiento de la página de consentimiento. Después, se mostrará la página principal de la aplicación.

    > [!NOTE]
    > Los clientes solo pueden agregar una instancia de la aplicación. Si ya ha agregado una y ha intentado proporcionar el consentimiento de nuevo, no se agregará nuevo en el inquilino. Por tanto, como es lógico, solo pueden usar una instancia de la aplicación en el inquilino.

## <a name="authentication-flow-using-openid-connect"></a>Flujo de autenticación con OpenID Connect

El flujo de inicio de sesión más básico contiene los pasos siguientes, cada uno de los cuales se describe en detalle a continuación.

![Flujo de autenticación con OpenID Connect](./media/openidoauth-tutorial/authenticationflow.png)

* **Aplicación multiempresa**: el uso de una aplicación multiempresa se destina a varias organizaciones, no solo a una. Suele tratarse de aplicaciones de software como servicio (SaaS) escritas por un proveedor de software independiente (ISV). Las aplicaciones multiempresa se deben aprovisionar en cada uno de los directorios en los que se usarán, lo que requiere el consentimiento del usuario o del administrador para registrarlas. El proceso de consentimiento se inicia cuando una aplicación se ha registrado en el directorio y se le proporciona acceso a Graph API o quizás a otra API web. Cuando un usuario o un administrador de otra organización inicia sesión para usar la aplicación, se muestra un cuadro de diálogo que indica los permisos requeridos por la aplicación. A continuación, el usuario o el administrador pueden dar su consentimiento a la aplicación, lo que proporciona a la aplicación acceso a los datos establecidos y, por último, registra la aplicación en el directorio.

    > [!NOTE]
    > Si pone la aplicación a disposición de usuarios de varios directorios, necesitará un mecanismo para determinar en qué inquilino se encuentran. Una aplicación de un solo inquilino únicamente necesita buscar un usuario en su propio directorio, mientras que una aplicación multiempresa debe identificar un usuario específico en todos los directorios de Azure AD. Para realizar esta tarea, Azure AD proporciona un extremo de autenticación común donde cualquier aplicación multiempresa puede dirigir solicitudes de inicio de sesión, en lugar de un extremo específico del inquilino. Este punto de conexión es [https://login.microsoftonline.com/common](https://login.microsoftonline.com/common) para todos los directorios en Azure AD, mientras que un punto de conexión específico del inquilino podría ser [https://login.microsoftonline.com/contoso.onmicrosoft.com](https://login.microsoftonline.com/contoso.onmicrosoft.com). Es especialmente importante tener en cuenta el extremo común al desarrollar la aplicación porque necesitará toda la lógica necesaria para administrar varios inquilinos durante el inicio y el cierre de sesión y la validación de tokens.

De forma predeterminada, el equipo de Azure AD promociona la aplicación multiinquilino, ya que se puede acceder fácilmente a ella a través de varias organizaciones y es fácil de usar después de aceptar el consentimiento.

## <a name="what-is-consent-framework"></a>¿Qué es el marco de consentimiento?

El marco de consentimiento de Azure AD facilita el desarrollo de aplicaciones web de varios inquilinos y aplicaciones de cliente nativo. Estas aplicaciones permiten el inicio de sesión por cuentas de usuario de un inquilino de Azure AD diferente al que ha registrado la aplicación. También pueden necesitar acceso a diferentes API web como Microsoft Graph API (para tener acceso a Azure Active Directory, Intune y los servicios de Office 365) y otras API de servicios de Microsoft, además de sus propias API web. El marco se basa en que un usuario o un administrador da consentimiento a una aplicación que solicita el registro en su directorio, lo cual puede implicar el acceso a los datos de directorio. Después de dar su consentimiento, la aplicación cliente podrá llamar a Microsoft Graph API en nombre del usuario y usar la información según sea necesario.

[Microsoft Graph API](https://graph.microsoft.io/) proporciona acceso a datos de Office 365 (como calendarios y mensajes de Exchange, sitios y listas de SharePoint, documentos de OneDrive, blocs de notas de OneNote, tareas de Planner, libros de trabajo de Excel, etc.), así como a usuarios y grupos de Azure AD y otros objetos de datos de más servicios en la nube de Microsoft.

Los siguientes pasos muestran cómo funciona la experiencia de consentimiento para el desarrollador de la aplicación y para el usuario.

1. Suponga que tiene una aplicación de cliente web que necesita solicitar permisos concretos para tener acceso a un recurso o API. Azure Portal se utiliza para declarar las solicitudes de permiso en el momento de la configuración. Al igual que otras opciones de configuración, pasan a formar parte del registro de Azure AD de la aplicación:

    ![Graph API](./media/openidoauth-tutorial/graphapi.png)

2. Plantéese la posibilidad de que los permisos de la aplicación estén actualizados, la aplicación se ejecute y un usuario esté a punto de usarla por primera vez. En primer lugar, la aplicación debe obtener un código de autorización del punto de conexión /authorize de Azure AD. El código de autorización, a continuación, se puede utilizar para adquirir un nuevo acceso y un token de actualización.

3. Si el usuario no está aún autenticado, el punto de conexión /authorize de Azure AD solicita el inicio de sesión.

    ![Autenticación](./media/openidoauth-tutorial/authentication.png)

4. Cuando el usuario inicie sesión, Azure AD determinará si se debe mostrar una página de consentimiento al usuario. Esta determinación se basa en que el usuario (o el administrador de la organización) ya concediese el consentimiento de la aplicación. Si todavía no se concedió el consentimiento, Azure AD se lo pide al usuario y muestra los permisos necesarios para que funcione. El conjunto de permisos que aparecen en el cuadro de diálogo de consentimiento coinciden con los que se seleccionaron en Permisos delegados en Azure Portal.

    ![Página de consentimiento](./media/openidoauth-tutorial/consentpage.png)

Algunos permisos pueden tener el consentimiento de un usuario normal, mientras que otros necesitan el del administrador del inquilino.

## <a name="whats-the-difference-between-admin-consent-and-user-consent"></a>¿Cuál es la diferencia entre el consentimiento del administrador y el consentimiento del usuario?

Como administrador, también puede dar su consentimiento para permisos delegados de una aplicación en nombre de todos los usuarios del inquilino. El consentimiento del administrador evita que el cuadro de diálogo de consentimiento aparezca para cada usuario en el inquilino y lo pueden llevar a cabo los usuarios con el rol de administrador en Azure Portal. En la página Configuración de la aplicación, haga clic en Permisos necesarios y luego en el botón Conceder permisos.

![Conceder permiso](./media/openidoauth-tutorial/grantpermission.png)

> [!NOTE]
> La concesión explícita de consentimiento con el botón Conceder permisos se requiere actualmente para aplicaciones de página única (SPA) que usan ADAL.js. En caso contrario, se produce un error en la aplicación cuando se solicita el token de acceso.

Los permisos de solo aplicación siempre requieren el consentimiento del administrador de inquilinos. Si la aplicación solicita un permiso de solo aplicación y un usuario intenta iniciar sesión en la aplicación, aparece un mensaje de error que indica que el usuario no puede dar su consentimiento.

Si la aplicación usa permisos que requieren el consentimiento del administrador, necesita tener un gesto, como un botón o un vínculo donde el administrador pueda iniciar la acción. La solicitud que la aplicación envía para esta acción es la solicitud de autorización habitual de OAuth2 u OpenID Connect que también incluye el parámetro de cadena de consulta prompt=admin_consent. Una vez que el administrador ha dado su consentimiento y la entidad de servicio se crea en el inquilino del cliente, las posteriores solicitudes de inicio de sesión no necesitan el parámetro prompt=admin_consent. Dado que el administrador ha decido que los permisos solicitados son aceptables, en adelante no se solicitará consentimiento a ningún otro usuario. Un administrador de inquilinos puede deshabilitar la posibilidad de que los usuarios normales den su consentimiento a las aplicaciones. Si esta capacidad está deshabilitada, para que la aplicación se use en el inquilino siempre se solicitará el consentimiento del administrador. Si quiere probar la aplicación con el consentimiento de usuario final deshabilitado, puede encontrar el modificador de configuración en [Azure Portal](https://portal.azure.com/), en la sección [Configuración de usuario](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) en **Aplicaciones empresariales**.

El parámetro prompt=admin_consent también se puede utilizar en las aplicaciones que solicitan permisos que no requieren el consentimiento del administrador. Un ejemplo de cuándo se debe usar esta opción es en el caso de que la aplicación requiera una experiencia en la que el administrador del inquilino se "registra" una vez, y no se solicita que otros usuarios den su consentimiento a partir de ese momento.

Si una aplicación requiere el consentimiento del administrador y un administrador inicia sesión sin enviar el parámetro prompt=admin_consent, cuando el administrador conceda su consentimiento correctamente a la aplicación, se aplicará solo para su cuenta de usuario. Los usuarios normales seguirán sin poder iniciar sesión ni dar su consentimiento a la aplicación. Esta característica resulta útil si quiere dar al administrador de inquilinos la posibilidad de explorar la aplicación antes de permitir el acceso a otros usuarios.