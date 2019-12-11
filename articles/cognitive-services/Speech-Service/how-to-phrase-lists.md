---
title: 'Listas de frases: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Aprenda cómo proporcionar al servicio de voz una lista de frases mediante el objeto `PhraseListGrammar` para mejorar los resultados de reconocimiento de voz a texto.
services: cognitive-services
author: rhurey
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: rhurey
ms.openlocfilehash: 052e02ef562da0637b6b5b9683120f0c397dbfd5
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74805882"
---
# <a name="phrase-lists-for-speech-to-text"></a>Listas de frases para reconocimiento de voz a texto

Al proporcionar al servicio de voz una lista de frases, puede mejorar la precisión del reconocimiento de voz. Las listas de frases se usan para identificar frases conocidas en datos de audio, como el nombre de una persona o una ubicación específica.

Por ejemplo, si tiene el comando "Mover a" y "Cerca" como posible destino que se puede decir, puede añadir la entrada "Mover a Cerca". Al agregar una frase, aumentará la probabilidad de que, cuando se reconozca el audio, se reconozca "Mover a Cerca" en lugar de "Mover acerca".

A una lista de frases se pueden agregar palabras solas o frases completas. Durante el reconocimiento, se usa una entrada de una lista de frases si el audio incluye una coincidencia exacta. Siguiendo con el ejemplo anterior, si la lista de frases incluye "Mover a Cerca" y el audio capturado suena muy parecido a "Mover acerca" y "Mover a Cerca", el resultado del reconocimiento se reconocerá con mayor probabilidad como "Mover a Cerca lentamente".

>[!Note]
> Actualmente, las listas de frases solo admiten el inglés para la conversión de voz en texto.

## <a name="how-to-use-phrase-lists"></a>Cómo usar las listas de frases

Los ejemplos siguientes muestran cómo crear una lista de frases mediante el objeto `PhraseListGrammar`.

```C++
auto phraselist = PhraseListGrammar::FromRecognizer(recognizer);
phraselist->AddPhrase("Move to Ward");
phraselist->AddPhrase("Move to Bill");
phraselist->AddPhrase("Move to Ted");
```

```cs
PhraseListGrammar phraseList = PhraseListGrammar.FromRecognizer(recognizer);
phraseList.AddPhrase("Move to Ward");
phraseList.AddPhrase("Move to Bill");
phraseList.AddPhrase("Move to Ted");
```

```Python
phrase_list_grammar = speechsdk.PhraseListGrammar.from_recognizer(reco)
phrase_list_grammar.addPhrase("Move to Ward")
phrase_list_grammar.addPhrase("Move to Bill")
phrase_list_grammar.addPhrase("Move to Ted")
```

```JavaScript
var phraseListGrammar = SpeechSDK.PhraseListGrammar.fromRecognizer(reco);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

```Java
PhraseListGrammar phraseListGrammar = PhraseListGrammar.fromRecognizer(recognizer);
phraseListGrammar.addPhrase("Move to Ward");
phraseListGrammar.addPhrase("Move to Bill");
phraseListGrammar.addPhrase("Move to Ted");
```

>[!Note]
> El número máximo de listas de frases que usará el servicio de voz para establecer coincidencias con la voz es 1024 frases.

También puede borrar las frases asociadas con `PhraseListGrammar` llamando a clear().

```C++
phraselist->Clear();
```

```cs
phraseList.Clear();
```

```Python
phrase_list_grammar.clear()
```

```JavaScript
phraseListGrammar.clear();
```

```Java
phraseListGrammar.clear();
```

> [!NOTE]
> Los cambios realizados en el objeto `PhraseListGrammar` se aplican en el siguiente reconocimiento o tras una reconexión con el servicio de voz.

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de referencia del SDK de voz](speech-sdk.md)
