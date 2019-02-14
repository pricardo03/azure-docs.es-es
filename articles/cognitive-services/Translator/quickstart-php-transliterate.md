---
title: 'Inicio rápido: Conversión de script de texto, PHP (Translator Text API)'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, se convierte texto en un lenguaje de un script a otro mediante el uso de Translator Text API con PHP.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 02/08/2019
ms.author: erhopf
ms.openlocfilehash: add0dd2898ebaf91991c0d07e90d34c345d6b3cb
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55982075"
---
# <a name="quickstart-transliterate-text-with-the-translator-text-rest-api-php"></a>Inicio rápido: Transliteración de texto con Translator Text REST API (PHP)

En esta guía de inicio rápido se convierte texto en un idioma de un script en otro mediante Translator Text API.

## <a name="prerequisites"></a>Requisitos previos

Se requiere [PHP 5.6.x](http://php.net/downloads.php) para ejecutar este código.

Para usar Translator Text API, también necesita una clave de suscripción; consulte [Cómo suscribirse a Translator Text API](translator-text-how-to-signup.md).

## <a name="transliterate-request"></a>Solicitud Transliterate

La siguiente convierte texto en un idioma de un script en otro script mediante el método [Transliterate](./reference/v3-0-transliterate.md).

1. Cree un nuevo proyecto de PHP en el editor de código que prefiera.
2. Agregue el código que se proporciona a continuación.
3. Reemplace el valor `key` por una clave de acceso válida para la suscripción.
4. Ejecute el programa.

```php
<?php

// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
$key = 'ENTER KEY HERE';

$host = "https://api.cognitive.microsofttranslator.com";
$path = "/transliterate?api-version=3.0";

// Transliterate text in Japanese from Japanese script (i.e. Hiragana/Katakana/Kanji) to Latin script.
$params = "&language=ja&fromScript=jpan&toScript=latn";

// Transliterate "good afternoon".
$text = "こんにちは";

if (!function_exists('com_create_guid')) {
  function com_create_guid() {
    return sprintf( '%04x%04x-%04x-%04x-%04x-%04x%04x%04x',
        mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff ),
        mt_rand( 0, 0xffff ),
        mt_rand( 0, 0x0fff ) | 0x4000,
        mt_rand( 0, 0x3fff ) | 0x8000,
        mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff ), mt_rand( 0, 0xffff )
    );
  }
}

function Transliterate ($host, $path, $key, $params, $content) {

    $headers = "Content-type: application/json\r\n" .
        "Content-length: " . strlen($content) . "\r\n" .
        "Ocp-Apim-Subscription-Key: $key\r\n" .
        "X-ClientTraceId: " . com_create_guid() . "\r\n";

    // NOTE: Use the key 'http' even if you are making an HTTPS request. See:
    // http://php.net/manual/en/function.stream-context-create.php
    $options = array (
        'http' => array (
            'header' => $headers,
            'method' => 'POST',
            'content' => $content
        )
    );
    $context  = stream_context_create ($options);
    $result = file_get_contents ($host . $path . $params, false, $context);
    return $result;
}

$requestBody = array (
    array (
        'Text' => $text,
    ),
);
$content = json_encode($requestBody);

$result = Transliterate ($host, $path, $key, $params, $content);

// Note: We convert result, which is JSON, to and from an object so we can pretty-print it.
// We want to avoid escaping any Unicode characters that result contains. See:
// http://php.net/manual/en/function.json-encode.php
$json = json_encode(json_decode($result), JSON_UNESCAPED_UNICODE | JSON_PRETTY_PRINT);
echo $json;
?>
```

## <a name="transliterate-response"></a>Respuesta de Transliterate

Se devuelve una respuesta correcta en JSON, tal como se muestra en el siguiente ejemplo:

```json
[
  {
    "text": "konnnichiha",
    "script": "latn"
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

Explore el código de ejemplo de esta guía de inicio rápido y otros, incluida la traducción y la identificación del idioma, así como otros proyectos de Translator Text de ejemplo de GitHub.

> [!div class="nextstepaction"]
> [Explorar ejemplos de PHP en GitHub](https://aka.ms/TranslatorGitHub?type=&language=php)
