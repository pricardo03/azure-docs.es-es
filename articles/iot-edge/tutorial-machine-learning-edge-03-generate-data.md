---
title: 'Generación de datos de dispositivos simulados: Machine Learning en Azure IoT Edge | Microsoft Docs'
description: Cree dispositivos virtuales que generen datos de telemetría simulados que puedan utilizarse posteriormente para entrenar un modelo de aprendizaje automático.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 666172e3685b923ca0d0e5fa02878341fcd0a216
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67543872"
---
# <a name="tutorial-generate-simulated-device-data"></a>Tutorial: Generación de datos de dispositivos simulados

> [!NOTE]
> Este artículo forma parte de un tutorial sobre el uso de Azure Machine Learning en IoT Edge. Si ha llegado directamente a este artículo, le recomendamos que comience desde el [primer el artículo](tutorial-machine-learning-edge-01-intro.md) de la serie para obtener los mejores resultados.

En este artículo, se usan datos de entrenamiento de aprendizaje automático para simular un dispositivo que envía datos de telemetría a IoT Hub. Como se indica en la introducción, este tutorial completo usa el [conjunto de datos de simulación de degradación del motor de turbofán](https://c3.nasa.gov/dashlink/resources/139/) para simular datos de un conjunto de motores de avión para entrenamiento y pruebas.

Por el archivo readme.txt que lo acompaña, disponemos de la siguiente información:

* Los datos constan de varias series temporales multivariantes.
* Cada conjunto de datos se divide en subconjuntos de entrenamiento y prueba.
* Cada serie temporal proviene de un motor distinto.
* Cada motor se inicia con diferentes grados de desgaste inicial y variación de fabricación.

Para este tutorial, se usa el subconjunto de datos de entrenamiento de un único conjunto de datos (FD003).

En realidad, cada motor sería un dispositivo IoT independiente. Suponiendo que no tenga disponible una colección de motores de turbofán conectados a internet, crearemos un software sustituto para estos dispositivos.

El simulador es un programa de C# que usa las API de IoT Hub para registrar dispositivos virtuales con IoT Hub mediante programación. A continuación, se leen los datos de cada dispositivo del subconjunto de datos proporcionado por la NASA y se envían a su instancia de IoT Hub mediante un dispositivo IoT simulado. Todo el código para esta parte del tutorial está disponible en el directorio DeviceHarness del repositorio.

El proyecto DeviceHarness es un proyecto de .NET Core escrito en C# que consta de cuatro clases:

* **Program:** el punto de entrada para la ejecución responsable de controlar la entrada del usuario y la coordinación general.
* **TrainingFileManager:** responsable de leer y analizar el archivo de datos seleccionado.
* **CycleData:** representa una sola fila de datos en un archivo convertido a formato de mensaje.
* **TurbofanDevice:** responsable de crear un dispositivo IoT que se corresponde con un único dispositivo (serie temporal) en los datos y de transmitir los datos a IoT Hub a través del dispositivo IoT.

Las tareas descritas en este artículo deberían tardar unos 20 minutos en completarse.

En un contexto real, el trabajo de este paso seguramente lo realizarían desarrolladores de dispositivos y desarrolladores de nube.

## <a name="configure-visual-studio-code-and-build-deviceharness-project"></a>Configuración de Visual Studio Code y compilación del proyecto DeviceHarness

1. Abra una sesión de escritorio remoto en la máquina virtual, como se muestra en el artículo anterior.

1. Abra Visual Studio Code.

1. En Visual Studio Code, seleccione **Archivo** > **Abrir carpeta...** .

1. En el cuadro de texto **Carpeta**, escriba `C:\source\IoTEdgeAndMlSample\DeviceHarness` y haga clic en el botón **Seleccionar carpeta**.

   Si aparecen errores de OmniSharp en la ventana de salida, deberá desinstalar la extensión de C#, cerrar VS Code y volver a abrirlo, instalar la extensión de C# y, a continuación, volver a cargar la ventana.

1. Puesto que es la primera vez que usa extensiones en esta máquina, algunas extensiones se actualizarán e instalarán sus dependencias. Es posible que se le solicite que actualice la extensión. En tal caso, seleccione **Recargar ventana**.

1. Se le indicará que agregue los recursos necesarios para DeviceHarness. Seleccione **Sí** para agregarlos.

   * Es posible que la notificación tarde unos segundos en aparecer.
   * Si no ve la notificación, compruebe el icono de "campana" en la esquina inferior derecha.

   ![Mensaje emergente de la extensión de VS Code](media/tutorial-machine-learning-edge-03-generate-data/add-required-assets.png)

1. Seleccione **Restaurar** para restaurar las dependencias del paquete.

   ![Mensaje de restauración de VS Code](media/tutorial-machine-learning-edge-03-generate-data/restore-package-dependencies.png)

1. Compruebe que el entorno está configurado correctamente mediante el desencadenamiento de una compilación, `Ctrl + Shift + B` o **Terminal** > **Ejecutar tarea de compilación**.

1. Se le pedirá que seleccione la tarea de compilación que ejecutar. Seleccione **Build** (Compilar).

1. La compilación se ejecutará y generará un mensaje de operación correcta.

   ![Mensaje de salida de compilación correcta](media/tutorial-machine-learning-edge-03-generate-data/build-success.png)

1. Puede establecer esta compilación como tarea de compilación predeterminada si selecciona **Terminal** > **Configure Default Build Task...**  (Configurar tarea de compilación predeterminada) y elige **Compilar** en el mensaje.

## <a name="connect-to-iot-hub-and-run-deviceharness"></a>Conexión a IoT Hub y ejecución de DeviceHarness

Ahora que el proyecto se está compilando, conéctese a su instancia de IoT Hub para acceder a la cadena de conexión y supervisar el progreso de la generación de datos.

### <a name="sign-in-to-azure-in-visual-studio-code"></a>Inicio de sesión en Azure en Visual Studio Code

1. Inicie sesión en su suscripción de Azure en Visual Studio Code, abra la paleta de comandos, `Ctrl + Shift + P` o **Ver** > **Paleta de comandos...** .

1. En el símbolo del sistema, busque y seleccione **Azure: Iniciar sesión**.

1. Se abrirá una ventana del explorador, que le pedirá sus credenciales. Cuando se le redirija a una página de operación correcta, podrá cerrar el explorador.

### <a name="connect-to-your-iot-hub-and-retrieve-hub-connection-string"></a>Conexión a la instancia de IoT Hub y recuperación de la cadena de conexión del centro

1. En la sección inferior del explorador de Visual Studio Code, seleccione el marco de **dispositivos de Azure IoT Hub** para expandirlo.

1. En el marco expandido, haga clic en **Select IoT Hub** (Seleccionar IoT Hub).

1. Cuando se le solicite, seleccione la suscripción de Azure y, a continuación, su instancia de IoT Hub.

1. Haga clic en el marco de **dispositivos de Azure IoT Hub** y, a continuación, en **...** para ver más acciones. Seleccione **Copy IoT Hub connection string** (Copiar cadena de conexión de IoT Hub).

   ![Copia de cadena de conexión de IoT Hub](media/tutorial-machine-learning-edge-03-generate-data/copy-hub-connection-string.png)

### <a name="run-the-deviceharness-project"></a>Ejecución del proyecto DeviceHarness

1. Seleccione **Ver** > **Terminal** para abrir el terminal de Visual Studio Code.

   Si no ve ningún símbolo del sistema, presione Entrar.

1. Escriba `dotnet run` en el terminal.

1. Cuando se le solicite la cadena de conexión de IoT Hub, pegue la cadena de conexión que copió en la sección anterior.

1. En el marco de **dispositivos de Azure IoT Hub**, haga clic en el botón Actualizar.

   ![Actualización de la lista de dispositivos de IoT Hub](media/tutorial-machine-learning-edge-03-generate-data/refresh-hub-device-list.png)

1. Observe cómo se agregan los dispositivos a la instancia de IoT Hub y cómo se muestran en verde para indicar que los datos se están enviando mediante ese dispositivo.

1. Puede ver los mensajes que se envían al centro si hace clic con el botón derecho en cualquier dispositivo y selecciona **Start Monitoring Built-in Event Endpoint** (Iniciar supervisión del punto de conexión del evento integrado). Los mensajes se mostrarán en el panel de salida de Visual Studio Code.

1. Para detener la supervisión, haga clic en el panel de salida de **Azure IoT Hub Toolkit** y seleccione **Stop Monitoring Built-in Event Endpoint** (Detener supervisión del punto de conexión del evento integrado).

1. Deje que la aplicación se ejecute hasta que finalice, lo que tardará unos minutos.

## <a name="check-iot-hub-for-activity"></a>Comprobación de actividad en IoT Hub

Los datos enviados por DeviceHarness van a su instancia de IoT Hub. En Azure Portal, puede comprobar fácilmente que los datos han llegado al centro.

1. Abra [Azure Portal](https://portal.azure.com/) y vaya a su instancia de IoT Hub.

1. En la página de información general debe ver que se han enviado datos al centro:  

   ![Visualización de mensajes de dispositivo a la nube en IoT Hub](media/tutorial-machine-learning-edge-03-generate-data/iot-hub-usage.png)

## <a name="validate-data-in-azure-storage"></a>Validación de datos en Azure Storage

Los datos que acabamos de enviar a IoT Hub se han enrutado al contenedor de almacenamiento que creamos en el artículo anterior. Vamos a echar un vistazo a los datos en nuestra cuenta de almacenamiento.

1. En Azure Portal, vaya a la cuenta de almacenamiento.

1. En el navegador de la cuenta de almacenamiento, seleccione **Explorador de Storage (versión preliminar)** .

1. En el explorador de almacenamiento, seleccione **Contenedores de blobs** y, a continuación, **devicedata**.

1. En el panel de contenido, haga clic en la carpeta del nombre de la instancia de IoT Hub y, a continuación, en el año, el mes, el día y la hora. Verá varias carpetas que representan los minutos en los que se escribieron los datos.

   ![Visualización de carpetas en Blob Storage](media/tutorial-machine-learning-edge-03-generate-data/confirm-data-storage-results.png)

1. Haga clic en una de esas carpetas para buscar archivos de datos etiquetados como **00** y **01** correspondientes a la partición.

1. Los archivos están escritos en formato [Avro](https://avro.apache.org/), pero al hacer doble clic en uno de ellos se abrirá otra pestaña del explorador y se representarán parcialmente los datos. Si, en su lugar, se le pide que abra el archivo en un programa, puede elegir VS Code y los datos se representarán correctamente.

1. Ahora no es necesario que intente leer ni interpretar los datos; esto se hará en el siguiente artículo.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, hemos usado un proyecto de .NET Core para crear un conjunto de dispositivos virtuales y enviar datos con esos dispositivos a través de nuestra instancia de IoT Hub hacia un contenedor de Azure Storage. Este proyecto simula un escenario real, donde los dispositivos físicos envían datos, como lecturas de sensores, configuraciones operativas, modos y señales de error, etc., a una instancia de IoT Hub y, a continuación, a un almacenamiento mantenido. Una vez que se recopilan suficientes datos, se usan para entrenar modelos que predicen la vida útil restante (RUL) del dispositivo, lo que se demostrará en el próximo artículo.

Continúe con el siguiente artículo para entrenar un modelo de aprendizaje automático con los datos.

> [!div class="nextstepaction"]
> [Entrenamiento e implementación de un modelo de Azure Machine Learning](tutorial-machine-learning-edge-04-train-model.md)
