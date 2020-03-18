---
title: 'Ejecución de script de Python: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Ejecución de script de Python en Azure Machine Learning para ejecutar código de Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 03/10/2020
ms.openlocfilehash: 52eb3bdb463389d075421661610b5ee94d14d77d
ms.sourcegitcommit: b8d0d72dfe8e26eecc42e0f2dbff9a7dd69d3116
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/10/2020
ms.locfileid: "79037076"
---
# <a name="execute-python-script-module"></a>Módulo Ejecutar script de Python

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Utilice este módulo para ejecutar código de Python. Para obtener más información acerca de los principios de diseño y arquitectura de Python, consulte [el siguiente artículo.](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts)

Con Python, puede realizar las tareas que no son compatibles actualmente con los módulos existentes, como:

+ Visualizar datos mediante `matplotlib`
+ Usar bibliotecas de Python para enumerar los conjuntos de datos y modelos en el área de trabajo
+ Leer, cargar y manipular datos de orígenes que no admitidos por el módulo [Import Data](./import-data.md) (Importar datos)
+ Ejecutar código de aprendizaje profundo propio 


Azure Machine Learning usa la distribución Anaconda de Python, que incluye muchas utilidades comunes para el procesamiento de datos. La versión de Anaconda se actualizará automáticamente. La versión actual es:
 -  Anaconda 4.5+ distribution for Python 3.6 

Los paquetes instalados previamente son:
-  asn1crypto==0.24.0
- attrs==19.1.0
- azure-common==1.1.18
- azure-storage-blob==1.5.0
- azure-storage-common==1.4.0
- certifi==2019.3.9
- cffi==1.12.2
- chardet==3.0.4
- cryptography==2.6.1
- distro==1.4.0
- idna==2.8
- jsonschema==3.0.1
- lightgbm==2.2.3
- more-itertools==6.0.0
- numpy==1.16.2
- pandas==0.24.2
- Pillow==6.0.0
- pip==19.0.3
- pyarrow==0.12.1
- pycparser==2.19
- pycryptodomex==3.7.3
- pyrsistent==0.14.11
- python-dateutil==2.8.0
- pytz==2018.9
- requests==2.21.0
- scikit-learn==0.20.3
- scipy==1.2.1
- setuptools==40.8.0
- six==1.12.0
- torch==1.0.1.post2
- torchvision==0.2.2.post3
- urllib3==1.24.1
- wheel==0.33.1 

 Para instalar otros paquetes que no están en la lista instalada previamente, por ejemplo *scikit-misc*, agregue el código siguiente al script: 

 ```python
import os
os.system(f"pip install scikit-misc")
```

## <a name="upload-files"></a>Carga de archivos
**Ejecución de script de Python** admite la carga de archivos con el [SDK de Python de Azure Machine Learning](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py#upload-file-name--path-or-stream-).

En el ejemplo siguiente se muestra cómo cargar un archivo de imagen en **Ejecución de script de Python**:

```Python

# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# imports up here can be used to
import pandas as pd

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame
#   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):

    # Execution logic goes here
    print(f'Input pandas.DataFrame #1: {dataframe1}')

    from matplotlib import pyplot as plt
    plt.plot([1, 2, 3, 4])
    plt.ylabel('some numbers')
    img_file = "line.png"
    plt.savefig(img_file)

    from azureml.core import Run
    run = Run.get_context(allow_offline=True)
    run.upload_file(f"graphics/{img_file}", img_file)

    # Return value must be of a sequence of pandas.DataFrame
    # E.g.
    #   -  Single return value: return dataframe1,
    #   -  Two return values: return dataframe1, dataframe2
    return dataframe1,
}
```

Una vez que la canalización se envía correctamente, puede obtener una vista previa de la imagen en el panel derecho del módulo ![Imagen cargada](media/module/upload-image-in-python-script.png).

## <a name="how-to-configure-execute-python-script"></a>Procedimiento para configurar Ejecución de script de Python

El módulo **Ejecución de script de Python** contiene código de Python de ejemplo que puede usar como punto de partida. Para configurar el módulo **Ejecución de script de Python**, debe proporcionar un conjunto de entradas y código de Python que se ejecutará en el cuadro de texto **Python script** (Script de Python).

1. Agregue el módulo **Execute Python Script** (Ejecutar script de Python) al experimento.

2. Agregue y conecte en **Dataset1** los conjuntos de datos del diseñador que quiera usar para la entrada. Haga referencia a este conjunto de datos en el script de Python como **DataFrame1**.

    El uso de un conjunto de datos es opcional, si desea generar datos mediante Python, o usar código de Python para importar los datos directamente al módulo.

    Este módulo admite la adición de un segundo conjunto de datos en **Dataset2**. Haga referencia al segundo conjunto de datos en el script de Python como DataFrame2.

    Los conjuntos de datos almacenados en Azure Machine Learning se convierten automáticamente en data.frames de **pandas** cuando se cargan con este módulo.

    ![Ejecución de asignación de entrada de Python](media/module/python-module.png)

4. Para incluir nuevos paquetes o código de Python, agregue el archivo comprimido que contiene estos recursos personalizados a **Script bundle** (Agrupación de scripts). La entrada para **Script bundle** (Agrupación de scripts) debe ser un archivo comprimido cargado en el área de trabajo como conjunto de datos de tipo de archivo. 

    Cualquier archivo incluido en el archivo comprimido cargado puede usarse durante la ejecución de la canalización. Si el archivo incluye una estructura de directorios, se conserva la estructura, pero debe anteponerse un directorio llamado **src** a la ruta de acceso.

5. En el cuadro de texto **Python Script** (Script de Python), escriba o pegue el script de Python válido.

    El cuadro de texto **Python Script** (Script de Python) se rellena previamente con algunas instrucciones de comentarios y código de ejemplo para el acceso a datos y salida. Debe editar o reemplazar este código. Asegúrese de seguir las convenciones de Python sobre la aplicación de sangría y mayúsculas y minúsculas.

    + El script debe contener una función denominada `azureml_main` como punto de entrada para este módulo.
    + La función de punto de entrada puede contener hasta dos argumentos de entrada: `Param<dataframe1>` y `Param<dataframe2>`
    + Los archivos comprimidos conectados al tercer puerto de entrada se descomprimen y almacenan en el directorio, `.\Script Bundle`, que también se agrega a `sys.path` de Python. 

    Por lo tanto, si el archivo ZIP contiene `mymodule.py`, impórtelo mediante `import mymodule`.

    + Se pueden devolver dos conjuntos de datos al diseñador, que deben ser una secuencia de tipo `pandas.DataFrame`. Puede crear otras salidas en el código de Python y escribirlas directamente en Azure Storage.

6. Ejecute la canalización, o seleccione el módulo y haga clic en **Run selected** (Ejecutar seleccionados) para ejecutar el script de Python.

    Todos los datos y el código se cargan en una máquina virtual y se ejecutan con el entorno de Python especificado.

## <a name="results"></a>Results

Los resultados de todos los cálculos realizados por el código de Python insertado deben proporcionarse como pandas.DataFrame, que se convierte automáticamente en el formato de conjunto de datos de Azure Machine Learning, por lo que puede usar los resultados con otros módulos en la canalización.

El módulo devuelve dos conjuntos de datos:  
  
+ **Conjunto de datos de resultados 1**, definido por el primer dataframe de pandas devuelto en el script de Python

+ **Conjunto de datos de resultados 2**, definido por el segundo dataframe de pandas devuelto en el script de Python


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 