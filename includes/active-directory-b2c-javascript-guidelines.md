---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 02/11/2020
ms.author: marsma
ms.openlocfilehash: 687853720e8f963d5a58093b824bb36e8063bcf7
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77149086"
---
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
