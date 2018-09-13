---
title: Adición de ADFS como proveedor de identidades de SAML mediante directivas personalizadas en Azure Active Directory B2C | Microsoft Docs
description: Configurar ADFS 2016 mediante el protocolo SAML y las directivas personalizadas en Azure Active Directory B2C
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 08/31/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: 2c2e6861fda42a9e8c1aabcba303bfede47ac3c1
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43669233"
---
# <a name="add-adfs-as-a-saml-identity-provider-using-custom-policies-in-azure-active-directory-b2c"></a>Agregar ADFS como proveedor de identidades de SAML mediante las directivas personalizadas de Azure Active Directory B2C

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

En este artículo se muestra cómo habilitar el inicio de sesión para una cuenta de usuario de ADFS mediante el uso de [directivas personalizadas](active-directory-b2c-overview-custom.md) en Azure Active Directory (Azure AD) B2C.

## <a name="prerequisites"></a>Requisitos previos

Complete los pasos del artículo [Introducción a las directivas personalizadas](active-directory-b2c-get-started-custom.md).

## <a name="add-the-adfs-account-application-key-to-azure-ad-b2c"></a>Adición de la clave de aplicación de la cuenta de ADFS a Azure AD B2C

La federación con cuentas de ADFS requiere un secreto de cliente para que la cuenta de ADFS confíe en Azure AD B2C en nombre de la aplicación. Debe almacenar el certificado de ADFS en el inquilino de Azure AD B2C. 

