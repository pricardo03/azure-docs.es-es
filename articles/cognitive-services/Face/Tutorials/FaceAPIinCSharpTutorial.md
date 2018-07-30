---
title: Tutorial de Face API en C# | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: En este tutorial, va a crear una aplicación Windows que use el servicio Face de Cognitive Services para detectar y enmarcar caras en una imagen.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: nolachar
ms.openlocfilehash: e4f2192c40f0b650b31ed59642dee89e42eca703
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39125956"
---
# <a name="tutorial-create-a-wpf-app-to-detect-and-frame-faces-in-an-image"></a>Tutorial: Creación de una aplicación WPF para detectar y enmarcar caras en una imagen

En este tutorial, se crea una aplicación de Windows Presentation Framework (WPF) que utiliza el servicio Face mediante su biblioteca clientes de .NET. La aplicación detecta caras en una imagen, dibuja un marco alrededor de cada cara y muestra una descripción de la cara en la barra de estado. El código de ejemplo completo está disponible en GitHub en [Detección y enmarcado de caras de una imagen en Windows](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample).

![Captura de pantalla que muestra las caras detectadas enmarcadas en rectángulos](../Images/getting-started-cs-detected.png)

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> - Crear una aplicación WPF
> - Instalar la biblioteca cliente del servicio Face
> - Usar la biblioteca cliente para detectar caras en una imagen
> - Dibujar un marco alrededor de cada cara detectada
> - Mostrar una descripción de la cara en la barra de estado

## <a name="prerequisites"></a>Requisitos previos

- Necesita una clave de suscripción para ejecutar el ejemplo. Puede obtener las claves de la suscripción de evaluación gratuita en la página de [Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api).
- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/). Para Visual Studio 2017, se necesita la carga de trabajo de desarrollo de aplicaciones de escritorio .NET. En este tutorial se usa Visual Studio 2017 Community Edition.
- El paquete NuGet de la biblioteca cliente [Microsoft.Azure.CognitiveServices.Vision.Face 2.0.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.0.0-preview). No es necesario descargar el paquete. A continuación, se proporcionan instrucciones de instalación.

## <a name="create-the-visual-studio-solution"></a>Creación de la solución de Visual Studio

Siga estos pasos para crear un proyecto de aplicación WPF de Windows.

1. Abra Visual Studio y, en el menú **Archivo**, haga clic en **Nuevo** y en **Proyecto**.
   - En Visual Studio 2017, expanda **Instalado** y **Otros lenguajes**. Seleccione **Visual C#** y, después, **Aplicación de WPF (.NET Framework)**.
   - En Visual Studio 2015, expanda **Instalado** y, después, **Plantillas**. Seleccione **Visual C#** y, después, **Aplicación de WPF**.
1. Asigne a la aplicación el nombre **FaceTutorial** y haga clic en **Aceptar**.

## <a name="install-the-face-service-client-library"></a>Instalar la biblioteca cliente del servicio Face

Siga estas instrucciones para instalar la biblioteca cliente,

1. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** y después **Consola del Administrador de paquetes**.
1. En la **consola del administrador de paquetes**, pegue lo siguiente y presiones **ENTRAR**.

    `Install-Package Microsoft.Azure.CognitiveServices.Vision.Face -Version 2.0.0-preview`

## <a name="add-the-initial-code"></a>Adición del código inicial

### <a name="mainwindowxaml"></a>MainWindow.xaml

Abra *MainWindow.xaml* (sugerencia: intercambie los paneles mediante el **icono de flecha arriba/abajo**) y reemplace el contenido por el código siguiente. Este código xaml se usa para crear la ventana de la interfaz de usuario. Tenga en cuenta los controladores de eventos, `FacePhoto_MouseMove` y `BrowseButton_Click`.

