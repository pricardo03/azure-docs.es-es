---
title: 'Localization: Azure Active Directory B2C'
description: Especifique el elemento Localization de una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 08/27/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 9a9fb1d64fb27507e4ee42c39be9ea80fbe5f168
ms.sourcegitcommit: 225a0b8a186687154c238305607192b75f1a8163
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/29/2020
ms.locfileid: "78184458"
---
# <a name="localization"></a>Localización

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

El elemento **Localization** permite incluir varios idiomas o configuraciones regionales en la directiva para los recorridos del usuario. Admitir la localización en las directivas le permite:

- Configurar la lista explícita de los idiomas admitidos en una directiva y seleccionar un idioma predeterminado.
- Proporcionar cadenas y colecciones específicas del idioma.

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
  <LocalizedResources Id="api.localaccountsignup.en">
  <LocalizedResources Id="api.localaccountsignup.es">
  ...
```

El elemento **Localization** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| habilitado | Sin | Valores posibles: `true` o `false`. |

El elemento **Localization** contiene los siguientes elementos XML:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| SupportedLanguages | 1:n | Lista de idiomas admitidos. |
| LocalizedResources | 0:n | Lista de recursos localizados. |

## <a name="supportedlanguages"></a>SupportedLanguages

El elemento **SupportedLanguages** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| DefaultLanguage | Sí | El idioma que se usará como el predeterminado para los recursos localizados. |
| MergeBehavior | Sin | Valores de enumeración de los valores combinados con cualquier elemento ClaimType presente en una directiva principal con el mismo identificador. Use este atributo al sobrescribir una notificación especificada en la directiva de base. Valores posibles: `Append`, `Prepend` o `ReplaceAll`. El valor `Append` especifica que la colección de datos presente tiene que anexarse al final de la colección especificada en la directiva principal. El valor `Prepend` especifica que la colección de datos presente tiene que agregarse antes de la colección especificada en la directiva principal. El valor `ReplaceAll` especifica que la colección de datos definidos en la directiva principal tiene que omitirse y, en su lugar, tienen que usarse los datos definidos en la directiva actual. |

### <a name="supportedlanguages"></a>SupportedLanguages

El elemento **SupportedLanguages** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| SupportedLanguage | 1:n | Muestra contenido que cumple con una etiqueta de idioma según RFC 5646 (etiquetas para identificar idiomas). |

## <a name="localizedresources"></a>LocalizedResources

El elemento **LocalizedResources** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Identificador | Sí | Identificador único para recursos localizados. |

El elemento **LocalizedResources** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| LocalizedCollections | 0:n | Define colecciones enteras de distintas referencias culturales. Una colección puede tener un número distinto de elementos y cadenas diferentes para varias referencias culturales. Entre los ejemplos de colecciones, se incluyen las enumeraciones que aparecen en los tipos de notificación. Por ejemplo, una lista de países o regiones se muestra al usuario en una lista desplegable. |
| LocalizedStrings | 0:n | Define todas las cadenas en distintas referencias culturales, excepto las que aparecen en colecciones. |

### <a name="localizedcollections"></a>LocalizedCollections

El elemento **LocalizedCollections** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| LocalizedCollection | 1:n | Lista de idiomas admitidos. |

#### <a name="localizedcollection"></a>LocalizedCollection

El elemento **LocalizedCollection** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ElementType | Sí | Hace referencia a un elemento ClaimType o a un elemento de la interfaz de usuario en el archivo de directiva. |
| ElementId | Sí | Cadena que contiene una referencia a un tipo de notificación que ya se ha definido en la sección ClaimsSchema y que se usa si se establece **ElementType** en un elemento ClaimType. |
| TargetCollection | Sí | La colección de destino. |

El elemento **LocalizedCollection** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| Elemento | 0:n | Define una opción disponible para que el usuario seleccione una notificación en la interfaz de usuario, como un valor en una lista desplegable. |

El elemento **Item** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| Texto | Sí | Cadena para mostrar fácil de identificar que se mostrará al usuario en la interfaz de usuario para esta opción. |
| Value | Sí | Valor de notificación de cadena asociado a la selección de esta opción. |
| SelectByDefault | Sin | Indica si esta opción se puede seleccionar o no de forma predeterminada en la interfaz de usuario. Valores posibles: True o False. |

En el ejemplo siguiente, se muestra cómo usar el elemento **LocalizedCollections**. Contiene dos elementos **LocalizedCollection**, uno para inglés y otro para español. Ambos establecen la colección **Restriction** de la notificación `Gender` con una lista de elementos para inglés y español.

```XML
<LocalizedResources Id="api.selfasserted.en">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>

