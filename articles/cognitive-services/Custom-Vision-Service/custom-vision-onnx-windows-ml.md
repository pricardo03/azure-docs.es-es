---
title: 'Tutorial: Uso de un modelo ONNX con Windows ML - Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Obtenga información acerca de cómo crear una aplicación para UWP de Windows que utilice un modelo ONNX exportado desde Azure Cognitive Services.
services: cognitive-services
author: larryfr
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 06/19/2018
ms.author: larryfr
ms.openlocfilehash: 3a9e9bc92ce38c4bb8d6d83c8017fa223342e7d2
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365611"
---
# <a name="tutorial-use-an-onnx-model-from-custom-vision-with-windows-ml-preview"></a>Tutorial: uso de un modelo ONNX de Custom Vision con Windows ML (versión preliminar)

Obtenga información acerca de cómo usar un modelo ONNX exportado desde Custom Vision Service con Windows ML (versión preliminar).

La información de este documento muestra cómo usar un archivo ONNX exportado desde Custom Vision Service con Windows ML. Se proporciona un ejemplo de una aplicación para UWP de Windows. En el ejemplo, se incluye un modelo entrenado que puede reconocer perros y gatos. También se proporcionan pasos acerca de cómo puede usar su propio modelo.

> [!div class="checklist"]
> * Acerca de la aplicación de ejemplo
> * Obtención del código de ejemplo
> * Ejecutar el ejemplo
> * Uso de su propio modelo

## <a name="prerequisites"></a>Requisitos previos

