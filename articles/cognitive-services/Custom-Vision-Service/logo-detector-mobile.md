---
title: 'Tutorial: Uso del detector de logotipos personalizados para reconocer servicios de Azure: Custom Vision'
titlesuffix: Azure Cognitive Services
description: En este tutorial, examinará una aplicación de ejemplo que usa Azure Custom Vision como parte de un escenario de detección de logotipos. Obtenga información acerca de la forma en que Custom Vision se usa con otros componentes para proporcionar una aplicación de un extremo a otro.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: pafarley
ms.openlocfilehash: 520e1981544f6ad0a6f1412a47dc8ea50fdd53a5
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55771468"
---
# <a name="tutorial-recognize-azure-service-logos-in-camera-pictures"></a>Tutorial: Reconocimiento de logotipos de servicios de Azure imágenes de cámara

En este tutorial, examinará una aplicación de ejemplo que usa Azure Custom Vision como parte de un escenario mayor. La aplicación AI Visual Provision es una aplicación de Xamarin.Forms para plataformas móviles que analiza imágenes de cámara de logotipos de servicios de Azure y, después, implementa dichos servicios en la cuenta de Azure del usuario. Aquí obtendrá información acerca de cómo usa Custom Vision en coordinación con otros componentes para proporcionar una útil aplicación de un extremo a otro. Si lo desea, puede ejecutar usted mismo toda la aplicación, o bien puede completar solamente la parte de Custom Vision de la instalación y explorar la forma en que lo usa la aplicación.

Este tutorial le mostrará cómo:

> [!div class="checklist"]
> - Crear un detector de objetos personalizado que reconozca los logotipos de los servicios de Azure
> - Conectar una aplicación a Azure Computer Vision y Custom Vision
> - Crear una cuenta de entidad de servicio de Azure para implementar servicios de Azure desde la aplicación

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar. 

## <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2017](https://www.visualstudio.com/downloads/)
- Xamarin Workload for Visual Studio (consulte [Instalación de Xamarin](https://docs.microsoft.com/xamarin/cross-platform/get-started/installation/windows))
- Un emulador de iOS o de Android para Visual Studio
- La [interfaz de la línea de comandos (CLI) de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli-windows?view=azure-cli-latest) (opcional)

## <a name="get-the-source-code"></a>Obtención del código fuente

Si desea usar la aplicación web que se proporciona, clone o descargue el código fuente de la aplicación del repositorio [AI Visual Provision](https://github.com/Microsoft/AIVisualProvision) de GitHub. Abra el archivo *Source/VisualProvision.sln* en Visual Studio. Más adelante realizará los modificaciones necesarias en algunos de los archivos del proyecto para poder ejecutar la aplicación.

## <a name="create-an-object-detector"></a>Creación de un detector de objetos

Inicie sesión en el [sitio web de Custom Vision](https://customvision.ai/) y cree un proyecto. Especifique un proyecto de detección de objetos y use el dominio Logo, ya que esto permitirá al servicio usar un algoritmo optimizado para la detección de logotipos. 

![ventana del cuadro de diálogo de nuevo proyecto en el sitio web de Custom Vision en el navegador Chrome](media/azure-logo-tutorial/new-project.png)

## <a name="upload-and-tag-images"></a>Carga y etiquetado de imágenes

A continuación, deberá entrenar el algoritmo de detección de logotipos, para lo que debe cargar imágenes de logotipos de servicios de Azure y etiquetarlas manualmente. El repositorio AIVisualProvision incluye un conjunto de imágenes de entrenamiento que puede usar. En el sitio web, seleccione el botón **Add images**  (Agregar imágenes) de la pestaña **Training Images** (Imágenes de entrenamiento) y desplácese a la carpeta **Documents/Images/Training_DataSet** del repositorio. Tendrá que etiquetar manualmente los logotipos en cada imagen, por lo que si solo va a probar este proyecto, es posible que desea cargar solo un subconjunto de las imágenes. Como mínimo, debe cargar 15 instancias de cada etiqueta que planee usar.

Una vez que haya cargado las imágenes de entrenamiento, seleccione la primera en la pantalla. Se abrirá la ventana de etiquetado. Dibuje cuadros y asigne al logotipo de cada imagen. 

![imagen de logotipos con etiquetas aplicadas en el sitio web de Custom Vision](media/azure-logo-tutorial/tag-logos.png)

La aplicación está configurada para trabajar con cadenas de etiquetas concretas; consulte las definiciones en el archivo *Source\VisualProvision\Services\Recognition\RecognitionService.cs*:

[!code-csharp[tag definitions](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/RecognitionService.cs?range=18-33)]

Cuando haya etiquetado una imagen, vaya a la derecha para etiquetar la siguiente. Cuando haya terminado salga de la ventana de etiquetado.

## <a name="train-the-object-detector"></a>Entrenamiento del detector de objetos

En el panel izquierdo, en el conmutador **Tags** (Etiquetas), seleccione **Tagged** (Con etiqueta) y debería ver todas las imágenes. Luego, haga clic en el botón verde de la parte superior de la página para entrenar el modelo. De esta forma se enseña al algoritmo a reconocer las mismas en las imágenes nuevas. También probará el modelo en algunas de las imágenes existentes para generar puntuaciones de precisión.

![el sitio web de Custom Vision, en la pestaña Training Images (Imágenes de entrenamiento); se resalta el botón Train (Entrenar)](media/azure-logo-tutorial/train-model.png)

## <a name="get-the-prediction-url"></a>Obtención de la dirección URL de predicción

Una vez que el modelo esté entrenado, estará listo para integrarlo en la aplicación. Para ello, deberá obtener la dirección URL del punto de conexión (la dirección del modelo que la aplicación consultará) y la clave de predicción (para conceder a la aplicación acceso a las solicitudes de predicción). En la pestaña **Performance** (Rendimiento), haga clic en el botón **Prediction URL** (Dirección URL de predicción) de la parte superior de la página.

![el sitio web de Custom Vision con una pantalla de Prediction API, que muestra una dirección URL y la clave de API](media/azure-logo-tutorial/cusvis-endpoint.png)

Copie la dirección URL del archivo de imagen y el valor `Prediction-key` en los campos adecuados del archivo *Source\VisualProvision\AppSettings.cs*:

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=22-26)]

## <a name="examine-custom-vision-usage"></a>Examen del uso de Custom Vision

Abra el archivo *Source/VisualProvision/Services/Recognition/CustomVisionService.cs* para ver cómo se utilizan la clave de Custom Vision y la dirección URL del punto de conexión en la aplicación. El método **PredictImageContentsAsync** toma una secuencia de bytes de un archivo de imagen junto con un token de cancelación (para la administración de tareas asincrónicas), llama a Custom Vision Prediction API y devuelve el resultado de la predicción. 

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/CustomVisionService.cs?range=12-28)]

