---
title: Ejecución de scripts de aprendizaje automático de Python
titleSuffix: Azure Machine Learning Studio
description: Aprenda a usar Python en Azure Machine Learning Studio.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/12/2019
ms.openlocfilehash: 4b4f3877b56752756050de0af226571ac2a93293
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57888166"
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio"></a>Ejecución de scripts de Python en Azure Machine Learning Studio

Python es una herramienta valiosa en el pecho herramienta de muchos científicos de datos. Se utiliza en cada etapa de flujos de trabajo de aprendizaje de máquina típica incluyendo la exploración de datos, extracción de características, entrenamiento del modelo y validación e implementación.

En este artículo se describe cómo puede usar el módulo ejecutar Script de Python para usar código de Python en sus experimentos de Azure Machine Learning Studio y servicios web.

## <a name="using-the-execute-python-script-module"></a>Con el módulo ejecutar Script de Python

La interfaz principal para Python en Studio es a través de la [Execute Python Script] [ execute-python-script] módulo. Se acepta hasta tres entradas y genera hasta dos salidas, similar a la [Execute R Script] [ execute-r-script] módulo. Código de Python se escribe en el cuadro de parámetro a través de una función de punto de entrada especialmente denominada llamada `azureml_main`.

![Ejecutar el módulo de Script de Python](./media/execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![Código de python de ejemplo en el cuadro de parámetro de módulo](./media/execute-python-scripts/embedded-machine-learning-python-script.png)

### <a name="input-parameters"></a>Parámetros de entrada

Las entradas en el módulo Python se exponen como tramas de datos de Pandas. El `azureml_main` función acepta hasta dos las tramas de datos de Pandas opcional como parámetros.

La asignación entre los puertos de entrada y los parámetros de función es posicional:

- El primer puerto de entrada conectado se asigna al primer parámetro de la función.
- La segunda entrada (si está conectada) se asigna al segundo parámetro de la función.
- Se usa para la tercera entrada [Importar módulos de Python adicionales](#import-modules).

Más detallada de cómo se asignan los puertos de entrada a parámetros de una semántica el `azureml_main` función se muestran a continuación.

![Tabla de configuraciones de puerto de entrada y la firma resultante de Python](./media/execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

### <a name="output-return-values"></a>Valores de salida devueltos

El `azureml_main` función debe devolver un solo datos de Pandas empaquetada en un Python [secuencia](https://docs.python.org/2/c-api/sequence.html) como una tupla, lista o matriz de NumPy. El primer elemento de esta secuencia se devuelve al primer puerto de salida del módulo. El segundo puerto de salida del módulo se usa para [visualizaciones](#visualizations) y no requiere un valor devuelto. Este esquema se muestra a continuación.

![Asignación de puertos para los parámetros de entrada y devuelve el valor para el puerto de salida](./media/execute-python-scripts/map-of-python-script-inputs-outputs.png)

## <a name="translation-of-input-and-output-data-types"></a>Conversión de tipos de datos de entrada y salida

Studio conjuntos de datos no son los mismos como tramas de datos de Panda. Como resultado, los conjuntos de datos de entrada en Studio se convierten en datos de Pandas y tramas de datos de salida se convierten en conjuntos de datos de Studio. Durante este proceso de conversión, también se realizan las conversiones siguientes:

 **Tipo de datos de Python** | **Procedimiento de traducción de Studio** |
| --- | --- |
| Las cadenas y valores numéricos| Traducir como está |
| Pandas 'NA' | Traducir como 'Falta el valor' |
| Vectores de índice | No compatible * |
| Nombres de columna que no son cadena | Llamar a `str` en nombres de columna |
| Nombres de columna duplicados | Agregue el sufijo numérico: (1), (2), (3), y así sucesivamente.

**Todas las tramas de datos de entrada en la función Python siempre tienen un índice numérico de 64 bits del 0 al número de filas menos 1*

## <a id="import-modules"></a>Importar módulos de script de Python existentes

El back-end que usa para ejecutar Python se basa en [Anaconda](https://store.continuum.io/cshop/anaconda/), ampliamente distribución científica de Python. Incluye cerca de 200 de los paquetes de Python más comunes utilizados en las cargas de trabajo centradas en datos. Studio no admite actualmente el uso de sistemas de administración de paquetes como Pip o Conda, instalar y administrar las bibliotecas externas.  Si encuentra la necesidad de incorporar bibliotecas adicionales, use el siguiente escenario como guía.

Un caso de uso común es incorporar scripts existentes de Python en experimentos de Studio. El [Execute Python Script] [ execute-python-script] módulo acepta un archivo zip que contiene los módulos de Python en el tercer puerto de entrada. El marco de ejecución descomprime el archivo en runtime y el contenido se agrega a la ruta de acceso de la biblioteca del intérprete de Python. La función de punto de entrada `azureml_main` luego puede importar directamente estos módulos. 

Por ejemplo, considere el archivo Hello.py que contiene una función "Hello, World".

![Función definida por el usuario en el archivo Hello.py](./media/execute-python-scripts/figure4.png)

A continuación, creamos un archivo Hello.zip que contiene Hello.py:

![Archivo zip que contiene código Python definido por el usuario](./media/execute-python-scripts/figure5.png)

Cargue el archivo zip como un conjunto de datos en Studio. A continuación, crear y ejecutar un experimento que usa el código de Python en el archivo Hello.zip asociándolo al tercer puerto de entrada de la **Execute Python Script** módulo tal como se muestra en la siguiente imagen.

![Experimento de ejemplo con Hello.zip como entrada a un módulo ejecutar Script de Python](./media/execute-python-scripts/figure6a.png)

![Código Python definido por el usuario cargado como un archivo zip](./media/execute-python-scripts/figure6b.png)

La salida del módulo muestra que el archivo ZIP se ha desempaquetado y que la función `print_hello` se ha ejecutado.

![Salida del módulo que muestra la función definida por el usuario](./media/execute-python-scripts/figure7.png)

## <a name="accessing-azure-storage-blobs"></a>Obtener acceso a Blobs de Azure Storage

Puede tener acceso a datos almacenados en una cuenta de Azure Blob Storage mediante estos pasos:

1. Descargue el [paquete de Azure Blob Storage para Python](https://azuremlpackagesupport.blob.core.windows.net/python/azure.zip) localmente.
1. Cargue el archivo zip en el área de trabajo de Studio como un conjunto de datos.
1. Crear el objeto BlobService con `protocol='http'`

```
from azure.storage.blob import BlockBlobService

# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='account_name', account_key='account_key', protocol='http')
```

1. Deshabilitar **se requiere transferencia segura** en Storage **configuración** ficha Configuración

![Deshabilitar la transferencia segura necesaria en el portal de Azure](./media/execute-python-scripts/disable-secure-transfer-required.png)

## <a name="operationalizing-python-scripts"></a>Funcionamiento de los scripts de Python

Cuando un experimento de puntuación se publica como servicio web, se llama a todos los módulos [Ejecutar script de Python][execute-python-script] usados en él. Por ejemplo, la imagen siguiente muestra un experimento de puntuación que contiene el código para evaluar una expresión de Python.

![Área de trabajo de Studio para un servicio web](./media/execute-python-scripts/figure3a.png)

![Expresión de Python Pandas](./media/execute-python-scripts/python-script-with-python-pandas.png)

Un servicio web creado a partir de este experimento tendría las siguientes acciones:

1. Tomar una expresión de Python como entrada (como una cadena)
1. Envío de la expresión de Python para el intérprete de Python
1. Devuelve una tabla que contiene la expresión y el resultado evaluado.

## <a id="visualizations"></a>Trabajar con visualizaciones

Pueden devolver gráficos creados con MatplotLib el [Execute Python Script][execute-python-script]. Sin embargo, los trazados no redirigen automáticamente a las imágenes tal como están al usar R. Por lo que el usuario debe guardar explícitamente todo gráfico como archivo PNG.

Para generar imágenes desde MatplotLib, debe realizar los pasos siguientes:

1. Cambiar el back-end a "AGG" desde el representador basado en Qt predeterminado.
1. Cree un nuevo objeto de figura.
1. Obtenga el eje y genere todos los gráficos en él.
1. Guarde la figura en un archivo PNG.

Este proceso se ilustra en las siguientes imágenes que creación una matriz de trazado de dispersión con la función scatter_matrix de Pandas.

![Código para guardar figuras de MatplotLib en imágenes](./media/execute-python-scripts/figure-v1-8.png)

![Haga clic en visualizar en un módulo ejecutar Script de Python para ver las cifras](./media/execute-python-scripts/figure-v2-9a.png)

![Visualización de los gráficos para un experimento de ejemplo con código Python](./media/execute-python-scripts/figure-v2-9b.png)

Es posible devolver varias figuras si se guardan en diferentes imágenes. El tiempo de ejecución de Studio selecciona todas las imágenes y los concatena para visualización.

## <a name="advanced-examples"></a>Ejemplos avanzados

El entorno Anaconda instalado en Studio contiene paquetes comunes como NumPy, SciPy y Scikits-Learn. Estos paquetes pueden usarse de forma eficaz para el procesamiento de datos en un canalización de aprendizaje automático.

Por ejemplo, el siguiente experimento y script ilustran el uso de sistemas aprendices de conjunto en Scikits-Learn para calcular las puntuaciones de importancia de características de un conjunto de datos. Las puntuaciones se pueden usar para realizar la selección de características supervisada antes de transmitirlas a otro modelo.

Aquí se muestra la función de Python usada para calcular las puntuaciones de importancia y ordenar las características según esas puntuaciones:

![Función para clasificar características por puntuaciones](./media/execute-python-scripts/figure8.png)

El siguiente experimento calcula y devuelve las puntuaciones de importancia de características en el conjunto de datos "Diabetes en los indios Pima" en Azure Machine Learning Studio:

![Experimento para clasificar las características en el conjunto de datos de la Diabetes indios Pima mediante Python](./media/execute-python-scripts/figure9a.png)

![Visualización de la salida del módulo ejecutar Script de Python](./media/execute-python-scripts/figure9b.png)

## <a name="limitations"></a>Limitaciones

El [Execute Python Script] [ execute-python-script] módulo actualmente tiene las siguientes limitaciones:

### <a name="sandboxed-execution"></a>Ejecución en espacio aislado

El runtime de Python está actualmente en espacio aislado y no permite el acceso a la red o sistema de archivos local de forma permanente. Todos los archivos guardados localmente son aislados y eliminados una vez que se finaliza el módulo. El código Python no puede tener acceso a la mayoría de los directorios de la máquina en que se ejecuta, con la excepción del directorio actual y sus subdirectorios.

### <a name="lack-of-sophisticated-development-and-debugging-support"></a>Falta de compatibilidad con la depuración y desarrollo sofisticado

 El módulo Python actualmente no es compatible con características de IDE, como IntelliSense y depuración. Además, si se produce un error en el módulo en runtime, el seguimiento de la pila completo de Python está disponible. Pero debe verse en el registro de salida del módulo. De momento se recomienda que los usuarios desarrollen y depuren scripts de Python en un entorno como IPython y luego importen el código al módulo.

### <a name="single-data-frame-output"></a>Salida de datos único marco

 El punto de entrada de Python solo tiene permitido devolver una trama de datos como salida. No es actualmente posible devolver objetos arbitrarios de Python, como modelos entrenados directamente al tiempo de ejecución de Studio. Al igual que [Ejecutar script R][execute-r-script], que tiene la misma limitación, en muchos casos es posible incluir objetos en una matriz de bytes y luego devolverla dentro de una trama de datos.

### <a name="inability-to-customize-python-installation"></a>Incapacidad de personalizar la instalación de Python

Actualmente, la única manera de agregar módulos personalizados de Python es a través del mecanismo de archivo ZIP descrito anteriormente. Aunque esto es factible para módulos pequeños, es complicado para módulos grandes (especialmente módulos con DLL nativas) o un gran número de módulos.

## <a name="next-steps"></a>Pasos siguientes

Para más información, vea el [Centro para desarrolladores de Python](https://azure.microsoft.com/develop/python/).

<!-- Module References -->
[execute-python-script]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-python-script
[execute-r-script]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-r-script