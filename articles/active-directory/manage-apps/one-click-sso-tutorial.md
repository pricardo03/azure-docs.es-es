---
title: Configuración del Inicio de sesión único de One Click a una aplicación desde la galería de aplicaciones de Azure AD | Microsoft Docs
description: Pasos para configurar el Inicio de sesión único de One Click a una aplicación desde la galería de aplicaciones de Azure AD.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: e0416991-4b5d-4b18-89bb-91b6070ed3ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 358240823da469551e254356fc0613bea20d78c5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67064819"
---
# <a name="one-click-sso-feature-for-azure-ad-gallery-applications"></a>Característica Inicio de sesión único de One Click para aplicaciones de la galería de Azure AD

 En este tutorial, aprenderás a realizar el Inicio de sesión único de One Click para todas las aplicaciones de SAML que proporcionan la interfaz de usuario para la configuración de inicio de sesión único (SSO).

## <a name="introduction-to-one-click-sso"></a>Introducción al Inicio de sesión único de One Click

La característica Inicio de sesión único de One Click se presenta para configurar el inicio de sesión único para aplicaciones de la galería de Azure AD que admiten el protocolo SAML. En la página de configuración de SSO de Azure AD, hemos proporcionado esta opción para permitir que nuestros clientes configuren automáticamente los metadatos de Azure AD en el lado de la aplicación. El objetivo es ayudar a los clientes a configurar rápidamente el SSO con el mínimo esfuerzo manual. 

## <a name="advantages-of-the-one-click-sso"></a>Ventajas del Inicio de sesión único de One Click

- Configuración rápida de inicio de sesión único de las aplicaciones de la galería donde los clientes tienen que llevar a cabo la configuración manual en el lado de la aplicación.
- Manera más eficaz y precisa de configuración.
- Ninguna necesidad de comunicación ni asistencia del partner para la configuración, ya que la aplicación proporciona la interfaz de usuario para la configuración de SAML.

## <a name="prerequisites"></a>Requisitos previos

- Suscripción activa de la aplicación con las credenciales de administrador que quieres configurar con Inicio de sesión único de One Click.
- **Extensión de inicio de sesión seguro de Mis aplicaciones para el explorador** de la versión de Microsoft instalada en el explorador. Si quieres más información sobre esta extensión, consulta este [vínculo](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access).

## <a name="one-click-sso-feature-step-by-step-details"></a>Instrucciones paso a paso de la característica Inicio de sesión único de One Click

1. Agrega la aplicación desde la galería de aplicaciones de Azure AD.

2. Haz clic en Inicio de sesión único.

3. Haz clic en Habilitar el inicio de sesión único.

4. Rellena los valores de configuración obligatorios en la sección de Configuración básica de SAML.

    > [!NOTE] 
    > Si es necesario configurar notificaciones personalizadas para la aplicación, configúralas antes de Inicio de sesión único de One Click.

5. Si se implementa la característica Inicio de sesión único de One Click para cualquier aplicación de la galería, verás la pantalla siguiente. Si la **extensión de inicio de sesión seguro de Mis aplicaciones para el explorador** aún no está instalada, haz clic en la opción **Instalar la extensión**.

    ![Instalar la extensión de inicio de sesión seguro de Mis aplicaciones para el explorador](./media/one-click-sso-tutorial/install-myappssecure-extension.png)

6. Después de agregar la extensión al explorador, haz clic en **Configurar nombre de la aplicación**, que te redirigirá al portal de administración de la aplicación. Tienes que iniciar sesión como administrador para acceder a la aplicación.

    ![Configurar nombre de la aplicación](./media/one-click-sso-tutorial/setup-sso.png)

7. La extensión de explorador configurará automáticamente la aplicación. Primero pide que confirmes si quieres continuar. Haga clic en **Sí**.

    ![Guardar los datos rellenados automáticamente](./media/one-click-sso-tutorial/save-autopopulate.png)

    > [!NOTE]
    > Si una aplicación necesita pasos o una navegación adicionales, deberías ver los mensajes apropiados para realizar esos pasos. 

8. Una vez que completes la configuración, haz clic en **Aceptar** para guardar los cambios.

    ![Guardar los datos rellenados automáticamente](./media/one-click-sso-tutorial/save-data.png)

9. Se muestra un mensaje emergente de confirmación correcta, y las opciones de inicio de sesión único se han configurado correctamente. Puedes probar la aplicación.

    ![Inicio de sesión único configurado](./media/one-click-sso-tutorial/sso-configured.png)

10. Una vez finalizada correctamente la configuración, se cerrará la sesión de la aplicación y volverás a Azure Portal.

11. Puedes hacer clic en el botón Probar para probar el inicio de sesión único.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
* [Qué es la extensión de inicio de sesión seguro de Mis aplicaciones para el explorador](https://docs.microsoft.com/azure/active-directory/user-help/my-apps-portal-end-user-access)
 