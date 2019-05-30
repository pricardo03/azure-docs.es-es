---
title: Configuración de la aplicación
titleSuffix: Azure Cognitive Services
description: Comprender la configuración de aplicaciones para las aplicaciones de comprensión de lenguaje.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/29/2019
ms.author: diberry
ms.openlocfilehash: c19d328c3a5f4dd6dbe14ca94809e42c5655ea72
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391149"
---
# <a name="application-settings"></a>Configuración de la aplicación

Esta configuración de aplicación se almacena en el [exportado](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40) app y [actualizado](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/versions-update-application-version-settings) con las API de REST. Cambio de la configuración de la versión de aplicación restablece su estado de entrenamiento de la aplicación no entrenado.

|Configuración|Valor predeterminado|Notas|
|--|--|--|
|NormalizePunctuation|True |Quita los signos de puntuación.|
|NormalizeDiacritics|True |Quita los signos diacríticos.|

## <a name="diacritics-normalization"></a>Normalización de los signos diacríticos 

Activar la normalización de declaración para los signos diacríticos al archivo de aplicación de LUIS JSON en el `settings` parámetro.

```JSON
"settings": [
    {"name": "NormalizeDiacritics", "value": "true"}
] 
```

Las declaraciones siguientes muestran cómo afecta la normalización de los signos diacríticos a grabaciones de voz:

|Con los signos diacríticos establecidos en false|Con los signos diacríticos establecido en true|
|--|--|
|`quiero tomar una piña colada`|`quiero tomar una pina colada`|
|||

### <a name="language-support-for-diacritics"></a>Compatibilidad con idiomas de los signos diacríticos

#### <a name="brazilian-portuguese-pt-br-diacritics"></a>Portugués (Brasil) `pt-br` los signos diacríticos

|Los signos diacríticos establecida en false|Los signos diacríticos establecida en false|
|-|-|
|`á`|`a`|
|`â`|`a`|
|`ã`|`a`|
|`à`|`a`|
|`ç`|`c`|
|`é`|`e`|
|`ê`|`e`|
|`í`|`i`|
|`ó`|`o`|
|`ô`|`o`|
|`õ`|`o`|
|`ú`|`u`| 
|||

#### <a name="dutch-nl-nl-diacritics"></a>Neerlandés `nl-nl` los signos diacríticos

|Los signos diacríticos establecida en false|Los signos diacríticos establecida en false|
|-|-|
|`á`|`a`|
|`à`|`a`|
|`é`|`e`|
|`ë`|`e`|
|`è`|`e`|
|`ï`|`i`|
|`í`|`i`|
|`ó`|`o`|
|`ö`|`o`|
|`ú`|`u`| 
|`ü`|`u`|
|||

#### <a name="french-fr--diacritics"></a>Francés `fr-` los signos diacríticos

Esto incluye referencias culturales secundarias tanto francés canadienses.

|Los signos diacríticos establecida en false|Los signos diacríticos establecida en false|
|--|--|
|`é`|`e`|
|`à`|`a`|
|`è`|`e`|
|`ù`|`u`|
|`â`|`a`| 
|`ê`|`e`| 
|`î`|`i`| 
|`ô`|`o`| 
|`û`|`u`| 
|`ç`|`c`| 
|`ë`|`e`| 
|`ï`|`i`| 
|`ü`|`u`| 
|`ÿ`|`y`| 

#### <a name="german-de-de-diacritics"></a>Alemán `de-de` los signos diacríticos

|Los signos diacríticos establecida en false|Los signos diacríticos establecida en false|
|--|--|
|`ä`|`a`|
|`ö `|`o`| 
|`ü `|`u`| 

#### <a name="italian-it-it-diacritics"></a>Italiano `it-it` los signos diacríticos

|Los signos diacríticos establecida en false|Los signos diacríticos establecida en false|
|--|--|
|`à`|`a`|
|`è`|`e`|
|`é`|`e`|
|`ì`|`i`| 
|`í`|`i`| 
|`î`|`i`| 
|`ò`|`o`| 
|`ó `|`o`| 
|`ù`|`u`|
|`ú`|`u`|

#### <a name="spanish-es--diacritics"></a>Español `es-` los signos diacríticos

Esto incluye a mexicanos español y canadiense.

|Los signos diacríticos establecida en false|Los signos diacríticos establecida en false|
|-|-|
|`á`|`a`|
|`é`|`e`|
|`í`|`i`| 
|`ó `|`o`| 
|`ú`|`u`|
|`ü`|`u`|
|`ñ`|`u`|


## <a name="punctuation-normalization"></a>Normalización de puntuación

Activar la normalización utterance signos de puntuación al archivo de aplicación de LUIS JSON en el `settings` parámetro.

```JSON
"settings": [
    {"name": "NormalizePunctuation", "value": "true"}
] 
```

Las declaraciones siguientes muestran cómo los signos diacríticos afecta a las grabaciones de voz:

|Con los signos diacríticos establecidos en False|Con los signos diacríticos establecidos en True|
|--|--|
|`Hmm..... I will take the cappuccino`|`Hmm I will take the cappuccino`|
|||

### <a name="punctuation-removed"></a>Puntuación

Se quita el signo de puntuación con `NormalizePunctuation` se establece en true.

|Signos de puntuación|
|--|
|`-`| 
|`.`| 
|`'`|
|`"`|
|`\`|
|`/`|
|`?`|
|`!`|
|`_`|
|`,`|
|`;`|
|`:`|
|`(`|
|`)`|
|`[`|
|`]`|
|`{`|
|`}`|
|`+`|
|`¡`|
