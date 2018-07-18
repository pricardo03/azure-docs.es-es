---
title: Inicio rápido de Python para Bing Visual Search API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Aquí le mostramos cómo cargar una imagen a Bing Visual Search API y obtener información detallada sobre la misma.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: a520466825eb429e45e0500b52bd7af502c0a38c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382474"
---
# <a name="your-first-bing-visual-search-query-in-python"></a>Su primera consulta de Bing Visual Search en Python

Bing Visual Search API devuelve información sobre una imagen proporcionada. Puede proporcionar la imagen utilizando la dirección URL de la misma, un token de conclusiones, o simplemente cargando una imagen. Para obtener más información acerca de estas opciones, consulte [What is Bing Visual Search API?](../overview.md) (¿Qué es Bing Visual Search API?). En este artículo se muestra la forma de cargar una imagen. Cargar una imagen puede serle de utilidad en aquellos momentos en los que use un dispositivo móvil para, por ejemplo, hacer una foto de un lugar interesante y así poder obtener información acerca de él. Asimismo, estos detalles pueden incluir curiosidades sobre ese lugar de interés. 

Si carga una imagen local, a continuación verá los datos del formulario que debe incluir en el cuerpo del elemento POST. Los datos del formulario deben incluir el encabezado Content-Disposition. Asimismo, el parámetro `name` se debe establecer en "imagen" y el parámetro `filename` se puede establecer en cualquier cadena. El contenido del formulario es el binario de la imagen. Recuerde que el tamaño de imagen máximo que puede cargar es de 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

En este artículo se incluye una aplicación simple de consola que envía una solicitud de Bing Visual Search API y muestra los resultados de búsqueda de JSON. Si bien esta aplicación está escrita en Python, la API es un servicio web RESTful compatible con cualquier lenguaje de programación que pueda realizar solicitudes HTTP y analizar JSON. 

## <a name="prerequisites"></a>requisitos previos

Necesita [Python 3](https://www.python.org/) para ejecutar este código.

Para realizar este tutorial de inicio rápido, puede usar una clave de suscripción de [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) o una clave de suscripción de pago.

## <a name="running-the-walkthrough"></a>Ejecutar el tutorial

Para ejecutar esta aplicación, siga estos pasos:

1. Cree un nuevo proyecto de Python en su IDE o editor favorito.
2. Cree un archivo denominado visualsearch.py y agregue el código que se muestra en esta guía de inicio rápido.
3. Reemplace el valor `SUBSCRIPTION_KEY` con la clave de suscripción.
3. Reemplace el valor `imagePath` con la ruta de acceso de la imagen que va a cargar.
4. Ejecute el programa.



A continuación se muestra cómo enviar un mensaje mediante los datos de formularios con varias partes en Python.

```python
"""Bing Visual Search upload image example"""

# Download and install Python at https://www.python.org/
# Run the following in a command console window
# pip3 install requests

import requests, json


BASE_URI = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch'

SUBSCRIPTION_KEY = '<yoursubscriptionkeygoeshere>'

HEADERS = {'Ocp-Apim-Subscription-Key': SUBSCRIPTION_KEY}

imagePath = '<pathtoyourimagetouploadgoeshere>'

file = {'image' : ('myfile', open(imagePath, 'rb'))}

def main():
    
    try:
        response = requests.post(BASE_URI, headers=HEADERS, files=file)
        response.raise_for_status()
        print_json(response.json())

    except Exception as ex:
        raise ex


def print_json(obj):
    """Print the object as json"""
    print(json.dumps(obj, sort_keys=True, indent=2, separators=(',', ': ')))



# Main execution
if __name__ == '__main__':
    main()
```


## <a name="next-steps"></a>Pasos siguientes

[Obtenga información acerca de una imagen mediante un token de conclusión](../use-insights-token.md)  
[Tutorial de aplicación de una sola página de Bing Visual Search](../tutorial-bing-visual-search-single-page-app.md)  
[Introducción a Bing Visual Search API](../overview.md)  
[Pruébelo](https://aka.ms/bingvisualsearchtryforfree)  
[Obtenga una clave de acceso para evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Referencia a Bing Visual Search API](https://aka.ms/bingvisualsearchreferencedoc)
