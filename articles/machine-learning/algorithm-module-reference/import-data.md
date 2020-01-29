---
title: 'Importación de datos: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Importación de datos en Azure Machine Learning para cargar datos en una canalización de aprendizaje automático desde servicios de datos en la nube existentes.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 161c6816bbef31142c576f52fd122d9dd8af7883
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76546663"
---
# <a name="import-data-module"></a>Módulo Importación de datos

En este artículo se describe un módulo del diseñador de Azure Machine Learning (versión preliminar).

Use este módulo para cargar datos en una canalización de aprendizaje automático desde los servicios de datos en la nube existentes. 

> [!Note]
> Toda la funcionalidad proporcionada por este módulo se puede realizar mediante el **almacén de datos** y los **y conjuntos de datos** en la página de aterrizaje del área de trabajo. Se recomienda usar el **almacén de datos** y el **conjunto de datos** que incluye características adicionales, como la supervisión de datos. Para obtener más información, vea los artículos sobre [cómo obtener acceso a datos](../how-to-access-data.md) y [cómo registrar conjuntos de datos](../how-to-create-register-datasets.md).
> Después de registrar un conjunto de datos, puede encontrarlo en la categoría **Conjuntos de datos** -> **My datasets** (Mis conjuntos de datos) en la interfaz del diseñador. Este módulo está reservado para los usuarios de Studio (clásico) para una experiencia familiar. 
>

El módulo **Importación de datos** admite la lectura de datos de los siguientes orígenes:

- URL mediante HTTP
- Almacenamientos en la nube de Azure a través de [**almacenes de información**](../how-to-access-data.md)
    - Azure Blob Container
    - Recurso compartido de archivos de Azure
    - Azure Data Lake
    - Azure Data Lake Gen2
    - Azure SQL Database
    - Azure PostgreSQL    

Antes de usar el almacenamiento en la nube, debe registrar un almacén de datos en el área de trabajo de Azure Machine Learning. Para obtener más información, consulte [Cómo acceder a datos](../how-to-access-data.md). 

Después de definir los datos que desee y conectarse al origen, **[Importación de datos](./import-data.md)** deduce el tipo de datos de cada columna basándose en los valores que contiene y carga los datos en la canalización del diseñador. La salida de **Importación de datos** es un conjunto de datos que puede utilizarse con todas las canalizaciones del diseñador.

Si cambian los datos de origen, puede actualizar el conjunto de datos y agregar nuevos datos volviendo a ejecutar la [Importación de datos](./import-data.md). Sin embargo, si no desea volver a leer el origen cada vez que ejecute la canalización, establezca la opción **Use cached results** (Utilizar resultados almacenados en caché) en TRUE. Cuando se selecciona esta opción, el módulo comprueba si la canalización se ha ejecutado anteriormente con el mismo origen y las mismas opciones de entrada. Si se encuentra una ejecución anterior, se utilizan los datos de la memoria caché, en lugar de volver a cargar los datos desde el origen.

## <a name="how-to-configure-import-data"></a>Procedimiento para configurar la importación de datos

1. Agregue el módulo **Importación de datos** a la canalización. Puede encontrar el módulo en la categoría **Entrada y salida de datos** del diseñador.

1. Haga clic en **Launch Data Import Wizard** (Iniciar el asistente para la importación de datos) para configurar el origen de datos mediante un asistente.

    El asistente obtiene el nombre de cuenta y las credenciales y le ayuda a configurar otras opciones. Si va a editar una configuración existente, primero carga los valores actuales.

1. Seleccione **Origen de datos** y elija el tipo de origen de datos. Podría ser HTTP o almacén de datos.

    Si elige almacén de datos, puede seleccionar los almacenes de datos existentes que ya están registrados en el área de trabajo Azure Machine Learning o crear un nuevo almacén de datos. A continuación, defina la ruta a los datos que se van a importar en el almacén de datos. Puede examinar fácilmente la ruta de acceso haciendo clic en **Examinar ruta**. ![import-data-path](media/module/import-data-path.png)

1. Seleccione el esquema de vista previa para filtrar las columnas que desea incluir. También puede definir la configuración avanzada, como el delimitador en las opciones del análisis.

    ![import-data-preview](media/module/import-data.png)

1. Seleccione la opción **Use cached results** (Utilizar resultados almacenados en caché) si desea almacenar en caché el conjunto de datos para su reutilización en ejecuciones sucesivas.

    Suponiendo que no haya habido ningún otro cambio en los parámetros del módulo, la canalización solo carga los datos la primera vez que se ejecuta el módulo y, a partir de entonces, usa una versión almacenada en caché del conjunto de datos.

    Anule la selección de esta opción si tiene que volver a cargar los datos cada vez que ejecute la canalización.

1. Ejecución de la canalización

    Cuando la importación de datos carga los datos en el diseñador, deduce el tipo de datos de cada columna basándose en los valores que contiene, numéricos o categóricos.

    Si el encabezado está presente, se utiliza para asignar nombres a las columnas del conjunto de datos de salida.

    Si no existe ningún encabezado de columna en los datos, se generan nuevos nombres de columna con el formato col1, col2... , coln *.

## <a name="results"></a>Results

Cuando haya terminado la importación, haga clic en el conjunto de datos de salida y seleccione **Visualizar** para ver si los datos se han importado correctamente.

Si desea guardar los datos para su reutilización, en lugar de importar un nuevo conjunto de datos cada vez que se ejecute la canalización, seleccione el icono **Register dataset** (Registrar conjunto de datos) en la pestaña **Outputs** (Resultados) del panel derecho del módulo. Escriba un nombre para el conjunto de datos. El conjunto de datos guardado conserva los datos del momento en que se guarda y el conjunto de datos no se actualizan cuando se vuelve a ejecutar la canalización, aunque el conjunto de datos cambie en la canalización, lo que puede ser útil para tomar instantáneas de los datos.

Después de importar los datos, es posible que tenga que realizar algunos preparativos adicionales para el modelado y análisis:

- Use [Editar metadatos](./edit-metadata.md) para cambiar los nombres de columna, para controlar una columna como un tipo de datos diferente, o para indicar que algunas columnas son etiquetas o características.

- Use [Seleccionar columnas de conjunto de datos](./select-columns-in-dataset.md) para seleccionar un subconjunto de columnas para transformar o usar en el modelado. Puede volver a unir fácilmente las columnas transformadas o quitadas al conjunto de datos original mediante el módulo [Agregar columnas](./add-columns.md).  

- Use [Partición y muestra](./partition-and-sample.md) para dividir el conjunto de datos, realizar un muestreo u obtener las primeras n filas.

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning. 