<LocalizedResources Id="api.selfasserted.es">
 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
</LocalizedCollections>
```

### <a name="localizedstrings"></a>LocalizedStrings

El elemento **LocalizedStrings** contiene los elementos siguientes:

| Elemento | Repeticiones | Descripción |
| ------- | ----------- | ----------- |
| LocalizedString | 1:n | Cadena localizada. |

El elemento **LocalizedString** contiene los atributos siguientes:

| Atributo | Obligatorio | Descripción |
| --------- | -------- | ----------- |
| ElementType | Sí | Referencia a un elemento de tipo de notificación o a un elemento de la interfaz de usuario en la directiva. Valores posibles: `ClaimType`, `UxElement`, `ErrorMessage` o `Predicate`. El valor `ClaimType` se usa para localizar uno de los atributos de la notificación, según se especifica en el elemento StringId. El valor `UxElement` se usa para localizar uno de los elementos de la interfaz de usuario, según se especifica en el elemento StringId. El valor `ErrorMessage` se usa para localizar uno de los mensajes de error del sistema, según se especifica en el elemento StringId. El valor `Predicate` se usa para localizar uno de los mensajes de error de [Predicate](predicates.md), según se especifica en el elemento StringId. El valor `InputValidation` se usa para localizar uno de los mensajes de error de grupo de [PredicateValidation](predicates.md), según se especifica en el elemento StringId. |
| ElementId | Sí | Si **ElementType** se establece en `ClaimType`, `Predicate` o `InputValidation`, este elemento contiene una referencia a un tipo de notificación que ya se ha definido en la sección ClaimsSchema. |
| StringId | Sí | Si **ElementType** se establece en `ClaimType`, este elemento contiene una referencia a un atributo de un tipo de notificación. Valores posibles: `DisplayName`, `AdminHelpText` o `PatternHelpText`. El valor `DisplayName` se usa para establecer el nombre para mostrar de la notificación. El valor `AdminHelpText` se usa para establecer el nombre del texto de ayuda del usuario de la notificación. El valor `PatternHelpText` se usa para establecer el texto de ayuda del patrón de la notificación. Si **ElementType** se establece en `UxElement`, este elemento contiene una referencia a un atributo de un elemento de la interfaz de usuario. Si **ElementType** se establece en `ErrorMessage`, este elemento especifica el identificador de un mensaje de error. Vea la lista completa de los identificadores de `UxElement` en [Id. de cadena Localization](localization-string-ids.md).|


En el ejemplo siguiente, se muestra una página de registro localizada. Los primeros tres valores de **LocalizedString** establecen el atributo de la notificación. El tercero cambia el valor del botón Continuar. El último cambia el mensaje de error.

```XML
<LocalizedResources Id="api.selfasserted.en">
  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

En el ejemplo siguiente, se muestra una localización de **UserHelpText** de **Predicate** con el id. `IsLengthBetween8And64`. Y un elemento **UserHelpText** localizado de **PredicateGroup** con id. `CharacterClasses` de **PredicateValidation** con id. `StrongPassword`.

```XML
<PredicateValidation Id="StrongPassword">
  <PredicateGroups>
    ...
    <PredicateGroup Id="CharacterClasses">
    ...
    </PredicateGroup>
  </PredicateGroups>
</PredicateValidation>

...

<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
  ...
</Predicate>
...


<LocalizedString ElementType="InputValidation" ElementId="StrongPassword" StringId="CharacterClasses">The password must have at least 3 of the following:</LocalizedString>

<LocalizedString ElementType="Predicate" ElementId="IsLengthBetween8And64" StringId="HelpText">The password must be between 8 and 64 characters.</LocalizedString>
```