* Un dispositivo Windows 10 con:

    * Una cámara.

    * La versión 15.7 de Visual Studio 2017 o una posterior con la carga de trabajo __Desarrollo de la plataforma universal de Windows__ habilitada.

    * Modo de programador habilitado. Para obtener más información, consulte el documento [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

* (Opcional) Un archivo ONNX exportado desde Custom Vision Service. Para obtener más información, consulte el documento [Exportación del modelo para su uso con dispositivos móviles](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model).

    > [!NOTE]
    > Para usar su propio modelo, siga los pasos descritos en la sección [Uso de su propio modelo](#use-your-own-model).

## <a name="about-the-example-app"></a>Acerca de la aplicación de ejemplo

La aplicación es una aplicación para UWP genérica de Windows. Use la cámara de su dispositivo Windows 10 para suministrar imágenes para el modelo. Las etiquetas y los resultados devueltos por el modelo se muestran debajo de la vista previa de vídeo.

* A medida que los datos aparecen en la cámara, se usa [MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader) para extraer fotogramas individuales. Los fotogramas se envían al modelo para obtener los resultados.

* El modelo devuelve las etiquetas en las que se entrenó y un valor float que indica el nivel de seguridad de la imagen que contiene ese elemento.

### <a name="the-ui"></a>La IU

La IU de la aplicación de ejemplo se crea con los controles __CaptureElement__ y __TextBlock__. El control CaptureElement muestra una versión preliminar del vídeo de la cámara, y TextBlock los resultados devueltos del modelo. 

### <a name="the-model"></a>El modelo

El modelo (`cat-or-dog.onnx`) suministrado con el ejemplo se creó y se entrenó con Custom Vision Service de Cognitive Services. Después, el modelo entrenado se exportó como un modelo ONNX. Para obtener más información acerca de cómo usar este servicio, consulte los documentos [How to build a classifier](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) (Compilación de un clasificador) y [Exportación del modelo para su uso con dispositivos móviles](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model).

> [!IMPORTANT]
> El modelo proporcionado con el ejemplo se entrenó con un pequeño conjunto de imágenes de perros y gatos. Por lo tanto, es posible que no sea el mejor del mundo en reconocer perros y gatos.

### <a name="the-model-class-file"></a>El archivo de clase de modelo

Al agregar un archivo ONNX a una aplicación para UWP de Windows, se crea un archivo .cs. Este archivo tiene el mismo nombre que el archivo `.onnx` (`cat-or-dog` en este ejemplo) y contiene las clases usadas para funcionar con el modelo de C#. Sin embargo, las entidades de la clase generada pueden tener nombres como `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Puede cambiar el nombre de estas entradas de forma segura (hacer clic con el botón derecho, cambiar nombre) por un nombre descriptivo.

> [!NOTE]
> El código de ejemplo ha refactorizado los nombres de clase y método generados del siguiente modo:
>
> * `ModelInput`
> * `ModelOutput`
> * `Model`
> * `CreateModel`

### <a name="camera-access"></a>Acceso a la cámara

La pestaña __Capabilities__ (Funcionalidades) del archivo `Package.appxmanifest` está configurada para permitir el acceso a la cámara web y al micrófono.

> [!NOTE]
> Aunque este ejemplo no utiliza audio, tuve que habilitar el micrófono para poder obtener acceso a la cámara en el dispositivo.

La aplicación intenta acceder a la cámara de la parte de atrás del dispositivo, en caso de que haya una disponible. Usa la clase [MediaCapture](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture) para empezar a capturar vídeo desde la cámara. [MediaFrameReader](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.Frames.MediaFrameReader) se utiliza para capturar fotogramas de vídeo y enviarlos al modelo.

## <a name="get-the-example-code"></a>Obtención del código de ejemplo

La aplicación de ejemplo está disponible en [https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP](https://github.com/Azure-Samples/Custom-Vision-ONNX-UWP).

## <a name="run-the-example"></a>Ejecutar el ejemplo

1. Use la tecla `F5` para iniciar la aplicación desde Visual Studio. Es posible que se le solicite que habilite el modo de desarrollador. Para obtener más información, consulte el documento [Habilitar el dispositivo para el desarrollo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

2. Cuando se le solicite, permita que la aplicación pueda obtener acceso a la cámara y al micrófono del dispositivo.

3. Apunte la cámara hacia un perro o un gato. El resultado acerca de si la imagen contiene un perro o un gato se muestra debajo de la vista previa de la aplicación.

    > [!TIP]
    > Si no tiene ningún perro o gato cerca, puede usar una foto.

## <a name="use-your-own-model"></a>Uso de su propio modelo

Para usar su propio modelo, siga estos pasos:

> [!IMPORTANT]
> Los pasos descritos en esta sección cambian el nombre del modelo actual (cat-or-dog.cs) y refactorizan los nombres de clase y método del modelo nuevo. Esto sirve para evitar conflictos de nomenclatura con el modelo de ejemplo.

1. Entrene un modelo con Custom Vision Service. Para obtener información sobre cómo entrenar un modelo, consulte [How to build a classifier](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/getting-started-build-a-classifier) (Compilación de un clasificador).

2. Exporte el modelo entrenado como un modelo ONNX. Para obtener información acerca de cómo exportar un modelo, consulte el documento [Exportación del modelo para su uso con dispositivos móviles](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model).

3. En el Explorador de soluciones, haga clic en __cat-or-dog.cs__ y cambie el nombre a __cat-or-dog.txt__. El nuevo nombre evita conflictos de nomenclatura con el modelo nuevo.

    > [!TIP]
    > También puede utilizar nombres diferentes para los nombres de clase en el modelo nuevo, pero es más fácil reutilizar los nombres existentes.

4. En el Explorador de soluciones, haga clic con el botón derecho en la entrada __VisionApp__ y seleccione __Agregar__ > __Elemento existente...__.

5. Para generar una clase para el modelo, seleccione el archivo ONNX para importarlo y, a continuación, seleccione el botón __Agregar__. Una nueva clase con el mismo nombre que el archivo ONNX (pero una extensión `.cs`) se agrega en el Explorador de soluciones.

6. Abra el archivo .cs generado y busque los nombres de los elementos siguientes:

    > [!IMPORTANT]
    > Use el archivo `cat-or-dog.txt` de ejemplo como guía para reconocer las clases y funciones.

    * La clase que define la entrada del modelo. El nombre generado puede ser similar a `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelInput`. Cambie el nombre de esta clase a __ModelInput__.
    * La clase que define la salida del modelo. El nombre generado puede ser similar a `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70ModelOutput`. Cambie el nombre de esta clase a __ModelOutput__.
    * La clase que define el modelo. El nombre generado puede ser similar a `_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Cambie el nombre de esta clase a __Model__.
    * El método que crea el modelo. El nombre generado puede ser similar a `Create_x0033_04aa07b_x002D_6c8c_x002D_4641_x002D_93a6_x002D_f3152f8740a1_028da4e3_x002D_9c6e_x002D_480b_x002D_b53c_x002D_c1db13d24d70Model`. Cambie el nombre de este método a __CreateModel__.

7. En el Explorador de soluciones, mueva el archivo `.onnx` a la carpeta __Recursos__. 

8. Para incluir el archivo ONNX en el paquete de aplicación, seleccione el archivo `.onnx` y establezca __Acción de compilación__ en __Contenido__ en las propiedades.

9. Abra el archivo __MainPage.xaml.cs__. Busque la línea siguiente y cambie el nombre de archivo al nuevo archivo `.onnx`:

    ```csharp
    var file = await StorageFile.GetFileFromApplicationUriAsync(new Uri($"ms-appx:///Assets/cat-or-dog.onnx"));
    ```

    Este cambio carga el modelo nuevo en el entorno de ejecución.

10. Compile y ejecute la aplicación. Ahora usa el modelo nuevo para puntuar las imágenes.

## <a name="next-steps"></a>Pasos siguientes

Para conocer otras maneras de exportar y usar un modelo de Custom Vision, consulte los documentos siguientes:

* [Exportación del modelo](https://docs.microsoft.com/azure/cognitive-services/custom-vision-service/export-your-model)
* [Use exported Tensorflow model in an Android application](https://github.com/Azure-Samples/cognitive-services-android-customvision-sample) (Uso del modelo Tensorflow exportado en una aplicación de Android)
* [Use exported CoreML model in a Swift iOS application](https://go.microsoft.com/fwlink/?linkid=857726) (Uso del modelo CoreML exportado en una aplicación de iOS con Swift)
* [Use exported CoreML model in an iOS application with Xamarin](https://github.com/xamarin/ios-samples/tree/master/ios11/CoreMLAzureModel) (Uso del modelo CoreML exportado en una aplicación de iOS con Xamarin)

Para obtener más información sobre el uso de modelos ONNX con Windows ML, consulte el documento [Integrar un modelo en la aplicación con Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/integrate-model).