Este resultado adopta la forma de una instancia de **PredictionResult** que, a su vez, contiene una lista de instancias de **Prediction** (Predicción). Una **predicción** contiene una etiqueta detectada y la ubicación de su rectángulo delimitador en la imagen.

[!code-csharp[Custom Vision fields](~/AIVisualProvision/Source/VisualProvision/Services/Recognition/Prediction.cs?range=3-12)]

Si desea más información acerca de la forma en que la aplicación controla estos datos, comience en el método **GetResourcesAsync** método, definido en el archivo *Source/VisualProvision/Services/Recognition/RecognitionService.cs*. 

## <a name="add-computer-vision"></a>Incorporación de Computer Vision

La parte de Custom Vision del tutorial ha finalizado, pero si desea ejecutar la aplicación, deberá integrar también el servicio Computer Vision. La aplicación usa la característica de reconocimiento de texto de Computer Vision para complementar el proceso de detección de logotipos; los logotipos de Azure se pueden reconocer por su apariencia _o_ por el texto que tienen impreso cerca. A diferencia de los modelos de Custom Vision, Computer Vision está entrenado previamente para realizar determinadas operaciones en las imágenes o los vídeos.

Solo hay que suscribirse al servicio Computer Vision para obtener una clave y una dirección URL del punto de conexión. Si necesita ayuda con este paso, consulte [Obtención de las claves de suscripción](https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe).

![el servicio Computer Vision en Azure Portal, con el menú de inicio rápido seleccionado; están resaltados un vínculo para las claves y la dirección URL del punto de conexión de la API](media/azure-logo-tutorial/comvis-keys.png)

Luego, abra el archivo *Source\VisualProvision\AppSettings.cs* y rellene las variables `ComputerVisionEndpoint` y `ComputerVisionKey` con los valores correctos.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=28-32)]


## <a name="create-a-service-principal"></a>Creación de una entidad de servicio

