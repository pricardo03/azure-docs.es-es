---
title: 'Método de similitud: Academic Knowledge API'
titlesuffix: Azure Cognitive Services
description: Use el método de similitud para calcular la similitud académica de dos cadenas.
services: cognitive-services
author: alch-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 01/18/2017
ms.author: alch
ms.openlocfilehash: 0e42d5e05268d4a572376f2bdf25d0ce6f8f4be7
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55490131"
---
# <a name="similarity-method"></a>Método de similitud

La API REST de **similitud** se usa para calcular la similitud académica entre dos cadenas. 
<br>

**Punto de conexión de REST:**
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?
```

## <a name="request-parameters"></a>Parámetros de solicitud
Parámetro        |Tipo de datos      |Obligatorio | DESCRIPCIÓN
----------|----------|----------|------------
**s1**        |string   |Sí  |Cadena* que se va a comparar
**s2**        |string   |Sí  |Cadena* que se va a comparar
<sub> *Las cadenas que se van a comparar tienen una longitud máxima de 1 MB. </sub>
<br>
## <a name="response"></a>Response
NOMBRE | DESCRIPCIÓN
--------|---------
**SimilarityScore**        |Valor de punto flotante que representa la similitud de coseno de s1 y s2, con valores cercanos a 1.0, lo que significa que son más similares, y valores cercanos a -1,0, lo que significa que son menos similares
<br>

## <a name="successerror-conditions"></a>Condiciones de éxito/error
Estado HTTP | Motivo | Response
-----------|----------|--------
**200**         |Correcto | Número de punto flotante
**400**         | Solicitud incorrecta o solicitud no válida | Mensaje de error      
**500**         |Error interno del servidor | Mensaje de error
**Timed out**     | Se ha agotado el tiempo de espera para la solicitud.  | Mensaje de error
<br>
## <a name="example-calculate-similarity-of-two-partial-abstracts"></a>Ejemplo: Calcular la similitud de dos resúmenes parciales
#### <a name="request"></a>Solicitud:
```
https://westus.api.cognitive.microsoft.com/academic/v1.0/similarity?s1=Using complementary priors, we derive a fast greedy algorithm that can learn deep directed belief networks one layer at a time, provided the top two layers form an undirected associative memory
&s2=Deepneural nets with a large number of parameters are very powerful machine learning systems. However, overfitting is a serious problem in such networks
```
En este ejemplo, se genera la puntuación de similitud entre dos resúmenes parciales mediante la API de **similitud**.
#### <a name="response"></a>Respuesta:
```
0.520
```
#### <a name="remarks"></a>Comentarios:
La puntuación de similitud se determina mediante la evaluación de los conceptos académicos a través de la incrustación de palabras. En este ejemplo, 0,52 significa que los dos resúmenes parciales son en cierto modo similares.
<br>
