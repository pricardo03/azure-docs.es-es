---
title: 'Tutorial: Integración de Azure Active Directory con Amazon Web Services (AWS) | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Amazon Web Services (AWS).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 7561c20b-2325-4d97-887f-693aa383c7be
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f897653442a3e1b2d6098b3be60c85e75ca54f9a
ms.sourcegitcommit: 978e1b8cac3da254f9d6309e0195c45b38c24eb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67551495"
---
# <a name="tutorial-integrate-amazon-web-services-aws-with-azure-active-directory"></a>Tutorial: Integración de Amazon Web Services (AWS) con Azure Active Directory

En este tutorial, aprenderá a integrar Amazon Web Services (AWS) con Azure Active Directory (Azure AD). Al integrar Amazon Web Services (AWS) con Azure AD, puede:

* Controlar en Azure AD quién tiene acceso a Amazon Web Services (AWS)
* Permitir que los usuarios inicien sesión automáticamente en Amazon Web Services (AWS) con sus cuentas de Azure AD
* Administrar las cuentas desde una ubicación central (Azure Portal).

Para más información sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

![Amazon Web Services (AWS)](./media/amazon-web-service-tutorial/tutorial_amazonwebservices_image.png)

Puede configurar varios identificadores para varias instancias de la manera siguiente.

* `https://signin.aws.amazon.com/saml#1`

* `https://signin.aws.amazon.com/saml#2`

Con estos valores, Azure AD quitará el valor de **#** y enviará el valor correcto `https://signin.aws.amazon.com/saml` como dirección URL del público en el token de SAML.

**Se recomienda usar este enfoque por las siguientes razones:**

a. Cada aplicación le proporcionará un certificado X509 único. Cada instancia de aplicación de AWS puede tener una fecha de expiración del certificado diferente que se puede administrar de forma individual en cada cuenta de AWS. En este caso, la sustitución general del certificado será fácil.

b. Puede habilitar el aprovisionamiento de usuarios con la aplicación AWS en Azure AD y, luego, nuestro servicio capturará todos los roles de esa cuenta de AWS. No tiene que agregar ni actualizar manualmente los roles de AWS en la aplicación.

c. Puede asignar individualmente el propietario de la aplicación, quien puede administrar la aplicación directamente en Azure AD.

> [!Note]
> Asegurarse de usar solo aplicaciones de la galería

## <a name="prerequisites"></a>Requisitos previos

Para empezar, necesita los siguientes elementos:

* Una suscripción de Azure AD. Si no tiene una suscripción, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/).
* Suscripción habilitada para el inicio de sesión único en Amazon Web Services (AWS)

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, va a configurar y probar el inicio de sesión único de Azure AD en un entorno de prueba. Amazon Web Services (AWS) admite el inicio de sesión único iniciado por **SP e IDP**.

## <a name="adding-amazon-web-services-aws-from-the-gallery"></a>Adición de Amazon Web Services (AWS) desde la galería

Para configurar la integración de Amazon Web Services (AWS) en Azure AD, es preciso agregar Amazon Web Services (AWS) desde la galería a la lista de aplicaciones SaaS administradas.

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta personal, profesional o educativa de Microsoft.
1. En el panel de navegación de la izquierda, seleccione el servicio **Azure Active Directory**.
1. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.
1. Para agregar una nueva aplicación, seleccione **Nueva aplicación**.
1. En la sección **Agregar desde la galería**, escriba **Amazon Web Services (AWS)** en el cuadro de búsqueda.
1. Seleccione **Amazon Web Services (AWS)** en el panel de resultados y, luego, agregue la aplicación. Espere unos segundos mientras la aplicación se agrega al inquilino.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

Configure y pruebe el inicio de sesión único de Azure AD con Amazon Web Services (AWS) mediante un usuario de prueba llamado **B.Simon**. Para que el inicio de sesión único funcione, es necesario establecer una relación de vinculación entre un usuario de Azure AD y el usuario relacionado de Amazon Web Services (AWS).

