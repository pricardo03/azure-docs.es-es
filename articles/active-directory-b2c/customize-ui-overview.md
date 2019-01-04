---
title: Acerca de la personalización de la interfaz de usuario en Azure Active Directory B2C | Microsoft Docs
description: Obtenga información acerca de cómo personalizar la interfaz de usuario en las aplicaciones que usan Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/30/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 94f7239bdcdc5ddc91f3ed8b49b59378309711e8
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52844964"
---
# <a name="about-user-interface-customization-in-azure-active-directory-b2c"></a>Acerca de la personalización de la interfaz de usuario en Azure Active Directory B2C

La capacidad para marcar y personalizar la interfaz de usuario (UI) que Azure Active Directory (Azure AD) B2C ofrece a sus aplicaciones es importante para poder proporcionar una experiencia sin problemas a su cliente. Estas experiencias incluyen las opciones de registro, inicio de sesión, edición de perfiles y restablecimiento de contraseñas. En este artículo se proporciona información que le ayudará a personalizar la interfaz de usuario de sus aplicaciones.

Dependiendo de sus necesidades en lo que a estas experiencias se refiere, puede personalizar la interfaz de usuario de su aplicación de diferentes maneras. Por ejemplo: 

- si usa [flujos de usuario](active-directory-b2c-reference-policies.md) para proporcionar experiencias de registro o inicio de sesión, restablecimiento de contraseña o edición de perfiles en su aplicación, utilice [Azure Portal para personalizar la interfaz de usuario](tutorial-customize-ui.md).
- Si solo proporciona la opción de inicio de sesión junto con la página de restablecimiento de contraseña y los correos electrónicos de comprobación, use los mismos pasos de personalización que se deben usar en una [página de inicio de sesión de Azure AD](../active-directory/fundamentals/customize-branding.md).
- Si los clientes intentan editar su perfil antes de iniciar sesión, se les redirige a una página que usted mismo puede personalizar siguiendo los mismos pasos que se usan para personalizar la página de inicio de sesión de Azure AD.
- Si está usando [directivas personalizadas](active-directory-b2c-overview-custom.md) para proporcionar en la aplicación las opciones de registro o inicio de sesión, de restablecimiento de contraseña o de edición de perfiles, use los [archivos de directiva para personalizar la interfaz de usuario](active-directory-b2c-ui-customization-custom.md).
- Si necesita proporcionar contenido dinámico en función de la decisión de un cliente, use [directivas personalizadas que puedan cambiar el contenido de la página](active-directory-b2c-ui-customization-custom-dynamic.md) según el parámetro que se envíe en una cadena de consulta. Por ejemplo, puede cambiar la imagen de fondo en la página de inicio de sesión o de registro de Azure AD B2C en función de un parámetro que se pasa desde la aplicación web o el dispositivo móvil.

Azure AD B2C ejecuta código en el explorador del consumidor y usa un enfoque moderno denominado [Uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/). En tiempo de ejecución, el contenido se carga desde una dirección URL especificada en un flujo de usuario o directiva. Puede especificar diferentes direcciones URL para distintas páginas. Cuando el contenido se carga desde la dirección URL se combina con un fragmento HTML insertado en Azure AD B2C y, a continuación, se muestra la página al cliente.

Antes de comenzar, repase las siguientes advertencias:

- Azure AD B2C combina contenido HTML en las páginas. No copie ni intente cambiar el contenido predeterminado que proporciona Azure AD B2C. Es preferible compilar su propio contenido de HTML desde cero y usar el contenido predeterminado como referencia.
- Por motivos de seguridad, no puede incluir JavaScript en el contenido.
- Estas son las versiones de explorador admitidas: 
    - Internet Explorer 11, 10 y Edge.
    - Compatibilidad limitada con Internet Explorer 9 y 8.
    - Google Chrome 42.0 y versiones posteriores
    - Mozilla Firefox 38.0 y versiones posteriores
- Asegúrese de que no incluye etiquetas de formato en el HTML, ya que esto podría interferir con las operaciones POST que genera el HTML insertado desde Azure AD B2C.

## <a name="where-do-i-store-ui-content"></a>¿Dónde se puede almacenar el contenido de la interfaz de usuario?

El contenido de la interfaz de usuario se puede hospedar en cualquier lugar como, por ejemplo, en [Azure Blob Storage](../storage/blobs/storage-blobs-introduction.md), en servidores web, en CDN, en AWS S3 o en sistemas de uso compartido de archivos. Recuerde que podrá continuar trabajando siempre y cuando el contenido se hospede en un punto de conexión HTTPS disponible públicamente con CORS habilitado. Debe usar una dirección URL absoluta cuando la especifique en el contenido.

## <a name="how-do-i-get-started"></a>¿Cómo empiezo?

