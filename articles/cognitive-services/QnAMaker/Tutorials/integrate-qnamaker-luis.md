---
title: 'Integración de QnA Maker y LUIS: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Un tutorial detallado sobre cómo integrar QnA Maker y LUIS
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: 0a0eeb3815b793ed81f60b2b239bc459e5574788
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381831"
---
# <a name="integrate-qna-maker-and-luis-to-distribute-your-knowledge-base"></a>Integración de QnA Maker y LUIS para distribuir la base de conocimiento
A medida que el tamaño de la base de conocimiento de QnA Maker aumenta, resulta complicado mantenerla como un único conjunto monolítico y es necesario dividir la base de conocimiento en fragmentos lógicos más pequeños.

Aunque es fácil crear varias bases de conocimiento en QnA Maker, necesitará alguna lógica para enrutar la pregunta entrante a la base de conocimiento apropiada. Puede hacerlo si utiliza LUIS.

## <a name="architecture"></a>Architecture

![Arquitectura de LUIS en QnA Maker](../media/qnamaker-tutorials-qna-luis/qnamaker-luis-architecture.PNG)

En el escenario anterior, QnA Maker primero obtiene la intención de la pregunta entrante a partir de un modelo LUIS y luego la usa para enrutarla a la base de conocimiento correcta de QnA Maker.

## <a name="prerequisites"></a>requisitos previos
- Inicie sesión en el portal de [LUIS](https://www.luis.ai/) y [cree una aplicación](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/create-new-app).
- [Agregue intenciones](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/add-intents) según su escenario.
- [Entrene](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-how-to-train) y [publique](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/publishapp) su aplicación LUIS.
- Inicie sesión en [QnA Maker](https://qnamaker.ai) y [cree bases de conocimiento]() en función de su escenario.
- [Pruebe]() y [publique]() las bases de conocimiento.

## <a name="qna-maker--luis-bot"></a>Bot QnA Maker + LUIS
1. Primero cree un bot de aplicación web con la plantilla de LUIS, vincúlelo con la aplicación LUIS creada anteriormente y modifique las intenciones. Consulte los pasos detallados [aquí](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/luis-csharp-tutorial-build-bot-framework-sample).

2. Agregue dependencias a la parte superior del archivo, con las demás dependencias:

    ```
    using RestSharp;
    using System.Collections.Generic;
    using Newtonsoft.Json;
    ```
3. Agregue la clase siguiente para llamar al servicio QnA Maker:

    ```
        /// <summary>
        /// QnAMakerService is a wrapper over the QnA Maker REST endpoint
        /// </summary>
        [Serializable]
        public class QnAMakerService
        {
            private string qnaServiceHostName;
            private string knowledgeBaseId;
            private string endpointKey;
    
            /// <summary>
            /// Initialize a particular endpoint with it's details
            /// </summary>
            /// <param name="hostName">Hostname of the endpoint</param>
            /// <param name="kbId">Knowledge base ID</param>
            /// <param name="ek">Endpoint Key</param>
            public QnAMakerService(string hostName, string kbId, string ek)
            {
                qnaServiceHostName = hostName;
                knowledgeBaseId = kbId;
                endpointKey = ek;
            }
    
            /// <summary>
            /// Call the QnA Maker endpoint and get a response
            /// </summary>
            /// <param name="query">User question</param>
            /// <returns></returns>
            public string GetAnswer(string query)
            {
                var client = new RestClient( qnaServiceHostName + "/qnamaker/knowledgebases/" + knowledgeBaseId + "/generateAnswer");
                var request = new RestRequest(Method.POST);
                request.AddHeader("authorization", "EndpointKey " + endpointKey);
                request.AddHeader("content-type", "application/json");
                request.AddParameter("application/json", "{\"question\": \"" + query + "\"}", ParameterType.RequestBody);
                IRestResponse response = client.Execute(request);
    
                // Deserialize the response JSON
                QnAAnswer answer = JsonConvert.DeserializeObject<QnAAnswer>(response.Content);
    
                // Return the answer if present
                if (answer.answers.Count > 0)
                    return answer.answers[0].answer;
                else
                    return "No good match found.";
            }
        }
    
        /* START - QnA Maker Response Class */
        public class Metadata
        {
            public string name { get; set; }
            public string value { get; set; }
        }
    
        public class Answer
        {
            public IList<string> questions { get; set; }
            public string answer { get; set; }
            public double score { get; set; }
            public int id { get; set; }
            public string source { get; set; }
            public IList<object> keywords { get; set; }
            public IList<Metadata> metadata { get; set; }
        }
    
        public class QnAAnswer
        {
            public IList<Answer> answers { get; set; }
        }
        /* END - QnA Maker Response Class */
    ```

3. Vaya a https://qnamaker.ai -> Mis bases de conocimiento -> Ver código de la base de conocimiento correspondiente. Obtenga la siguiente información:

    ![Solicitud HTTP de QnA Maker](../media/qnamaker-tutorials-qna-luis/qnamaker-http-request.png)

4. Cree una instancia de la clase QnAMakerService para cada base de conocimiento:
    ```
            // Instantiate the knowledge bases
            public QnAMakerService hrQnAService = new QnAMakerService("https://hrkb.azurewebsites.net", "<HR knowledge base id>", "<HR endpoint key>");
            public QnAMakerService payrollQnAService = new QnAMakerService("https://payrollkb.azurewebsites.net", "<Payroll knowledge base id>", "<Payroll endpoint key>");
            public QnAMakerService financeQnAService = new QnAMakerService("https://financekb.azurewebsites.net", "<Finance knowledge base id>", "<Finance endpoint key>");
    ```

5. Llame a la base de conocimiento correspondiente para la intención.
    ```
            // HR Intent
            [LuisIntent("HR")]
            public async Task CancelIntent(IDialogContext context, LuisResult result)
            {
                // Ask the HR knowledge base
                await context.PostAsync(hrQnAService.GetAnswer(result.Query));
            }
    
            // Payroll intent
            [LuisIntent("Payroll")]
            public async Task GreetingIntent(IDialogContext context, LuisResult result)
            {
                // Ask the payroll knowledge base
                await context.PostAsync(payrollQnAService.GetAnswer(result.Query));
            }
    
            // Finance intent
            [LuisIntent("Finance")]
            public async Task HelpIntent(IDialogContext context, LuisResult result)
            {
                // Ask the finance knowledge base
                await context.PostAsync(financeQnAService.GetAnswer(result.Query));
            }
    ```

## <a name="build-the-bot"></a>Compilación del bot
1. En el editor de código, haga clic con el botón derecho en `build.cmd` y seleccione **Run from Console** (Ejecutar desde la consola).

    ![Ejecutar desde la consola](../media/qnamaker-tutorials-qna-luis/run-from-console.png)

2. La vista de código se reemplaza con una ventana de terminal en la que se muestran el progreso y los resultados de la compilación.

    ![compilación de consola](../media/qnamaker-tutorials-qna-luis/console-build.png)

## <a name="test-the-bot"></a>Probar el bot
En Azure Portal, haga clic en **Test in Web Chat** (Probar en Chat en web) para probar el bot. Escriba mensajes a partir de intenciones diferentes para obtener la respuesta de la base de conocimiento correspondiente.

![Probar en Chat en web](../media/qnamaker-tutorials-qna-luis/qnamaker-web-chat.png)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Crear un plan de continuidad del negocio para QnA Maker](../How-To/business-continuity-plan.md)
