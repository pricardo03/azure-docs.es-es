---
title: 'Bing Custom Search: uso de marcadores de decoración para resaltar texto | Microsoft Docs'
description: Se muestra cómo habilitar las decoraciones de texto en las respuestas de búsqueda.
services: cognitive-services
author: brapel
manager: ehansen
ms.assetid: 5365B568-EA55-4D97-8FBE-0AF60158D4D5
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: d2d0070865aa29257ac827bbb4fc313d87ea7282
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380162"
---
# <a name="using-decoration-markers-to-highlight-text"></a>Uso de marcadores de decoración para resaltar texto

Bing admite el resaltado de aciertos, que marca los términos de la consulta (u otros términos que Bing considere relevantes) en las cadenas de presentación de algunas de las respuestas. Por ejemplo, los campos `name`, `displayUrl` y `snippet` de una página web pueden marcar los términos de la consulta.

De forma predeterminada, Bing no incluye marcadores de resaltado en las cadenas de presentación. Para incluir los marcadores, incluya el parámetro `textDecorations` en la solicitud de consulta y establézcalo en **true**. Bing marca los términos de la consulta con los caracteres Unicode E000 y E001 para designar el principio y final del término. Por ejemplo, si el término de consulta es Sailing Dinghy y cualquiera de los términos existe en el campo, el término se incluye entre caracteres de resaltado de aciertos, tal y como se muestra en el ejemplo siguiente:  
  
![Resaltado de aciertos](./media/bing-hit-highlighting.PNG) 

Antes de mostrar la cadena en la interfaz de usuario, podría reemplazar los caracteres Unicode por caracteres que sean adecuados para el formato de presentación. Por ejemplo, si se muestra el texto como HTML, puede resaltar el término de la consulta reemplazando E000 por < b\> y E001 por < /b\>. Si no desea aplicar formato, quite los marcadores de la cadena. 

Bing proporciona la opción de usar caracteres Unicode o etiquetas HTML como marcadores. Para especificar los marcadores que se van a usar, incluya el parámetro de consulta `textFormat`. Para marcar el contenido con caracteres Unicode, establezca `textFormat` en Raw (valor predeterminado) y para marcar el contenido con etiquetas HTML, establezca `textFormat` en HTML. 
  
Si `textDecorations` es **true**, Bing puede incluir los siguientes marcadores en las cadenas de presentación de las respuestas. Si no hay ningún equivalente en código HTML, la celda de la tabla HTML aparece vacía.

|Unicode|HTML|DESCRIPCIÓN
|-|-|-
|U+E000|\<b&gt;|Marca el principio del término de la consulta (resaltado de aciertos)
|U+E001|\</b&gt;|Marca el final del término de la consulta
|U+E002|\<i&gt;|Marca el principio del contenido en cursiva 
|U+E003|\</i&gt;|Marca el final del contenido en cursiva
|U+E004|\<br/&gt;|Marca un salto de línea
|U+E005||Marca el principio de un número de teléfono
|U+E006||Marca el final de un número de teléfono
|U+E007||Marca el principio de una dirección
|U+E008||Marca el final de una dirección
|U+E009|\&nbsp;|Marca un espacio de no separación
|U+E00C|\<strong&gt;|Marca el principio del contenido en negrita
|U+E00D|\</strong&gt;|Marca el final del contenido en negrita
|U+E00E||Marca el principio del contenido cuyo fondo debe ser más claro que el fondo circundante
|U+E00F||Marca el final del contenido cuyo fondo debe ser más claro que el fondo circundante
|U+E010||Marca el principio del contenido cuyo fondo debe ser más oscuro que el fondo circundante
|U+E011||Marca el final del contenido cuyo fondo debe ser más oscuro que el fondo circundante
|U+E012|\<del&gt;|Marca el principio del contenido que debe tacharse
|U+E013|\</del&gt;|Marca el final del contenido que debe tacharse
|U+E016|\<sub&gt;|Marca el principio del contenido de subíndice
|U+E017|\</sub&gt;|Marca el final del contenido de subíndice
|U+E018|\<sup&gt;|Marca el principio del contenido de superíndice
|U+E019|\</sup&gt;|Marca el final del contenido de superíndice

El ejemplo siguiente muestra una respuesta `Computation` que contiene marcadores de subíndice del término de consulta log(2). El campo `expression` contiene los marcadores solo si `textDecoration` es **true**.

![marcadores de cálculo](./media/bing-markers-computation.PNG) 

Si la solicitud no incluía decoraciones, la expresión sería log10(2). 
  