Realice el siguiente procedimiento para personalizar la interfaz de usuario:

- Cree contenido HTML bien formado con un elemento `<div id="api"></div>` vacío ubicado en algún lugar de `<body>`. Ese elemento marca el lugar en el que se inserta el contenido de Azure AD B2C. En el ejemplo siguiente se muestra una página mínima.

    ```html
    <!DOCTYPE html>
    <html>
      <head>
        <title>!Add your title here!</title>
        <link rel="stylesheet" href="https://mystore1.blob.core.windows.net/b2c/style.css">
      </head>
      <body>
        <h1>My B2C Application</h1>
        <div id="api"></div>   <!-- Leave this element empty because Azure AD B2C will insert content here. -->
      </body>
    </html>
    ```

- Hospedar el contenido en un punto de conexión HTTPS (donde se permite CORS). Tenga en cuenta que los métodos de solicitud GET y OPTIONS se deben habilitar al configurar CORS.
- Use CSS para aplicar estilo a los elementos de la interfaz de usuario que inserta Azure AD B2C en la página. En el ejemplo siguiente se muestra un archivo CSS sencillo que también incluye la configuración de los elementos HTML insertados en el registro:

    ```css 
    h1 {
      color: blue;
      text-align: center;
    }
    .intro h2 {
      text-align: center; 
    }
    .entry {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    .divider h2 {
      text-align: center; 
    }
    .create {
      width: 400px ;
      margin-left: auto ;
      margin-right: auto ;
    }
    ```

- Cree o edite una directiva para usar el contenido que haya creado.

En la tabla siguiente se muestran fragmentos de HTML que Azure AD B2C combina en el elemento `<div id="api"></div>` que se encuentra en su contenido.

| Página insertada | Descripción de HTML |
| ------------- | ------------------- |
| Selección del proveedor de identidades | Contiene una lista de botones para los proveedores de identidades entre los que el usuario puede elegir durante el registro o el inicio de sesión. Estos botones incluyen proveedores de identidades sociales como Facebook y Google o las cuentas locales (que están basadas en una dirección de correo electrónico o un nombre de usuario). |
| Registro en una cuenta local | Contiene un formulario para el registro en una cuenta local basada en una dirección de correo electrónico o un nombre de usuario. El formulario puede contener diferentes controles de entrada, como un cuadro de entrada de texto, un cuadro de entrada de contraseña, un botón de radio, cuadros desplegables de selección única y casillas de verificación de selección múltiple. |
| Registro en una cuenta social | Puede aparecer al registrarse con una cuenta existente de un proveedor de identidades de redes sociales, como Facebook o Google. Se usa cuando se debe recopilar información adicional del usuario mediante un formulario de registro. |
| Inicio de sesión o registro unificado | Controla tanto el registro como el inicio de sesión de los clientes, los cuales pueden usar proveedores de identidades sociales como Facebook o Google, o cuentas locales. |
| Multi-Factor Authentication | Los usuarios pueden comprobar sus números de teléfono (mediante mensajes de texto o con la voz) durante el registro o el inicio de sesión. |
| Error | Proporciona información del error al cliente. |


## <a name="how-do-i-localize-content"></a>¿Cómo localizo el contenido?

Puede localizar el contenido HTML si habilita la [personalización de idioma](active-directory-b2c-reference-language-customization.md) en su inquilino de Azure AD B2C. Al habilitar esta característica, Azure AD B2C puede reenviar el parámetro de OpenID Connect, `ui-locales`, al punto de conexión. El servidor de contenido puede usar este parámetro para proporcionar páginas HTML específicas del idioma.

Como alternativa, puede extraer contenido de distintos lugares, según la configuración regional que use. En el punto de conexión habilitado para CORS, puede configurar una estructura de carpetas para hospedar contenido de idiomas concretos. Deberá llamar al idioma adecuado si usa el valor de carácter comodín {Culture:RFC5646}. Por ejemplo, la URI de la página personalizada podría ser similar a `https://contoso.blob.core.windows.net/{Culture:RFC5646}/myHTML/unified.html`. Puede cargar la página en francés mediante la extracción de contenido de `https://contoso.blob.core.windows.net/fr/myHTML/unified.html`.

## <a name="examples"></a>Ejemplos

Para obtener ejemplos de personalización, descargue y revise estos [archivos de plantillas de ejemplo](https://github.com/azureadquickstarts/b2c-azureblobstorage-client/archive/master.zip).

## <a name="next-steps"></a>Pasos siguientes

- Si usa flujos de usuario, puede empezar a personalizar la interfaz de usuario con el tutorial: [Personalización de la interfaz de usuario de la aplicación en Azure Active Directory B2C](tutorial-customize-ui.md).
- Si va a usar directivas personalizadas, puede empezar a personalizar la interfaz de usuario con el artículo: [Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada en Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md).

