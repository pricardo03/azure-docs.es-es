---
title: 'Selección de un diseño de página: Azure Active Directory B2C'
description: Obtenga información sobre cómo seleccionar un diseño de página en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 452687f3886a85bea796e3899410667ee1d592fa
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76840322"
---
# <a name="select-a-page-layout-in-azure-active-directory-b2c-using-custom-policies"></a>Selección de un diseño de página en Azure Active Directory B2C mediante directivas personalizadas

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

Puede habilitar el código de JavaScript del lado cliente en las directivas de Azure Active Directory B2C (Azure AD B2C), independientemente de si usa flujos de usuario o directivas personalizadas. Para habilitar JavaScript para sus aplicaciones, debe agregar un elemento a su [directiva personalizada](custom-policy-overview.md), seleccionar un diseño de página y usar [b2clogin.com](b2clogin.md) en las solicitudes.

Un diseño de página es una asociación de elementos que proporciona Azure AD B2C y el contenido que proporciona el usuario.

En este artículo se trata cómo seleccionar un diseño de página en Azure AD B2C mediante su configuración en una directiva personalizada.

> [!NOTE]
> Si quiere habilitar JavaScript para los flujos de usuario, consulte [Versiones de diseño de página y JavaScript en Azure Active Directory B2C](user-flow-javascript-overview.md).

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

Para seleccionar un diseño de página, cambie los valores de **DataUri** en los elementos [ContentDefinitions](contentdefinitions.md) de las directivas. Al cambiar los antiguos valores de **DataUri** por los nuevos valores, está seleccionando un paquete inmutable. La ventaja de usar este paquete es que sabe que no cambiará y provocará un comportamiento inesperado en la página.

Para especificar un diseño de página en las directivas personalizadas que utilizan un valor de **DataUri** antiguo, inserte `contract` entre `elements` y el tipo de página (por ejemplo, `selfasserted`) y especifique el número de versión. Por ejemplo:

| Valor antiguo de DataUri | Nuevo valor DataUri |
| ----------------- | ----------------- |
| `urn:com:microsoft:aad:b2c:elements:claimsconsent:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:claimsconsent:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:globalexception:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:globalexception:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:idpselection:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:providerselection:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:multifactor:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:multifactor:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:selfasserted:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:selfasserted:1.1.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssd:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssd:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.0.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.0.0` |
| `urn:com:microsoft:aad:b2c:elements:unifiedssp:1.1.0` | `urn:com:microsoft:aad:b2c:elements:contract:unifiedssp:1.1.0` |

## <a name="version-change-log"></a>Registro de cambios de versión

Los paquetes de diseño de página se actualizan periódicamente para incluir correcciones y mejoras en sus elementos de la página. El registro de cambios siguiente especifica los cambios introducidos en cada versión.

### <a name="200"></a>2.0.0

- Página autoafirmada (`selfasserted`)
  - Se ha agregado compatibilidad para los [controles de visualización](display-controls.md) en las directivas personalizadas.

### <a name="120"></a>1.2.0

- Todas las páginas
  - Correcciones de accesibilidad
  - Ahora puede agregar el atributo `data-preload="true"` en las etiquetas HTML para controlar el orden de carga de CSS y JavaScript. Los escenarios incluyen:
    - Usarlo en el vínculo de CSS para cargar el CSS al mismo tiempo que el código HTML para que no parpadee en la carga de los archivos
    - Este atributo le permite controlar el orden en el que se capturan y ejecutan las etiquetas de Script antes de la carga de la página.
  - El campo de correo electrónico es ahora `type=email` y los teclados para móviles proporcionarán las sugerencias correctas.
  - Compatibilidad con la traducción de Chrome
- Página unificada y autoafirmada
  - Los campos de nombre de usuario/correo electrónico y contraseña ahora usan el elemento de formulario HTML.  Ahora se permitirá que Edge e IE guarden esta información correctamente

### <a name="110"></a>1.1.0

- Página de excepciones (globalexception)
  - Corrección de accesibilidad.
  - Se ha quitado el mensaje predeterminado cuando no hay ningún contacto de la directiva.
  - Se ha quitado la hoja CSS predeterminada.
- Página de MFA (multifactor)
  - Se ha quitado el botón “Confirmar código”.
  - El campo de entrada del código ahora solo acepta la entrada de hasta seis (6) caracteres.
  - La página intentará comprobar el código introducido automáticamente cuando se introduzca un código de 6 dígitos, sin tener que hacer clic en ningún botón.
  - Si el código es incorrecto, el campo de entrada se borra automáticamente.
  - Después de tres (3) intentos con un código incorrecto, B2C reenvía un error al usuario de confianza.
  - Correcciones de accesibilidad
  - Se ha quitado la hoja CSS predeterminada.
- Página autoafirmada (selfasserted)
  - Se ha quitado la alerta de cancelación.
  - Clase CSS para elementos de error.
  - Se ha mejorado la lógica para mostrar u ocultar errores.
  - Se ha quitado la hoja CSS predeterminada.
- SSP unificado (unifiedssp)
  - Se ha agregado el control para mantener la sesión iniciada (KMSI)

### <a name="100"></a>1.0.0

- Versión inicial

## <a name="next-steps"></a>Pasos siguientes

Encuentre más información acerca de cómo personalizar la interfaz de usuario de las aplicaciones en [Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada en Azure Active Directory B2C](custom-policy-ui-customization.md).