```xml
<Window x:Class="FaceTutorial.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        Title="MainWindow" Height="700" Width="960">
    <Grid x:Name="BackPanel">
        <Image x:Name="FacePhoto" Stretch="Uniform" Margin="0,0,0,50" MouseMove="FacePhoto_MouseMove" />
        <DockPanel DockPanel.Dock="Bottom">
            <Button x:Name="BrowseButton" Width="72" Height="20" VerticalAlignment="Bottom" HorizontalAlignment="Left"
                    Content="Browse..."
                    Click="BrowseButton_Click" />
            <StatusBar VerticalAlignment="Bottom">
                <StatusBarItem>
                    <TextBlock Name="faceDescriptionStatusBar" />
                </StatusBarItem>
            </StatusBar>
        </DockPanel>
    </Grid>
</Window>
```

### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

Expanda *MainWindow.xaml*, después abra *MainWindow.xaml.cs* y reemplace el código existente por el código siguiente. Pase por alto las líneas onduladas rojas, porque desaparecerán después de la primera compilación.

Las dos primeras líneas importan los espacios de nombres de la biblioteca cliente. A continuación, se crea `FaceClient`, pasando la clave de suscripción, mientras que la región Azure se establece en el constructor `MainWindow`. Los dos métodos, `BrowseButton_Click` y `FacePhoto_MouseMove`, corresponden a los controladores de eventos declarados en *MainWindow.xaml*.

`BrowseButton_Click` crea un `OpenFileDialog`, lo que permite al usuario seleccionar una imagen jpg. La imagen se lee y se muestra en la ventana principal. El código restante para `BrowseButton_Click` y el código para `FacePhoto_MouseMove` se insertan en los pasos siguientes.

```csharp
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;

using System;
using System.Collections.Generic;
using System.IO;
using System.Text;
using System.Threading.Tasks;
using System.Windows;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Imaging;

namespace FaceTutorial
{
    public partial class MainWindow : Window
    {
        // Replace <SubscriptionKey> with your valid subscription key.
        // For example, subscriptionKey = "0123456789abcdef0123456789ABCDEF"
        private const string subscriptionKey = "<SubscriptionKey>";

        // Replace or verify the region.
        //
        // You must use the same region as you used to obtain your subscription
        // keys. For example, if you obtained your subscription keys from the
        // westus region, replace "westcentralus" with "westus".
        //
        // NOTE: Free trial subscription keys are generated in the westcentralus
        // region, so if you are using a free trial subscription key, you should
        // not need to change this region.
        private const string baseUri =
            "https://westcentralus.api.cognitive.microsoft.com/face/v1.0";

        private readonly IFaceClient faceClient = new FaceClient(
            new ApiKeyServiceClientCredentials(subscriptionKey),
            new System.Net.Http.DelegatingHandler[] { });

        IList<DetectedFace> faceList;   // The list of detected faces.
        String[] faceDescriptions;      // The list of descriptions for the detected faces.
        double resizeFactor;            // The resize factor for the displayed image.

        public MainWindow()
        {
            InitializeComponent();

            if (Uri.IsWellFormedUriString(baseUri, UriKind.Absolute))
            {
                faceClient.BaseUri = new Uri(baseUri);
            }
            else
            {
                MessageBox.Show(baseUri,
                    "Invalid URI", MessageBoxButton.OK, MessageBoxImage.Error);
                Environment.Exit(0);
            }
        }

        // Displays the image and calls UploadAndDetectFaces.
        private async void BrowseButton_Click(object sender, RoutedEventArgs e)
        {
            // Get the image file to scan from the user.
            var openDlg = new Microsoft.Win32.OpenFileDialog();

            openDlg.Filter = "JPEG Image(*.jpg)|*.jpg";
            bool? result = openDlg.ShowDialog(this);

            // Return if canceled.
            if (!(bool)result)
            {
                return;
            }

            // Display the image file.
            string filePath = openDlg.FileName;

            Uri fileUri = new Uri(filePath);
            BitmapImage bitmapSource = new BitmapImage();

            bitmapSource.BeginInit();
            bitmapSource.CacheOption = BitmapCacheOption.None;
            bitmapSource.UriSource = fileUri;
            bitmapSource.EndInit();

            FacePhoto.Source = bitmapSource;
        }

        // Displays the face description when the mouse is over a face rectangle.
        private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
        {
        }
    }
}
```

