---
title: 'Importar datos: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo importar datos en el servicio de Azure Machine Learning para cargar datos en un experimento de machine learning desde servicios de datos en la nube existente.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ed51c4e7b6c7d226c7827d1ba00bc96a7be1e6b0
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028311"
---
# <a name="import-data-module"></a>Módulo de importación de datos

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para cargar datos en un experimento de machine learning desde servicios de datos en la nube existente.  
El módulo ahora cuenta con un Asistente para ayudarle a elegir una opción de almacenamiento y seleccionar de entre las suscripciones existentes y las cuentas para configurar rápidamente todas las opciones. ¿Necesidad de editar una conexión de datos existente? No hay problema; el asistente carga todos los detalles de configuración anterior para que no tiene que volver a empezar desde cero. 
  
Después de definir los datos que desee y conectarse al origen, [importar datos](./import-data.md) deduce el tipo de datos de cada columna basándose en los valores que contiene y carga los datos en el área de trabajo de Azure Machine Learning. La salida de [importar datos](./import-data.md) es un conjunto de datos que puede utilizarse con todos los experimentos.

  
Si cambian los datos de origen, puede actualizar el conjunto de datos y agregar nuevos datos volviendo a [importar datos](./import-data.md). Sin embargo, si no desea volver a leer desde el origen cada vez que ejecute el experimento, seleccione el **usar en caché de resultados** opción en TRUE. Cuando se selecciona esta opción, el módulo comprueba si el experimento se ha ejecutado anteriormente con el mismo origen y las mismas opciones de entrada. Si se encuentra una ejecución anterior, se utilizan los datos en la memoria caché, en lugar de volver a cargar los datos desde el origen.
 

## <a name="data-sources"></a>Orígenes de datos

El módulo de importación de datos admite los siguientes orígenes de datos. Haga clic en los vínculos para obtener instrucciones detalladas y ejemplos del uso de cada origen de datos. 
 
Si no está seguro de cómo o dónde debe almacenar los datos, consulte a esta guía para escenarios comunes de datos en el proceso de ciencia de datos:  [Escenarios para análisis avanzado en Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-plan-sample-scenarios). 


|Origen de datos| Usar con|
|-----------|-----------|  
|[Dirección URL Web a través de HTTP](./import-from-web-url-via-http.md)|Obtención de datos que se hospedan en una dirección URL web que usa HTTP y que se ha proporcionado en los formatos CSV, TSV, ARFF o SvmLight|  
|[Importación de Azure Blob Storage](./import-from-azure-blob-storage.md) |Obtener los datos que se almacenan en Azure blob service|  

## <a name="how-to-use-import-data"></a>Importar datos de uso
 
1. Agregar el **importar datos** módulo al experimento. Puede encontrar este módulo en el **datos de entrada y salida** categoría en la interfaz.

2. Haga clic en **iniciar Asistente de importación de datos** para configurar el origen de datos mediante un asistente.

    El asistente obtiene el nombre de cuenta y las credenciales y le ayudan a configurar otras opciones. Si va a editar una configuración existente, primero carga los valores actuales.

3. Si no desea usar el asistente, haga clic en **origen de datos**y elija el tipo de almacenamiento en nube que se están leyendo. 

    Configuración adicional depende del tipo de almacenamiento que elija, y si el almacenamiento está protegido o no. Es posible que deba proporcionar el nombre de cuenta, tipo de archivo o las credenciales. Algunos orígenes no requieren autenticación; en otros casos, es posible que necesita saber el nombre de cuenta, una clave o el nombre del contenedor.

4. Seleccione el **usar en caché de resultados** opción si desea almacenar en caché el conjunto de datos para su reutilización en las ejecuciones sucesivas.

    Suponiendo que no ha habido ningún otro cambio en los parámetros del módulo, el experimento de carga a la hora de datos solo la primera se ejecuta el módulo y, a partir de entonces usa una versión almacenada en caché del conjunto de datos.

    Desactive esta opción si tiene que volver a cargar los datos cada vez que ejecute el experimento.

5. Ejecute el experimento.

    Al importar datos carga los datos en la interfaz, deduce el tipo de datos de cada columna basándose en los valores que contiene, numéricos o de categorías.

    - Si el encabezado está presente, el encabezado se utiliza para asignar a las columnas del conjunto de datos de salida.

    - Si no hay ningún encabezado de columna existente en los datos, se generan nuevos nombres de columna con el formato col1, col2... , coln *.

## <a name="results"></a>Results

Una vez completada la importación, haga clic en el conjunto de datos de salida y seleccione **visualizar** para ver si los datos se importan correctamente.

Si desea guardar los datos para su reutilización, en lugar de importar un nuevo conjunto de datos cada vez que se ejecute el experimento, haga clic en la salida y seleccione **Guardar como conjunto de datos**. Elija un nombre para el conjunto de datos. El conjunto de datos guardado conserva los datos en el momento de guardar y datos no se actualizan cuando se vuelve a ejecutar el experimento, incluso si cambia el conjunto de datos en el experimento. Esto puede ser útil para tomar instantáneas de datos.

Después de importar los datos, es posible que tenga algunos preparativos adicionales para el modelado y análisis:


- Use [editar metadatos](./edit-metadata.md) para cambiar los nombres de columna, para controlar una columna como un tipo de datos diferente, o para indicar que algunas columnas son las etiquetas o las características.

- Usar [Select Columns in Dataset](./select-columns-in-dataset.md) para seleccionar un subconjunto de columnas para transformar o usar en el modelado. Las columnas transformadas o quitadas pueden fácilmente volver a unirse al conjunto de datos original mediante el uso de la [agregar columnas](./add-columns.md) módulo.  

- Use [partición y ejemplo](./partition-and-sample.md) para dividir el conjunto de datos, realizar un muestreo u obtener las primeras n filas.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 