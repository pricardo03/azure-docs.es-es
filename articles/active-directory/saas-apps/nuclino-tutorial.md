---
title: 'Tutorial: Integración de Azure Active Directory con Nuclino | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Nuclino.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 74bbab82-5581-4dcf-8806-78f77c746968
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2018
ms.author: jeedes
ms.openlocfilehash: 1a5346b98de48b1a2f8928c3c2bf30730588e9c1
ms.sourcegitcommit: a1140e6b839ad79e454186ee95b01376233a1d1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2018
ms.locfileid: "43145631"
---
# <a name="tutorial-azure-active-directory-integration-with-nuclino"></a>Tutorial: integración de Azure Active Directory con Nuclino

En este tutorial, aprenderá a integrar Nuclino con Azure Active Directory (Azure AD).

La integración de Nuclino con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Nuclino.
- Puede permitir que los usuarios inicien sesión automáticamente en Nuclino (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Nuclino, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Nuclino.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Nuclino desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-nuclino-from-the-gallery"></a>Incorporación de Nuclino desde la galería

Para configurar la integración de Nuclino en Azure AD, deberá agregar Nuclino desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Nuclino desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Nuclino**, seleccione **Nuclino** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Nuclino en la lista de resultados](./media/nuclino-tutorial/tutorial_nuclino_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Nuclino con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Nuclino para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Nuclino.

Para configurar y probar el inicio de sesión único de Azure AD con Nuclino, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Nuclino](#create-a-nuclino-test-user)**: para tener un homólogo de Britta Simon en Nuclino que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Nuclino.

**Para configurar el inicio de sesión único de Azure AD con Nuclino, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Nuclino**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/nuclino-tutorial/tutorial_nuclino_samlbase.png)

3. En la sección **Dominio y direcciones URL de Nuclino**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Nuclino](./media/nuclino-tutorial/tutorial_nuclino_url1.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://api.nuclino.com/api/sso/<UNIQUE-ID>/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://api.nuclino.com/api/sso/<UNIQUE-ID>/acs`.

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la dirección URL de respuesta reales de la sección **Autenticación** que se explica más adelante en este tutorial.

4. Active **Mostrar configuración avanzada de URL** y siga estos pasos si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Nuclino](./media/nuclino-tutorial/tutorial_nuclino_url2.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://app.nuclino.com/<UNIQUE-ID>/login`.

    > [!NOTE]
    > Este valor no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico de Nuclino](mailto:contact@nuclino.com) para obtener este valor.

5. La aplicación Nuclino espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección "**Atributos de usuario**" de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.

    ![Configurar inicio de sesión único](./media/Nuclino-tutorial/tutorial_attribute.png)

6. En la sección **Atributos de usuario**, active la casilla **Ver y editar todos los demás atributos de usuario** para expandir los atributos. Realice los pasos siguientes en cada uno de los atributos mostrados:

    | Nombre del atributo | Valor de atributo |
    | ---------------| --------------- |
    | first_name | user.givenname |
    | last_name | user.surname |

    a. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.

    ![Configurar inicio de sesión único](./media/nuclino-tutorial/tutorial_attribute_04.png)

    ![Configurar inicio de sesión único](./media/nuclino-tutorial/tutorial_attribute_05.png)

    b. En el cuadro de texto **Nombre**, escriba el **nombre de atributo** que se muestra para esa fila.

    c. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.

    d. Haga clic en **Aceptar**.

7. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/nuclino-tutorial/tutorial_nuclino_certificate.png)

8. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/nuclino-tutorial/tutorial_general_400.png)

9. En la sección **Configuración de Nuclino**, haga clic en **Configurar Nuclino** para abrir la ventana **Configurar inicio de sesión**. Copie **SAML Entity ID and SAML Single Sign-On Service URL** (URL del servicio de inicio de sesión único de SAML e Identificador de entidad de SAML) de la sección **Referencia rápida**.

    ![Configuración de Nuclino](./media/nuclino-tutorial/tutorial_nuclino_configure.png)

10. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Nuclino como administrador.

11. Haga clic en el **ICONO**.

    ![Configuración de Nuclino](./media/nuclino-tutorial/configure1.png)

12. Haga clic en **Inicio de sesión único de Azure AD** y seleccione **Configuración del equipo** en la lista desplegable.

    ![Configuración de Nuclino](./media/nuclino-tutorial/configure2.png)

13. Seleccione **Autenticación** en el panel de navegación izquierdo.

    ![Configuración de Nuclino](./media/nuclino-tutorial/configure3.png)

14. En la sección **Autenticación**, realice los pasos siguientes:

    ![Configuración de Nuclino](./media/nuclino-tutorial/configure4.png)

    a. Seleccione **Inicio de sesión único (SSO) basado en SAML**.

    b. Copie el valor de la **dirección URL de ACS (tiene que copiarlo y pegarlo en su proveedor de inicio de sesión único)** y péguelo en el cuadro de texto **URL de respuesta** de la sección de **Dominio y direcciones URL de Nuclino** en Azure Portal.

    c. Copie el valor del **identificador de entidad (tiene que copiarlo y pegarlo en su proveedor de inicio de sesión único)** y péguelo en el cuadro de texto **Identificador** de la sección de **Dominio y direcciones URL de Nuclino** en Azure Portal.

    d. En el cuadro de texto **Dirección URL de inicio de sesión único**, pegue el valor de **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    e. En el cuadro de texto **Entity ID** (Id. de entidad), pegue el valor de **SAML Entity ID** (Id. de entidad de SAML) que copió de Azure Portal.

    f. Abra el archivo **Certificado (Base64)** en el Bloc de notas. Copie el contenido del mismo en el Portapapeles y, después, péguelo en el cuadro de texto **Public certificate** (Certificado público).

    g. Haga clic en **GURDAR CAMBIOS**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/nuclino-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/nuclino-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/nuclino-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/nuclino-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-nuclino-test-user"></a>Creación de un usuario de prueba de Nuclino

El objetivo de esta sección es crear un usuario llamado Britta Simon en Nuclino. Nuclino admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a Nuclino, se crea un nuevo usuario, en caso de que no exista.

> [!Note]
> Si necesita crear manualmente un usuario, póngase en contacto con el [equipo de soporte técnico de Nuclino](mailto:contact@nuclino.com).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Nuclino.

![Asignación de rol de usuario][200]

**Para asignar Britta Simon a Nuclino, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Nuclino**.

    ![Vínculo a Nuclino en la lista de aplicaciones](./media/nuclino-tutorial/tutorial_nuclino_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Nuclino en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Nuclino.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/nuclino-tutorial/tutorial_general_01.png
[2]: ./media/nuclino-tutorial/tutorial_general_02.png
[3]: ./media/nuclino-tutorial/tutorial_general_03.png
[4]: ./media/nuclino-tutorial/tutorial_general_04.png

[100]: ./media/nuclino-tutorial/tutorial_general_100.png

[200]: ./media/nuclino-tutorial/tutorial_general_200.png
[201]: ./media/nuclino-tutorial/tutorial_general_201.png
[202]: ./media/nuclino-tutorial/tutorial_general_202.png
[203]: ./media/nuclino-tutorial/tutorial_general_203.png