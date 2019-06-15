---
title: Formato de datos de Knowledge Exploration Service API
titlesuffix: Azure Cognitive Services
description: Más información sobre el formato de datos en Knowledge Exploration Service (KES) API.
services: cognitive-services
author: bojunehsu
manager: nitinme
ms.service: cognitive-services
ms.subservice: knowledge-exploration
ms.topic: conceptual
ms.date: 03/26/2016
ms.author: paulhsu
ms.openlocfilehash: bba257c61509d862bb3161625750f05a86af8770
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60815091"
---
# <a name="data-format"></a>Formato de datos

El archivo de datos describe la lista de objetos que se van a indexar.
Cada línea del archivo especifica los valores de atributo de un objeto en [formato JSON](https://json.org/) con codificación UTF-8.
Además de los atributos definidos en el [esquema](SchemaFormat.md), cada objeto tiene un atributo "logprob" opcional que especifica la probabilidad logarítmica relativa entre los objetos.
Cuando el servicio devuelve objetos en orden de probabilidad decreciente, podemos usar "logprob" para indicar el orden de devolución de los objetos coincidentes.
Dada una probabilidad *p* entre 0 y 1, la probabilidad logarítmica correspondiente se puede calcular como log(*p*), donde log() es la función logarítmica natural.
Cuando no se especifica ningún valor para logprob, se usa el valor predeterminado 0.

```json
{"logprob":-5.3, "Title":"latent dirichlet allocation", "Year":2003, "Author":{"Name":"david m blei", "Affiliation":"uc berkeley"}, "Author":{"Name":"andrew y ng", "Affiliation":"stanford"}, "Author":{"Name":"michael i jordan", "Affiliation":"uc berkeley"}}
{"logprob":-6.1, "Title":"probabilistic latent semantic indexing", "Year":1999, "Author":{"Name":"thomas hofmann", "Affiliation":"uc berkeley"}}
...
```

Para los atributos String, GUID y Blob, el valor se representa como una cadena JSON entrecomillada.  Para los atributos numéricos (Int32, Int64, Double), el valor se representa como un número JSON.  Para los atributos compuestos, el valor es un objeto JSON que especifica los valores de atributo secundario.  Para compilaciones de índice más rápidas, ordene previamente los objetos por probabilidad logarítmica decreciente.

En general, un atributo puede tener 0 o más valores.  Si un atributo no tiene ningún valor, se descarta del archivo JSON.  Si un atributo tiene 2 o más valores, se puede repetir el par valor-atributo en el objeto JSON.  Como alternativa, se puede asignar al atributo una matriz JSON que contiene varios valores.

```json
{"logprob":0, "Title":"0 keyword"}
{"logprob":0, "Title":"1 keyword", "Keyword":"foo"}
{"logprob":0, "Title":"2 keywords", "Keyword":"foo", "Keyword":"bar"}
{"logprob":0, "Title":"2 keywords (alt)", "Keyword":["foo", "bar"]}
```
