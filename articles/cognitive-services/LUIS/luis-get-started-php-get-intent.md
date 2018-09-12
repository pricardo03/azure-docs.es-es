---
title: 'Guía de inicio rápido de análisis de texto en lenguaje natural en Language Understanding (LUIS) mediante PHP en Cognitive Services: Azure Cognitive Services | Microsoft Docs'
description: En esta guía de inicio rápido, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional. Con PHP, envíe la intención del usuario como texto al punto de conexión de predicción HTTP de la aplicación pública. En el punto de conexión, LUIS aplica el modelo de la aplicación pública para analizar el texto en lenguaje natural y lo que significa, y así determinar la intención general y extraer los datos que son pertinentes para el dominio del sujeto de la aplicación.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/23/2018
ms.author: diberry
ms.openlocfilehash: 14ac32ffc0f4332d0cbc0da59a55ccc500e216d7
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "43771691"
---
# <a name="quickstart-analyze-text-using-php"></a>Guía de inicio rápido: análisis de texto con PHP

[!include[Quickstart introduction for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

<a name="create-luis-subscription-key"></a>

## <a name="prerequisites"></a>Requisitos previos

* Lenguaje de programación [PHP](http://php.net/)
* [Visual Studio Code](https://code.visualstudio.com/)
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2


[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-luis-repo-note.md)]

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-get-key-para.md)]

## <a name="analyze-text-with-browser"></a>Análisis de texto con el explorador

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-browser-para.md)]

## <a name="analyze-text-with-php"></a>Análisis de texto con PHP 

Puede usar PHP para acceder a los mismos resultados que vio en la ventana del explorador del paso anterior. 

1. Copie el código siguiente y guárdelo con el nombre de archivo `endpoint-call.php`:

   [!code-php[PHP code that calls a LUIS endpoint](~/samples-luis/documentation-samples/quickstarts/analyze-text/php/endpoint-call.php)]

2. Reemplace `"YOUR-KEY"` con su clave de punto de conexión.

3. Ejecute la aplicación PHP con `php endpoint-call.php`. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

## <a name="luis-keys"></a>Claves de LUIS

[!include[Use authoring key for endpoint](../../../includes/cognitive-services-luis-qs-endpoint-key-usage-para.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Elimine el archivo PHP.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de grabaciones de voz](luis-get-started-php-add-utterance.md)