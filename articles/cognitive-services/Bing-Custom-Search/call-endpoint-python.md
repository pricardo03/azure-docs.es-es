---
title: 'Guía de inicio rápido: Llamada al punto de conexión mediante Python en Bing Custom Search'
titlesuffix: Azure Cognitive Services
description: Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando Python para llamar al punto de conexión de Bing Custom Search.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: aahi
ms.openlocfilehash: d7977f49a484665f133a119f491bd8986c0b6fc4
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52310726"
---
# <a name="quickstart-call-bing-custom-search-endpoint-python"></a>Guía de inicio rápido: Llamada a un punto de conexión de Bing Custom Search (Python)

Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando Python para llamar al punto de conexión de Bing Custom Search. 

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido necesita instalar:

- Una instancia de Custom Search lista para usar. Consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).
- [Python](https://www.python.org/) instalado.
- Una clave de suscripción. Puede obtener una clave de suscripción cuando active su [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) o puede utilizar una clave de suscripción de pago desde el panel de Azure (consulte [Cuenta de la API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).   Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).


## <a name="run-the-code"></a>Ejecución del código

Para ejecutar el ejemplo, siga estos pasos:

1. Cree una carpeta para el código.  
  
2. Desde un terminal o símbolo del sistema de administrador, vaya a la carpeta que acaba de crear.  
  
3. Instale el módulo de Python **requests**:  
  
    <pre>
    pip install pipenv
    pipenv install requests
    </pre>  
      
4. Cree un archivo denominado BingCustomSearch.py en la carpeta que creó y copie el código siguiente en él. Reemplace los valores de **YOUR-SUBSCRIPTION-KEY** y **YOUR-CUSTOM-CONFIG-ID** por su clave y su identificador de configuración.  
  
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
  
7. Ejecute el código con el comando siguiente.  
  
    ```
    python BingCustomSearch.py
    ```

## <a name="next-steps"></a>Pasos siguientes
- [Configuración de la experiencia de interfaz de usuario hospedada](./hosted-ui.md)
- [Uso de marcadores de decoración para resaltar texto](./hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)
