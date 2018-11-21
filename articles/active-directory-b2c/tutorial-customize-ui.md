---
title: 'Tutorial: Personalización de la interfaz de usuario de la aplicación en Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información acerca de cómo personalizar la interfaz de usuario de la aplicación en Azure Active Directory B2C con Azure Portal.
services: B2C
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/12/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ee6d7735a2983f642eff82a7dabe036af100e60e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622676"
---
# <a name="tutorial-customize-the-user-interface-of-your-applications-in-azure-active-directory-b2c"></a>Tutorial: Personalización de la interfaz de usuario de la aplicación en Azure Active Directory B2C

Para obtener experiencias de usuario más comunes, como registrarse, iniciar sesión y editar perfiles, puede usar las [directivas integradas](active-directory-b2c-reference-policies.md) de Azure Active Directory (Azure AD) B2C. La información de este tutorial le ayuda a obtener información acerca de cómo [personalizar la interfaz de usuario (IU)](customize-ui-overview.md) de estas experiencias mediante sus propios archivos HTML y CSS.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Crear archivos de personalización de la interfaz de usuario
> * Crear una directiva de registro o de inicio de sesión que use los archivos
> * Probar la interfaz de usuario personalizada

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Si aún no ha creado su propio [inquilino de Azure AD B2C](tutorial-create-tenant.md), cree una ahora. Puede usar un inquilino existente, si creó uno en un tutorial anterior.

## <a name="create-customization-files"></a>Crear archivos de personalización

Cree una cuenta de almacenamiento de Azure y el contenedor y, después, coloque los archivos básicos de HTML y CSS en el contenedor.

### <a name="create-a-storage-account"></a>Crear una cuenta de almacenamiento

Aunque se pueden almacenar los archivos de muchas maneras, para este tutorial, almacénelos en el [almacenamiento de blobs de Azure](../storage/blobs/storage-blobs-introduction.md).

1. Asegúrese de que está usando el directorio que contiene su suscripción de Azure. Seleccione el **filtro de directorio y suscripciones** en el menú superior y elija el directorio que contiene su suscripción. Este directorio es diferente que el de la carpeta que contiene el inquilino de Azure B2C.

    ![Cambiar al directorio de suscripción](./media/tutorial-customize-ui/switch-directories.png)

2. Elija Todos los servicios en la esquina superior izquierda de Azure Portal, busque y seleccione **Cuentas de almacenamiento**. 
3. Seleccione **Agregar**.
4. En **Grupo de recursos**, seleccione **Crear nuevo** y, después, proporcione un nombre válido para el nuevo grupo de recursos y haga clic en **Aceptar**.
5. Escriba un nombre para la cuenta de almacenamiento. El nombre que elija debe ser único en Azure, tener entre tres y 24 caracteres y solo pueden contener números y letras minúsculas.
6. Seleccione la ubicación de la cuenta de almacenamiento o acepte la ubicación predeterminada. 
7. Acepte los demás valores predeterminados, seleccione **Revisar y crear**y, después, haga clic en **Crear**.
8. Una vez creada la cuenta de almacenamiento, seleccione **Ir al recurso**.

### <a name="create-a-container"></a>Crear un contenedor

1. En la página de información general de la cuenta de almacenamiento, seleccione **Blobs**.
2. Seleccione **Contenedor**, escriba un nombre para el contenedor, elija **Blob (acceso de lectura anónimo solo para blobs)** y, después, haga clic en **Aceptar**.

### <a name="enable-cors"></a>Habilitación de CORS

 El código de Azure AD B2C en un explorador utiliza un enfoque moderno y estándar para cargar contenido personalizado desde una dirección URL que especifique en una directiva. Uso compartido de recursos entre orígenes (CORS) permite que los recursos restringidos en una página web se soliciten desde otros dominios.

1. En el menú, seleccione **CORS**.
2. En **Orígenes permitidos**, escriba `your-tenant-name.b2clogin.com`. Reemplace `your-tenant-name` por el nombre del inquilino de Azure AD B2C. Por ejemplo, `fabrikam.b2clogin.com`.
3. En **Métodos permitidos**, seleccione `GET` y `OPTIONS`.
4. En **Encabezados permitidos**, escriba un asterisco (*).
5. En **Encabezados expuestos**, escriba un asterisco (*).
6. Para **Antigüedad máxima**, introduzca 200.

    ![Habilitación de CORS](./media/tutorial-customize-ui/enable-cors.png)

5. Haga clic en **Save**(Guardar).

### <a name="create-the-customization-files"></a>Crear archivos de personalización

Para personalizar la interfaz de usuario de la experiencia de registro, primero debe crear un archivo sencillo HTML y CSS. Puede configurar su archivo HTML como desee, pero debe tener un elemento **div** con un identificador de `api`. Por ejemplo, `<div id="api"></div>`. Azure AD B2C inserta elementos en el contenedor `api` cuando se muestra la página.

