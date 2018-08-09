---
title: 'Tutorial: Integración de Azure Active Directory con SumoLogic | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SumoLogic.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: fbb76765-92d7-4801-9833-573b11b4d910
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: dbebf2605fb214a167a276ec8dc344ff450ae5c0
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39420077"
---
# <a name="tutorial-azure-active-directory-integration-with-sumologic"></a>Tutorial: Integración de Azure Active Directory con SumoLogic

En este tutorial, aprenderá cómo integrar SumoLogic con Azure Active Directory (Azure AD).

La integración de SumoLogic con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a SumoLogic
- Puede permitir que los usuarios inicien sesión automáticamente en SumoLogic (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SumoLogic, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en SumoLogic

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregación de SumoLogic desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-sumologic-from-the-gallery"></a>Agregación de SumoLogic desde la galería
Para configurar la integración de SumoLogic en Azure AD, deberá agregar SumoLogic desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SumoLogic desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **SumoLogic**.

    ![Creación de un usuario de prueba de Azure AD](./media/sumologic-tutorial/tutorial_sumologic_search.png)

1. En el panel de resultados, seleccione **SumoLogic** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/sumologic-tutorial/tutorial_sumologic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con SumoLogic con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de SumoLogic para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SumoLogic.

Para establecer la relación de vínculo, en SumoLogic, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con SumoLogic, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba para SumoLogic](#creating-a-sumologic-test-user)**: para tener un homólogo de Britta Simon en SumoLogic que esté vinculado a la representación de usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación SumoLogic.

**Para configurar el inicio de sesión único de Azure AD con SumoLogic, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **SumoLogic**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/sumologic-tutorial/tutorial_sumologic_samlbase.png)

1. En la sección **Dominio y direcciones URL de SumoLogic**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/sumologic-tutorial/tutorial_sumologic_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<tenantname>.SumoLogic.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://<tenantname>.us2.sumologic.com` |
    | `https://<tenantname>.sumologic.com` |
    | `https://<tenantname>.us4.sumologic.com` |
    | `https://<tenantname>.eu.sumologic.com` |
    | `https://<tenantname>.au.sumologic.com` |

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte al cliente de SumoLogic](https://www.sumologic.com/contact-us/) para obtener estos valores. 
 
1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/sumologic-tutorial/tutorial_sumologic_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/sumologic-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de SumoLogic**, haga clic en **Configurar SumoLogic** para abrir la ventana **Configurar inicio de sesión**. Copie los valores de **SAML Entity ID y SAML Single Sign-On Service URL** (Identificador de entidad de SAML y Dirección URL del servicio de inicio de sesión único de SAML) de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/sumologic-tutorial/tutorial_sumologic_configure.png) 

1. En otra ventana del explorador web, inicie sesión en su sitio de la compañía de SumoLogic como administrador.

1. Vaya a **Administrar \> Seguridad**.
   
    ![Administración](./media/sumologic-tutorial/ic778556.png "Administración")

1. Haga clic en **SAML**.
   
    ![Configuración de seguridad global](./media/sumologic-tutorial/ic778557.png "configuración de seguridad global")

1. En la lista **Seleccionar una configuración o crear una nueva**, seleccione **Azure AD** y, después, haga clic en **Configurar**.
   
    ![Configuración de SAML 2.0](./media/sumologic-tutorial/ic778558.png "Configuración de SAML 2.0")

1. En el cuadro de diálogo **Configurar SAML 2.0** , realice los pasos siguientes:
   
    ![Configuración de SAML 2.0](./media/sumologic-tutorial/ic778559.png "Configuración de SAML 2.0")
   
    a. En el cuadro de texto **Nombre de configuración**, escriba **Azure AD**. 

    b. Seleccione **Modo de depuración**.

    c. En el cuadro de texto **Issuer** (Emisor), pegue el valor de **SAML Entity ID** (Identificador de entidad de SAML) que ha copiado de Azure Portal. 

    d. En el cuadro de texto **Authn Request URL** (Dirección URL de solicitud de autenticación), pegue el valor de la **dirección URL del servicio de inicio de sesión único de SAML** que ha copiado de Azure Portal.

    e. Abra el certificado codificado en base 64 en el Bloc de notas, copie su contenido en el Portapapeles y luego pegue todo el certificado en el cuadro de texto **Certificado X.509** .

    f. Como **Atributo de correo electrónico**, seleccione **Usar SAML Subject**.  

    g. Seleccione **Configuración de inicio de sesión iniciada por el SP**.

    h. En el cuadro de texto de la **ruta de acceso de inicio de sesión**, escriba **Azure** y haga clic en **Guardar**.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/sumologic-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/sumologic-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/sumologic-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/sumologic-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-sumologic-test-user"></a>Creación de un usuario de prueba de SumoLogic

Para permitir que los usuarios de Azure AD inicien sesión en SumoLogic, deben aprovisionarse a SumoLogic.  

* En el caso de SumoLogic, el aprovisionamiento es una tarea manual.

**Para aprovisionar una cuenta de usuario, realice estos pasos:**

1. Inicie sesión en su inquilino de **SumoLogic** .

1. Vaya a **Administrar \> usuarios**.
   
    ![Usuarios](./media/sumologic-tutorial/ic778561.png "Usuarios")

1. Haga clic en **Agregar**.
   
    ![Usuarios](./media/sumologic-tutorial/ic778562.png "Usuarios")

1. En el cuadro de diálogo **Nuevo usuario** , realice los pasos siguientes:
   
    ![Nuevo usuario](./media/sumologic-tutorial/ic778563.png "nuevo usuario") 
 
    a. Escriba la información relacionada de la cuenta de Azure AD que quiere aprovisionar en los cuadros de texto **First Name** (Nombre), **Last Name** (Apellidos) y **Email** (Correo electrónico).
  
    b. Seleccione un rol.
  
    c. Como **Estado**, seleccione **Activo**.
  
    d. Haga clic en **Save**(Guardar).

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de SumoLogic ofrecida por SumoLogic para aprovisionar cuentas de usuario de AAD. 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a SumoLogic.

![Asignar usuario][200] 

**Para asignar a Britta Simon a SumoLogic, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **SumoLogic**.

    ![Configurar inicio de sesión único](./media/sumologic-tutorial/tutorial_sumologic_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

El objetivo de esta sección es probar la configuración del inicio de sesión único de Azure AD mediante el panel de acceso.

Al hacer clic en el icono de SumoLogic en el panel de acceso, debería iniciar sesión automáticamente en la aplicación SumoLogic.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sumologic-tutorial/tutorial_general_01.png
[2]: ./media/sumologic-tutorial/tutorial_general_02.png
[3]: ./media/sumologic-tutorial/tutorial_general_03.png
[4]: ./media/sumologic-tutorial/tutorial_general_04.png

[100]: ./media/sumologic-tutorial/tutorial_general_100.png

[200]: ./media/sumologic-tutorial/tutorial_general_200.png
[201]: ./media/sumologic-tutorial/tutorial_general_201.png
[202]: ./media/sumologic-tutorial/tutorial_general_202.png
[203]: ./media/sumologic-tutorial/tutorial_general_203.png

