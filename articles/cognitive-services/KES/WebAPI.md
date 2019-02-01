---
title: Interfaz de API web - Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Use la interfaz de API web para crear una experiencia de búsqueda completa y semántica en Knowledge Exploration Service (KES) API.
services: cognitive-services
author: bojunehsu
manager: cgronlun
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: 131d8d58982b5430063aa3dc7b3d1982cd430148
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55211311"
---
# <a name="web-api-interface"></a>Interfaz de API web

Los archivos de modelo generados por Knowledge Exploration Service se pueden hospedar mediante un conjunto de API web y también se puede acceder a ellos mediante dichas API.  Las API pueden estar hospedadas en la máquina local mediante el comando [`host_service`](CommandLine.md#host_service-command), o se pueden implementar en un servicio en la nube de Azure con el comando [`deploy_service`](CommandLine.md#deploy_service-command).  Ambas técnicas exponen los siguientes puntos de conexión de API:

* [*interpret*](interpretMethod.md): interpreta una cadena de consulta en lenguaje natural. Devuelve interpretaciones anotadas para permitir completas experiencias de finalización automática en el cuadro de búsqueda que se anticipen a lo que el usuario escribe.
* [*evaluate*](evaluateMethod.md): evalúa y devuelve la salida de una expresión de consulta estructurada.
* [*calchistogram*](calchistogramMethod.md): calcula un histograma de valores de atributos para objetos devueltos por una expresión de consulta estructurada.

Cuando se usan juntos, estos métodos de API permiten la creación de una experiencia de búsqueda semántica completa.  Dada una cadena de consulta en lenguaje natural, el método *interpret* proporciona versiones anotadas de la consulta de entrada con expresiones de consulta estructuradas que se basan en la gramática y los datos de índice subyacentes.  El método *evaluate* evalúa la expresión de consulta estructurada y devuelve los objetos de índice coincidentes para mostrar.  El método *calchistogram* calcula las distribuciones de valores de atributo para permitir filtrado y refinamiento.

**Ejemplo**

En un dominio de publicaciones académicas, si un usuario escribe la cadena "latent s", el método *interpret* puede proporcionar un conjunto de interpretaciones clasificadas, y sugerir que el usuario podría estar buscando la palabra clave "latent semantic analysis", el título "latent structure analysis" u otra expresión que empieza por "latent s".  Esta información puede utilizarse para guiar rápidamente al usuario a los resultados de búsqueda deseados.

En este dominio, el método *evaluate* puede usarse para recuperar un conjunto de publicaciones coincidentes del índice académico, y el método *calchistogram* se puede usar para calcular la distribución de valores de atributo de las publicaciones coincidentes, que se puede usar para filtrar y refinar aún más los resultados de búsqueda.

Tenga en cuenta que, para mejorar la legibilidad de los ejemplos, las llamadas API REST contienen caracteres (como espacios) que no tienen codificación URL. El código deberá aplicar las codificaciones URL adecuadas.