1. En una carpeta local, cree el archivo siguiente y asegúrese de que cambia `your-storage-account` por el nombre de la cuenta de almacenamiento y `your-container` por el nombre del contenedor que ha creado. Por ejemplo, `https://store1.blob.core.windows.net/b2c/style.css`.

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>My B2C Application</title>
        <link rel="stylesheet" href="https://your-storage-account.blob.core.windows.net/your-container/style.css">
      </head>
      <body>  
        <h1>My B2C Application</h1>
        <div id="api"></div>
      </body>
    </html>
    ```

    La página se puede diseñar como quiera, pero el elemento div **api** es necesario para cualquier archivo de personalización HTML que cree. 

3. Guarde el archivo como *custom-ui.html*.
4. Cree el siguiente archivo CSS sencillo que centra todos los elementos en la página de registro o de inicio de sesión, incluidos los elementos que Azure AD B2C inserta.

    ```css
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center; 
    }
    .entry {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center; 
    }
    .create {
      width: 300px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

5. Guarde el archivo como *style.css*.

### <a name="upload-the-customization-files"></a>Cargar archivos de personalización

En este tutorial, almacenará los archivos que ha creado en la cuenta de almacenamiento para que Azure AD B2C puede tener acceso a ellos.

1. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Cuentas de almacenamiento**.
2. Seleccione la cuenta de almacenamiento que creó, seleccione **Blobs** y, después, seleccione el contenedor que ha creado.
3. Seleccione **Cargar**, vaya al archivo*custom-ui.html* y selecciónelo y, después, haga clic en **Cargar**.

    ![Cargar archivos de personalización](./media/tutorial-customize-ui/upload-blob.png)

4. Copie la dirección URL del archivo que cargó para usarla más adelante en el tutorial.
5. Repita el paso 3 y 4 para el archivo *style.css*.

## <a name="create-a-sign-up-and-sign-in-policy"></a>Creación de una directiva de registro o de inicio de sesión

Para completar los pasos de este tutorial, deberá crear una aplicación de prueba y una directiva de registro o inicio de sesión en Azure AD B2C. Puede aplicar los principios descritos en este tutorial a otras experiencias de usuario, como la edición de perfiles.

### <a name="create-an-azure-ad-b2c-application"></a>Creación de una aplicación de Azure AD B2C

La comunicación con Azure AD B2C se produce mediante una aplicación que se crea en el inquilino. Los pasos siguientes crean una aplicación que redirige el token de autorización que se devuelve a [https://jwt.ms](https://jwt.ms).

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y **Agregar**.
5. Escriba un nombre para la aplicación; por ejemplo, *testapp1*.
6. En **Aplicación web/API web**, seleccione `Yes` y escriba `https://jwt.ms` como **URL de respuesta**.
7. Haga clic en **Create**(Crear).

### <a name="create-the-policy"></a>Creación de la directiva

Para probar los archivos de personalización, cree una directiva de registro o inicio de sesión integrada que use la aplicación que creó anteriormente.

1. En el inquilino de Azure AD B2C, seleccione **Sign-up or sign-in policies** (Directivas de registro o inicio de sesión) y, después, haga clic en **Agregar**.
2. Escriba un nombre para la directiva. Por ejemplo, *signup_signin*. El prefijo *B2C_1* se agrega automáticamente al nombre cuando se crea la directiva.
3. Seleccione **Proveedores de identidades**, establezca **Email sign-up** (Registro de correo electrónico) para una cuenta local y, después, haga clic en **Aceptar**.
4. Seleccione **Atributos de registro**, elija los atributos que quiere recopilar del cliente durante el registro. Por ejemplo, configure **País o región**, **Nombre para mostrar** y **Código postal** y, luego, haga clic en **Aceptar**.
5. Seleccione **Notificaciones de la aplicación**, puede elegir las notificaciones que quiere que se devuelvan en los tokens de autorización enviados de vuelta a su aplicación después de una experiencia de registro o de inicio de sesión correcta. Por ejemplo, seleccione **Nombre para mostrar**, **Proveedor de identidades**, **Código Postal**, **El usuario es nuevo** e **Id. de objeto del usuario**. Luego, haga clic en **Aceptar**.
6. Seleccione **Personalización de la interfaz de usuario de la página**, seleccione **Página de inicio de sesión o registro unificada** y haga clic en **Sí** para **Usar página personalizada**.
7. En **URI de página personalizado**, escriba la dirección URL para el archivo *custom-ui.html* que anotó anteriormente y, después, haga clic en **Aceptar**.
8. Haga clic en **Create**(Crear).

## <a name="test-the-policy"></a>Prueba de la directiva

1. En el inquilino de Azure AD B2C, seleccione **Sign-up or sign-in policies** (Directivas de registro o inicio de sesión) y, después, seleccione la directiva que creó. Por ejemplo, *B2C_1_signup_signin*.
2. Asegúrese de que la aplicación que creó está seleccionada en **Seleccionar aplicación** y, después, haga clic en **Ejecutar ahora**.

    ![Ejecutar la directiva de registro o de inicio de sesión](./media/tutorial-customize-ui/signup-signin.png)

    Debería ver una página similar al ejemplo siguiente con los elementos centrados basándose en el archivo CSS que ha creado:

    ![Resultados de directivas](./media/tutorial-customize-ui/run-now.png) 

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo:

> [!div class="checklist"]
> * Crear archivos de personalización de la interfaz de usuario
> * Crear una directiva de registro o de inicio de sesión que use los archivos
> * Probar la interfaz de usuario personalizada

> [!div class="nextstepaction"]
> [Personalización de idioma en Azure Active Directory B2C](active-directory-b2c-reference-language-customization.md)