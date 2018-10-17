---
title: 'Guía de inicio rápido: SDK de Bing Entity Search con C#'
titleSuffix: Azure Cognitive Services
description: Configuración de la aplicación de consola del SDK de Entity Search con C#.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: quickstart
ms.date: 01/30/2018
ms.author: v-gedod
ms.openlocfilehash: abf7e4344bea88fd86b86135ffab30672d77922e
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814113"
---
# <a name="quickstart-bing-entity-search-sdk-with-c"></a>Guía de inicio rápido: SDK de Bing Entity Search con C#

Bing Entity Search API contiene la funcionalidad de la API REST para consultas de entidad y análisis de resultados.

El [código fuente de los ejemplos del SDK de Bing Entity Search para C#](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingEntitySearch) está disponible en GitHub.

## <a name="application-dependencies"></a>Dependencias de aplicaciones

Para configurar una aplicación de consola con el SDK de Bing Entity Search, vaya a la opción `Manage NuGet Packages` del Explorador de soluciones en Visual Studio.  Agregue el paquete `Microsoft.Azure.CognitiveServices.Search.EntitySearch`.

Al instalar el [paquete de NuGet Entity Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.EntitySearch/1.2.0), también se instalarán dependencias, incluidos los ensamblados siguientes:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="entity-search-client"></a>Cliente de Entity Search
Para crear una instancia del cliente `EntitySearchAPI`, agregue directivas Using:
```
using Microsoft.Azure.CognitiveServices.Search.EntitySearch;
using Microsoft.Azure.CognitiveServices.Search.EntitySearch.Models;

```
Después, cree una instancia del cliente:
```
var client = new EntitySearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
Utilice al cliente para buscar con un texto de consulta:
```
var entityData = client.Entities.Search(query: "Satya Nadella");

```
Analice los resultados de la consulta anterior:
```
if (entityData?.Entities?.Value?.Count > 0)
{
    // find the entity that represents the dominant one
    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();

    if (mainEntity != null)
    {
        Console.WriteLine("Searched for \"Satya Nadella\" and found a dominant entity with this description:");
        Console.WriteLine(mainEntity.Description);
    }
    else
    {
        Console.WriteLine("Couldn't find main entity Satya Nadella!");
    }
}
else
{
    Console.WriteLine("Didn't see any data..");
}

```

## <a name="complete-console-application"></a>Aplicación completa de consola
La siguiente aplicación de consola busca una entidad única en la consulta "Satya Nadella" e imprime una breve descripción.
```
using System;
using System.Linq;
using System.Text;
using Microsoft.Azure.CognitiveServices.Search.EntitySearch;
using Microsoft.Azure.CognitiveServices.Search.EntitySearch.Models;
using Newtonsoft.Json;

namespace EntitySrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new EntitySearchAPI(new ApiKeyServiceClientCredentials("19aa718a79d6444daaa415981d9f54ad"));

            DominantEntityLookup(client);
            
            // Include the following methods to use queries defined under the headings following this example.
            //HandlingDisambiguation(client);
            //RestaurantLookup(client);
            //MultipleRestaurantLookup(client);
            //Error(client);

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

        public static void DominantEntityLookup(EntitySearchAPI client)
        {
            try
            {
                var entityData = client.Entities.Search(query: "Satya Nadella");

                if (entityData?.Entities?.Value?.Count > 0)
                {
                    // find the entity that represents the dominant one
                    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();

                    if (mainEntity != null)
                    {
                        Console.WriteLine("Searched for \"Satya Nadella\" and found a dominant entity with this description:");
                        Console.WriteLine(mainEntity.Description);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find main entity Satya Nadella!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any data..");
                }
            }
            catch (ErrorResponseException ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
}

```

## <a name="ambiguous-results"></a>Resultados ambiguos
El código siguiente controla la desambiguación de los resultados de una consulta ambigua "William Gates".
```
       public static void HandlingDisambiguation(EntitySearchAPI client)
        {
            try
            {
                var entityData = client.Entities.Search(query: "william gates");

                if (entityData?.Entities?.Value?.Count > 0)
                {
                    // find the entity that represents the dominant one
                    var mainEntity = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DominantEntity).FirstOrDefault();
                    var disambigEntities = entityData.Entities.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.DisambiguationItem).ToList();

                    if (mainEntity != null)
                    {
                        Console.WriteLine("Searched for \"William Gates\" and found a dominant entity with type hint \"{0}\" with this description:", mainEntity.EntityPresentationInfo.EntityTypeDisplayHint);
                        Console.WriteLine(mainEntity.Description);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find a reliable dominant entity for William Gates!");
                    }

                    if (disambigEntities?.Count > 0)
                    {
                        Console.WriteLine();
                        Console.WriteLine("This query is pretty ambiguous and can be referring to multiple things. Did you mean one of these: ");

                        var sb = new StringBuilder();

                        foreach (var disambig in disambigEntities)
                        {
                            sb.AppendFormat(", or {0} the {1}", disambig.Name, disambig.EntityPresentationInfo.EntityTypeDisplayHint);
                        }

                        Console.WriteLine(sb.ToString().Substring(5) + "?");
                    }
                    else
                    {
                        Console.WriteLine("We didn't find any disambiguation items for William Gates, so we must be certain what you're talking about!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any data..");
                }
            }
            catch (ErrorResponseException ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```

## <a name="entitydata-places"></a>Lugares de EntityData
El código siguiente busca un almacén único de "Microsoft Store" e imprime su número de teléfono.
```
        public static void StoreLookup(EntitySearchAPI client)
        {
            try
            {
                var entityData = client.Entities.Search(query: "microsoft store");

                if (entityData?.Places?.Value?.Count > 0)
                {
                    // Some local entities will be places, others won't be. Depending on the data you want, try to cast to the appropriate schema.
                    // In this case, the item being returned is technically a store, but the Place schema has the data we want (telephone)
                    var store = entityData.Places.Value.FirstOrDefault() as Place;

                    if (store != null)
                    {
                        Console.WriteLine("Searched for \"Microsoft Store\" and found a store with this phone number:");
                        Console.WriteLine(store.Telephone);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find a place!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any data..");
                }
            }
            catch (ErrorResponseException ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
## <a name="entityscenario-list"></a>Lista de EntityScenario
El código siguiente busca una lista de los "restaurantes de Seattle" e imprime sus nombres y números de teléfono.
```
       public static void MultipleRestaurantLookup(EntitySearchAPI client)
        {
            try
            {
                var restaurants = client.Entities.Search(query: "seattle restaurants");

                if (restaurants?.Places?.Value?.Count > 0)
                {
                    // get all the list items that relate to this query
                    var listItems = restaurants.Places.Value.Where(thing => thing.EntityPresentationInfo.EntityScenario == EntityScenario.ListItem).ToList();

                    if (listItems?.Count > 0)
                    {
                        var sb = new StringBuilder();

                        foreach (var item in listItems)
                        {
                            var place = item as Place;

                            if (place == null)
                            {
                                Console.WriteLine("Unexpectedly found something that isn't a place named \"{0}\"", item.Name);
                                continue;
                            }

                            sb.AppendFormat(",{0} ({1}) ", place.Name, place.Telephone);
                        }

                        Console.WriteLine("Ok, we found these places: ");
                        Console.WriteLine(sb.ToString().Substring(1));
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find any relevant results for \"seattle restaurants\"");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any data..");
                }
            }
            catch (ErrorResponseException ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
## <a name="error-results"></a>Resultados del error
El código siguiente desencadena una solicitud incorrecta y muestra cómo leer la respuesta del error.
```
        public static void Error(EntitySearchAPI client)
        {
            try
            {
                var entityData = client.Entities.Search(query: "satya nadella", market: "no-ty");
            }
            catch (ErrorResponseException ex)
            {
                // The status code of the error should be a good indication of what occurred. However, if you'd like more details, you can dig into the response.
                // Please note that depending on the type of error, the response schema might be different, so you aren't guaranteed a specific error response schema.
                Console.WriteLine("Exception occurred, status code {0} with reason {1}.", ex.Response.StatusCode, ex.Response.ReasonPhrase);

                // if you'd like more descriptive information (if available), attempt to parse the content as an ErrorResponse
                var issue = JsonConvert.DeserializeObject<ErrorResponse>(ex.Response.Content);

                if (issue != null && issue.Errors?.Count > 0)
                {
                    if (issue.Errors[0].SubCode == ErrorSubCode.ParameterInvalidValue)
                    {
                        Console.WriteLine("Turns out the issue is parameter \"{0}\" has an invalid value \"{1}\". Detailed message is \"{2}\"", issue.Errors[0].Parameter, issue.Errors[0].Value, issue.Errors[0].Message);
                    }
                }
            }
        }

```
## <a name="next-steps"></a>Pasos siguientes

[Ejemplos del SDK de Cognitive Services para .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)