### <a name="insert-your-subscription-key-and-verify-or-change-the-region"></a>Inserción de la clave de suscripción y comprobación o cambio de la región

- Busque la siguiente línea en *MainWindow.xaml.cs* y reemplace `<Subscription Key>` por su clave de suscripción a Face API:

    ```csharp
    private const string subscriptionKey = "<SubscriptionKey>";
    ```

- Busque la línea siguiente en *MainWindow.xaml.cs* y reemplace o compruebe la región de Azure asociada con la clave de suscripción:

    ```csharp
    private const string baseUri =
        "https://westcentralus.api.cognitive.microsoft.com/face/v1.0";
    ```

    Asegúrese de que la ubicación es la misma que en la que obtuvo las claves de suscripción. Por ejemplo, si obtuvo las claves de suscripción de la región **westus**, reemplace por ejemplo, reemplace `Westcentralus` por `Westus`.

    Si ha recibido las claves de suscripción desde la evaluación gratuita, la región para las claves es **westcentralus**, por lo que no se requiere ningún cambio.

### <a name="test-the-app"></a>Prueba de la aplicación

Presione **Iniciar** en el menú para probar la aplicación. Cuando se abre la ventana, haga clic en **Examinar** en la esquina inferior izquierda. Aparecerá un cuadro de diálogo **Abrir archivo** en el que podrá examinar y seleccionar una foto, que se mostrará en la ventana.

![Captura de pantalla que se muestra la imagen sin modificar de caras](../Images/getting-started-cs-ui.png)

## <a name="upload-an-image-to-detect-faces"></a>Carga de una imagen para detectar caras

La forma más sencilla de detectar caras es llamar al método `FaceClient.Face.DetectWithStreamAsync`, que envuelve el método de [Detect](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) API para cargar la imagen local.

Inserte el siguiente método a la clase `MainWindow`, debajo del método `FacePhoto_MouseMove`.

Se crea una lista de atributos de cara para analizar y el archivo de imagen enviado se lee en un `Stream`. Ambos se pasan a la llamada `DetectWithStreamAsync`.

```csharp
// Uploads the image file and calls DetectWithStreamAsync.
private async Task<IList<DetectedFace>> UploadAndDetectFaces(string imageFilePath)
{
    // The list of Face attributes to return.
    IList<FaceAttributeType> faceAttributes =
        new FaceAttributeType[]
        {
            FaceAttributeType.Gender, FaceAttributeType.Age,
            FaceAttributeType.Smile, FaceAttributeType.Emotion,
            FaceAttributeType.Glasses, FaceAttributeType.Hair
        };

    // Call the Face API.
    try
    {
        using (Stream imageFileStream = File.OpenRead(imageFilePath))
        {
            // The second argument specifies to return the faceId, while
            // the third argument specifies not to return face landmarks.
            IList<DetectedFace> faceList =
                await faceClient.Face.DetectWithStreamAsync(
                    imageFileStream, true, false, faceAttributes);
            return faceList;
        }
    }
    // Catch and display Face API errors.
    catch (APIErrorException f)
    {
        MessageBox.Show(f.Message);
        return new List<DetectedFace>();
    }
    // Catch and display all other errors.
    catch (Exception e)
    {
        MessageBox.Show(e.Message, "Error");
        return new List<DetectedFace>();
    }
}
```

## <a name="draw-rectangles-around-each-face"></a>Trazado de rectángulos alrededor de las caras

Agregue el código para dibujar un rectángulo alrededor de cada cara detectada en la imagen.

En *MainWindow.xaml.cs*, agregue el modificador `async` al método `BrowseButton_Click`.

