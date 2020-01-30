---
title: Personalización de la interfaz de usuario de la aplicación con una directiva personalizada
titleSuffix: Azure AD B2C
description: Obtenga información acerca de cómo personalizar una interfaz de usuario mediante una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/11/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 42426e8249f3b6ceb592254833f38ab0df99c83c
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76851087"
---
# <a name="customize-the-user-interface-of-your-application-using-a-custom-policy-in-azure-active-directory-b2c"></a>Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Después de completar este artículo, tendrá una directiva personalizada de registro e inicio de sesión con su marca y apariencia. Con Azure Active Directory B2C (Azure AD B2C), controlará prácticamente todo el contenido HTML y CSS que se presenta a los usuarios. Cuando se usa una directiva personalizada, la personalización de la interfaz del usuario se configura en XML en lugar de con controles de Azure Portal.

## <a name="prerequisites"></a>Prerequisites

Realice los pasos del artículo [Introducción a las directivas personalizadas](custom-policy-get-started.md). Debe tener una directiva personalizada activa para registrar e iniciar sesión de cuentas locales.

## <a name="page-ui-customization"></a>Personalización de la interfaz de usuario de la página

La característica de personalización de la interfaz de usuario de la página sirve para personalizar la apariencia y la experiencia de cualquier directiva personalizada. También puede mantener la coherencia visual y de la marca entre la aplicación y Azure AD B2C.

Funcionamiento: Azure AD B2C ejecuta código en el explorador del consumidor y usa un enfoque moderno denominado [Uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/). En primer lugar, especifique una dirección URL en la directiva personalizada con contenido HTML personalizado. Azure AD B2C combina elementos de la interfaz de usuario con el contenido HTML cargado desde la dirección URL y muestra la página al consumidor.

## <a name="create-your-html5-content"></a>Creación de contenido HTML5

Puede crear contenido HTML con el nombre de la marca del producto en el título.

1. Copie el siguiente fragmento de código HTML. Es HTML5 bien formado con un elemento vacío denominado *\<div id="api"\>\</div\>* situado en las etiquetas *\<body\>* . Este elemento indica dónde se va a insertar el contenido de Azure AD B2C.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

1. Pegue el fragmento de código copiado en un editor de texto y guarde el archivo como *customize-ui.html*.

> [!NOTE]
> Si se usa login.microsoftonline.com, los elementos de formulario HTML se quitarán debido a las restricciones de seguridad. Si quiere usar elementos de formulario HTML en el contenido HTML personalizado, utilice b2clogin.com. Consulte [Uso de b2clogin.com](b2clogin.md) para conocer otras ventajas.

## <a name="create-an-azure-blob-storage-account"></a>Creación de una cuenta de Almacenamiento de blobs de Azure

