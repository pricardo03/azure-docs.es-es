---
title: 'Tutorial del modelo de regresión: Machine Learning Automatizado'
titleSuffix: Azure Machine Learning service
description: Aprenda a generar un modelo de ML mediante el aprendizaje automático automatizado. Azure Machine Learning puede realizar automáticamente el procesamiento previo de los datos, y la selección del algoritmo y de los hiperparámetros. Después, el modelo final se implementa con Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: nacharya1
ms.author: nilesha
ms.reviewer: trbye
ms.date: 04/11/2019
ms.custom: seodec18
ms.openlocfilehash: bbb9653173925e1443504aa3f2e9c5e6edbfc486
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68371035"
---
# <a name="tutorial-use-automated-machine-learning-to-build-your-regression-model"></a>Tutorial: Uso del aprendizaje automático para crear un modelo de regresión

Este tutorial es la **segunda parte de dos**. En el tutorial anterior, [preparó los datos de taxi de Nueva York para el modelado de regresión](tutorial-data-prep.md).

Ahora, está preparado para comenzar a crear el modelo con Azure Machine Learning Service. En esta parte del tutorial, utilizará los datos preparados y generará automáticamente un modelo de regresión para predecir las tarifas de los taxis. Mediante las funcionalidades del servicio de aprendizaje automático automatizado, defina sus objetivos y restricciones de aprendizaje automático. Inicie el proceso de aprendizaje automático automatizado. A continuación, permita la selección del algoritmo y la optimización de los hiperparámetros. La técnica de ML automatizado recorre en iteración muchas combinaciones de algoritmos e hiperparámetros hasta que encuentra el mejor modelo según su criterio.

![Diagrama de flujo](./media/tutorial-auto-train-models/flow2.png)

En este tutorial, ha aprendido las tareas siguientes:

> [!div class="checklist"]
> * Configurar un entorno de Python e importar los paquetes del SDK.
> * Configurar un área de trabajo de Azure Machine Learning Service.
> * Entrenar automáticamente un modelo de regresión.
> * Ejecutar el modelo de forma local con parámetros personalizados.
> * Explorar los resultados.

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

>[!NOTE]
> El código de este artículo se ha probado con el SDK de Azure Machine Learning, versión 1.0.39.

## <a name="prerequisites"></a>Requisitos previos

