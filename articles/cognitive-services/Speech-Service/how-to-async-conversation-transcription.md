---
title: 'Transcripción de conversaciones asincrónica (versión preliminar): servicio de voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar la transcripción de conversaciones asincrónica con el servicio de voz. Disponible solo para Java.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: amishu
ms.openlocfilehash: 433ea2778d99de39991565b2529e2f8eab4e13df
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506495"
---
# <a name="asynchronous-conversation-transcription-preview"></a>Transcripción de conversaciones asincrónica (versión preliminar)

En este artículo, se muestra la transcripción de conversaciones asincrónica mediante la API **RemoteConversationTranscriptionClient**. Si ha configurado la transcripción de conversaciones para realizar la transcripción asincrónica y tiene un `conversationId`, puede obtener la transcripción asociada a ese `conversationId` mediante la API **RemoteConversationTranscriptionClient**.

## <a name="asynchronous-vs-real-time--asynchronous"></a>Asincrónica frente a la combinación de tiempo real y asincrónica

Con la transcripción asincrónica, se transmite el audio de conversación, pero no es necesario que se devuelva una transcripción en tiempo real. En su lugar, después de enviar el audio, use el `conversationId` de `ConversationTranscriber` para consultar el estado de la transcripción asincrónica. Cuando la transcripción asincrónica esté lista, obtendrá un `RemoteConversationTranscriptionResult`.

Con una combinación de transcripción en tiempo real y asincrónica, se obtiene la transcripción en tiempo real, pero también se obtiene la transcripción mediante consultas con el `conversationId` (similar a un escenario asincrónico).

Se requieren dos pasos para realizar la transcripción asincrónica. El primer paso consiste en cargar el audio y elegir solo transcripción asincrónica o en tiempo real y asincrónica. El segundo paso es obtener los resultados de la transcripción.

## <a name="upload-the-audio"></a>Carga del audio

Antes de que se pueda realizar la transcripción asincrónica, debe enviar el audio a la transcripción de conversación mediante el SDK del cliente de Microsoft Cognitive Speech (versión 1.8.0 o superior).

En este ejemplo de código se muestra cómo crear un transcriptor de conversaciones para el modo de solo asincrónico. Para transmitir audio al transcriptor, debe agregar código de streaming de audio derivado de la [transcripción de conversaciones en tiempo real con el SDK de Voz](how-to-use-conversation-transcription-service.md). Consulte la sección de **limitaciones** de ese tema para ver las API de lenguajes y plataformas admitidas.

```java
// Create the speech config object
// Substitute real information for "YourSubscriptionKey" and "Region"
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");
speech_config.setProperty("ConversationTranscriptionInRoomAndOnline", "true");

// Set the property for asynchronous transcription
speechConfig.setServiceProperty("transcriptionMode", "Async", ServicePropertyChannel.UriQueryParameter);

// Set the property for real-time plus asynchronous transcription
//speechConfig.setServiceProperty("transcriptionMode", "RealTimeAndAsync", ServicePropertyChannel.UriQueryParameter);

// Do rest of the things as explained in how to use Conversation Transcription

// pick a conversation Id that is a GUID.
Conversation conversation = new Conversation(speechConfig, conversationId);

// Create a conversation transcriber
ConversationTranscriber transcriber = new ConversationTranscriber(AudioConfig.fromDefaultMicrophoneInput());

// join a conversation
transcriber.joinConversationAsync(conversation).get();

// stream audio as shown in “Transcribe conversations in real time”
...
```

Si quiere que sea en tiempo real _y_ asincrónica, agregue y quite la marca de comentario de las líneas de código adecuadas como se indica a continuación:

```java
// Set the property for asynchronous transcription
//speechConfig.setServiceProperty("transcriptionMode", "Async", ServicePropertyChannel.UriQueryParameter);

// Set the property for real-time plus asynchronous transcription
speechConfig.setServiceProperty("transcriptionMode", "RealTimeAndAsync", ServicePropertyChannel.UriQueryParameter);
```

