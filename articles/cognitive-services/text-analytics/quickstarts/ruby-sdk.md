---
title: 'Inicio rápido: Llamada a Text Analytics de Cognitive Service con el SDK de Ruby'
titleSuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Text Analytics API en Azure Cognitive Services.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 05/08/2019
ms.author: tasharm
ms.openlocfilehash: 688887826fa803b616ca737bc8558aa17ed80e37
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66297768"
---
# <a name="quickstart-call-the-text-analytics-service-using-the-ruby-sdk"></a>Inicio rápido: Llamada al servicio Text Analytics con el SDK de Ruby

<a name="HOLTop"></a>


Use esta guía de inicio rápido para empezar a analizar el lenguaje con el SDK de Text Analytics para Ruby. Aunque la API de REST [Text Analytics](//go.microsoft.com/fwlink/?LinkID=759711) es compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-ruby-sdk-samples/blob/master/samples/text_analytics.rb).

Consulte las [definiciones de API](//go.microsoft.com/fwlink/?LinkID=759346) para obtener la documentación técnica de las API.

## <a name="prerequisites"></a>Requisitos previos

* [Ruby 2.5.5 o posterior](https://www.ruby-lang.org/)
* El [SDK para Ruby](https://rubygems.org/gems/azure_cognitiveservices_textanalytics) de Text Analytics
 
[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

También debe tener la [clave de acceso y punto de conexión](../How-tos/text-analytics-how-to-access-key.md) que se generó automáticamente durante el registro. 

<a name="RubyProject"></a>

## <a name="create-a-ruby-project-and-install-the-sdk"></a>Creación de un proyecto de Ruby e instalación del SDK

1. Cree un proyecto de Ruby y agregue un archivo nuevo denominado `Gemfile`.
2. Para agregar el SDK de Text Analytics al proyecto, agregue el código siguiente a `Gemfile`.

    ```ruby
    source 'https://rubygems.org'
    gem 'azure_cognitiveservices_textanalytics', '~>0.17.3'
    ```

## <a name="create-a-text-analytics-client"></a>Creación de un cliente de Text Analytics

1. Cree un archivo denominado `TextAnalyticsExamples.rb` en el editor que prefiera o en IDE. Importe el SDK de Text Analytics.

2. El cliente de Text Analytics usará un objeto de credenciales. Para crearlo, use `CognitiveServicesCredentials.new()` y pase la clave de suscripción.

3. Cree el cliente con el punto de conexión de Text Analytics correcto.

    ```ruby
    require 'azure_cognitiveservices_textanalytics'
    
    include Azure::CognitiveServices::TextAnalytics::V2_1::Models
    
    credentials =
        MsRestAzure::CognitiveServicesCredentials.new("enter key here")
    # Replace 'westus' with the correct region for your Text Analytics subscription
    endpoint = String.new("https://westus.api.cognitive.microsoft.com/")
    
    textAnalyticsClient =
        Azure::TextAnalytics::Profiles::Latest::Client.new({
            credentials: credentials
        })
    textAnalyticsClient.endpoint = endpoint
    ```

<a name="SentimentAnalysis"></a>

## <a name="sentiment-analysis"></a>análisis de opiniones

Mediante el SDK de Text Analytics o Text Analytics API, puede realizar el análisis de sentimiento en un conjunto de registros de texto. En el ejemplo siguiente se muestran las puntuaciones de opinión para varios documentos.

1. Cree una función denominada `SentimentAnalysisExample()` que toma como parámetro el cliente de Text Analytics que se creó anteriormente.

2. Defina un conjunto de objetos `MultiLanguageInput` que se debe analizar. Agregue un idioma y texto para cada objeto. El identificador puede ser cualquier valor.

    ```ruby
    def SentimentAnalysisExample(client)
      # The documents to be analyzed. Add the language of the document. The ID can be any value.
      input_1 = MultiLanguageInput.new
      input_1.id = '1'
      input_1.language = 'en'
      input_1.text = 'I had the best day of my life.'
    
      input_2 = MultiLanguageInput.new
      input_2.id = '2'
      input_2.language = 'en'
      input_2.text = 'This was a waste of my time. The speaker put me to sleep.'
    
      input_3 = MultiLanguageInput.new
      input_3.id = '3'
      input_3.language = 'es'
      input_3.text = 'No tengo dinero ni nada que dar...'
    
      input_4 = MultiLanguageInput.new
      input_4.id = '4'
      input_4.language = 'it'
      input_4.text = "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."
    ```

3. Dentro de la misma función, combine los documentos en una lista. Agréguela al campo `documents` de un objeto `MultiLanguageBatchInput`. 

4. Llame a la función `sentiment()` del cliente con el objeto `MultiLanguageBatchInput` como parámetro para enviar los documentos. Imprima los resultados que se generen.
    ```ruby
      input_documents =  MultiLanguageBatchInput.new
      input_documents.documents = [input_1, input_2, input_3, input_4]
    
      result = client.sentiment(
          multi_language_batch_input: input_documents
      )
      
      if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
        puts '===== SENTIMENT ANALYSIS ====='
        result.documents.each do |document|
          puts "Document Id: #{document.id}: Sentiment Score: #{document.score}"
        end
      end
    end
    ```

5. Llame a la función `SentimentAnalysisExample()`.

    ```ruby
    SentimentAnalysisExample(textAnalyticsClient)
    ```

### <a name="output"></a>Salida

```console
===== SENTIMENT ANALYSIS =====
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

<a name="LanguageDetection"></a>

## <a name="language-detection"></a>Detección de idiomas

El servicio Text Analytics puede detectar el idioma de un documento de texto con respecto a un gran número de idiomas y configuraciones regionales. En el ejemplo siguiente se muestra el idioma en l que se escribieron varios documentos.

1. Cree una función denominada `DetectLanguageExample()` que toma como parámetro de Text Analytics que se creó anteriormente. 

2. Defina un conjunto de objetos `LanguageInput` que se debe analizar. Agregue un idioma y texto para cada objeto. El identificador puede ser cualquier valor.

    ```ruby
    def DetectLanguageExample(client)
       # The documents to be analyzed.
       language_input_1 = LanguageInput.new
       language_input_1.id = '1'
       language_input_1.text = 'This is a document written in English.'
    
       language_input_2 = LanguageInput.new
       language_input_2.id = '2'
       language_input_2.text = 'Este es un document escrito en Español..'
    
       language_input_3 = LanguageInput.new
       language_input_3.id = '3'
       language_input_3.text = '这是一个用中文写的文件'
    ```

3. Dentro de la misma función, combine los documentos en una lista. Agréguela al campo `documents` de un objeto `LanguageBatchInput`. 

4. Llame a la función `detect_language()` del cliente con el objeto `LanguageBatchInput` como parámetro para enviar los documentos. Imprima los resultados que se generen.
    ```ruby
       input_documents = LanguageBatchInput.new
       input_documents.documents = [language_input_1, language_input_2, language_input_3]
    
    
       result = client.detect_language(
           language_batch_input: input_documents
       )
    
       if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
         puts '===== LANGUAGE DETECTION ====='
         result.documents.each do |document|
           puts "Document ID: #{document.id} , Language: #{document.detected_languages[0].name}"
         end
       else
         puts 'No results data..'
       end
     end
    ```

5. Llame a la función `DetectLanguageExample`.

    ```ruby
    DetectLanguageExample(textAnalyticsClient)
    ```

### <a name="output"></a>Salida

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

<a name="EntityRecognition"></a>

## <a name="entity-recognition"></a>Reconocimiento de entidades

El servicio Text Analytics puede distinguir y extraer las distintas entidades (personas, lugares y objetos) en los documentos de texto. En el ejemplo siguiente se muestran las entidades que se encuentran en varios documentos de ejemplo.

1. Cree una función denominada `Recognize_Entities()` que toma como parámetro el cliente de Text Analytics que se creó anteriormente.

2. Defina un conjunto de objetos `MultiLanguageInput` que se debe analizar. Agregue un idioma y texto para cada objeto. El identificador puede ser cualquier valor.

    ```ruby
      def RecognizeEntitiesExample(client)
        # The documents to be analyzed.
        input_1 = MultiLanguageInput.new
        input_1.id = '1'
        input_1.language = 'en'
        input_1.text = 'Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.'
    
        input_2 = MultiLanguageInput.new
        input_2.id = '2'
        input_2.language = 'es'
        input_2.text = 'La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.'
    ```

3. Dentro de la misma función, combine los documentos en una lista. Agréguela al campo `documents` de un objeto `MultiLanguageBatchInput`. 

4. Llame a la función `entities()` del cliente con el objeto `MultiLanguageBatchInput` como parámetro para enviar los documentos. Imprima los resultados que se generen.

    ```ruby
        input_documents =  MultiLanguageBatchInput.new
        input_documents.documents = [input_1, input_2]
    
        result = client.entities(
            multi_language_batch_input: input_documents
        )
    
        if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
          puts '===== ENTITY RECOGNITION ====='
          result.documents.each do |document|
            puts "Document ID: #{document.id}"
              document.entities.each do |entity|
                puts "\tName: #{entity.name}, \tType: #{entity.type == nil ? "N/A": entity.type},\tSub-Type: #{entity.sub_type == nil ? "N/A": entity.sub_type}"
                entity.matches.each do |match|
                  puts "\tOffset: #{match.offset}, \Length: #{match.length},\tScore: #{match.entity_type_score}"
                end
                puts
              end
          end
        else
          puts 'No results data..'
        end
      end
    ```

5. Llame a la función `RecognizeEntitiesExample`.
    ```ruby
    RecognizeEntitiesExample(textAnalyticsClient)
    ```

### <a name="output"></a>Salida

```console
===== ENTITY RECOGNITION =====
Document ID: 1
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0, Length: 9,   Score: 1.0

        Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25, Length: 10, Score: 0.999847412109375

        Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40, Length: 10, Score: 0.9988409876823425

        Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54, Length: 7,  Score: 0.8

        Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54, Length: 13, Score: 0.8

        Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89, Length: 5,  Score: 0.8

        Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116, Length: 11,        Score: 0.8

Document ID: 2
        Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 21, Length: 9,  Score: 0.999755859375

        Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
        Offset: 60, Length: 7,  Score: 0.9911284446716309

        Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
        Offset: 71, Length: 13, Score: 0.8

        Name: Seattle,  Type: Location, Sub-Type: N/A
        Offset: 88, Length: 7,  Score: 0.9998779296875
```

<a name="KeyPhraseExtraction"></a>

## <a name="key-phrase-extraction"></a>Extracción de la frase clave

El servicio Text Analytics puede extraer las frases clave de las oraciones. En el ejemplo siguiente se muestran las entidades que se encuentran en varios documentos de ejemplo en varios idiomas.

1. Cree una función denominada `KeyPhraseExtractionExample()` que toma como parámetro el cliente de Text Analytics que se creó anteriormente.

2. Defina un conjunto de objetos `MultiLanguageInput` que se debe analizar. Agregue un idioma y texto para cada objeto. El identificador puede ser cualquier valor.

    ```ruby
    def KeyPhraseExtractionExample(client)
      # The documents to be analyzed.
      input_1 = MultiLanguageInput.new
      input_1.id = '1'
      input_1.language = 'ja'
      input_1.text = '猫は幸せ'
  
      input_2 = MultiLanguageInput.new
      input_2.id = '2'
      input_2.language = 'de'
      input_2.text = 'Fahrt nach Stuttgart und dann zum Hotel zu Fu.'
  
      input_3 = MultiLanguageInput.new
      input_3.id = '3'
      input_3.language = 'en'
      input_3.text = 'My cat is stiff as a rock.'
  
      input_4 = MultiLanguageInput.new
      input_4.id = '4'
      input_4.language = 'es'
      input_4.text = 'A mi me encanta el fútbol!'
      ```

3. Dentro de la misma función, combine los documentos en una lista. Agréguela al campo `documents` de un objeto `MultiLanguageBatchInput`. 

4. Llame a la función `key_phrases()` del cliente con el objeto `MultiLanguageBatchInput` como parámetro para enviar los documentos. Imprima los resultados que se generen.

    ```ruby
      input_documents =  MultiLanguageBatchInput.new
      input_documents.documents = [input_1, input_2, input_3, input_4]
    
      result = client.key_phrases(
          multi_language_batch_input: input_documents
      )
    
      if (!result.nil? && !result.documents.nil? && result.documents.length > 0)
        result.documents.each do |document|
          puts "Document Id: #{document.id}"
          puts '  Key Phrases'
          document.key_phrases.each do |key_phrase|
            puts "    #{key_phrase}"
          end
        end
      else
        puts 'No results data..'
      end
    end
    ```

5. Llame a la función `KeyPhraseExtractionExample`.

    ```ruby
    KeyPhraseExtractionExample(textAnalyticsClient)
    ```

### <a name="output"></a>Salida

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                rock
Document ID: 4
         Key phrases:
                fútbol
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Text Analytics con Power BI](../tutorials/tutorial-power-bi-key-phrases.md)

## <a name="see-also"></a>Otras referencias

 [Información general de Text Analytics](../overview.md)  
 [Preguntas más frecuentes](../text-analytics-resource-faq.md)
