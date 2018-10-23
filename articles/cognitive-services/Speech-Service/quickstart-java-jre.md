---
title: 'Guía de inicio rápido: Reconocimiento de voz en Java (Windows o Linux)'
titleSuffix: Microsoft Cognitive Services
description: Aprenda cómo utilizar el reconocimiento de voz en Java (Windows o Linux)
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.component: Speech
ms.topic: quickstart
ms.date: 10/12/2018
ms.author: fmegen
ms.openlocfilehash: 1f0c19524ebd59dcf0377cc173cd780d656ba447
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339203"
---
# <a name="quickstart-recognize-speech-in-java-on-windows-or-linux-by-using-the-speech-sdk"></a>Inicio rápido: Reconocimiento de voz en Java para Windows o Linux mediante el SDK de Voz

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

En este artículo creará una aplicación de consola Java mediante el [SDK de Voz](speech-sdk.md). Transcribirá la conversión de voz en texto en tiempo real desde el micrófono de su PC. La aplicación se crea con el paquete de Maven del SDK de Voz y el IDE de Java de Eclipse (v4.8) en Windows de 64 bits o Ubuntu Linux 16.04. Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

> [!NOTE]
> Para el SDK de dispositivos de Voz y el dispositivo Roobo, consulte [SDK de dispositivos de voz](speech-devices-sdk.md).

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción del servicio Voz para completar este inicio rápido. Puede obtener una gratis. Para más detalles, consulte [Prueba gratuita del servicio Voz](get-started.md).


## <a name="create-and-configure-project"></a>Creación y configuración de un proyecto

Si usa Ubuntu 16.04, antes de iniciar Eclipse, ejecute los comandos siguientes para asegurarse de que los paquetes necesarios están instalados.

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

1. Inicie Eclipse.

1. En el selector de Eclipse, en el campo **Workspace** (Área de trabajo), escriba el nombre de un nuevo directorio de área de trabajo. Luego, seleccione **Launch** (Iniciar).

   ![Captura de pantalla del selector de Eclipse](media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Al cabo de unos segundos, aparece la ventana principal del IDE de Eclipse. Cierre la pantalla de bienvenida si hay alguna.

1. En la barra de menús de Eclipse, cree un nuevo proyecto eligiendo **File (Archivo)** > **New (Nuevo)** > **Project (Proyecto)**.

1. Aparecerá el cuadro de diálogo **Nuevo proyecto** . Seleccione **Java Project** (Proyecto de Java) y seleccione **Next** (Siguiente).

   ![Captura de pantalla del cuadro de diálogo New Project (Nuevo proyecto), con el proyecto de Java resaltado](media/sdk/qs-java-jre-02-select-wizard.png)

1. Se inicia el asistente para nuevo proyecto de Java. En el campo **Project name** (Nombre del proyecto), escriba **quickstart** y elija **JavaSE-1.8** como el entorno de ejecución. Seleccione **Finalizar**.

   ![Captura de pantalla del asistente de nuevo proyecto de Java](media/sdk/qs-java-jre-03-create-java-project.png)

1. Si aparece una ventana titulada **Open Associated Perspective?** (¿Abrir perspectiva asociada?), seleccione **Open Perspective** (Abrir perspectiva).

1. En el **Explorador de paquetes**, haga clic en el proyecto **quickstart**. Elija **Configure (Configurar)** > **Convert to Maven Project (Convertir en proyecto de Maven)** en el menú contextual.

   ![Captura de pantalla del Explorador de paquetes](media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. Aparece la ventana **Create new POM** (Crear nuevo POM). En el campo **Group Id** (Identificador de grupo), escriba **com.microsoft.cognitiveservices.speech.samples**, y en el campo **Artifact Id** (Identificador de artefacto), escriba **quickstart**. A continuación, seleccione **Finish** (Finalizar).

   ![Captura de pantalla de la pantalla Create new POM (Crear nuevo POM)](media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. Abra el archivo **pom.xml** y edítelo.

   * Al final del archivo, antes cerrar la etiqueta de cierre `</project>`, cree un elemento `repositories` con una referencia al repositorio de Maven para el SDK de Voz, tal y como se muestra aquí:

     [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#repositories)]

  * Agregue también un elemento `dependencies`, con la versión 1.0.1 del SDK de Voz como dependencia:

     [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#dependencies)]

   * Guarde los cambios.

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Para agregar una nueva clase vacía al proyecto de Java, seleccione **File (Archivo)**  >  **New (Nuevo)**  >  **Class (Clase)**.

1. En la ventana **New Java Class** (Nueva clase de Java) escriba **speechsdk.quickstart** en el campo **Package** (Paquete) y **Main** en el campo **Name** (Nombre).

   ![Captura de pantalla de la ventana New Java Class (Nueva clase de Java)](media/sdk/qs-java-jre-06-create-main-java.png)

1. Reemplace el código en `Main.java` con el siguiente fragmento de código:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

Presione F11, o seleccione **Run (Ejecutar)** > **Debug (Depurar)**.
Los próximos 15 segundos de la entrada de voz del micrófono se reconocen y se registran en la ventana de consola.

![Captura de pantalla de la salida de la consola después de un reconocimiento correcto](media/sdk/qs-java-jre-07-console-output.png)

[!INCLUDE [Download this sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/java-jre`.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Reconocimiento de intenciones a partir de contenido de voz con el SDK de Voz para Java](how-to-recognize-intents-from-speech-java.md)

## <a name="see-also"></a>Otras referencias

- [Traducción de voz](how-to-translate-speech-csharp.md)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
