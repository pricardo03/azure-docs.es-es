---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/18/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 5e605e4ad987db16b98649b32dc96fa1620b1564
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "69011992"
---
La biblioteca cliente [Microsoft Azure App Authentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) para .NET (versión preliminar) simplifica el proceso de adquisición y renovación de un token desde el código. La biblioteca cliente App Authentication administra la autenticación automáticamente. La biblioteca usa las credenciales del desarrollador para la autenticación durante el desarrollo local. El uso de credenciales de desarrollador durante el desarrollo local es más seguro porque no es necesario crear credenciales de Azure AD o compartir credenciales entre los programadores. Cuando la solución se implementa más adelante en Azure, la biblioteca cambia automáticamente para usar las credenciales de la aplicación.

Para usar la biblioteca App Authentication en una aplicación de Azure Storage, instale el paquete de versión preliminar más reciente de [Nuget](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication), así como la versión más reciente de la [biblioteca cliente Azure Storage Common para .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Common/) y de la [biblioteca cliente Azure Blob Storage para .NET](https://www.nuget.org/packages/Microsoft.Azure.Storage.Blob/). Agregue las siguientes instrucciones **using** al código:

```csharp
using Microsoft.Azure.Services.AppAuthentication;
using Microsoft.Azure.Storage.Auth;
using Microsoft.Azure.Storage.Blob;
```
