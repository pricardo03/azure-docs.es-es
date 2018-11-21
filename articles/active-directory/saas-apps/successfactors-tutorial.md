---
title: 'Tutorial: integración de Azure Active Directory con SuccessFactors | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SuccessFactors.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 32bd8898-c2d2-4aa7-8c46-f1f5c2aa05f1
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2018
ms.author: jeedes
ms.openlocfilehash: 89224b32efaecdf7a2797b034b1beac7ad191ee5
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685232"
---
# <a name="tutorial-azure-active-directory-integration-with-successfactors"></a>Tutorial: integración de Azure Active Directory con SuccessFactors

En este tutorial, aprenderá a integrar SuccessFactors con Azure Active Directory (Azure AD).

La integración de SuccessFactors con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a SuccessFactors.
- Puede permitir que los usuarios inicien sesión automáticamente en SuccessFactors (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SuccessFactors, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en SuccessFactors

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de SuccessFactors desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-successfactors-from-the-gallery"></a>Incorporación de SuccessFactors desde la galería

Para configurar la integración de SuccessFactors en Azure AD, deberá agregar SuccessFactors desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SuccessFactors desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **SuccessFactors**, seleccione **SuccessFactors** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![SuccessFactors en la lista de resultados](./media/successfactors-tutorial/tutorial_successfactors_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con SuccessFactors utilizando un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de SuccessFactors para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SuccessFactors.

Para configurar y probar el inicio de sesión único de Azure AD con SuccessFactors, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de SuccessFactors](#creating-a-successfactors-test-user)** : para tener un homólogo de Britta Simon en SuccessFactors que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación SuccessFactors.

**Para configurar el inicio de sesión único de Azure AD con SuccessFactors, realice los pasos siguientes:**

1. En la página de integración de la aplicación **SuccessFactors** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial_general_301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información de dominio y direcciones URL de inicio de sesión único de SuccessFactors](./media/successfactors-tutorial/tutorial_successfactors_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.eu`|

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente modelo:
    | |
    |--|
    | `https://www.successfactors.com/<companyname>`|
    | `https://www.successfactors.com`|
    | `https://<companyname>.successfactors.eu`|
    | `https://www.successfactors.eu/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://hcm4preview.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.eu`|
    | `https://www.successfactors.cn`|
    | `https://www.successfactors.cn/<companyname>`|

    c. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://<companyname>.successfactors.com/<companyname>`|
    | `https://<companyname>.successfactors.com`|
    | `https://<companyname>.sapsf.com/<companyname>`|
    | `https://<companyname>.sapsf.com`|
    | `https://<companyname>.successfactors.eu/<companyname>`|
    | `https://<companyname>.successfactors.eu`|
    | `https://<companyname>.sapsf.eu`|
    | `https://<companyname>.sapsf.eu/<companyname>`|
    | `https://<companyname>.sapsf.cn`|
    | `https://<companyname>.sapsf.cn/<companyname>`|
         
    > [!NOTE] 
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Para obtener estos valores, póngase en contacto con el [equipo de soporte técnico de SuccessFactors](https://www.successfactors.com/support.html). 

5. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic **Descargar** para descargar el **certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/successfactors-tutorial/tutorial_successfactors_certificate.png) 

6. En la sección **Configurar SuccessFactors**, copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de SuccessFactors](common/configuresection.png)

7. En otra ventana del explorador web, inicie sesión en el **Portal de administración de SuccessFactors** como administrador.
    
8. Visite **Application Security** (Seguridad de aplicaciones) y establezca nativo en **Single Sign On Features** (Características de Inicio de sesión único). 

9. Coloque cualquier valor en **Reset Token** (Restablecer Token) y haga clic en **Save Token** (Guardar Token) para habilitar SSO de SAML.
   
    ![Configuración del inicio de sesión único en la aplicación][11]

    > [!NOTE] 
    > Este valor se utiliza como el conmutador de activado y desactivado. Si se guarda algún valor, el SSO de SAML está activado. Si se guarda un valor en blanco, el SSO de SAML está desactivado.

10. Vaya a la siguiente captura de pantalla y realice las acciones siguientes:
   
    ![Configuración del inicio de sesión único en la aplicación][12]
   
     a. Seleccione el botón de selección **SAML v2 SSO** (SSO de SAML v2).
   
    b. Establezca el **nombre de entidad asertivo de SAML** (por ejemplo, emisor de SAML + nombre de la empresa).
   
    c. En el cuadro de texto **URL del emisor**, pegue el valor del **identificador de Azure AD** que ha copiado de Azure Portal.
   
    d. Seleccione **Assertion** (Aserción) como **Require Mandatory Signature** (Solicitar firma obligatoria).
   
    e. Seleccione **Enabled** (Habilitado) como **Enable SAML Flag** (Habilitar marca SAML).
   
    f. Seleccione **No** como **Login Request Signature(SF Generated/SP/RP)** [Firma de solicitud de inicio de sesión (SF generado/SP/RP)].
   
    g. Seleccione **Browser/Post Profile** (Perfil de explorador/envío) como **SAML Profile** (Perfil SAML).
   
    h. Seleccione **No** como **Enforce Certificate Valid Period** (Aplicar período válido de certificado).
   
    i. Copie el contenido del archivo de certificado descargado de Azure Portal y péguelo en el cuadro de texto **SAML Verifying Certificate** (Certificado de verificación de firma).

    > [!NOTE] 
    > El contenido del certificado debe tener etiquetas de inicio y fin del certificado.

11. Vaya a SAML V2 y realice los pasos siguientes:
   
    ![Configuración del inicio de sesión único en la aplicación][13]
   
     a. Seleccione **Yes** (Sí) como **Support SP-initiated Global Logout** (Permitir cierre de sesión global iniciado por SP).
   
    b. En el cuadro de texto **Global Logout Service URL (LogoutRequest destination)** (URL del servicio de cierre de sesión global (destino LogoutRequest)), pegue el valor **Sign-Out URL** (Dirección URL de cierre de sesión) que copió de Azure Portal.
   
    c. Seleccione **No** en **Require sp must encrypt all NameID element** (Requerir que sp cifre todos los elementos NameID).
   
    d. Seleccione **Unspecified** (Sin especificar) como **NameID Format** (Formato de NameID).
   
    e. Seleccione **Yes** (Sí) como **Enable sp initiated login (AuthnRequest)** [Permitir inicio de sesión iniciado por sp (AuthnRequest)].
   
    f. En el cuadro de texto **Send request as Company-Wide issuer** (Enviar solicitud como emisor para toda la compañía), pegue el valor de **SAML Single Sign-On Service URL** (URL de servicio de inicio de sesión único de SAML) que copió de Azure Portal.

12. Siga estos pasos si desea que los nombres de usuario de inicio de sesión no distingan mayúsculas de minúsculas.
   
    ![Configurar inicio de sesión único][29]
    
     a. Visite **Company Settings**(Configuración de la empresa) en la parte inferior.
   
    b. Seleccione la casilla junto a **Enable Non-Case-Sensitive Username**(Habilitar nombre de usuario sin distinción de mayúsculas y minúsculas).
   
    Haga clic en **Save**(Guardar).
   
    > [!NOTE] 
    > Si intenta habilitar esta opción, el sistema comprueba si creará un nombre de inicio de sesión de SAML duplicado. Por ejemplo, si el cliente tiene nombres de usuario User1 y user1. Al no distinguir mayúsculas de minúsculas, estos nombres pasan a ser duplicados. El sistema mostrará un mensaje de error y no se habilitará la característica. El cliente deberá cambiar uno de los nombres de usuario, para que esté escrito diferente.

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

### <a name="creating-a-successfactors-test-user"></a>Creación de un usuario de prueba de SuccessFactors

Para permitir que los usuarios de Azure AD inicien sesión en SuccessFactors, deben aprovisionarse en SuccessFactors.  
En el caso de SuccessFactors, el aprovisionamiento es una tarea manual.

Para que se creen los usuarios en SuccessFactors, deberá ponerse en contacto con el [equipo de soporte técnico de SuccessFactors](https://www.successfactors.com/support.html).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a SuccessFactors.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **SuccessFactors**.

    ![Configurar inicio de sesión único](./media/successfactors-tutorial/tutorial_successfactors_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SuccessFactors en el panel de acceso, debería iniciar sesión automáticamente en la aplicación SuccessFactors.
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
[11]: ./media/successfactors-tutorial/tutorial_successfactors_07.png
[12]: ./media/successfactors-tutorial/tutorial_successfactors_08.png
[13]: ./media/successfactors-tutorial/tutorial_successfactors_09.png
[29]: ./media/successfactors-tutorial/tutorial_successfactors_10.png
