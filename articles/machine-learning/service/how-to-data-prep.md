---
title: Preparación de los datos con el SDK de preparación de datos de Machine Learning para Python en Azure
description: Obtenga información sobre cómo usar el SDK de preparación de datos para Python de Azure Machine Learning a fin de cargar datos de diversos formatos, transformarlos para que resulten más fáciles de usar, y escribir datos en una ubicación para que los modelos accedan a ellos.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: cforbe
author: cforbe
manager: cgronlun
ms.reviewer: jmartens
ms.date: 11/27/2018
ms.openlocfilehash: 91d0f3565db484504a67a3b6ae0989b9291cd24f
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52446439"
---
# <a name="prepare-data-for-modeling-with-azure-machine-learning"></a>Preparación de los datos para el modelado con Azure Machine Learning

En este artículo, aprenderá sobre los casos de uso y las características únicas del SDK de preparación de datos de Azure Machine Learning. La preparación de datos es la parte más importante del flujo de trabajo de aprendizaje automático. Los datos del mundo real a menudo son incorrectos, son incoherentes o no pueden utilizarse como datos de aprendizaje sin una limpieza y transformación significativas. La corrección de errores y las anomalías en los datos sin procesar, y la creación de nuevas características que sean pertinentes para el problema que está intentando resolver, aumentará la precisión del modelo. El SDK está diseñado para que los usuarios de otras bibliotecas comunes de preparación de datos estén familiarizados con él, a la vez que ofrece ventajas para escenarios clave y mantiene la interoperabilidad con esas otras bibliotecas.

Puede preparar los datos en Python con el [SDK de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk).

## <a name="azure-machine-learning-data-prep-sdk"></a>SDK de preparación de datos de Azure Machine Learning

El [SDK de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk) es una biblioteca de Python que ofrece:

* Transformaciones inteligentes que ahorran tiempo, como la agrupación aproximada, columna derivada por ejemplo, división automática, archivo de lectura inteligente y procesamiento de esquemas desordenadas a la derecha.
* Una única API que funciona con datos pequeños localmente o con datos grandes en la nube, con **pocos o ningún cambio de código**.
* La capacidad de escalar de forma más efectiva en una sola máquina mediante el uso de un enfoque de streaming para procesar los datos, en lugar de cargarlos en la memoria.

### <a name="install-the-sdk"></a>Instalación del SDK

Instale el SDK en su entorno Python mediante el siguiente comando.

```shell
pip install azureml-dataprep
```

Use el código siguiente para importar el paquete.

```python
import azureml.dataprep as dprep
```

### <a name="examples-and-reference"></a>Referencia y ejemplos

Para más información sobre los módulos y funciones de este SDK, consulte los [documentos de referencia del SDK de preparación de datos](https://aka.ms/data-prep-sdk).

En los siguientes ejemplos se destacan algunas de las funcionalidades únicas del SDK, como:

* Detección automática de tipos de archivo
* Transformaciones inteligentes
* Estadísticas de resumen
* Funcionalidad entre entornos


#### <a name="automatic-file-type-detection"></a>Detección automática de tipos de archivo

Use la función `smart_read_file()` para cargar los datos sin tener que especificar el tipo de archivo. Esta función reconoce y analiza automáticamente el tipo de archivo.

```python
dataflow = dprep.smart_read_file(path="<your-file-path>")
```

#### <a name="intelligent-transforms"></a>Transformaciones inteligentes

Utilice el SDK para dividir y derivar columnas tanto por ejemplo como por inferencia para automatizar la ingeniería de características. Suponga que tiene un campo en su objeto de flujo de datos denominado `datetime` con un valor de `2018-09-15 14:30:00`.

Para dividir automáticamente el campo `datetime`, llame a la siguiente función.

```python
new_dataflow = dataflow.split_column_by_example(source_column="datetime")
```

Al no definir el parámetro de ejemplo, la función dividirá automáticamente el campo `datetime` en dos nuevos campos `datetime_1` y `datetime_2`. Los valores resultantes son `2018-09-15` y `14:30:00`, respectivamente. También es posible proporcionar un patrón de ejemplo, y el SDK se encargará de predecir y ejecutar la transformación deseada. Con el mismo objeto `datetime`, el siguiente código creará una nueva columna `datetime_weekday` para el día de la semana basada en el ejemplo proporcionado.

```python
new_dataflow = dataflow.derive_column_by_example(
        source_columns="datetime", 
        new_column_name="datetime_weekday", 
        example_data=[("2009-01-04 10:12:00", "Sunday"), ("2013-08-22 17:00:00", "Thursday")]
    )
```

#### <a name="summary-statistics"></a>Estadísticas de resumen

Puede generar estadísticas de resumen rápidas para un flujo de datos con una línea de código. Este método ofrece una manera cómoda de entender sus datos y cómo se distribuyen.

```python
dataflow.get_profile()
```

Si se llama esta función en un objeto de flujo de datos, se obtendrá una salida como la siguiente tabla.

![Salida de las estadísticas de resumen](./media/concept-data-preparation/output-example.png)

## <a name="multiple-environment-compatibilities"></a>Varias compatibilidades de entorno

El SDK también permite serializar objetos de flujo de datos y abrirlos en *cualquier* entorno Python. El entorno en el que se abre puede ser diferente del entorno en el que se guarda. Esta funcionalidad permite una fácil transferencia entre entornos Python y una rápida integración con los modelos de Azure Machine Learning.

Utilice el siguiente código para guardar los objetos de flujo de datos.

```python
package = dprep.Package([dataflow_1, dataflow_2])
package.save("<your-local-path>")
```

Utilice el siguiente código para reabrir el paquete en cualquier entorno y recuperar una lista de objetos de flujo de datos.

```python
package = dprep.Package.open("<your-local-path>")
dataflow_list = package.dataflows
```

## <a name="data-preparation-pipeline"></a>Canalización de preparación de datos

Para ver ejemplos detallados y código para cada paso de preparación, utilice las siguientes guías paso a paso:

1. [Carga de los datos](how-to-load-data.md), que pueden estar en varios formatos
2. [Transformación](how-to-transform-data.md) en una estructura más fácil de usar
3. [Escritura](how-to-write-data.md) de esos datos en una ubicación accesible para los modelos

![Proceso de preparación de datos](./media/concept-data-preparation/data-prep-process.png)

## <a name="next-steps"></a>Pasos siguientes

Revise un [cuaderno de ejemplo](https://github.com/Microsoft/AMLDataPrepDocs/tree/master/tutorials/getting-started/getting-started.ipynb) de preparación de datos mediante el SDK de preparación de datos de Azure Machine Learning.

[Documentación de referencia](https://docs.microsoft.com/python/api/overview/azure/dataprep/intro?view=azure-dataprep-py) del SSDK de preparación de datos de Azure Machine Learning.
