---
title: 'Bing Custom Search: búsqueda de una vista personalizada | Microsoft Docs'
description: Describe cómo buscar una vista personalizada de la web
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 75f6c8d299c7eed901dda0631fca74b040f72e30
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380991"
---
# <a name="call-your-custom-search"></a>Llamada a la búsqueda personalizada
Antes de realizar la primera llamada a Custom Search API para obtener resultados de búsqueda para la instancia, debe obtener una clave de suscripción de Cognitive Services. Para obtener una clave de Custom Search API, consulte [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).

> [!NOTE]
> Los usuarios de Bing Custom Search existentes que tengan una clave de versión preliminar aprovisionada el 15 de octubre de 2017 (o antes) podrán usar sus claves hasta el 30 de noviembre de 2017, o hasta que agoten el número máximo de consultas permitidas. Después de esta fecha tendrán que migrar a la versión de disponibilidad general en Azure.

## <a name="try-it-out"></a>Prueba
Después de configurar su experiencia de búsqueda personalizada, puede probar la configuración desde el portal de Custom Search. Inicie sesión en [Custom Search](https://customsearch.ai), haga clic en una instancia de Custom Search y haga clic en la pestaña **Producción**. Aparece la pestaña **Puntos de conexión**. Su suscripción determinará qué puntos de conexión están disponibles para probar, consulte las [páginas de precios](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/). Para probar un punto de conexión, selecciónelo en la lista desplegable y establezca las opciones de configuración asociadas. 

A continuación se indican las opciones disponibles.

- **Consulta**: el término de búsqueda para buscar. Solo está disponible para los puntos de conexión de web, imagen y sugerencias automáticas.
- **Custom Configuration ID** (Id. de configuración personalizada): el identificador de configuración de la instancia de Custom Search seleccionada. Este campo es de solo lectura.
- **Mercado**: mercado de donde proceden los resultados. Solo está disponible para los puntos de conexión de web, imagen y IU hospedada.
- **Clave de suscripción**: la clave de la suscripción con la que probar. Puede seleccionar una clave en la lista desplegable o escribir una manualmente.
- **Búsqueda segura**: un filtro usado para filtrar las páginas web de contenido para adultos. Solo está disponible para los puntos de conexión de web, imagen y IU hospedada.
- **Recuento**: número de resultados de la búsqueda que se devolverán en la respuesta. Solo está disponible para los puntos de conexión de web e imagen.
- **Offset** (Compensación): número de resultados de la búsqueda que se devolverán en la respuesta. Solo está disponible para los puntos de conexión de web e imagen.

Después de especificar todas las opciones necesarias para web, imagen o sugerencias automáticas, haga clic en **Llamar** para ver la respuesta de JSON en el panel derecho. 

Si selecciona el punto de conexión de la IU hospedada, puede probar la experiencia de búsqueda en el panel derecho.

## <a name="next-steps"></a>Pasos siguientes
- [Call your custom view with C#](./call-endpoint-csharp.md) (Llamada a la vista personalizada con C#)
- [Call your custom view with Java](./call-endpoint-java.md) (Llamada a la vista personalizada con Java)
- [Call your custom view with NodeJs](./call-endpoint-nodejs.md) (Llamada a la vista personalizada con NodeJs)
- [Call your custom view with Python](./call-endpoint-python.md) (Llamada a la vista personalizada con Python)