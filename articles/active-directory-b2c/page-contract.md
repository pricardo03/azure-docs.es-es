---
title: 'Seleccione un contrato de página: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo seleccionar un contrato de página en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 4cd29df19179f07fd9b61a2f484b1d49cc05c4cf
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64570581"
---
# <a name="select-a-page-contract-in-azure-active-directory-b2c-using-custom-policies"></a>Selección de un contrato de página en Azure Active Directory B2C con directivas personalizadas

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Puede habilitar el código de JavaScript del lado cliente en las directivas de Azure Active Directory (Azure AD) B2C, independientemente de si usa flujos de usuario o directivas personalizadas. Para habilitar JavaScript para sus aplicaciones, debe agregar un elemento a su [directiva personalizada](active-directory-b2c-overview-custom.md), seleccione un contrato de página y use [b2clogin.com](b2clogin.md) en las solicitudes. Un contrato de página es una asociación de elementos que proporciona Azure AD B2C y el contenido que proporciona el usuario. En este artículo se describe cómo seleccionar un contrato de página en Azure AD B2C al configurarla en una directiva personalizada.

> [!NOTE]
> Si desea habilitar JavaScript para flujos de usuario, consulte [JavaScript y página del contrato de las versiones en Azure Active Directory B2C](user-flow-javascript-overview.md).

## <a name="replace-datauri-values"></a>Reemplazo de los valores de DataUri

En las directivas personalizadas, puede que tenga elementos [ContentDefinitions](contentdefinitions.md) que definen las plantillas HTML que se usan en el recorrido del usuario. El elemento **ContentDefinition** contiene un elemento **DataUri** que hace referencia a los elementos de página proporcionados por Azure AD B2C. El elemento **LoadUri** es la ruta de acceso relativa al contenido HTML y CSS que proporcione.

```XML
<ContentDefinition Id="api.idpselections">
  <LoadUri>~/tenant/default/idpSelector.cshtml</LoadUri>
  <RecoveryUri>~/common/default_page_error.html</RecoveryUri>
  <DataUri>urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0</DataUri>
  <Metadata>
    <Item Key="DisplayName">Idp selection page</Item>
    <Item Key="language.intro">Sign in</Item>
  </Metadata>
</ContentDefinition>
```

Para seleccionar un contrato de página, cambie los valores de **DataUri** en los elementos [ContentDefinitions](contentdefinitions.md) de las directivas. Al cambiar los antiguos valores de **DataUri** por los nuevos valores, está seleccionando un paquete inmutable. La ventaja de usar este paquete es que sabe que no cambiará y provocará un comportamiento inesperado en la página. 

Para configurar un contrato de página, utilice la siguiente tabla para encontrar los valores de **DataUri**. 

| Valor antiguo de DataUri | Nuevo valor DataUri |
| ----------------- | ----------------- |
| urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0 | 
| urn:com:microsoft:aad:b2c:elements:claimsconsent:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0 |
| urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0 | 
| urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0 |
| urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0 | urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0 |
| urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0 | urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0 |

## <a name="next-steps"></a>Pasos siguientes

Encuentre más información acerca de cómo personalizar la interfaz de usuario de las aplicaciones en [Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada en Azure Active Directory B2C](active-directory-b2c-ui-customization-custom.md).



