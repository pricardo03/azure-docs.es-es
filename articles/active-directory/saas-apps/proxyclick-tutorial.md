---
title: 'Tutorial: Integración de Azure Active Directory con Proxyclick | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Proxyclick.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5c58a859-71c2-4542-ae92-e5f16a8e7f18
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: jeedes
ms.openlocfilehash: 6e0d8c5c178afc63c9c177d31d0a14104b127941
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36213345"
---
# <a name="tutorial-azure-active-directory-integration-with-proxyclick"></a>Tutorial: Integración de Azure Active Directory con Proxyclick

En este tutorial se aprende a integrar Proxyclick con Azure Active Directory (Azure AD).

La integración de Proxyclick con Azure AD proporciona las siguientes ventajas:

- En Azure AD, puede controlar quién tiene acceso a Proxyclick.
- Puede permitir que los usuarios inicien sesión automáticamente en Proxyclick (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>requisitos previos

Para configurar la integración de Azure AD con Proxyclick, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en Proxyclick

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Proxyclick desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-proxyclick-from-the-gallery"></a>Incorporación de Proxyclick desde la galería
Para configurar la integración de Proxyclick en Azure AD, es preciso agregar Proxyclick desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Proxyclick desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Proxyclick**, seleccione **Proxyclick** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Proxyclick en la lista de resultados](./media/proxyclick-tutorial/tutorial_proxyclick_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección se configura y se prueba el inicio de sesión único de Azure AD con Proxyclick con un usuario de prueba de nombre "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Proxyclick para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Proxyclick.

Para configurar y probar el inicio de sesión único de Azure AD con Proxyclick, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Proxyclick](#create-a-proxyclick-test-user)**, para tener un homólogo de Britta Simon en Proxyclick que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección se habilita el inicio de sesión único de Azure AD en Azure Portal y se configura en la aplicación Proxyclick.

**Para configurar el inicio de sesión único de Azure AD con Proxyclick, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Proxyclick**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/proxyclick-tutorial/tutorial_proxyclick_samlbase.png)

3. En la sección **Dominio y direcciones URL de Proxyclick**, realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de inicio de sesión único de Dominio y direcciones URL de Proxyclick](./media/proxyclick-tutorial/tutorial_proxyclick_url2.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://saml.proxyclick.com/init/<companyId>`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://saml.proxyclick.com/consume/<companyId>`.

4. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de inicio de sesión único de Dominio y direcciones URL de Proxyclick](./media/proxyclick-tutorial/tutorial_proxyclick_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://saml.proxyclick.com/init/<companyId>`.

    > [!NOTE]
    > Estos valores no son reales. Los valores se actualizan con el identificador, la dirección URL de inicio de sesión y la dirección URL de respuesta reales, lo que se explica más adelante en el tutorial.

5. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/proxyclick-tutorial/tutorial_proxyclick_certificate.png) 

6. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/proxyclick-tutorial/tutorial_general_400.png)

7. En la sección **Configuración de Proxyclick**, haga clic en **Configurar Proxyclick** para abrir la ventana **Configurar inicio de sesión**. Copie **SAML Entity ID and SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML e Identificador de entidad de SAML) de la sección **Referencia rápida**.

    ![Configuración de Proxyclick](./media/proxyclick-tutorial/tutorial_proxyclick_configure.png)

8. En otra ventana del explorador web, inicie sesión en el sitio de la empresa de Proxyclick como administrador.

9. Seleccione **Cuenta y configuración**.

    ![Configuración de Proxyclick](./media/proxyclick-tutorial/configure1.png)

10. Desplácese hacia abajo hasta **INTEGRACIONES** y seleccione **SAML**.

    ![Configuración de Proxyclick](./media/proxyclick-tutorial/configure2.png)

11. En la sección **SAML**, realice los pasos siguientes:

    ![Configuración de Proxyclick](./media/proxyclick-tutorial/configure3.png)

    a. Copie el valor **SAML Consumer URL** (URL de consumidor SAML) y péguelo en el cuadro de texto **URL de respuesta** de la sección **Dominio y direcciones URL de Proxyclick** de Azure Portal.

    b. Copie el valor **SAML SSO Redirect URL** (URL de redireccionamiento de SSO de SAML) y péguelo en los cuadros de texto **URL de inicio de sesión** e **Identificador** de la sección **Dominio y direcciones URL de Proxyclick** de Azure Portal.

    c. Seleccione **SAML Request Method** (Método de solicitud SAML) como **Redirección HTTP**.

    d. En el cuadro de texto **Emisor**, pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que ha copiado de Azure Portal.

    e. En el cuadro de texto **Dirección URL del punto de conexión de SAML 2.0**, pegue el valor de **Dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    f. Abra el archivo de certificado descargado de Azure Portal en el Bloc de notas y luego péguelo en el cuadro de texto **Certificado**.

    g. Haga clic en **Guardar cambios**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/proxyclick-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/proxyclick-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/proxyclick-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/proxyclick-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-proxyclick-test-user"></a>Crear un usuario de prueba de Proxyclick

Para permitir que los usuarios de Azure AD inicien sesión en Proxyclick, deben aprovisionarse en Proxyclick. En el caso de Proxyclick, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en el sitio de la empresa de Proxyclick como administrador.

2. Haga clic en **Compañeros** en la barra de navegación superior.

    ![Agregar empleado](./media/proxyclick-tutorial/user1.png)

3. Haga clic en **Agregar compañero**

    ![Agregar empleado](./media/proxyclick-tutorial/user2.png)

4. En la sección **Agregar un compañero**, realice estos pasos:

    ![Agregar empleado](./media/proxyclick-tutorial/user3.png)

    a. En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico de un usuario; por ejemplo, **brittasimon@contoso.com**.

    b. En el cuadro de texto **Nombre**, escriba el nombre del usuario, en este caso, Britta.

    c. En el cuadro de texto **Apellidos**, escriba los apellidos del usuario, en este caso, Simon.

    d. Haga clic en **Agregar usuario**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección se habilita a Britta Simon para que use el inicio de sesión único de Azure al concederle acceso a Proxyclick.

![Asignación de rol de usuario][200]

**Para asignar a Britta Simon a Proxyclick, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Proxyclick**.

    ![Vínculo de Proxyclick en la lista de aplicaciones](./media/proxyclick-tutorial/tutorial_proxyclick_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Proxyclick en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Proxyclick.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/proxyclick-tutorial/tutorial_general_01.png
[2]: ./media/proxyclick-tutorial/tutorial_general_02.png
[3]: ./media/proxyclick-tutorial/tutorial_general_03.png
[4]: ./media/proxyclick-tutorial/tutorial_general_04.png

[100]: ./media/proxyclick-tutorial/tutorial_general_100.png

[200]: ./media/proxyclick-tutorial/tutorial_general_200.png
[201]: ./media/proxyclick-tutorial/tutorial_general_201.png
[202]: ./media/proxyclick-tutorial/tutorial_general_202.png
[203]: ./media/proxyclick-tutorial/tutorial_general_203.png

