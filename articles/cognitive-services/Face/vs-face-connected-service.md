---
title: 'Tutorial: Servicio conectado Face'
titleSuffix: Azure Cognitive Services
description: Cree una aplicación de Windows que use el servicio Face de Cognitive Services para detectar características de caras en una imagen.
services: cognitive-services
author: ghogen
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 12/05/2019
ms.author: ghogen
ms.openlocfilehash: e0fe92fc7f19c3c899bcccfa9f9cc18029af049c
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76170237"
---
# <a name="connect-to-the-face-service-by-using-connected-services-in-visual-studio"></a>Conexión al servicio Face con Servicios conectados en Visual Studio

Mediante el uso de del servicio Azure Face, puede detectar, analizar, organizar y etiquetar caras en fotos.

En este artículo y sus artículos complementarios se proporcionan detalles acerca del uso de la característica de Servicios conectados de Visual Studio para el servicio Face. La funcionalidad está disponible en Visual Studio 2017 15.7 o posterior, con la extensión Cognitive Services instalada.

## <a name="prerequisites"></a>Prerequisites

- Suscripción a Azure. Si no tiene, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- Visual Studio 2017, versión 15.7 o posterior, con la carga de trabajo **Desarrollo web** instalada. [Descárguelo ahora](https://www.visualstudio.com/downloads/).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="create-a-project-and-add-support-for-face"></a>Creación de un proyecto e incorporación de compatibilidad para Face

1. Cree un proyecto web ASP.NET Core. Use la plantilla de proyecto Vacío. 

1. En el **Explorador de soluciones**, elija **Agregar** > **Servicio conectado**.
   Aparece la página del servicio conectado con los servicios que puede agregar al proyecto.

   ![Elemento de menú Agregar servicio conectado](./media/vs-face-connected-service/Connected-Service-Menu.PNG)

1. En el menú de servicios disponibles, elija **Cognitive Services Face API**.

   ![Elección del servicio al que desea conectarse](./media/vs-face-connected-service/Cog-Face-Connected-Service-0.PNG)

   Si ha iniciado sesión en Visual Studio y tiene una suscripción de Azure asociada a su cuenta, aparece una página con una lista desplegable con las suscripciones.

   ![Seleccione su suscripción.](media/vs-face-connected-service/Cog-Face-Connected-Service-1.PNG)

1. Seleccione la suscripción que desea utilizar y, a continuación, elija un nombre para el servicio Face, o elija el vínculo Editar para modificar el nombre generado automáticamente, elija el grupo de recursos y el plan de tarifa.

   ![Edición de detalles del servicio conectado](media/vs-face-connected-service/Cog-Face-Connected-Service-2.PNG)

   Siga el vínculo para obtener más información sobre los planes de tarifa.

1. Elija Agregar para agregar compatibilidad con el servicio conectado.
   Visual Studio modifica su proyecto para agregar los paquetes NuGet, las entradas del archivo de configuración y otros cambios para admitir una conexión con el servicio Face.

## <a name="use-the-face-service-to-detect-attributes-of-faces-in-an-image"></a>Uso del servicio Face para detectar atributos de caras en una imagen

1. Agregue lo siguiente usando las instrucciones de Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   using System.Net.Http;
   using System.Net.Http.Headers;
   ```
 
1. Agregue un campo de configuración y después un constructor que inicializa el campo de configuración de la clase Startup para habilitar la configuración del programa.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. En la carpeta wwwroot del proyecto, agregue una carpeta de imágenes y agregue un archivo de imagen a la carpeta wwwroot. Por ejemplo, puede usar una de las imágenes en la [página de Face](https://azure.microsoft.com/services/cognitive-services/face/) de Azure Portal. Haga clic con el botón derecho en una de las imágenes, guárdela en la unidad de disco duro local y, a continuación, en el Explorador de soluciones, haga clic con el botón derecho en la carpeta imágenes y seleccione **Agregar** > **Elemento existente** para agregarlo al proyecto. El proyecto debe tener un aspecto similar al siguiente en el Explorador de soluciones:
 
   ![carpeta de imágenes con el archivo de imagen](media/vs-face-connected-service/Cog-Face-Connected-Service-6.PNG)

1. Haga clic con el botón derecho en el archivo de imagen, elija Propiedades y luego seleccione **Copiar si es posterior**.

   ![Copiar si es posterior](media/vs-face-connected-service/Cog-Face-Connected-Service-5.PNG)
 
1. Reemplace el método de configuración con el código siguiente para acceder al servicio de Face y probar una imagen. Cambie la cadena imagePath a la ruta de acceso correcta y el nombre de archivo para la imagen de cara.

   ```csharp
    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
        public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            // TODO: Change this to your image's path on your site.
            string imagePath = @"images/face1.png";

            // Enable static files such as image files.
            app.UseStaticFiles();

            string faceApiKey = this.configuration["FaceAPI_ServiceKey"];
            string faceApiEndPoint = this.configuration["FaceAPI_ServiceEndPoint"];

            HttpClient client = new HttpClient();

            // Request headers.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", faceApiKey);

            // Request parameters. A third optional parameter is "details".
            string requestParameters = "returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise";

            // Assemble the URI for the REST API Call.
            string uri = faceApiEndPoint + "/detect?" + requestParameters;

            // Request body. Posts an image you've added to your site's images folder.
            var fileInfo = env.WebRootFileProvider.GetFileInfo(imagePath);
            var byteData = GetImageAsByteArray(fileInfo.PhysicalPath);

            string contentStringFace = string.Empty;
            using (ByteArrayContent content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");

                // Execute the REST API call.
                var response = client.PostAsync(uri, content).Result;

                // Get the JSON response.
                contentStringFace = response.Content.ReadAsStringAsync().Result;
            }

            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }

            app.Run(async (context) =>
            {
                await context.Response.WriteAsync($"<p><b>Face Image:</b></p>");
                await context.Response.WriteAsync($"<div><img src=\"" + imagePath + "\" /></div>");
                await context.Response.WriteAsync($"<p><b>Face detection API results:</b></p>");
                await context.Response.WriteAsync("<p>");
                await context.Response.WriteAsync(JsonPrettyPrint(contentStringFace));
                await context.Response.WriteAsync("<p>");
            });
        }
   ```
    El código de este paso construye una solicitud HTTP con una llamada a la API REST Face, usando la clave establecida cuando agregó el servicio conectado.

1. Agregue las funciones auxiliares GetImageAsByteArray y JsonPrettyPrint.

   ```csharp
        /// <summary>
        /// Returns the contents of the specified file as a byte array.
        /// </summary>
        /// <param name="imageFilePath">The image file to read.</param>
        /// <returns>The byte array of the image data.</returns>
        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        /// <summary>
        /// Formats the given JSON string by adding line breaks and indents.
        /// </summary>
        /// <param name="json">The raw JSON string to format.</param>
        /// <returns>The formatted JSON string.</returns>
        static string JsonPrettyPrint(string json)
        {
            if (string.IsNullOrEmpty(json))
                return string.Empty;

            json = json.Replace(Environment.NewLine, "").Replace("\t", "");

            string INDENT_STRING = "    ";
            var indent = 0;
            var quoted = false;
            var sb = new StringBuilder();
            for (var i = 0; i < json.Length; i++)
            {
                var ch = json[i];
                switch (ch)
                {
                    case '{':
                    case '[':
                        sb.Append(ch);
                        if (!quoted)
                        {
                            sb.AppendLine();
                        }
                        break;
                    case '}':
                    case ']':
                        if (!quoted)
                        {
                            sb.AppendLine();
                        }
                        sb.Append(ch);
                        break;
                    case '"':
                        sb.Append(ch);
                        bool escaped = false;
                        var index = i;
                        while (index > 0 && json[--index] == '\\')
                            escaped = !escaped;
                        if (!escaped)
                            quoted = !quoted;
                        break;
                    case ',':
                        sb.Append(ch);
                        if (!quoted)
                        {
                            sb.AppendLine();
                        }
                        break;
                    case ':':
                        sb.Append(ch);
                        if (!quoted)
                            sb.Append(" ");
                        break;
                    default:
                        sb.Append(ch);
                        break;
                }
            }
            return sb.ToString();
        }
   ```

1. Ejecute la aplicación web y compruebe qué el servicio Face encontró la imagen.
 
   ![Imagen del servicio Face y resultados con formato](media/vs-face-connected-service/Cog-Face-Connected-Service-4.PNG)

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite el grupo de recursos, elimínelo. De esta forma, se eliminan la instancia de Cognitive Services y los recursos relacionados. Para eliminar el grupo de recursos mediante el portal:

1. Escriba el nombre del grupo de recursos en el cuadro de búsqueda de la parte superior del portal. Cuando vea el grupo de recursos que se utiliza en esta guía de inicio rápido en los resultados de búsqueda, selecciónelo.
1. Seleccione **Eliminar grupo de recursos**.
1. En el cuadro **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el servicio Face, lea la [documentación de Face](Overview.md).
