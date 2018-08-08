---
title: 'Tutorial: Integración de Azure Active Directory con Spotinst | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Spotinst.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2f6dbd70-c2db-4ae9-99ee-976c3090d214
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.author: jeedes
ms.openlocfilehash: 7edf1cbc5cc351e25a9ae7b319768376ea9968a3
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39267537"
---
# <a name="tutorial-azure-active-directory-integration-with-spotinst"></a>Tutorial: Integración de Azure Active Directory con Spotinst

En este tutorial, aprenderá a integrar Spotinst con Azure Active Directory (Azure AD).

La integración de Spotinst con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Spotinst.
- Puede habilitar que los usuarios inicien sesión automáticamente en Spotinst (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Spotinst, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción de Spotinst habilitada para el inicio de sesión único

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Spotinst desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-spotinst-from-the-gallery"></a>Incorporación de Spotinst desde la galería
Para configurar la integración de Spotinst en Azure AD, es preciso agregar Spotinst desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Spotinst desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **Spotinst**, seleccione **Spotinst** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

    ![Spotinst en la lista de resultados](./media/spotinst-tutorial/tutorial_spotinst_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Spotinst con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Spotinst para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Spotinst.

Para configurar y probar el inicio de sesión único de Azure AD con Spotinst, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Spotinst](#create-a-spotinst-test-user)**: para tener un homólogo de Britta Simon en Spotinst que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Spotinst.

**Para configurar el inicio de sesión único de Azure AD con Spotinst, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Spotinst**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Cuadro de diálogo Inicio de sesión único](./media/spotinst-tutorial/tutorial_spotinst_samlbase.png)

3. En la sección **Dominio y direcciones URL de Spotinst**, realice los siguientes pasos si desea configurar la aplicación en el modo iniciado por IDP:

    ![Información de dominio y direcciones URL de inicio de sesión único de Spotinst](./media/spotinst-tutorial/tutorial_spotinst_url1.png)

    a. Active **Mostrar configuración avanzada de URL**.

    b. En el cuadro de texto **Estado de la retransmisión**, escriba un valor: `<ID>`

    c. Si quiere configurar la aplicación en modo iniciado por **SP**, en el cuadro de texto **URL de inicio de sesión**, escriba la dirección URL: `https://console.spotinst.com`

    > [!NOTE]
    > El valor del estado de la retransmisión no es real. El valor se actualizará con el valor del estado de la retransmisión real, que se explica más adelante en el tutorial.

4. La aplicación Spotinst espera las aserciones de SAML en un formato específico. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **Atributos de usuario** de la página de integración de aplicaciones. La siguiente captura de pantalla le muestra un ejemplo de esto.

    ![Configurar inicio de sesión único](./media/spotinst-tutorial/tutorial_Spotinst_attribute.png)

5. En la sección **Atributos de usuario** del cuadro de diálogo **Inicio de sesión único**, configure el atributo Token SAML como muestra la imagen anterior y realice los siguientes pasos:

    | Nombre del atributo | Valor de atributo |
    | ---------------| --------------- |
    | Email | user.mail |
    | Nombre | user.givenname |
    | Apellidos | user.surname |
    
    a. Haga clic en **Agregar atributo** para abrir el cuadro de diálogo **Agregar atributo**.

    ![Configurar inicio de sesión único](./media/spotinst-tutorial/tutorial_attribute_04.png)

    ![Configurar inicio de sesión único](./media/spotinst-tutorial/tutorial_attribute_05.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. En la lista **Valor**, seleccione el atributo que se muestra para esa fila.

    d. Deje **Espacio de nombres** en blanco.

    e. Haga clic en **Aceptar**.

6. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Vínculo de descarga del certificado](./media/spotinst-tutorial/tutorial_spotinst_certificate.png) 

7. Haga clic en el botón **Guardar** .

    ![Botón Configurar inicio de sesión único](./media/spotinst-tutorial/tutorial_general_400.png)

8. En otra ventana del explorador web, inicie sesión en Spotinst como administrador de seguridad.

9. Haga clic en el **icono de usuario** en la parte superior derecha de la pantalla y haga clic en **Configuración**.

    ![Configuración de Spotinst](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

10. Haga clic en la pestaña **Seguridad** en la parte superior y, a continuación, seleccione **Proveedores de identidades** y realice los pasos siguientes:

    ![Seguridad de Spotinst](./media/spotinst-tutorial/tutorial_spotinst_security.png)

    a. Copie el valor de **Estado de la retransmisión** de su instancia y péguelo en el cuadro de texto **Estado de la retransmisión** de la sección **Dominio y direcciones URL de Spotinst** de Azure Portal.

    b. Haga clic en **EXAMINAR** para cargar el archivo XML de metadatos que descargó de Azure Portal.

    c. Haga clic en **GUARDAR**.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

   ![Creación de un usuario de prueba de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel izquierdo de Azure Portal, haga clic en el botón **Azure Active Directory**.

    ![Botón Azure Active Directory](./media/spotinst-tutorial/create_aaduser_01.png)

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y, luego, haga clic en **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](./media/spotinst-tutorial/create_aaduser_02.png)

3. En la parte superior del cuadro de diálogo **Todos los usuarios**, haga clic en **Agregar** para abrir el cuadro de diálogo **Agregar**.

    ![Botón Agregar](./media/spotinst-tutorial/create_aaduser_03.png)

4. En el cuadro de diálogo **Usuario** , realice los pasos siguientes:

    ![Cuadro de diálogo Usuario](./media/spotinst-tutorial/create_aaduser_04.png)

    a. En el cuadro **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la dirección de correo electrónico del usuario Britta Simon.

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).

### <a name="create-a-spotinst-test-user"></a>Creación de un usuario de prueba Spotinst

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Spotinst.

1. Si ha configurado la aplicación en modo iniciado por **SP**, realice los siguientes pasos:

   a. En otra ventana del explorador web, inicie sesión en Spotinst como administrador de seguridad.

   b. Haga clic en el **icono de usuario** en la parte superior derecha de la pantalla y haga clic en **Configuración**.

    ![Configuración de Spotinst](./media/spotinst-tutorial/tutorial_spotinst_settings.png)

    c. Haga clic en **Usuarios** y seleccione **Agregar usuario**.

    ![Configuración de Spotinst](./media/spotinst-tutorial/adduser1.png)

    d. En la sección Agregar usuario, realice los pasos siguientes:

    ![Configuración de Spotinst](./media/spotinst-tutorial/adduser2.png)

    * En el cuadro de texto **Nombre completo**, escriba el nombre completo del usuario, por ejemplo, **BrittaSimon**.

    * En el cuadro de texto **Correo electrónico**, escriba la dirección de correo electrónico del usuario, en este caso, **brittasimon@contoso.com**.

    * Seleccione los detalles específicos de su organización para el **rol de la organización, el rol de cuenta y las cuentas**.

2. Si ha configurado la aplicación en el modo iniciado por **IDP**, no hay ningún elemento de acción para usted en esta sección. Spotinst admite el aprovisionamiento Just-In-Time, que está habilitado de forma predeterminada. Al intentar acceder a Spotinst, se crea un nuevo usuario, en caso de que no exista.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Spotinst.

![Asignación de rol de usuario][200] 

**Para asignar a Britta Simon a Spotinst, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Spotinst**.

    ![Vínculo a Spotinst en la lista de aplicaciones](./media/spotinst-tutorial/tutorial_spotinst_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Spotinst en el panel de acceso, debería iniciar sesión automáticamente en la aplicación Spotinst.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/spotinst-tutorial/tutorial_general_01.png
[2]: ./media/spotinst-tutorial/tutorial_general_02.png
[3]: ./media/spotinst-tutorial/tutorial_general_03.png
[4]: ./media/spotinst-tutorial/tutorial_general_04.png

[100]: ./media/spotinst-tutorial/tutorial_general_100.png

[200]: ./media/spotinst-tutorial/tutorial_general_200.png
[201]: ./media/spotinst-tutorial/tutorial_general_201.png
[202]: ./media/spotinst-tutorial/tutorial_general_202.png
[203]: ./media/spotinst-tutorial/tutorial_general_203.png

