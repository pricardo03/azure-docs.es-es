---
title: 'Tutorial: Integración de Azure Active Directory con moconavi | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y moconavi.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: e1916224-e1c2-426f-b233-0a2518fa41db
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: jeedes
ms.openlocfilehash: 3009cb42ac477b18d45ab5968d6f5793ce1cd36c
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55165904"
---
# <a name="tutorial-azure-active-directory-integration-with-moconavi"></a>Tutorial: Integración de Azure Active Directory con moconavi

En este tutorial obtendrá información sobre cómo integrar moconavi con Azure Active Directory (Azure AD).

La integración de moconavi con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a moconavi.
- Puede permitir que los usuarios inicien sesión automáticamente en moconavi (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con moconavi, necesita estos elementos:

- Una suscripción de Azure AD
- Una suscripción de moconavi habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de moconavi desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-moconavi-from-the-gallery"></a>Incorporación de moconavi desde la galería
Para configurar la integración de moconavi en Azure AD, será preciso que agregue moconavi desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar moconavi desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **moconavi**, seleccione **moconavi** en el panel de resultados y, luego, haga clic en el botón **Agregar** para agregar la aplicación.

    ![moconavi en la lista de resultados](./media/moconavi-tutorial/tutorial_moconavi_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con moconavi con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de moconavi para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de moconavi.

Para configurar y probar el inicio de sesión único de Azure AD con moconavi, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de moconavi](#create-a-moconavi-test-user)**: para tener un homólogo de Britta Simon en moconavi que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación moconavi.

**Para configurar el inicio de sesión único de Azure AD con moconavi, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **moconavi**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.

    ![Cuadro de diálogo Inicio de sesión único](./media/moconavi-tutorial/tutorial_moconavi_samlbase.png)

3. En la sección **Dominio y direcciones URL de moconavi**, siga estos pasos:

    ![Información sobre dominio y direcciones URL de inicio de sesión único de moconavi](./media/moconavi-tutorial/tutorial_moconavi_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<yourserverurl>/moconavi-saml2/saml/login`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<yourserverurl>/moconavi-saml2`

    C. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<yourserverurl>/moconavi-saml2/saml/SSO`.

    > [!NOTE]
    > Estos valores no son reales. Actualícelos con la dirección URL de inicio de sesión, el identificador y la dirección URL de respuesta reales. Póngase en contacto con el [equipo de soporte de cliente de moconavi](mailto:support@recomot.co.jp) para obtener estos valores.

4. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/moconavi-tutorial/tutorial_moconavi_certificate.png)

5. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/moconavi-tutorial/tutorial_general_400.png)

6. Para configurar el inicio de sesión único en **moconavi**, necesita enviar el archivo **XML de metadatos** descargado al [equipo de soporte técnico de moconavi](mailto:support@recomot.co.jp). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/moconavi-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/moconavi-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/moconavi-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/moconavi-tutorial/create_aaduser_04.png)

     a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-moconavi-test-user"></a>Creación de un usuario de prueba de moconavi

En esta sección creará un usuario llamado Britta Simon en moconavi. Trabaje con el  [equipo de soporte técnico de moconavi](mailto:support@recomot.co.jp)  para agregar los usuarios en la plataforma de moconavi. Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a moconavi.

![Asignación de rol de usuario][200]

**Para asignar el usuario Britta Simon a moconavi, haga lo siguiente:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. Seleccione **moconavi** en la lista de aplicaciones.

    ![Vínculo de moconavi en la lista de aplicaciones](./media/moconavi-tutorial/tutorial_moconavi_app.png)

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

1. Instale moconavi desde Microsoft Store.

2. Inicie moconavi.

3. Haga clic en el botón **Connect setting** (Conectar configuración).

    ![Prueba del inicio de sesión único ](./media/moconavi-tutorial/testing1.png)

4. Escriba `https://mcs-admin.moconavi.biz/gateway` en el cuadro **Connect to URL** (URL de conexión) y haga clic en el botón **Done** (Listo).

    ![Prueba del inicio de sesión único ](./media/moconavi-tutorial/testing2.png)

5. En la siguiente captura de pantalla, siga estos pasos:

    ![Prueba del inicio de sesión único ](./media/moconavi-tutorial/testing3.png)

     a. Escriba la **clave de autenticación** `azureAD` en el cuadro de texto **Input Authentication Key** (Indique la clave de autenticación).

    b. Escriba el **id. de usuario** `your ad account` en el cuadro de texto **Input User ID** (Indique el id. de usuario).

    c. Haga clic en **LOGIN** (INICIAR SESIÓN).

6. Indique su contraseña de Azure AD en el cuadro de texto **Password** (Contraseña) y haga clic en el botón **Login** (Iniciar sesión).

    ![Prueba del inicio de sesión único ](./media/moconavi-tutorial/testing4.png)

7. La autenticación de Azure AD se habrá realizado correctamente cuando se muestre el menú.

    ![Prueba del inicio de sesión único ](./media/moconavi-tutorial/testing5.png)

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/moconavi-tutorial/tutorial_general_01.png
[2]: ./media/moconavi-tutorial/tutorial_general_02.png
[3]: ./media/moconavi-tutorial/tutorial_general_03.png
[4]: ./media/moconavi-tutorial/tutorial_general_04.png

[100]: ./media/moconavi-tutorial/tutorial_general_100.png

[200]: ./media/moconavi-tutorial/tutorial_general_200.png
[201]: ./media/moconavi-tutorial/tutorial_general_201.png
[202]: ./media/moconavi-tutorial/tutorial_general_202.png
[203]: ./media/moconavi-tutorial/tutorial_general_203.png