Vaya a [Configuración del entorno de desarrollo](#start) para leer los pasos del cuaderno, o utilice las siguientes instrucciones para obtener el cuaderno y ejecutarlo en Azure Notebooks o en su propio servidor de cuadernos. Para ejecutar el cuaderno, necesita:

* [Tutorial sobre la ejecución de la preparación de datos](tutorial-data-prep.md).
* Un servidor de cuadernos de Python 3.6 con lo siguiente instalado:
    * El SDK de Azure Machine Learning para Python con los extras `automl` y `notebooks`
    * `matplotlib`
* El cuaderno del tutorial
* Un área de trabajo de Machine Learning
* El archivo de configuración para el área de trabajo en el mismo directorio que el cuaderno

Obtenga todos estos requisitos previos de cualquiera de las siguientes secciones.

* Uso de un [servidor de cuadernos en la nube en el área de trabajo](#azure) 
* Uso del [propio servidor de Notebook](#server)

### <a name="azure"></a>Uso de un servidor de cuadernos en la nube en el área de trabajo

Es fácil empezar con su propio servidor de cuadernos basado en la nube. El [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) ya está instalado y configurado para el usuario una vez creado este recurso en la nube.

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

* Después de iniciar la página web del cuaderno, ejecute el cuaderno **tutorials/regression-part2-automated-ml.ipynb**.

### <a name="server"></a>Uso del propio servidor de Jupyter Notebook

Siga estos pasos para crear un servidor de Jupyter Notebook local en su equipo.  Asegúrese de que instalar `matplotlib` y `automl` y `notebooks` adicionales en su entorno.

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Después de completar los pasos siguientes, ejecute el cuaderno **tutorials/regression-part2-automated-ml.ipynb**.

## <a name="start"></a>Configuración de su entorno de desarrollo

Toda la configuración para el trabajo de desarrollo puede realizarse en un cuaderno de Python. La configuración incluye las siguientes acciones:

* Instalación del SDK
* La importación de los paquetes de Python
* Configuración de un área de trabajo

### <a name="install-and-import-packages"></a>Instalación e importación de paquetes

Si está siguiendo el tutorial en su propio entorno de Python, use lo siguiente para instalar los paquetes necesarios.

```shell
pip install azureml-sdk[automl,notebooks] matplotlib
```

Importe los paquetes de Python que necesite para este tutorial:

```python
import azureml.core
import pandas as pd
from azureml.core.workspace import Workspace
import logging
import os
```

### <a name="configure-workspace"></a>Configuración del área de trabajo

Cree un objeto de área de trabajo desde el área de trabajo existente. Un [área de trabajo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) es una clase que acepta la información de recursos y suscripciones de Azure. También crea un recurso en la nube para supervisar y realizar un seguimiento de las ejecuciones del modelo.

`Workspace.from_config()` lee el archivo **config.json** y carga los detalles en un objeto denominado `ws`.  En el resto del código de este tutorial se usa `ws`.

Una vez que tenga un objeto de área de trabajo, especifique un nombre para el experimento. Cree y registre un directorio local con el área de trabajo. El historial de todas las ejecuciones se registra en el experimento especificado y en [Azure Portal](https://portal.azure.com).


```python
ws = Workspace.from_config()
# choose a name for the run history container in the workspace
experiment_name = 'automated-ml-regression'
# project folder
project_folder = './automated-ml-regression'

output = {}
output['SDK version'] = azureml.core.VERSION
output['Subscription ID'] = ws.subscription_id
output['Workspace'] = ws.name
output['Resource Group'] = ws.resource_group
output['Location'] = ws.location
output['Project Directory'] = project_folder
pd.set_option('display.max_colwidth', -1)
pd.DataFrame(data=output, index=['']).T
```

## <a name="explore-data"></a>Exploración de los datos

Use el objeto de flujo de datos que se creó en el tutorial anterior. En resumen, en la parte 1 de este tutorial se limpiaron los datos de NYC Taxi para usarlos en un modelo de Machine Learning. Ahora se usarán distintas características del conjunto de datos y se permitirá un modelo automatizado para crear relaciones entre las características y el precio de la carrera. Abra y ejecute el flujo de datos y revise los resultados:


```python
import azureml.dataprep as dprep

file_path = os.path.join(os.getcwd(), "dflows.dprep")

dflow_prepared = dprep.Dataflow.open(file_path)
dflow_prepared.get_profile()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>type</th>
      <th>Min</th>
      <th>max</th>
      <th>Count</th>
      <th>Falta el recuento</th>
      <th>No falta el recuento</th>
      <th>Falta el porcentaje</th>
      <th>Recuento de errores</th>
      <th>Recuento vacío</th>
      <th>Cuantil 0,1 %</th>
      <th>Cuantil 1 %</th>
      <th>Cuantil 5 %</th>
      <th>Cuantil 25 %</th>
      <th>Cuantil 50 %</th>
      <th>Cuantil 75 %</th>
      <th>Cuantil 95 %</th>
      <th>Cuantil 99 %</th>
      <th>Cuantil 99,9 %</th>
      <th>Media</th>
      <th>Desviación estándar</th>
      <th>Variance</th>
      <th>Asimetría</th>
      <th>Curtosis</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>vendor</th>
      <td>FieldType.STRING</td>
      <td>1</td>
      <td>VTS</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_weekday</th>
      <td>FieldType.STRING</td>
      <td>Viernes</td>
      <td>Miércoles</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>2,90047</td>
      <td>2,69355</td>
      <td>9,72889</td>
      <td>16</td>
      <td>19,3713</td>
      <td>22,6974</td>
      <td>23</td>
      <td>23</td>
      <td>14,2731</td>
      <td>6,59242</td>
      <td>43,46</td>
      <td>-0,693723</td>
      <td>-0,570403</td>
    </tr>
    <tr>
      <th>pickup_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>4,99701</td>
      <td>4,95833</td>
      <td>14,1528</td>
      <td>29,3832</td>
      <td>44,6825</td>
      <td>56,4444</td>
      <td>58,9909</td>
      <td>59</td>
      <td>29,427</td>
      <td>17,4333</td>
      <td>303,921</td>
      <td>0,0120999</td>
      <td>-1,20981</td>
    </tr>
    <tr>
      <th>pickup_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5,28131</td>
      <td>5</td>
      <td>14,7832</td>
      <td>29,9293</td>
      <td>44,725</td>
      <td>56,7573</td>
      <td>59</td>
      <td>59</td>
      <td>29,7443</td>
      <td>17,3595</td>
      <td>301,351</td>
      <td>-0,0252399</td>
      <td>-1,19616</td>
    </tr>
    <tr>
      <th>dropoff_weekday</th>
      <td>FieldType.STRING</td>
      <td>Viernes</td>
      <td>Miércoles</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>dropoff_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>2,57153</td>
      <td>2</td>
      <td>9,58795</td>
      <td>15,9994</td>
      <td>19,6184</td>
      <td>22,8317</td>
      <td>23</td>
      <td>23</td>
      <td>14,2105</td>
      <td>6,71093</td>
      <td>45,0365</td>
      <td>-0,687292</td>
      <td>-0,61951</td>
    </tr>
    <tr>
      <th>dropoff_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5,44383</td>
      <td>4,84694</td>
      <td>14,1036</td>
      <td>28,8365</td>
      <td>44,3102</td>
      <td>56,6892</td>
      <td>59</td>
      <td>59</td>
      <td>29,2907</td>
      <td>17,4108</td>
      <td>303,136</td>
      <td>0,0222514</td>
      <td>-1,2181</td>
    </tr>
    <tr>
      <th>dropoff_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5,07801</td>
      <td>5</td>
      <td>14,5751</td>
      <td>29,5972</td>
      <td>45,4649</td>
      <td>56,2729</td>
      <td>59</td>
      <td>59</td>
      <td>29,772</td>
      <td>17,5337</td>
      <td>307,429</td>
      <td>-0,0212575</td>
      <td>-1,226</td>
    </tr>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>Y</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74,0781</td>
      <td>-73,7459</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74,0578</td>
      <td>-73,9639</td>
      <td>-73,9656</td>
      <td>-73,9508</td>
      <td>-73,9255</td>
      <td>-73,8529</td>
      <td>-73,8302</td>
      <td>-73,8238</td>
      <td>-73,7697</td>
      <td>-73,9123</td>
      <td>0,0503757</td>
      <td>0,00253771</td>
      <td>0,352172</td>
      <td>-0,923743</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.5755</td>
      <td>40.8799</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40,632</td>
      <td>40,7117</td>
      <td>40,7115</td>
      <td>40,7213</td>
      <td>40,7565</td>
      <td>40,8058</td>
      <td>40,8478</td>
      <td>40.8676</td>
      <td>40,8778</td>
      <td>40,7649</td>
      <td>0,0494674</td>
      <td>0,00244702</td>
      <td>0,205972</td>
      <td>-0,777945</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.0857</td>
      <td>-73.7209</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74,0775</td>
      <td>-73,9875</td>
      <td>-73,9882</td>
      <td>-73,9638</td>
      <td>-73,935</td>
      <td>-73,8755</td>
      <td>-73,8125</td>
      <td>-73,7759</td>
      <td>-73,7327</td>
      <td>-73,9202</td>
      <td>0,0584627</td>
      <td>0,00341789</td>
      <td>0,623622</td>
      <td>-0,262603</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.5835</td>
      <td>40.8797</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40,5973</td>
      <td>40,6928</td>
      <td>40,6911</td>
      <td>40,7226</td>
      <td>40,7567</td>
      <td>40,7918</td>
      <td>40,8495</td>
      <td>40.868</td>
      <td>40,8787</td>
      <td>40,7583</td>
      <td>0,0517399</td>
      <td>0,00267701</td>
      <td>0,0390404</td>
      <td>-0,203525</td>
    </tr>
    <tr>
      <th>passengers</th>
      <td>FieldType.DECIMAL</td>
      <td>1</td>
      <td>6</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>6</td>
      <td>2,39249</td>
      <td>1,83197</td>
      <td>3,3561</td>
      <td>0,763144</td>
      <td>-1,23467</td>
    </tr>
    <tr>
      <th>distancia</th>
      <td>FieldType.DECIMAL</td>
      <td>0,01</td>
      <td>32,34</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0,0108744</td>
      <td>0,743898</td>
      <td>0,738194</td>
      <td>1,243</td>
      <td>2,40168</td>
      <td>4,74478</td>
      <td>10,5136</td>
      <td>14,9011</td>
      <td>21,8035</td>
      <td>3,5447</td>
      <td>3,2943</td>
      <td>10,8524</td>
      <td>1,91556</td>
      <td>4,99898</td>
    </tr>
    <tr>
      <th>cost</th>
      <td>FieldType.DECIMAL</td>
      <td>0,1</td>
      <td>88</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>6148,0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2,33837</td>
      <td>5,00491</td>
      <td>5</td>
      <td>6,93129</td>
      <td>10,524</td>
      <td>17,4811</td>
      <td>33,2343</td>
      <td>50,0093</td>
      <td>63,1753</td>
      <td>13,6843</td>
      <td>9,66571</td>
      <td>93,426</td>
      <td>1,78518</td>
      <td>4,13972</td>
    </tr>
  </tbody>
</table>

Puede preparar los datos para el experimento agregando columnas a `dflow_x` para que sean características para la creación del modelo. Puede definir `dflow_y` para que sea nuestro valor de predicción **costo**:

```python
dflow_X = dflow_prepared.keep_columns(
    ['pickup_weekday', 'pickup_hour', 'distance', 'passengers', 'vendor'])
dflow_y = dflow_prepared.keep_columns('cost')
```

### <a name="split-the-data-into-train-and-test-sets"></a>Dividir los datos en conjuntos de entrenamiento y prueba

Ahora puede dividir los datos en conjuntos de entrenamiento y prueba mediante la función `train_test_split` de la biblioteca `sklearn`. Esta función segrega los datos entre el conjunto de datos "x", **características**, para el entrenamiento del modelo y el conjunto de datos "y", **valores a predecir**, para la realización de pruebas. El parámetro `test_size` determina el porcentaje de datos que se va a asignar a las pruebas. El parámetro `random_state` establece un valor de inicialización para el generador aleatorio, de forma que las divisiones de entrenamiento o prueba son siempre deterministas:

```python
from sklearn.model_selection import train_test_split

x_df = dflow_X.to_pandas_dataframe()
y_df = dflow_y.to_pandas_dataframe()

x_train, x_test, y_train, y_test = train_test_split(
    x_df, y_df, test_size=0.2, random_state=223)
# flatten y_train to 1d array
y_train.values.flatten()
```

El propósito de este paso es tener puntos de datos para probar el modelo terminado que no se hayan usado para entrenar el modelo y así poder medir la precisión real. En otras palabras, un modelo bien entrenado debe ser capaz predecir de manera precisa a partir de datos que aún no ha visto. Ahora tiene los paquetes necesarios y los datos preparados para el entrenamiento automático del modelo.

## <a name="automatically-train-a-model"></a>Entrenamiento automático de un modelo

Para entrenar automáticamente un modelo, realice los pasos siguientes:
1. Defina la configuración de la ejecución del experimento. Asocie los datos de entrenamiento a la configuración y modifíquela para controlar el entrenamiento.
1. Envíe el experimento para la optimización del modelo. Después de enviar el experimento, el proceso se itera mediante distintos algoritmos de aprendizaje automático y configuraciones de hiperparámetros, de conformidad con las restricciones definidas. Elige el modelo de ajuste perfecto mediante la optimización de una métrica de precisión.

### <a name="define-settings-for-autogeneration-and-tuning"></a>Definición de la configuración para la generación automática y la optimización

Defina los parámetros del experimento y la configuración de los modelos para la generación automática y la optimización. Vea la lista completa de [valores](how-to-configure-auto-train.md). El envío del experimento con esta configuración predeterminada tardará unos 10-15 min, pero si desea que la ejecución sea más breve, puede reducir `iterations` o `iteration_timeout_minutes`.


|Propiedad| Valor en este tutorial |DESCRIPCIÓN|
|----|----|---|
|**iteration_timeout_minutes**|10|Límite de tiempo en minutos para cada iteración. Reduzca este valor para reducir el tiempo de ejecución total.|
|**iterations**|30|Número de iteraciones. En cada iteración se entrena un nuevo modelo de aprendizaje automático con los datos. Este es el valor principal que afecta al tiempo de ejecución total.|
|**primary_metric**| spearman_correlation | Métrica que desea optimizar. El modelo de ajuste perfecto se elegirá según esta métrica.|
|**preprocess**| True | Mediante el uso de **True**, el experimento puede preprocesar los datos de entrada (al administrar los datos que faltan, convertir texto a numérico, etc.).|
|**verbosity**| logging.INFO | Controla el nivel de registro.|
|**n_cross_validations**|5|Número de divisiones de la validación cruzada que se realizarán cuando no se especifiquen datos de validación.|



```python
automl_settings = {
    "iteration_timeout_minutes": 10,
    "iterations": 30,
    "primary_metric": 'spearman_correlation',
    "preprocess": True,
    "verbosity": logging.INFO,
    "n_cross_validations": 5
}
```

Use la configuración de entrenamiento definida como parámetro en un objeto `AutoMLConfig`. Además, puede especificar los datos de entrenamiento y el tipo de modelo, `regression` en este caso.

```python
from azureml.train.automl import AutoMLConfig

# local compute
automated_ml_config = AutoMLConfig(task='regression',
                                   debug_log='automated_ml_errors.log',
                                   path=project_folder,
                                   X=x_train.values,
                                   y=y_train.values.flatten(),
                                   **automl_settings)
```

### <a name="train-the-automatic-regression-model"></a>Entrenamiento del modelo de regresión automática

Inicie el experimento para ejecutarlo localmente. Pase el objeto `automated_ml_config` definido para el experimento. Establezca la salida en `True` para ver el progreso durante el experimento:


```python
from azureml.core.experiment import Experiment
experiment = Experiment(ws, experiment_name)
local_run = experiment.submit(automated_ml_config, show_output=True)
```

La salida que se muestra se actualiza en directo con el experimento. Para cada iteración, verá el tipo de modelo, la duración de la ejecución y la precisión del entrenamiento. El campo `BEST` realiza el seguimiento de la mejor puntuación de entrenamiento en ejecución en función del tipo de métrica.

    Parent Run ID: AutoML_02778de3-3696-46e9-a71b-521c8fca0651
    *******************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    *******************************************************************************************

     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   MaxAbsScaler ExtremeRandomTrees                0:00:08       0.9447    0.9447
             1   StandardScalerWrapper GradientBoosting         0:00:09       0.9536    0.9536
             2   StandardScalerWrapper ExtremeRandomTrees       0:00:09       0.8580    0.9536
             3   StandardScalerWrapper RandomForest             0:00:08       0.9147    0.9536
             4   StandardScalerWrapper ExtremeRandomTrees       0:00:45       0.9398    0.9536
             5   MaxAbsScaler LightGBM                          0:00:08       0.9562    0.9562
             6   StandardScalerWrapper ExtremeRandomTrees       0:00:27       0.8282    0.9562
             7   StandardScalerWrapper LightGBM                 0:00:07       0.9421    0.9562
             8   MaxAbsScaler DecisionTree                      0:00:08       0.9526    0.9562
             9   MaxAbsScaler RandomForest                      0:00:09       0.9355    0.9562
            10   MaxAbsScaler SGD                               0:00:09       0.9602    0.9602
            11   MaxAbsScaler LightGBM                          0:00:09       0.9553    0.9602
            12   MaxAbsScaler DecisionTree                      0:00:07       0.9484    0.9602
            13   MaxAbsScaler LightGBM                          0:00:08       0.9540    0.9602
            14   MaxAbsScaler RandomForest                      0:00:10       0.9365    0.9602
            15   MaxAbsScaler SGD                               0:00:09       0.9602    0.9602
            16   StandardScalerWrapper ExtremeRandomTrees       0:00:49       0.9171    0.9602
            17   SparseNormalizer LightGBM                      0:00:08       0.9191    0.9602
            18   MaxAbsScaler DecisionTree                      0:00:08       0.9402    0.9602
            19   StandardScalerWrapper ElasticNet               0:00:08       0.9603    0.9603
            20   MaxAbsScaler DecisionTree                      0:00:08       0.9513    0.9603
            21   MaxAbsScaler SGD                               0:00:08       0.9603    0.9603
            22   MaxAbsScaler SGD                               0:00:10       0.9602    0.9603
            23   StandardScalerWrapper ElasticNet               0:00:09       0.9603    0.9603
            24   StandardScalerWrapper ElasticNet               0:00:09       0.9603    0.9603
            25   MaxAbsScaler SGD                               0:00:09       0.9603    0.9603
            26   TruncatedSVDWrapper ElasticNet                 0:00:09       0.9602    0.9603
            27   MaxAbsScaler SGD                               0:00:12       0.9413    0.9603
            28   StandardScalerWrapper ElasticNet               0:00:07       0.9603    0.9603
            29    Ensemble                                      0:00:38       0.9622    0.9622

## <a name="explore-the-results"></a>Exploración de los resultados

Explore los resultados del entrenamiento automático con un widget de Jupyter o examinando el historial del experimento.

### <a name="option-1-add-a-jupyter-widget-to-see-results"></a>Opción 1: Agregar un widget de Jupyter para ver los resultados

Si va a usar una instancia de Jupyter Notebook, use este [widget de Jupyter](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) para ver un grafo y una tabla de todos los resultados:


```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```

![Detalles de la ejecución del widget de Jupyter](./media/tutorial-auto-train-models/automl-dash-output.png)
![Trazado del widget de Jupyter](./media/tutorial-auto-train-models/automl-chart-output.png)

Los mismos resultados se almacenan en el área de trabajo.  Puede obtener un vínculo a los resultados de la ejecución:

```
local_run.get_portal_url()
```
  

### <a name="option-2-get-and-examine-all-run-iterations-in-python"></a>Opción 2: Obtener y examinar todas las iteraciones de ejecución de Python

También puede recuperar el historial de cada experimento y explorar las métricas individuales de cada ejecución de iteración. Al examinar el valor RMSE (root_mean_squared_error) de cada ejecución de modelo individual, verá que la mayoría de iteraciones predicen el costo justo del taxi con un margen razonable (3-4 USD).

```python
children = list(local_run.get_children())
metricslist = {}
for run in children:
    properties = run.get_properties()
    metrics = {k: v for k, v in run.get_metrics().items()
               if isinstance(v, float)}
    metricslist[int(properties['iteration'])] = metrics

rundata = pd.DataFrame(metricslist).sort_index(1)
rundata
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
      <th>5</th>
      <th>6</th>
      <th>7</th>
      <th>8</th>
      <th>9</th>
      <th>...</th>
      <th>20</th>
      <th>21</th>
      <th>22</th>
      <th>23</th>
      <th>24</th>
      <th>25</th>
      <th>26</th>
      <th>27</th>
      <th>28</th>
      <th>29</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>explained_variance</th>
      <td>0,811037</td>
      <td>0,880553</td>
      <td>0,398582</td>
      <td>0,776040</td>
      <td>0,663869</td>
      <td>0,875911</td>
      <td>0,115632</td>
      <td>0,586905</td>
      <td>0,851911</td>
      <td>0,793964</td>
      <td>...</td>
      <td>0,850023</td>
      <td>0,883603</td>
      <td>0,883704</td>
      <td>0,880797</td>
      <td>0,881564</td>
      <td>0,883708</td>
      <td>0,881826</td>
      <td>0,585377</td>
      <td>0,883123</td>
      <td>0,886817</td>
    </tr>
    <tr>
      <th>mean_absolute_error</th>
      <td>2,189444</td>
      <td>1,500412</td>
      <td>5,480531</td>
      <td>2,626316</td>
      <td>2,973026</td>
      <td>1,550199</td>
      <td>6,383868</td>
      <td>4,414241</td>
      <td>1,743328</td>
      <td>2,294601</td>
      <td>...</td>
      <td>1,797402</td>
      <td>1,415815</td>
      <td>1,418167</td>
      <td>1,578617</td>
      <td>1,559427</td>
      <td>1,413042</td>
      <td>1,551698</td>
      <td>4,069196</td>
      <td>1,505795</td>
      <td>1,430957</td>
    </tr>
    <tr>
      <th>median_absolute_error</th>
      <td>1,438417</td>
      <td>0,850899</td>
      <td>4,579662</td>
      <td>1,765210</td>
      <td>1,594600</td>
      <td>0,869883</td>
      <td>4,266450</td>
      <td>3,627355</td>
      <td>0,954992</td>
      <td>1,361014</td>
      <td>...</td>
      <td>0,973634</td>
      <td>0,774814</td>
      <td>0,797269</td>
      <td>1,147234</td>
      <td>1,116424</td>
      <td>0,783958</td>
      <td>1,098464</td>
      <td>2,709027</td>
      <td>1,003728</td>
      <td>0,851724</td>
    </tr>
    <tr>
      <th>normalized_mean_absolute_error</th>
      <td>0,024908</td>
      <td>0,017070</td>
      <td>0,062350</td>
      <td>0,029878</td>
      <td>0,033823</td>
      <td>0,017636</td>
      <td>0,072626</td>
      <td>0,050219</td>
      <td>0,019833</td>
      <td>0,026105</td>
      <td>...</td>
      <td>0,020448</td>
      <td>0,016107</td>
      <td>0,016134</td>
      <td>0,017959</td>
      <td>0,017741</td>
      <td>0,016076</td>
      <td>0,017653</td>
      <td>0,046293</td>
      <td>0,017131</td>
      <td>0,016279</td>
    </tr>
    <tr>
      <th>normalized_median_absolute_error</th>
      <td>0,016364</td>
      <td>0,009680</td>
      <td>0,052101</td>
      <td>0,020082</td>
      <td>0,018141</td>
      <td>0,009896</td>
      <td>0,048538</td>
      <td>0,041267</td>
      <td>0,010865</td>
      <td>0,015484</td>
      <td>...</td>
      <td>0,011077</td>
      <td>0,008815</td>
      <td>0,009070</td>
      <td>0,013052</td>
      <td>0,012701</td>
      <td>0,008919</td>
      <td>0,012497</td>
      <td>0,030819</td>
      <td>0,011419</td>
      <td>0,009690</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_error</th>
      <td>0,047968</td>
      <td>0,037882</td>
      <td>0,085572</td>
      <td>0,052282</td>
      <td>0,065809</td>
      <td>0,038664</td>
      <td>0,109401</td>
      <td>0,071104</td>
      <td>0,042294</td>
      <td>0,049967</td>
      <td>...</td>
      <td>0,042565</td>
      <td>0,037685</td>
      <td>0,037557</td>
      <td>0,037643</td>
      <td>0,037513</td>
      <td>0,037560</td>
      <td>0,037465</td>
      <td>0,072077</td>
      <td>0,037249</td>
      <td>0,036716</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_log_error</th>
      <td>0,055353</td>
      <td>0,045000</td>
      <td>0,110219</td>
      <td>0,065633</td>
      <td>0,063589</td>
      <td>0,044412</td>
      <td>0,123433</td>
      <td>0,092312</td>
      <td>0,046130</td>
      <td>0,055243</td>
      <td>...</td>
      <td>0,046540</td>
      <td>0,041804</td>
      <td>0,041771</td>
      <td>0,045175</td>
      <td>0,044628</td>
      <td>0,041617</td>
      <td>0,044405</td>
      <td>0,079651</td>
      <td>0,042799</td>
      <td>0,041530</td>
    </tr>
    <tr>
      <th>r2_score</th>
      <td>0,810900</td>
      <td>0,880328</td>
      <td>0,398076</td>
      <td>0,775957</td>
      <td>0,642812</td>
      <td>0,875719</td>
      <td>0,021603</td>
      <td>0,586514</td>
      <td>0,851767</td>
      <td>0,793671</td>
      <td>...</td>
      <td>0,849809</td>
      <td>0,880142</td>
      <td>0,880952</td>
      <td>0,880586</td>
      <td>0,881347</td>
      <td>0,880887</td>
      <td>0,881613</td>
      <td>0,548121</td>
      <td>0,882883</td>
      <td>0,886321</td>
    </tr>
    <tr>
      <th>root_mean_squared_error</th>
      <td>4,216362</td>
      <td>3,329810</td>
      <td>7,521765</td>
      <td>4,595604</td>
      <td>5,784601</td>
      <td>3,398540</td>
      <td>9,616354</td>
      <td>6,250011</td>
      <td>3,717661</td>
      <td>4,392072</td>
      <td>...</td>
      <td>3,741447</td>
      <td>3,312533</td>
      <td>3,301242</td>
      <td>3,308795</td>
      <td>3,297389</td>
      <td>3,301485</td>
      <td>3,293182</td>
      <td>6,335581</td>
      <td>3,274209</td>
      <td>3,227365</td>
    </tr>
    <tr>
      <th>root_mean_squared_log_error</th>
      <td>0,243184</td>
      <td>0,197702</td>
      <td>0,484227</td>
      <td>0,288349</td>
      <td>0,279367</td>
      <td>0,195116</td>
      <td>0,542281</td>
      <td>0,405559</td>
      <td>0,202666</td>
      <td>0,242702</td>
      <td>...</td>
      <td>0,204464</td>
      <td>0,183658</td>
      <td>0,183514</td>
      <td>0,198468</td>
      <td>0,196067</td>
      <td>0,182836</td>
      <td>0,195087</td>
      <td>0,349935</td>
      <td>0,188031</td>
      <td>0,182455</td>
    </tr>
    <tr>
      <th>spearman_correlation</th>
      <td>0,944743</td>
      <td>0,953618</td>
      <td>0,857965</td>
      <td>0,914703</td>
      <td>0,939846</td>
      <td>0,956159</td>
      <td>0,828187</td>
      <td>0,942069</td>
      <td>0,952581</td>
      <td>0,935477</td>
      <td>...</td>
      <td>0,951287</td>
      <td>0,960335</td>
      <td>0,960195</td>
      <td>0,960279</td>
      <td>0,960288</td>
      <td>0,960323</td>
      <td>0,960161</td>
      <td>0,941254</td>
      <td>0,960293</td>
      <td>0,962158</td>
    </tr>
    <tr>
      <th>spearman_correlation_max</th>
      <td>0,944743</td>
      <td>0,953618</td>
      <td>0,953618</td>
      <td>0,953618</td>
      <td>0,953618</td>
      <td>0,956159</td>
      <td>0,956159</td>
      <td>0,956159</td>
      <td>0,956159</td>
      <td>0,956159</td>
      <td>...</td>
      <td>0,960303</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,960335</td>
      <td>0,962158</td>
    </tr>
  </tbody>
</table>
<p>12 filas × 30 columnas</p>
</div>

## <a name="retrieve-the-best-model"></a>Recuperación del mejor modelo

Seleccione la mejor canalización en nuestras iteraciones. El método `get_output` en `automl_classifier` devuelve la mejor ejecución y el modelo ajustado de la última invocación de ajuste. Mediante el uso de las sobrecargas en `get_output`, puede recuperar la mejor ejecución y el modelo ajustado para cualquier métrica registrada o para una iteración concreta.

```python
best_run, fitted_model = local_run.get_output()
print(best_run)
print(fitted_model)
```

## <a name="test-the-best-model-accuracy"></a>Prueba de la precisión del mejor modelo

Use el mejor modelo para ejecutar predicciones en el conjunto de datos de prueba para predecir las tarifas del taxi. La función `predict` usa el mejor modelo y predice los valores de "y", **precio del recorrido**, a partir del conjunto de datos `x_test`. Imprima los 10 primeros valores de costo predichos a partir de `y_predict`:

```python
y_predict = fitted_model.predict(x_test.values)
print(y_predict[:10])
```

Cree un gráfico de dispersión para visualizar los valores de predicción de costos en comparación con los valores de costos reales. El siguiente código utiliza la característica `distance` como el eje x y el recorrido `cost` como el eje y. A fin de comparar la varianza del costo previsto en cada valor de distancia recorrida, los cien primeros valores de costos reales y previstos se crean como series independientes. Examinar el gráfico muestra que la relación entre distancia y costo es casi lineal, y los valores de los costos previstos, en la mayoría de los casos, están muy próximos a valores de costo real para la misma distancia recorrida.

```python
%matplotlib inline

import matplotlib.pyplot as plt

fig = plt.figure(figsize=(14, 10))
ax1 = fig.add_subplot(111)

distance_vals = [x[4] for x in x_test.values]
y_actual = y_test.values.flatten().tolist()

ax1.scatter(distance_vals[:100], y_predict[:100],
            s=18, c='b', marker="s", label='Predicted')
ax1.scatter(distance_vals[:100], y_actual[:100],
            s=18, c='r', marker="o", label='Actual')

ax1.set_xlabel('distance (mi)')
ax1.set_title('Predicted and Actual Cost/Distance')
ax1.set_ylabel('Cost ($)')

plt.legend(loc='upper left', prop={'size': 12})
plt.rcParams.update({'font.size': 14})
plt.show()
```

![Gráfico de dispersión de predicción](./media/tutorial-auto-train-models/automl-scatter-plot.png)

Calcule el valor `root mean squared error` de los resultados. Use el dataframe `y_test`. Conviértalo en una lista para compararla con los valores predichos. La función `mean_squared_error` toma dos matrices de valores y calcula el error medio al cuadrado entre ellos. Al tomar la raíz cuadrada del resultado, se produce un error en las mismas unidades que la variable y, **costo**. Indica aproximadamente cuánto se alejan las predicciones de la tarifa del taxi de las reales:

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

    3.2204936862688798

Ejecute el siguiente código para calcular el error medio absoluto porcentual (MAPE) usando los conjuntos de datos `y_actual` y `y_predict` completos. Esta métrica calcula una diferencia absoluta entre cada valor predicho y real, y suma todas las diferencias. A continuación, expresa esa suma como porcentaje del total de los valores reales:

```python
sum_actuals = sum_errors = 0

for actual_val, predict_val in zip(y_actual, y_predict):
    abs_error = actual_val - predict_val
    if abs_error < 0:
        abs_error = abs_error * -1

    sum_errors = sum_errors + abs_error
    sum_actuals = sum_actuals + actual_val

mean_abs_percent_error = sum_errors / sum_actuals
print("Model MAPE:")
print(mean_abs_percent_error)
print()
print("Model Accuracy:")
print(1 - mean_abs_percent_error)
```

    Model MAPE:
    0.10545153869569586

    Model Accuracy:
    0.8945484613043041

A partir de las métricas de precisión de la predicción final, verá que el modelo es bastante bueno en la predicción de las tarifas de taxi a partir de las características del conjunto de datos, normalmente en un margen de +/-3,00 USD. El proceso tradicional de desarrollo de modelos de aprendizaje automático consume muchos recursos y requiere conocimiento del dominio y una inversión de tiempo significativa para ejecutar y comparar los resultados de docenas de modelos. El uso del aprendizaje automático automatizado es una manera estupenda de probar muchos modelos distintos para el escenario rápidamente.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de aprendizaje automático, ha realizado las tareas siguientes:

> [!div class="checklist"]
> * Ha configurado un área de trabajo y ha preparado datos para un experimento.
> * Ha realizado un entrenamiento mediante un modelo de regresión automatizado localmente con parámetros personalizados.
> * Ha explorado y revisado los resultados del entrenamiento.

[Implemente el modelo](tutorial-deploy-models-with-aml.md) con Azure Machine Learning.
