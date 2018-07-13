---
title: Moderación de catálogo de comercio electrónico con aprendizaje automático e inteligencia artificial con Azure Content Moderator | Microsoft Docs
description: Moderación automática de los catálogos de comercio electrónico con aprendizaje automático e inteligencia artificial
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 09/25/2017
ms.author: sajagtap
ms.openlocfilehash: 6177758eaa3e611ad67da0778d889df48b052d90
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37095758"
---
# <a name="ecommerce-catalog-moderation-with-machine-learning"></a>Moderación de catálogo de comercio electrónico con aprendizaje automático

En este tutorial, aprenderá a implementar la moderación inteligente de catálogos de comercio electrónico basada en aprendizaje automático, mediante la combinación de tecnologías de inteligencia artificial asistidas automáticamente con la moderación humana para proporcionar un sistema de catálogo inteligente.

![Imágenes de productos clasificadas](images/tutorial-ecommerce-content-moderator.PNG)

## <a name="business-scenario"></a>Escenario empresarial

Use tecnologías asistidas automáticamente para clasificar y moderar imágenes de productos de estas categorías:

1. Adultos (desnudez)
2. Subido de tono (provocativo)
3. Celebridades
4. Banderas de Estados Unidos
5. Juguetes
6. Bolígrafos

## <a name="tutorial-steps"></a>Pasos del tutorial

Este tutorial le guiará a través de estos pasos:

1. Registro y creación de un equipo de Content Moderator.
2. Configuración de etiquetas de moderación para posibles celebridades y contenidos con banderas.
3. Uso de la API de imagen de Content Moderator para detectar posible contenido para adultos o subido de tono.
4. Uso de Computer Vision API para detectar posibles celebridades.
5. Uso de Custom Vision Service para detectar la posible presencia de banderas.
6. Presentación de los resultados matizados de la búsqueda para una revisión humana y la toma de decisión final.

## <a name="create-a-team"></a>Creación de un equipo

Consulte la página de [inicio rápido](quick-start.md) para registrarse en Content Moderator y crear un equipo. Observe la opción **Id. de equipo** en la página **Credenciales**.


## <a name="define-custom-tags"></a>Definición de etiquetas personalizadas

Consulte el artículo sobre [etiquetas](https://docs.microsoft.com/azure/cognitive-services/content-moderator/review-tool-user-guide/tags) para agregar etiquetas personalizadas. Además de las etiquetas **para adultos** y **subido de tono**, las nuevas etiquetas permiten que la herramienta de revisión muestre los nombres descriptivos de las etiquetas.

En este caso, se definen estas etiquetas personalizadas (**celebridad**, **bandera**, **EE. UU.**, **juguete**, **bolígrafo**):

![Configuración de etiquetas personalizadas](images/tutorial-ecommerce-tags2.PNG)

## <a name="list-your-api-keys-and-endpoints"></a>Enumeración de las claves y puntos de conexión de API

1. El tutorial utiliza tres API y las correspondientes claves y puntos de conexión.
2. Los puntos de conexión de la API son diferentes en función de las regiones de la suscripción y del identificador del equipo de revisión de Content Moderator.

> [!NOTE]
> El tutorial está diseñado para utilizar las claves de suscripción de las regiones visibles en los siguientes puntos de conexión. Asegúrese de que las claves de API coinciden con los identificadores URI ya que, en caso contrario, es posible que las claves no funcionen con los siguientes puntos de conexión:

         // Your API keys
        public const string ContentModeratorKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string ComputerVisionKey = "XXXXXXXXXXXXXXXXXXXX";
        public const string CustomVisionKey = "XXXXXXXXXXXXXXXXXXXX";

        // Your end points URLs will look different based on your region and Content Moderator Team ID.
        public const string ImageUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/moderate/v1.0/ProcessImage/Evaluate";
        public const string ReviewUri = "https://westus.api.cognitive.microsoft.com/contentmoderator/review/v1.0/teams/YOURTEAMID/reviews";
        public const string ComputerVisionUri = "https://westcentralus.api.cognitive.microsoft.com/vision/v1.0";
        public const string CustomVisionUri = "https://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/Prediction/XXXXXXXXXXXXXXXXXXXX/url";

## <a name="scan-for-adult-and-racy-content"></a>Búsqueda de contenido para adultos y subido de tono

1. La función emplea una dirección URL de imagen y una matriz de pares de clave-valor como parámetros.
2. Llama a API de imagen de Content Moderator para obtener las puntuaciones de las categorías "para adultos" y "subido de tono".
3. Si la puntuación es superior a 0,4 (el intervalo es de 0 a 1), establece el valor de la matriz **ReviewTags** en **True**.
4. La matriz **ReviewTags** se utiliza para resaltar la etiqueta correspondiente en la herramienta de revisión.

        public static bool EvaluateAdultRacy(string ImageUrl, ref KeyValuePair[] ReviewTags)
        {
            float AdultScore = 0;
            float RacyScore = 0;

            var File = ImageUrl;
            string Body = $"{{\"DataRepresentation\":\"URL\",\"Value\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(ImageUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // {“answers”:[{“answer”:“Hello”,“questions”:[“Hi”],“score”:100.0}]}
                // Parse the response body. Blocking!
                GetAdultRacyScores(response.Content.ReadAsStringAsync().Result, out AdultScore, out RacyScore);
            }

            ReviewTags[0] = new KeyValuePair();
            ReviewTags[0].Key = "a";
            ReviewTags[0].Value = "false";
            if (AdultScore > 0.4)
            {
                ReviewTags[0].Value = "true";
            }

            ReviewTags[1] = new KeyValuePair();
            ReviewTags[1].Key = "r";
            ReviewTags[1].Value = "false";
            if (RacyScore > 0.3)
            {
                ReviewTags[1].Value = "true";
            }
            return response.IsSuccessStatusCode;
        }

