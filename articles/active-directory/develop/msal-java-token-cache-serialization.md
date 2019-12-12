---
title: Serialización de la memoria caché de tokens personalizada en MSAL para Java
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo serializar la memoria caché de tokens para MSAL para Java
services: active-directory
author: sangonzal
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: nacanuma
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: e2166cda772c358ed060b0e52a7410c7039fedf5
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74916543"
---
# <a name="custom-token-cache-serialization-in-msal-for-java"></a>Serialización de la memoria caché de tokens personalizada en MSAL para Java

Para persistir la memoria caché del token entre las instancias de la aplicación, es necesario personalizar la serialización. Las clases e interfaces de Java implicadas en la serialización de la memoria caché de tokens son las siguientes:

- [ITokenCache](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCache.html):  interfaz que representa la memoria caché de tokens de seguridad.
- [ITokenCacheAccessAspect](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessAspect.html): interfaz que representa la operación de ejecutar código antes y después del acceso. Se debe reemplazar con @Override *beforeCacheAccess* y *afterCacheAccess* con la lógica responsable de serializar y deserializar la memoria caché.
- [ITokenCacheContext](https://static.javadoc.io/com.microsoft.azure/msal4j/0.5.0-preview/com/microsoft/aad/msal4j/ITokenCacheAccessContext.html): interfaz que representa el contexto en el que se accede a la memoria caché de tokens. 

A continuación se muestra una implementación simple de serialización personalizada de la serialización y deserialización de la memoria caché de tokens. No lo copie y pegue en un entorno de producción.

```Java
static class TokenPersistence implements ITokenCacheAccessAspect {
String data;

TokenPersistence(String data) {
        this.data = data;
}

@Override
public void beforeCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        iTokenCacheAccessContext.tokenCache().deserialize(data);
}

@Override
public void afterCacheAccess(ITokenCacheAccessContext iTokenCacheAccessContext) {
        data = iTokenCacheAccessContext.tokenCache().serialize();
}
```

```Java
// Loads cache from file
String dataToInitCache = readResource(this.getClass(), "/cache_data/serialized_cache.json");

ITokenCacheAccessAspect persistenceAspect = new TokenPersistence(dataToInitCache);

// By setting *TokenPersistence* on the PublicClientApplication, MSAL will call *beforeCacheAccess()* before accessing the cache and *afterCacheAccess()* after accessing the cache. 
PublicClientApplication app = 
PublicClientApplication.builder("my_client_id").setTokenCacheAccessAspect(persistenceAspect).build();
```

## <a name="learn-more"></a>Más información

Obtenga información sobre cómo [obtener y eliminar cuentas de la memoria caché de tokens mediante MSAL para Java](msal-java-get-remove-accounts-token-cache.md).
