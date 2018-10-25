---
title: Implementación de dispositivos IoT Edge desde el servicio Azure Machine Learning | Microsoft Docs
description: Obtenga información sobre cómo implementar un modelo desde el servicio Azure Machine Learning en dispositivos Azure IoT Edge. Implementar un modelo en un dispositivo perimetral permite puntuar los datos en el dispositivo, en lugar de enviarlos a la nube y esperar una respuesta.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shipatel
author: shivanipatel
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: 7d706cf71761496fd740c729224ee4331eeb2911
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091630"
---
# <a name="prepare-to-deploy-models-on-iot-edge"></a>Preparación de la implementación de modelos en IoT Edge

En este documento, aprenda a usar el servicio Azure Machine Learning para preparar un modelo entrenado para su implementación en un dispositivo Azure IoT Edge.

Un dispositivo Azure IoT Edge es un dispositivo basado en Linux o Windows que ejecuta el entorno de ejecución de Azure IoT Edge. En estos dispositivos pueden implementarse modelos de aprendizaje automático como módulos IoT Edge. Implementar un modelo en un dispositivo IoT Edge permite que el dispositivo use el modelo directamente, en lugar de tener que enviar datos a la nube para su procesamiento. Obtendrá tiempos de respuesta más rápidos y una menor transferencia de datos.

Antes de implementar un modelo en un dispositivo perimetral, utilice los pasos descritos en este documento para preparar el modelo y el dispositivo.

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

* Un área de trabajo de Azure Machine Learning. Para crear uno, siga los pasos del documento [Get started with Azure Machine Learning service](quickstart-get-started.md) (Introducción al servicio Azure Machine Learning).

* Un entorno de desarrollo. Para obtener más información, consulte el documento sobre [cómo configurar un entorno de desarrollo](how-to-configure-environment.md).

* Una instancia de [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md) en la suscripción de Azure. 

* Un modelo entrenado. Para obtener un ejemplo de entrenamiento de un modelo, consulte el documento [Train an image classification model with Azure Machine Learning](tutorial-train-models-with-aml.md) (Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning). Hay disponible un modelo previamente entrenado en el [Kit de herramientas de AI para el repositorio de Azure IoT Edge GitHub](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

## <a name="prepare-the-iot-device"></a>Preparación del dispositivo IoT

Para obtener información sobre cómo registrar el dispositivo e instalar el entorno de ejecución de IoT, siga los pasos descritos en el documento [Guía de inicio rápido: Implementación del primer módulo de IoT Edge en un dispositivo Linux x64](../../iot-edge/quickstart-linux.md).

## <a name="register-the-model"></a>Registro del modelo

Los módulos de Azure IoT Edge se basan en las imágenes de contenedor. Para implementar el modelo en un dispositivo IoT Edge, siga estos pasos para registrar el modelo en un área de trabajo del servicio Azure Machine Learning y crear una imagen de Docker. 

1. Inicialice el área de trabajo y cargue el archivo config.json:

    ```python
    from azureml.core  import Workspace

    #Load existing workspace from the config file info.
    ws  = Workspace.from_config()
    ```    

1. Registre el modelo en el área de trabajo. Reemplace el texto predeterminado con la ruta de acceso del modelo,el nombre, las etiquetas y la descripción:

    > [!IMPORTANT]
    > Si ha usado Azure Machine Learning para entrenar el modelo, puede que ya esté registrado en el área de trabajo. En este caso, omita este paso. Para ver una lista de los modelos registrados con este área de trabajo, use `Model.list(ws)`.

    ```python
    from azureml.core.model import Model
    model = Model.register(model_path = "model.pkl", # this path points to the local file
                        model_name = "best_model", # the model gets registered as this name
                        tags = {'attribute': "myattribute", 'classification': "myclassification"},
                        description = "My awesome model",
                        workspace = ws)
    ```    

1. Recupere el modelo registrado: 

    ```python
    from azureml.core.model import Model

    model_name = "best_model"
    model = Model(ws, model_name)                     
    ```    

## <a name="create-a-docker-image"></a>Cree una imagen de Docker.

1. Cree un **script de puntuación** denominado `score.py`. Este archivo se usa para ejecutar el modelo en la imagen. Debe incluir las siguientes funciones:

    * La función `init()`, que normalmente se carga el modelo en un objeto global. Esta función solo se ejecuta una vez cuando se inicia el contenedor de Docker. 

    * La función `run(input_data)` usa el modelo para predecir un valor basado en los datos de entrada. Las entradas y salidas de la ejecución suelen usan JSON para la serialización y deserialización, pero se admiten otros formatos.

    Para ver un ejemplo, consulte el [tutorial de clasificación de imágenes](tutorial-deploy-models-with-aml.md#make-script).

1. Cree un **archivo de entorno** denominado `myenv.yml`. Este archivo es una especificación del entorno Conda y contiene una lista de todas las dependencias necesarias para el script y el modelo. Para ver un ejemplo, consulte el [tutorial de clasificación de imágenes](tutorial-deploy-models-with-aml.md#make-myenv).

1. Configure la imagen de Docker con los archivos `score.py` y `myenv.yml`:
    
    ```python
    from azureml.core.image import Image, ContainerImage
    
    #Image configuration
    image_config = ContainerImage.image_configuration( runtime = "python", 
                           execution_script = "score.py",
                           conda_file = "myenv.yml", 
                           tags = {"attributes", "calssification"},
                           description = "Image that contains my model",
                           
                        )
    ```    

1. Para crear la imagen, use la configuración de imágenes y modelos:

    ```python
    image = ContainerImage.create (name = "myimage", 
                           models = [model], #this is the model object
                           image_config = image_config,
                           workspace = ws
                        )
    ```     

    La creación de la imagen tarda aproximadamente 5 minutos.

## <a name="get-the-container-registry-credentials"></a>Obtención de las credenciales del registro de contenedor

Azure IoT necesita las credenciales para el registro de contenedor que almacena las imágenes de Docker en el servicio de Azure Machine Learning. Utilice los pasos siguientes para obtener las credenciales:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/signin/index).

1. Vaya al área de trabajo del servicio Azure Machine Learning y seleccione __Introducción__. Para ir a la configuración del registro de contenedor, seleccione el vínculo __Registro__.

    ![Imagen de la entrada en el registro de contenedor](./media/how-to-deploy-to-iot/findregisteredcontainer.png)

1. Una vez en el registro de contenedor, seleccione **Claves de acceso** y habilite el usuario administrador.

    ![Imagen de la pantalla de claves de acceso](./media/how-to-deploy-to-iot/findaccesskey.png)

1. Guarde los valores de servidor de inicio de sesión, nombre de usuario y contraseña. 

   Estas credenciales son necesarias para proporcionar el acceso de dispositivo IoT Edge a las imágenes en su registro de contenedor privado.

## <a name="next-steps"></a>Pasos siguientes

Ha completado los preparativos de implementación. Ahora, siga los pasos del documento [Implementación de módulos de Azure IoT Edge desde Azure Portal](../../iot-edge/how-to-deploy-modules-portal.md) para realizar la implementación en el dispositivo Edge. Al establecer la __configuración del registro__ para el dispositivo, use las credenciales que configuró.