Para configurar y probar el inicio de sesión único de Azure AD con Amazon Web Services (AWS), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configure-azure-ad-sso)** , para permitir que los usuarios puedan utilizar esta característica.
2. **[Configuración de Amazon Web Services (AWS)](#configure-amazon-web-services-aws)**  para configurar las opciones de inicio de sesión único en el lado de la aplicación.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)** , para probar el inicio de sesión único de Azure AD con B.Simon.
4. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)** , para permitir que B. Simon use el inicio de sesión único de Azure AD.
5. **[Creación de un usuario de prueba de Amazon Web Services (AWS)](#create-amazon-web-services-aws-test-user)** , para tener un homólogo de B.Simon en Amazon Web Services (AWS) que esté vinculado a la representación de este usuario en Azure AD.
6. **[Comprobación del inicio de sesión único](#test-sso)** , para verificar que la configuración funciona correctamente.

### <a name="configure-azure-ad-sso"></a>Configuración del inicio de sesión único de Azure AD

Siga estos pasos para habilitar el inicio de sesión único de Azure AD en Azure Portal.

1. En [Azure Portal](https://portal.azure.com/), en la página de integración de aplicaciones de **Amazon Web Services (AWS)** , busque la sección **Administrar** y seleccione **Inicio de sesión único**.
1. En la página **Seleccione un método de inicio de sesión único**, seleccione **SAML**.
1. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el icono de edición o con forma de lápiz para abrir el cuadro de diálogo **Configuración básica de SAML** y modificar la configuración.

   ![Edición de la configuración básica de SAML](common/edit-urls.png)

4. En la sección **Configuración básica de SAML**, la aplicación está preconfigurada y las direcciones URL necesarias ya se han rellenado previamente con Azure. El usuario debe guardar la configuración, para lo que debe hacer clic en el botón **Guardar**.

5. Cuando se configure más de una instancia, proporcione el valor del identificador. Desde la segunda instancia en adelante, proporcione el valor de identificador en el siguiente formato: Use un signo **#** para especificar un valor único de SPN.

    `https://signin.aws.amazon.com/saml#2`

6. La aplicación Amazon Web Services (AWS) espera las aserciones de SAML en un formato específico, lo que requiere que se agreguen asignaciones de atributos personalizados a la configuración de los atributos del token de SAML. La siguiente captura de muestra la lista de atributos predeterminados. Haga clic en el icono  **Editar**  para abrir el cuadro de diálogo Atributos de usuario.

    ![imagen](common/edit-attribute.png)

7. Además de lo anterior, la aplicación Amazon Web Services (AWS) espera que se usen algunos atributos más en la respuesta de SAML. En la sección **Notificaciones del usuario** del cuadro de diálogo **Atributos de usuario**, realice los siguientes pasos para agregar el atributo Token SAML como se muestra en la tabla siguientes:

    | NOMBRE  | Atributo de origen  | Espacio de nombres |
    | --------------- | --------------- | --------------- |
    | RoleSessionName | user.userprincipalname | https://aws.amazon.com/SAML/Attributes |
    | Role            | user.assignedroles |  https://aws.amazon.com/SAML/Attributes |
    | SessionDuration             | "proporcione un valor comprendido entre 900 segundos (15 minutos) y 43200 segundos (12 horas)" |  https://aws.amazon.com/SAML/Attributes |

    a. Haga clic en **Agregar nueva notificación** para abrir el cuadro de diálogo **Administrar las notificaciones del usuario**.

    ![imagen](common/new-save-attribute.png)

    ![imagen](common/new-attribute-details.png)

    b. En el cuadro de texto **Nombre**, escriba el nombre que se muestra para la fila.

    c. En el cuadro de texto **Espacio de nombres**, escriba el valor del espacio de nombres que se muestra para esa fila.

    d. Seleccione **Atributo** como origen.

    e. En la lista **Atributo de origen**, escriba el valor de atributo que se muestra para esa fila.

    f. Haga clic en **Aceptar**.

    g. Haga clic en **Save**(Guardar).

1. En la página **Configurar el inicio de sesión único con SAML**, en la sección **Certificado de firma de SAML**, busque **XML de metadatos de federación** y seleccione **Descargar** para descargar el certificado y guardarlo en su equipo.

   ![Vínculo de descarga del certificado](common/metadataxml.png)

1. En la sección **Configurar Amazon Web Services (AWS)** , copie las direcciones URL adecuadas según sus necesidades.

   ![Copiar direcciones URL de configuración](common/copy-configuration-urls.png)

### <a name="configure-amazon-web-services-aws"></a>Configuración de Amazon Web Services (AWS)

1. En otra ventana del explorador, inicie sesión en su sitio de la compañía de Amazon Web Services (AWS) como administrador.

2. Haga clic en **AWS Home** (Página principal de AWS).

    ![Configurar página principal de inicio de sesión único][11]

3. Haga clic en **Administración de identidades y acceso**.

    ![Configurar identidad de inicio de sesión único][12]

4. Haga clic en **Proveedores de identidades** y, después, en **Create Provider** (Crear proveedor).

    ![Configurar proveedor de inicio de sesión único][13]

5. En la página de diálogo **Configure Provider** (Configurar proveedor), realice los pasos siguientes:

    ![Configurar cuadro de diálogo de inicio de sesión único][14]

    a. En **Tipo de proveedor**, seleccione **SAML**.

    b. En el cuadro de texto **Provider Name** (Nombre de proveedor), escriba un nombre de proveedor (por ejemplo, *WAAD*).

    c. Para cargar el **archivo de metadatos** descargado de Azure Portal, haga clic en **Choose file** (Elegir archivo).

    d. Haga clic en **Siguiente paso**.

6. En la página de diálogo **Verify Provider Information** (Comprobar la información del proveedor), haga clic en **Crear**.

    ![Configurar verificación de inicio de sesión único][15]

7. Haga clic en **Roles** y, después, en **Crear rol**.

    ![Configurar roles de inicio de sesión único][16]

8. En la página **Crear rol**, realice los pasos siguientes:  

    ![Configurar confianza de inicio de sesión único][19]

    a. Seleccione **SAML 2.0 federation** (Federación de SAML 2.0) en **Select type of trusted entity** (Seleccionar tipo de entidad de confianza).

    b. En la sección **Choose a SAML 2.0 Provider** (Elegir un proveedor de SAML 2.0), seleccione **SAML provider** (Proveedor de SAML) que ha creado anteriormente (por ejemplo, *WAAD*)

    c. Seleccione **Allow programmatic and AWS Management Console access** (Permitir acceso mediante programación y a consola de AWS Management Console).
  
    d. Haga clic en **Siguiente: Permisos**.

9. En el cuadro de diálogo **Attach Permissions Policies** (Adjuntar directivas de permisos), adjunte la directiva adecuada según su organización. Haga clic en **Siguiente: Review** (Siguiente: revisar).  

    ![Configurar directiva de inicio de sesión único][33]

10. En el cuadro de diálogo **Revisar** , realice los pasos siguientes:

    ![Configurar revisión de inicio de sesión único][34]

    a. En el cuadro de texto **Role name** (Nombre de rol), escriba su nombre de rol.

    b. En el cuadro de texto **Role description**, escriba la descripción.

    c. Haga clic en **Crear rol**.

    d. Cree tantos roles como sea necesario y asígnelos al proveedor de identidades.

11. Use credenciales de cuenta de servicio de AWS para obtener los roles de cuenta de AWS en aprovisionamiento de usuarios de Azure AD. Para ello, abra la página principal de la consola de AWS.

12. Haga clic en **Servicios** -> **Seguridad, identidad y cumplimiento** -> **IAM**.

    ![obtención de roles de la cuenta de AWS](./media/amazon-web-service-tutorial/fetchingrole1.png)

13. Seleccione la pestaña **Tabs** (Directivas) en la sección IAM.

    ![obtención de roles de la cuenta de AWS](./media/amazon-web-service-tutorial/fetchingrole2.png)

14. Para crear una nueva directiva, haga clic en **Create policy** (Crear directiva) para recuperar los roles de la cuenta de AWS de aprovisionamiento de usuarios de Azure AD.

    ![Creación de una nueva directiva](./media/amazon-web-service-tutorial/fetchingrole3.png)

15. Cree su propia directiva para obtener todos los roles de las cuentas de AWS con estos pasos:

    ![Creación de una nueva directiva](./media/amazon-web-service-tutorial/policy1.png)

    a. En la sección **"Crear directiva"** , haga clic en la pestaña **"JSON"** .

    b. En el documento de la directiva, agregue el siguiente JSON.

    ```json
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

16. Defina la **directiva nueva** con estos pasos:

    ![Definición de una nueva directiva](./media/amazon-web-service-tutorial/policy2.png)

    a. En **Policy Name** (Nombre de la directiva), especifique **AzureAD_SSOUserRole_Policy**.

    b. En **Description** (Descripción), puede describir la directiva como **Esta directiva permitirá obtener los roles de cuentas de AWS**.

    c. Haga clic en el botón **"Crear directiva"** .

17. Cree una nueva cuenta de usuario en el servicio IAM de AWS mediante los pasos siguientes:

    a. Haga clic en el panel de navegación **Users** (Usuarios) en la consola de IAM de AWS.

    ![Definición de una nueva directiva](./media/amazon-web-service-tutorial/policy3.png)

    b. Haga clic en el botón **Add user** (Agregar usuario) para crear un nuevo usuario.

    ![Agregar usuario](./media/amazon-web-service-tutorial/policy4.png)

    c. En la sección **Add user** (Agregar usuario), lleve a cabo estos pasos:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser1.png)

    * Escriba el nombre de usuario como **AzureADRoleManager**.

    * En el tipo de acceso, seleccione la opción **Programmatic access** (Acceso mediante programación). De este modo, el usuario puede llamar a las API y obtener los roles de la cuenta de AWS.

    * Haga clic en el botón **Next Permissions** (Permisos siguientes) en la esquina inferior derecha.

18. Ahora cree una nueva directiva para este usuario mediante los pasos siguientes:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser2.png)

    a. Haga clic en el botón **Attach existing policies directly** (Asociar directivas existentes directamente).

    b. Busque la directiva recién creada en la sección de filtro **AzureAD_SSOUserRole_Policy**.

    c. Seleccione la **directiva** y, a continuación, haga clic en el botón **Next: Review** (Siguiente: Revisión).

19. Revise la directiva del usuario asociado mediante los pasos siguientes:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser3.png)

    a. Revise el nombre de usuario, el tipo de acceso y la directiva asociada al usuario.

    b. Haga clic en el botón **Create user** (Crear usuario) situado en la esquina inferior derecha para crear el usuario.

20. Descargue las credenciales de usuario de un usuario mediante estos pasos:

    ![Agregar usuario](./media/amazon-web-service-tutorial/adduser4.png)

    a. Copie el **identificador de la clave de acceso** y la **clave de acceso secreta** del usuario.

    b. Escriba estas credenciales en la sección de aprovisionamiento de usuarios de Azure AD para obtener los roles de la consola de AWS.

    c. Haga clic en el botón **Close** (Cerrar) en la parte inferior.

21. Vaya a la sección **Aprovisionamiento de usuarios** de la aplicación Amazon Web Services en el Portal de administración de Azure AD.

    ![Agregar usuario](./media/amazon-web-service-tutorial/provisioning.png)

22. Escriba la **clave de acceso** y la **clave secreta** en los campos **Secreto de cliente** y **Token secreto** respectivamente.

    ![Agregar usuario](./media/amazon-web-service-tutorial/provisioning1.png)

    a. Escriba la clave de acceso de usuario de AWS en el campo **Secreto de cliente**.

    b. Escriba el secreto de usuario de AWS en el campo **Token secreto**.

    c. Haga clic en el botón **Probar conexión**. Al hacerlo, debería poder probar correctamente esta conexión.

    d. Haga clic en el botón **Guardar** situado en la parte superior para guardar la configuración.

23. Ahora, asegúrese de establecer el estado de aprovisionamiento en **Activado** en la sección de configuración, para lo que debe activar el conmutador y, a continuación, hacer clic en el botón **Guardar** situado en la parte superior.

    ![Agregar usuario](./media/amazon-web-service-tutorial/provisioning2.png)

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

En esta sección, va a crear un usuario de prueba llamado B.Simon en Azure Portal.

1. En el panel izquierdo de Azure Portal, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.
1. Seleccione **Nuevo usuario** en la parte superior de la pantalla.
1. En las propiedades del **usuario**, siga estos pasos:
   1. En el campo **Nombre**, escriba `B.Simon`.  
   1. En el campo **Nombre de usuario**, escriba username@companydomain.extension. Por ejemplo, `B.Simon@contoso.com`.
   1. Active la casilla **Show password** (Mostrar contraseña) y, después, anote el valor que se muestra en el cuadro **Contraseña**.
   1. Haga clic en **Create**(Crear).

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a B.Simon a Amazon Web Services (AWS) para que use el inicio de sesión único de Azure.

1. En Azure Portal, seleccione sucesivamente **Aplicaciones empresariales** y **Todas las aplicaciones**.
1. En la lista de aplicaciones, seleccione **Amazon Web Services (AWS)** .
1. En la página de información general de la aplicación, busque la sección **Administrar** y seleccione **Usuarios y grupos**.

   ![Vínculo "Usuarios y grupos"](common/users-groups-blade.png)

1. Seleccione **Agregar usuario**. A continuación, en el cuadro de diálogo **Agregar asignación**, seleccione **Usuarios y grupos**.

    ![Vínculo de Agregar usuario](common/add-assign-user.png)

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **B.Simon** de la lista de usuarios y haga clic en el botón **Seleccionar** de la parte inferior de la pantalla.
1. Si espera que haya un valor de rol en la aserción de SAML, en el cuadro de diálogo **Seleccionar rol**, seleccione en la lista el rol adecuado para el usuario y haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.
1. En el cuadro de diálogo **Agregar asignación**, haga clic en el botón **Asignar**.

### <a name="create-amazon-web-services-aws-test-user"></a>Creación de un usuario de prueba de Amazon Web Services (AWS)

El objetivo de esta sección es crear un usuario llamado B.Simon en Amazon Web Services (AWS). Amazon Web Services (AWS) no necesita que se cree un usuario en su sistema para el inicio de sesión único, por lo que no es necesario realizar ninguna acción aquí.

### <a name="test-sso"></a>Prueba de SSO

Al seleccionar el icono de Amazon Web Services (AWS) en el Panel de acceso, automáticamente iniciará sesión en la versión de Amazon Web Services (AWS) para la que se configuró el inicio de sesión único. Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="known-issues"></a>Problemas conocidos

 * En la sección **Aprovisionamiento**, en la subsección **Asignaciones** aparece un mensaje del tipo "Cargando..." y no se muestran las asignaciones de atributos. El único flujo de trabajo de aprovisionamiento que se admite actualmente es la importación de roles desde AWS a Azure AD para su selección durante la asignación de usuarios y grupos. Las asignaciones de atributos para esto vienen predeterminadas y no se pueden configurar.
 
 * La sección **Aprovisionamiento** solo admite escribir un conjunto de credenciales para un inquilino de AWS cada vez. Todos los roles importados se escriben en la propiedad appRoles del [objeto servicePrincipal](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) de Azure AD para el inquilino de AWS. Se pueden agregar varios inquilinos de AWS (representados por objetos servicePrincipal) a Azure AD desde la galería para el aprovisionamiento, pero hay un problema conocido que impide escribir automáticamente todos los roles importados desde los múltiples objetos servicePrincipal de AWS que se usaron para el aprovisionamiento en el objeto servicePrincipal único empleado para el inicio de sesión único. Una posible solución alternativa consiste en usar [Microsoft Graph API](https://docs.microsoft.com/graph/api/resources/serviceprincipal?view=graph-rest-beta) para extraer todos los objetos appRoles importados en cada objeto servicePrincipal de AWS en los que esté configurado el aprovisionamiento. Estas cadenas de roles se pueden agregar posteriormente al objeto servicePrincipal de AWS donde esté configurado el inicio de sesión único.

## <a name="additional-resources"></a>Recursos adicionales

- [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [¿Qué es el acceso condicional en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

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
