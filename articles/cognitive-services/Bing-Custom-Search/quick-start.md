---
title: 'Guía de inicio rápido: Creación de una primera instancia de Bing Custom Search'
titlesuffix: Azure Cognitive Services
description: Para usar Bing Custom Search, debe crear una instancia de búsqueda personalizada que defina la vista o el segmento de la web. La instancia contiene valores que especifican los dominios públicos, subsitios y páginas web que desee que busque Bing, así como los ajustes en la clasificación.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 866d32aa4de45076fcbc4e413d8c2e67d5346878
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816249"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Guía de inicio rápido: Creación de la primera instancia de Bing Custom Search
Para usar Bing Custom Search, debe crear una instancia de búsqueda personalizada que defina la vista o el segmento de la web. La instancia contiene valores que especifican los dominios públicos, sitios web y páginas web que desee que busque Bing, así como los ajustes en la clasificación. Para crear la instancia, utilice el [portal](https://customsearch.ai) de Bing Custom Search. 

## <a name="create-a-custom-search-instance"></a>Creación de una instancia de búsqueda personalizada

Para crear una instancia de Bing Custom Search:

1.  Obtenga una clave para Custom Search API. Consulte [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Haga clic en el botón **Iniciar sesión** e inicie sesión en el portal mediante una cuenta de Microsoft (MSA). 
    - Si no tiene una MSA, haga clic en **Creación de una cuenta Microsoft**. El portal solicita permisos para tener acceso a los datos. Haga clic en **Sí**.
    - Acepte los términos de Cognitive Services. Active **Acepto** y haga clic en **Acepto**.  
3.  Después de iniciar sesión, haga clic en **Nueva instancia** y ponga un nombre a la instancia. Utilice un nombre que sea significativo y describa el tipo de contenido que devuelve la búsqueda. Puede cambiar el nombre en cualquier momento. 
4.  En la pestaña **Activo** de **Experiencia de búsqueda**, escriba la dirección URL de uno o varios sitios web que quiera incluir en la búsqueda.
5.  Para confirmar que la instancia devuelve resultados, escriba una consulta en el panel de vista previa de la derecha. Si no hay ningún resultado, especifique un nuevo sitio web. Bing devuelve resultados solo de sitios web públicos que se han indexado.
6.  Haga clic en **Publicar** para publicar los cambios de configuración en producción. Cuando se le solicite, haga clic en **Publicar** para confirmar.
7.  Haga clic en **Producción** > **Punto de conexión** y copie el valor de **Custom Configuration ID** (Id. de configuración personalizada). Necesita este identificador para llamar a Custom Search API.

## <a name="next-steps"></a>Pasos siguientes

Siga trabajando con la instancia de búsqueda personalizada que acaba de crear siguiendo las instrucciones de estas guías de procedimientos:

- [Configure your custom search experience](./define-your-custom-view.md) (Configuración de la experiencia de búsqueda personalizada)
- [Call your custom search](./search-your-custom-view.md) (Llamada a la búsqueda personalizada)
- [Share your custom search](./share-your-custom-search.md) (Uso compartido de la búsqueda personalizada)
- [Configuración de la experiencia de interfaz de usuario hospedada](./hosted-ui.md)
- [Uso de marcadores de decoración para resaltar texto](./hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)
