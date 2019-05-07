---
title: Bucle de comentarios (Personalizer)
titleSuffix: Azure Cognitive Services
description: Personalice el contenido en este inicio rápido de C# con el servicio Personalizer.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: c566d1fd4b151efc0d28b7059504e60a1451c034
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025513"
---
# <a name="quickstart-personalize-content-using-c"></a>Inicio rápido: Personalización de contenido mediante C# 

Muestre contenido personalizado en este inicio rápido de C# con el servicio Personalizer.

En este ejemplo se muestra cómo usar la biblioteca cliente de Personalization para C# para realizar las siguientes acciones: 

 * Clasificar una lista de acciones para su personalización.
 * Informe de la recompensa que se asignará a las principales acciones en función de la selección del usuario para el evento especificado.

Empezar a usar Personalization implica los siguientes pasos:

1. Hacer referencia al SDK 
1. Escribir código para clasificar las acciones que desea mostrar a los usuarios.
1. Escribir código para enviar recompensas para entrenar el bucle.

## <a name="prerequisites"></a>Requisitos previos

* Necesita una clave de suscripción y un token que generen la dirección URL del servicio.
* [Visual Studio 2015 o 2017](https://visualstudio.microsoft.com/downloads/).
* El paquete NuGet del SDK Microsoft.Azure.CognitiveServices.Personalization. A continuación, se proporcionan instrucciones de instalación.

## <a name="creating-a-new-console-app-and-referencing-the-personalizer-sdk"></a>Creación de una aplicación de consola y referencia al SDK de Personalizer 

<!--
Get the latest code as a Visual Studio solution from [GitHub] (add link).
-->

1. Cree una aplicación de consola de Visual C# en Visual Studio.
1. Instale el paquete NuGet de la biblioteca cliente de Personalization. En el menú, seleccione **Herramientas**, seleccione **Administrador de paquetes NuGet** y, luego, **Administrar paquetes NuGet para la solución**.
1. Seleccione la pestaña **Examinar** y, en el cuadro **Buscar**, escriba `Microsoft.Azure.CognitiveServices.Personalization`.
1. Seleccione **Microsoft.Azure.CognitiveServices.Personalization** cuando se muestre.
1. Seleccione la casilla que hay junto al nombre del proyecto y, después, seleccione **Instalar**.

## <a name="add-the-code-and-put-in-your-personalizer-and-azure-keys"></a>Adición del código y escritura de las claves de Personalizer y Azure

1. Sustituya Program.cs por el código siguiente. 
1. Reemplace el valor de `serviceKey` por su clave de suscripción válida de Personalizer.
1. Reemplace `serviceEndpoint` por su punto de conexión del servicio. Un ejemplo es `https://westus2.api.cognitive.microsoft.com/`.
1. Ejecute el programa.

## <a name="add-code-to-rank-the-actions-you-want-to-show-to-your-users"></a>Adición de código para clasificar las acciones que desea mostrar a los usuarios

El siguiente código de C# es una lista completa para pasar información de usuario, características e información acerca del contenido, _acciones_, a Personalizer mediante el SDK. Personalizer devuelve la principal acción que se muestra al usuario.  

```csharp
using Microsoft.Azure.CognitiveServices.Personalization;
using Microsoft.Azure.CognitiveServices.Personalization.Models;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;

namespace PersonalizationExample
{
    class Program
    {
        // The key specific to your personalization service instance; e.g. "0123456789abcdef0123456789ABCDEF"
        private const string serviceKey = "";

        // The endpoint specific to your personalization service instance; e.g. https://westus2.api.cognitive.microsoft.com/
        private const string serviceEndpoint = "";

        static void Main(string[] args)
        {
            int iteration = 1;
            bool runLoop = true;

            Uri url = new Uri(serviceEndpoint);

            // Get the actions list to choose from personalization with their features.
            IList<RankableAction> actions = GetActions();

            // Initialize Personalization client.
            PersonalizationClient client = InitializePersonalizationClient(url);

            do
            {
                Console.WriteLine("\nIteration: " + iteration++);

                // Get context information from the user.
                string timeOfDayFeature = GetUsersTimeOfDay();
                string tasteFeature = GetUsersTastePreference();

                // Create current context from user specified data.
                IList<object> currentContext = new List<object>() {
                    new { time = timeOfDayFeature },
                    new { taste = tasteFeature }
                };

                // Exclude an action for personalization ranking. This action will be held at its current position.
                IList<string> excludeActions = new List<string> { "juice" };

                // Generate an ID to associate with the request.
                string eventId = Guid.NewGuid().ToString();

                // Rank the actions
                var request = new RankRequest(actions, currentContext, excludeActions, eventId);
                RankResponse response = client.Rank(request);

                Console.WriteLine("\nPersonalization service thinks you would like to have: " + response.RewardActionId + ". Is this correct? (y/n)");

                float reward = 0.0f;
                string answer = GetKey();

                if (answer == "Y")
                {
                    reward = 1;
                    Console.WriteLine("\nGreat! Enjoy your food.");
                }
                else if (answer == "N")
                {
                    reward = 0;
                    Console.WriteLine("\nYou didn't like the recommended food choice.");
                }
                else
                {
                    Console.WriteLine("\nEntered choice is invalid. Service assumes that you didn't like the recommended food choice.");
                }

                Console.WriteLine("\nPersonalization service ranked the actions with the probabilities as below:");
                foreach (var rankedResponse in response.Ranking)
                {
                    Console.WriteLine(rankedResponse.Id + " " + rankedResponse.Probability);
                }

                // Send the reward for the action based on user response.
                client.Reward(response.EventId, new RewardRequest(reward));

                Console.WriteLine("\nPress q to break, any other key to continue:");
                runLoop = !(GetKey() == "Q");

            } while (runLoop);
        }

        /// <summary>
        /// Initializes the personalization client.
        /// </summary>
        /// <param name="url">Azure endpoint</param>
        /// <returns>Personalization client instance</returns>
        static PersonalizationClient InitializePersonalizationClient(Uri url)
        {
            PersonalizationClient client = new PersonalizationClient(url,
            new ApiKeyServiceClientCredentials(serviceKey),
            new DelegatingHandler[] { });

            return client;
        }

        /// <summary>
        /// Get users time of the day context.
        /// </summary>
        /// <returns>Time of day feature selected by the user.</returns>
        static string GetUsersTimeOfDay()
        {
            string[] timeOfDayFeatures = new string[] { "morning", "afternoon", "evening", "night" };

            Console.WriteLine("\nWhat time of day is it (enter number)? 1. morning 2. afternoon 3. evening 4. night");
            if (!int.TryParse(GetKey(), out int timeIndex) || timeIndex < 1 || timeIndex > timeOfDayFeatures.Length)
            {
                Console.WriteLine("\nEntered value is invalid. Setting feature value to " + timeOfDayFeatures[0] + ".");
                timeIndex = 1;
            }

            return timeOfDayFeatures[timeIndex - 1];
        }

        /// <summary>
        /// Gets user food preference.
        /// </summary>
        /// <returns>Food taste feature selected by the user.</returns>
        static string GetUsersTastePreference()
        {
            string[] tasteFeatures = new string[] { "salty", "sweet" };

            Console.WriteLine("\nWhat type of food would you prefer (enter number)? 1. salty 2. sweet");
            if (!int.TryParse(GetKey(), out int tasteIndex) || tasteIndex < 1 || tasteIndex > tasteFeatures.Length)
            {
                Console.WriteLine("\nEntered value is invalid. Setting feature value to " + tasteFeatures[0] + ".");
                tasteIndex = 1;
            }

            return tasteFeatures[tasteIndex - 1];
        }

        /// <summary>
        /// Creates personalization actions feature list.
        /// </summary>
        /// <returns>List of actions for personalization.</returns>
        static IList<RankableAction> GetActions()
        {
            IList<RankableAction> actions = new List<RankableAction>
            {
                new RankableAction
                {
                    Id = "pasta",
                    Features =
                    new List<object>() { new { taste = "salty", spiceLevel = "medium" }, new { nutritionLevel = 5, cuisine = "italian" } }
                },

                new RankableAction
                {
                    Id = "ice cream",
                    Features =
                    new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionalLevel = 2 } }
                },

                new RankableAction
                {
                    Id = "juice",
                    Features =
                    new List<object>() { new { taste = "sweet", spiceLevel = "none" }, new { nutritionLevel = 5 }, new { drink = true } }
                },

                new RankableAction
                {
                    Id = "salad",
                    Features =
                    new List<object>() { new { taste = "salty", spiceLevel = "low" }, new { nutritionLevel = 8 } }
                }
            };

            return actions;
        }

        private static string GetKey()
        {
            return Console.ReadKey().Key.ToString().Last().ToString().ToUpper();
        }
    }
}
```

## <a name="run-the-program"></a>Ejecución del programa

Compile y ejecute el programa. El programa del inicio rápido realiza dos preguntas para recopilar las preferencias del usuario, conocidas como características, y, después, proporciona la acción principal.

![El programa del inicio rápido realiza dos preguntas para recopilar las preferencias del usuario, conocidas como características, y, después, proporciona la acción principal.](media/csharp-quickstart-commandline-feedback-loop/quickstart-program-feedback-loop-example.png)

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando haya terminado con la guía de inicio rápido, quite todos los archivos creados en ella. 

## <a name="next-steps"></a>Pasos siguientes

[Funcionamiento de Personalizer](how-personalizer-works.md)


