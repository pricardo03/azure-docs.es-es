---
title: Personalizar la interfaz del usuario
titleSuffix: Azure AD B2C
description: Obtenga información acerca de cómo personalizar la interfaz de usuario en las aplicaciones que usan Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 09/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d14e6f98f49f112c8b20abec573b48c3b12705db
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841240"
---
# <a name="customize-the-user-interface-in-azure-active-directory-b2c"></a>Personalización de la interfaz de usuario en Azure Active Directory B2C

La personalización de marca y la personalización de la interfaz de usuario que Azure Active Directory B2C (Azure AD B2C) muestra a los clientes ayuda a proporcionar una experiencia de usuario sin problemas en la aplicación. Estas experiencias incluyen las opciones de registro, inicio de sesión, edición de perfiles y restablecimiento de contraseñas. En este artículo se presentan los métodos de personalización de la interfaz de usuario (UI) para los flujos de usuario y las directivas personalizadas.

## <a name="ui-customization-in-different-scenarios"></a>Personalización de la interfaz de usuario en diferentes escenarios

Hay varias maneras de personalizar la interfaz de usuario del usuario que experimenta su aplicación, cada una de las cuales es adecuada para distintos escenarios.

### <a name="user-flows"></a>Flujos de usuario

Si usa [flujos de usuario](user-flow-overview.md), puede cambiar el aspecto de las páginas de flujo de usuario mediante *plantillas de diseño de página* o mediante su propio HTML y CSS. Ambos métodos se analizarán más adelante en este artículo.

Use [Azure Portal](tutorial-customize-ui.md) para configurar la personalización de la interfaz de usuario para los flujos de usuarios.

### <a name="custom-policies"></a>Directivas personalizadas

Si va a usar [directivas personalizadas](custom-policy-overview.md) para proporcionar en la aplicación las opciones de registro o inicio de sesión, de restablecimiento de contraseña o de edición de perfiles, use los [archivos de directiva para personalizar la interfaz de usuario](custom-policy-ui-customization.md).

Si necesita proporcionar contenido dinámico en función de la decisión de un cliente, use [directivas personalizadas que puedan cambiar el contenido de la página](custom-policy-ui-customization-dynamic.md) según el parámetro que se envíe en una cadena de consulta. Por ejemplo, puede cambiar la imagen de fondo en la página de inicio de sesión o de registro de Azure AD B2C en función de un parámetro que se pasa desde la aplicación web o dispositivo móvil.

### <a name="javascript"></a>JavaScript

Puede habilitar el código JavaScript del lado cliente en ambos [flujos de usuario](user-flow-javascript-overview.md) y [directivas personalizadas](page-layout.md).

### <a name="sign-in-only-ui-customization"></a>Personalización de la interfaz de usuario de solo inicio de sesión

Si solo proporciona la opción de inicio de sesión junto con la página de restablecimiento de contraseña y los correos electrónicos de comprobación, use los mismos pasos de personalización que se deben usar en una [página de inicio de sesión de Azure AD](../active-directory/fundamentals/customize-branding.md).

Si los clientes intentan editar su perfil antes de iniciar sesión, se les redirige a una página que usted mismo puede personalizar siguiendo los mismos pasos que se usan para personalizar la página de inicio de sesión de Azure AD.

## <a name="page-layout-templates"></a>Plantillas de diseño de página

Los flujos de usuario proporcionan varias plantillas integradas que puede elegir para dar a sus páginas de experiencia del usuario una apariencia profesional. Estas plantillas de diseño también pueden servir como punto de partida para su propia personalización.

En **Personalizar** en el menú de la izquierda, seleccione **Diseños de página** y, después, seleccione **Plantilla**.

![Lista desplegable de selección de plantilla en la página flujo de usuario de Azure Portal](media/customize-ui-overview/template-selection.png)

Después, seleccione una plantilla de la lista. Estos son algunos ejemplos de las páginas de inicio de sesión de cada plantilla:

| Azul océano | Gris pizarra | Clásico |
|:-:|:-:|:-:|
|![Ejemplo de la plantilla Ocean Blue que se representa en la página de inicio de sesión de suscripción](media/customize-ui-overview/template-ocean-blue.png)|![Ejemplo de la plantilla Gris pizarra que se representa en la página de inicio de sesión de suscripción](media/customize-ui-overview/template-slate-gray.png)|![Ejemplo de la plantilla Clásica que se representa en la página de inicio de sesión de suscripción](media/customize-ui-overview/template-classic.png)|

Cuando elige una plantilla, el diseño seleccionado se aplica a todas las páginas del flujo de usuarios y el identificador URI de cada página es visible en el campo **URI de página personalizado**.

## <a name="custom-html-and-css"></a>HTML y CSS personalizados

Si quiere crear su propio diseño de directiva con código HTML y CSS personalizado, active la opción "Usar contenido de la página personalizada" para cada uno de los nombres de diseño presentes en la directiva. Siga las instrucciones siguientes en relación con las configuraciones de diseño personalizadas:

