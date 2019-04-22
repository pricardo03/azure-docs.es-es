---
title: 'Tutorial: Integración de Azure Active Directory con EBSCO | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y EBSCO.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 144f7f65-69e9-4016-a151-fe1104fd6ba8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 2753daf225016d3bd8e07383193a1260b40a36d5
ms.sourcegitcommit: b8a8d29fdf199158d96736fbbb0c3773502a092d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59564982"
---
# <a name="tutorial-azure-active-directory-integration-with-ebsco"></a>Tutorial: Integración de Azure Active Directory con EBSCO

En este tutorial, aprenderá a integrar EBSCO con Azure Active Directory (Azure AD).
La integración de EBSCO con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a EBSCO.
* Puede permitir que los usuarios inicien sesión automáticamente en EBSCO (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con EBSCO, se necesitan los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para inicio de sesión único en EBSCO

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* EBSCO admite el inicio de sesión único iniciado por **SP** e **IDP**

* EBSCO admite el aprovisionamiento de usuarios **Just-In-Time**

## <a name="adding-ebsco-from-the-gallery"></a>Adición de EBSCO desde la galería

Para configurar la integración de EBSCO en Azure AD, será preciso agregar EBSCO desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar EBSCO desde la galería, realice los pasos siguientes:**

1. En **[Azure Portal](https://portal.azure.com)**, en el panel de navegación izquierdo, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Haga clic en el botón **Nueva aplicación** en la parte superior del cuadro de diálogo para agregar una nueva aplicación.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **EBSCO**, seleccione **EBSCO** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

     ![EBSCO en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con EBSCO con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de EBSCO.

Para configurar y probar el inicio de sesión único de Azure AD con EBSCO, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único en EBSCO](#configure-ebsco-single-sign-on)**: para configurar los valores de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación del usuario de prueba en EBSCO](#create-ebsco-test-user)**: para tener un homólogo de Britta Simon en EBSCO vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con EBSCO, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **EBSCO**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Set-up Single Sign-On with SAML** (Configurar inicio de sesión único con SAML), haga clic en el icono **Edit** (Editar) para abrir el cuadro de diálogo **Basic SAML Configuration** (Configuración básica de SAML).

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si desea configurar la aplicación en modo iniciado por **IDP**, realice el siguiente paso:

    ![Información de dominio y direcciones URL de inicio de sesión único de EBSCO](common/idp-identifier.png)

    En el cuadro de texto **Identificador**, escriba una dirección URL: `pingsso.ebscohost.com`

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![imagen](common/both-preintegrated-signon.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `http://search.ebscohost.com/login.aspx?authtype=sso&custid=<unique EBSCO customer ID>&profile=<profile ID>`

    > [!NOTE]
    > El valor de la dirección URL de inicio de sesión no es real. Actualícelo con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico para clientes de EBSCO](mailto:sso@ebsco.com) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

    o   **Elementos únicos:**  

    o   **Custid** = escriba el identificador de cliente único de EBSCO 

    o   **Profile** = los clientes pueden personalizar el vínculo para dirigir a los usuarios a un perfil específico (en función de lo que compran en EBSCO). Pueden especificar un identificador de perfil específico. Los identificadores principales son eds (servicio de detección de EBSCO) y ehost (bases de datos EBSOCOhost). Se proporcionan [aquí](https://help.ebsco.com/interfaces/EBSCOhost/EBSCOhost_FAQs/How_do_I_set_up_direct_links_to_EBSCOhost_profiles_and_or_databases#profile) instrucciones.

6. EBSCO espera las aserciones de SAML en un formato específico, que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono  **Editar**  para abrir el cuadro de diálogo  **Atributos de usuario** .

    ![imagen](common/edit-attribute.png)

     > [!Note]
    > El atributo **name** es obligatorio y se asigna con el valor del **identificador de nombre** en la aplicación EBSCO. Se agrega de forma predeterminada, por lo que no es necesario agregarla manualmente.

7. Además de lo anterior, la aplicación EBSCO espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes: 

    | NOMBRE | Atributo de origen|
    | ---------------| --------------- |    
    | Nombre   | user.givenname |
    | Apellidos   | user.surname |
    | Email   | user.mail |

     a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

9. En la sección **Set up EBSCO** (Configurar EBSCO), copie las direcciones URL que necesite.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-ebsco-single-sign-on"></a>Configuración del inicio de sesión único en EBSCO

Para configurar el inicio de sesión único en **EBSCO**, es preciso enviar el **XML de metadatos** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de EBSCO](mailto:sso@ebsco.com). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba brittasimon@yourcompanydomain.extension. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a EBSCO.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**,  **EBSCO**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **EBSCO**.

    ![Vínculo a EBSCO en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-ebsco-test-user"></a>Creación del usuario de prueba en EBSCO

En el caso de EBSCO, el aprovisionamiento de usuarios es automática.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

Azure AD pasa los datos necesarios a la aplicación de EBSCO. El aprovisionamiento de usuarios de EBSCO puede ser automático O requerir un formulario de un solo uso. Depende de si el cliente tiene muchas cuentas EBSCOhost preexistentes con configuraciones personales guardadas. Esto mismo puede analizarse con el [equipo de soporte técnico de EBSCO](mailto:sso@ebsco.com) durante la implementación. En cualquier caso, el cliente no tiene que crear las cuentas de EBSCOhost antes de la prueba.

   >[!Note]
   >Puede automatizar el aprovisionamiento o la personalización de usuarios de EBSCOhost. Póngase en contacto con el [equipo de soporte técnico de EBSCO](mailto:sso@ebsco.com) sobre el aprovisionamiento de usuarios Just-In-Time. 

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

1. Al hacer clic en el icono de EBSCO en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación EBSCO.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

2. Cuando inicie sesión en la aplicación, haga clic en el botón **Iniciar sesión** en la esquina superior derecha.

    ![Inicio de sesión en EBSCO en la lista de aplicaciones](./media/ebsco-tutorial/tutorial_ebsco_signin.png)
 
3. Recibirá un aviso único para emparejar el inicio de sesión institucional o de SAML y **enlazar la cuenta MyEBSCOhost existente a la cuenta de su institución ahora** O BIEN para **crear una nueva cuenta MyEBSCOhost y vincularla a la cuenta de su institución**. La cuenta se utiliza para la personalización en la aplicación EBSCOhost. Seleccione la opción **Crear una cuenta** y verá que el formulario de personalización está completado previamente con los valores de la respuesta saml, tal como se muestra en la captura de pantalla siguiente. Haga clic en **'Continuar'** para guardar esta selección.
    
     ![Usuario de EBSCO en la lista de aplicaciones](./media/ebsco-tutorial/tutorial_ebsco_user.png)

1. Después de completar la instalación anterior, limpie las cookies y la memoria caché y vuelva a iniciar sesión. No tendrá que iniciar sesión manualmente de nuevo ya que la configuración de personalización se recuerda.

## <a name="additional-sesources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

