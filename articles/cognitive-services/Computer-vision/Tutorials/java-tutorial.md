---
title: Realización de operaciones de imagen (Java)
titlesuffix: Azure Cognitive Services
description: Explore una aplicación básica de Java Swing que usa Computer Vision API en Azure Cognitive Services. Realice OCR, cree miniaturas y trabaje con características visuales en una imagen.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.author: kefre
ms.custom: seodec18
ms.date: 04/30/2019
ms.openlocfilehash: a22308e0c7ff924205f715692d011a4572b2bdb8
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232635"
---
# <a name="use-computer-vision-features-with-the-rest-api-and-java"></a>Usar las características de Computer Vision con la API de REST y Java

En este tutorial se muestran las características de la API de REST Computer Vision de Azure Cognitive Services.

Explore una aplicación de Java Swing que usa la API de REST Computer Vision para realizar el reconocimiento óptico de caracteres (OCR), crear miniaturas con recorte inteligente, y detectar, clasificar, etiquetar y describir características visuales, como caras, en una imagen. Este ejemplo le permite enviar la dirección URL de una imagen para su análisis o procesamiento. Puede usar este ejemplo de código abierto como plantilla para crear su propia aplicación de Java para que use la API de REST Computer Vision.

En este tutorial se explica cómo usar Computer Vision para:

> [!div class="checklist"]
> * Análisis de una imagen
> * Identificar un punto de referencia artificial o natural en una imagen
> * Identificar a una celebridad en una imagen
> * Crear una miniatura de calidad de una imagen
> * Leer texto impreso en una imagen
> * Leer texto manuscrito en una imagen

La aplicación de formulario Java Swing ya se ha escrito, pero no tiene ninguna funcionalidad. En este tutorial, agregará el código específico de la API REST Computer Vision API para completar la funcionalidad de la aplicación.

## <a name="prerequisites"></a>Requisitos previos

### <a name="platform-requirements"></a>Requisitos de la plataforma

