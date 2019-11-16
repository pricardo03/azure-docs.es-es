---
title: Referencia del SDK de iOS del Lector inmersivo
titleSuffix: Azure Cognitive Services
description: El SDK de iOS del Lector inmersivo es un CocoaPod de Swift que permite integrar dicho lector en una aplicación iOS.
services: cognitive-services
author: metanMSFT
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 08/01/2019
ms.author: metan
ms.openlocfilehash: 67d6b8c22c5635bd789078a7f91b02f8b07e5e70
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2019
ms.locfileid: "73903120"
---
# <a name="immersive-reader-sdk-reference-for-ios"></a>Referencia del SDK del Lector inmersivo para iOS

El SDK de iOS del Lector inmersivo es un CocoaPod de Swift que permite integrar dicho lector en una aplicación iOS.

## <a name="functions"></a>Functions

El SDK expone una sola función: `launchImmersiveReader(navController, token, subdomain, content, options, onSuccess, onFailure)`.

### <a name="launchimmersivereader"></a>launchImmersiveReader

Inicia el Lector inmersivo. Para ello, inicia un controlador de vista en la aplicación de iOS.

```swift
public func launchImmersiveReader(navController: UINavigationController, token: String, subdomain: String, content: Content, options: Options?, onSuccess: @escaping () -> Void, onFailure: @escaping (_ error: Error) -> Void)
```

#### <a name="parameters"></a>Parámetros

| NOMBRE | type | DESCRIPCIÓN |
| ---- | ---- |------------ |
| `navController` | UINavigationController | El controlador de navegación de la aplicación iOS desde la que se llama a la función. |
| `token` | Cadena | Token de autenticación de Azure AD Consulte el [procedimiento de autenticación de Azure AD](./azure-active-directory-authentication.md). |
| `subdomain` | Cadena | El subdominio personalizado del recurso Lector inmersivo en Azure. Consulte el [procedimiento de autenticación de Azure AD](./azure-active-directory-authentication.md). |
| `content` | [Contenido](#content) | Un objeto que contiene el contenido que se mostrará en el Lector inmersivo. |
| `options` | [Opciones](#options) | Opciones para configurar ciertos comportamientos del Lector inmersivo. Opcional. |
| `onSuccess` | () -> Void | Un cierre que se invoca cuando el Lector inmersivo se inicia correctamente. |
| `onFailure` | (_ error: [Error](#error)) -> Void | Un cierre que se invoca cuando el Lector inmersivo no se carga. Este cierre devuelve un objeto [ `Error` ](#error) que representa un código de error y un mensaje de error asociado al error. Para más información, consulte los [códigos de error](#error-codes). |

## <a name="types"></a>Tipos

### <a name="content"></a>Contenido

Contiene el contenido que se mostrará en el Lector inmersivo.

```swift
struct Content: Encodable {
    var title: String
    var chunks: [Chunk]
}
```

#### <a name="supported-mime-types"></a>Tipos MIME compatibles

| Tipo MIME | DESCRIPCIÓN |
| --------- | ----------- |
| text/plain | Texto sin formato. |
| application/mathml+xml | Lenguaje de marcado matemático (MathML). [Más información](https://developer.mozilla.org/en-US/docs/Web/MathML).

### <a name="options"></a>Opciones

Contiene propiedades que configuran ciertos comportamientos del Lector inmersivo.

```swift
struct Options {
    var uiLang: String?
    var timeout: TimeInterval?
}
```

### <a name="error"></a>Error

Contiene información sobre el error.

```swift
struct Error {
    var code: String
    var message: String
}
```

#### <a name="error-codes"></a>Códigos de error

| Código | DESCRIPCIÓN |
| ---- | ----------- |
| BadArgument | El argumento proporcionado no es válido; consulte `message` para detalles. |
| Tiempo de espera | El Lector inmersivo no se pudo cargar en el tiempo de espera especificado. |
| TokenExpired | El token suministrado en caché ha expirado. |
| Limitado | Se ha superado el límite de frecuencia de llamadas. |
| InternalError | Se produjo un error interno en el controlador de vista del Lector inmersivo. En `message` encontrará los detalles.|

## <a name="os-version-support"></a>Compatibilidad con la versión del SO

El SDK de iOS del Lector inmersivo es compatible con iOS 9.0, o cualquier superior, en iPad y iPhone.

## <a name="next-steps"></a>Pasos siguientes

* Explore el [SDK de iOS del Lector inmersivo en GitHub](https://github.com/microsoft/immersive-reader-sdk/tree/master/iOS)
* [Inicio rápido: Creación de una aplicación iOS que inicia el Lector inmersivo (Swift)](./ios-quickstart.md)