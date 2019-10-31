---
title: Importar desde Azure SQL Database
titleSuffix: Azure Machine Learning service
description: Aprenda a usar el módulo Import Data (Importar datos) en la interfaz visual de Azure Machine Learning para obtener datos de una instancia de Azure SQL Database.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 09/09/2019
ms.openlocfilehash: 374776088d93813f39122f2018b00466c55d2b6e
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72694260"
---
# <a name="import-from-azure-sql-database"></a>Importar desde Azure SQL Database

En este artículo se describe cómo usar el módulo [Import Data](import-data.md) (Importar datos) de la interfaz visual de Azure Machine Learning para obtener datos de una instancia de Azure SQL Database.  

Para importar datos de una base de datos, debe especificar el nombre del servidor y el nombre de la base de datos, así como una instrucción SQL que defina la tabla, la vista o la consulta.  

En general, el almacenamiento de datos en bases de datos de Azure es más caro que el uso de tablas o blobs en Azure. También puede haber límites en la cantidad de datos que puede almacenar en una base de datos, en función del tipo de suscripción. Sin embargo, no hay cuotas de transacciones en Azure SQL Database, por lo que esta opción es idónea para un acceso rápido a cantidades más pequeñas de información usada con frecuencia, como tablas de búsqueda de datos o diccionarios de datos.

También se prefiere almacenar datos en una base de datos de Azure si es necesario poder filtrar los datos antes de leerlos, o si se desea guardar las predicciones o las métricas en la base de datos para la creación de informes.

## <a name="how-to-import-data-from-azure-sql-database"></a>Importación de datos desde una base de datos de Azure SQL Database

1. Agregue el módulo [Import Data](import-data.md) (Importar datos) a la canalización. Puede encontrar este módulo en la interfaz visual, en la categoría de entrada y salida de datos.

1. En **Data source** (Origen de datos), seleccione **Azure SQL Database**.

1. Establezca las siguientes opciones específicas a Azure SQL Database.

    **Nombre del servidor de bases de datos**: escriba el nombre del servidor que se genera en Azure. Normalmente tiene el formato `<generated_identifier>.database.windows.net`.

    **Nombre de base de datos**: escriba el nombre de la base de datos en el servidor que acaba de especificar.

    **Nombre de cuenta de usuario del servidor**: escriba el nombre de usuario de una cuenta que tenga permisos de acceso a la base de datos.

    **Contraseña de cuenta de usuario del servidor**: proporcione la contraseña de la cuenta de usuario especificada.

    **Consulta de base de datos**: escriba o pegue una instrucción SQL que describa los datos que desea leer. Valide siempre la instrucción SQL y compruebe los resultados de la consulta con antelación, mediante una herramienta como el Explorador de servidores de Visual Studio o SQL Server Data Tools.

1. Si el conjunto de resultados que lee en Azure Machine Learning no se espera que cambie entre las ejecuciones de la canalización, seleccione la opción **Use cached results** (Utilizar resultados almacenados en caché).

    Cuando se selecciona, si no hay ningún otro cambio en los parámetros del módulo, la canalización carga los datos la primera vez que se ejecuta el módulo y, a partir de entonces, usa una versión almacenada en caché del conjunto de datos.

    Si desea volver a cargar el conjunto datos en cada iteración de la canalización, anule la selección de esta opción. El conjunto de datos se recarga desde el origen cada vez que se cambia cualquier parámetro en [Import Data](import-data.md) (Importar datos).

1. Ejecución de la canalización

    Como [Import Data](import-data.md) (Importar datos) carga los datos en la interfaz visual, también podría realizarse una conversión implícita de tipos, en función de los tipos de datos utilizados en la base de datos de origen.

## <a name="results"></a>Results

Cuando haya terminado la importación, haga clic en el conjunto de datos de salida y seleccione **Visualize** (Visualizar) para ver si los datos se han importado correctamente.

Opcionalmente, puede cambiar el conjunto de datos y sus metadatos con las herramientas de la interfaz visual:

- Use [Edit Metadata](edit-metadata.md) (Editar metadatos) para cambiar los nombres de columna, para convertir una columna en un tipo de datos diferente, o para indicar qué columnas son etiquetas o características.

- Use [Select Columns in Dataset](select-columns-in-dataset.md) (Seleccionar columnas de conjunto de datos) para seleccionar un subconjunto de columnas.

- Use [Partition and Sample](partition-and-sample.md) (Partición y muestra) para separar el conjunto de datos por criterios u obtener las primeras n filas.

## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 
