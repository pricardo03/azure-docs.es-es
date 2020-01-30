---
title: Incorporación de atributos a las directivas personalizadas
titleSuffix: Azure AD B2C
description: Un tutorial sobre cómo usar las propiedades de extensión y los atributos personalizados, y cómo incluirlos en la interfaz de usuario.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/04/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 04cc45956fc5aedc4c14dfb138be5db02ddec500
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76850499"
---
# <a name="azure-active-directory-b2c-use-custom-attributes-in-a-custom-profile-edit-policy"></a>Azure Active Directory B2C: uso de los atributos personalizados en una directiva de edición de perfil personalizada

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se creará un atributo personalizado en el directorio de Azure Active Directory B2C (Azure AD B2C). Este atributo nuevo se usará como notificación personalizada en el recorrido del usuario de la edición de perfil.

## <a name="prerequisites"></a>Prerequisites

Siga los pasos que aparecen en el artículo [Azure Active Directory B2C: introducción a las directivas personalizadas](custom-policy-get-started.md).

## <a name="use-custom-attributes-to-collect-information-about-your-customers-in-azure-ad-b2c-by-using-custom-policies"></a>Uso de los atributos personalizados para recopilar información sobre los clientes en Azure AD B2C mediante directivas personalizadas
El directorio de Azure AD B2C viene con un conjunto integrado de atributos. Algunos ejemplos son **Given Name**, **Surname**, **City**, **Postal Code** y **userPrincipalName**. A menudo, debe crear sus propios atributos como en estos ejemplos:
* Una aplicación orientada al cliente necesita persistir en un atributo como **LoyaltyNumber.**
* Un proveedor de identidades tiene un identificador de usuario único como **uniqueUserGUID** que se debe guardar.
* Un recorrido del usuario personalizado debe persistir en un estado de un usuario como **migrationStatus**.

Azure AD B2C extiende el conjunto de atributos almacenado en cada cuenta de usuario. También puede leer y escribir estos atributos mediante [Graph API de Azure AD](manage-user-accounts-graph-api.md).

Las propiedades de extensión extienden el esquema de los objetos de usuario en el directorio. En el contexto de este artículo, los términos *propiedad de extensión*, *atributo personalizado* y *notificación personalizada* se refieren a lo mismo. El nombre varía según el contexto, como aplicación, objeto o directiva.

Las propiedades de extensión solo se pueden registrar en un objeto de aplicación, aunque pueden contener datos de un usuario. La propiedad está conectada a la aplicación. El objeto de aplicación debe tener acceso de escritura para registrar una propiedad de extensión. Cien propiedades de extensión, de todos los tipos y todas las aplicaciones, se pueden escribir en un objeto individual. Las propiedades de extensión se agregan al tipo de directorio de destino y se puede acceder a ellas de inmediato en el inquilino del directorio de Azure AD B2C.
Si la aplicación se elimina, también se quitan las propiedades de extensión, junto con los datos que contienen de todos los usuarios. Si la aplicación elimina una propiedad de extensión, se quita en los objetos de directorio de destino, y también se quitan los valores.

Las propiedades de extensión solo existen en el contexto de una aplicación registrada en el inquilino. El identificador de objeto de la aplicación debe estar incluido en el elemento **TechnicalProfile** que lo utiliza.

>[!NOTE]
>El directorio de Azure AD B2C normalmente incluye una aplicación de web llamada `b2c-extensions-app`. Esta aplicación la usan principalmente las directivas integradas de B2C para las notificaciones personalizadas creadas a través de Azure Portal. Se recomienda que solo los usuarios avanzados registren las extensiones para las directivas personalizadas de B2C mediante esta aplicación.
Se incluyen instrucciones en la sección **Pasos siguientes** de este artículo.

## <a name="create-a-new-application-to-store-the-extension-properties"></a>Creación de una aplicación nueva para almacenar las propiedades de extensión