```csharp
private async void BrowseButton_Click(object sender, RoutedEventArgs e)
```

Inserte el código siguiente al final del método `BrowseButton_Click`, después de la línea `FacePhoto.Source = bitmapSource`.

La lista de caras detectadas se rellena con la llamada a `UploadAndDetectFaces`. A continuación, se dibuja un rectángulo alrededor de cada cara y la imagen modificada se muestra en la ventana principal.

```csharp
// Detect any faces in the image.
Title = "Detecting...";
faceList = await UploadAndDetectFaces(filePath);
Title = String.Format(
    "Detection Finished. {0} face(s) detected", faceList.Count);

if (faceList.Count > 0)
{
    // Prepare to draw rectangles around the faces.
    DrawingVisual visual = new DrawingVisual();
    DrawingContext drawingContext = visual.RenderOpen();
    drawingContext.DrawImage(bitmapSource,
        new Rect(0, 0, bitmapSource.Width, bitmapSource.Height));
    double dpi = bitmapSource.DpiX;
    resizeFactor = (dpi > 0) ? 96 / dpi : 1;
    faceDescriptions = new String[faceList.Count];

    for (int i = 0; i < faceList.Count; ++i)
    {
        DetectedFace face = faceList[i];

        // Draw a rectangle on the face.
        drawingContext.DrawRectangle(
            Brushes.Transparent,
            new Pen(Brushes.Red, 2),
            new Rect(
                face.FaceRectangle.Left * resizeFactor,
                face.FaceRectangle.Top * resizeFactor,
                face.FaceRectangle.Width * resizeFactor,
                face.FaceRectangle.Height * resizeFactor
                )
        );

        // Store the face description.
        faceDescriptions[i] = FaceDescription(face);
    }

    drawingContext.Close();

    // Display the image with the rectangle around the face.
    RenderTargetBitmap faceWithRectBitmap = new RenderTargetBitmap(
        (int)(bitmapSource.PixelWidth * resizeFactor),
        (int)(bitmapSource.PixelHeight * resizeFactor),
        96,
        96,
        PixelFormats.Pbgra32);

    faceWithRectBitmap.Render(visual);
    FacePhoto.Source = faceWithRectBitmap;

    // Set the status bar text.
    faceDescriptionStatusBar.Text =
        "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="describe-the-faces-in-the-image"></a>Descripción de las caras en la imagen

Anexe el siguiente método a la clase `MainWindow`, debajo del método `UploadAndDetectFaces`.

El método convierte los atributos de la cara en una cadena que la describe. Esta cadena aparece cuando el puntero se sitúa sobre el rectángulo de la cara.

```csharp
// Creates a string out of the attributes describing the face.
private string FaceDescription(DetectedFace face)
{
    StringBuilder sb = new StringBuilder();

    sb.Append("Face: ");

    // Add the gender, age, and smile.
    sb.Append(face.FaceAttributes.Gender);
    sb.Append(", ");
    sb.Append(face.FaceAttributes.Age);
    sb.Append(", ");
    sb.Append(String.Format("smile {0:F1}%, ", face.FaceAttributes.Smile * 100));

    // Add the emotions. Display all emotions over 10%.
    sb.Append("Emotion: ");
    Emotion emotionScores = face.FaceAttributes.Emotion;
    if (emotionScores.Anger >= 0.1f)
        sb.Append(String.Format("anger {0:F1}%, ", emotionScores.Anger * 100));
    if (emotionScores.Contempt >= 0.1f)
        sb.Append(String.Format("contempt {0:F1}%, ", emotionScores.Contempt * 100));
    if (emotionScores.Disgust >= 0.1f)
        sb.Append(String.Format("disgust {0:F1}%, ", emotionScores.Disgust * 100));
    if (emotionScores.Fear >= 0.1f)
        sb.Append(String.Format("fear {0:F1}%, ", emotionScores.Fear * 100));
    if (emotionScores.Happiness >= 0.1f)
        sb.Append(String.Format("happiness {0:F1}%, ", emotionScores.Happiness * 100));
    if (emotionScores.Neutral >= 0.1f)
        sb.Append(String.Format("neutral {0:F1}%, ", emotionScores.Neutral * 100));
    if (emotionScores.Sadness >= 0.1f)
        sb.Append(String.Format("sadness {0:F1}%, ", emotionScores.Sadness * 100));
    if (emotionScores.Surprise >= 0.1f)
        sb.Append(String.Format("surprise {0:F1}%, ", emotionScores.Surprise * 100));

    // Add glasses.
    sb.Append(face.FaceAttributes.Glasses);
    sb.Append(", ");

    // Add hair.
    sb.Append("Hair: ");

    // Display baldness confidence if over 1%.
    if (face.FaceAttributes.Hair.Bald >= 0.01f)
        sb.Append(String.Format("bald {0:F1}% ", face.FaceAttributes.Hair.Bald * 100));

    // Display all hair color attributes over 10%.
    IList<HairColor> hairColors = face.FaceAttributes.Hair.HairColor;
    foreach (HairColor hairColor in hairColors)
    {
        if (hairColor.Confidence >= 0.1f)
        {
            sb.Append(hairColor.Color.ToString());
            sb.Append(String.Format(" {0:F1}% ", hairColor.Confidence * 100));
        }
    }

    // Return the built string.
    return sb.ToString();
}
```

## <a name="display-the-face-description"></a>Visualización de la descripción de la cara

Reemplace el método `FacePhoto_MouseMove` por el código siguiente.

Este controlador de eventos muestra la cadena de descripción de la cara cuando el puntero del mouse pasa por encima del rectángulo de la cara.

```csharp
private void FacePhoto_MouseMove(object sender, MouseEventArgs e)
{
    // If the REST call has not completed, return.
    if (faceList == null)
        return;

    // Find the mouse position relative to the image.
    Point mouseXY = e.GetPosition(FacePhoto);

    ImageSource imageSource = FacePhoto.Source;
    BitmapSource bitmapSource = (BitmapSource)imageSource;

    // Scale adjustment between the actual size and displayed size.
    var scale = FacePhoto.ActualWidth / (bitmapSource.PixelWidth / resizeFactor);

    // Check if this mouse position is over a face rectangle.
    bool mouseOverFace = false;

    for (int i = 0; i < faceList.Count; ++i)
    {
        FaceRectangle fr = faceList[i].FaceRectangle;
        double left = fr.Left * scale;
        double top = fr.Top * scale;
        double width = fr.Width * scale;
        double height = fr.Height * scale;

        // Display the face description if the mouse is over this face rectangle.
        if (mouseXY.X >= left && mouseXY.X <= left + width &&
            mouseXY.Y >= top  && mouseXY.Y <= top + height)
        {
            faceDescriptionStatusBar.Text = faceDescriptions[i];
            mouseOverFace = true;
            break;
        }
    }

    // String to display when the mouse is not over a face rectangle.
    if (!mouseOverFace)
        faceDescriptionStatusBar.Text =
            "Place the mouse pointer over a face to see the face description.";
}
```

## <a name="run-the-app"></a>Ejecución de la aplicación

Ejecute la aplicación y busque una imagen que contenga una cara. Espere unos segundos para permitir que el servicio Face responda. Después de eso, verá un rectángulo rojo en las caras de la imagen. Al mover el puntero sobre el rectángulo de una cara, su descripción aparecerá en la barra de estado.

![Captura de pantalla que muestra las caras detectadas enmarcadas en rectángulos](../Images/getting-started-cs-detected.png)

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido el proceso básico para usar la biblioteca cliente de Face y ha creado una aplicación para mostrar y enmarcar caras en una imagen.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre cómo detectar y usar los puntos de referencia de la cara.

> [!div class="nextstepaction"]
> [Detección de caras en una imagen](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
