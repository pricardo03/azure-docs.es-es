---
title: 'Tutorial: Integración de Azure Active Directory con Huddle | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Huddle.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.openlocfilehash: d9d145aa5da636574426f1ff4ad978eb857ab252
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54827931"
---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Tutorial: Integración de Azure Active Directory con Huddle

En este tutorial, obtendrá información sobre cómo integrar Huddle con Azure Active Directory (Azure AD).

La integración de Huddle con Azure AD le proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a Huddle
- Puede permitir que los usuarios inicien sesión automáticamente en Huddle (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Huddle, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Huddle

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Huddle desde la Galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-huddle-from-the-gallery"></a>Incorporación de Huddle desde la Galería

Para configurar la integración de Huddle en Azure AD, deberá agregar Huddle desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Huddle desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

4. En el cuadro de búsqueda, escriba **Huddle**. En el panel de resultados, seleccione **Huddle** y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/huddle-tutorial/tutorial_huddle_addfromgallery.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Huddle con un usuario de prueba llamado "Britta Simon"

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Huddle para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Huddle.

Para configurar y probar el inicio de sesión único de Azure AD con Huddle, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Huddle](#creating-a-huddle-test-user)**: para tener un homólogo de Britta Simon en Huddle que esté vinculado a la representación de Azure AD del usuario.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación de Huddle.

**Para configurar el inicio de sesión único de Azure AD con Huddle, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Huddle**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

2. Haga clic en **Cambiar el modo de inicio de sesión único** en la parte superior de la pantalla para seleccionar el modo **SAML**.

    ![Configurar inicio de sesión único](./media/huddle-tutorial/tutorial_general_300.png)

3. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/huddle-tutorial/tutorial_general_301.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](./media/huddle-tutorial/tutorial_general_302.png)

5. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    > [!NOTE]
    > La instancia de Huddle se detectará automáticamente en el dominio que especifique a continuación.

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Huddle](./media/huddle-tutorial/tutorial_huddle_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL:

    | | |
    |--|--|
    | `https://login.huddle.net`|
    | `https://login.huddle.com`|
    | |

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL:

    | | |
    |--|--|
    | `https://login.huddle.net/saml/browser-sso`|
    | `https://login.huddle.com/saml/browser-sso`|
    | `https://login.huddle.com/saml/idp-initiated-sso`|
    | |

    c. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Huddle](./media/huddle-tutorial/tutorial_huddle_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba cualquier dirección URL con el siguiente patrón:

    | | |
    |--|--|
    | `https://<customsubdomain>.huddle.com`|
    | `https://us.huddle.com`|
    | |

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Huddle](https://huddle.zendesk.com) para obtener este valor.

6. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el certificado adecuado según sus requisitos y guárdelo en el equipo.

    ![Configurar inicio de sesión único](./media/huddle-tutorial/tutorial_huddle_certificate.png)

7. En la sección **Set up Huddle** (Configurar Huddle), copie las direcciones URL adecuada según sus necesidades.

    ![Configurar inicio de sesión único](./media/huddle-tutorial/tutorial_huddle_configure.png)

8. Para configurar el inicio de sesión único en **Huddle** es preciso enviar el certificado que ha descargado y las direcciones URL que copió de la sección **Set up** **Huddle** (Configurar Huddle) de Azure Portal para el [equipo de soporte técnico de Huddle](https://huddle.zendesk.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

    >[!NOTE]
    > El inicio de sesión único debe habilitarlo el equipo de soporte técnico de Huddle. Cuando se haya completado la configuración, recibirá una notificación.

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](./media/huddle-tutorial/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](./media/huddle-tutorial/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-a-huddle-test-user"></a>Creación de un usuario de prueba de Huddle

Para permitir que los usuarios de Azure AD inicien sesión en Huddle, deben aprovisionarse en Huddle. En el caso de Huddle, el aprovisionamiento es una tarea manual.

**Siga estos pasos para configurar el aprovisionamiento de usuario:**

1. Inicie sesión en el sitio de compañía de **Huddle** como administrador.

2. Haga clic en **Área de trabajo**.

3. Haga clic en **Contactos\>Invitar a contactos**.

    ![Personas](./media/huddle-tutorial/IC787838.png "Personas")

4. En la sección **Crear nueva invitación** , lleve a cabo estos pasos:
  
    ![Nueva invitación](./media/huddle-tutorial/IC787839.png "Nueva invitación")
  
     a. En la lista **Choose a team to invite people to join** (Elegir un equipo al que invitar a unirse a los contactos), seleccione **equipo**.

    b. Escriba la **dirección de correo electrónico** de una cuenta de Azure AD válida que desee aprovisionar en el cuadro de texto **Escriba la dirección de correo electrónico de las personas que desea invitar**.

    c. Haga clic en **Invitar**.

    >[!NOTE]
    > El titular de la cuenta de Azure AD recibirá un mensaje de correo electrónico con un vínculo para confirmar la cuenta antes de que se active.

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Huddle que proporcione Huddle para aprovisionar cuentas de usuario de Azure AD.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Huddle para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Huddle**.

    ![Configurar inicio de sesión único](./media/huddle-tutorial/tutorial_huddle_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Huddle en el panel de acceso, debería iniciar sesión automáticamente en su aplicación de Huddle.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/huddle-tutorial/tutorial_general_01.png
[2]: ./media/huddle-tutorial/tutorial_general_02.png
[3]: ./media/huddle-tutorial/tutorial_general_03.png
[4]: ./media/huddle-tutorial/tutorial_general_04.png
[100]: ./media/huddle-tutorial/tutorial_general_100.png
[200]: ./media/huddle-tutorial/tutorial_general_200.png
[201]: ./media/huddle-tutorial/tutorial_general_201.png
[202]: ./media/huddle-tutorial/tutorial_general_202.png
[203]: ./media/huddle-tutorial/tutorial_general_203.png
