---
title: 'Tutorial: Integración de Azure Active Directory en Cloud Service (inicio de sesión único basado en roles) | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Alibaba Cloud Service (inicio de sesión único basado en roles).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3667841e-acfc-4490-acf5-80d9ca3e71e8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdfd19d9a0e928e26ad6f01ba4b9c3f493aacb0c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67107162"
---
# <a name="tutorial-azure-active-directory-integration-with-alibaba-cloud-service-role-based-sso"></a>Tutorial: Integración de Azure Active Directory en Cloud Service (inicio de sesión único basado en roles)

En este tutorial, aprenderá a integrar Alibaba Cloud Service (inicio de sesión único basado en roles) en Azure Active Directory (Azure AD).
La integración de Alibaba Cloud Service (inicio de sesión único basado en roles) en Azure AD proporciona las siguientes ventajas:

* Puede controlar en Azure AD quién tiene acceso a Alibaba Cloud Service (inicio de sesión único basado en roles).
* Puede permitir que los usuarios inicien sesión automáticamente en Alibaba Cloud Service (inicio de sesión único basado en roles) (inicio de sesión único) con sus cuentas de Azure AD.
* Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea obtener más información sobre la integración de aplicaciones SaaS con Azure AD, vea [Qué es el acceso a las aplicaciones y el inicio de sesión único en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD en Alibaba Cloud Service (inicio de sesión único basado en roles), necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no dispone de un entorno de Azure AD, puede obtener [una cuenta gratuita](https://azure.microsoft.com/free/)
* Una suscripción habilitada para el inicio de sesión único de Alibaba Cloud Service (inicio de sesión único basado en roles)

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba.

* Alibaba Cloud Service (inicio de sesión único basado en roles) admite SSO iniciado por **IDP**

## <a name="adding-alibaba-cloud-service-role-based-sso-from-the-gallery"></a>Agregar Alibaba Cloud Service (inicio de sesión único basado en roles) desde la galería

Para configurar la integración de Alibaba Cloud Service (inicio de sesión único basado en roles) en Azure AD, es preciso agregar Alibaba Cloud Service (inicio de sesión único basado en roles) desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Alibaba Cloud Service (inicio de sesión único basado en roles) desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)** , haga clic en el icono de **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione la opción **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Alibaba Cloud Service (inicio de sesión único basado en roles)** , seleccione **Alibaba Cloud Service (inicio de sesión único basado en roles)** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Alibaba Cloud Service (inicio de sesión único basado en roles) en la lista de resultados](common/search-new-app.png)

5. En la página de **Alibaba Cloud Service (inicio de sesión único basado en roles)** , haga clic en **Properties** (Propiedades) en el panel de navegación izquierdo, copie el **identificador de objeto** y guárdelo en su equipo para usarlo más adelante.

    ![Configuración de propiedades](./media/alibaba-cloud-service-role-based-sso-tutorial/Properties.png)
    
## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, se configura y se prueba el inicio de sesión único de Azure AD con Alibaba Cloud Service (inicio de sesión único basado en roles) con un usuario de prueba llamado **Britta Simon**.
Para que el inicio de sesión único funcione, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Alibaba Cloud Service (inicio de sesión único basado en roles).

