---
title: Método auxiliar del SDK de Azure Content Moderator para .NET | Microsoft Docs
description: Cómo devolver un cliente de Content Moderator mediante el SDK de Azure Content Moderator para .NET
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/04/2018
ms.author: sajagtap
ms.openlocfilehash: 36f2124708731f78f34849d8210ed39ea8f59140
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380070"
---
# <a name="helper-code-to-return-a-content-moderator-client"></a>Código auxiliar para devolver un cliente de Content Moderator

En este artículo se proporciona información y ejemplos de código que le ayudarán a empezar a usar el SDK de Content Moderator para .NET para crear un cliente de Content Moderator para su suscripción.

La biblioteca se usa en otros inicios rápidos de esta sección.

En este artículo se da por supuesto que ya está familiarizado con Visual Studio y C#.

> [!IMPORTANT]
> El código contenido en esta biblioteca de clases es solo para demostración.
> Si adapta este código para su uso en producción, emplee un método seguro de almacenamiento y utilización de la clave de suscripción de Content Moderator.

## <a name="sign-up-for-content-moderator-services"></a>Suscripción a los servicios de Content Moderator

Antes de poder usar los servicios de Content Moderator mediante la API REST o el SDK, necesita una clave de suscripción.
Consulte el [inicio rápido](quick-start.md) para más información sobre cómo puede obtener la clave.

## <a name="create-your-visual-studio-project"></a>Creación del proyecto de Visual Studio

1. Cree un nuevo proyecto de **Biblioteca de clases (.NET Framework)**.

   En el código de ejemplo, se ha denominado al proyecto **ModeratorHelper**.

1. Agregue una referencia al ensamblado **System.Configuration** de Framework.

### <a name="install-required-packages"></a>Instalación de los paquetes requeridos

Instale los siguientes paquetes NuGet:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="create-the-content-moderator-client"></a>Creación del cliente de Content Moderator

Reemplace el contenido del archivo ModeratorHelper.cs por el código siguiente:

    using Microsoft.CognitiveServices.ContentModerator;

    namespace ModeratorHelper
    {
    /// <summary>
    /// Wraps the creation and configuration of a Content Moderator client.
    /// </summary>
    /// <remarks>This class library contains insecure code. If you adapt this 
    /// code for use in production, use a secure method of storing and using
    /// your Content Moderator subscription key.</remarks>
    public static class Clients
    {
        /// <summary>
        /// The region/location for your Content Moderator account, 
        /// for example, westus.
        /// </summary>
        private static readonly string AzureRegion = "myRegion";

        /// <summary>
        /// The base URL fragment for Content Moderator calls.
        /// </summary>
        private static readonly string AzureBaseURL =
            $"{AzureRegion}.api.cognitive.microsoft.com";

        /// <summary>
        /// Your Content Moderator subscription key.
        /// </summary>
        private static readonly string CMSubscriptionKey = "myKey";

        /// <summary>
        /// Returns a new Content Moderator client for your subscription.
        /// </summary>
        /// <returns>The new client.</returns>
        /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
        /// When you have finished using the client,
        /// you should dispose of it either directly or indirectly. </remarks>
        public static ContentModeratorClient NewClient()
        {
            // Create and initialize an instance of the Content Moderator API wrapper.
            ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

            client.BaseUrl = AzureBaseURL;
            return client;
        }
    }
    }


> [!IMPORTANT]
> Actualice los campos **AzureRegion** y **CMSubscriptionKey** con los valores de su identificador de región y clave de suscripción.

Ahora tiene una forma rápida de crear un cliente de Content Moderator para su suscripción.

## <a name="next-steps"></a>Pasos siguientes

[Descargue la solución de Visual Studio](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) para este y otros inicios rápidos de Content Moderator para .NET y empiece a trabajar en la integración.
