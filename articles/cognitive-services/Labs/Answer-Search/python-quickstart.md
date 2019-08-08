---
title: 'Inicio rápido: Project Answer Search, Python'
titlesuffix: Azure Cognitive Services
description: Ejemplo de Python para empezar a usar Project Answer Search.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: c35bf9649a0a22f3488c45d1f4f8729e211e0ddb
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707082"
---
# <a name="quickstart-project-answer-search-with-python"></a>Guía de inicio rápido de Project Answer Search con Python

El siguiente ejemplo de Python crea y envía una solicitud de información sobre el "Peñón de Gibraltar".

## <a name="prerequisites"></a>Requisitos previos

Obtenga una clave de acceso para la evaluación gratuita de los [Laboratorios de Cognitive Services](https://labs.cognitive.microsoft.com/en-us/project-answer-search).

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

Este es el código completo de esta demostración:

```
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

```
## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](c-sharp-quickstart.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de Node](node-quickstart.md)
