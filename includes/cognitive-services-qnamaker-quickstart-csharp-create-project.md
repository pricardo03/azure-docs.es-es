---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: include
ms.custom: include file
ms.date: 09/27/2018
ms.author: diberry
ms.openlocfilehash: 420757c5b7f6d19bb0abe87232e75497b7b97322
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2018
ms.locfileid: "48019112"
---
1. Abra Visual Studio 2017 Community Edition.
1. Cree un nuevo proyecto de **aplicación de consola (.Net Core)** y asígnele el nombre `QnaMakerQuickstart`. Acepte los valores predeterminados del resto de la configuración.
1. En el Explorador de soluciones, haga clic con el botón derecho en el nombre del proyecto, **QnaMakerQuickstart** y, a continuación, seleccione **Administrar paquetes NuGet...** .
1. En la ventana de NuGet, seleccione **Explorador**, busque **Newtonsoft.JSON** e instale el paquete. Este paquete se usa para analizar el código JSON devuelto en la respuesta HTTP de QnaMaker. 