---
title: Selección de un contrato de página en Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre cómo seleccionar un contrato de página en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: 39198c0093f018b64a1292f023914651b51b4faf
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403789"
---
# <a name="select-a-page-contract-in-azure-active-directory-b2c-using-custom-policies"></a>Selección de un contrato de página en Azure Active Directory B2C con directivas personalizadas

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Puede habilitar el código de JavaScript del lado cliente en las directivas de Azure Active Directory (Azure AD) B2C, independientemente de si usa flujos de usuario o directivas personalizadas. En este artículo se trata cómo seleccionar un contrato de página en Azure AD B2C mediante su configuración en una [directiva personalizada](active-directory-b2c-overview-custom.md). Un contrato de página es una asociación de elementos que proporciona Azure AD B2C y el contenido que proporciona el usuario. Si piensa usar [JavaScript](javascript-samples.md), deberá definir una versión del contrato de la página para todas las definiciones de contenido de la directiva personalizada.

> [!NOTE]
> Si quiere habilitar JavaScript para los flujos de usuario, consulte [About using JavaScript and page contract versions in a user flow](user-flow-javascript-overview.md) (Acerca del uso de las versiones del contrato de página y de JavaScript en un flujo de usuario).

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



