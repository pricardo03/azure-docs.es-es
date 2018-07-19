---
title: 'Tutorial: Integración de Azure Active Directory con Adobe Sign | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Adobe Sign.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: f9385723-8fe7-4340-8afb-1508dac3e92b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2018
ms.author: jeedes
ms.openlocfilehash: d5cdc2ec0c6cfcf52f84629485d0dd879fbf6fa2
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054005"
---
# <a name="tutorial-azure-active-directory-integration-with-adobe-sign"></a>Tutorial: integración de Azure Active Directory con Adobe Sign

En este tutorial, obtendrá información sobre cómo integrar Adobe Sign con Azure Active Directory (Azure AD).

La integración de Adobe Sign con Azure AD proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a Adobe Sign.
- Puede permitir que los usuarios inicien sesión automáticamente en Adobe Sign (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Para obtener más detalles sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Adobe Sign, se necesita:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en Adobe Sign

Para probar los pasos de este tutorial, siga estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Adobe Sign desde la galería.
2. Configuración y prueba del inicio de sesión único de Azure AD

## <a name="add-adobe-sign-from-the-gallery"></a>Adición de Adobe Sign desde la galería
Para configurar la integración de Adobe Sign en Azure AD, será preciso que agregue Adobe Sign desde la galería a la lista de aplicaciones SaaS administradas.

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione el icono **Azure Active Directory**. 

    ![Captura de pantalla del icono de Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales** > **Todas las aplicaciones**.

    ![Captura de pantalla de los menús de Azure Active Directory, con Aplicaciones empresariales y Todas las aplicaciones resaltados][2]
    
3. Para agregar una aplicación nueva, en la parte superior del cuadro de diálogo, seleccione **Nueva aplicación**.

    ![Captura de pantalla de la opción Nueva aplicación en la parte superior del cuadro de diálogo][3]

4. En el cuadro de búsqueda, escriba **Adobe Sign**.

    ![Captura de pantalla del cuadro de búsqueda](./media/adobe-echosign-tutorial/tutorial_adobesign_search.png)

5. En el panel de resultados, seleccione **Adobe Sign** y, a continuación, seleccione **Agregar**.

    ![Captura de pantalla del panel de resultados](./media/adobe-echosign-tutorial/tutorial_adobesign_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD
En esta sección, configurará y probará el inicio de sesión único de Azure AD con Adobe Sign con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD necesita reconocer una relación vinculada entre un usuario de Azure AD y el usuario relacionado en Adobe Sign.

Para establecer la relación vinculada, en Adobe Sign, asigne el valor del **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Adobe Sign, complete los siguientes bloques de creación:

1. [Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on), para permitir que los usuarios utilicen esta característica.
2. [Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user), para probar el inicio de sesión único de Azure AD con Britta Simon.
3. [Creación de un usuario de prueba de Adobe Sign](#creating-an-adobe-sign-test-user): para tener un homólogo de Britta Simon en Adobe Sign que esté vinculado a la representación del usuario en Azure AD.
4. [Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user), para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. [Prueba del inicio de sesión único](#testing-single-sign-on), para comprobar si funciona la configuración.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Adobe Sign.

1. En Azure Portal, en la página de integración de la aplicación **Adobe Sign**, seleccione **Inicio de sesión único**.

    ![Captura de pantalla de la página de integración de la aplicación de Adobe Sign, con el Inicio de sesión único resaltado][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo**, seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Captura de pantalla del cuadro de diálogo Inicio de sesión único, con el cuadro Modo resaltado](./media/adobe-echosign-tutorial/tutorial_adobesign_samlbase.png)

3. En la sección **Dominio y direcciones URL de Adobe Sign**, lleve a cabo los pasos siguientes:

    ![Captura de pantalla de la sección Dominio y direcciones URL](./media/adobe-echosign-tutorial/tutorial_adobesign_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.echosign.com/`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.echosign.com`.

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL de inicio de sesión y el identificador reales. Póngase en contacto con el [equipo de soporte al cliente de Adobe Sign](https://helpx.adobe.com/in/contact/support.html) para obtener estos valores.

4. En la sección **Certificado de firma de SAML**, seleccione **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Captura de pantalla de la sección Certificado de firma de SAML](./media/adobe-echosign-tutorial/tutorial_adobesign_certificate.png) 

5. Seleccione **Guardar**.

    ![Captura de pantalla del botón Guardar](./media/adobe-echosign-tutorial/tutorial_general_400.png)

6. En la sección **Configuración de Adobe Sign**, seleccione **Configurar Adobe Sign** para abrir la ventana **Configurar inicio de sesión**. Copie la **Dirección URL de cierre de sesión**, el **Identificador de entidad de SAML** y la **Dirección URL del servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Captura de pantalla de la sección Configuración de Adobe Sign, con Configurar Adobe Sign resaltado](./media/adobe-echosign-tutorial/tutorial_adobesign_configure.png)

7. Antes de la configuración, póngase en contacto con el [equipo de soporte técnico al cliente de Adobe Sign](https://helpx.adobe.com/in/contact/support.html) para incluir su dominio en la lista blanca de Adobe Sign. Aquí se muestra cómo agregar el dominio:

    a. El [equipo de soporte técnico al cliente de Adobe Sign](https://helpx.adobe.com/in/contact/support.html) le envía un token generado aleatoriamente. Para el dominio, el token será similar a: **adobe-sign-verification= xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx**

    b. Publique el token de comprobación en un registro de texto DNS y notifique al [equipo de soporte técnico al cliente de Adobe Sign](https://helpx.adobe.com/in/contact/support.html).
    
    > [!NOTE]
    > Esto puede tardar unos cuantos días, o más. Tenga en cuenta que los retrasos de propagación de DNS significan que un valor que se publica en DNS puede no estar visible durante una hora o más. Su administrador de TI debe tener conocimientos sobre cómo publicar este token en un registro de texto DNS.
    
    c. Cuando notifique al [equipo de soporte técnico al cliente de Adobe Sign](https://helpx.adobe.com/in/contact/support.html) a través del vale de soporte, después de publicado el token, se valida el dominio y se agrega a su cuenta.
    
    d. Por lo general, estos son los pasos necesarios para publicar el token en un registro DNS:

    * Inicie sesión en su cuenta de dominio
    * Busque la página para actualizar el registro de DNS. Esta página se puede llamar Administración de DNS, Administración del servidor de nombres o Configuración avanzada.
    * Busque los registros TXT para el dominio.
    * Agregue un registro TXT con el valor completo del token proporcionado por Adobe.
    * Guarde los cambios.

8. En otra ventana del explorador web, inicie sesión en el sitio de la compañía de Adobe Sign como administrador.

9. En el menú SAML, seleccione **Configuración de cuenta** > **Configuración de SAML**.
   
    ![Captura de pantalla de Configuración de SAML de Adobe Sign](./media/adobe-echosign-tutorial/ic789520.png "Cuenta")

10. En la sección **SAML Settings** (Configuración de SAML), realice los pasos siguientes:
  
    ![Captura de pantalla de Configuración de SAML](./media/adobe-echosign-tutorial/ic789521.png "Configuración de SAML")
   
    a. En **SAML Mode** (Modo de SAML), seleccione **SAML Mandatory** (SAML obligatorio).
   
    b. Seleccione **Allow EchoSign Account Administrators to log in using their EchoSign Credentials**(Permitir a los administradores de cuentas de EchoSign iniciar sesión con sus credenciales de EchoSign).
   
    c. En **User Creation** (Creación de usuario), seleccione **Automatically add users authenticated through SAML** (Agregar automáticamente usuarios autenticados a través de SAML).

    d. Pegue el valor del **identificador de entidad de SAML** que ha copiado de Azure Portal, en el cuadro de texto **Entity ID/Issuer URL** (Identificador de entidad/Dirección URL del emisor).
    
    e. Pegue el valor de **Dirección URL del servicio de inicio de sesión único de SAML** que copió de Azure Portal en el cuadro de texto **Login URL/SSO Endpoint** (Dirección URL de inicio de sesión/Punto de conexión de SSO).
   
    f. Pegue el valor de **Dirección URL de cierre de sesión** que copió de Azure Portal en el cuadro de texto **Logout URL/SLO Endpoint** (Dirección URL de cierre de sesión/Punto de conexión de SLO).

    g. Abra el archivo **Certificado (Base64)** en el Bloc de notas. Copie el contenido del mismo en el Portapapeles y, después, péguelo en el cuadro de texto **IdP Certificate** (Certificado IDP).

    h. Seleccione **Save changes** (Guardar los cambios).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Azure Portal.

![Captura de pantalla de nombre de usuario de prueba en Azure Portal][100]

1. En el panel izquierdo de **Azure Portal**, seleccione el icono **Azure Active Directory**.

    ![Captura de pantalla del icono de Azure AD](./media/adobe-echosign-tutorial/create_aaduser_01.png) 

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y seleccione **Todos los usuarios**.
    
    ![Captura de pantalla de los menús de Azure AD con Usuarios y grupos y Todos los usuarios resaltado](./media/adobe-echosign-tutorial/create_aaduser_02.png) 

3. Para abrir el cuadro de diálogo **Usuario**, seleccione **Agregar**.
 
    ![Captura de pantalla de la parte superior del cuadro de diálogo Todos los usuarios, con la opción Agregar resaltada](./media/adobe-echosign-tutorial/create_aaduser_03.png) 

4. En el cuadro de diálogo **Usuario**, realice los pasos siguientes:
 
    ![Captura de pantalla del cuadro de diálogo Usuario](./media/adobe-echosign-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Seleccione **Crear**.
 
### <a name="create-an-adobe-sign-test-user"></a>Creación de un usuario de prueba de Adobe Sign

Para permitir que los usuarios de Azure AD inicien sesión en Adobe Sign, tienen que aprovisionarse en esta aplicación. Se trata de una tarea manual.

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Adobe Sign que esta le ofrezca para aprovisionar cuentas de usuario de Azure AD. 

1. Inicie sesión en el sitio de la compañía de **Adobe Sign** como administrador.

2. En el menú de la parte superior, seleccione **Account** (Cuenta). A continuación, en el panel izquierdo, seleccione **Users & Groups** (Usuarios y grupos) > **Create a new user** (Crear un usuario nuevo).
   
    ![Captura de pantalla del sitio empresarial de Adobe Sign, con Account (Cuenta), Users &Groups (Usuarios y grupos) y Create a new user (Crear nuevo usuario) resaltados](./media/adobe-echosign-tutorial/ic789524.png "Cuenta")
   
3. En la sección **Create New User** (Crear nuevo usuario), lleve a cabo estos pasos:
   
    ![Captura de pantalla de la sección Crear nuevo usuario](./media/adobe-echosign-tutorial/ic789525.png "Crear usuario")
   
    a. Escriba en los campos de texto pertinentes los datos de **Email Address** (Dirección de correo electrónico), **Name** (Nombre) y **Last Name** (Apellidos) de la cuenta de correo válida de Azure AD que desea aprovisionar.
   
    b. Seleccione **Create User** (Crear usuario).

>[!NOTE]
>El titular de la cuenta de Azure Active Directory recibe un mensaje de correo electrónico con un vínculo para confirmar la cuenta antes de que se active. 

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilita a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Adobe Sign.

![Captura de pantalla de opción Inicio de sesión único de Azure Portal][200] 

1. En Azure Portal, abra la vista de aplicaciones. Vaya a la vista de directorios, vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Captura de pantalla de la vista de aplicaciones de Azure Portal, con Aplicaciones empresariales y Todas las aplicaciones resaltados][201] 

2. En la lista de aplicaciones, seleccione **Adobe Sign**.

    ![Captura de pantalla de la lista de aplicaciones, con Adobe Sign resaltado](./media/adobe-echosign-tutorial/tutorial_adobesign_app.png) 

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Captura de pantalla de menú, con Usuarios y grupos resaltado][202] 

4. Seleccione **Agregar**. Después, en la sección **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Captura de pantalla de la página Usuarios y grupos y de la sección Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, en la lista Usuarios seleccione **Britta Simon**.

6. En el cuadro de diálogo **Usuarios y grupos**, haga clic en **Seleccionar**.

7. En el cuadro de diálogo **Agregar asignación**, seleccione **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

Al seleccionar el icono de Adobe Sign en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Adobe Sign. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/adobe-echosign-tutorial/tutorial_general_01.png
[2]: ./media/adobe-echosign-tutorial/tutorial_general_02.png
[3]: ./media/adobe-echosign-tutorial/tutorial_general_03.png
[4]: ./media/adobe-echosign-tutorial/tutorial_general_04.png

[100]: ./media/adobe-echosign-tutorial/tutorial_general_100.png

[200]: ./media/adobe-echosign-tutorial/tutorial_general_200.png
[201]: ./media/adobe-echosign-tutorial/tutorial_general_201.png
[202]: ./media/adobe-echosign-tutorial/tutorial_general_202.png
[203]: ./media/adobe-echosign-tutorial/tutorial_general_203.png
