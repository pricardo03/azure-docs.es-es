---
title: 'Devuelva las N mejores traducciones: Translator Text API'
titlesuffix: Azure Cognitive Services
description: Devuelva las N mejores traducciones con Microsoft Translator Text API.
services: cognitive-services
author: Jann-Skotdal
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: conceptual
ms.date: 12/14/2017
ms.author: v-jansko
ms.openlocfilehash: a0f4e4dd5cea9c3ea7b682e6372d2ffa8726e6ef
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49646480"
---
# <a name="how-to-return-n-best-translations"></a>Devolución de las N mejores traducciones

> [!NOTE]
> Este método está obsoleto. No está disponible en la versión 3.0 de Translator Text API.

Los métodos GetTranslations() y GetTranslationsArray() de Microsoft Translator API incluyen una marca booleana opcional "IncludeMultipleMTAlternatives".
El método devolverá hasta maxTranslations alternativas, donde la diferencia se proporciona a partir de la lista de N mejores del motor de traducción.

La firma es:

**Sintaxis**

| C# |
|:---|
| GetTranslationsResponse Microsoft.Translator.GetTranslations(appId, text, from, to, maxTranslations, options); |

**Parámetros**

| Parámetro | DESCRIPCIÓN |
|:---|:---|
| appId | **Obligatorio** Si se usa el encabezado de autorización, deje el campo appid vacío o incluya una cadena que contenga "Bearer" + " " + token de acceso.|
| text | **Obligatorio** Una cadena que representa el texto que se va a traducir. El tamaño del texto no debe superarlos 10 000 caracteres.|
| De | **Obligatorio** Una cadena que representa el código de idioma del texto que se va a traducir. |
| to | **Obligatorio** Una cadena que representa el código de idioma al que se va a traducir el texto. |
| maxTranslations | **Obligatorio** Un entero que representa el número máximo de traducciones que se van a devolver. |
| options | **Opcional** Un objeto TranslateOptions que contiene los valores que se enumeran a continuación. Son todos opcionales y toman como valor predeterminado las opciones de configuración más comunes.

* Category: la única opción admitida, así como la predeterminada, es "general".
* ContentType: la única opción admitida, así como la predeterminada, es "text/plain".
* State: estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Se devolverá el mismo contenido en la respuesta.
* IncludeMultipleMTAlternatives: marca para determinar si se devuelve más de un alternativas desde el motor de MT. El valor predeterminado es false e incluye solo una alternativa.

## <a name="ratings"></a>Clasificaciones
Las clasificaciones se aplican de la siguiente manera: la mejor traducción automática tiene una clasificación de 5.
Las alternativas de traducción (las N mejores) generadas automáticamente tienen una clasificación de 0 y un grado de coincidencia de 100.

## <a name="number-of-alternatives"></a>Número de alternativas
El número de alternativas devueltas va hasta maxTranslations, pero puede ser menor.

## <a name="language-pairs"></a>Pares de idiomas
Esta funcionalidad no está disponible para las traducciones entre chino simplificado y tradicional, en ambas direcciones. Está disponible para el resto de pares de idiomas admitidos de Microsoft Translator.
