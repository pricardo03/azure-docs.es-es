---
title: 'Listas de frases: servicio de Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda cómo proporcionar a Servicios de voz una lista de frases mediante el objeto `PhraseListGrammar` para mejorar los resultados de reconocimiento de voz a texto.
services: cognitive-services
author: rhurey
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: rhurey
ms.openlocfilehash: 0e552d502184d1b537263c2c1f6b2a8562cdf791
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562788"
---
# <a name="phrase-lists-for-speech-to-text"></a>Listas de frases para reconocimiento de voz a texto

Al proporcionar a Servicios de voz una lista de frases, puede mejorar la precisión del reconocimiento de voz. Las listas de frases se usan para identificar frases conocidas en datos de audio, como el nombre de una persona o una ubicación específica.

Por ejemplo, si tiene el comando "Mover a" y "Cerca" como posible destino que se puede decir, puede añadir la entrada "Mover a Cerca". Al agregar una frase, aumentará la probabilidad de que, cuando se reconozca el audio, se reconozca "Mover a Cerca" en lugar de "Mover acerca".

A una lista de frases se pueden agregar palabras solas o frases completas. Durante el reconocimiento, se usa una entrada de una lista de frases si el audio incluye una coincidencia exacta. Siguiendo con el ejemplo anterior, si la lista de frases incluye "Mover a Cerca" y la frase capturada es "Mover acerca lentamente", el resultado del reconocimiento será "Mover a Cerca lentamente".

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
> Los cambios realizados en el objeto `PhraseListGrammar` se aplican en el siguiente reconocimiento o tras una reconexión con Servicios de voz.

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de referencia del SDK de voz](speech-sdk.md)
