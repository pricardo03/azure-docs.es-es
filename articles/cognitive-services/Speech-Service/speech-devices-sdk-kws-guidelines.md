---
title: 'Directrices de nomenclatura de palabras clave: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Crear una palabra clave efectiva es fundamental para garantizar que el dispositivo responda de forma coherente y precisa.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/11/2019
ms.author: dapine
ms.openlocfilehash: 3c3943f7269fa2d0ed25acf2215549635b5f16ac
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75453327"
---
# <a name="guidelines-for-creating-an-effective-keyword"></a>Directrices para crear una palabra clave efectiva

Crear una palabra clave efectiva es fundamental para garantizar que el dispositivo responda de forma coherente y precisa. La personalización de la palabra clave es una forma eficaz de diferenciar el dispositivo y de reforzar la marca. En este artículo, aprenderá algunos principios de orientación para crear una palabra clave efectiva.

## <a name="choose-an-effective-keyword"></a>Elección de una palabra clave eficaz

Tenga en cuenta las siguientes directrices al elegir una palabra clave:

* La palabra clave debe ser una palabra o frase en inglés.

* No debería tardar más de dos segundos en decirse.

* Las palabras de 4 a 7 sílabas funcionan mejor. Por ejemplo, "Hey, Computer" es una buena palabra clave, mientras que "Hey" no es adecuada.

* Las palabras clave deben seguir las reglas comunes de pronunciación en inglés.

* Una palabra única o incluso inventada que siga las reglas comunes de pronunciación en inglés puede reducir los falsos positivos. Por ejemplo, "computerama" puede ser una buena palabra clave.

* No elija una palabra común. Por ejemplo, "eat" y "go" son palabras que la gente dice con frecuencia en una conversación ordinaria. Pueden ser desencadenadores falsos para su dispositivo.

* Evite usar una palabra clave que pueda tener pronunciaciones alternativas. Los usuarios tendrían que saber la pronunciación "correcta" para que su dispositivo respondiera. Por ejemplo, "509" puede pronunciarse como "cinco cero nueve" o "quinientos nueve". "R.E.I." puede pronunciarse como "r-e-i" o "rey". "Live" puede pronunciarse como "/līv/" o "/liv/".

* No utilice caracteres especiales, símbolos o dígitos. Por ejemplo, "Go#" y "20 + cats" podrían ser palabras clave problemáticas. Sin embargo, "go sharp" o "twenty plus cats" pueden funcionar. Todavía puede utilizar los símbolos en su marca y utilizar el marketing y la documentación para reforzar la pronunciación adecuada.

> [!NOTE]
> Si elige una palabra registrada como marca comercial como palabra clave, asegúrese de que es el propietario de esa marca comercial, o bien de tener el permiso del propietario para usar la palabra. Microsoft no es responsable de ningún problema legal que pueda surgir de su elección de la palabra clave.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [crear una palabra clave personalizada con Speech Studio](speech-devices-sdk-create-kws.md).
