---
title: 'Tutorial: Integración de Azure Active Directory con Infinite Campus | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e Infinite Campus.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3995b544-e751-4e0f-ab8b-c9a3862da6ba
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: jeedes
ms.openlocfilehash: 694999048d2f1df92e6b8631953f124c4009d46c
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55733967"
---
# <a name="tutorial-azure-active-directory-integration-with-infinite-campus"></a>Tutorial: Integración de Azure Active Directory con Infinite Campus

En este tutorial, obtendrá información sobre cómo integrar Infinite Campus con Azure Active Directory (Azure AD).

La integración de Infinite Campus con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Infinite Campus.
- Puede permitir que los usuarios inicien sesión automáticamente en Infinite Campus (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Infinite Campus, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Infinite Campus.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).
- Como mínimo, debe ser un administrador de Azure Active Directory para completar la configuración.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Infinite Campus desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-infinite-campus-from-the-gallery"></a>Adición de Infinite Campus desde la galería

Para configurar la integración de Infinite Campus en Azure AD, debe agregar Infinite Campus desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Infinite Campus desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Infinite Campus**, seleccione **Infinite Campus** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Infinite Campus en la lista de resultados](./media/infinitecampus-tutorial/tutorial_infinitecampus_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Infinite Campus con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Infinite Campus para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Infinite Campus.

Para configurar y probar el inicio de sesión único de Azure AD con Infinite Campus, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Infinite Campus](#creating-a-infinite-campus-test-user)**: para tener un homólogo de Britta Simon en Infinite Campus que esté vinculado a la representación de ella en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Infinite Campus.

**Para configurar el inicio de sesión único de Azure AD con Infinite Campus, realice los pasos siguientes:**

1. En la página de integración de la aplicación **Infinite Campus** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, si tiene un **archivo de metadatos del proveedor de servicios** lleve a cabo los pasos del 4.a al 4.d, y luego salte al 11.c. Si no tiene un archivo de metadatos del proveedor de servicio, vaya al paso 5.

     a. Haga clic en **Cargar el archivo de metadatos**.

        ![image](common/b9_saml.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![imagen](common/b9(1)_saml.png)

    c. Una vez que se haya cargado correctamente el archivo de metadatos, los valores **Identificador** y **Dirección URL de respuesta** se rellenan automáticamente en el cuadro de texto de la sección **Configuración básica de SAML**, como se muestra a continuación:

    ![imagen](./media/infinitecampus-tutorial/tutorial_infinitecampus_url.png)

    d. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón (el dominio variará con el modelo de hospedaje): `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`

5. Si no dispone del **archivo de metadatos del proveedor de servicios**, lleve a cabo los pasos siguientes (tenga en cuenta que el dominio variará en función del modelo de hospedaje):

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>/SIS`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<DOMAIN>.infinitecampus.com/campus/<DISTRICTNAME>`

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<DOMAIN>.infinitecampus.com/campus/SSO/<DISTRICTNAME>`.

    ![Información de dominio y direcciones URL de inicio de sesión único de Infinite Campus](./media/infinitecampus-tutorial/tutorial_infinitecampus_url1.png)

6. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic en el icono de **copia** para copiar la **dirección URL de metadatos de federación de la aplicación** y péguela en el Bloc de notas.

    ![Vínculo de descarga del certificado](./media/infinitecampus-tutorial/tutorial_infinitecampus_certificate.png) 

7. En la sección **Set up Infinite Campus** (Configuración de Infinite Campus), utilice los siguientes valores para validar cuándo cargar o utilizar el archivo de metadatos o dirección URL de Azure.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de Infinite Campus](common/configuresection.png)

8. En otra ventana del explorador web, inicie sesión en Infinite Campus como Administrador de seguridad.

9. En el lado izquierdo del menú, haga clic en **System Administration** (Administración del sistema).

    ![El administrador](./media/infinitecampus-tutorial/tutorial_infinitecampus_admin.png)

10. Vaya a **User Security** (Seguridad de usuarios) > **SAML Management** (Administración de SAML) > **SSO Service Provider Configuration** (Configuración del proveedor de servicios de SSO).

    ![SAML](./media/infinitecampus-tutorial/tutorial_infinitecampus_saml.png)

11. En la página **SSO Service Provider Configuration** (Configuración del proveedor de servicios de SSO), siga estos pasos:

    ![Inicio de sesión único](./media/infinitecampus-tutorial/tutorial_infinitecampus_sso.png)

     a. Seleccione **Enable SAML v2 Single Sign On**(Habilitar inicio de sesión único de SAML v2).
    
    b. En la sección **Select an option to retrieve Identity Provider (IDP) server data** (Seleccionar una opción para recuperar los datos del servidor del proveedor de identidades), seleccione **Metadata URL** (Dirección URL de metadatos), pegue el valor de **App Federation Metadata Url** (Dirección URL de metadatos de federación de aplicaciones) en el cuadro y después haga clic en **Sync** (Sincronizar).

    c. Haga clic en el vínculo **Service Provider Metadata** (Metadatos del proveedor de servicios) para guardar el **archivo de metadatos del proveedor de servicios** en el equipo y cárguelo en la sección **Configuración básica de SAML** para rellenar de forma automática los valores **Identificador** y **Dirección URL de respuesta** en Azure Portal (consulte el paso 4 para cargar y rellenar automáticamente valores, o el paso 5 para introducirlos manualmente).

    d. Después de hacer clic en **Sync** (Sincronizar), los valores se rellenan automáticamente en la página **SSO Service Provider Configuration** (Configuración del proveedor de servicios de SSO).

    e. Haga clic en **Save**(Guardar).

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un _único_ usuario de prueba en Azure Portal llamado Britta Simon.

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

### <a name="creating-a-infinite-campus-test-user"></a>Creación de un usuario de prueba de Infinite Campus

Infinite Campuss tiene una arquitectura centrada en datos demográficos. Póngase en contacto con el [equipo de soporte técnico de Infinite Campus](mailto:sales@infinitecampus.com) para agregar los usuarios a la plataforma de Infinite Campus.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Infinite Campus.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Infinite Campus**.

    ![Configurar inicio de sesión único](./media/infinitecampus-tutorial/tutorial_infinitecampus_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Infinite Campus en el panel de acceso, debe iniciar sesión automáticamente en su aplicación de Infinite Campus. Si está iniciando sesión en la aplicación Infinite Campus en el mismo navegador en el que está administrando Azure AD, asegúrese de que ha iniciado sesión en Azure AD como usuario de prueba. Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

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
