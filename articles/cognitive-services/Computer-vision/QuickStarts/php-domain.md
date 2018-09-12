---
title: 'Guía de inicio rápido de Computer Vision API para PHP: Modelo de dominio | Microsoft Docs'
titleSuffix: Microsoft Cognitive Services
description: En esta guía de inicio rápido, usará un modelo de dominio para identificar sitios emblemáticos en una imagen mediante Computer Vision con PHP en Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 335065b45781dd2712f5416fb03a2f8726182472
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2018
ms.locfileid: "43772256"
---
# <a name="quickstart-use-a-domain-model---rest-php"></a>Guía de inicio rápido: Uso de un modelo de dominio (REST, PHP)

En esta guía de inicio rápido, usará un modelo de dominio para identificar sitios emblemáticos o personales en una imagen con Computer Vision.

## <a name="prerequisites"></a>Requisitos previos

Para usar Computer Vision, necesita una clave de suscripción; consulte [Obtención de claves de suscripción](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="recognize-landmark-request"></a>Solicitud Recognize Landmark

Con el método [Recognize Domain Specific Content](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200), puede identificar un conjunto específico de objetos en una imagen. Los dos modelos específicos del dominio que están actualmente disponibles son _personajes_ y _sitios emblemáticos_.

Para ejecutar el ejemplo, siga estos pasos:

1. Copie el código siguiente en un editor.
1. Reemplace `<Subscription Key>` por una clave de suscripción válida.
1. Si es necesario, cambie `uriBase` para usar la ubicación donde obtuvo las claves de suscripción.
1. También puede establecer `imageUrl` en la imagen que quiere analizar.
1. También tiene la opción de establecer `domain` en `celebrities` para usar el modelo de personajes.
1. Guarde el archivo con la extensión `.php`.
1. Abra el archivo en una ventana del explorador con compatibilidad con PHP.

En el ejemplo siguiente se identifica un punto de referencia en una imagen.

En este ejemplo se usa el paquete [HTTP_Request2](http://pear.php.net/package/HTTP_Request2) de PHP5.

```php
<html>
<head>
    <title>Analyze Domain Model Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

// Change 'landmarks' to 'celebrities' to use the Celebrities model.
$domain = 'landmarks';

$imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . 'models/' . $domain . '/analyze');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'model' => $domain
);
$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body parameters
$body = json_encode(array('url' => $imageUrl));

// Request body
$request->setBody($body);

try
{
    $response = $request->send();
    echo "<pre>" .
        json_encode(json_decode($response->getBody()), JSON_PRETTY_PRINT) . "</pre>";
}
catch (HttpException $ex)
{
    echo "<pre>" . $ex . "</pre>";
}
?>
</body>
</html>
```

## <a name="recognize-landmark-response"></a>Respuesta de Recognize Landmark

Se devuelve una respuesta correcta en código JSON, por ejemplo:

```json
{
    "result": {
        "landmarks": [
            {
                "name": "Space Needle",
                "confidence": 0.9998177886009216
            }
        ]
    },
    "requestId": "4d26587b-b2b9-408d-a70c-1f8121d84b0d",
    "metadata": {
        "height": 4132,
        "width": 2096,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Computer Vision API que se usan para analizar una imagen, detectar personajes y sitios emblemáticos, crear una miniatura y extraer texto impreso y escrito a mano. Para experimentar rápidamente con las versiones de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Exploración de las versiones de Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
