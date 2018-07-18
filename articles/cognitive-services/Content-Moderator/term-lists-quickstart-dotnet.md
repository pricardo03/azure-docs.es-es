---
title: Moderación con listas de términos personalizadas en Azure Content Moderator | Microsoft Docs
description: Cómo moderar con listas de términos personalizadas con Azure Content Moderator SDK para .NET.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/11/2018
ms.author: sajagtap
ms.openlocfilehash: 6da72ad070d9c3a6be38e24626dff77b52fed852
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380322"
---
# <a name="moderate-with-custom-term-lists-in-net"></a>Moderación con listas de términos personalizadas en .NET

La lista global predeterminada de términos de Azure Content Moderator es suficiente para la mayoría de las necesidades de moderación de contenido. Sin embargo, puede que deba filtrar términos específicos de la organización. Por ejemplo, es aconsejable etiquetar los nombres de la competencia para revisarlos en profundidad. 

Puede usar Content Moderator SDK para .NET para crear listas personalizadas de términos para emplearlas con Text Moderation API.

> [!NOTE]
> Hay un límite máximo de **5 listas de términos** y cada lista **no debe superar los 10 000 términos**.
>

En este artículo se proporciona información y ejemplos de código que le ayudarán a empezar a usar el SDK de Content Moderator para .NET con las siguientes finalidades:
- Crea una lista.
- Agregar términos a una lista.
- Filtrar términos con los términos de una lista.
- Eliminar términos de una lista.
- Eliminar una lista.
- Editar información de la lista.
- Actualizar el índice para que los cambios en la lista se incluyan en un nuevo examen.

En este artículo se da por supuesto que ya está familiarizado con Visual Studio y C#.

## <a name="sign-up-for-content-moderator-services"></a>Suscribirse a los servicios de Content Moderator

Antes de poder usar los servicios de Content Moderator mediante la API REST o el SDK, necesita una clave de suscripción.

En el panel de Content Moderator, verá la clave de suscripción en **Configuración** > **Credenciales** > **API** > **Trial Ocp-Apim-Subscription-Key**. Para más información, consulte [Información general](overview.md).

## <a name="create-your-visual-studio-project"></a>Crear un proyecto de Visual Studio

1. Agregue un nuevo proyecto de **Aplicación de consola (.NET Framework)** a la solución.

1. Asigne al proyecto el nombre **TermLists**. Seleccione este proyecto como proyecto de inicio único para la solución.

1. Agregue una referencia al conjunto de proyectos **ModeratorHelper** que creó en [Content Moderator client helper quickstart](content-moderator-helper-quickstart-dotnet.md) (Inicio rápido de la aplicación auxiliar cliente de Content Moderator).

### <a name="install-required-packages"></a>Instalación de los paquetes requeridos

Instale los siguientes paquetes NuGet para el proyecto TermLists:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Microsoft.Rest.ClientRuntime.Azure
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>Actualizar las instrucciones using del programa

Modifique las instrucciones using del programa.

    using System;
    using System.Threading;
    using Microsoft.CognitiveServices.ContentModerator;
    using Microsoft.CognitiveServices.ContentModerator.Models;
    using ModeratorHelper;

### <a name="add-private-properties"></a>Adición de propiedades privadas

Agregue las siguientes propiedades privadas al espacio de nombres TermLists, clase Program.

    /// <summary>
    /// The language of the terms in the term lists.
    /// </summary>
    private const string lang = "eng";

    /// <summary>
    /// The minimum amount of time, in milliseconds, to wait between calls
    /// to the Content Moderator APIs.
    /// </summary>
    private const int throttleRate = 3000;

    /// <summary>
    /// The number of minutes to delay after updating the search index before
    /// performing image match operations against a the list.
    /// </summary>
    private const double latencyDelay = 0.5;

## <a name="create-a-term-list"></a>Crear una lista de términos

Puede crear una lista de términos con **ContentModeratorClient.ListManagementTermLists.Create**. El primer parámetro para **crear** es una cadena que contiene un tipo MIME, que debe ser "application/json". Para más información, consulte la [referencia de API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). El segundo parámetro es un objeto **body** que contiene un nombre y una descripción para la nueva lista de términos.

Agregue la siguiente definición de método al espacio de nombres TermLists, clase Program.

> [!NOTE]
> La clave de servicio de Content Moderator tiene un límite de solicitudes por segundo (RPS) y, si se supera el límite, el SDK emite una excepción con un código de error 429. 
>
> Una clave de un plan gratuito tiene un límite de una solicitud por segundo.

    /// <summary>
    /// Creates a new term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <returns>The term list ID.</returns>
    static string CreateTermList (ContentModeratorClient client)
    {
        Console.WriteLine("Creating term list.");

        Body body = new Body("Term list name", "Term list description");
        TermList list = client.ListManagementTermLists.Create("application/json", body);
        if (false == list.Id.HasValue)
        {
            throw new Exception("TermList.Id value missing.");
        }
        else
        {
            string list_id = list.Id.Value.ToString();
            Console.WriteLine("Term list created. ID: {0}.", list_id);
            Thread.Sleep(throttleRate);
            return list_id;
        }
    }