Este tutorial se ha desarrollado con NetBeans IDE. En concreto, la versión **Java SE** de NetBeans, que puede [descargar aquí](https://netbeans.org/downloads/index.html).

### <a name="subscribe-to-computer-vision-api-and-get-a-subscription-key"></a>Suscripción a Computer Vision API y obtención de una clave de suscripción

Antes de crear el ejemplo, debe suscribirse a Computer Vision API, que forma parte de Azure Cognitive Services. Para más información sobre la administración de claves y suscripciones, consulte [Suscripciones](https://azure.microsoft.com/try/cognitive-services/). En este tutorial son válidas las claves principal y secundaria.

## <a name="acquire-incomplete-tutorial-project"></a>Adquirir el proyecto del tutorial incompleta

### <a name="download-the-project"></a>Descarga del proyecto

1. Vaya al repositorio del [tutorial de Computer Vision para Java en Cognitive Services](https://github.com/Azure-Samples/cognitive-services-java-computer-vision-tutorial).
1. Haga clic en el botón **Clonar o Descargar**.
1. Haga clic en **Descargar ZIP** para descargar un archivo ZIP del proyecto del tutorial.

No hay ninguna necesidad de extraer el contenido del archivo ZIP porque NetBeans importa el proyecto desde el archivo ZIP.

### <a name="import-the-tutorial-project"></a>Importación del proyecto del tutorial

Importe el archivo **cognitive-services-java-computer-vision-tutorial-master.zip** en NetBeans.

1. En NetBeans, haga clic en **File** (Archivo)  >  **Import Project** (Importar proyecto)  >  **From ZIP...** (Desde archivo ZIP). Aparece el cuadro de diálogo **Import Project(s) from ZIP** (Importar proyectos desde archivo ZIP).
1. En el campo **ZIP File:** (Archivo ZIP:), haga clic en el botón **Examinar** para buscar el archivo **cognitive-services-java-computer-vision-tutorial-master.zip** y luego haga clic en **Open** (Abrir).
1. Haga clic en **Import** (Importar) en el cuadro de diálogo **Import Project(s) from ZIP** (Importar proyectos desde archivo ZIP).
1. En el panel **Projects** (Proyectos), expanda **ComputerVision** > **Source Packages** (Paquetes de origen)  > **&lt;default package&gt;** (Paquete predeterminado). 
   Algunas versiones de NetBeans utilizan **src** en lugar de **Source Packages** >  (Paquetes de origen) **&lt;default package&gt;** (Paquete predeterminado). En ese caso, expanda **src**.
1. Haga doble clic en **MainFrame.java** para cargar el archivo en el editor de NetBeans. Aparece la pestaña **Design** (Diseño) del archivo **MainFrame.java**.
1. Haga clic en la pestaña **Source** (Origen) para ver el código fuente de Java.

### <a name="build-and-run-the-tutorial-project"></a>Compilación y ejecución del proyecto del tutorial

1. Presione **F6** para compilar y ejecutar la aplicación del tutorial.

    En la aplicación del tutorial, haga clic en una pestaña para que aparezca el panel para esa característica. Los botones tienen métodos vacíos, por lo que no hacen nada.

    En la parte inferior de la ventana se encuentran los campos **Subscription Key** (Clave de suscripción) y **Subscription Region** (Región de suscripción). Estos campos deben rellenarse con una clave de suscripción válida y la región correcta para esa clave de suscripción. Para obtener una clave de suscripción, vea [Suscripciones](https://azure.microsoft.com/try/cognitive-services/). Si ha obtenido la clave de suscripción en la evaluación gratuita de ese vínculo, la región predeterminada **westcentralus** es la región correcta para las claves de suscripción.

1. Salga de la aplicación del tutorial.

## <a name="add-tutorial-code-to-the-project"></a>Agregue el código del tutorial al proyecto

La aplicación Java Swing está configurada con seis pestañas. Cada pestaña muestra una función diferente de Computer Vision (analizar, OCR, etc.). Las seis secciones del tutorial no son interdependientes, por lo que puede agregar una sola sección, las seis o cualquier subconjunto de ellas. Las secciones se pueden agregar en cualquier orden.

### <a name="analyze-an-image"></a>Análisis de una imagen

La característica Analyze de Computer Vision examina una imagen en busca de más de 2000 objetos reconocibles, seres vivos, escenarios y acciones. Una vez completado el análisis, Analyze devuelve un objeto JSON que describe la imagen con etiquetas descriptivas, análisis de color o títulos, entre otros.

Para completar la característica Analyze de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-analyze-button"></a>Agregue el código de controlador de eventos para el botón analizar

El método del controlador de eventos **analyzeImageButtonActionPerformed** borra el formulario, muestra la imagen especificada en la dirección URL y llama al método **AnalyzeImage** para analizar la imagen. Cuando se devuelve **AnalyzeImage**, el método muestra la respuesta JSON con formato en el área de texto **Respuesta**, extrae el primer título de **JSONObject** y muestra el título y el nivel de confianza de que el título es correcto.

Copie y pegue el código siguiente en el método **analyzeImageButtonActionPerformed**.

> [!NOTE]
> NetBeans no le permitirá pegarlo en la línea de definición del método (```private void```) ni en la llave de cierre de ese método. Para copiar el código, copie las líneas entre la definición del método y la llave de cierre y péguelas en el contenido del método.

```java
    private void analyzeImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL analyzeImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        analyzeImage.setIcon(new ImageIcon());
        analyzeCaptionLabel.setText("");
        analyzeResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            analyzeImageUrl = new URL(analyzeImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(analyzeImageUrl);
            scaleAndShowImage(bImage, analyzeImage);
        } catch(IOException e) {
            analyzeResponseTextArea.setText("Error loading Analyze image: " + e.getMessage());
            return;
        }
        
        // Analyze the image.
        JSONObject jsonObj = AnalyzeImage(analyzeImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        analyzeResponseTextArea.setText(jsonObj.toString(2));
                
        // Extract the text and confidence from the first caption in the description object.
        if (jsonObj.has("description") && jsonObj.getJSONObject("description").has("captions")) {

            JSONObject jsonCaption = jsonObj.getJSONObject("description").getJSONArray("captions").getJSONObject(0);
            
            if (jsonCaption.has("text") && jsonCaption.has("confidence")) {
                
                analyzeCaptionLabel.setText("Caption: " + jsonCaption.getString("text") + 
                        " (confidence: " + jsonCaption.getDouble("confidence") + ").");
            }
        }
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

El método **AnalyzeImage** ajusta la llamada API de REST para analizar una imagen. El método devuelve un elemento **JSONObject** que describe la imagen, o **null** si se produjo un error.

Copie y pegue el método **AnalyzeImage** justo debajo del método **analyzeImageButtonActionPerformed**.

```java
    /**
     * Encapsulates the Microsoft Cognitive Services REST API call to analyze an image.
     * @param imageUrl: The string URL of the image to analyze.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject AnalyzeImage(String imageUrl) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call for Analyze Image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseAnalyze;
            URIBuilder builder = new URIBuilder(uriString);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color,Adult");
            builder.setParameter("language", "en");

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // If we got a response, parse it and display it.
            if (entity != null)
            {
                // Return the JSONObject.
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            } else {
                // No response. Return null.
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
 ```

#### <a name="run-the-analyze-function"></a>Ejecute la función analizar

Presione **F6** para ejecutar la aplicación. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Escriba una dirección URL a la imagen que desea analizar y haga clic en el botón **Analyze Image** (Analizar imagen) para analizar una imagen y ver el resultado.

### <a name="recognize-a-landmark"></a>Reconocimiento de un lugar de interés

La característica Landmark de Computer Vision analiza una imagen en busca de lugares de interés naturales y artificiales, tales como montañas o edificios famosos. Una vez completado el análisis, Landmark devuelve un objeto JSON que identifica los lugares de interés que encuentre en la imagen.

Para completar la característica Landmark de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-form-button"></a>Adición del código de controlador de eventos para el botón del formulario

El método del controlador de eventos **landmarkImageButtonActionPerformed** borra el formulario, muestra la imagen especificada en la dirección URL y llama al método **LandmarkImage** para analizar la imagen. Cuando se devuelve **LandmarkImage**, el método muestra la respuesta JSON con formato en el área de texto **Respuesta**, después extrae el primer nombre del punto de referencia de **JSONObject** y muestra el nombre en la ventana junto con el nivel de confianza con el que el punto de referencia se identificó correctamente.

Copie y pegue el código siguiente en el método **landmarkImageButtonActionPerformed**.

> [!NOTE]
> NetBeans no le permitirá pegarlo en la línea de definición del método (```private void```) ni en la llave de cierre de ese método. Para copiar el código, copie las líneas entre la definición del método y la llave de cierre y péguelas en el contenido del método.

```java
    private void landmarkImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL landmarkImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        landmarkImage.setIcon(new ImageIcon());
        landmarkCaptionLabel.setText("");
        landmarkResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            landmarkImageUrl = new URL(landmarkImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(landmarkImageUrl);
            scaleAndShowImage(bImage, landmarkImage);
        } catch(IOException e) {
            landmarkResponseTextArea.setText("Error loading Landmark image: " + e.getMessage());
            return;
        }
        
        // Identify the landmark in the image.
        JSONObject jsonObj = LandmarkImage(landmarkImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        landmarkResponseTextArea.setText(jsonObj.toString(2));
                
        // Extract the text and confidence from the first caption in the description object.
        if (jsonObj.has("result") && jsonObj.getJSONObject("result").has("landmarks")) {

            JSONObject jsonCaption = jsonObj.getJSONObject("result").getJSONArray("landmarks").getJSONObject(0);
            
            if (jsonCaption.has("name") && jsonCaption.has("confidence")) {

                landmarkCaptionLabel.setText("Caption: " + jsonCaption.getString("name") + 
                        " (confidence: " + jsonCaption.getDouble("confidence") + ").");
            }
        }
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

El método **LandmarkImage** encapsula la llamada API de REST para analizar una imagen. El método devuelve un elemento **JSONObject** que describe los puntos de referencia encontrados en la imagen, o **null** si se produjo algún error.

Copie y pegue el método**LandmarkImage** justo debajo del método **landmarkImageButtonActionPerformed**.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to identify a landmark in an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject LandmarkImage(String imageUrl) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to identify a Landmark in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseLandmark;
            URIBuilder builder = new URIBuilder(uriString);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color");
            builder.setParameter("language", "en");

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // If we got a response, parse it and display it.
            if (entity != null)
            {
                // Return the JSONObject.
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            } else {
                // No response. Return null.
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
```

#### <a name="run-the-landmark-function"></a>Ejecute la función de punto de referencia

Presione **F6** para ejecutar la aplicación. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Haga clic en la pestaña **Landmark** (Punto de referencia), escriba una dirección URL de una imagen de un punto de referencia y haga clic en el botón **Analyze Image** (Analizar imagen) para analizar una imagen y ver el resultado.

### <a name="recognize-celebrities"></a>Reconocimiento de celebridades

La característica Celebrities de Computer Vision analiza una imagen en busca de personas famosas. Una vez completado el análisis, Celebrities devuelve un objeto JSON que identifica las personas famosas que encuentre en la imagen.

Para completar la característica Celebrities de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-celebrities-button"></a>Agregue el código de controlador de eventos para el botón de celebridades

El método del controlador de eventos **celebritiesImageButtonActionPerformed** borra el formulario, muestra la imagen especificada en la dirección URL y llama al método **CelebritiesImage** para analizar la imagen. Cuando se devuelve **CelebritiesImage**, el método muestra la respuesta JSON con formato en el área de texto **Respuesta**, después extrae el primer nombre de celebridades de **JSONObject** y muestra el nombre en la ventana junto con el nivel de confianza con el que la celebridad se identificó correctamente.

Copie y pegue el código siguiente en el método **celebritiesImageButtonActionPerformed**.

> [!NOTE]
> NetBeans no le permitirá pegarlo en la línea de definición del método (```private void```) ni en la llave de cierre de ese método. Para copiar el código, copie las líneas entre la definición del método y la llave de cierre y péguelas en el contenido del método.

```java
    private void celebritiesImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL celebritiesImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        celebritiesImage.setIcon(new ImageIcon());
        celebritiesCaptionLabel.setText("");
        celebritiesResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            celebritiesImageUrl = new URL(celebritiesImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(celebritiesImageUrl);
            scaleAndShowImage(bImage, celebritiesImage);
        } catch(IOException e) {
            celebritiesResponseTextArea.setText("Error loading Celebrity image: " + e.getMessage());
            return;
        }
        
        // Identify the celebrities in the image.
        JSONObject jsonObj = CelebritiesImage(celebritiesImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        celebritiesResponseTextArea.setText(jsonObj.toString(2));
                
        // Extract the text and confidence from the first caption in the description object.
        if (jsonObj.has("result") && jsonObj.getJSONObject("result").has("celebrities")) {

            JSONObject jsonCaption = jsonObj.getJSONObject("result").getJSONArray("celebrities").getJSONObject(0);
            
            if (jsonCaption.has("name") && jsonCaption.has("confidence")) {

                celebritiesCaptionLabel.setText("Caption: " + jsonCaption.getString("name") + 
                        " (confidence: " + jsonCaption.getDouble("confidence") + ").");
            }
        }
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

El método **CelebritiesImage** ajusta la llamada API de REST para analizar una imagen. El método devuelve un elemento **JSONObject** que describe las celebridades encontradas en la imagen, o **null** si se produjo un error.

Copie y pegue el método**CelebritiesImage** justo debajo del método **celebritiesImageButtonActionPerformed**.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to identify celebrities in an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject CelebritiesImage(String imageUrl) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to identify celebrities in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseCelebrities;
            URIBuilder builder = new URIBuilder(uriString);

            // Request parameters. All of them are optional.
            builder.setParameter("visualFeatures", "Categories,Description,Color");
            builder.setParameter("language", "en");

            // Prepare the URI for the REST API call.
            URI uri = builder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // If we got a response, parse it and display it.
            if (entity != null)
            {
                // Return the JSONObject.
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            } else {
                // No response. Return null.
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
```

#### <a name="run-the-celebrities-function"></a>Ejecutar la función de celebridades

Presione **F6** para ejecutar la aplicación. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Haga clic en la pestaña **Celebrities** (Celebridades), escriba una dirección URL de una imagen de una celebridad y haga clic en el botón **Analyze Image** (Analizar imagen) para analizar una imagen y ver el resultado.

### <a name="intelligently-generate-a-thumbnail"></a>Generación inteligente de una miniatura

La característica Thumbnail de Computer Vision genera una miniatura de una imagen. Con la característica **Smart Crop**, la característica Thumbnail identificará el área de interés de una imagen y centrará la miniatura en esta área para generar miniaturas más bonitas.

Para completar la característica Thumbnail de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-thumbnail-button"></a>Agregue el código de controlador de eventos para el botón de miniatura

El método del controlador de eventos **thumbnailImageButtonActionPerformed** borra el formulario, muestra la imagen especificada en la dirección URL y llama al método **getThumbnailImage** para crear la miniatura. Cuando se devuelve **getThumbnailImage**, el método muestra la miniatura generada.

Copie y pegue el código siguiente en el método **thumbnailImageButtonActionPerformed**.

> [!NOTE]
> NetBeans no le permitirá pegarlo en la línea de definición del método (```private void```) ni en la llave de cierre de ese método. Para copiar el código, copie las líneas entre la definición del método y la llave de cierre y péguelas en el contenido del método.

```java
    private void thumbnailImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL thumbnailImageUrl;
        JSONObject jsonError[] = new JSONObject[1];
        
        // Clear out the previous image, response, and thumbnail, if any.
        thumbnailSourceImage.setIcon(new ImageIcon());
        thumbnailResponseTextArea.setText("");
        thumbnailImage.setIcon(new ImageIcon());

        // Display the image specified in the text box.
        try {
            thumbnailImageUrl = new URL(thumbnailImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(thumbnailImageUrl);
            scaleAndShowImage(bImage, thumbnailSourceImage);
        } catch(IOException e) {
            thumbnailResponseTextArea.setText("Error loading image to thumbnail: " + e.getMessage());
            return;
        }
        
        // Get the thumbnail for the image.
        BufferedImage thumbnail = getThumbnailImage(thumbnailImageUrl.toString(), jsonError);
        
        // A non-null value indicates error.
        if (jsonError[0] != null) {
            // Format and display the JSON error.
            thumbnailResponseTextArea.setText(jsonError[0].toString(2));
            return;
        }
        
        // Display the thumbnail.
        if (thumbnail != null) {
            scaleAndShowImage(thumbnail, thumbnailImage);
        }
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

El método **getThumbnailImage** ajusta la llamada API de REST para analizar una imagen. El método devuelve un elemento **BufferedImage** que contiene la miniatura, o **null** si se produjo un error. Se devolverá el mensaje de error en el primer elemento de la matriz de cadenas **jsonError**.

Copie y pegue el siguiente método **getThumbnailImage** justo debajo del método **thumbnailImageButtonActionPerformed**.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to create a thumbnail for an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A BufferedImage containing the thumbnail, or null if a runtime error occurs. In the case
     * of an error, the error message will be returned in the first element of the jsonError string array.
     */
    private BufferedImage getThumbnailImage(String imageUrl, JSONObject[] jsonError) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to identify celebrities in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseThumbnail;
            URIBuilder uriBuilder = new URIBuilder(uriString);

            // Request parameters.
            uriBuilder.setParameter("width", "100");
            uriBuilder.setParameter("height", "150");
            uriBuilder.setParameter("smartCropping", "true");

            // Prepare the URI for the REST API call.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity requestEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(requestEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // Check for success.
            if (response.getStatusLine().getStatusCode() == 200)
            {
                // Return the thumbnail.
                return ImageIO.read(entity.getContent());
            }
            else
            {
                // Format and display the JSON error message.
                String jsonString = EntityUtils.toString(entity);
                jsonError[0] = new JSONObject(jsonString);
                return null;
            }
        }
        catch (Exception e)
        {
            String errorMessage = e.getMessage();
            System.out.println(errorMessage);
            jsonError[0] = new JSONObject(errorMessage);
            return null;
        }
    }
```

#### <a name="run-the-thumbnail-function"></a>Ejecute la función en miniatura

Presione **F6** para ejecutar la aplicación. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Haga clic en la pestaña **Thumbnail** (Miniatura), escriba una dirección URL a la imagen y haga clic en el botón **Generate Thumbnail** (Generar miniatura) para analizar una imagen y ver el resultado.

### <a name="read-printed-text-ocr"></a>Lectura de texto impreso (OCR)

La característica de reconocimiento óptico de caracteres (OCR) de Computer Vision analiza una imagen de texto impreso. Una vez completado el análisis, OCR devuelve un objeto JSON que contiene el texto y la ubicación del texto de la imagen.

Para completar la característica OCR de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-ocr-button"></a>Agregue el código de controlador de eventos para el botón de OCR

El método del controlador de eventos **ocrImageButtonActionPerformed** borra el formulario, muestra la imagen especificada en la dirección URL y llama al método **OcrImage** para analizar la imagen. Cuando se devuelve **OcrImage**, el método muestra el texto detectado como JSON con formato en el área de texto **Respuesta**.

Copie y pegue el código siguiente en el método **ocrImageButtonActionPerformed**.

> [!NOTE]
> NetBeans no le permitirá pegarlo en la línea de definición del método (```private void```) ni en la llave de cierre de ese método. Para copiar el código, copie las líneas entre la definición del método y la llave de cierre y péguelas en el contenido del método.

```java
    private void ocrImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL ocrImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        ocrImage.setIcon(new ImageIcon());
        ocrResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            ocrImageUrl = new URL(ocrImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(ocrImageUrl);
            scaleAndShowImage(bImage, ocrImage);
        } catch(IOException e) {
            ocrResponseTextArea.setText("Error loading OCR image: " + e.getMessage());
            return;
        }
        
        // Read the text in the image.
        JSONObject jsonObj = OcrImage(ocrImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        ocrResponseTextArea.setText(jsonObj.toString(2));
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

El método **OcrImage** ajusta la llamada API de REST para analizar una imagen. El método devuelve un elemento **JSONObject** de los datos JSON devueltos desde la llamada, o **null** si se produjo un error.

Copie y pegue el método siguiente **OcrImage** justo debajo del método **ocrImageButtonActionPerformed**.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to read text in an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject OcrImage(String imageUrl) {
        try (CloseableHttpClient httpclient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to read text in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseOcr;
            URIBuilder uriBuilder = new URIBuilder(uriString);

            // Request parameters.
            uriBuilder.setParameter("language", "unk");
            uriBuilder.setParameter("detectOrientation ", "true");

            // Prepare the URI for the REST API call.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response entity.
            HttpResponse response = httpclient.execute(request);
            HttpEntity entity = response.getEntity();

            // If we got a response, parse it and display it.
            if (entity != null)
            {
                // Return the JSONObject.
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            } else {
                // No response. Return null.
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
```

#### <a name="run-the-ocr-function"></a>Ejecute la función de reconocimiento óptico de caracteres

Presione **F6** para ejecutar la aplicación. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Haga clic en la pestaña **OCR**, escriba una dirección URL a una imagen de texto impreso y después haga clic en el botón **Read Image** (Leer imagen) para analizar una imagen y ver el resultado.

### <a name="read-handwritten-text-handwriting-recognition"></a>Lectura de texto manuscrito (reconocimiento de escritura a mano)

La característica Handwriting Recognition de Computer Vision analiza una imagen de texto escrito a mano. Una vez completado el análisis, Handwriting Recognition devuelve un objeto JSON que contiene el texto y la ubicación del texto de la imagen.

Para completar la característica Handwriting Recognition de la aplicación del tutorial, siga estos pasos:

#### <a name="add-the-event-handler-code-for-the-handwriting-button"></a>Agregue el código de controlador de eventos para el botón de escritura a mano

El método del controlador de eventos **handwritingImageButtonActionPerformed** borra el formulario, muestra la imagen especificada en la dirección URL y llama al método **HandwritingImage** para analizar la imagen. Cuando se devuelve **HandwritingImage**, el método muestra el texto detectado como JSON con formato en el área de texto **Respuesta**.

Copie y pegue el código siguiente en el método **handwritingImageButtonActionPerformed**.

> [!NOTE]
> NetBeans no le permitirá pegarlo en la línea de definición del método (```private void```) ni en la llave de cierre de ese método. Para copiar el código, copie las líneas entre la definición del método y la llave de cierre y péguelas en el contenido del método.

```java
    private void handwritingImageButtonActionPerformed(java.awt.event.ActionEvent evt) {
        URL handwritingImageUrl;
        
        // Clear out the previous image, response, and caption, if any.
        handwritingImage.setIcon(new ImageIcon());
        handwritingResponseTextArea.setText("");
        
        // Display the image specified in the text box.
        try {
            handwritingImageUrl = new URL(handwritingImageUriTextBox.getText());
            BufferedImage bImage = ImageIO.read(handwritingImageUrl);
            scaleAndShowImage(bImage, handwritingImage);
        } catch(IOException e) {
            handwritingResponseTextArea.setText("Error loading Handwriting image: " + e.getMessage());
            return;
        }
        
        // Read the text in the image.
        JSONObject jsonObj = HandwritingImage(handwritingImageUrl.toString());
        
        // A return of null indicates failure.
        if (jsonObj == null) {
            return;
        }
        
        // Format and display the JSON response.
        handwritingResponseTextArea.setText(jsonObj.toString(2));
    }
```

#### <a name="add-the-wrapper-for-the-rest-api-call"></a>Adición del contenedor para la llamada a API REST

La función **HandwritingImage** encapsula las dos llamadas API de REST necesarias para analizar una imagen. Como el reconocimiento de la escritura a mano es un proceso lento, se utiliza un proceso en dos pasos. La primera llamada envía la imagen para su procesamiento; la segunda llamada recupera el texto detectado una vez completado el procesamiento.

Una vez recuperado el texto, el método **HandwritingImage** devuelve un elemento **JSONObject** que describe el texto y las ubicaciones del texto, o **null** si se produjo un error.

Copie y pegue el método siguiente **HandwritingImage** justo debajo del método **handwritingImageButtonActionPerformed**.

```java
     /**
     * Encapsulates the Microsoft Cognitive Services REST API call to read handwritten text in an image.
     * @param imageUrl: The string URL of the image to process.
     * @return: A JSONObject describing the image, or null if a runtime error occurs.
     */
    private JSONObject HandwritingImage(String imageUrl) {
        try (CloseableHttpClient textClient = HttpClientBuilder.create().build();
             CloseableHttpClient resultClient = HttpClientBuilder.create().build())
        {
            // Create the URI to access the REST API call to read text in an image.
            String uriString = uriBasePreRegion + 
                    String.valueOf(subscriptionRegionComboBox.getSelectedItem()) + 
                    uriBasePostRegion + uriBaseHandwriting;
            URIBuilder uriBuilder = new URIBuilder(uriString);
            
            // Request parameters.
            uriBuilder.setParameter("handwriting", "true");

            // Prepare the URI for the REST API call.
            URI uri = uriBuilder.build();
            HttpPost request = new HttpPost(uri);

            // Request headers.
            request.setHeader("Content-Type", "application/json");
            request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());

            // Request body.
            StringEntity reqEntity = new StringEntity("{\"url\":\"" + imageUrl + "\"}");
            request.setEntity(reqEntity);

            // Execute the REST API call and get the response.
            HttpResponse textResponse = textClient.execute(request);
            
            // Check for success.
            if (textResponse.getStatusLine().getStatusCode() != 202) {
                // An error occurred. Return the JSON error message.
                HttpEntity entity = textResponse.getEntity();
                String jsonString = EntityUtils.toString(entity);
                return new JSONObject(jsonString);
            }
            
            String operationLocation = null;

            // The 'Operation-Location' in the response contains the URI to retrieve the recognized text.
            Header[] responseHeaders = textResponse.getAllHeaders();
            for(Header header : responseHeaders) {
                if(header.getName().equals("Operation-Location"))
                {
                    // This string is the URI where you can get the text recognition operation result.
                    operationLocation = header.getValue();
                    break;
                }
            }
            
            // NOTE: The response may not be immediately available. Handwriting recognition is an
            // async operation that can take a variable amount of time depending on the length
            // of the text you want to recognize. You may need to wait or retry this operation.
            //
            // This example checks once per second for ten seconds.
            
            JSONObject responseObj = null;
            int i = 0;
            do {
                // Wait one second.
                Thread.sleep(1000);
                
                // Check to see if the operation completed.
                HttpGet resultRequest = new HttpGet(operationLocation);
                resultRequest.setHeader("Ocp-Apim-Subscription-Key", subscriptionKeyTextField.getText());
                HttpResponse resultResponse = resultClient.execute(resultRequest);
                HttpEntity responseEntity = resultResponse.getEntity();
                if (responseEntity != null)
                {
                    // Get the JSON response.
                    String jsonString = EntityUtils.toString(responseEntity);
                    responseObj = new JSONObject(jsonString);
                }
            }
            while (i < 10 && responseObj != null &&
                    !responseObj.getString("status").equalsIgnoreCase("Succeeded"));
            
            // If the operation completed, return the JSON object.
            if (responseObj != null) {
                return responseObj;
            } else {
                // Return null for timeout error.
                System.out.println("Timeout error.");
                return null;
            }
        }
        catch (Exception e)
        {
            // Display error message.
            System.out.println(e.getMessage());
            return null;
        }
    }
```

#### <a name="run-the-handwriting-function"></a>Ejecute la función de escritura a mano

Para ejecutar la aplicación, presione **F6**. Coloque la clave de suscripción en el campo **Subscription Key** (Clave de suscripción) y compruebe que está usando la región correcta en **Subscription Region** (Región de suscripción). Haga clic en la pestaña **Read Handwritten Text** (Leer texto manuscrito), escriba una dirección URL a una imagen de texto manuscrito y después haga clic en el botón **Read Image** (Leer imagen) para analizar una imagen y ver el resultado.

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha utilizado el resto de Computer Vision API con Java para probar muchas de las características de análisis de imágenes disponibles. A continuación, consulte la documentación de referencia para obtener que más información acerca de las API implicados.

- [Computer Vision API de REST](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)
