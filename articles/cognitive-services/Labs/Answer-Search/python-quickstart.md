---
title: Inicio rápido de Python para Project Answer Search de Microsoft Cognitive Services | Microsoft Docs
description: Ejemplo de Python para empezar a usar Project Answer Search de Microsoft Cognitive Services en Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 9cb5406c616ed8e96d73c00c788a0d20f66dcabd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381711"
---
# <a name="project-answer-search-python-quickstart"></a>Guía de inicio rápido de Project Answer Search para Python

El siguiente ejemplo de Python crea y envía una solicitud de información sobre el "Peñón de Gibraltar".

## <a name="prerequisites"></a>requisitos previos

Obtenga una clave de acceso para la evaluación gratuita de los [Laboratorios de Cognitive Services](https://aka.ms/answersearchsubscription).

Este ejemplo utiliza Python 3.6.4.

## <a name="code-scenario"></a>Escenario de código 

El código siguiente crea una vista previa de la dirección URL.
Se implementa en los pasos siguientes:
1. Declare las variables para especificar el punto de conexión por host y ruta de acceso.
2. Especifique la dirección URL de la que desea obtener una vista previa y agregue el parámetro de consulta.  
3. Defina el parámetro de consulta.
4. Defina la función de búsqueda que crea la solicitud y agrega el encabezado *Ocp-Apim-Subscription-Key*.
5. Defina el encabezado *Ocp-Apim-Subscription-Key*. 
6. Realice la conexión y envíe la solicitud.
7. Imprima los resultados JSON.

El código completo de esta demostración es el siguiente:

````
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'https://api.labs.cognitive.microsoft.com'
path = '/answerSearch/v7.0/search '

query = 'Rock of Gibraltar'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

````
## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](c-sharp-quickstart.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de Node](node-quickstart.md)