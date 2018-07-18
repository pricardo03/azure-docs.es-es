---
title: Llamada a un punto de conexión con Python - Bing Custom Search - Microsoft Cognitive Services
description: Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando Python para llamar al punto de conexión de Bing Custom Search.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 889762ae0b401438f25546738268c584ddd58389
ms.sourcegitcommit: 15bfce02b334b67aedd634fa864efb4849fc5ee2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "35382958"
---
# <a name="call-bing-custom-search-endpoint-python"></a>Llamada a un punto de conexión de Bing Custom Search (Python)

Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando Python para llamar al punto de conexión de Bing Custom Search. 

## <a name="prerequisites"></a>Requisitos previos
Para completar este inicio rápido necesita instalar:

- Una instancia de Custom Search. Consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).

-  [Python](https://www.python.org/) instalado.

- Una [cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con **Bing Search APIs**. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) es suficiente para esta guía de inicio rápido. Necesita la clave de acceso que se le proporciona al activar la versión de evaluación gratuita, o puede usar una clave de suscripción de pago desde su panel de Azure. 

## <a name="run-the-code"></a>Ejecución del código

Para llamar al punto de conexión Bing Custom Search, siga estos pasos:

1. Cree una carpeta para el código.
2. Desde un terminal o símbolo del sistema de administrador, vaya a la carpeta que acaba de crear.
3. Instale el módulo de Python **requests**:
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>
7. Cree el archivo BingCustomSearch.py y copie en él el código siguiente.
8. Reemplace los valores de **YOUR-SUBSCRIPTION-KEY** y **YOUR-CUSTOM-CONFIG-ID** por su clave y su identificador de configuración (consulte el paso 1).

    ``` Python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=' + searchTerm + '&customconfig=' + customConfigId
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```
9. Ejecute el código con el comando siguiente.
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Pasos siguientes
- [Configuración de la experiencia de interfaz de usuario hospedada](./hosted-ui.md)
- [Uso de marcadores de decoración para resaltar texto](./hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)