## <a name="update-term-list-name-and-description"></a>Actualización del nombre y la descripción de la lista de términos

Puede actualizar la información de la lista de términos con **ContentModeratorClient.ListManagementTermLists.Update**. El primer parámetro para **actualizar** es el identificador de la lista de términos. El segundo parámetro es un tipo MIME, que debe ser "application/json". Para más información, consulte la [referencia de API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f685). El tercer parámetro es un objeto **body** que contiene el nuevo nombre y la descripción.

Agregue la siguiente definición de método al espacio de nombres TermLists, clase Program.

    /// <summary>
    /// Update the information for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="name">The new name for the term list.</param>
    /// <param name="description">The new description for the term list.</param>
    static void UpdateTermList (ContentModeratorClient client, string list_id, string name = null, string description = null)
    {
        Console.WriteLine("Updating information for term list with ID {0}.", list_id);
        Body body = new Body(name, description);
        client.ListManagementTermLists.Update(list_id, "application/json", body);
        Thread.Sleep(throttleRate);
    }

## <a name="add-a-term-to-a-term-list"></a>Incorporación de un término a una lista de términos

Agregue la siguiente definición de método al espacio de nombres TermLists, clase Program.

    /// <summary>
    /// Add a term to the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to update.</param>
    /// <param name="term">The term to add to the term list.</param>
    static void AddTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Adding term \"{0}\" to term list with ID {1}.", term, list_id);
        client.ListManagementTerm.AddTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

## <a name="get-all-terms-in-a-term-list"></a>Obtención de todos los términos de una lista de términos

Agregue la siguiente definición de método al espacio de nombres TermLists, clase Program.

    /// <summary>
    /// Get all terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to get all terms.</param>
    static void GetAllTerms(ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Getting terms in term list with ID {0}.", list_id);
        Terms terms = client.ListManagementTerm.GetAllTerms(list_id, lang);
        TermsData data = terms.Data;
        foreach (TermsInList term in data.Terms)
        {
            Console.WriteLine(term.Term);
        }
        Thread.Sleep(throttleRate);
    }

## <a name="add-code-to-refresh-the-search-index"></a>Incorporación de código para actualizar el índice de búsqueda

Después de realizar cambios en una lista de términos, actualice su índice de búsqueda para que los cambios se incluyan la próxima vez que utilice la lista de términos para filtrar texto. Este paso es similar a cómo un motor de búsqueda de escritorio (si está habilitado) o un motor de búsqueda en web actualiza continuamente su índice para incluir nuevos archivos o páginas.

Puede actualizar un índice de búsqueda de la lista de términos con **ContentModeratorClient.ListManagementTermLists.RefreshIndexMethod**.

Agregue la siguiente definición de método al espacio de nombres TermLists, clase Program.

    /// <summary>
    /// Refresh the search index for the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to refresh.</param>
    static void RefreshSearchIndex (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Refreshing search index for term list with ID {0}.", list_id);
        client.ListManagementTermLists.RefreshIndexMethod(list_id, lang);
        Thread.Sleep((int)(latencyDelay * 60 * 1000));
    }

## <a name="screen-text-using-a-term-list"></a>Filtrado de texto mediante una lista de términos

Puede filtrar texto mediante una lista de términos con **ContentModeratorClient.TextModeration.ScreenText**, que necesita los siguientes parámetros.

- El idioma de los términos de la lista.
- Un tipo MIME, que puede ser "text/html", "text/xml", "text/markdown" o "text/plain".
- El texto que se analizará.
- Un valor booleano. Establezca este campo en **true** para autocorregir el texto antes de filtrarlo.
- Un valor booleano. Establezca este campo en **true** para detectar información de identificación Personal (PII) en el texto.
- El identificador de la lista de términos.

