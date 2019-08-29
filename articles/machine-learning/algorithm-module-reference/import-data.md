---
title: 'Importación de datos: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Importación de datos en Azure Machine Learning Service para cargar datos en un experimento de Machine Learning desde servicios de datos en la nube existentes.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: e8522c759efb0cc599bbcf8e7569f6045da571a8
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128784"
---
# <a name="import-data-module"></a>Módulo Importación de datos

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Use este módulo para cargar datos en un experimento de Machine Learning desde los servicios de datos en la nube existentes.  
El módulo ahora cuenta con un asistente para ayudarle a elegir una opción de almacenamiento y seleccionar entre las suscripciones y las cuentas existentes para configurar rápidamente todas las opciones. ¿Necesita editar una conexión de datos existente? Ningún problema, el asistente carga todos los detalles de configuración anteriores para que no tenga que volver a empezar desde cero. 
  
Después de definir los datos que desee y conectarse al origen, [Importación de datos](./import-data.md) deduce el tipo de datos de cada columna basándose en los valores que contiene y carga los datos en el área de trabajo de Azure Machine Learning. La salida de [Importación de datos](./import-data.md) es un conjunto de datos que puede utilizarse con todos los experimentos.

  
Si cambian los datos de origen, puede actualizar el conjunto de datos y agregar nuevos datos volviendo a ejecutar la [Importación de datos](./import-data.md). Sin embargo, si no desea volver a leer el origen cada vez que ejecute el experimento, seleccione TRUE para la opción **Use cached results** (Utilizar resultados almacenados en caché). Cuando se selecciona esta opción, el módulo comprueba si el experimento se ha ejecutado anteriormente con el mismo origen y las mismas opciones de entrada. Si se encuentra una ejecución anterior, se utilizan los datos de la memoria caché, en lugar de volver a cargar los datos desde el origen.
 

## <a name="data-sources"></a>Orígenes de datos

El módulo Importación de datos admite los siguientes orígenes de datos. Haga clic en los vínculos para obtener instrucciones detalladas y ejemplos del uso de cada origen de datos. 
 
Si no está seguro de cómo o dónde debe almacenar los datos, consulte en esta guía los escenarios comunes de datos en el proceso de ciencia de datos:  [Escenarios para análisis avanzado en Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-plan-sample-scenarios). 


|Origen de datos| Uso con|
|-----------|-----------|  
|[Dirección URL web a través de HTTP](./import-from-web-url-via-http.md)|La obtención de datos que se hospedan en una dirección URL web que usa HTTP y que se ha proporcionado en los formatos CSV, TSV, ARFF o SvmLight|  
|[Importación desde Azure Blob Storage](./import-from-azure-blob-storage.md) |La obtención de datos que se almacenan en Azure Blob service|  

## <a name="how-to-use-import-data"></a>Procedimiento para usar la importación de datos
 
1. Agregue el módulo **Importación de datos** al experimento. Puede encontrar el módulo en la categoría **Entrada y salida de datos** de la interfaz.

2. Haga clic en **Launch Data Import Wizard** (Iniciar el asistente para la importación de datos) para configurar el origen de datos mediante un asistente.

    El asistente obtiene el nombre de cuenta y las credenciales y le ayuda a configurar otras opciones. Si va a editar una configuración existente, primero carga los valores actuales.

3. Si no desea usar el asistente, haga clic en **Origen de datos**y elija el tipo de almacenamiento basado en la nube que lee. 

    La configuración adicional depende del tipo de almacenamiento que elija, y si el almacenamiento está protegido o no. Es posible que deba proporcionar el nombre de cuenta, el tipo de archivo o las credenciales. Algunos orígenes no requieren autenticación; en otros casos, es posible que necesite saber el nombre de la cuenta, una clave o el nombre del contenedor.

4. Seleccione la opción **Use cached results** (Utilizar resultados almacenados en caché) si desea almacenar en caché el conjunto de datos para su reutilización en ejecuciones sucesivas.

    Suponiendo que no ha habido ningún otro cambio en los parámetros del módulo, el experimento carga los datos solo la primera vez que se ejecuta el módulo y, a partir de entonces, usa una versión almacenada en caché del conjunto de datos.

    Anule la selección de esta opción si tiene que volver a cargar los datos cada vez que ejecute el experimento.

5. Ejecute el experimento.

    Cuando Importación de datos carga los datos en la interfaz, deduce el tipo de datos de cada columna basándose en los valores que contiene, numéricos o categoricales.

    - Si el encabezado está presente, se utiliza para asignar nombres a las columnas del conjunto de datos de salida.

    - Si no existe ningún encabezado de columna en los datos, se generan nuevos nombres de columna con el formato col1, col2... , coln *.

## <a name="results"></a>Results

Cuando haya terminado la importación, haga clic en el conjunto de datos de salida y seleccione **Visualizar** para ver si los datos se han importado correctamente.

Si desea guardar los datos para su reutilización, en lugar de importar un nuevo conjunto de datos cada vez que se ejecute el experimento, haga clic con el botón derecho en la salida y seleccione **Guardar como conjunto de datos**. Escriba un nombre para el conjunto de datos. El conjunto de datos guardado conserva los datos del momento en que se guarda y los datos no se actualizan cuando se vuelve a ejecutar el experimento, incluso si cambia el conjunto de datos en el experimento. Esto puede ser útil para tomar instantáneas de datos.

Después de importar los datos, es posible que tenga que realizar algunos preparativos adicionales para el modelado y análisis:


- Use [Editar metadatos](./edit-metadata.md) para cambiar los nombres de columna, para controlar una columna como un tipo de datos diferente, o para indicar que algunas columnas son etiquetas o características.

- Use [Seleccionar columnas de conjunto de datos](./select-columns-in-dataset.md) para seleccionar un subconjunto de columnas para transformar o usar en el modelado. Puede volver a unir fácilmente las columnas transformadas o quitadas al conjunto de datos original mediante el módulo [Agregar columnas](./add-columns.md).  

- Use [Partición y muestra](./partition-and-sample.md) para dividir el conjunto de datos, realizar un muestreo u obtener las primeras n filas.

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 