---
title: 'Tutorial: Configuración de la extensión Azure Machine Learning para Visual Studio Code'
titleSuffix: Azure Machine Learning
description: Aprenda a configurar la extensión Azure Machine Learning para Visual Studio Code.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: luisquintanilla
ms.author: luquinta
ms.date: 01/16/2019
ms.openlocfilehash: 4000fcc80d507d3b1e871d7f3288fc3b77693c76
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76157466"
---
# <a name="set-up-azure-machine-learning-visual-studio-code-extension"></a>Configuración de la extensión Azure Machine Learning para Visual Studio Code

Aprenda a instalar y ejecutar scripts con la extensión Azure Machine Learning para Visual Studio Code.

En este tutorial, ha aprendido las tareas siguientes:

> [!div class="checklist"]
> * Instalación de la extensión Azure Machine Learning para Visual Studio Code
> * Inicio de sesión en la cuenta de Azure desde Visual Studio Code
> * Uso de la extensión Azure Machine Learning para ejecutar un script de ejemplo

## <a name="prerequisites"></a>Prerequisites

- Suscripción de Azure. Si no tiene una, regístrese para probar la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).
- Instale [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview), un editor de código ligero y multiplataforma. 

## <a name="install-the-extension"></a>Instalación de la extensión

1. Abra Visual Studio Code.
1. Seleccione el icono **Extensiones** de la **barra de actividades** para abrir la vista Extensiones.
1. En la vista Extensiones, busque "Azure Machine Learning".
1. Seleccione **Instalar**.

> [!NOTE]
> Como alternativa, puede instalar la extensión Azure Machine Learning al [descargar el instalador directamente](https://aka.ms/vscodetoolsforai) desde Visual Studio Marketplace. 

El resto de los pasos de este tutorial se han probado con la **versión 0.6.8** de la extensión.

## <a name="sign-in-to-your-azure-account"></a>Inicio de sesión en la cuenta de Azure

Para aprovisionar recursos y ejecutar cargas de trabajo en Azure, tiene que iniciar sesión con las credenciales de su cuenta de Azure. Para ayudar con la administración de cuentas, Azure Machine Learning instala automáticamente la extensión Azure Account. Visite el sitio siguiente para [más información sobre la extensión Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

1. Seleccione **Ver > Paleta de comandos** en la barra de menús para abrir la paleta de comandos. 
1. Escriba el comando "Azure: Sign-In" en el cuadro de texto para empezar el proceso de inicio de sesión.

## <a name="run-a-script-in-azure"></a>Ejecución de un script en Azure

Ahora que ha iniciado sesión en Azure con sus credenciales de cuenta, siga los pasos de esta sección para aprender a usar la extensión para entrenar un modelo de aprendizaje automático.

1. Descargue y descomprima el [repositorio VS Code Tools for AI](https://github.com/microsoft/vscode-tools-for-ai/archive/master.zip) donde quiera en el equipo.
1. Abra este directorio `mnist-vscode-docs-sample` en Visual Studio Code.
1. Seleccione el icono de **Azure** en la barra de actividades.
1. Seleccione el icono **Run Experiment** (Ejecutar experimento) en la parte superior de la vista de Azure Machine Learning.

    > [!div class="mx-imgBorder"]
    > ![Run Experiment](./media/tutorial-setup-vscode-extension/run-experiment.PNG) (Ejecutar experimento)

1. Cuando se expanda la paleta de comandos, siga las indicaciones.

    1. Seleccione su suscripción a Azure.
    1. Seleccione **Create a new Azure ML workspace** (Crear un área de trabajo de Azure Machine Learning)
    1. Seleccione el tipo de trabajo **TensorFlow Single-Node Training** (Entrenamiento de nodo único de TensorFlow).
    1. Escriba `train.py` como el script que se va a entrenar. Este es el archivo que contiene el código de un modelo de aprendizaje automático que clasifica imágenes de dígitos manuscritos.
    1. Especifique los paquetes siguientes como requisitos de ejecución.

        ```text
        pip: azureml-defaults; conda: python=3.6.2, tensorflow=1.15.0
        ```

1. En este momento aparecerá un archivo de configuración similar al siguiente en el editor de texto. La configuración contiene la información necesaria para ejecutar el trabajo de entrenamiento, como el archivo que contiene el código para entrenar el modelo y las dependencias de Python especificadas en el paso anterior.

    ```json
    {
        "workspace": "WS12191742",
        "resourceGroup": "WS12191742-rg2",
        "location": "South Central US",
        "experiment": "WS12191742-exp2",
        "compute": {
            "name": "WS12191742-com2",
            "vmSize": "Standard_D1_v2, Cores: 1; RAM: 3.5GB;"
        },
        "runConfiguration": {
            "filename": "WS12191742-com2-rc1",
            "condaDependencies": [
                "python=3.6.2",
                "tensorflow=1.15.0"
            ],
            "pipDependencies": [
                "azureml-defaults"
            ]
        }
    }
    ```

1. Seleccione **Submit experiment** (Enviar experimento) para ejecutar el experimento en Azure. Así se envían el archivo `train.py` y el de configuración al área de trabajo de Azure Machine Learning. A continuación, el trabajo de entrenamiento se inicia en un recurso de proceso de Azure.
1. Después de varios minutos se crea un directorio denominado `output` en el entorno local que contiene un modelo de TensorFlow entrenado.

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Entrenamiento e implementación de un modelo de TensorFlow de clasificación de imágenes con la extensión Azure Machine Learning para Visual Studio Code](tutorial-train-deploy-image-classification-model-vscode.md).
