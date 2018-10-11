---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: e7b97d0d4489544275436b8048ca033480d53c59
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48842959"
---
## <a name="add-the-applications-registration-information-to-your-app"></a>Adición de información de registro de la aplicación a su aplicación
En este paso, debe agregar el identificador de aplicación a su proyecto.

1.  Abra `App.xaml.cs` y reemplace la línea que contiene el `ClientId` con:

```csharp
private static string ClientId = "[Enter the application Id here]";
```
