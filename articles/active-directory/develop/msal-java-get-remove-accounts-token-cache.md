---
title: Obtención y eliminación de cuentas de la memoria caché de tokens mediante MSAL para Java (MSAL4j)
titleSuffix: Microsoft identity platform
description: Aprenda a ver y eliminar cuentas de la caché de tokens con la biblioteca de autenticación de Microsoft para Java.
services: active-directory
documentationcenter: dev-center-name
author: sangonzal
manager: henrikm
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/07/2019
ms.author: sagonzal
ms.reviewer: navyasri.canumalla
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 343abd87d3b5e8b82989b8b370cef61ec6d051df
ms.sourcegitcommit: bc193bc4df4b85d3f05538b5e7274df2138a4574
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2019
ms.locfileid: "73905169"
---
# <a name="get-and-remove-accounts-from-the-token-cache-using-msal-for-java-msal4j"></a>Obtención y eliminación de cuentas de la memoria caché de tokens mediante MSAL para Java (MSAL4j)

MSAL4J proporciona una caché de tokens en memoria de forma predeterminada. La caché de tokens en memoria tiene la misma duración que la instancia de la aplicación.

## <a name="see-which-accounts-are-in-the-cache"></a>Consulta de las cuentas que están en la memoria caché

Puede comprobar qué cuentas se encuentran en la memoria caché mediante una llamada a `PublicClientApplication.getAccounts()`, tal y como se muestra en el ejemplo siguiente:

```java
PublicClientApplication pca = new PublicClientApplication.Builder(
                labResponse.getAppId()).
                authority(TestConstants.ORGANIZATIONS_AUTHORITY).
                build();

Set<IAccount> accounts = pca.getAccounts().join();
```

## <a name="remove-accounts-from-the-cache"></a>Quitar cuentas de la memoria caché

Para quitar una cuenta de la memoria caché, busque la cuenta que tiene que quitar y realice una llamada a `PublicClientApplicatoin.removeAccount()`, tal y como se muestra en el ejemplo siguiente:

```java
Set<IAccount> accounts = pca.getAccounts().join();

IAccount accountToBeRemoved = accounts.stream().filter(
                x -> x.username().equalsIgnoreCase(
                        UPN_OF_USER_TO_BE_REMOVED)).findFirst().orElse(null);

pca.removeAccount(accountToBeRemoved).join();
```

## <a name="learn-more"></a>Más información

Si usa MSAL para Java, obtenga información sobre [Serialización de la memoria caché de tokens personalizada en MSAL para Java](msal-java-token-cache-serialization.md).
