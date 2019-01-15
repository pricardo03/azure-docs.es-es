---
title: 'Tutorial: Integración de Azure Active Directory con Palo Alto Networks - Captive Portal | Microsoft Docs'
description: Obtenga más información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Palo Alto Networks - Captive Portal.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 67a0b476-2305-4157-8658-2ec3625850d5
ms.service: Azure-Active-Directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/25/2018
ms.author: jeedes
ms.openlocfilehash: eff08cc17f475e2b6ad6406e463de27371bbe5b1
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2019
ms.locfileid: "54064736"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---captive-portal"></a>Tutorial: Integración de Azure Active Directory con Palo Alto Networks - Captive Portal

En este tutorial obtendrá más información sobre cómo integrar Palo Alto Networks - Captive Portal con Azure Active Directory (Azure AD).
La integración de Palo Alto Networks - Captive Portal con Azure AD le proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Palo Alto Networks - Captive Portal.
* Puede permitir que los usuarios inicien sesión automáticamente en Palo Alto Networks - Captive Portal (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Palo Alto Networks - Captive Portal, se necesita lo siguiente:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/)
* Una suscripción con inicio de sesión único de Palo Alto Networks - Captive Portal

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Palo Alto Networks - Captive Portal admite el inicio de sesión único iniciado por **IDP**

* Palo Alto Networks - Captive Portal admite el aprovisionamiento de usuarios **Just-in-time**

## <a name="adding-palo-alto-networks---captive-portal-from-the-gallery"></a>Agregar Palo Alto Networks - Captive Portal desde la galería

Para configurar la integración de Palo Alto Networks - Captive Portal en Azure AD, es preciso agregar Palo Alto Networks - Captive Portal desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Palo Alto Networks - Captive Portal desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Palo Alto Networks - Captive Portal**, seleccione **Palo Alto Networks - Captive Portal** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

     ![Palo Alto Networks - Captive Portal en la lista de resultados](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con Palo Alto Networks - Captive Portal con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Palo Alto Networks - Captive Portal.

Para configurar y probar el inicio de sesión único de Azure AD con Palo Alto Networks - Captive Portal, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Palo Alto Networks - Captive Portal](#configure-palo-alto-networks---captive-portal-single-sign-on)**: permite configurar las opciones de inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación del usuario de prueba de Palo Alto Networks - Captive Portal](#create-palo-alto-networks---captive-portal-test-user)**: para tener un homólogo de Britta Simon en Palo Alto Networks - Captive Portal vinculado a la representación del usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Palo Alto Networks - Captive Portal, siga estos pasos:

1. En la página de integración de la aplicación [Palo Alto Networks - Captive Portal](https://portal.azure.com/) de **Azure Portal**, seleccione **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En el cuadro de diálogo **Configuración básica de SAML**, realice los siguientes pasos:

    ![Información de inicio de sesión único con el dominio y las direcciones URL de Palo Alto Networks - Captive Portal](common/idp-intiated.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el patrón siguiente: `https://<Customer Firewall Hostname>/SAML20/SP`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<Customer Firewall Hostname>/SAML20/SP/ACS`

    > [!NOTE]
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de clientes de Palo Alto Networks - Captive Portal](https://support.paloaltonetworks.com/support) para obtener estos valores. También puede hacer referencia a los patrones que se muestran en la sección **Configuración básica de SAML** de Azure Portal.

5. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

### <a name="configure-palo-alto-networks---captive-portal-single-sign-on"></a>Configuración del inicio de sesión único de Palo Alto Networks - Captive Portal

1. Abra el sitio de Palo Alto como administrador en otra ventana del explorador.

2. Haga clic en **Dispositivo**.

    ![Configuración del inicio de sesión único de Palo Alto](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin1.png)

3. Seleccione **Proveedor de identidades SAML** en la barra de navegación izquierda y haga clic en "Importar" para importar el archivo de metadatos.

    ![Configuración del inicio de sesión único de Palo Alto](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin2.png)

4. Realice las siguientes acciones en la ventana Importar:

    ![Configuración del inicio de sesión único de Palo Alto](./media/paloaltonetworks-captiveportal-tutorial/tutorial_paloaltoadmin_admin3.png)

     a. En el cuadro de texto **Nombre de perfil**, proporcione un nombre, por ejemplo, Azure AD Admin UI.
    
    b. En **Metadatos del proveedor de identidades**, haga clic en **Examinar** y seleccione el archivo metadata.xml que ha descargado de Azure Portal.
    
    c. Haga clic en **Aceptar**

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para usar el inicio de sesión único de Azure al concederle acceso a Palo Alto Networks - Captive Portal.

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Palo Alto Networks - Captive Portal**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, escriba y seleccione **Palo Alto Networks - Captive Portal**.

    ![Vínculo a Palo Alto Networks - Captive Portal en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. Si espera cualquier valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol** seleccione en la lista el rol adecuado para el usuario y, después, haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.

7. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-palo-alto-networks---captive-portal-test-user"></a>Creación del usuario de prueba de Palo Alto Networks - Captive Portal

En esta sección, se va a crear un usuario llamado Britta Simon en Palo Alto Networks - Captive Portal. Palo Alto Networks - Captive Portal admite el **aprovisionamiento de usuarios Just-In-Time**, que está habilitado de forma predeterminada. No hay ningún elemento de acción para usted en esta sección. Si un usuario no existe aún en Palo Alto Networks - Captive Portal, se crea uno después de la autenticación.

> [!NOTE]
> Si necesita crear un usuario manualmente, póngase en contacto con el [equipo de soporte técnico de clientes de Palo Alto Networks - Captive Portal](https://support.paloaltonetworks.com/support).

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

La configuración de Captive Portal se realiza tras el firewall de la máquina virtual de Windows. Para probar el inicio de sesión único en Captive Portal, inicie sesión en la máquina virtual de Windows mediante RDP. En la sesión RDP, abra un explorador para acceder a cualquier sitio web. El explorador abrirá automáticamente la dirección URL de inicio de sesión único (SSO) y solicitará la autenticación. Una vez completada la autenticación, podrá navegar por los sitios web.

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales acerca de cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a las aplicaciones y el inicio de sesión único con Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