## <a name="set-up-localization"></a>Configurar la localización

En este artículo, se muestra cómo admitir varios idiomas o configuraciones regionales en la directiva para los recorridos del usuario. Localization necesita tres pasos: configurar la lista explícita de idiomas admitidos; proporcionar las colecciones y cadenas específicas del idioma; y editar el elemento ContentDefinition de la página.

### <a name="set-up-the-explicit-list-of-supported-languages"></a>Configurar la lista explícita de idiomas admitidos

En el elemento **BuildingBlocks**, agregue el elemento **Localization** con la lista de idiomas admitidos. En el ejemplo siguiente, se muestra cómo definir la compatibilidad de localización para inglés (predeterminado) y español:

```XML
<Localization Enabled="true">
  <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
    <SupportedLanguage>en</SupportedLanguage>
    <SupportedLanguage>es</SupportedLanguage>
  </SupportedLanguages>
</Localization>
```

### <a name="provide-language-specific-strings-and-collections"></a>Proporcionar cadenas y colecciones específicas del idioma

Agregue los elementos **LocalizedResources** dentro del elemento **Localization** después de cerrar el elemento **SupportedLanguages**. Agregue los elementos **LocalizedResources** por cada página (definición de contenido) e idioma que quiera admitir. Para personalizar la página unificada de registro o inicio de sesión, las páginas de registro y autenticación multifactor (MFA) para inglés, español y francés, agregue los siguientes elementos **LocalizedResources**.

- Página unificada de registro o inicio de sesión, inglés `<LocalizedResources Id="api.signuporsignin.en">`
- Página unificada de registro o inicio de sesión, español `<LocalizedResources Id="api.signuporsignin.es">`
- Página unificada de registro o inicio de sesión, francés `<LocalizedResources Id="api.signuporsignin.fr">`
- Registro, inglés `<LocalizedResources Id="api.localaccountsignup.en">`
- Registro, español `<LocalizedResources Id="api.localaccountsignup.es">`
- Registro, francés `<LocalizedResources Id="api.localaccountsignup.fr">`
- MFA, inglés `<LocalizedResources Id="api.phonefactor.en">`
- MFA, español `<LocalizedResources Id="api.phonefactor.es">`
- MFA, francés `<LocalizedResources Id="api.phonefactor.fr">`

Cada elemento **LocalizedResources** contiene todos los elementos **LocalizedStrings** necesarios con varios elementos **LocalizedString**, así como elementos **LocalizedCollections** con varios elementos **LocalizedCollection**.  En la ejemplo siguiente, se agrega la localización de inglés para la página de registro:

Nota: En este ejemplo, se realiza una referencia a los tipos de notificación `Gender` y `City`. Para usar este ejemplo, asegúrese de definir esas notificaciones. Para obtener más información, vea [ClaimsSchema](claimsschema.md).

```XML
<LocalizedResources Id="api.localaccountsignup.en">

 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Female" Value="F" />
      <Item Text="Male" Value="M" />
    </LocalizedCollection>
   <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
      <Item Text="New York" Value="NY" />
      <Item Text="Paris" Value="Paris" />
      <Item Text="London" Value="London" />
    </LocalizedCollection>
  </LocalizedCollections>

  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

Localización de la página de registro para español.

```XML
<LocalizedResources Id="api.localaccountsignup.es">

 <LocalizedCollections>
   <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
      <Item Text="Femenino" Value="F" />
      <Item Text="Masculino" Value="M" />
    </LocalizedCollection>
   <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
      <Item Text="Nueva York" Value="NY" />
      <Item Text="París" Value="Paris" />
      <Item Text="Londres" Value="London" />
    </LocalizedCollection>
  </LocalizedCollections>

  <LocalizedStrings>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Dirección de correo electrónico</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Dirección de correo electrónico que puede usarse para ponerse en contacto con usted.</LocalizedString>
    <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Introduzca una dirección de correo electrónico.</LocalizedString>
    <LocalizedString ElementType="UxElement" StringId="button_continue">Crear</LocalizedString>
   <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">Ya existe un usuario con el id. especificado. Elija otro diferente.</LocalizedString>
  </LocalizedStrings>
