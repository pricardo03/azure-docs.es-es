---
title: 'Tutorial: Integración de Azure Active Directory con Cisco Webex | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Cisco Webex.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 26704ca7-13ed-4261-bf24-fd6252e2072b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2018
ms.author: jeedes
ms.openlocfilehash: 73e20afdcacec76482f8ebf01bf2cef2105912a6
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "40005532"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-webex"></a>Tutorial: Integración de Azure Active Directory con Cisco Webex

En este tutorial, aprenderá a integrar Cisco Webex con Azure Active Directory (Azure AD).

Integrar Cisco Webex con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Cisco Webex.
- Puede permitir que los usuarios inicien sesión automáticamente en Cisco Webex con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para obtener más detalles sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Cisco Webex, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Cisco Webex

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, siga estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba gratuita durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Cisco Webex desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="add-cisco-webex-from-the-gallery"></a>Adición de Cisco Webex desde la galería
Para configurar la integración de Cisco Webex en Azure AD, será preciso que agregue Cisco Webex desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Cisco Webex desde la galería, realice los pasos siguientes:**

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione el icono **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Cisco Webex**. 

5. Seleccione **Cisco Webex** en el panel de resultados. A continuación, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Cisco Webex en la lista de resultados](./media/cisco-webex-tutorial/tutorial_ciscowebex_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, va a configurar y probar el inicio de sesión único de Azure AD con Cisco Webex con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Cisco Webex para un usuario de Azure AD. Es decir, es necesario establecer un vínculo entre un usuario de Azure AD y el usuario relacionado de Cisco Webex.

En Cisco Webex, asigne a **Nombre de usuario** el mismo valor que definió en **nombre de usuario** en Azure AD. Ahora ya ha establecido el vínculo entre los dos usuarios. 

Para configurar y probar el inicio de sesión único de Azure AD con Cisco Webex, complete los siguientes bloques de creación:

1. [Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on), para permitir que los usuarios utilicen esta característica.
2. [Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con Britta Simon.
3. [Creación de un usuario de prueba de Cisco Webex](#create-a-cisco-webex-test-user), para tener un homólogo de Britta Simon en Cisco Webex que esté vinculado a la representación de ella en Azure AD.
4. [Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user), para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. [Prueba del inicio de sesión único](#test-single-sign-on) para comprobar que la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Cisco Webex.

**Para configurar el inicio de sesión único de Azure AD con Cisco Webex, siga estos pasos:**

1. En la página de integración de la aplicación **Cisco Webex** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en la lista desplegable **Modo**, seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/cisco-webex-tutorial/tutorial_ciscowebex_samlbase.png)

3. En otra ventana del explorador web, inicie sesión como administrador en el sitio de la compañía Cisco Webex.

4. Haga clic en **Settings** (Configuración) a la izquierda del menú.

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_10.png)

5. En la página de configuración, desplácese hacia abajo a la sección **Authentication** (Autenticación) y haga clic en **Modify** (Modificar).

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_14.png)

6. Seleccione **Integrate a 3rd-party identity provider. (Advanced)** (Integrar un proveedor de identidades de terceros [avanzado]).y vaya a la pantalla siguiente.

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_15.png)

7. En la página **Export Directory Metadata** (Exportar metadatos de directorio), haga clic en **Download Metadata File** (Descargar archivo de metadatos) para descargar el archivo de metadatos.

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_16.png)

