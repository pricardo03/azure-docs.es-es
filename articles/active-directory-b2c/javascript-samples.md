---
title: 'Ejemplos de JavaScript: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información acerca de cómo usar JavaScript en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 42dc09ef4518bfda8c63ee183499b1b2e8c22991
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841938"
---
# <a name="javascript-samples-for-use-in-azure-active-directory-b2c"></a>Ejemplos de JavaScript para usar en Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Puede agregar su propio código cliente de JavaScript a las aplicaciones de Azure Active Directory B2C (Azure AD B2C). Para habilitar JavaScript para sus aplicaciones, debe agregar un elemento a su [directiva personalizada](custom-policy-overview.md), seleccionar un [diseño de página](page-layout.md) y usar [b2clogin.com](b2clogin.md) en las solicitudes. En este artículo se describe cómo se puede cambiar su directiva personalizada para habilitar la ejecución del script.

> [!NOTE]
> Si quiere habilitar JavaScript para los flujos de usuario, consulte [Versiones de diseño de página y JavaScript en Azure Active Directory B2C](user-flow-javascript-overview.md).

## <a name="prerequisites"></a>Prerequisites

### <a name="select-a-page-layout"></a>Selección de un diseño de página

* [Seleccione un diseño de página](page-layout.md) para los elementos de la interfaz de usuario de su aplicación.

    Si piensa usar JavaScript, tendrá que [definir una versión del diseño de página](page-layout.md#replace-datauri-values) para *todas* las definiciones de contenido de la directiva personalizada.

## <a name="add-the-scriptexecution-element"></a>Adición del elemento ScriptExecution

Habilite la ejecución del script agregando el elemento **ScriptExecution** al elemento [RelyingParty](relyingparty.md).

1. Abra el archivo de la directiva personalizada. Por ejemplo, *SignUpOrSignin.xml*.
2. Agregue el elemento **ScriptExecution** al elemento **UserJourneyBehaviors** de **RelyingParty**:

    ```XML
    <RelyingParty>
      <DefaultUserJourney ReferenceId="B2CSignUpOrSignInWithPassword" />
      <UserJourneyBehaviors>
        <ScriptExecution>Allow</ScriptExecution>
      </UserJourneyBehaviors>
      ...
    </RelyingParty>
    ```
3. Guarde y cargue el archivo.

## <a name="guidelines-for-using-javascript"></a>Directrices para usar JavaScript

Siga estas directrices cuando personalice la interfaz de la aplicación con JavaScript:

- No enlace un evento de clic con elementos HTML `<a>`.
- No tome una dependencia del código Azure AD B2C ni de los comentarios.
- No cambie el orden ni la jerarquía de los elementos HTML de Azure AD B2C. Use una directiva de Azure AD B2C para controlar el orden de los elementos de la interfaz de usuario.
- Puede llamar a cualquier servicio RESTful con estas consideraciones:
    - Deberá establecer el servicio RESTful CORS para que permita llamadas HTTP del lado cliente.
    - Asegúrese de que el servicio RESTful es seguro y solo usa el protocolo HTTPS.
    - No use JavaScript directamente para llamar a puntos de conexión de Azure AD B2C.
- Puede incrustar el archivo JavaScript o puede vincular archivos JavaScript externos. Cuando use un archivo JavaScript externo, asegúrese de usar la dirección URL absoluta y no una relativa.
- Marcos de JavaScript:
    - Azure AD B2C usa una versión específica de jQuery. No incluya otra versión de jQuery. Usar más de una versión en la misma página provoca problemas.
    - No se admite el uso de RequireJS.
    - Azure AD B2C no admite la mayoría de los marcos de JavaScript.
- La configuración de Azure AD B2C puede leerse mediante una llamada a los objetos `window.SETTINGS` y `window.CONTENT`, como el idioma actual de la interfaz de usuario. No cambie el valor de estos objetos.
- Para personalizar el mensaje de error de Azure AD B2C, use la localización en una directiva.
- Si se puede lograr algo con una política, es la forma recomendada.

## <a name="javascript-samples"></a>Ejemplos de JavaScript

### <a name="show-or-hide-a-password"></a>Mostrar u ocultar una contraseña

Una forma habitual de ayudar a sus clientes para que se registren correctamente es permitirles ver la contraseña que escriben. Esta opción permite que los usuarios se registren permitiéndoles ver y realizar correcciones en su contraseña si es necesario. Cualquier campo de contraseña escrita tiene una casilla con la etiqueta **Mostrar contraseña**.  Esto permite al usuario ver la contraseña en texto sin formato. Incluya este fragmento de código en la plantilla de registro o de inicio de sesión para una página autoafirmada:

```Javascript
function makePwdToggler(pwd){
  // Create show-password checkbox
  var checkbox = document.createElement('input');
  checkbox.setAttribute('type', 'checkbox');
  var id = pwd.id + 'toggler';
  checkbox.setAttribute('id', id);

  var label = document.createElement('label');
  label.setAttribute('for', id);
  label.appendChild(document.createTextNode('show password'));

  var div = document.createElement('div');
  div.appendChild(checkbox);
  div.appendChild(label);

  // Add show-password checkbox under password input
  pwd.insertAdjacentElement('afterend', div);

  // Add toggle password callback
  function toggle(){
    if(pwd.type === 'password'){
      pwd.type = 'text';
    } else {
      pwd.type = 'password';
    }
  }
  checkbox.onclick = toggle;
  // For non-mouse usage
  checkbox.onkeydown = toggle;
}

function setupPwdTogglers(){
  var pwdInputs = document.querySelectorAll('input[type=password]');
  for (var i = 0; i < pwdInputs.length; i++) {
    makePwdToggler(pwdInputs[i]);
  }
}

setupPwdTogglers();
```

### <a name="add-terms-of-use"></a>Agregar términos de uso

Incluya el código siguiente en la página donde quiera la casilla **Términos de uso**. Normalmente, esta casilla suele ser necesaria en las páginas de registro de cuenta sociales o de cuentas locales.

```Javascript
function addTermsOfUseLink() {
    // find the terms of use label element
    var termsOfUseLabel = document.querySelector('#api label[for="termsOfUse"]');
    if (!termsOfUseLabel) {
        return;
    }

    // get the label text
    var termsLabelText = termsOfUseLabel.innerHTML;

    // create a new <a> element with the same inner text
    var termsOfUseUrl = 'https://docs.microsoft.com/legal/termsofuse';
    var termsOfUseLink = document.createElement('a');
    termsOfUseLink.setAttribute('href', termsOfUseUrl);
    termsOfUseLink.setAttribute('target', '_blank');
    termsOfUseLink.appendChild(document.createTextNode(termsLabelText));

    // replace the label text with the new element
    termsOfUseLabel.replaceChild(termsOfUseLink, termsOfUseLabel.firstChild);
}
```

En el código, reemplace `termsOfUseUrl` con el vínculo al contrato de los términos de uso. Para su directorio, cree un nuevo atributo de usuario denominado **termsOfUse** y, después, incluya **termsOfUse** como un atributo de usuario.

## <a name="next-steps"></a>Pasos siguientes

Encuentre más información acerca de cómo personalizar la interfaz de usuario de las aplicaciones en [Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada en Azure Active Directory B2C](custom-policy-ui-customization.md).
