---
title: 'Tutorial: integración de Azure Active Directory con Supermood | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Supermood.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: afc04efa-2eba-4e47-8ce4-b71eb293cd09
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.openlocfilehash: 98a39c52f206f19d3330695fd05f9a96c0bf4d36
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39443357"
---
# <a name="tutorial-azure-active-directory-integration-with-supermood"></a>Tutorial: Integración de Azure Active Directory con Supermood

En este tutorial, aprenderá a integrar Supermood con Azure Active Directory (Azure AD).

La integración de Supermood con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Supermood.
- Puede habilitar que los usuarios inicien sesión automáticamente en Supermood (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Supermood, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Supermood

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Supermood desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-supermood-from-the-gallery"></a>Adición de Supermood desde la galería
Para configurar la integración de Supermood en Azure AD, será preciso agregar Supermood desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Supermood desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

1. En el cuadro de búsqueda, escriba **Supermood**, seleccione **Supermood** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Supermood en la lista de resultados](./media/supermood-tutorial/tutorial_supermood_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Supermood con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Supermood para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Supermood.

Para configurar y probar el inicio de sesión único de Azure AD con Supermood, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Supermood](#create-a-supermood-test-user)**: para tener un homólogo de Britta Simon en Supermood que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Supermood.

**Para configurar el inicio de sesión único de Azure AD con Supermood, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Supermood**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/supermood-tutorial/tutorial_supermood_samlbase.png)

1. En la sección **Supermood Domain and URLs** (Dominio y direcciones URL de Supermood), lleve a cabo los pasos siguientes:

    ![Información de dominio y direcciones URL de inicio de sesión único de Supermood](./media/supermood-tutorial/tutorial_supermood_url.png)

    a. Active **Mostrar configuración avanzada de URL**.

    b. Si quiere configurar la aplicación en modo iniciado por **IDP**, en el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL: `https://supermood.co/auth/sso/saml20`

    c. Si quiere configurar la aplicación en modo iniciado por **IDP**, en el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL: `https://supermood.co/app/#!/loginv2`

1. La aplicación Supermood espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.
    
    ![Configurar inicio de sesión único](./media/supermood-tutorial/tutorial_supermood_attribute.png)

1. En la sección **Atributos de usuario** del cuadro de diálogo **Inicio de sesión único**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    | Nombre del atributo | Valor de atributo |
    | ---------------| --------------- |    
    | firstName | user.givenname |
    | lastName | user.surname |

    a. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.

    ![Configurar inicio de sesión único](./media/supermood-tutorial/tutorial_attribute_04.png)

    ![Configurar inicio de sesión único](./media/supermood-tutorial/tutorial_attribute_05.png)
    
    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.
    
    c. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.

    d. Deje **Espacio de nombres** en blanco.
    
    d. Haga clic en **Aceptar**.

1. En la sección **Certificado de firma de SAML**, haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/supermood-tutorial/tutorial_supermood_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/supermood-tutorial/tutorial_general_400.png)

1. Vaya a su panel de administración de Supermood.co como Administrador de seguridad.

1. Haga clic en **My account** (Mi cuenta) (abajo a la izquierda) y **Single Sign On (SSO)** (Inicio de sesión único [SSO]).

    ![Sencillo del certificado](./media/supermood-tutorial/tutorial_supermood_single.png)
1. En **Your SAML 2.0 configurations** (Sus configuraciones SAML 2.0), haga clic en **Add an SAML 2.0 configuration for an email domain** (Agregar una configuración de SAML 2.0 para un dominio de correo electrónico).

    ![Agregar del certificado](./media/supermood-tutorial/tutorial_supermood_add.png)

1. En **Add an SAML 2.0 configuration for an email domain** (Agregar una configuración de SAML 2.0 para un dominio de correo electrónico). sección, realice los siguientes pasos:

    ![SAML del certificado](./media/supermood-tutorial/tutorial_supermood_saml.png)

    a. En el cuadro de texto **email domain for this Identity provider** (dominio de correo electrónico para este proveedor de identidades), escriba su dominio.

    b. En el cuadro de texto **Use a metadata URL** (Utilizar una dirección URL de metadatos), pegue la **dirección URL de metadatos de federación de aplicación** que ha copiado de Azure Portal.

    c. Haga clic en **Agregar**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/supermood-tutorial/create_aaduser_01.png)

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/supermood-tutorial/create_aaduser_02.png)

1. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/supermood-tutorial/create_aaduser_03.png)

1. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/supermood-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-supermood-test-user"></a>Creación de un usuario de prueba de Supermood

En esta sección, creará un usuario llamado Britta Simon en Supermood. Supermood admite el aprovisionamiento Just-In-Time, que está habilitado de manera predeterminada para los usuarios cuyos mensajes de correo electrónico pertenecen a los dominios que se agregan durante la configuración final de Supermood. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a Supermood, se crea un nuevo usuario, en caso de que no exista.

>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Supermood](mailto:hello@supermood.fr).


### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Supermood.

![Asignación de rol de usuario][200] 

**Para asignar Britta Simon a Supermood, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Supermood**.

    ![Enlace a Supermood en la lista de aplicaciones](./media/supermood-tutorial/tutorial_supermood_app.png)  

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Supermood del panel de acceso, debería iniciar sesión automáticamente en su aplicación Showpad.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/supermood-tutorial/tutorial_general_01.png
[2]: ./media/supermood-tutorial/tutorial_general_02.png
[3]: ./media/supermood-tutorial/tutorial_general_03.png
[4]: ./media/supermood-tutorial/tutorial_general_04.png

[100]: ./media/supermood-tutorial/tutorial_general_100.png

[200]: ./media/supermood-tutorial/tutorial_general_200.png
[201]: ./media/supermood-tutorial/tutorial_general_201.png
[202]: ./media/supermood-tutorial/tutorial_general_202.png
[203]: ./media/supermood-tutorial/tutorial_general_203.png

