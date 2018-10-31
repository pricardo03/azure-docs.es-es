---
title: 'Tutorial: Integración de Azure Active Directory con Amazon Web Services (AWS) | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: jeedes
ms.openlocfilehash: 8e91fbf0befaef9088e9afaa6e69c0cb29ad4858
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49363766"
---
# <a name="tutorial-azure-active-directory-integration-with-amazon-web-services-aws"></a>Tutorial: Integración de Azure Active Directory con Amazon Web Services (AWS)

En este tutorial, obtendrá información sobre cómo integrar Amazon Web Services (AWS) con Azure Active Directory (Azure AD).

La integración de Amazon Web Services (AWS) con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Amazon Web Services (AWS).
- Puede permitir que los usuarios inicien sesión automáticamente en Amazon Web Services (AWS) (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Puede configurar varios identificadores para varias instancias de la manera siguiente. 

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Con estos valores, Azure AD quitará el valor de **#** y enviará el valor correcto `https://signin.aws.amazon.com/saml` como dirección URL del público en el token de SAML.

**Se recomienda usar este enfoque por las siguientes razones:**

a. Cada aplicación le proporcionará el certificado X509 único, luego cada instancia puede tener una fecha de expiración de certificado diferente, que puede administrar según cada cuenta de AWS. En este caso la sustitución general del certificado será fácil.

b. Puede habilitar el aprovisionamiento de usuarios con la aplicación AWS en Azure AD y, luego, nuestro servicio capturará todos los roles de esa cuenta de AWS. No tiene que agregar ni actualizar manualmente los roles de AWS en la aplicación.

c. Puede asignar individualmente el propietario de la aplicación, quien puede administrar la aplicación directamente en Azure AD.

> [!Note]
> Asegurarse de usar solo aplicaciones de la galería

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Amazon Web Services (AWS), necesita los siguientes elementos:

- Una suscripción de Azure AD
- Un suscripción habilitada para el inicio de sesión único en Amazon Web Services (AWS)

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

> [!Note]
> Si desea integrar varias cuentas de AWS en una cuenta de Azure para el inicio de sesión único, consulte [este](https://docs.microsoft.com/azure/active-directory/active-directory-saas-aws-multi-accounts-tutorial) artículo.

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de Amazon Web Services (AWS) desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Adición de Amazon Web Services (AWS) desde la galería
Para configurar la integración de Amazon Web Services (AWS) en Azure AD, es preciso agregar Amazon Web Services (AWS) desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Amazon Web Services (AWS) desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![imagen](./media/amazon-web-service-tutorial/selectazuread.png)

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![imagen](./media/amazon-web-service-tutorial/a_select_app.png)
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![imagen](./media/amazon-web-service-tutorial/a_new_app.png)

4. En el cuadro de búsqueda, escriba **Amazon Web Services (AWS)**, seleccione **Amazon Web Services (AWS)** en el panel de resultados y, a continuación, haga clic en el botón **Agregar** para agregar la aplicación.

     ![imagen](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Amazon Web Services (AWS) con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Amazon Web Services (AWS) para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Amazon Web Services (AWS).

Para configurar y probar el inicio de sesión único de Azure AD con Amazon Web Services (AWS), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-single-sign-on)**: para que los usuarios puedan usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Amazon Web Services (AWS)](#create-an-amazon-web-services-aws-test-user)**: para tener un homólogo de Britta Simon en Amazon Web Services (AWS) que esté vinculado a la representación del usuario en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación de Amazon Web Services (AWS).

**Para configurar el inicio de sesión único de Azure AD con Amazon Web Services (AWS), realice los pasos siguientes:**

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Amazon Web Services (AWS)**, seleccione **Inicio de sesión único**.

    ![imagen](./media/amazon-web-service-tutorial/B1_B2_Select_SSO.png)

2. En el cuadro de diálogo **Seleccionar un método de inicio de sesión único**, seleccione el modo **SAML** para habilitar el inicio de sesión único.

    ![imagen](./media/amazon-web-service-tutorial/b1_b2_saml_sso.png)

3. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![imagen](./media/amazon-web-service-tutorial/b1-domains_and_urlsedit.png)

4. En la sección **Configuración básica de SAML**, el usuario no tiene que realizar ningún paso porque la aplicación ya se ha integrado previamente con Azure.

    ![imagen](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_url.png)

5. Cuando se configure más de una instancia, proporcione el valor del identificador. Desde la segunda instancia en adelante, proporcione el valor de identificador en el siguiente formato: Use un signo **#** para especificar un valor único de SPN. 

    `https://signin.aws.amazon.com/saml#2`

    ![Información de dominio y direcciones URL de inicio de sesión único de Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_identifier.png)

6. La aplicación Amazon Web Services (AWS) espera las aserciones de SAML en un formato concreto. Configure las siguientes notificaciones para esta aplicación. Puede administrar los valores de estos atributos en la sección **User Attributes & Claims** (Atributos y notificaciones del usuario) de la página de integración de aplicaciones. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario).

    ![imagen](./media/amazon-web-service-tutorial/i4-attribute.png)

7. En la sección **Notificaciones del usuario** del cuadro de diálogo **User Attributes & Claims** (Atributos y notificaciones del usuario), configure el atributo token SAML como se muestra en la imagen anterior y realice los siguientes pasos:
    
    | NOMBRE  | Atributo de origen  | Espacio de nombres |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Rol            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "proporcione un valor comprendido entre 900 segundos (15 minutos) y 43200 segundos (12 horas)" |  https://aws.amazon.com/SAML/Attributes |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](./media/amazon-web-service-tutorial/i2-attribute.png)

    ![imagen](./media/amazon-web-service-tutorial/i3-attribute.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. Escriba el valor de **Espacio de nombres**.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Save**(Guardar).

8. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, haga clic en **Descargar** para descargar el archivo **XML de metadatos de federación**  y guárdelo en su equipo.

    ![imagen](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_certificate.png) 

9. En otra ventana del explorador, inicie sesión en su sitio de la compañía de Amazon Web Services (AWS) como administrador.

10. Haga clic en **AWS Home** (Página principal de AWS).

    ![Configurar página principal de inicio de sesión único][11]

11. Haga clic en **Administración de identidades y acceso**.

    ![Configurar identidad de inicio de sesión único][12]

12. Haga clic en **Proveedores de identidades** y, después, en **Create Provider** (Crear proveedor).

    ![Configurar proveedor de inicio de sesión único][13]

13. En la página de diálogo **Configure Provider** (Configurar proveedor), realice los pasos siguientes:

    ![Configurar cuadro de diálogo de inicio de sesión único][14]

    a. En **Tipo de proveedor**, seleccione **SAML**.

    b. En el cuadro de texto **Provider Name** (Nombre de proveedor), escriba un nombre de proveedor (por ejemplo, *WAAD*).

    c. Para cargar el **archivo de metadatos** descargado de Azure Portal, haga clic en **Choose file** (Elegir archivo).

    d. Haga clic en **Siguiente paso**.

14. En la página de diálogo **Verify Provider Information** (Comprobar la información del proveedor), haga clic en **Crear**.

    ![Configurar verificación de inicio de sesión único][15]

15. Haga clic en **Roles** y, después, en **Crear rol**.

    ![Configurar roles de inicio de sesión único][16]

16. En la página **Crear rol**, realice los pasos siguientes:  

    ![Configurar confianza de inicio de sesión único][19]

    a. Seleccione **SAML 2.0 federation** (Federación de SAML 2.0) en **Select type of trusted entity** (Seleccionar tipo de entidad de confianza).

    b. En la sección **Choose a SAML 2.0 Provider** (Elegir un proveedor de SAML 2.0), seleccione **SAML provider** (Proveedor de SAML) que ha creado anteriormente (por ejemplo: *WAAD*).

    c. Seleccione **Allow programmatic and AWS Management Console access** (Permitir acceso mediante programación y a consola de AWS Management Console).
  
    d. Haga clic en **Next: Permissions** (Siguiente: permisos).

17. En el cuadro de diálogo **Attach Permissions Policies** (Adjuntar directivas de permisos), no es necesario adjuntar ninguna directiva. Haga clic en **Siguiente: revisión**.  

    ![Configurar directiva de inicio de sesión único][33]

18. En el cuadro de diálogo **Revisar** , realice los pasos siguientes:

    ![Configurar revisión de inicio de sesión único][34]

    a. En el cuadro de texto **Role name** (Nombre de rol), escriba su nombre de rol.

    b. En el cuadro de texto **Role description**, escriba la descripción.

    c. Haga clic en **Crear rol**.

    d. Cree tantos roles como sea necesario y asígnelos al proveedor de identidades.

19. Use credenciales de cuenta de servicio de AWS para obtener los roles de cuenta de AWS en aprovisionamiento de usuarios de Azure AD. Para ello, abra la página principal de la consola de AWS.

20. Haga clic en **Servicios** -> **Seguridad, identidad y cumplimiento** -> **IAM**.

    ![obtención de roles de la cuenta de AWS](./media/amazon-web-service-tutorial/fetchingrole1.png)

21. Seleccione la pestaña **Tabs** (Directivas) en la sección IAM.

    ![obtención de roles de la cuenta de AWS](./media/amazon-web-service-tutorial/fetchingrole2.png)

22. Para crear una nueva directiva, haga clic en **Create policy** (Crear directiva) para recuperar los roles de la cuenta de AWS de aprovisionamiento de usuarios de Azure AD.

    ![Creación de una nueva directiva](./media/amazon-web-service-tutorial/fetchingrole3.png)

23. Cree su propia directiva para obtener todos los roles de las cuentas de AWS con estos pasos:

    ![Creación de una nueva directiva](./media/amazon-web-service-tutorial/policy1.png)

    a. En la sección **"Crear directiva"**, haga clic en la pestaña **"JSON"**.

    b. En el documento de la directiva, agregue el siguiente JSON.

    ```

    {

    "Version": "2012-10-17",

    "Statement": [

    {

    "Effect": "Allow",

    "Action": [

    "iam:ListRoles"

    ],

    "Resource": "*"

    }

    ]

    }

    ```

    c. Haga clic en el botón **Revisar directiva** para validar la directiva.

    ![Definición de una nueva directiva](./media/amazon-web-service-tutorial/policy5.png)

24. Defina la **directiva nueva** con estos pasos:

    ![Definición de una nueva directiva](./media/amazon-web-service-tutorial/policy2.png)

    a. En **Policy Name** (Nombre de la directiva), especifique **AzureAD_SSOUserRole_Policy**.

    b. En **Description** (Descripción), puede describir la directiva como **Esta directiva permitirá obtener los roles de cuentas de AWS**.

    c. Haga clic en el botón **"Crear directiva"**.

25. Cree una nueva cuenta de usuario en el servicio IAM de AWS mediante los pasos siguientes:

    a. Haga clic en el panel de navegación **Users** (Usuarios) en la consola de IAM de AWS.

    ![Definición de una nueva directiva](./media/amazon-web-service-tutorial/policy3.png)

    b. Haga clic en el botón **Add user** (Agregar usuario) para crear un nuevo usuario.

    ![Agregar usuario](./media/amazon-web-service-tutorial/policy4.png)

    c. En la sección **Add user** (Agregar usuario), lleve a cabo estos pasos:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser1.png)

    * Escriba el nombre de usuario como **AzureADRoleManager**.

    * En el tipo de acceso, seleccione la opción **Programmatic access** (Acceso mediante programación). De este modo, el usuario puede llamar a las API y obtener los roles de la cuenta de AWS.

    * Haga clic en el botón **Next Permissions** (Permisos siguientes) en la esquina inferior derecha.

26. Ahora cree una nueva directiva para este usuario mediante los pasos siguientes:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser2.png)

    a. Haga clic en el botón **Attach existing policies directly** (Asociar directivas existentes directamente).

    b. Busque la directiva recién creada en la sección de filtro **AzureAD_SSOUserRole_Policy**.

    c. Seleccione la **directiva** y, a continuación, haga clic en el botón **Next: Review** (Siguiente: Revisión).