La aplicación requiere una cuenta de entidad de servicio de Azure para implementar los servicios en su suscripción de Azure. Las entidades de servicio le permiten delegar permisos concretos en una aplicación mediante el control de acceso basado en roles. Si desea más información, consulte la [guía de las entidades de servicio](https://docs.microsoft.com/azure/azure-stack/user/azure-stack-create-service-principals).

Las entidades de servicio se pueden crear con Azure Cloud Shell o con la CLI de Azure (como se indica a continuación). En primer lugar, inicie sesión y seleccione la suscripción que desea usar.

```console
az login
az account list
az account set --subscription "<subscription name or subscription id>"
```

Luego, cree la entidad de servicio (tenga en cuenta que esta operación puede tardar un tiempo).

```console
az ad sp create-for-rbac --name <servicePrincipalName> --password <yourSPStrongPassword>
```

Cuando se haya completado correctamente, debería ver el siguiente código JSON de salida, que contiene las credenciales necesarias.

```json
{
  "clientId": "(...)",
  "clientSecret": "(...)",
  "subscriptionId": "(...)",
  "tenantId": "(...)",
  ...
}
```
Tome nota de los valores de `clientId` y `tenantId`. Agréguelos a los campos pertinentes del archivo *Source\VisualProvision\AppSettings.cs*.

[!code-csharp[Computer Vision fields](~/AIVisualProvision/Source/VisualProvision/AppSettings.cs?range=8-16)]

## <a name="run-the-app"></a>Ejecución de la aplicación

Ya ha dado a la aplicación acceso a:
* un modelo de Custom Vision entrenado
* el servicio Computer Vision
* una cuenta de entidad de servicio 

Para ejecutar la aplicación, siga estos pasos:

1. En el Explorador de soluciones de Visual Studio, seleccione los proyectos VisualProvision.Android o VisualProvision.iOS, y elija el emulador correspondiente o un dispositivo móvil conectado en el menú desplegable en la barra de herramientas principal (tenga en cuenta que para ejecutar un emulador de iOS se necesita un dispositivo MacOS). Después, ejecute la aplicación.

1. En la primera pantalla que se carga, escriba su identificador de cliente, identificador de inquilino y contraseña de la entidad de servicio. Haga clic en el botón **Login** (Inicio de sesión).

    > [!NOTE]
    > En algunos de los emuladores, es posible que el botón **Login** (Inicio de sesión) no se active en este paso. En ese caso, detenga la aplicación, abra el archivo _Source/VisualProvision/Pages/LoginPage.xaml_, busque el elemento `Button` con la etiqueta LOGIN BUTTON y quite la línea:
      ```xaml
      IsEnabled="{Binding IsValid}"
      ```
    
    > Luego, vuelva a ejecutar la aplicación.

    ![la pantalla de la aplicación con los campos de las credenciales de la entidad de servicio](media/azure-logo-tutorial/app-credentials.png)

1. En la siguiente pantalla, seleccione la suscripción de Azure en el menú desplegable (debe contener todas las suscripciones a las que la entidad de servicio tiene acceso). Haga clic en el botón **Continue** (Continuar).

    ![la pantalla de la aplicación con un campo desplegable para la suscripción de Azure de destino](media/azure-logo-tutorial/app-az-subscription.png)

    En este momento la aplicación puede pedirle que conceda acceso a los dispositivos a la cámara y al almacenamiento de fotografías. Acepte dichos permisos.

1. A continuación, se activará la cámara del dispositivo. Haga una foto del logotipo de uno de los servicios de Azure que ha entrenado. Debería aparecer un cuadro de diálogo de implementación en el que se le solicite que seleccione la región y el grupo de recursos de los nuevos servicios (tal como lo haría si fuera a implementarlos desde Azure Portal). 

    ![una pantalla de la cámara de un smartphone en la que se ven dos recortes de papel de logotipos de Azure](media/azure-logo-tutorial/app-camera-capture.png)

    ![una pantalla de la aplicación con los campos de la región de implementación y la entrada del grupo de recursos](media/azure-logo-tutorial/app-deployment-options.png)

## <a name="clean-up-resources"></a>Limpieza de recursos 

Si ha seguido todos los pasos de este escenario y ha usado la aplicación para implementar los servicios de Azure en su cuenta, asegúrese de ir a [Azure Portal](https://ms.portal.azure.com/) cuando haya terminado para cancelar los servicios que no desee usar.

Además, si planea crear un proyecto de detección de objetos propio con Custom Vision en el futuro, es posible que desea eliminar el proyecto de detección de logotipos que ha creado en este tutorial. La versión de evaluación gratuita de Custom Vision permite ambos proyectos. En el [sitio web de Custom Vision](https://customvision.ai), vaya a **Projects** (Proyectos) y seleccione la papelera en **My New Project** (Mi proyecto nuevo).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha configurado y explorado una aplicación de Xamarin.Forms con todas las características que utiliza Custom Vision Service para detectar logotipos en las imágenes de la cámara de un móvil. A continuación, puede aprender los procedimientos recomendados para crear un modelo de Custom Vision, con el fin de que cuando cree para su propia aplicación sea más eficaz y preciso.

> [!div class="nextstepaction"]
> [Mejora del clasificador](getting-started-improving-your-classifier.md)