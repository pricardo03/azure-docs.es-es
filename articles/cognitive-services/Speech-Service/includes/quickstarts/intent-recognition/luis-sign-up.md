---
title: 'Inicio rápido: Reconocimiento de voz, intenciones y entidades en Python: servicio de voz'
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 12/27/2019
ms.author: erhopf
ms.openlocfilehash: aee5d6e1f6ed0519df7d1059b39f215d9f0d9091
ms.sourcegitcommit: 51ed913864f11e78a4a98599b55bbb036550d8a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2020
ms.locfileid: "75660577"
---
Para completar el inicio rápido de reconocimiento de la intención, deberá crear una cuenta de LUIS y un proyecto mediante el portal de la versión preliminar de LUIS. Este inicio rápido solo requiere una suscripción a LUIS. No se requiere una suscripción al servicio de voz.

Lo primero que debe hacer es crear una cuenta y una aplicación de LUIS mediante el portal de vista previa de LUIS. La aplicación de LUIS que cree usará un dominio precompilado para la automatización doméstica, que proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión de LUIS que se ejecuta en la nube al que puede llamar mediante el SDK de Voz. 

Siga estas instrucciones para crear una aplicación de LUIS: 

* <a href="https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-get-started-create-app" target="_blank">Inicio rápido: Compilación de una aplicación de un dominio creado previamente</a>

Cuando haya terminado, necesitará tres cosas: 

* Su clave de LUIS
* Su región de LUIS
* El identificador de la aplicación de LUIS

Aquí es donde puede encontrar esta información en el [portal de vista previa de LUIS](https://preview.luis.ai/):

1. En el portal de vista previa de LUIS, seleccione **Administrar**y, después, seleccione **Recursos de Azure**. En esta página, encontrará la clave y la ubicación de LUIS (que a veces se denomina _región_).  

   > [!div class="mx-imgBorder"]
   > ![Clave y ubicación de LUIS](../../../media/luis/luis-key-region.png)

2. Una vez que tenga la clave y ubicación, necesitará el identificador de la aplicación. Seleccione **Configuración de la aplicación** (el identificador de la aplicación está disponible en esta página).

   > [!div class="mx-imgBorder"]
   > ![Identificador de la aplicación de LUIS](../../../media/luis/luis-app-id.png)