27. Revise la directiva del usuario asociado mediante los pasos siguientes:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser3.png)

    a. Revise el nombre de usuario, el tipo de acceso y la directiva asociada al usuario.

    b. Haga clic en el botón **Create user** (Crear usuario) situado en la esquina inferior derecha para crear el usuario.

28. Descargue las credenciales de usuario de un usuario mediante estos pasos:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser4.png)

    a. Copie el **identificador de la clave de acceso** y la **clave de acceso secreta** del usuario.

    b. Escriba estas credenciales en la sección de aprovisionamiento de usuarios de Azure AD para obtener los roles de la consola de AWS.

    c. Haga clic en el botón **Close** (Cerrar) en la parte inferior.

29. Vaya a la sección **Aprovisionamiento de usuarios** de la aplicación Amazon Web Services en el Portal de administración de Azure AD.

    ![Agregar usuario](./media/amazon-web-service-tutorial/provisioning.png)

30. Escriba la **clave de acceso** y la **clave secreta** en los campos **Secreto de cliente** y **Token secreto** respectivamente.

    ![Agregar usuario](./media/amazon-web-service-tutorial/provisioning1.png)

    a. Escriba la clave de acceso de usuario de AWS en el campo **Secreto de cliente**.

    b. Escriba el secreto de usuario de AWS en el campo **Token secreto**.

    c. Haga clic en el botón **Probar conexión**. Al hacerlo, debería poder probar correctamente esta conexión.

    d. Haga clic en el botón **Guardar** situado en la parte superior para guardar la configuración.

