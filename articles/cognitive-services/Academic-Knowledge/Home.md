---
title: ¿Qué es Academic Knowledge API?
titlesuffix: Azure Cognitive Services
description: Use Academic Knowledge API para interpretar las consultas de usuario y recuperar información detallada de Academic Graph.
services: cognitive-services
author: darrine
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: overview
ms.date: 10/30/2018
ms.author: darrine
ms.openlocfilehash: b15ed5e2b31ed817d3f6558858e2b7285f98a70f
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57551728"
---
# <a name="academic-knowledge-api"></a>Academic Knowledge API

Le damos la bienvenida a Academic Knowledge API. Con este servicio, podrá interpretar consultas de usuario con fines académicos y recuperar información completa de Microsoft Academic Graph (MAG). La base de conocimiento de MAG es un gráfico de entidades heterogéneas de escala web que consta de las entidades que modelan actividades académicas: campo de estudio, autor, institución, artículo, lugar y evento. 

Los datos de MAG se extraen del índice de web de Bing, así como de una base de conocimiento interna de Bing. Como resultado de la indexación de Bing en curso, esta API contendrá información nueva de la web después de la detección y la indexación por parte de Bing. En función de este conjunto de datos, Academic Knowledge API permite un diálogo interactivo, guiado por el conocimiento, que combina perfectamente la búsqueda reactiva con experiencias de sugerencia proactivas, resultados de búsqueda de grafos de documentos de investigación enriquecidos y distribuciones de histograma de los valores de atributo para un conjunto de artículos y las entidades relacionadas.

Para obtener más información sobre Microsoft Academic Graph, consulte [https://aka.ms/academicgraph](https://aka.ms/academicgraph).

Academic Knowledge API se ha movido de Cognitive Services (versión preliminar) a Laboratorios de Cognitive Services. La nueva página de inicio para el proyecto es: [https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge](https://labs.cognitive.microsoft.com/en-us/project-academic-knowledge). La clave de API existente seguirá funcionando hasta el 24 de mayo de 2018. Tras esta fecha, vuelva a generar una nueva clave de API. Tenga en cuenta esa versión preliminar de pago ya no estará disponible una vez que expire la clave existente. Póngase en contacto con nuestro equipo si el nivel gratuito de la API no es suficiente para sus fines. 

## <a name="features"></a>Características
Academic Knowledge API consta de cuatro puntos de conexión de REST relacionados:  
  1. **interpret**: interpreta una cadena de consulta de usuario en lenguaje natural. Devuelve interpretaciones anotadas para habilitar una funcionalidad de autocompletar de cuadro de búsqueda que se anticipe a lo que el usuario escribe.  
  2. **evaluate**: evalúa una expresión de consulta y devuelve resultados de entidades de Academic Knowledge.  
  3. **calchistogram**: calcula un histograma de la distribución de valores de atributo para las entidades académicas que devuelve una expresión de consulta, como la distribución de citas por año de un autor determinado.  
  
Usados en conjunto, estos métodos de API permiten crear una experiencia de búsqueda semántica enriquecida. Dada una cadena de consulta de usuario, el método **interpret** le proporciona una versión anotada de la consulta y una expresión de consulta estructurada, a la vez que tiene la opción de completar la consulta del usuario basándose en la semántica de los datos académicos subyacentes. Por ejemplo, si el usuario escribe la cadena *latent s*, el método **interpret** podrá proporcionar un conjunto de interpretaciones clasificadas, lo que sugiere que el usuario puede estar buscando el campo de estudio *latent semantic análisis*, el artículo *latent structure analysis* u otras expresiones de entidad que comienzan con *latent s*. Esta información puede utilizarse para guiar rápidamente al usuario a los resultados de búsqueda que desee.

El método **evaluate** se puede utilizar para recuperar un conjunto de entidades de artículo coincidentes de la base de conocimiento académica, y el método **calchistogram** se puede utilizar para calcular la distribución de valores de atributo para un conjunto de entidades de artículo que puede usarse para filtrar aún más los resultados de la búsqueda.        

## <a name="getting-started"></a>Introducción 
Consulte los temas secundarios a la izquierda para obtener documentación detallada.  Tenga en cuenta que, para mejorar la legibilidad de los ejemplos, las llamadas de la API de REST contienen caracteres (como espacios) que no tienen codificación URL.  El código deberá aplicar las codificaciones URL adecuadas.
