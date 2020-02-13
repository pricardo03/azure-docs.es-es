---
title: 'Importación de datos: referencia para los módulos'
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo usar el módulo Importación de datos en Azure Machine Learning para cargar datos en una canalización de aprendizaje automático desde servicios de datos en la nube existentes.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: e7aa19c1d189eb19237ea85aae1ad2441d7e98b9
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77163201"
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

Si cambian los datos de origen, puede actualizar el conjunto de datos y agregar nuevos datos volviendo a ejecutar la [Importación de datos](./import-data.md).

## <a name="how-to-configure-import-data"></a>Procedimiento para configurar la importación de datos

1. Agregue el módulo **Importación de datos** a la canalización. Puede encontrar el módulo en la categoría **Entrada y salida de datos** del diseñador.

1. Haga clic en **Launch Data Import Wizard** (Iniciar el asistente para la importación de datos) para configurar el origen de datos mediante un asistente.

    El asistente obtiene el nombre de cuenta y las credenciales y le ayuda a configurar otras opciones. Si va a editar una configuración existente, primero carga los valores actuales.

1. Seleccione **Origen de datos** y elija el tipo de origen de datos. Podría ser HTTP o almacén de datos.

    Si elige almacén de datos, puede seleccionar los almacenes de datos existentes que ya están registrados en el área de trabajo Azure Machine Learning o crear un nuevo almacén de datos. A continuación, defina la ruta a los datos que se van a importar en el almacén de datos. Puede examinar fácilmente la ruta de acceso haciendo clic en **Examinar ruta**. ![import-data-path](media/module/import-data-path.png)

1. Seleccione el esquema de vista previa para filtrar las columnas que desea incluir. También puede definir la configuración avanzada, como el delimitador en las opciones del análisis.

    ![import-data-preview](media/module/import-data.png)



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
