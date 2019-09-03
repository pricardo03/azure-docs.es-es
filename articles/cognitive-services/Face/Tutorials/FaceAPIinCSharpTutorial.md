---
title: 'Tutorial: Detección y presentación de los datos de las caras en una imagen mediante el SDK de .NET'
titleSuffix: Azure Cognitive Services
description: En este tutorial, creará una aplicación Windows que use Face API para detectar y enmarcar las caras en una imagen.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: tutorial
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 54069fbaa8ad06d257ab835ed3b170fecb76d800
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603338"
---
# <a name="tutorial-create-a-wpf-app-to-display-face-data-in-an-image"></a>Tutorial: Creación de una aplicación WPF que muestre los datos de las caras en una imagen

En este tutorial, aprenderá a usar la API Face de Azure a través del SDK de cliente para .NET, para detectar caras en una imagen y, después, presentar dichos datos en la interfaz de usuario. Creará una aplicación Windows Presentation Framework (WPF) simple que detecta caras, dibuja un marco alrededor de cada una de ellas y muestra una descripción de la cara en la barra de estado. 

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> - Crear una aplicación WPF
> - Instalar la biblioteca cliente de Face API
> - Usar la biblioteca cliente para detectar caras en una imagen
> - Dibujar un marco alrededor de cada cara detectada
> - Mostrar una descripción de la cara resaltada en la barra de estado

![Captura de pantalla que muestra las caras detectadas enmarcadas en rectángulos](../Images/getting-started-cs-detected.png)

El código de ejemplo completo está disponible en el repositorio del [ejemplo de Cognitive Face CSharp](https://github.com/Azure-Samples/Cognitive-Face-CSharp-sample) de GitHub.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. 


## <a name="prerequisites"></a>Requisitos previos

- Una clave de suscripción de Face API. Puede obtener una clave de suscripción de evaluación gratuita en la página[Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). O bien, siga las instrucciones de [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse al servicio Face API y obtener la clave.
- Cualquier edición de [Visual Studio 2015 o 2017](https://www.visualstudio.com/downloads/).

## <a name="create-the-visual-studio-project"></a>Creación del proyecto de Visual Studio

Siga estos pasos para crear un nuevo proyecto de aplicación WPF.

1. En Visual Studio, abra el cuadro de diálogo Nuevo proyecto. Expanda **Instalado**, luego, **Visual C#**  y, después, seleccione **Aplicación WPF (.NET Framework)** .
1. Asigne a la aplicación el nombre **FaceTutorial** y haga clic en **Aceptar**.
1. Obtenga los paquetes NuGet requeridos. Haga clic con el botón derecho en el proyecto en el Explorador de soluciones y seleccione **Administrar paquetes NuGet**; luego, busque e instale el siguiente paquete:
    - [Microsoft.Azure.CognitiveServices.Vision.Face 2.2.0-preview](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/2.2.0-preview)

## <a name="add-the-initial-code"></a>Adición del código inicial

En esta sección, agregará el marco básico de la aplicación sin sus características específicas de la cara.

### <a name="create-the-ui"></a>Creación de la interfaz de usuario

Abra *MainWindow.xaml* y reemplace el contenido por el código siguiente; así se crea la ventana de la interfaz de usuario. Los métodos `FacePhoto_MouseMove` y `BrowseButton_Click` son controladores de eventos que definirá más adelante.

[!code-xaml[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml?range=1-18)]

### <a name="create-the-main-class"></a>Creación de la clase main

Abra *MainWindow.xaml.cs* y agregue los espacios de nombres de la biblioteca cliente, junto con otros espacios de nombres necesarios. 

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=1-12)]

Luego, inserte el código siguiente en la clase **MainWindow**. Este código crea una instancia de **FaceClient** mediante la clave de suscripción, que debe escribir usted mismo. También debe establecer la cadena de región en `faceEndpoint` para la región correcta de la suscripción (consulte la [documentación de Face API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para obtener una lista de los puntos de conexión de todas las regiones).

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=18-46)]

