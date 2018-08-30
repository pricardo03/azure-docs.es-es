---
title: 'Guía de inicio rápido: Reconocimiento de voz en Java (Windows o Linux)'
titleSuffix: Microsoft Cognitive Services
description: Aprenda cómo utilizar el reconocimiento de voz en Java (Windows o Linux)
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 08/16/2018
ms.author: fmegen
ms.openlocfilehash: 11aba1ae6f49d6c00fabd928ae3aefedcbea8ed8
ms.sourcegitcommit: 3f8f973f095f6f878aa3e2383db0d296365a4b18
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/20/2018
ms.locfileid: "40234309"
---
# <a name="quickstart-recognize-speech-in-java-windows-or-linux"></a>Guía de inicio rápido: Reconocimiento de voz en Java (Windows o Linux)

[!include[Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Este documento describe cómo crear una aplicación de consola basada en Java para Java Runtime Environment (JRE) que hace uso de Speech SDK.
La aplicación se basa en el paquete Maven del SDK de Microsoft Cognitive Services.
Usaremos Eclipse como entorno de desarrollo integrado (IDE).

## <a name="prerequisites"></a>Requisitos previos

* Clave de suscripción para Speech Service. Consulte [Pruebe Speech Service gratis](get-started.md).
* Un PC (Windows x64, Ubuntu 16.04 x64) que pueda ejecutar Eclipse, con un micrófono operativo.
* JDK/JRE de 64 bits para Java 8.
* Versión 4.8 de [Eclipse](https://www.eclipse.org), versión de 64 bits.
* En Ubuntu 16.04, ejecute los siguientes comandos para la instalación de los paquetes necesarios:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

## <a name="create-and-configure-your-project"></a>Creación y configuración del proyecto

1. Inicie Eclipse.

1. En el selector de Eclipse, escriba el nombre de un nuevo directorio en el campo **Workspace** (Área de trabajo).
   Después haga clic en **Launch** (Iniciar).

   ![Creación del área de trabajo de Eclipse](media/sdk/qs-java-jre-01-create-new-eclipse-workspace.png)

1. Tras unos minutos, se muestra la ventana principal de Eclipse IDE.
   Si hay una pantalla de bienvenida en la pantalla, ciérrela.

1. Seleccione **File (Archivo)** \> **New (Nuevo)** \> **Project (Proyecto)**.

1. En el asistente **New Project** (Nuevo proyecto) que aparece, seleccione **Java Project** (Proyecto Java) y haga clic en **Next** (Siguiente).

   ![Selección de un asistente](media/sdk/qs-java-jre-02-select-wizard.png)

1. En la siguiente ventana, escriba **quickstart** como nombre de proyecto y elija **JavaSE-1.8** (o superior) como entorno de ejecución.
   Haga clic en **Finalizar**

   ![Selección de un asistente](media/sdk/qs-java-jre-03-create-java-project.png)

1. Si aparece una ventana titulada **Open Associated Perspective?** (¿Abrir perspectiva asociada?), seleccione **Open Perspective** (Abrir perspectiva).

1. En el **Package explorer** (Explorador de paquetes), haga clic con el botón derecho en el proyecto **quickstart** y seleccione **Configure (Configurar)** \> **Convert to Maven Project (Convertir en proyecto de Maven)**.

   ![Conversión en proyecto de Maven](media/sdk/qs-java-jre-04-convert-to-maven-project.png)

1. En la ventana emergente, escriba **com.microsoft.cognitiveservices.speech.samples** como **Group Id** (Identificador de grupo) y **quickstart** como **Artifact Id.** (Identificador de artefacto). Seleccione **Finalizar**.

   ![Configuración de Maven POM](media/sdk/qs-java-jre-05-configure-maven-pom.png)

1. Edite el archivo **pom.xml**.

  * Al final del archivo, antes cerrar la pestaña `</project>`, cree una sección de repositorios con una referencia al repositorio de Maven para el SDK de voz:

    [!code-xml[POM Repositories](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#repositories)]

  * También agregue después una sección de dependencias con la versión 0.6.0 de SDK de voz como una dependencia:

    [!code-xml[POM Dependencies](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/pom.xml#dependencies)]

  * Guarde los cambios.

## <a name="add-the-sample-code"></a>Incorporación del código de ejemplo

1. Seleccione **File (Archivo)** \> **New (Nuevo)** \> **Class (Clase)** para agregar una nueva clase vacía para el proyecto de Java.

1. En la ventana **New Java Class** (Nueva clase Java) escriba **speechsdk.quickstart** en el campo**Package** (Paquete) campo, y **Main** en el campo **Name** (Nombre).

   ![Creación de una clase principal](media/sdk/qs-java-jre-06-create-main-java.png)

1. Reemplace el código en `Main.java` con el siguiente fragmento de código:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java-jre/src/speechsdk/quickstart/Main.java#code)]

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-sample"></a>Compilación y ejecución del ejemplo

Presione F11, o seleccione **Run (Ejecutar)** \> **Debug (Depurar)**.
Los próximos 15 segundos de la entrada de voz del micrófono se reconocen y se registran en la ventana de consola.

![Salida de la consola después de un reconocimiento correcto](media/sdk/qs-java-jre-07-console-output.png)

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque este ejemplo en la carpeta `quickstart/java-jre`.

## <a name="next-steps"></a>Pasos siguientes

* [Obtener nuestros ejemplos](speech-sdk.md#get-the-samples)
