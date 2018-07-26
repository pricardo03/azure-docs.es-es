---
title: Descripción de los conceptos de conversión de datos en LUIS - Azure | Microsoft Docs
description: Obtenga información sobre cómo se pueden modificar las expresiones antes de las predicciones en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/27/2018
ms.author: diberry
ms.openlocfilehash: 05a02191af171936dd9f5c4bf469b9c84aa144a3
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223708"
---
# <a name="data-conversion-concepts-in-luis"></a>Conceptos de conversión de datos en LUIS
LUIS ofrece un método para convertir las expresiones de voz en texto antes de la predicción. 

## <a name="speech-to-intent-conversion-concepts"></a>Conceptos de la conversión de voz en intención
La conversión de voz en texto en LUIS permite enviar expresiones de voz a un punto de conexión y recibir una respuesta de predicción de LUIS. El proceso consiste en integrar el servicio [Speech](https://docs.microsoft.com/azure/cognitive-services/Speech) con LUIS. 

### <a name="key-requirements"></a>Requisitos principales
No es necesario crear una clave de **Bing Speech API** para esta integración. Para esta integración, solo servirá una clave de **Language Understanding** creada en Azure Portal. No utilice la clave de inicio de LUIS, ya que no servirá para esta integración.

### <a name="new-endpoint"></a>Nuevo punto de conexión 
Esta integración crea un punto de conexión y un modelo de [precios](luis-boundaries.md#key-limits). El punto de conexión, mediante el [SDK de Speech](https://github.com/Azure-Samples/cognitive-services-speech-sdk), puede recibir expresiones de voz y texto que permiten usarlo como un único punto de conexión. 

### <a name="quota-usage"></a>Uso de cuota
Vea los [Límites clave](luis-boundaries.md#key-limits) para obtener información. 

### <a name="data-retention"></a>Retención de datos
Los datos enviados al punto de conexión, mediante el SDK de Speech, con independencia de que se trate de voz o texto, solo se usan para mejorar el modelo de voz. No se usa más allá del modelo para mejorar Speech o LUIS con una capacidad general. Cuando se elimina la aplicación de LUIS, también se eliminan los datos conservados.

<!-- TBD: Machine translation conversion concepts -->

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Use la conversión de voz a texto](luis-tutorial-speech-to-intent.md)

