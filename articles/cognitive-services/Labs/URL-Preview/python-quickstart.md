---
title: 'Inicio rápido de Python para Project URL Preview: Microsoft Cognitive Services | Microsoft Docs'
description: Ejemplo de script para empezar a usar rápidamente Project URL Preview de Microsoft Cognitive Services en Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 78b2d83b02aa9ea32509029c7456e04e420b8572
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381767"
---
# <a name="url-preview-python-quickstart"></a>Inicio rápido de Python para URL Preview

En el ejemplo de Python siguiente se crea una vista previa de la dirección URL del sitio web de SwiftKey: https://swiftkey.com/en.

## <a name="prerequisites"></a>requisitos previos

Obtenga una clave de acceso para la evaluación gratuita de los [Laboratorios de Cognitive Services](https://aka.ms/answersearchsubscription).

Este ejemplo utiliza Python 3.6.

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
subscriptionKey = 'your-subscription-key'

host = 'api.labs.cognitive.microsoft.com'
path = '/urlpreview/v7.0/search'

query = 'https://SwiftKey.com'

params = '?q=' + urllib.parse.quote (query)

def get_preview ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_preview ()
print (json.dumps(json.loads(result), indent=4))
````
## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](csharp.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de JavaScript](javascript.md)
- [Inicio rápido de Node URL](node-quickstart.md)