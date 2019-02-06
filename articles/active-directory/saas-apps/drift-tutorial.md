---
title: 'Tutorial: Integración de Azure Active Directory con Drift | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Drift.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 39dcbb95-c192-448c-86a1-cedede1c0972
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2018
ms.author: jeedes
ms.openlocfilehash: f7973ccb384a8e882a9ced5020a53824bf0c4e7d
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55169882"
---
# <a name="tutorial-azure-active-directory-integration-with-drift"></a>Tutorial: Integración de Azure Active Directory con Drift

En este tutorial, aprenderá a integrar Drift con Azure Active Directory (Azure AD).

La integración de Drift con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Drift.
- Puede permitir que los usuarios inicien sesión automáticamente en Drift (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Drift, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Drift

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Drift desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-drift-from-the-gallery"></a>Adición de Drift desde la galería

Para configurar la integración de Drift en Azure AD, tendrá que agregar Drift desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Drift desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Drift**, seleccione **Drift** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Drift en la lista de resultados](./media/drift-tutorial/tutorial_drift_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Drift con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Drift para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario correspondiente de Drift.

Para configurar y probar el inicio de sesión único de Azure AD con Drift, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Drift](#creating-a-drift-test-user)**: para tener un homólogo de Britta Simon en Drift que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en su aplicación Drift.

**Para configurar el inicio de sesión único de Azure AD con Drift, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Drift**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de dominio y direcciones URL de inicio de sesión único de Drift](./media/drift-tutorial/tutorial_drift_url.png)

     a. Haga clic en **Establecer direcciones URL adicionales**.

    b. En el cuadro de texto **Estado de la retransmisión**, escriba una dirección URL: `https://app.drift.com`

    c. Si quiere configurar la aplicación en modo iniciado por **SP**, en el cuadro de texto **Sign-on URL** (URL de inicio de sesión), escriba una dirección URL: `https://start.drift.com`

5. La aplicación Drift espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **User Attributes & Claims** (Atributos y notificaciones del usuario) de la página de integración de aplicaciones. Haga clic en el botón **Edit** (Editar) para abrir el cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario).

    ![imagen](./media/drift-tutorial/tutorial_drift_attribute.png)

6. En la sección **Notificaciones del usuario** del cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario), configure el atributo token SAML como se muestra en la imagen anterior y realice los siguientes pasos:
    
    | Nombre del atributo | Valor de atributo |
    | ---------------| --------------- |    
    | NOMBRE | user.displayname |

     a. Haga clic en la reclamación `name` (reclamación resaltada) para abrir el cuadro de diálogo **Manage user claims** (Administrar las reclamaciones del usuario).

    ![imagen](./media/drift-tutorial/tutorial_drift_attribute1.png)

    ![imagen](./media/drift-tutorial/tutorial_drift_attribute3.png)

    b. Seleccione **Atributo** como origen.

    c. Deje el campo de **espacio de nombres** en blanco.

    d. En la lista **Source attribute** (Atributo de origen), seleccione **user.mail**.

    e. Haga clic en **Save**(Guardar).

7. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **archivo XML de metadatos de federación** y, luego, guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/drift-tutorial/tutorial_drift_certificate.png) 

8. En otra ventana del explorador web, inicie sesión en Drift como administrador.

9. En la parte izquierda de la barra de menús, haga clic en el **icono de configuración** > **App Settings** > **Authentication** (Configuración de la aplicación > Autenticación) y realice los pasos siguientes:

    ![Vínculo de administración](./media/drift-tutorial/tutorial_drift_admin.png)

     a. Cargue el **XML de metadatos de la federación** que ha descargado de Azure Portal en el cuadro de texto **Upload Identity Provider metadata file** (Cargar archivo de metadatos del proveedor de identidades).

    b. Después de cargar el archivo de metadatos, los valores restantes se rellenan automáticamente en la página.

    c. Haga clic en **Enable SAML**(Habilitar SAML).

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

### <a name="creating-a-drift-test-user"></a>Creación de un usuario de prueba de Drift

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Drift. Drift admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Al intentar acceder a Drift, se crea un nuevo usuario, en caso de que no exista.
>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el  [equipo de soporte técnico de Drift](mailto:integrations@drift.com).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Drift.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Drift**.

    ![Configurar inicio de sesión único](./media/drift-tutorial/tutorial_drift_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Add Assignment** (Agregar asignación), seleccione el botón **Assign** (Asignar).

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Drift en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Drift.
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