1. Inicie sesión en [Azure Portal](https://portal.azure.com/).
2. Asegúrese de que está usando el directorio que contiene su inquilino de Azure AD B2C cambiando a él en la esquina superior derecha de Azure Portal. Seleccione **Cambiar directorio** y luego elija el directorio que contiene el inquilino que creó. En este tutorial, se usa el directorio *contoso* que contiene el inquilino denominado *contoso0522Tenant.onmicrosoft.com*.

    ![Cambio de directorios](./media/active-directory-b2c-custom-setup-adfs2016-idp/switch-directories.png)

3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, busque y seleccione **Azure AD B2C**. Ahora debería estar usando el inquilino.
4. En la página de introducción, seleccione **Identity Experience Framework**.
5. Seleccione **Claves de directiva** para ver las claves disponibles en el inquilino y haga clic en **Agregar**.
6. Elija **Cargar** como opción.
7. Escriba `ADFSSamlCert` como nombre. Es posible que se agregue automáticamente el prefijo `B2C_1A_`.
8. Busque el archivo .pfx de certificado con la clave privada y selecciónelo. Este certificado con la clave privada debe ser el mismo que el que se emite y se usa con el usuario de confianza de ADFS.
9. Haga clic en **Crear** y confirme que creó la clave `B2C_1A_ADFSSamlCert`.

## <a name="add-a-claims-provider-in-your-extension-policy"></a>Adición de un proveedor de notificaciones en la directiva de extensión

Si quiere que los usuarios inicien sesión mediante una cuenta de ADFS, debe definir la cuenta como proveedor de notificaciones. Para ello, especifique un punto de conexión con el que Azure AD B2C se comunica. El punto de conexión proporciona un conjunto de notificaciones que Azure AD B2C usa para comprobar que un usuario concreto se ha autenticado.

Defina ADFS como proveedor de notificaciones. Para ello, agregue el elemento **ClaimsProvider** en el archivo de directiva de extensión.

1. Abra el archivo de directiva *TrustFrameworkExtensions.xml* del directorio de trabajo. Si necesita un editor XML, [pruebe Visual Studio Code](https://code.visualstudio.com/download), un editor multiplataforma ligero.
2. Agregue el siguiente código XML en el elemento **ClaimsProviders**, reemplace **el dominio de ADFS** por el nombre de dominio de ADFS y reemplace el valor de la notificación de salida **identityProvider** por el DNS (valor arbitrario que indica el dominio). Luego guarde el archivo. 

    ```xml
    <ClaimsProvider>
      <Domain>contoso.com</Domain>
      <DisplayName>Contoso ADFS</DisplayName>
      <TechnicalProfiles>
        <TechnicalProfile Id="Contoso-SAML2">
          <DisplayName>Contoso ADFS</DisplayName>
          <Description>Login with your Contoso account</Description>
          <Protocol Name="SAML2"/>
          <Metadata>
            <Item Key="RequestsSigned">false</Item>
            <Item Key="WantsEncryptedAssertions">false</Item>
            <Item Key="PartnerEntity">https://your-ADFS-domain/federationmetadata/2007-06/federationmetadata.xml</Item>
          </Metadata>
          <CryptographicKeys>
            <Key Id="SamlAssertionSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
            <Key Id="SamlMessageSigning" StorageReferenceId="B2C_1A_ADFSSamlCert"/>
          </CryptographicKeys>
          <OutputClaims>
            <OutputClaim ClaimTypeReferenceId="socialIdpUserId" PartnerClaimType="userPrincipalName" />
            <OutputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="given_name"/>
            <OutputClaim ClaimTypeReferenceId="surname" PartnerClaimType="family_name"/>
            <OutputClaim ClaimTypeReferenceId="email" PartnerClaimType="email"/>
            <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="name"/>
            <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="contoso.com" />
            <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication"/>
          </OutputClaims>
          <OutputClaimsTransformations>
            <OutputClaimsTransformation ReferenceId="CreateRandomUPNUserName"/>
            <OutputClaimsTransformation ReferenceId="CreateUserPrincipalName"/>
            <OutputClaimsTransformation ReferenceId="CreateAlternativeSecurityId"/>
            <OutputClaimsTransformation ReferenceId="CreateSubjectClaimFromAlternativeSecurityId"/>
          </OutputClaimsTransformations>
          <UseTechnicalProfileForSessionManagement ReferenceId="SM-Noop"/>
        </TechnicalProfile>
      </TechnicalProfiles>
    </ClaimsProvider>
    ```

## <a name="register-the-claims-provider-for-sign-up-and-sign-in"></a>Registrar el proveedor de notificaciones para registrarse e iniciar sesión

Para que el proveedor de identidades de la cuenta de ADFS esté disponible en las páginas de registro e inicio de sesión, deberá agregarlo a su recorrido del usuario **SignUpOrSignIn**. 

Realice una copia de un recorrido del usuario de plantilla existente y luego modifíquelo para que incluya el proveedor de identidades de ADFS:

>[!NOTE]
>Si copió anteriormente el elemento **UserJourneys** del archivo base de la directiva en el archivo de extensión (*TrustFrameworkExtensions.xml*), puede omitir esta sección.

1. Abra el archivo base de la directiva. Por ejemplo, *TrustFrameworkBase.xml*.
2. Copie todo el contenido del elemento **UserJourneys**.
3. Abra el archivo de extensión (*TrustFrameworkExtensions.xml*) y pegue todo el contenido del elemento **UserJourneys** que copió en el archivo de extensión.

### <a name="display-the-button"></a>Visualización del botón

El elemento **ClaimsProviderSelections** define la lista de las selecciones del proveedor de notificaciones y su orden.  El elemento **ClaimsProviderSelection** es análogo a un botón de proveedor de identidades en una página de registro e inicio de sesión. Si agrega un elemento **ClaimsProviderSelection** para una cuenta de ADFS, se muestra un nuevo botón cuando un usuario ve la página. Para agregar este elemento:

1. En el elemento **UserJourney** con un identificador `SignUpOrSignIn` en los recorridos del usuario que copió, busque el elemento **OrchestrationStep** de `Order="1"`.
2. Agregue el siguiente elemento **ClaimsProviderSelection** debajo del elemento **ClaimsProviderSelections**:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

Ahora que hay un botón colocado, es preciso vincularlo a una acción. En este caso, la acción es para que Azure AD B2C se comunique con la cuenta de ADFS para recibir un token. Vincule el botón a una acción mediante la vinculación del perfil técnico del proveedor de notificaciones de la cuenta de ADFS:

1. Busque el elemento **OrchestrationStep** de `Order="2"` debajo del elemento **UserJourney**.
2. Agregar el siguiente elemento **ClaimsExchange** debajo del elemento **ClaimsExchanges**:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

> [!NOTE]
> * Asegúrese de que el valor de `Id` sea el mismo que el de `TargetClaimsExchangeId` en la sección anterior.
> * Asegúrese de que el valor de `TechnicalProfileReferenceId` se estabelzca en el perfil técnico que creó anteriormente (Contoso-SAML2).


## <a name="optional-register-the-claims-provider-for-profile-edit"></a>[Opcional] Registrar el proveedor de notificaciones para la edición del perfil

También puede agregar el proveedor de identidades de la cuenta de ADFS a su recorrido del usuario para la edición del perfil.

### <a name="display-the-button"></a>Visualización del botón

1. Abra el archivo de extensión de la directiva. Por ejemplo, *TrustFrameworkExtensions.xml*.
2. En el elemento **UserJourney** con un identificador `ProfileEdit` en los recorridos del usuario que copió, busque el elemento **OrchestrationStep** de `Order="1"`.
3. Agregue el siguiente elemento **ClaimsProviderSelection** debajo del elemento **ClaimsProviderSelections**:

    ```xml
    <ClaimsProviderSelection TargetClaimsExchangeId="ContosoExchange" />
    ```

### <a name="link-the-button-to-an-action"></a>Vincular el botón a una acción

1. Busque el elemento **OrchestrationStep** de `Order="2"` debajo del elemento **UserJourney**.
2. Agregar el siguiente elemento **ClaimsExchange** debajo del elemento **ClaimsExchanges**:

    ```xml
    <ClaimsExchange Id="ContosoExchange" TechnicalProfileReferenceId="Contoso-SAML2" />
    ```

## <a name="upload-the-policy-to-your-tenant"></a>Carga de la directiva en el inquilino

1. En Azure Portal, seleccione **Todas las directivas**.
2. Seleccione **Cargar directiva**.
3. Habilite **Sobrescribir la directiva si existe**.
4. Busque el archivo de directiva *TrustFrameworkExtensions.xml*, selecciónelo y luego seleccione **Cargar**. Asegúrese de que la validación sea correcta.


## <a name="configure-an-adfs-relying-party-trust"></a>Configurar una relación de confianza para usuarios de confianza de ADFS

Para usar ADFS como proveedor de identidades en Azure AD B2C, debe crear una relación de confianza para usuarios de confianza de ADFS con los metadatos de SAML de Azure AD B2C. En el ejemplo siguiente se muestra una dirección URL a los metadatos SAML de un perfil técnico de Azure AD B2C:

```
https://login.microsoftonline.com/te/your-tenant/your-policy/samlp/metadata?idptp=your-technical-profile
```

Reemplace los valores siguientes:

- **your-tenant** por el nombre del inquilino, como inquilino.onmicrosoft.com.
- **your-policy** por el nombre de la directiva. Use la directiva donde configura el perfil técnico del proveedor de SAML o una directiva que hereda de esa directiva.
- **your-technical-profile** por el nombre de su perfil técnico del proveedor de identidades de SAML.
 
Abra un explorador y vaya a esta dirección URL. Asegúrese de escribir la dirección URL correcta y que tenga acceso al archivo XML de metadatos.

Para agregar una nueva relación de confianza para usuarios de confianza mediante el complemento de administración de ADFS y configurar manualmente los valores, realice el procedimiento siguiente en un servidor de federación. Para realizar este procedimiento, se requiere al menos la pertenencia al grupo **Administradores**, o equivalente, en el equipo local. Revise los detalles sobre el uso de las cuentas adecuadas y las pertenencias a grupos en [Local and Domain Default Groups](http://go.microsoft.com/fwlink/?LinkId=83477) (Grupos locales y de dominio).

1. En el Administrador del servidor, seleccione **Herramientas** y luego **ADFS Management** (Administración de ADFS).
2. Seleccione **Agregar confianza para usuario de confianza**.
3. En la **página principal**, elija **Claims aware** (Compatible con notificaciones) y haga clic en **Iniciar**.
4. En la página **Seleccionar origen de datos**, seleccione **Import data about the relying party publish online or on a local network** (Importar datos sobre el usuario de confianza para publicar en línea o en una red local), proporcione la dirección URL de metadatos de Azure AD B2C y luego haga clic en **Siguiente**.
5. En la página **Especificar nombre para mostrar**, escriba un nombre en **Nombre para mostrar**. En **Notas** escriba una descripción de esta relación de confianza de usuario y luego haga clic en **Siguiente**.
6. En **Choose Access Control Policy** (Elegir directiva de control de acceso), seleccione una directiva y haga clic en **Siguiente**.
7. En la página **Ready to Add Trust** (Listo para agregar confianza), revise la configuración y luego haga clic en **Siguiente** para guardar la información de la relación de confianza para usuario autenticado.
8. En la página **Finalizar**, haga clic en **Cerrar**; esta acción muestra automáticamente el cuadro de diálogo **Edit Claim Rules** (Editar reglas de notificación).
9. Seleccione **Agregar regla**.  
10. En **Claim rule template** (Plantilla de regla de notificación), seleccione **Send LDAP attributes as claims** (Enviar atributos LDAP como notificaciones).
11. Proporcione un valor en **Claim rule name** (Nombre de la regla de notificación). Para **Attribute store** (Almacén de atributos), seleccione **Select Active Directory** (Seleccionar Active Directory), agregue las siguientes notificaciones y haga clic en **Finalizar** y luego en **Aceptar**.

    ![Establecer las propiedades de la regla](./media/active-directory-b2c-custom-setup-adfs2016-idp/aadb2c-ief-setup-adfs2016-idp-claims-3.png)

12.  Según el tipo de certificado, es posible que tenga que establecer el algoritmo HASH. En la ventana de propiedades de la relación de confianza de usuario (demo de B2C), haga clic en la pestaña **Avanzado** y cambie el valor de **Algoritmo hash seguro** a `SHA-1` o `SHA-256` y luego haga clic en **Aceptar**.  

### <a name="update-the-relying-party-metadata"></a>Actualizar los metadatos del usuario de confianza

El cambio del perfil técnico de SAML requiere la actualización de ADFS con la versión actualizada de los metadatos. No es necesario actualizar los metadatos al crear la aplicación del usuario de confianza, pero cuando se realiza un cambio, se actualizan los metadatos en ADFS.

1. En el Administrador del servidor, seleccione **Herramientas** y luego **ADFS Management** (Administración de ADFS).
2. Seleccione la relación de usuario de confianza que creó, seleccione **Actualizar a partir de los metadatos de federación** y luego haga clic en **Actualizar**. 

### <a name="test-the-policy-by-using-run-now"></a>Probar la directiva mediante Ejecutar ahora

1.  Abra la **configuración de Azure AD B2C** y vaya a **Marco de experiencia de identidad**.
2.  Abra **B2C_1A_ProfileEdit**, que es la directiva personalizada del usuario de confianza (RP) que ha cargado. Seleccione **Ejecutar ahora**. Debería poder iniciar sesión con la cuenta de ADFS.

## <a name="download-the-complete-policy-files"></a>Descarga de los archivos de directiva completos

Opcional: puede compilar el escenario con archivos de directiva personalizada propios tras completar los pasos de [Azure Active Directory B2C: introducción a las directivas personalizadas](active-directory-b2c-get-started-custom.md). Para obtener archivos de ejemplo, consulte los [archivos de ejemplo de directiva solo a modo de referencia](https://github.com/Azure-Samples/active-directory-b2c-custom-policy-starterpack/tree/master/scenarios/aadb2c-ief-setup-adfs2016-app).