</LocalizedResources>
```

### <a name="edit-the-contentdefinition-for-the-page"></a>Edición de ContentDefinition para la página

Por cada página que quiera localizar, especifique los códigos de idioma que tienen que buscarse en el elemento **ContentDefinition**.

En el ejemplo siguiente, las cadenas personalizadas inglés (en) y español (es) se agregan a la página de registro. El elemento **LocalizedResourcesReferenceId** por cada elemento **LocalizedResourcesReference** es el mismo que la configuración regional, pero puede usarse cualquier cadena como el identificador. Por cada combinación de idioma y página, apunte al elemento **LocalizedResources** correspondiente que ha creado anteriormente.

```XML
<ContentDefinition Id="api.localaccountsignup">
...
  <LocalizedResourcesReferences MergeBehavior="Prepend">
    <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
    <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
  </LocalizedResourcesReferences>
</ContentDefinition>
```

En el ejemplo siguiente, se muestra el XML final:

```XML
<BuildingBlocks>
  <ContentDefinitions>
    <ContentDefinition Id="api.localaccountsignup">
      <!-- Other content definitions elements... -->
      <LocalizedResourcesReferences MergeBehavior="Prepend">
         <LocalizedResourcesReference Language="en" LocalizedResourcesReferenceId="api.localaccountsignup.en" />
         <LocalizedResourcesReference Language="es" LocalizedResourcesReferenceId="api.localaccountsignup.es" />
      </LocalizedResourcesReferences>
    </ContentDefinition>
    <!-- More content definitions... -->
  </ContentDefinitions>

  <Localization Enabled="true">

    <SupportedLanguages DefaultLanguage="en" MergeBehavior="ReplaceAll">
      <SupportedLanguage>en</SupportedLanguage>
      <SupportedLanguage>es</SupportedLanguage>
      <!-- More supported language... -->
    </SupportedLanguages>

    <LocalizedResources Id="api.localaccountsignup.en">
      <LocalizedCollections>
        <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
          <Item Text="Female" Value="F" />
          <Item Text="Male" Value="M" />
          <!-- More items... -->
        </LocalizedCollection>
        <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
          <Item Text="New York" Value="NY" />
          <Item Text="Paris" Value="Paris" />
          <Item Text="London" Value="London" />
        </LocalizedCollection>
        <!-- More localized collections... -->
      </LocalizedCollections>
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Email</LocalizedString>
      <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Please enter your email</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Please enter a valid email address</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_continue">Create new account</LocalizedString>
       <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">The account you are trying to create already exists, please sign-in.</LocalizedString>
        <!-- More localized strings... -->
      </LocalizedStrings>
    </LocalizedResources>

    <LocalizedResources Id="api.localaccountsignup.es">
      <LocalizedCollections>
       <LocalizedCollection ElementType="ClaimType" ElementId="Gender" TargetCollection="Restriction">
          <Item Text="Femenino" Value="F" />
          <Item Text="Masculino" Value="M" />
        </LocalizedCollection>
        <LocalizedCollection ElementType="ClaimType" ElementId="City" TargetCollection="Restriction">
          <Item Text="Nueva York" Value="NY" />
          <Item Text="París" Value="Paris" />
          <Item Text="Londres" Value="London" />
        </LocalizedCollection>
      </LocalizedCollections>
      <LocalizedStrings>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="DisplayName">Dirección de correo electrónico</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="UserHelpText">Dirección de correo electrónico que puede usarse para ponerse en contacto con usted.</LocalizedString>
        <LocalizedString ElementType="ClaimType" ElementId="email" StringId="PatternHelpText">Introduzca una dirección de correo electrónico.</LocalizedString>
        <LocalizedString ElementType="UxElement" StringId="button_continue">Crear</LocalizedString>
      <LocalizedString ElementType="ErrorMessage" StringId="UserMessageIfClaimsPrincipalAlreadyExists">Ya existe un usuario con el id. especificado. Elija otro diferente.</LocalizedString>
      </LocalizedStrings>
    </LocalizedResources>
    <!-- More localized resources... -->
  </Localization>
</BuildingBlocks>
```
