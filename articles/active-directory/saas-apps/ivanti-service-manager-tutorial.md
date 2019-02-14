---
title: 'Tutorial: Integración de Azure Active Directory con Ivanti Service Manager (ISM) | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory e Ivanti Service Manager (ISM).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 14297c74-0d57-4146-97fa-7a055fb73057
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/20/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c8859c0363a738c744f5c9b496cf77f5aaf1ba61
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56203512"
---
# <a name="tutorial-azure-active-directory-integration-with-ivanti-service-manager-ism"></a>Tutorial: Integración de Azure Active Directory con Ivanti Service Manager (ISM)

En este tutorial, aprenderá a integrar Ivanti Service Manager (ISM) con Azure Active Directory (Azure AD).

La integración de Ivanti Service Manager (ISM) con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a Ivanti Service Manager (ISM).
- Puede permitir que los usuarios inicien sesión automáticamente en Ivanti Service Manager (ISM) (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Ivanti Service Manager (ISM), necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en Ivanti Service Manager (ISM)

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Ivanti Service Manager (ISM) desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-ivanti-service-manager-ism-from-the-gallery"></a>Incorporación de Ivanti Service Manager (ISM) desde la galería

Para configurar la integración de Ivanti Service Manager (ISM) en Azure AD, será preciso que lo agregue desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Ivanti Service Manager (ISM) desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Ivanti Service Manager (ISM)**, seleccione **Ivanti Service Manager (ISM)** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Ivanti Service Manager (ISM) en la lista de resultados](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Ivanti Service Manager (ISM)utilizando un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Ivanti Service Manager (ISM) para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Ivanti Service Manager (ISM).

Para configurar y probar el inicio de sesión único de Azure AD con Ivanti Service Manager (ISM), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba en Ivanti Service Manager (ISM)](#creating-an-ivanti-service-manager-ism-test-user)**: para tener un homólogo de Britta Simon en Ivanti Service Manager (ISM) que esté vinculado a su representación en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#testing-single-sign-on)**: para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Ivanti Service Manager (ISM).

**Para configurar el inicio de sesión único de Azure AD con Ivanti Service Manager (ISM), siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Ivanti Service Manager (ISM)**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](common/tutorial-general-301.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](common/editconfigure.png)

4. En la sección **Configuración de SAML básica**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por **IDP**:

    ![Información de direcciones URL de inicio de sesión único y dominio de Ivanti Service Manager (ISM)](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://<customer>.saasit.com/` |
    | `https://<customer>.saasiteu.com/` |
    | `https://<customer>.saasitau.com/` |

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<customer>/handlers/sso/SamlAssertionConsumerHandler.ashx`.

5. Haga clic en **Establecer direcciones URL adicionales** y siga este paso si desea configurar la aplicación en el modo iniciado por **SP**:

    ![Información de direcciones URL de inicio de sesión único y dominio de Ivanti Service Manager (ISM)](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-url1.png)

    En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<customer>.saasit.com/`.

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con los valores reales de Identificador, URL de respuesta y URL de inicio de sesión. Póngase en contacto con el [equipo de soporte de cliente de Ivanti Service Manager (ISM)](https://www.ivanti.com/support/contact) para obtener estos valores.

6. En la página **Certificado de firma de SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **certificado (sin procesar)** y, luego, guarde el archivo de certificado en el equipo.

    ![Vínculo de descarga del certificado](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-certificate.png) 

7. En la sección **Configurar Ivanti Service Manager (ISM)**, copie la dirección URL adecuada según sus necesidades.

     a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

    ![Configuración de Ivanti Service Manager (ISM)](common/configuresection.png)

8. Para configurar el inicio de sesión único en **Ivanti Service Manager (ISM)**, es preciso enviar el **certificado (Raw)** descargado y los valores de **URL de inicio de sesión**, **identificador de Azure AD** y **URL de cierre de sesión** copiados al [equipo de soporte técnico de Ivanti Service Manager (ISM)](https://www.ivanti.com/support/contact). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](common/create-aaduser-01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](common/create-aaduser-02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**.  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="creating-an-ivanti-service-manager-ism-test-user"></a>Creación de un usuario de prueba de Ivanti Service Manager (ISM)

El objetivo de esta sección es crear un usuario llamado Britta Simon en Ivanti Service Manager (ISM). Ivanti Service Manager (ISM) admite el aprovisionamiento Just-In-Time, que está habilitado de manera predeterminada. No hay ningún elemento de acción para usted en esta sección. Durante un intento de acceder a Ivanti Service Manager (ISM), se crea un usuario, en caso de que no exista.
>[!Note]
>Si necesita crear manualmente un usuario, póngase en contacto con el  [equipo de soporte técnico de Ivanti Service Manager (ISM)](https://www.ivanti.com/support/contact).

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, se habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Ivanti Service Manager (ISM).

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **Ivanti Service Manager (ISM)**.

    ![Configurar inicio de sesión único](./media/ivanti-service-manager-tutorial/tutorial-ivanti-service-manager-app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Ivanti Service Manager (ISM) en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Ivanti Service Manager (ISM).
Para más información sobre el Panel de acceso, consulte la [introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: common/tutorial-general-01.png
[2]: common/tutorial-general-02.png
[3]: common/tutorial-general-03.png
[4]: common/tutorial-general-04.png

[100]: common/tutorial-general-100.png

[201]: common/tutorial-general-201.png
[202]: common/tutorial-general-202.png
[203]: common/tutorial-general-203.png