31. Ahora, asegúrese de establecer el estado de aprovisionamiento en **Activado** en la sección de configuración, para lo que debe activar el conmutador y, a continuación, hacer clic en el botón **Guardar** situado en la parte superior.

    ![Agregar usuario](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![imagen](./media/amazon-web-service-tutorial/d_users_and_groups.png)

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![imagen](./media/amazon-web-service-tutorial/d_adduser.png)

3. En las propiedades Usuario, siga estos pasos.

    ![imagen](./media/amazon-web-service-tutorial/d_userproperties.png)

    a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.
 
### <a name="create-an-amazon-web-services-aws-test-user"></a>Creación de un usuario de prueba de Amazon Web Services (AWS)

El objetivo de esta sección es crear un usuario llamado Britta Simon en Amazon Web Services (AWS). Amazon Web Services (AWS) no necesita que se cree un usuario en su sistema para el inicio de sesión único, por lo que no es necesario realizar ninguna acción aquí.

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Amazon Web Services (AWS).

1. En Azure Portal, seleccione **Aplicaciones empresariales**, **Todas las aplicaciones**.

    ![imagen](./media/amazon-web-service-tutorial/d_all_applications.png)

2. En la lista de aplicaciones, seleccione **Amazon Web Services (AWS)**.

    ![imagen](./media/amazon-web-service-tutorial/tutorial_amazonwebservices(aws)_app.png)

3. En el menú de la izquierda, seleccione **Usuarios y grupos**.

    ![imagen](./media/amazon-web-service-tutorial/d_leftpaneusers.png)

4. Seleccione el botón **Agregar** y, después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![imagen](./media/amazon-web-service-tutorial/d_assign_user.png)

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

    ![imagen](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_users.png)

6. En el cuadro de diálogo **Seleccionar rol**, elija el rol de usuario adecuado de la lista y, luego, haga clic en el botón **Seleccionar** situado en la parte inferior de la pantalla.

    ![imagen](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_roles.png)

    >[!NOTE]
    >Después de habilitar el aprovisionamiento de usuarios con la aplicación, tendrá que esperar 30 minutos para obtener todos los roles de Amazon Web Services (AWS), luego deberá actualizar la página y, mientras asigna la aplicación a los usuarios y grupos, verá los roles para el usuario.

7. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

    ![imagen](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_assign.png)
    
### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Amazon Web Services (AWS) en el panel de acceso, automáticamente iniciará sesión en su aplicación de Amazon Web Services (AWS).
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[11]: ./media/amazon-web-service-tutorial/ic795031.png
[12]: ./media/amazon-web-service-tutorial/ic795032.png
[13]: ./media/amazon-web-service-tutorial/ic795033.png
[14]: ./media/amazon-web-service-tutorial/ic795034.png
[15]: ./media/amazon-web-service-tutorial/ic795035.png
[16]: ./media/amazon-web-service-tutorial/ic795022.png
[17]: ./media/amazon-web-service-tutorial/ic795023.png
[18]: ./media/amazon-web-service-tutorial/ic795024.png
[19]: ./media/amazon-web-service-tutorial/ic795025.png
[32]: ./media/amazon-web-service-tutorial/ic7950251.png
[33]: ./media/amazon-web-service-tutorial/ic7950252.png
[35]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning.png
[34]: ./media/amazon-web-service-tutorial/ic7950253.png
[36]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials.png
[37]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_securitycredentials_continue.png
[38]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_createnewaccesskey.png
[39]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_automatic.png
[40]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_testconnection.png
[41]: ./media/amazon-web-service-tutorial/tutorial_amazonwebservices_provisioning_on.png
