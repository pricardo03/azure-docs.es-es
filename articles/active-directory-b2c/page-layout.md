---
title: Versiones de diseño de página
titleSuffix: Azure AD B2C
description: Historial de versiones de diseño de página de personalización de interfaz de usuario en directivas personalizadas.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 02/10/2020
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 2a0a786d3e2135467c7279c76bae273bff0ba2d0
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77149514"
---
# <a name="page-layout-versions"></a>Versiones de diseño de página

Los paquetes de diseño de página se actualizan periódicamente para incluir correcciones y mejoras en sus elementos de la página. El registro de cambios siguiente especifica los cambios introducidos en cada versión.

[!INCLUDE [active-directory-b2c-public-preview](../../includes/active-directory-b2c-public-preview.md)]

## <a name="200"></a>2.0.0

- Página autoafirmada (`selfasserted`)
  - Se ha agregado compatibilidad para los [controles de visualización](display-controls.md) en las directivas personalizadas.

## <a name="120"></a>1.2.0

- Todas las páginas
  - Correcciones de accesibilidad
  - Ahora puede agregar el atributo `data-preload="true"` en las etiquetas HTML para controlar el orden de carga de CSS y JavaScript.
    - Cargue los archivos CSS vinculados al mismo tiempo que la plantilla HTML para que no "vacile" durante la carga de los archivos.
    - Controle el orden en el que se capturan y ejecutan las etiquetas `script` antes de la carga de la página.
  - El campo de correo electrónico es ahora `type=email` y los teclados para móviles proporcionarán las sugerencias correctas.
  - Compatibilidad con la traducción de Chrome
- Páginas unificadas y autoafirmadas
  - Los campos de nombre de usuario/correo electrónico y contraseña ahora usan el elemento HTML `form` para permitir que Edge e Internet Explorer (IE) guarden correctamente esta información.

## <a name="110"></a>1.1.0

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

## <a name="100"></a>1.0.0

- Versión inicial

## <a name="next-steps"></a>Pasos siguientes

Para obtener detalles sobre cómo personalizar la interfaz de usuario de las aplicaciones en directivas personalizadas, vea [Personalización de la interfaz de usuario de la aplicación mediante una directiva personalizada](custom-policy-ui-customization.md).