8. En Azure Portal, en la sección **Dominio y direcciones URL de Cisco Webex**, cargue el **archivo de metadatos del proveedor de servicios** que descargó y configure la aplicación realizando estos pasos:

    a. Haga clic en **Cargar el archivo de metadatos**.

    ![Información de inicio de sesión único de dominio y direcciones URL de Cisco Webex](./media/cisco-webex-tutorial/tutorial_ciscowebex_upload.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![Información de inicio de sesión único de dominio y direcciones URL de Cisco Webex](./media/cisco-webex-tutorial/tutorial_ciscowebex_uploadconfig.png)

    c. Cuando el **archivo de metadatos del proveedor de servicios** se haya cargado correctamente, los valores **Identificador** y **URL de respuesta** se rellenan automáticamente en el cuadro de texto de la sección **Dominio y direcciones URL de Cisco Webex**, como se muestra a continuación:

    ![Información de inicio de sesión único de dominio y direcciones URL de Cisco Webex](./media/cisco-webex-tutorial/tutorial_ciscowebex_url.png)

    d. En el cuadro **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<SUBDOMAIN>.webex.com/`
     
    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con la dirección URL de inicio de sesión real. Póngase en contacto con el [equipo de soporte técnico para clientes de Cisco Webex](https://www.webex.co.in/support/support-overview.html) para obtener estos valores.

9. La aplicación de Cisco Webex espera que las aserciones SAML contengan atributos específicos. Configure los siguientes atributos para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.
    
    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_ciscowebex_07.png) 

10. En la sección **Atributos de usuario** del cuadro de diálogo **Inicio de sesión único**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:
    
    |  Nombre del atributo  | Valor de atributo |
    | --------------- | -------------------- |    
    |   firstname    | user.givenname |
    |   lastname    | user.surname |
    |   uid    | user.mail |

    a. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_attribute_04.png)

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_attribute_05.png)
    
    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.
    
    c. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.
    
    d. Haga clic en **Aceptar**.

11. En la sección **Certificado de firma de SAML**, seleccione **XML de metadatos** y, a continuación, guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/cisco-webex-tutorial/tutorial_ciscowebex_certificate.png) 

12. Seleccione **Guardar**.

    ![Configuración del botón Guardar del inicio de sesión único](./media/cisco-webex-tutorial/tutorial_general_400.png)
    
13. En la página de administrador del sitio de la compañía Cisco Webex, use la opción del explorador de archivos para localizar y cargar el archivo de metadatos de Azure AD. Después, seleccione **Require certificate signed by a certificate authority in Metadata (more secure)** (Requerir certificado firmado por una entidad de certificación en metadatos [más seguro]) y vaya a la siguiente pantalla. 

    ![Configurar inicio de sesión único](./media/cisco-webex-tutorial/tutorial_cisco_webex_11.png)

14. Seleccione **Test SSO Connection** (Probar SSO de conexión) y cuando se abra una nueva pestaña de explorador, autentíquese con Azure AD mediante el inicio de sesión.

15. Vuelva a la pestaña del explorador de **Cisco Cloud Collaboration Management**. Si la prueba se realizó correctamente, seleccione **This test was successful. Enable Single Sign-On option** (Esta prueba se realizó correctamente. Habilite la opción de inicio de sesión único) y haga clic en **Save** (Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, seleccione el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/cisco-webex-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, seleccione **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/cisco-webex-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, seleccione **Agregar** para abrir el cuadro de diálogo **Usuario**.

    ![Botón Agregar](./media/cisco-webex-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario**, realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/cisco-webex-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Seleccione **Crear**.
 
### <a name="create-a-cisco-webex-test-user"></a>Creación de un usuario de prueba de Cisco Webex

El objetivo de esta sección es crear un usuario llamado Britta Simon en Cisco Webex. Cisco Webex admite el aprovisionamiento Just-In-Time y el aprovisionamiento automático de usuarios, el cual está habilitado de forma predeterminada. [Aquí](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, para permitir a Britta Simon usar el inicio de sesión único de Azure, le concederá acceso a Cisco Webex.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Cisco Webex, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones. Vaya a la vista de directorio y, a continuación, a **Aplicaciones empresariales**.  

2. Seleccione **Todas las aplicaciones**.

    ![Asignar usuario][201] 

3. En la lista de aplicaciones, seleccione **Cisco Webex**.

    ![Vínculo a Cisco Webex en la lista de aplicaciones](./media/cisco-webex-tutorial/tutorial_ciscowebex_app.png)  

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Seleccione el botón **Agregar**. A continuación, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista **Usuarios**.

6. En el cuadro de diálogo **Usuarios y grupos**, haga clic en el botón **Seleccionar**.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el panel de acceso.

Al seleccionar el icono de Cisco Webex en el panel de acceso, iniciará sesión automáticamente en la aplicación Cisco Webex.

Para más información sobre el panel de acceso, consulte [Introducción al panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/cisco-webex-tutorial/tutorial_general_01.png
[2]: ./media/cisco-webex-tutorial/tutorial_general_02.png
[3]: ./media/cisco-webex-tutorial/tutorial_general_03.png
[4]: ./media/cisco-webex-tutorial/tutorial_general_04.png

[100]: ./media/cisco-webex-tutorial/tutorial_general_100.png

[200]: ./media/cisco-webex-tutorial/tutorial_general_200.png
[201]: ./media/cisco-webex-tutorial/tutorial_general_201.png
[202]: ./media/cisco-webex-tutorial/tutorial_general_202.png
[203]: ./media/cisco-webex-tutorial/tutorial_general_203.png