Luego, pegue el siguiente código en el método **MainWindow**.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=50-61)]

Por último, agregue los métodos **BrowseButton_Click** y **FacePhoto_MouseMove** a la clase. Estos métodos corresponden a los controladores de eventos declarados en *MainWindow.xaml*. El método **BrowseButton_Click** crea **OpenFileDialog**, que permite al usuario seleccionar una imagen .jpg. Luego, muestra la imagen en la ventana principal. Se insertará el código restante de **BrowseButton_Click** y **FacePhoto_MouseMove** en los pasos posteriores. Tenga en cuenta también la referencia`faceList`&mdash; a una lista de objetos **DetectedFace**. En esta referencia es donde la aplicación almacenará y llamará a los datos reales de la cara.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=64-90,146)]

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=148-150,187)]

### <a name="try-the-app"></a>Prueba de la aplicación

Presione **Iniciar** en el menú para probar la aplicación. Cuando se abra la ventana de la aplicación, haga clic en **Examinar** en la esquina inferior izquierda. Debería aparecer el cuadro de diálogo **Abrir archivo**. Seleccione una imagen del sistema de archivos y compruebe que se muestra en la ventana. Luego, cierre la aplicación y vaya al paso siguiente.

![Captura de pantalla que se muestra la imagen sin modificar de caras](../Images/getting-started-cs-ui.png)

## <a name="upload-image-and-detect-faces"></a>Carga de imagen y detección de caras

La aplicación detectará las caras mediante una llamada al método **FaceClient.Face.DetectWithStreamAsync**, que ajusta la API REST de [detección](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para cargar una imagen local.

Inserte el siguiente método en la clase **MainWindow** debajo del método **FacePhoto_MouseMove**. Este método define una lista de los atributos de la cara que se recuperan y lee el archivo de imagen enviada en **Stream**. Luego pasa estos dos objetos a la llamada al método **DetectWithStreamAsync**.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=189-226)]

## <a name="draw-rectangles-around-faces"></a>Dibujar rectángulos alrededor de las caras

Luego, agregará el código para dibujar un rectángulo alrededor de cada cara detectada en la imagen. En la clase **MainWindow**, inserte el código siguiente al final del método **BrowseButton_Click**, después de la línea `FacePhoto.Source = bitmapSource`. Este código rellena una lista de las caras detectadas de la llamada a **UploadAndDetectFaces**. Luego traza un rectángulo alrededor de cada cara y muestra la imagen modificada en la ventana principal.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=92-145)]

## <a name="describe-the-faces"></a>Descripción de las caras

Agregue el método siguiente a la clase **MainWindow**, debajo del método **UploadAndDetectFaces**. Este método convierte los atributos recuperados de la cara en una cadena que la describe.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=228-286)]

## <a name="display-the-face-description"></a>Visualización de la descripción de la cara

Agregue el siguiente código al método **FacePhoto_MouseMove**. Este controlador de eventos muestra la cadena de descripción de la cara de`faceDescriptionStatusBar` cuando el cursor pasa por encima de un rectángulo de la cara detectada.

[!code-csharp[](~/Cognitive-Face-CSharp-sample/FaceTutorialCS/FaceTutorialCS/MainWindow.xaml.cs?range=151-186)]


## <a name="run-the-app"></a>Ejecución de la aplicación

Ejecute la aplicación y busque una imagen que contenga una cara. Espere unos segundos para permitir que el servicio Face responda. Debe ver un rectángulo rojo en cada una de las caras de la imagen. Si mueve el ratón sobre el rectángulo de una cara, su descripción debería aparecer en la barra de estado.

![Captura de pantalla que muestra las caras detectadas enmarcadas en rectángulos](../Images/getting-started-cs-detected.png)


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido el proceso básico para usar el SDK de .NET del servicio Face y ha creado una aplicación que detecta y enmarca las caras de una imagen. Después, obtendrá más información acerca de los detalles de la detección de caras.

> [!div class="nextstepaction"]
> [Detección de caras en una imagen](../Face-API-How-to-Topics/HowtoDetectFacesinImage.md)