1. Abra una sesión de exploración y desplácese hasta el [portal de Azure](https://portal.azure.com). Inicie sesión con las credenciales administrativas del directorio B2C que quiere configurar.
2. Seleccione **Azure Active Directory** en el menú de navegación de la izquierda. Puede que tenga que seleccionar **Más servicios** para encontrarlo.
3. Seleccione **App registrations** (Registros de aplicaciones). Seleccione **Nuevo registro de aplicaciones**.
4. Proporcione las entradas siguientes:
    * Un nombre para la aplicación web: **WebApp-GraphAPI-DirectoryExtensions**.
    * El tipo de aplicación: **Aplicación web/API**.
    * La dirección URL de inicio de sesión: **https://{NombreInquilino}.onmicrosoft.com/WebApp-GraphAPI-DirectoryExtensions**.
5. Seleccione **Crear**.
6. Seleccione la aplicación web recién creada.
7. Seleccione **Configuración** > **Permisos requeridos**.
8. Seleccione la API de **Windows Azure Active Directory**.
9. Coloque una marca de verificación en Permisos de la aplicación: **Leer y escribir datos de directorio**. Después, seleccione **Guardar**.
10. Seleccione **Conceder permisos** y haga clic en **Sí** para confirmar.
11. Copie los identificadores siguientes en el Portapapeles y guárdelos:
    * **Identificador de aplicación**. Ejemplo: `103ee0e6-f92d-4183-b576-8c3739027780`.
    * **Identificador de objeto**. Ejemplo: `80d8296a-da0a-49ee-b6ab-fd232aa45201`.

## <a name="modify-your-custom-policy-to-add-the-applicationobjectid"></a>Modificación de la directiva personalizada para agregar **ApplicationObjectId**

Después de seguir los pasos que aparecen en el artículo [Azure Active Directory B2C: introducción a las directivas personalizadas](custom-policy-get-started.md), se descargan y modifican los [archivos](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/archive/master.zip) denominados **TrustFrameworkBase.xml**,  **TrustFrameworkExtensions.xml**, **SignUpOrSignin.xml**, **ProfileEdit.xml** y **PasswordReset.xml**. En este paso, va a realizar más modificaciones en esos archivos.

* Abra el archivo **TrustFrameworkBase.xml** y agregue la sección `Metadata` tal como se muestra en el ejemplo siguiente. Inserte el id. de objeto que se ha registrado previamente para el valor `ApplicationObjectId` y el id. de aplicación que registró para el valor `ClientId`:

    ```xml
    <ClaimsProviders>
      <ClaimsProvider>
        <DisplayName>Azure Active Directory</DisplayName>
        <TechnicalProfile Id="AAD-Common">
          <DisplayName>Azure Active Directory</DisplayName>
          <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />

          <!-- Provide objectId and appId before using extension properties. -->
          <Metadata>
            <Item Key="ApplicationObjectId">insert objectId here</Item>
            <Item Key="ClientId">insert appId here</Item>
          </Metadata>
          <!-- End of changes -->

          <CryptographicKeys>
            <Key Id="issuer_secret" StorageReferenceId="TokenSigningKeyContainer" />
          </CryptographicKeys>
          <IncludeInSso>false</IncludeInSso>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop" />
        </TechnicalProfile>
      </ClaimsProvider>
    </ClaimsProviders>
    ```

> [!NOTE]
> La primera vez que la instancia de **TechnicalProfile** escribe en la propiedad de extensión recién creada, puede surgir un error que se da una sola vez. La propiedad de extensión se crea la primera vez que se utiliza.

## <a name="use-the-new-extension-property-or-custom-attribute-in-a-user-journey"></a>Uso de la nueva propiedad de extensión o atributo personalizado en un recorrido del usuario

1. Abra el archivo **ProfileEdit.xml**.
2. Agregue una notificación personalizada `loyaltyId`. Mediante la inclusión de una notificación personalizada en el elemento `<RelyingParty>`, se incluye en el token para la aplicación.

    ```xml
    <RelyingParty>
      <DefaultUserJourney ReferenceId="ProfileEdit" />
      <TechnicalProfile Id="PolicyProfile">
        <DisplayName>PolicyProfile</DisplayName>
        <Protocol Name="OpenIdConnect" />
        <OutputClaims>
          <OutputClaim ClaimTypeReferenceId="objectId" PartnerClaimType="sub"/>
          <OutputClaim ClaimTypeReferenceId="city" />

          <!-- Provide the custom claim identifier -->
          <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
          <!-- End of changes -->
        </OutputClaims>
        <SubjectNamingInfo ClaimType="sub" />
      </TechnicalProfile>
    </RelyingParty>
    ```

3. Abra el archivo **TrustFrameworkExtensions.xml** y agregue el elemento `<ClaimsSchema>` y sus elementos secundarios al elemento `BuildingBlocks`:

    ```xml
    <BuildingBlocks>
      <ClaimsSchema>
        <ClaimType Id="extension_loyaltyId">
          <DisplayName>Loyalty Identification Tag</DisplayName>
          <DataType>string</DataType>
          <UserHelpText>Your loyalty number from your membership card</UserHelpText>
          <UserInputType>TextBox</UserInputType>
        </ClaimType>
      </ClaimsSchema>
    </BuildingBlocks>
    ```

4. Agregue la misma definición `ClaimType` a **TrustFrameworkBase.xml**. No es necesario agregar una definición `ClaimType` en el archivo base y el archivo de extensión. Sin embargo, en los pasos siguientes se agrega `extension_loyaltyId` a **TechnicalProfiles** en el archivo base. Por tanto, el validador de directiva rechaza la carga del archivo base sin él. Es posible que sea útil realizar un seguimiento de la ejecución del recorrido del usuario denominado **ProfileEdit** del archivo **TrustFrameworkBase.xml**. Busque el recorrido del usuario con el mismo nombre en el editor. Observe que en el paso 5 de orquestación invoca a **TechnicalProfileReferenceID="SelfAsserted-ProfileUpdate**. Busque e inspeccione esta instancia de **TechnicalProfile** para familiarizarse con el flujo.

5. Abra el archivo **TrustFrameworkBase.xml** y agregue `loyaltyId` como notificación de entrada y salida en el elemento **TechnicalProfile SelfAsserted-ProfileUpdate**:

    ```xml
    <TechnicalProfile Id="SelfAsserted-ProfileUpdate">
      <DisplayName>User ID signup</DisplayName>
      <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.SelfAssertedAttributeProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
      <Metadata>
        <Item Key="ContentDefinitionReferenceId">api.selfasserted.profileupdate</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="alternativeSecurityId" />
        <InputClaim ClaimTypeReferenceId="userPrincipalName" />
        <InputClaim ClaimTypeReferenceId="givenName" />
        <InputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <InputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="executed-SelfAsserted-Input" DefaultValue="true" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId"/>
        <!-- End of changes -->

      </OutputClaims>
      <ValidationTechnicalProfiles>
        <ValidationTechnicalProfile ReferenceId="AAD-UserWriteProfileUsingObjectId" />
      </ValidationTechnicalProfiles>
    </TechnicalProfile>
    ```

6. En el archivo **TrustFrameworkBase.xml**, agregue la notificación `loyaltyId` a **TechnicalProfile AAD-UserWriteProfileUsingObjectId**. Esta incorporación conserva el valor de la notificación de la propiedad de extensión para el usuario actual en el directorio:

    ```xml
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      <Metadata>
        <Item Key="Operation">Write</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalAlreadyExists">false</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <PersistedClaims>
        <PersistedClaim ClaimTypeReferenceId="objectId" />
        <PersistedClaim ClaimTypeReferenceId="givenName" />
        <PersistedClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <PersistedClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </PersistedClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

7. En el archivo **TrustFrameworkBase.xml**, agregue la notificación `loyaltyId` a **TechnicalProfile AAD-UserReadUsingObjectId** para leer el valor del atributo de extensión cada vez que un usuario inicia sesión. Hasta ahora, las instancias de **TechnicalProfiles** solo se han cambiado en el flujo de las cuentas locales. Si el nuevo atributo se desea en el flujo de una cuenta social o federada, es preciso cambiar otro conjunto de instancias de **TechnicalProfiles**. Consulte la sección **Pasos siguientes**.

    ```xml
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      <Metadata>
        <Item Key="Operation">Read</Item>
        <Item Key="RaiseErrorIfClaimsPrincipalDoesNotExist">true</Item>
      </Metadata>
      <IncludeInSso>false</IncludeInSso>
      <InputClaims>
        <InputClaim ClaimTypeReferenceId="objectId" Required="true" />
      </InputClaims>
      <OutputClaims>
        <OutputClaim ClaimTypeReferenceId="signInNames.emailAddress" />
        <OutputClaim ClaimTypeReferenceId="displayName" />
        <OutputClaim ClaimTypeReferenceId="otherMails" />
        <OutputClaim ClaimTypeReferenceId="givenName" />
        <OutputClaim ClaimTypeReferenceId="surname" />

        <!-- Add the loyalty identifier -->
        <OutputClaim ClaimTypeReferenceId="extension_loyaltyId" />
        <!-- End of changes -->

      </OutputClaims>
      <IncludeTechnicalProfile ReferenceId="AAD-Common" />
    </TechnicalProfile>
    ```

## <a name="test-the-custom-policy"></a>Prueba de la directiva personalizada

1. Abra la hoja Azure AD B2C y vaya a **Marco de experiencia de identidad** > **Directivas personalizadas**.
1. Seleccione la directiva personalizada que cargó. Seleccione **Ejecutar ahora**.
1. Regístrese con una dirección de correo electrónico.

El token del identificador que se devuelve a la aplicación incluirá la nueva propiedad de extensión como notificación personalizada precedida por **extension_loyaltyId**. Vea el ejemplo siguiente:

```json
{
  "exp": 1493585187,
  "nbf": 1493581587,
  "ver": "1.0",
  "iss": "https://contoso.b2clogin.com/f06c2fe8-709f-4030-85dc-38a4bfd9e82d/v2.0/",
  "sub": "a58e7c6c-7535-4074-93da-b0023fbaf3ac",
  "aud": "4e87c1dd-e5f5-4ac8-8368-bc6a98751b8b",
  "acr": "b2c_1a_trustframeworkprofileedit",
  "nonce": "defaultNonce",
  "iat": 1493581587,
  "auth_time": 1493581587,
  "extension_loyaltyId": "abc",
  "city": "Redmond"
}
```

## <a name="next-steps"></a>Pasos siguientes

1. Para agregar la nueva notificación a los flujos para iniciar sesión en cuentas sociales, cambie el siguiente **TechnicalProfiles**. Las cuentas de redes sociales y federadas usan estas dos instancias de **TechnicalProfiles** para iniciar sesión. Escriben y leen los datos de usuario mediante el uso de **alternativeSecurityId** como localizador del objeto de usuario.

   ```xml
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">

    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
   ```

2. Use los mismos atributos de extensión entre directivas integradas y personalizadas. Cuando se agregan atributos de extensión (o personalizados) a través de la experiencia del portal, los atributos se registran mediante **b2c-extensions-app** que existe en cada inquilino B2C. Siga estos pasos para usar los atributos de extensión en la directiva personalizada:

   a. En el inquilino B2C en portal.azure.com, vaya a **Azure Active Directory** y seleccione **Registros de aplicaciones**.
   b. Encuentre su **b2c-extensiones-app** y selecciónelo.
   c. En **Información esencial**, escriba el **Identificador de la aplicación** y el **Identificador del objeto**.
   d. Inclúyalos en los metadatos de TechnicalProfile de **AAD-Common**:

   ```xml
      <ClaimsProviders>
        <ClaimsProvider>
          <DisplayName>Azure Active Directory</DisplayName>
          <TechnicalProfile Id="AAD-Common">
            <DisplayName>Azure Active Directory</DisplayName>
            <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.AzureActiveDirectoryProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
            <!-- Provide objectId and appId before using extension properties. -->
            <Metadata>
              <Item Key="ApplicationObjectId">insert objectId here</Item> <!-- This is the "Object ID" from the "b2c-extensions-app"-->
              <Item Key="ClientId">insert appId here</Item> <!--This is the "Application ID" from the "b2c-extensions-app"-->
            </Metadata>
   ```

3. Mantenga la coherencia con la experiencia del portal. Cree estos atributos mediante la interfaz de usuario del portal antes de usarlos en las directivas personalizadas. Cuando se crea un atributo **ActivationStatus** en el portal, tiene que hacer referencia a él como sigue:

   ```
   extension_ActivationStatus in the custom policy.
   extension_<app-guid>_ActivationStatus via Graph API.
   ```

## <a name="reference"></a>Referencia

Para más información sobre las propiedades de extensión, consulte el artículo [Extensiones de esquema de directorio | Conceptos de Graph API](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions).

> [!NOTE]
> * **TechnicalProfile** es un tipo de elemento (o función) que define el nombre, los metadatos y el protocolo de un punto de conexión. **TechnicalProfile** detalla el intercambio de notificaciones que realiza el Marco de experiencia de identidad. Cuando se llama a esta función en un paso de la orquestación o desde otra instancia de **TechnicalProfile**, quien realiza la llamada especifica **InputClaims** y **OutputClaims** como parámetros.
> * Los atributos de extensión en Graph API se denominan mediante la convención `extension_ApplicationObjectID_attributename`.
> * Las directivas personalizadas hacen referencia a los atributos de extensión como **extension_attributename**. Esta referencia omite el valor de **ApplicationObjectId** en el XML.
> * Tiene que especificar el Id. de atributo en el formato siguiente **extensión_nombredeatributo** donde se haga referencia a él.
