---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: include file
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.date: 11/05/2019
ms.author: diberry
ms.openlocfilehash: 39bfa5b6800c65112850faa8842b915ceedb9312
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73648069"
---
En la sección **Administrar** (menú de la parte superior derecha), en la página **Recursos de Azure** (menú izquierdo), copie la dirección URL de la **Consulta de ejemplo** y péguela en una nueva pestaña del explorador. 

La dirección URL del punto de conexión tiene un aspecto similar al siguiente; aunque sus propios valores de identificador de la aplicación y clave del punto de conexión reemplazarán al identificador de la aplicación y el identificador de clave:

```console
https://westus.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/APP-ID/slots/production/predict?subscription-key=KEY-ID&verbose=true&show-all-intents=true&log=true&query=YOUR_QUERY_HERE
```