Para más información, consulte la [referencia de API](https://westus2.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66f).

**ScreenText** devuelve un objeto **Screen** que tiene una propiedad **Terms** que enumera todos los términos que Content Moderator detectó en el filtrado. Tenga en cuenta que si Content Moderator no detectó ningún término durante el filtrado, la propiedad **Terms** tendrá un valor **null**.

Agregue la siguiente definición de método al espacio de nombres TermLists, clase Program.

    /// <summary>
    /// Screen the indicated text for terms in the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to use to screen the text.</param>
    /// <param name="text">The text to screen.</param>
    static void ScreenText (ContentModeratorClient client, string list_id, string text)
    {
        Console.WriteLine("Screening text: \"{0}\" using term list with ID {1}.", text, list_id);
        Screen screen = client.TextModeration.ScreenText(lang, "text/plain", text, false, false, list_id);
        if (null == screen.Terms)
        {
            Console.WriteLine("No terms from the term list were detected in the text.");
        }
        else
        {
            foreach (DetectedTerms term in screen.Terms)
            {
                Console.WriteLine(String.Format("Found term: \"{0}\" from list ID {1} at index {2}.", term.Term, term.ListId, term.Index));
            }
        }
        read.Sleep(throttleRate);
    }

## <a name="delete-terms-and-lists"></a>Eliminar términos y listas

La eliminación de un término o una lista es sencilla. Se utiliza el SDK para realizar las siguientes tareas:

- Eliminar un término. (**ContentModeratorClient.ListManagementTerm.DeleteTerm**)
- Eliminar todos los términos de una lista sin eliminar la lista. (**ContentModeratorClient.ListManagementTerm.DeleteAllTerms**)
- Eliminar una lista y todo su contenido. (**ContentModeratorClient.ListManagementTermLists.Delete**)

### <a name="delete-a-term"></a>Eliminar un término

Agregue la siguiente definición de método al espacio de nombres TermLists, clase Program.

    /// <summary>
    /// Delete a term from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete the term.</param>
    /// <param name="term">The term to delete.</param>
    static void DeleteTerm (ContentModeratorClient client, string list_id, string term)
    {
        Console.WriteLine("Removed term \"{0}\" from term list with ID {1}.", term, list_id);
        client.ListManagementTerm.DeleteTerm(list_id, term, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-all-terms-in-a-term-list"></a>Eliminación de todos los términos de una lista de términos

Agregue la siguiente definición de método al espacio de nombres TermLists, clase Program.

    /// <summary>
    /// Delete all terms from the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list from which to delete all terms.</param>
    static void DeleteAllTerms (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Removing all terms from term list with ID {0}.", list_id);
        client.ListManagementTerm.DeleteAllTerms(list_id, lang);
        Thread.Sleep(throttleRate);
    }

### <a name="delete-a-term-list"></a>Eliminación de una lista de términos

Agregue la siguiente definición de método al espacio de nombres TermLists, clase Program.

    /// <summary>
    /// Delete the indicated term list.
    /// </summary>
    /// <param name="client">The Content Moderator client.</param>
    /// <param name="list_id">The ID of the term list to delete.</param>
    static void DeleteTermList (ContentModeratorClient client, string list_id)
    {
        Console.WriteLine("Deleting term list with ID {0}.", list_id);
        client.ListManagementTermLists.Delete(list_id);
        Thread.Sleep(throttleRate);
    }

## <a name="putting-it-all-together"></a>Resumen

Agregue la definición de método **Main** al espacio de nombres TermLists, clase Program. Por último, cierre la clase Program y el espacio de nombres TermLists.

    static void Main(string[] args)
    {
        using (var client = Clients.NewClient())
        {
            string list_id = CreateTermList(client);

            UpdateTermList(client, list_id, "name", "description");
            AddTerm(client, list_id, "term1");
            AddTerm(client, list_id, "term2");

            GetAllTerms(client, list_id);

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            string text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteTerm(client, list_id, "term1");

            // Always remember to refresh the search index of your list
            RefreshSearchIndex(client, list_id);

            text = "This text contains the terms \"term1\" and \"term2\".";
            ScreenText(client, list_id, text);

            DeleteAllTerms(client, list_id);
            DeleteTermList(client, list_id);

            Console.WriteLine("Press ENTER to close the application.");
            Console.ReadLine();
        }
    }

## <a name="run-the-application-to-see-the-output"></a>Ejecute la aplicación para ver la salida

La salida se incluirá en las siguientes líneas, pero los datos pueden variar.

    Creating term list.
    Term list created. ID: 252.
    Updating information for term list with ID 252.
    
    Adding term "term1" to term list with ID 252.
    Adding term "term2" to term list with ID 252.
    
    Getting terms in term list with ID 252.
    term1
    term2
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term1" from list ID 252 at index 32.
    Found term: "term2" from list ID 252 at index 46.
    
    Removed term "term1" from term list with ID 252.
    
    Refreshing search index for term list with ID 252.
    
    Screening text: "This text contains the terms "term1" and "term2"." using term list with ID 252.
    Found term: "term2" from list ID 252 at index 46.
    
    Removing all terms from term list with ID 252.
    Deleting term list with ID 252.
    Press ENTER to close the application.
    
## <a name="next-steps"></a>Pasos siguientes

[Descargue la solución de Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para este y otros inicios rápidos de Content Moderator para .NET y empiece a trabajar en la integración.
