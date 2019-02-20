---
title: 'Tutorial: integración de Azure Active Directory con RingCentral | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y RingCentral.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e01b06b2031da20ee9349588bd4a7ee757fbc22c
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173307"
---
# <a name="tutorial-azure-active-directory-integration-with-ringcentral"></a>Tutorial: integración de Azure Active Directory con RingCentral

En este tutorial, aprenderá a integrar RingCentral con Azure Active Directory (Azure AD).

La integración de RingCentral con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a RingCentral.
- Puede permitir que los usuarios inicien sesión automáticamente en RingCentral (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con RingCentral, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción de RingCentral habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar RingCentral desde la Galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-ringcentral-from-the-gallery"></a>Agregar RingCentral desde la Galería
Para configurar la integración de RingCentral en Azure AD, será preciso que agregue RingCentral desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar RingCentral desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/ringcentral-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/ringcentral-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/ringcentral-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **RingCentral**, seleccione **RingCentral** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/ringcentral-tutorial/a_add_app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con RingCentral con un usuario de prueba llamado “Britta Simon”.

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de RingCentral para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de RingCentral.

Para configurar y probar el inicio de sesión único de Azure AD con RingCentral, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de RingCentral](#create-a-ringcentral-test-user)**: para tener un homólogo de Britta Simon en RingCentral que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación RingCentral.

**Para configurar el inicio de sesión único de Azure AD con RingCentral, siga estos pasos:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **RingCentral**, seleccione **Inicio de sesión único**.

    ![imagen](./media/ringcentral-tutorial/b1_b2_select_sso.png)

2. Haga clic en **Cambiar el modo de inicio de sesión único** en la parte superior de la pantalla para seleccionar el modo **SAML**.

      ![imagen](./media/ringcentral-tutorial/b1_b2_saml_ssso.png)

3. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/ringcentral-tutorial/b1_b2_saml_sso.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/ringcentral-tutorial/b1-domains_and_urlsedit.png)

5. En la sección **Configuración básica de SAML**, si tiene el **archivo de metadatos del proveedor de servicios**, lleve a cabo los siguientes pasos:

     a. Haga clic en **Cargar el archivo de metadatos**.

    ![imagen](./media/ringcentral-tutorial/b9_saml.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![imagen](./media/ringcentral-tutorial/b9(1)_saml.png)

    c. Una vez que se haya cargado correctamente el archivo de metadatos, los valores **Identificador** y **Dirección URL de respuesta** se rellenan automáticamente en el cuadro de texto de la sección **Configuración básica de SAML**, como se muestra a continuación:

    ![imagen](./media/ringcentral-tutorial/b21-domains_and_urls.png)

    d. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL:
    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    > [!NOTE]
    > Obtendrá el **archivo de metadatos del proveedor de servicios** en la página Configuración de inicio de sesión único de RingCentral que se explica más adelante en el tutorial.

6. Si no dispone del **archivo de metadatos del proveedor de servicios**, lleve a cabo los pasos siguientes:

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL:
    | |
    |--|
    | `https://service.ringcentral.com` |
    | `https://service.ringcentral.com.au` |
    | `https://service.ringcentral.co.uk` |
    | `https://service.ringcentral.eu` |

    b. En el cuadro de texto **Identificador**, escriba una dirección URL:
    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    c. En el cuadro de texto **URL de respuesta**, escriba una dirección URL:
    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

    ![imagen](./media/ringcentral-tutorial/b2-domains_and_urls.png)

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el certificado de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![imagen](./media/ringcentral-tutorial/certificatebase64.png)
    
8. En otra ventana del explorador web, inicie sesión en RingCentral como administrador de seguridad.

9. En la parte superior, haga clic en **Herramientas**.

    ![imagen](./media/ringcentral-tutorial/ringcentral1.png)

10. Vaya a **Inicio de sesión único**.

    ![imagen](./media/ringcentral-tutorial/ringcentral2.png)

11. En la página **Inicio de sesión único**, en la sección **Configuración de inicio de sesión único**, desde **Paso 1** haga clic en **Editar** y realice los siguientes pasos:

    ![imagen](./media/ringcentral-tutorial/ringcentral3.png)

12. En la página **Configurar inicio de sesión único** realice los siguientes pasos:

    ![imagen](./media/ringcentral-tutorial/ringcentral4.png)

     a. Haga clic en **Examinar** para cargar el archivo de metadatos que descargó de Azure Portal.

    b. Después de cargar los metadatos los valores se rellenan automáticamente en la sección **Información general de inicio de sesión único**.

    c. En la sección **Asignación de atributos**, seleccione **Asignar atributo de correo electrónico a** como `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. Haga clic en **Save**(Guardar).

    e. En **Paso 2** haga clic en **Descargar** para descargar el **archivo de metadatos del proveedor de servicios** y cargarlo en la sección **Configuración básica de SAML** para rellenar de forma automática los valores **Identificador** y **Dirección URL de respuesta** en Azure Portal.

    ![imagen](./media/ringcentral-tutorial/ringcentral6.png) 

    f. En la misma página, vaya a la sección **Habilitar Inicio de sesión único** y realice los siguientes pasos:

    ![imagen](./media/ringcentral-tutorial/ringcentral5.png)

     a. Seleccione **Habilitar inicio de sesión único**.
    
    b. Seleccione **Permitir a los usuarios iniciar sesión con credenciales de inicio de sesión único o de RingCentral**.

    c. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/ringcentral-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/ringcentral-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/ringcentral-tutorial/d_userproperties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-a-ringcentral-test-user"></a>Crear un usuario de prueba de RingCentral

En esta sección, creará un usuario llamado Britta Simon en RingCentral. Colabore con el  [equipo de soporte técnico para el cliente de RingCentral](https://success.ringcentral.com/RCContactSupp) para agregar usuarios en la plataforma de RingCentral. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a RingCentral.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/ringcentral-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **RingCentral**.

    ![imagen](./media/ringcentral-tutorial/d_all_proapplications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/ringcentral-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/ringcentral-tutorial/d_assign_user.png)

4. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

5. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de RingCentral en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación RingCentral.
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/ringcentral-tutorial/tutorial_general_01.png
[2]: ./media/ringcentral-tutorial/tutorial_general_02.png
[3]: ./media/ringcentral-tutorial/tutorial_general_03.png
[4]: ./media/ringcentral-tutorial/tutorial_general_04.png

[100]: ./media/ringcentral-tutorial/tutorial_general_100.png

[200]: ./media/ringcentral-tutorial/tutorial_general_200.png
[201]: ./media/ringcentral-tutorial/tutorial_general_201.png
[202]: ./media/ringcentral-tutorial/tutorial_general_202.png
[203]: ./media/ringcentral-tutorial/tutorial_general_203.png

