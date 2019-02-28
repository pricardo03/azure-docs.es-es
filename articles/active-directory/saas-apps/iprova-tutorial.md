---
title: 'Tutorial: Integración de Azure Active Directory con iProva | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e iProva.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 1eaeef9b-4479-4a9f-b1b2-bc13b857c75c
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c65f8e023e087c3ca49e0d53f5760fbcd9a5f4d9
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56869214"
---
# <a name="tutorial-azure-active-directory-integration-with-iprova"></a>Tutorial: Integración de Azure Active Directory con iProva

En este tutorial, obtendrá información sobre cómo integrar iProva con Azure Active Directory (Azure AD).
Integrar iProva con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a iProva.
* Puede permitir que los usuarios inicien sesión automáticamente en iProva (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [Inicio de sesión único en aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con iProva, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes en el sitio web de [Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/).
* Una suscripción habilitada para el inicio de sesión único de iProva.

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba:

* iProva admite el inicio de sesión único iniciado por SP.

## <a name="add-iprova-from-the-gallery"></a>Incorporación de iProva desde la galería

Para configurar la integración de iProva en Azure AD, agregue iProva desde la galería a la lista de aplicaciones SaaS administradas.

Para agregar iProva desde la galería, siga estos pasos:

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una aplicación nueva, en la parte superior del cuadro de diálogo, seleccione **Nueva aplicación**.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **iProva**. Seleccione **iProva** desde el panel de resultados y, a continuación, seleccione **Agregar** para agregar la aplicación.

     ![iProva en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con iProva con un usuario de prueba llamado Britta Simon.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de iProva.

Para configurar y probar el inicio de sesión único de Azure AD con iProva, complete los siguientes bloques de creación:

- [Recuperación de la información de configuración desde iProva](#retrieve-configuration-information-from-iprova) como preparación para los pasos siguientes.
- [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on), para permitir que los usuarios utilicen esta característica.
- [Configuración del inicio de sesión único de iProva](#configure-iprova-single-sign-on) para configurar los valores de Inicio de sesión único en la aplicación.
- [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con Britta Simon.
- [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para permitir que Britta Simon use el inicio de sesión único de Azure AD.
- [Creación de un usuario de prueba de iProva](#create-an-iprova-test-user) para tener un homólogo de Britta Simon en iProva que esté vinculado a la representación del usuario en Azure AD.
- [Prueba del inicio de sesión único](#test-single-sign-on), para comprobar si funciona la configuración.

### <a name="retrieve-configuration-information-from-iprova"></a>Recuperación de la información de configuración desde iProva

En esta sección, va a recuperar información de iProva para configurar el inicio de sesión único en Azure AD.

1. Abra un explorador web y vaya a la **página de información de SAML2** en iProva, con el siguiente patrón de dirección URL:

    | | |
    |-|-|
    | `https://SUBDOMAIN.iprova.nl/saml2info`|
    | `https://SUBDOMAIN.iprova.be/saml2info`|
    | | |

     ![Visualización de la página de información de SAML2 de iProva](media/iprova-tutorial/iprova-saml2-info.png)

2. Deje abierta la pestaña del explorador mientras continúa con los pasos siguientes en otra pestaña.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con iProva, realice los pasos siguientes.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones **iProva**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Set-up Single Sign-On with SAML** (Configurar inicio de sesión único con SAML), seleccione el icono **Edit** (Editar) para abrir el cuadro de diálogo **Basic SAML Configuration** (Configuración básica de SAML).

    ![Icono Editar de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos.

     a. Rellene el campo **Identifier** (Identificador) con el valor que se muestra detrás de la etiqueta **EntityID** (Identificador de la entidad) de la **página de información de SAML2 de iProva**. Esta página sigue abierta en la otra pestaña del explorador.

    b. Rellene el campo **URL de respuesta** con el valor que se muestra detrás de la etiqueta **URL de respuesta** de la página de **información de SAML2 de iProva**. Esta página sigue abierta en la otra pestaña del explorador.

    c. Rellene el campo **URL de inicio de sesión** con el valor que se muestra detrás de la etiqueta **URL de inicio de sesión** de la página de **información de SAML2 de iProva**. Esta página sigue abierta en la otra pestaña del explorador.

    ![Información de dominio y direcciones URL de inicio de sesión único de iProva](common/sp-identifier-reply.png)

5. La aplicación iProva espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. En la página **Set-up Single Sign-On with SAML** (Configurar inicio de sesión único con SAML), seleccione el icono **Edit** (Editar) para abrir el cuadro de diálogo **Atributos de usuario**.

    ![Cuadro de diálogo Atributos de usuario](common/edit-attribute.png)

6. En la sección **Notificaciones de usuario** del cuadro de diálogo **Atributos de usuario**, configure el atributo token SAML como muestra la imagen anterior. Siga estos pasos.

    | NOMBRE | Atributo de origen| Espacio de nombres |
    | ---------------| -------- | -----|
    | `samaccountname` | `user.onpremisessamaccountname`| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims`|
    | | |

     a. Seleccione **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![Notificaciones de usuario](common/new-save-attribute.png)

    ![Cuadro de diálogo Administrar las notificaciones del usuario](common/new-attribute-details.png)

    b. En el cuadro **Nombre**, escriba el nombre de atributo que se muestra para la fila.

    c. En la lista **Espacio de nombres**, escriba el valor del espacio de nombres que se muestra para esa fila.

    d. Seleccione la opción **Origen** como **Atributo**.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Seleccione **Aceptar**.

    g. Seleccione **Guardar**.

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, seleccione el icono **Copiar** para copiar la **Dirección URL de metadatos de federación de aplicación** y guárdela en su equipo.

    ![Vínculo de descarga del certificado](common/copy-metadataurl.png)

### <a name="configure-iprova-single-sign-on"></a>Configuración del inicio de sesión único de iProva

1. Inicie sesión en iProva con la cuenta de **Administrador**.

2. Abra el menú **Ir a**.

3. Seleccione **Administración de aplicaciones**.

4. Seleccione **General** en el panel **Configuración del sistema**.

5. Seleccione **Editar**.

6. Desplácese hacia abajo hasta **Control de acceso**.

    ![Configuración del control de acceso de iProva](media/iprova-tutorial/iprova-accesscontrol.png)

7. Busque la opción **Users are automatically logged on with their network accounts** (Los usuarios inician sesión automáticamente con sus cuentas de red) y cámbiela a **Yes, authentication via SAML** (Sí, autenticación vía SAML). Ahora aparecen otras opciones.

8. Seleccione **Configurar**.

9. Seleccione **Next** (Siguiente).

10. iProva le pregunta si quiere descargar datos de federación desde una dirección URL o cargarlos desde un archivo. Seleccione la opción **From URL** (Desde dirección URL).

    ![Descarga de metadatos de Azure AD](media/iprova-tutorial/iprova-download-metadata.png)

11. Pegue la dirección URL de metadatos que guardó en el último paso de la sección "Configuración del inicio de sesión único de Azure AD".

12. Seleccione el botón con forma de flecha para descargar los metadatos desde Azure AD.

13. Cuando finaliza la descarga, aparece el mensaje de confirmación **Valid Federation Data file downloaded** (Se descargó un archivo de datos de federación válido).

14. Seleccione **Next** (Siguiente).

15. Omita por ahora la opción **Test login** (Probar inicio de sesión) y seleccione **Siguiente**.

16. En el cuadro desplegable **Claim to use** (Notificación que usar), seleccione **windowsaccountname**.

17. Seleccione **Finalizar**.

18. Ahora vuelva a la pantalla **Edit general settings** (Editar la configuración general). Desplácese hasta el final de la página y seleccione **Aceptar** para guardar la configuración.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, creará un usuario de prueba en Azure Portal llamado Britta Simon.

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory** > **Usuarios** > **Todos los usuarios**.

    ![Vínculos Usuarios, grupos y Todos los usuarios](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En el cuadro de diálogo **Usuario**, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

     a. En el campo **Nombre**, escriba un nombre como **BrittaSimon**.
  
    b. En el cuadro **Nombre de usuario**, escriba *yourname@yourcompanydomain.extension*. 
    Un ejemplo es BrittaSimon@contoso.com.

    c. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Seleccione **Crear**.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a iProva.

1. En Azure Portal, seleccione **Aplicaciones empresariales** > **Todas las aplicaciones** > **iProva**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **iProva**.

    ![Vínculo a iProva en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo Usuarios y grupos](common/users-groups-blade.png)

4. Seleccione **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![El cuadro de diálogo Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista  **Usuarios** y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera algún valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione el rol adecuado para el usuario en la lista. Elija **Seleccionar** en la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, seleccione **Asignar**.

### <a name="create-an-iprova-test-user"></a>Creación de un usuario de prueba para iProva

1. Inicie sesión en iProva con la cuenta de **Administrador**.

2. Abra el menú **Ir a**.

3. Seleccione **Administración de aplicaciones**.

4. Seleccione **Usuarios** en el panel **Usuarios y grupos de usuarios**.

5. Seleccione **Agregar**.

6. En el cuadro **Nombre de usuario**, escriba *brittasimon@yourcompanydomain.extension*. 
    Un ejemplo es BrittaSimon@contoso.com.

7. En el cuadro **Nombre completo**, escriba el nombre completo, como **BrittaSimon**.

8. Seleccione la opción **No password (use single sign-on)** (Sin contraseña [ usar inicio de sesión único]).

9. En el cuadro **Dirección de correo electrónico**, escriba *yourname@yourcompanydomain.extension* . 
   Un ejemplo es BrittaSimon@contoso.com.

10. Desplácese al final de la página y seleccione **Finalizar**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al seleccionar el icono de iProva en el panel de acceso, debería iniciar sesión automáticamente en la versión de iProva para la que configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)
- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)
- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
- [iProva: cómo configurar el inicio de sesión único de SAML2](https://webshare.iprova.nl/0wqwm45yn09f5poh/Document.aspx)
