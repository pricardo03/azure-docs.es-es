---
title: 'Bing Custom Search: definición de las sugerencias de Custom Autosuggest'
description: Describe cómo configurar Custom Autosuggest con sugerencias personalizadas
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: a41b4e5b6c268ec68488c6764d4192cf8d2345a4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380990"
---
# <a name="configure-your-custom-autosuggest-experience"></a>Configuración de experiencia de Custom Autosuggest
Si está suscrito a Custom Search en el nivel adecuado (consulte las [páginas de precios](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), puede personalizar las sugerencias de búsqueda realizadas en su experiencia de Custom Search. Custom Autosuggest devuelve una lista de consultas sugeridas basándose en una cadena de consulta parcial proporcionada por el usuario. Con Custom Autosuggest, proporcione sugerencias de búsqueda personalizada pertinentes para su experiencia de búsqueda. Especifique si desea devolver solo sugerencias personalizadas o también incluir sugerencias de Bing. Si incluye sugerencias de Bing, las sugerencias personalizadas aparecen antes de las sugerencias de Bing. Las sugerencias de Bing están restringidas al contexto de la instancia de Custom Search.

## <a name="configure-custom-autosuggest"></a>Configuración de Custom Autosuggest
Utilice las siguientes instrucciones para configurar Custom Autosuggest para su instancia de Custom Search.

1.  Inicie sesión en [Custom Search](https://customsearch.ai).
2.  Haga clic en una instancia de Custom Search. Para crear una instancia, consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).
3.  Haga clic en la pestaña **Sugerencias automáticas**.

## <a name="enable-bing-suggestions"></a>Habilitación de las sugerencias de Bing
Para habilitar las sugerencias de Bing, ponga el control deslizante **Automatic Bing suggestions** (Sugerencias automáticas de Bing) en la posición de activado. El control deslizante se pone azul.

## <a name="add-suggestions"></a>Adición de sugerencias
Para agregar una sugerencia, escríbala en el cuadro de texto. Presione la tecla Entrar o haga clic en el icono **+**. Las sugerencias personalizadas pueden estar en cualquier idioma y aparecerán antes de las sugerencias de Bing.

## <a name="upload-suggestions"></a>Carga de sugerencias
Puede cargar una lista de sugerencias de un archivo. Coloque cada sugerencia en una línea independiente. Haga clic en el icono de carga y seleccione su archivo.

## <a name="remove-suggestions"></a>Eliminación de sugerencias
Para quitar una sugerencia, haga clic en el icono de eliminación situado junto a la sugerencia que desea quitar.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

  >[!NOTE]  
  >La configuración de Custom Autosuggest puede tardar hasta 24 horas en surtir efecto.

## <a name="next-steps"></a>Pasos siguientes

- [Get custom suggestions](./get-custom-suggestions.md) (Obtención de sugerencias personalizadas)
- [Search your custom instance](./search-your-custom-view.md) (Búsqueda de su instancia personalizada)
- [Configure and consume custom hosted UI](./hosted-ui.md) (Configuración y uso de la IU hospedada personalizada)