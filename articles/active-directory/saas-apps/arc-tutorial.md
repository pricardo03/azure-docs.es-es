---
title: 'Tutorial: Integración de Azure Active Directory con Arc Publishing - SSO | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Arc Publishing - SSO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ae609583-f875-4cb8-b68e-1b0b7938e9a7
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: jeedes
ms.openlocfilehash: eafd7998e5bc21a539b6709794fe3cd70d9e3179
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054325"
---
# <a name="tutorial-azure-active-directory-integration-with-arc-publishing---sso"></a>Tutorial: Integración de Azure Active Directory con Arc Publishing - SSO

En este tutorial aprenderá a integrar Arc Publishing - SSO con Azure Active Directory (Azure AD).

La integración de Arc Publishing - SSO con Azure AD le proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a Arc Publishing - SSO.
- Puede permitir que los usuarios inicien sesión automáticamente en Arc Publishing - SSO con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Arc Publishing - SSO, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Arc Publishing - SSO

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar Arc Publishing - SSO desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-arc-publishing---sso-from-the-gallery"></a>Agregar Arc Publishing - SSO desde la galería
Para configurar la integración de Arc Publishing - SSO en Azure AD, tiene que agregar Arc Publishing - SSO desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Arc Publishing - SSO desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Arc Publishing - SSO**, seleccione **Arc Publishing - SSO** en el panel de resultados y, después, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Arc Publishing - SSO en la lista de resultados](./media/arc-tutorial/tutorial_arc_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección podrá configurar y probar el inicio de sesión único de Azure AD con Arc Publishing - SSO mediante un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Arc Publishing - SSO para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Arc Publishing - SSO.

Para configurar y probar el inicio de sesión único de Azure AD con Arc Publishing - SSO, es preciso completar los siguientes bloques de compilación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Arc Publishing - SSO](#create-an-arc-publishing---sso-test-user)**: para tener un homólogo de Britta Simon en Arc Publishing - SSO que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Arc Publishing - SSO.

**Para configurar el inicio de sesión único de Azure AD con Arc Publishing - SSO, siga estos pasos:**

1. En la página de integración de la aplicación **Arc Publishing - SSO** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/arc-tutorial/tutorial_arc_samlbase.png)

1. En la sección **Dominio y direcciones URL de Arc Publishing - SSO**, siga estos pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Arc Publishing - SSO](./media/arc-tutorial/tutorial_arc_url.png)

    1. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://www.okta.com/saml2/service-provider/<Unique ID>`

    1. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`.

1. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Arc Publishing - SSO](./media/arc-tutorial/tutorial_arc_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://arcpublishing-<Customer>.okta.com/sso/saml2/<Unique ID>`.
     
    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de Arc Publishing - SSO](mailto:inf@washpost.com) para obtener estos valores. 

1. La aplicación Arc Publishing - SSO espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.
    
    ![Configurar inicio de sesión único](./media/arc-tutorial/tutorial_arc_attribute.png)

1. En la sección **Atributos de usuario** del cuadro de diálogo **Inicio de sesión único**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    | Nombre del atributo | Valor de atributo |
    | ---------------| --------------- |    
    | firstName | user.givenname |
    | lastName | user.surname |
    | email | user.mail |
    | groups | user.assignedroles |

    1. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.

     ![Configurar inicio de sesión único](./media/arc-tutorial/tutorial_attribute_04.png)

     ![Configurar inicio de sesión único](./media/arc-tutorial/tutorial_attribute_05.png)
    
    1. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.
    
    1. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.

    1. Deje **Espacio de nombres** en blanco.
    
    1. Haga clic en **Aceptar**.

    > [!NOTE]
    > Aquí, el atributo **groups** se asigna a **user.assignedroles**. Son roles personalizados creados en Azure AD para asignar los nombres de los grupos en la aplicación. [Aquí](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) encontrará más información sobre cómo crear roles personalizados en Azure AD. 

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/arc-tutorial/tutorial_arc_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/arc-tutorial/tutorial_general_400.png)
    
1. En la sección **Configuración de Arc Publishing - SSO**, haga clic en **Configurar Arc Publishing - SSO** para abrir la ventana **Configurar inicio de sesión único**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de Arc Publishing - SSO](./media/arc-tutorial/tutorial_arc_configure.png) 

1. Para configurar el inicio de sesión único en **Arc Publishing - SSO**, debe enviar los valores descargados de **Certificate (Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** [Certificado (Base64), Dirección URL de cierre de sesión, Identificador de entidad SAML y Dirección URL del servicio de inicio de sesión único de SAML] al [equipo de soporte técnico de Arc Publishing - SSO](mailto:inf@washpost.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/arc-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/arc-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/arc-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/arc-tutorial/create_aaduser_04.png)

    1. En el cuadro **Nombre**, escriba **BrittaSimon**.

    1. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    1. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    1. Haga clic en **Create**(Crear).
 
### <a name="create-an-arc-publishing---sso-test-user"></a>Crear un usuario de prueba de Arc Publishing - SSO

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Arc Publishing - SSO. Arc Publishing - SSO admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Durante un intento de acceder a Arc Publishing - SSO, se crea un usuario, en caso de que no exista.

>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Arc Publishing - SSO](mailto:inf@washpost.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure y concederá acceso a Arc Publishing - SSO.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Arc Publishing - SSO, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Arc Publishing - SSO**.

    ![Vínculo a Arc Publishing - SSO en la lista de aplicaciones](./media/arc-tutorial/tutorial_arc_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Arc Publishing - SSO del panel de acceso, debería iniciar sesión automáticamente en su aplicación Arc Publishing - SSO.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/arc-tutorial/tutorial_general_01.png
[2]: ./media/arc-tutorial/tutorial_general_02.png
[3]: ./media/arc-tutorial/tutorial_general_03.png
[4]: ./media/arc-tutorial/tutorial_general_04.png

[100]: ./media/arc-tutorial/tutorial_general_100.png

[200]: ./media/arc-tutorial/tutorial_general_200.png
[201]: ./media/arc-tutorial/tutorial_general_201.png
[202]: ./media/arc-tutorial/tutorial_general_202.png
[203]: ./media/arc-tutorial/tutorial_general_203.png