Para configurar y probar el inicio de sesión único de Azure AD con Alibaba Cloud Service (inicio de sesión único basado en roles), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)** : para que los usuarios puedan usar esta característica.
2. **[Configure el inicio de sesión único basado en roles de Alibaba Cloud Service](#configure-role-based-single-sign-on-in-alibaba-cloud-service)** para permitir que los usuarios puedan usar esta característica.
2. **[Configuración del inicio de sesión único de Alibaba Cloud Service (inicio de sesión único basado en roles)](#configure-alibaba-cloud-service-role-based-sso-single-sign-on)** : para configurar los valores de Inicio de sesión único en la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Creación de usuarios de prueba de Alibaba Cloud Service (inicio de sesión único basado en roles)](#create-alibaba-cloud-service-role-based-sso-test-user)** : para tener un homólogo de Britta Simon en Alibaba Cloud Service (inicio de sesión único basado en roles) que esté vinculado a la representación de usuario en Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)** : para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal.

Para configurar el inicio de sesión único de Azure AD con Alibaba Cloud Service (inicio de sesión único basado en roles), siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de la aplicación **Alibaba Cloud Service (inicio de sesión único basado en roles)** , haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único](common/select-sso.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML/WS-Fed** para habilitar el inicio de sesión único.

    ![Modo de selección de inicio de sesión único](common/select-saml-option.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, si tiene el **archivo de metadatos del proveedor de servicios**, lleve a cabo los siguientes pasos:

    >[!NOTE]
    >Obtendrá los metadatos del proveedor de servicios de esta [dirección URL](https://signin.alibabacloud.com/saml-role/sp-metadata.xml)

    a. Haga clic en **Cargar el archivo de metadatos**.

    ![imagen](common/upload-metadata.png)

    b. Haga clic en el **logotipo de la carpeta** para seleccionar el archivo de metadatos y luego en **Cargar**.

    ![imagen](common/browse-upload-metadata.png)

    c. Una vez que se haya cargado correctamente el archivo de metadatos, los valores **Identificador** y **Dirección URL de respuesta** se rellenan automáticamente en el cuadro de texto de la sección Alibaba Cloud Service (inicio de sesión único basado en roles):

    ![imagen](common/idp-intiated.png)

    > [!Note]
    > Si los valores **Identificador** y **Dirección URL de respuesta** no se rellenan automáticamente, hágalo manualmente según sus necesidades.

5. La aplicación Alibaba Cloud Service (inicio de sesión único basado en roles) espera que las aserciones de SAML estén en un formato específico, así que es necesario agregar asignaciones de atributos personalizados a la configuración de los atributos del token SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono **Editar** para abrir el cuadro de diálogo **Atributos de usuario**.

    ![imagen](common/edit-attribute.png)

6. Además de lo anterior, la aplicación Alibaba Cloud Service (inicio de sesión único basado en roles) espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes:

    | NOMBRE | Espacio de nombres | Atributo de origen|
    | ---------------| ------------| --------------- |
    | Role | https:\//www.aliyun.com/SAML-Role/Attribute | user.assignedroles |
    | RoleSessionName | https:\//www.aliyun.com/SAML-Role/Attribute | user.userprincipalname |

    > [!NOTE]
    > Haga clic [aquí](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) para saber cómo configurar el valor **Role** en Azure AD.

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Deje **Espacio de nombres** en blanco.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Save**(Guardar).

7. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el **XML de metadatos de federación** de las opciones proporcionadas según sus requisitos y guárdelo en el equipo.

    ![Vínculo de descarga del certificado](common/metadataxml.png)

8. En la sección **Set up Alibaba Cloud Service (Role-based SSO)** [Alibaba Cloud Service (inicio de sesión único basado en roles)], copie las direcciones URL adecuadas según sus necesidades.

    ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

    a. URL de inicio de sesión

    b. Identificador de Azure AD

    c. URL de cierre de sesión

### <a name="configure-role-based-single-sign-on-in-alibaba-cloud-service"></a>Configuración del inicio de sesión único basado en roles de Alibaba Cloud Service

1. Inicie sesión en la [consola RAM](https://account.alibabacloud.com/login/login.htm?oauth_callback=https%3A%2F%2Fram.console.aliyun.com%2F%3Fspm%3Da2c63.p38356.879954.8.7d904e167h6Yg9) de Alibaba Cloud Service mediante Account1.

2. En el panel de navegación izquierdo, seleccione **SSO**.

3. En la pestaña **Role-based SSO** (Inicio de sesión único basado en roles), haga clic en **Create IdP** (Crear IdP).

4. En la página que aparece, escriba `AAD` en el campo IdP Name (Nombre de IdP), escriba una descripción en el campo **Note** (Nota), haga clic en **Upload** (Cargar) para cargar el archivo de metadatos de federación que descargó anteriormente y, después, haga clic en **OK** (Aceptar).

5. Una vez creado el IdP satisfactoriamente, haga clic en **Create RAM Role** (Crear rol de RAM).

6. En el campo **RAM Role Name** (Nombre de rol de RAM) escriba `AADrole`, seleccione `AAD` en la lista desplegable **Select IdP** (Seleccionar IdP) y haga clic en OK (Aceptar).

    >[!NOTE]
    >Puede conceder al rol los permisos que sean necesarios. Después de crear el proveedor de identidades y el rol correspondiente, se recomienda que guarde los ARN del IdP y el rol para su uso posterior. Puede obtener los ARN en la página de información del IdP y en la página de información del rol.

7. Asocie el rol de la RAM de Alibaba Cloud Service (AADrole) con el usuario de Azure AD (u2): Para asociar el rol de la RAM con el usuario de Azure AD, debe crear un rol en Azure AD mediante estos pasos:

    a. Inicie sesión en el [Explorador de Azure AD Graph](https://developer.microsoft.com/graph/graph-explorer?spm=a2c63.p38356.879954.9.7d904e167h6Yg9).

    b. Haga clic en **Modificar permisos** para obtener los permisos necesarios para crear un rol.

    ![Configuración de Graph](./media/alibaba-cloud-service-role-based-sso-tutorial/graph01.png)

    c. Seleccione los permisos siguientes en la lista y haga clic en **Modificar permisos**, como se muestra en la ilustración siguiente.

    ![Configuración de Graph](./media/alibaba-cloud-service-role-based-sso-tutorial/graph02.png)

    >[!NOTE]
    >Una vez concedidos los permisos, inicie sesión en el Explorador de Graph de nuevo.

    d. En la página del Explorador de Graph, seleccione **GET** en la primera lista desplegable y **beta** en la segunda. A continuación, escriba `https://graph.microsoft.com/beta/servicePrincipals` en el campo situado junto a las listas desplegables y haga clic en **Ejecutar consulta**.

    ![Configuración de Graph](./media/alibaba-cloud-service-role-based-sso-tutorial/graph03.png)

    >[!NOTE]
    >Si usa varios directorios, puede escribir `https://graph.microsoft.com/beta/contoso.com/servicePrincipals` en el campo de la consulta.

    e. En la sección **Response Preview** (Vista previa de la respuesta), extraiga la propiedad appRoles de "Entidad de servicio" para su uso posterior.

    ![Configuración de Graph](./media/alibaba-cloud-service-role-based-sso-tutorial/graph05.png)

    >[!NOTE]
    >Puede encontrar la propiedad appRoles escribiendo `https://graph.microsoft.com/beta/servicePrincipals/<objectID>` en el campo de la consulta. Tenga en cuenta que `objectID` es el identificador del objeto que ha copiado de la página de **propiedades** de Azure AD.

    f. Vuelva al Explorador de Graph, cambie el método de **GET** a **PATCH**, pegue el siguiente contenido en la sección **Cuerpo de la solicitud** y haga clic en **Ejecutar consulta**:
    ```
    { 
    "appRoles": [
        { 
        "allowedMemberTypes":[
            "User"
        ],
        "description": "msiam_access",
        "displayName": "msiam_access",
        "id": "41be2db8-48d9-4277-8e86-f6d22d35****",
        "isEnabled": true,
        "origin": "Application",
        "value": null
        },
        { "allowedMemberTypes": [
            "User"
        ],
        "description": "Admin,AzureADProd",
        "displayName": "Admin,AzureADProd",
        "id": "68adae10-8b6b-47e6-9142-6476078cdbce",
        "isEnabled": true,
        "origin": "ServicePrincipal",
        "value": "acs:ram::187125022722****:role/aadrole,acs:ram::187125022722****:saml-provider/AAD"
        }
    ]
    }
    ```
    > [!NOTE]
    > `value` es el ARN del proveedor de identidades y el rol que creó en la consola de la RAM. En este caso, puede agregar varios roles según sea necesario. Azure AD enviará el valor de estos roles como valor de notificación en la respuesta de SAML. Sin embargo, solo puede agregar nuevos roles después del elemento `msiam_access` para la operación de revisión. Para facilitar el proceso de creación, se recomienda que use un generador de identificadores, como GUID Generator, para generar identificadores en tiempo real.

    g. Una vez que haya revisado la entidad de servicio con el rol necesario, asocie el rol con el usuario de Azure AD (u2) siguiendo los pasos de la sección **Asignación del usuario de prueba de Azure AD** del tutorial.

### <a name="configure-alibaba-cloud-service-role-based-sso-single-sign-on"></a>Configuración del inicio de sesión único de Alibaba Cloud Service (inicio de sesión único basado en roles)

Para configurar el inicio de sesión único en **Alibaba Cloud Service (inicio de sesión único basado en roles)** , es preciso enviar el **XML de metadatos de federación** descargado y las direcciones URL apropiadas copiadas de Azure Portal al [equipo de soporte técnico de Alibaba Cloud Service (inicio de sesión único basado en roles)](https://www.aliyun.com/service/). Dicho equipo lo configura para establecer la conexión de SSO de SAML correctamente en ambos lados.

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD 

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Vínculos "Usuarios y grupos" y "Todos los usuarios"](common/users.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Botón Nuevo usuario](common/new-user.png)

3. En las propiedades Usuario, siga estos pasos.

    ![Cuadro de diálogo Usuario](common/user-properties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba `brittasimon@yourcompanydomain.extension`. Por ejemplo: BrittaSimon@contoso.com

    c. Active la casilla **Mostrar contraseña** y, después, anote el valor que se muestra en el cuadro Contraseña.

    d. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Alibaba Cloud Service (inicio de sesión único basado en roles).

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones** y **Alibaba Cloud Service (inicio de sesión único basado en roles)** .

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Alibaba Cloud Service (inicio de sesión único basado en roles)** .

    ![El vínculo de Alibaba Cloud Service (inicio de sesión único basado en roles) en la lista de aplicaciones](common/all-applications.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

4. Haga clic en el botón **Agregar usuario** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación](common/add-assign-user.png)

5. En la pestaña **Usuarios y grupos**, seleccione u2 en la lista de usuarios y haga clic en **Seleccionar**. Después, haga clic en **Asignar**.

    ![Prueba de la configuración](./media/alibaba-cloud-service-role-based-sso-tutorial/test01.png)

6. Vea el rol asignado y pruebe Alibaba Cloud Service (inicio de sesión único basado en roles).

    ![Prueba de la configuración](./media/alibaba-cloud-service-role-based-sso-tutorial/test02.png)

    >[!NOTE]
    >Después de asignar el usuario (u2), se asociará automáticamente el rol creado al usuario. Si ha creado varios roles, deberá asociar el rol adecuado al usuario según sea necesario. Si desea implementar el inicio de sesión único basado en roles de Azure AD a varias cuentas de Alibaba Cloud, repita los pasos anteriores.

### <a name="create-alibaba-cloud-service-role-based-sso-test-user"></a>Creación de un usuario de prueba en Alibaba Cloud Service (inicio de sesión único basado en roles)

En esta sección, se crea un usuario llamado en Britta Simon en Alibaba Cloud Service (inicio de sesión único basado en roles). Trabaje con el [equipo de soporte técnico de Alibaba Cloud Service (inicio de sesión único basado en roles)](https://www.aliyun.com/service/) para agregar los usuarios a la plataforma de Alibaba Cloud Service (inicio de sesión único basado en roles). Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único 

Una vez completadas las configuraciones anteriores, pruebe Alibaba Cloud Service (inicio de sesión único basado en roles) con estos pasos:

1. En Azure Portal, vaya a la página de **Alibaba Cloud Service (inicio de sesión único basado en roles)** , seleccione **Inicio de sesión único** y haga clic en **Probar**.

    ![Prueba de la configuración](./media/alibaba-cloud-service-role-based-sso-tutorial/test03.png)

2. Haga clic en **Iniciar sesión en nombre del usuario actual**.

    ![Prueba de la configuración](./media/alibaba-cloud-service-role-based-sso-tutorial/test04.png)

3. En la página de selección de la cuenta, seleccione u2.

    ![Prueba de la configuración](./media/alibaba-cloud-service-role-based-sso-tutorial/test05.png)

4. Se abrirá la página siguiente, que indica que el inicio de sesión único basado en roles es correcto.

    ![Prueba de la configuración](./media/alibaba-cloud-service-role-based-sso-tutorial/test06.png)

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

