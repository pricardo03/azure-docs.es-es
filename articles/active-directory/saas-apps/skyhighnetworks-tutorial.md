---
title: 'Tutorial: Integración de Azure Active Directory con Skyhigh Networks | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Skyhigh Networks.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 48d6ddd1-4d3e-4019-8234-5e5212684d9c
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2018
ms.author: jeedes
ms.openlocfilehash: 403d5754052e616354413160c6c34fa16ff0d837
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39041949"
---
# <a name="tutorial-azure-active-directory-integration-with-skyhigh-networks"></a>Tutorial: Integración de Azure Active Directory con Skyhigh Networks

En este tutorial, aprenderá a integrar Skyhigh Networks con Azure Active Directory (Azure AD).

La integración de Skyhigh Networks con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Skyhigh Networks.
- Puede permitir que los usuarios inicien sesión automáticamente en Skyhigh Networks (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Skyhigh Networks, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Skyhigh Networks

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Skyhigh Networks desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-skyhigh-networks-from-the-gallery"></a>Adición de Skyhigh Networks desde la galería
Para configurar la integración de Skyhigh Networks en Azure AD, es preciso agregar Skyhigh Networks desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Skyhigh Networks desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Skyhigh Networks**, seleccione **Skyhigh Networks** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Skyhigh Networks en la lista de resultados](./media/skyhighnetworks-tutorial/tutorial_skyhighnetworks_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Skyhigh Networks con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Skyhigh Networks para un usuario de Azure AD. Es decir, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Skyhigh Networks.

Para configurar y probar el inicio de sesión único de Azure AD con Skyhigh Networks, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Skyhigh Networks](#create-a-skyhigh-networks-test-user)**: para tener un homólogo de Britta Simon en Skyhigh Networks que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Skyhigh Networks.

**Para configurar el inicio de sesión único de Azure AD con Skyhigh Networks, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de aplicaciones **Skyhigh Networks**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/skyhighnetworks-tutorial/tutorial_skyhighnetworks_samlbase.png)

3. En la sección **Skyhigh Networks Domain and URLs** (Dominio y direcciones URL de Skyhigh Networks), realice los siguientes pasos si quiere configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Skyhigh Networks](./media/skyhighnetworks-tutorial/tutorial_skyhighnetworks_url.png)

    a. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `https://<ENV>.myshn.net/shndash/saml/Azure_SSO`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<ENV>.myshn.net/shndash/response/saml-postlogin`.

4. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Skyhigh Networks](./media/skyhighnetworks-tutorial/tutorial_skyhighnetworks_url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<ENV>.myshn.net/shndash/saml/Azure_SSO`.
     
    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de Skyhigh Networks](mailto:support@skyhighnetworks.com) para obtener estos valores. 

5. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/skyhighnetworks-tutorial/tutorial_skyhighnetworks_certificate.png) 

6. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/skyhighnetworks-tutorial/tutorial_general_400.png)
    
7. En la sección **Configuración de Skyhigh Networks**, haga clic en **Configurar Skyhigh Networks** para abrir la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

    ![Configuración de Skyhigh Networks](./media/skyhighnetworks-tutorial/tutorial_skyhighnetworks_configure.png) 

8. Para configurar el inicio de sesión único en **Skyhigh Networks**, es preciso enviar los datos descargados de **certificado (Base64), dirección URL de inicio de sesión, identificador de identidad de SAML y dirección URL del servicio de inicio de sesión único de SAML** al [equipo de soporte técnico de Skyhigh Networks](mailto:support@skyhighnetworks.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/skyhighnetworks-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/skyhighnetworks-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/skyhighnetworks-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/skyhighnetworks-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="create-a-skyhigh-networks-test-user"></a>Creación de un usuario de prueba de Skyhigh Networks

En esta sección, creará un usuario llamado Britta Simon en Skyhigh Networks. Trabaje con el [equipo de soporte técnico de Skyhigh Networks](mailto:support@skyhighnetworks.com) para agregar los usuarios a la plataforma de Skyhigh Networks. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a Skyhigh Networks para que use el inicio de sesión único de Azure.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Skyhigh Networks, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Skyhigh Networks**.

    ![El vínculo de Skyhigh Networks en la lista de aplicaciones](./media/skyhighnetworks-tutorial/tutorial_skyhighnetworks_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Skyhigh Networks en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación Skyhigh Networks.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/skyhighnetworks-tutorial/tutorial_general_01.png
[2]: ./media/skyhighnetworks-tutorial/tutorial_general_02.png
[3]: ./media/skyhighnetworks-tutorial/tutorial_general_03.png
[4]: ./media/skyhighnetworks-tutorial/tutorial_general_04.png

[100]: ./media/skyhighnetworks-tutorial/tutorial_general_100.png

[200]: ./media/skyhighnetworks-tutorial/tutorial_general_200.png
[201]: ./media/skyhighnetworks-tutorial/tutorial_general_201.png
[202]: ./media/skyhighnetworks-tutorial/tutorial_general_202.png
[203]: ./media/skyhighnetworks-tutorial/tutorial_general_203.png

