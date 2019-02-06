---
title: 'Tutorial: Integración de Azure Active Directory con HeyBuddy | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y HeyBuddy.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d51b5af6-018e-4678-9a3f-b70438394f67
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2018
ms.author: jeedes
ms.openlocfilehash: 123b74298ebb01e8ebcfd70d023b4e71a879e7f9
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197898"
---
# <a name="tutorial-azure-active-directory-integration-with-heybuddy"></a>Tutorial: Integración de Azure Active Directory con HeyBuddy

En este tutorial, aprenderá a integrar HeyBuddy con Azure Active Directory (Azure AD).

La integración de HeyBuddy con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a HeyBuddy.
- Puede permitir que los usuarios inicien sesión automáticamente en HeyBuddy (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con HeyBuddy, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de HeyBuddy.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de HeyBuddy desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-heybuddy-from-the-gallery"></a>Adición de HeyBuddy desde la galería

Para configurar la integración de HeyBuddy en Azure AD, será preciso que agregue HeyBuddy desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar HeyBuddy desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **HeyBuddy**, seleccione **HeyBuddy** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![HeyBuddy en la lista de resultados](./media/heybuddy-tutorial/tutorial_heybuddy_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con HeyBuddy con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de HeyBuddy para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de HeyBuddy.

Para configurar y probar el inicio de sesión único de Azure AD con HeyBuddy, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de HeyBuddy](#creating-a-heybuddy-test-user)**: para tener un homólogo de Britta Simon en HeyBuddy que esté vinculado a la representación de Azure AD de usuario.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación HeyBuddy.

**Para configurar el inicio de sesión único de Azure AD con HeyBuddy, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **HeyBuddy**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de HeyBuddy](./media/heybuddy-tutorial/tutorial_heybuddy_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://api.heybuddy.com/auth/<ENTITY ID>`

    b. En el cuadro de texto **Identificador (id. de entidad)**, escriba una dirección URL con el siguiente patrón: `YourCompanyInstanceofHeyBuddy`

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con los valores reales de dirección URL de inicio de sesión e identificador (Id. de entidad). El identificador `Entity ID` en la dirección URL de inicio de sesión se genera automáticamente para cada organización. Póngase en contacto con el [equipo de soporte técnico al cliente de HeyBuddy](mailto:support@heybuddy.com) para obtener estos valores.

5. La aplicación HeyBuddy espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **User Attributes & Claims** (Atributos y notificaciones del usuario) de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario).

    ![imagen](./media/heybuddy-tutorial/tutorial_heybuddy_attribute.png)

    >[!NOTE]
    >Consulte este [vínculo](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) para obtener información sobre cómo configurar e instalar los roles de la aplicación.

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario), configure el atributo token SAML como se muestra en la imagen anterior y realice los siguientes pasos:
    
    | NOMBRE  | Atributo de origen  |
    | --------------- | --------------- |
    | Roles               | user.assignedroles |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./common/new_save_attribute.png)

    ![imagen](./common/new_attribute_details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje el valor de **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Save**(Guardar).

7. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic en el icono de **copia** para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/heybuddy-tutorial/tutorial_heybuddy_certificate.png) 

8. Para configurar el inicio de sesión único en **M**, tiene que enviar la **dirección URL de metadatos de federación de la aplicación** generada al [equipo de soporte técnico de HeyBuddy](mailto:support@heybuddy.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-a-heybuddy-test-user"></a>Creación de un usuario de prueba de HeyBuddy

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en HeyBuddy. HeyBuddy admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a HeyBuddy, se crea un nuevo usuario, en caso de que no exista.
>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el  [equipo de soporte técnico de HeyBuddy](mailto:support@heybuddy.com).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a HeyBuddy.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **HeyBuddy**.

    ![Configurar inicio de sesión único](./media/heybuddy-tutorial/tutorial_heybuddy_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de HeyBuddy en el panel de acceso, debería iniciar sesión automáticamente en su aplicación HeyBuddy.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial_general_01.png
[2]: common/tutorial_general_02.png
[3]: common/tutorial_general_03.png
[4]: common/tutorial_general_04.png

[100]: common/tutorial_general_100.png

[201]: common/tutorial_general_201.png
[202]: common/tutorial_general_202.png
[203]: common/tutorial_general_203.png