Azure AD B2C ejecuta código en el explorador del cliente con un enfoque denominado [Uso compartido de recursos entre orígenes (CORS)](https://www.w3.org/TR/cors/).

En tiempo de ejecución, el contenido se carga desde una dirección URL especificada en un flujo de usuario o directiva personalizada. Cada página de la experiencia del usuario carga su contenido desde la dirección URL que especifique para esa página. Cuando el contenido se carga desde la dirección URL se combina con un fragmento HTML insertado en Azure AD B2C y, a continuación, se muestra la página al cliente.

Revise la siguiente guía antes de usar sus propios archivos HTML y CSS para personalizar la interfaz de usuario:

- Azure AD B2C combina **contenido** HTML en las páginas. No copie ni intente cambiar el contenido predeterminado que proporciona Azure AD B2C. Es preferible compilar su propio contenido de HTML desde cero y usar el contenido predeterminado como referencia.
- **JavaScript** puede incluirse en el contenido personalizado tanto para los [flujos de usuario](user-flow-javascript-overview.md) como para las [directivas personalizadas](javascript-samples.md).
- Las **versiones de explorador** admitidas son las siguientes:
  - Internet Explorer 11 y 10 y Microsoft Edge
  - Compatibilidad limitada con Internet Explorer 9 y 8.
  - Google Chrome 42.0 y versiones posteriores
  - Mozilla Firefox 38.0 y versiones posteriores
- No incluya **etiquetas de formulario** en el código HTML. Las etiquetas de formulario interfieren con las operaciones POST generadas por el código HTML que Azure AD B2C inserta.

### <a name="where-do-i-store-ui-content"></a>¿Dónde se puede almacenar el contenido de la interfaz de usuario?

Al utilizar sus propios archivos HTML y CSS para personalizar la interfaz de usuario, puede hospedar el contenido de la interfaz de usuario en cualquier punto de conexión HTTPS disponible públicamente que admita CORS. Por ejemplo, [Azure BLOB Storage](../storage/blobs/storage-blobs-introduction.md), servidores web, redes CDN, AWS S3 o sistemas de uso compartido de archivos.

Recuerde que podrá continuar trabajando siempre y cuando el contenido se hospede en un punto de conexión HTTPS disponible públicamente con CORS habilitado. Debe usar una dirección URL absoluta cuando la especifique en el contenido.

## <a name="get-started-with-custom-html-and-css"></a>Introducción al código HTML y CSS personalizado

Para empezar a usar su propio código HTML y CSS en las páginas de experiencia del usuario, siga estas instrucciones.

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

- Hospedar el contenido en un punto de conexión HTTPS (donde se permite CORS). Tenga en cuenta que los métodos de solicitud GET y OPTIONS se deben habilitar al configurar CORS.
- Cree o edite un flujo de usuario o una directiva personalizada para usar el contenido que haya creado.

### <a name="html-fragments-from-azure-ad-b2c"></a>Fragmentos HTML de Azure AD B2C

En la tabla siguiente se muestran fragmentos de HTML que Azure AD B2C combina en el elemento `<div id="api"></div>` que se encuentra en su contenido.

| Página insertada | Descripción de HTML |
| ------------- | ------------------- |
| Selección del proveedor de identidades | Contiene una lista de botones para los proveedores de identidades entre los que el usuario puede elegir durante el registro o el inicio de sesión. Estos botones incluyen proveedores de identidades sociales como Facebook y Google o las cuentas locales (que están basadas en una dirección de correo electrónico o un nombre de usuario). |
| Registro en una cuenta local | Contiene un formulario para el registro en una cuenta local basada en una dirección de correo electrónico o un nombre de usuario. El formulario puede contener diferentes controles de entrada, como un cuadro de entrada de texto, un cuadro de entrada de contraseña, un botón de radio, cuadros desplegables de selección única y casillas de verificación de selección múltiple. |
| Registro en una cuenta social | Puede aparecer al registrarse con una cuenta existente de un proveedor de identidades de redes sociales, como Facebook o Google. Se usa cuando se debe recopilar información adicional del cliente mediante un formulario de registro. |
| Inicio de sesión o registro unificado | Controla tanto el registro como el inicio de sesión de los clientes, los cuales pueden usar proveedores de identidades sociales como Facebook o Google, o cuentas locales. |
| Multi-Factor Authentication | Los usuarios pueden comprobar sus números de teléfono (mediante mensajes de texto o con la voz) durante el registro o el inicio de sesión. |
| Error | Proporciona información del error al cliente. |

## <a name="localize-content"></a>Localización del contenido

Puede localizar el contenido HTML si habilita la [personalización de idioma](user-flow-language-customization.md) en su inquilino de Azure AD B2C. Al habilitar esta característica, Azure AD B2C puede reenviar el parámetro `ui-locales` de OpenID Connect al punto de conexión. El servidor de contenido puede usar este parámetro para proporcionar páginas HTML específicas del idioma.

Como alternativa, puede extraer contenido de distintos lugares, según la configuración regional que use. En el punto de conexión habilitado para CORS, puede configurar una estructura de carpetas para hospedar contenido de idiomas concretos. Se llamará al adecuado si usa el valor de carácter comodín `{Culture:RFC5646}`.

Por ejemplo, el URI de la página personalizada podría ser similar al siguiente:

```HTTP
https://contoso.blob.core.windows.net/{Culture:RFC5646}/myHTML/unified.html
```

Puede cargar la página en francés mediante la extracción de contenido de:

```HTTP
https://contoso.blob.core.windows.net/fr/myHTML/unified.html
```

## <a name="examples"></a>Ejemplos

Puede encontrar varios archivos de plantilla de ejemplo en el repositorio [B2C-AzureBlobStorage-Client](https://github.com/azureadquickstarts/b2c-azureblobstorage-client) en GitHub.

Los archivos HTML y CSS de ejemplo de las plantillas se encuentran en el directorio [/sample_templates](https://github.com/AzureADQuickStarts/B2C-AzureBlobStorage-Client/tree/master/sample_templates).

## <a name="next-steps"></a>Pasos siguientes

- Si utiliza **flujos de usuario**, puede empezar a personalizar la interfaz de usuario con el tutorial:

    [Personalización de la interfaz de usuario de la aplicación en Azure Active Directory B2C](tutorial-customize-ui.md).
- Si usa **directivas personalizadas**, puede empezar a personalizar la interfaz de usuario con el artículo:

    [Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada en Azure Active Directory B2C](custom-policy-ui-customization.md).
