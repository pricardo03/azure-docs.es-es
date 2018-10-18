---
title: 'Tutorial: Integración de Azure Active Directory con Convene | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Convene.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2540ac46-1aea-496b-a8c5-575a2690f7db
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2018
ms.author: jeedes
ms.openlocfilehash: 6ff502f46cfd96b767191d11e3fcc53eb1be81d2
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45607325"
---
# <a name="tutorial-azure-active-directory-integration-with-convene"></a>Tutorial: integración de Azure Active Directory con Convene

En este tutorial, aprenderá a integrar Convene en Azure Active Directory (Azure AD).

Integrar Convene con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Convene.
- Puede permitir que los usuarios inicien sesión automáticamente en Convene (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Convene, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción de Convene habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Convene desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-convene-from-the-gallery"></a>Adición de Convene desde la galería
Para configurar la integración de Convene en Azure AD, deberá agregar Convene desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Convene desde la galería, siga los pasos a continuación:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Convene**, seleccione **Convene** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Convene en la lista de resultados](./media/convene-tutorial/tutorial_convene_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configura y prueba el inicio de sesión único en Azure AD con Convene en función de una usuaria de prueba llamada "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Convene para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Convene.

Para configurar y probar el inicio de sesión único de Azure AD con Convene, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Convene](#create-a-convene-test-user)**: para tener un homólogo de Britta Simon en Convene que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Convene.

**Para configurar el inicio de sesión único de Azure AD con Convene, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Convene**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/convene-tutorial/tutorial_convene_samlbase.png)

3. En la sección **Dominio y direcciones URL de Convene**, siga los pasos a continuación si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Convene](./media/convene-tutorial/tutorial_convene_url.png)

    En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://portal.convene.me.uk/saml/acs/<UID>`.

4. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de Convene](./media/convene-tutorial/tutorial_convene_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://portal.convene.me.uk/login`
     
    > [!NOTE] 
    > El valor de dirección URL de respuesta no es real. Actualícelo con la dirección URL de respuesta real. Póngase en contacto con el [equipo de soporte técnico de Convene](mailto:support@convene.me.uk) para obtener el valor.

5. La aplicación Convene espera las instrucciones de aserción de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.
    
    ![Configurar el atributo de inicio de sesión único](./media/convene-tutorial/tutorial_convene_attribute.png)

6. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/convene-tutorial/tutorial_convene_certificate.png) 

7. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/convene-tutorial/tutorial_general_400.png)
    
8. En la sección **Configuración de Convene**, haga clic en **Configurar Convene** para abrir la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de Convene](./media/convene-tutorial/tutorial_convene_configure.png) 

9. Para configurar el inicio de sesión único en **Convene**, es preciso enviar los valores descargados de **Certificado (Base64)**, **Dirección URL de cierre de sesión, Identificador de entidad de SAML y Dirección URL del servicio de inicio de sesión único de SAML** al [equipo de soporte técnico de Convene](mailto:support@convene.me.uk). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/convene-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/convene-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/convene-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/convene-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-convene-test-user"></a>Creación de un usuario de prueba de Convene

El objetivo de esta sección es crear una usuaria llamada Britta Simon en Convene. Convene admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Durante un intento de obtener acceso a Convene se crea un nuevo usuario, en caso de que no exista.

>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Convene](mailto:support@convene.me.uk).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Convene.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Convene, siga los pasos a continuación:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Convene**.

    ![El vínculo a Convene en la lista de aplicaciones](./media/convene-tutorial/tutorial_convene_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Convene en el Panel de acceso, debe iniciar sesión automáticamente en la aplicación Convene.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/convene-tutorial/tutorial_general_01.png
[2]: ./media/convene-tutorial/tutorial_general_02.png
[3]: ./media/convene-tutorial/tutorial_general_03.png
[4]: ./media/convene-tutorial/tutorial_general_04.png

[100]: ./media/convene-tutorial/tutorial_general_100.png

[200]: ./media/convene-tutorial/tutorial_general_200.png
[201]: ./media/convene-tutorial/tutorial_general_201.png
[202]: ./media/convene-tutorial/tutorial_general_202.png
[203]: ./media/convene-tutorial/tutorial_general_203.png