>[!NOTE]
> En este artículo se usa Azure Blob Storage para hospedar el contenido. Puede elegir hospedar el contenido en un servidor web, pero deberá [habilitar CORS en el servidor web](https://enable-cors.org/server.html).

Para hospedar este contenido HTML en Blob Storage, haga lo siguiente:

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. En el menú **central**, seleccione **Nuevo** > **Almacenamiento** > **Cuenta de almacenamiento**.
1. Seleccione una **Suscripción** para la cuenta de almacenamiento.
1. Cree un **Grupo de recursos** o seleccione uno existente.
1. Escriba un **Nombre** único para la cuenta de almacenamiento.
1. Seleccione la **Ubicación geográfica** de la cuenta de almacenamiento.
1. El **Modelo de implementación** puede permanecer como **Resource Manager**.
1. El **Rendimiento** puede permanecer como **Estándar**.
1. Cambie **Tipo de cuenta** a **Almacenamiento de blobs**.
1. La **Replicación** puede permanecer como **RA-GRS**.
1. El **Nivel de acceso** puede permanecer **Activo**.
1. Haga clic en **Revisar + Crear** para crear la cuenta de almacenamiento.
    Espere a que termine la implementación y la página de la **cuenta de almacenamiento** se abrirá automáticamente.

## <a name="create-a-container"></a>Crear un contenedor

Para crear un contenedor público en Blob Storage, realice los siguientes pasos:

1. En **Blob service** en el menú izquierdo, seleccione **Blobs**.
1. Haga clic en **+Contenedor**.
1. En **Nombre**, escriba *root*. Este puede ser un nombre de su elección, por ejemplo *wingtiptoys*, sin embargo, usamos *root* en este ejemplo por motivos de simplicidad.
1. Para **Nivel de acceso público**, seleccione **Blob** y, luego, **Aceptar**.
1. Haga clic en **root** para abrir el contenedor nuevo.
1. Haga clic en **Cargar**.
1. Haga clic en el icono de carpeta junto a **Seleccione un archivo**.
1. Vaya a **customize-ui.html**, que creó anteriormente en la sección Personalización de la interfaz de usuario de la página, y selecciónelo.
1. Si quiere cargar en una subcarpeta, expanda **Avanzadas** y escriba un nombre de la carpeta en **Cargar en carpeta**.
1. Seleccione **Cargar**.
1. Seleccione el blob **customize-ui.html** que cargó.
1. A la derecha del cuadro de texto **URL**, seleccione el icono **Copiar al Portapapeles** para copiar la dirección URL en el Portapapeles.
1. En el explorador web, vaya a la dirección URL que copió para verificar que puede acceder al blob que cargó. Si no puede acceder, por ejemplo, si hay un error `ResourceNotFound`, asegúrese de que el tipo de acceso del contenedor esté establecido en **blob**.

## <a name="configure-cors"></a>Configuración de CORS

Para configurar Blob Storage para el uso compartido de recursos entre orígenes (CORS), realice los siguientes pasos:

1. En el menú, seleccione **CORS**.
1. En **Orígenes permitidos**, escriba `https://your-tenant-name.b2clogin.com`. Reemplace `your-tenant-name` por el nombre del inquilino de Azure AD B2C. Por ejemplo, `https://fabrikam.b2clogin.com`. Al escribir su nombre de inquilino, use solo minúsculas.
1. En **Métodos permitidos**, seleccione `GET` y `OPTIONS`.
1. En **Encabezados permitidos**, escriba un asterisco (*).
1. En **Encabezados expuestos**, escriba un asterisco (*).
1. Para **Antigüedad máxima**, introduzca 200.
1. Haga clic en **Save**(Guardar).

## <a name="test-cors"></a>Prueba de CORS

Para comprobar que está listo, realice los siguientes pasos:

1. Vaya al sitio web [www.test-cors.org](https://www.test-cors.org/) y pegue la dirección URL en el cuadro **Remote URL** (Dirección URL remota).
1. Haga clic en **Send Request** (Enviar solicitud).
    Si recibe un error, asegúrese de que la [configuración de CORS](#configure-cors) sea correcta. Puede que también deba borrar la caché del explorador o abrir una sesión de navegación privada, para ello, presione Ctrl + Mayús + P.

## <a name="modify-the-extensions-file"></a>Modificación del archivo de extensiones

Para configurar la personalización de la interfaz de usuario, se copia **ContentDefinition** y sus elementos secundarios del archivo base al archivo de extensiones.

1. Abra el archivo base de la directiva. Por ejemplo, <em>`SocialAndLocalAccounts/` **`TrustFrameworkBase.xml`** </em>. Este es uno de los archivos de directivas incluidos en el paquete de inicio de directivas personalizadas, que debería haber obtenido en el requisito previo, [Introducción a las directivas personalizadas](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-get-started-custom).
1. Busque y copie todo el contenido del elemento **ContentDefinitions**.
1. Abra el archivo de extensión. Por ejemplo, *TrustFrameworkExtensions.xml*. Busque el elemento **BuildingBlocks**. Si el elemento no existe, agréguelo.
1. Pegue todo el contenido del elemento **ContentDefinitions** que ha copiado como elemento secundario del elemento **BuildingBlocks**.
1. Busque el elemento **ContentDefinition** que contenga `Id="api.signuporsignin"` en el XML que ha copiado.
1. Cambie el valor de **LoadUri** a la dirección URL del archivo HTML que cargó en el almacenamiento. Por ejemplo, `https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html`.

    La directiva personalizada debería ser similar a la siguiente:

    ```xml
    <BuildingBlocks>
      <ContentDefinitions>
        <ContentDefinition Id="api.signuporsignin">
          <LoadUri>https://your-storage-account.blob.core.windows.net/your-container/customize-ui.html</LoadUri>
          <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
          <DataUri>urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0</DataUri>
          <Metadata>
            <Item Key="DisplayName">Signin and Signup</Item>
          </Metadata>
        </ContentDefinition>
      </ContentDefinitions>
    </BuildingBlocks>
    ```

1. Guarde el archivo de extensiones.

## <a name="upload-and-test-your-updated-custom-policy"></a>Carga y prueba de la directiva personalizada actualizada

### <a name="upload-the-custom-policy"></a>Carga de la directiva personalizada

1. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
1. Busque y seleccione **Azure AD B2C**.
1. En **Directivas**, seleccione **Identity Experience Framework**.
1. Seleccione **Cargar directiva personalizada**.
1. Cargue el archivo de extensiones que cambió anteriormente.

### <a name="test-the-custom-policy-by-using-run-now"></a>Prueba de la directiva personalizada con **Ejecutar ahora**

1. Seleccione la directiva cargada y, a continuación, **Ejecutar ahora**.
1. Debería poder registrarse con una dirección de correo electrónico.

## <a name="reference"></a>Referencia

### <a name="sample-templates"></a>Plantillas de ejemplo
Aquí encontrará plantillas de ejemplo de personalización de interfaz de usuario:

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

La carpeta sample_templates/wingtip contiene los siguientes archivos HTML:

| Plantilla HTML5 | Descripción |
|----------------|-------------|
| *phonefactor.html* | Use este archivo como plantilla para una página de autenticación multifactor. |
| *resetpassword.html* | Use este archivo como plantilla para una página de recuperación de contraseña. |
| *selfasserted.html* | Use este archivo como plantilla para una página de registro en una cuenta social, una página de registro en una cuenta local o una página de inicio de sesión en una cuenta local. |
| *unified.html* | Use este archivo como plantilla para una página de inicio de sesión o registro unificada. |
| *updateprofile.html* | Use este archivo como plantilla para una página de actualización de perfil. |

Estos son los pasos sobre cómo usar el ejemplo:

1. Clone el repositorio en la máquina local. Elija una carpeta de plantilla en sample_templates. Puede usar `wingtip` o `contoso`.
1. Cargue todos los archivos en las carpetas `css`, `fonts` y `images` en Blob Storage, como se describe en las secciones anteriores.
1. A continuación, abra cada archivo \*.html en la raíz de `wingtip` o `contoso` (el que haya seleccionado en el primer paso) y reemplace todas las instancias de "http://localhost" con las direcciones URL de los archivos css, de imágenes y de fuentes que cargó en el paso 2.
1. Guardar los archivos \*.html y cárguelos en Blob Storage.
1. Ahora, modifique el archivo de extensiones como se mencionó anteriormente en [Modificación del archivo de extensiones](#modify-the-extensions-file).
1. Si ve que faltan las fuentes, imágenes o CSS, compruebe las referencias en la directiva de extensiones y los archivos \*.html.

### <a name="content-definition-ids"></a>Id. de definición de contenido

En la sección Modificación de la directiva de inicio de sesión o de registro configuró la definición del contenido de `api.idpselections`. Todos los identificadores de definición de contenido que se reconocen en el marco de la experiencia de identidad de Azure AD B2C y sus descripciones aparecen en la tabla siguiente:

| Id. de definición de contenido | Descripción |
|-----------------------|-------------|
| *api.error* | **Página de error**. Esta página se muestra cuando se produce una excepción o un error. |
| *api.idpselections* | **Página de selección del proveedor de identidades**. Esta página contiene una lista de proveedores de identidades que el usuario puede elegir durante el inicio de sesión. Estas opciones son proveedores de identidades de empresa, proveedores de identidades sociales como Facebook y Google+ o cuentas locales. |
| *api.idpselections.signup* | **Selección del proveedor de identidades para el registro**. Esta página contiene una lista de proveedores de identidades que el usuario puede elegir durante el inicio de sesión. Estas opciones son proveedores de identidades de empresa, proveedores de identidades sociales como Facebook y Google+ o cuentas locales. |
| *api.localaccountpasswordreset* | **Página de contraseña olvidada**. Esta página contiene un formulario que el usuario tiene que rellenar para iniciar el restablecimiento de contraseña.  |
| *api.localaccountsignin* | **Página de inicio de sesión en una cuenta local**. Esta página contiene un formulario de registro con una cuenta local basada en una dirección de correo electrónico o un nombre de usuario. El formulario puede contener un cuadro de entrada de texto y un cuadro de entrada de contraseña. |
| *api.localaccountsignup* | **Página de registro en una cuenta local**. Esta página contiene un formulario de registro para una cuenta local basada en una dirección de correo electrónico o un nombre de usuario. El formulario puede contener varios controles de entrada, como un cuadro de entrada de texto, un cuadro de entrada de contraseña, un botón de radio, cuadros desplegables de selección única y casillas de selección múltiple. |
| *api.phonefactor* | **Página de autenticación multifactor**. Esta página permite a los usuarios verificar sus números de teléfono (mediante texto o voz) durante el registro o el inicio de sesión. |
| *api.selfasserted* | **Página de registro en una cuenta social**. Esta página contiene un formulario de registro que los usuarios tienen que rellenar al registrarse con una cuenta existente de un proveedor de identidades social, como Facebook o Google+. Esta página es similar a la página anterior de registro en una cuenta social, excepto por los campos de entrada de contraseña. |
| *api.selfasserted.profileupdate* | **Página de actualización de perfil**. Esta página contiene un formulario que los usuarios pueden usar para actualizar su perfil. Esta página es similar a la página de registro en una cuenta social, excepto por los campos de entrada de contraseña. |
| *api.signuporsignin* | **Página de inicio de sesión o registro unificada**. Esta página controla tanto la suscripción como el inicio de sesión de los usuarios, que pueden usar proveedores de identidades de empresa, proveedores de identidades sociales, como Facebook o Google +, o cuentas locales.  |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre los elementos de la interfaz de usuario que se pueden personalizar, consulte la [guía de referencia para la personalización de la interfaz de usuario para los flujos de usuario](customize-ui-overview.md).