## <a name="scan-for-celebrities"></a>Búsqueda de celebridades

1. Regístrese en una [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision) de [Computer Vision API](https://azure.microsoft.com/services/cognitive-services/computer-vision/).
2. Haga clic en el botón **Obtener clave de API**.
3. Acepte los términos.
4. Para iniciar sesión, elija en la lista de cuentas de Internet disponibles.
5. Anote las claves de API que aparecen en la página del servicio.
    
   ![Claves de Computer Vision API](images/tutorial-computer-vision-keys.PNG)
    
6. Consulte el código fuente del proyecto para ver la función que examina la imagen con Computer Vision API.

         public static bool EvaluateComputerVisionTags(string ImageUrl, string ComputerVisionUri, string ComputerVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage Response = CallAPI(ComputerVisionUri, ComputerVisionKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            if (Response.IsSuccessStatusCode)
            {
                ReviewTags[2] = new KeyValuePair();
                ReviewTags[2].Key = "cb";
                ReviewTags[2].Value = "false";

                ComputerVisionPrediction CVObject = JsonConvert.DeserializeObject<ComputerVisionPrediction>(Response.Content.ReadAsStringAsync().Result);

                if ((CVObject.categories[0].detail != null) && (CVObject.categories[0].detail.celebrities.Count() > 0))
                {                 
                    ReviewTags[2].Value = "true";
                }
            }

            return Response.IsSuccessStatusCode;
        }

## <a name="classify-into-flags-toys-and-pens"></a>Clasificación en banderas, juguetes y bolígrafos

1. [Inicie sesión](https://azure.microsoft.com/en-us/services/cognitive-services/custom-vision-service/) en la [versión preliminar de Custom Vision API](https://www.customvision.ai/).
2. Use la [guía de inicio rápido](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) para compilar el clasificador personalizado para detectar la posible presencia de banderas, juguetes y bolígrafos.
   ![Imágenes de entrenamiento de Custom Vision](images/tutorial-ecommerce-custom-vision.PNG)
3. [Obtenga la dirección URL del punto de conexión de predicción](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/use-prediction-api) del clasificador personalizado.
4. Consulte el código fuente del proyecto para ver la función que llama al punto de conexión de predicción del clasificador personalizado para examinar la imagen.

        public static bool EvaluateCustomVisionTags(string ImageUrl, string CustomVisionUri, string CustomVisionKey, ref KeyValuePair[] ReviewTags)
        {
            var File = ImageUrl;
            string Body = $"{{\"URL\":\"{File}\"}}";

            HttpResponseMessage response = CallAPI(CustomVisionUri, CustomVisionKey, CallType.POST,
                                                   "Prediction-Key", "application/json", "", Body);

            if (response.IsSuccessStatusCode)
            {
                // Parse the response body. Blocking!
                SaveCustomVisionTags(response.Content.ReadAsStringAsync().Result, ref ReviewTags);
            }
            return response.IsSuccessStatusCode;
        }       
 
## <a name="reviews-for-human-in-the-loop"></a>Revisiones humanas

1. En las secciones anteriores, examinó las imágenes entrantes en busca de contenido para adultos o subido de tono (Content Moderator), celebridades (Computer Vision) y banderas (Custom Vision).
2. Según los umbrales de coincidencia de cada examen, haga que estén disponibles los casos matizados para una revisión humana en la herramienta de revisión.
        public static bool CreateReview(string ImageUrl, KeyValuePair[] Metadata) {

            ReviewCreationRequest Review = new ReviewCreationRequest();
            Review.Item[0] = new ReviewItem();
            Review.Item[0].Content = ImageUrl;
            Review.Item[0].Metadata = new KeyValuePair[MAXTAGSCOUNT];
            Metadata.CopyTo(Review.Item[0].Metadata, 0);

            //SortReviewItems(ref Review);

            string Body = JsonConvert.SerializeObject(Review.Item);

            HttpResponseMessage response = CallAPI(ReviewUri, ContentModeratorKey, CallType.POST,
                                                   "Ocp-Apim-Subscription-Key", "application/json", "", Body);

            return response.IsSuccessStatusCode;
        }

## <a name="submit-batch-of-images"></a>Envío del lote de imágenes

1. En este tutorial se supone que existe un directorio "C:Test" con un archivo de texto que tiene una lista de direcciones URL de imágenes.
2. El código siguiente comprueba la existencia del archivo y lee todas las direcciones URL en la memoria.
            // Check for a test directory for a text file with the list of Image URLs to scan var topdir = @"C:\test\"; var Urlsfile = topdir + "Urls.txt";

            if (!Directory.Exists(topdir))
                return;

            if (!File.Exists(Urlsfile))
            {
                return;
            }

            // Read all image URLs in the file
            var Urls = File.ReadLines(Urlsfile);

## <a name="initiate-all-scans"></a>Iniciar todos los exámenes

1. Esta función de nivel superior recorre en bucle todas las direcciones URL de imágenes del archivo de texto que se mencionó anteriormente.
2. Las analiza con cada API y si la puntuación de confianza coincide con nuestros criterios, se crea una revisión para moderadores humanos.
             // for each image URL in the file... foreach (var Url in Urls) { // Initiatize a new review tags array ReviewTags = new KeyValuePair[MAXTAGSCOUNT];

                // Evaluate for potential adult and racy content with Content Moderator API
                EvaluateAdultRacy(Url, ref ReviewTags);

                // Evaluate for potential presence of celebrity (ies) in images with Computer Vision API
                EvaluateComputerVisionTags(Url, ComputerVisionUri, ComputerVisionKey, ref ReviewTags);

                // Evaluate for potential presence of custom categories other than Marijuana
                EvaluateCustomVisionTags(Url, CustomVisionUri, CustomVisionKey, ref ReviewTags);

                // Create review in the Content Moderator review tool
                CreateReview(Url, ReviewTags);
            }

## <a name="license"></a>Licencias

Todos los SDK de Microsoft Cognitive Services y los ejemplos tienen una licencia de MIT. Para más información, consulte [LICENCIA](https://microsoft.mit-license.org/).

## <a name="developer-code-of-conduct"></a>Código de conducta del desarrollador

Se espera que los desarrolladores que usen Cognitive Services, incluidos este ejemplo y la biblioteca cliente, cumplan con el "Código de conducta del desarrollador de Microsoft Cognitive Services" que se encuentra en http://go.microsoft.com/fwlink/?LinkId=698895.

## <a name="next-steps"></a>Pasos siguientes

Compile y amplíe el tutorial mediante los [archivos de origen del proyecto](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration) en Github.