## <a name="get-transcription-results"></a>Obtención de los resultados de la transcripción

Este paso obtiene los resultados de la transcripción asincrónica pero supone que el procesamiento en tiempo real que haya solicitado se realiza en otro lugar. Para obtener más información, consulte [transcripción de conversaciones en tiempo real con el SDK de Voz](how-to-use-conversation-transcription-service.md).

Para el código que se muestra aquí, necesita **remoteconversation-client-sdk version 1.0.0**, compatible solo con Java (1.8 o superior) en Windows, Linux y Android (nivel de API 26 o superior).

### <a name="obtaining-the-client-sdk"></a>Obtención del SDK de cliente

Puede obtener **remoteconversation-client-sdk** si edita el archivo pom.xml como se indica a continuación.

- Al final del archivo, antes de la etiqueta de cierre `</project>`, cree un elemento `repositories` con una referencia al repositorio de Maven para el SDK de Voz:

  ```xml
  <repositories>
    <repository>
      <id>maven-cognitiveservices-speech</id>
      <name>Microsoft Cognitive Services Speech Maven Repository</name>
      <url>https://csspeechstorage.blob.core.windows.net/maven/</url>
    </repository>
  </repositories>
  ```

- Agregue también un elemento `dependencies`, con remoteconversation-client-sdk 1.0.0 como dependencia:

  ```xml
  <dependencies>
    <dependency>
      <groupId>com.microsoft.cognitiveservices.speech.remoteconversation</groupId>
      <artifactId>remoteconversation-client-sdk</artifactId>
      <version>1.0.0</version>
    </dependency>
  </dependencies>
  ```

- Guarde los cambios.

### <a name="sample-transcription-code"></a>Código de transcripción de ejemplo

Una vez que tenga el `conversationId`, cree un objeto de operación remoto **RemoteConversationTranscriptionOperation** en el cliente para consultar el estado de la transcripción asincrónica. **RemoteConversationTranscriptionOperation** se extiende desde [Poller](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/core/azure-core/src/main/java/com/azure/core/util/polling/Poller.java). Una vez haya finalizado el sondeador, obtenga **RemoteConversationTranscriptionResult**. Para ello, suscríbase al sondeador y consulte el objeto. En este código, simplemente se imprime el contenido de los resultados en la salida del sistema.

```java
// Create the speech config object
SpeechConfig speechConfig = SpeechConfig.fromSubscription("YourSubscriptionKey", "Region");

// Create a remote Conversation Transcription client
RemoteConversationTranscriptionClient client = new RemoteConversationTranscriptionClient(speechConfig);

// Create a remote Conversation Transcription operation
RemoteConversationTranscriptionOperation operation = new RemoteConversationTranscriptionOperation(conversationId, client);

// Operation identifier now be the value of `conversationId`
System.out.println("Operation Identifier:" + operation.getId());

// Get the observer (which is a Flux) and subscribe to it for the response
operation.getObserver()
                .subscribe(
                        pollResponse -> {
                            System.out.println("Poll response status : " + pollResponse.getStatus());
                            if(pollResponse.getValue().getConversationTranscriptionResults() != null) {
                                for (int i = 0; i < pollResponse.getValue().getConversationTranscriptionResults().size(); i++) {
                                    ConversationTranscriptionResult result = pollResponse.getValue().getConversationTranscriptionResults().get(i);
                                    System.out.println(result.getOffset());
                                    System.out.println(result.getDuration());
                                    System.out.println(result.getUserId());
                                    System.out.println(result.getReason());
                                    System.out.println(result.getResultId());
                                    System.out.println(result.getText());
                                    System.out.println(result.toString());
                                }
                            }
                        }
                );
// Block on the operation till it is finished
operation.block();

System.out.println("Operation finished");
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos en GitHub](https://aka.ms/csspeech/samples)
