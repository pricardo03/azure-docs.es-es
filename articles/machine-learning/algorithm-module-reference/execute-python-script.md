---
title: 'Ejecutar Script de Python: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo ejecutar Script de Python en el servicio de Azure Machine Learning para ejecutar código de Python.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 6e61ed5a18e69b107b78bf2042de21d14cd6beb5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029136"
---
# <a name="execute-python-script-module"></a>Ejecutar el módulo de Script de Python

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para ejecutar código Python. Para obtener más información acerca de los principios de diseño y arquitectura de Python, consulte [el siguiente artículo.](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts)

Con Python, puede realizar las tareas que no son compatibles actualmente con los módulos existentes, como:

+ Visualizar datos mediante `matplotlib`
+ Uso de bibliotecas de Python para enumerar los conjuntos de datos y modelos en el área de trabajo
+ Leer, cargar y manipular datos de orígenes que no admitidos la [importar datos](./import-data.md) módulo
+ Ejecutar código de aprendizaje profundo propio 


Azure Machine Learning usa la distribución de Anaconda de Python, que incluye muchas utilidades comunes para el procesamiento de datos. Versión de Anaconda se actualizará automáticamente. La versión actual es:
 -  Distribución de anaconda 4.5 + para Python 3.6 

Los paquetes instalados previamente son:
-  asn1crypto==0.24.0
- attrs==19.1.0
- azure-common==1.1.18
- azure-storage-blob==1.5.0
- azure-storage-common==1.4.0
- certifi==2019.3.9
- cffi==1.12.2
- chardet==3.0.4
- criptografía == 2.6.1
- distro==1.4.0
- idna==2.8
- jsonschema == 3.0.1
- lightgbm == 2.2.3
- más itertools == 6.0.0
- numpy == 1.16.2
- pandas == 0.24.2
- Almohadilla para == 6.0.0
- PIP == 19.0.3
- pyarrow == la 0.12.1
- pycparser == 2.19
- pycryptodomex==3.7.3
- pyrsistent == 0.14.11
- python-dateutil==2.8.0
- pytz==2018.9
- requests==2.21.0
- scikit-aprender == 0.20.3
- scipy == 1.2.1
- setuptools==40.8.0
- six==1.12.0
- torch==1.0.1.post2
- torchvision==0.2.2.post3
- urllib3==1.24.1
- rueda == 0.33.1 

 Para instalar otros paquetes no está en la lista instalada previamente, por ejemplo *scikit-misc*, agregue el código siguiente a la secuencia de comandos: 

 ```python
import os
os.system(f"pip install scikit-misc")
```

## <a name="how-to-use"></a>Modo de uso

El **Execute Python Script** módulo contiene código Python de ejemplo que puede usar como punto de partida. Para configurar el **Execute Python Script** módulo, debe proporcionar un conjunto de entradas y código de Python que se ejecutará en el **script de Python** cuadro de texto.

1. Agregar el **Execute Python Script** módulo al experimento.

2. Agregar y conectar en **Dataset1** los conjuntos de datos de la interfaz que desea usar para la entrada. Hacer referencia a este conjunto de datos en el script de Python como **DataFrame1**.

    Uso de un conjunto de datos es opcional, si desea generar los datos mediante Python, o usar código de Python para importar los datos directamente en el módulo.

    Este módulo admite la adición de un segundo conjunto de datos en **Dataset2**. El segundo conjunto de datos en el script de Python como DataFrame2 de referencia.

    Conjuntos de datos almacenados en Azure Machine Learning se convierten automáticamente en **pandas** data.frames cuando carga con este módulo.

    ![Ejecutar la asignación de entrada de Python](media/module/python-module.png)

4. Para incluir nuevos paquetes de Python o código, agregue el archivo comprimido que contiene estos recursos personalizados en **agrupación de scripts**. La entrada a **agrupación de scripts** debe ser un archivo comprimido ya cargado en el área de trabajo. 

    Cualquier archivo incluido en el archivo zip cargado puede usarse durante la ejecución del experimento. Si el archivo incluye una estructura de directorios, se conserva la estructura, pero debe anteponer un directorio llamado **src** a la ruta de acceso.

5. En el **script de Python** cuadro de texto, escriba o pegue el script de Python válido.

    El **script de Python** cuadro de texto se rellena previamente con algunas instrucciones de comentarios y código de ejemplo para el acceso a datos y de salida. **Debe modificar o reemplazar este código.** Asegúrese de seguir las convenciones de Python sobre sangría y mayúsculas y minúsculas.

    + El script debe contener una función denominada `azureml_main` como punto de entrada para este módulo.
    + La función de punto de entrada puede contener hasta dos argumentos de entrada: `Param<dataframe1>` y `Param<dataframe2>`
    + Los archivos comprimidos conectados al tercer puerto de entrada se descomprime y almacenan en el directorio, `.\Script Bundle`, que también se agrega a la versión de Python `sys.path`. 

    Por lo tanto, si el archivo zip contiene `mymodule.py`, impórtelo mediante `import mymodule`.

    + Dos conjuntos de datos se pueden devolver a la interfaz, que debe ser una secuencia de tipo `pandas.DataFrame`. Puede crear otras salidas en el código de Python y escribirlos directamente al almacenamiento de Azure.

6. Ejecute el experimento, o seleccione el módulo y haga clic en **ejecutar seleccionado** para ejecutar el script de Python.

    Todos los datos y el código se carga en una máquina virtual y ejecute con el entorno de Python especificado.

## <a name="results"></a>Results

Los resultados de todos los cálculos realizados por el código Python incrustado deben proporcionarse como un pandas. Trama de datos, que se convierte automáticamente en el formato de conjunto de datos de Azure Machine Learning, por lo que puede usar los resultados con otros módulos en el experimento.

El módulo devuelve dos conjuntos de datos:  
  
+ **Da como resultado el conjunto de datos 1**, definido por la primera de datos de pandas devuelto en el script de Python

+ **Como resultado de conjunto de datos 2**, definido por el segundo de datos de pandas devuelto en el script de Python


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 