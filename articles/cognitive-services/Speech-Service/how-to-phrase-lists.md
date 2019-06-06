---
title: Listas de frase - servicios de voz
titlesuffix: Azure Cognitive Services
description: Aprenda a proporcionar los servicios de voz con una lista de frases mediante la `PhraseListGrammar` objeto para mejorar los resultados de reconocimiento de voz a texto.
services: cognitive-services
author: rhurey
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 5/02/2019
ms.author: rhurey
ms.openlocfilehash: a3be5d28ebe394771a2d8b492f1f6a9c8a82fb9e
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515306"
---
# <a name="phrase-lists-for-speech-to-text"></a>Listas de frase de texto a voz

Al proporcionar los servicios de voz con una lista de frases, puede mejorar la precisión del reconocimiento de voz. Frase listas se usan para identificar las frases conocidas de datos de audio, como nombre de una persona o una ubicación específica.

Por ejemplo, si tiene un comando "Mover a" y un posible destino de "Ward" que se puede hablar, puede agregar una entrada de "Mover a Ward". Adición de una frase, aumentará la probabilidad de que cuando el audio se reconoce que se reconozca "Mover a Ward" en lugar de "Mover hacia".

Solo palabras o frases completas pueden agregarse a una lista de frases. Durante el reconocimiento, una entrada en una lista de frases se utiliza si se incluye una coincidencia exacta en el audio. Compilar en el ejemplo anterior, si la lista de frases incluye "Mover a Ward" y la frase capturado es "Mover hacia lenta", entonces el resultado de reconocimiento se producirá "Mover a Ward lenta".

## <a name="how-to-use-phrase-lists"></a>Uso de listas de frase

Los ejemplos siguientes muestran cómo crear una lista de frases mediante la `PhraseListGrammar` objeto.

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
> El número máximo de frase listas que usará el servicio de voz para que coincida con la voz es 1024 frases.

También puede borrar las frases asociadas con el `PhraseListGrammar` por clear() que realiza la llamada.

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
> Cambia a un `PhraseListGrammar` objeto tienen efecto sobre el reconocimiento siguiente o siguiendo una reconexión con los servicios de voz.

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de referencia de Speech SDK](speech-sdk.md)
