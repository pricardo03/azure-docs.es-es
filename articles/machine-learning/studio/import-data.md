---
título: Importación de datos en Machine Learning Studio titleSuffix: Azure Machine Learning Studio description: Cómo importar los datos en Azure Machine Learning Studio desde varios orígenes de datos. Obtenga información sobre qué tipos de datos y formatos de datos son compatibles.
services: machine-learning ms.service: machine-learning ms.component: studio ms.topic: article

author: ericlicoding ms.author: amlstudiodocs ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro ms.date: 29/11/2017
---
# <a name="import-your-training-data-into-azure-machine-learning-studio-from-various-data-sources"></a>Importación de datos de entrenamiento en Azure Machine Learning Studio desde varios orígenes de datos

Para usar sus propios datos en Machine Learning Studio para desarrollar y entrenar una solución de análisis predictivo, puede usar los datos de: 

* Un [**archivo local**](import-data-from-local-file.md): cargue los datos locales de antemano desde la unidad de disco duro para crear un módulo de conjunto de datos en su área de trabajo
* [**Orígenes de datos en línea**](import-data-from-online-sources.md): use el módulo [Importar datos][import-data] para acceder a los datos de varios orígenes en línea mientras se ejecuta su experimento
* [**Experimento de Machine Learning Studio**](import-data-from-an-experiment.md): use los datos que se guardaron como un conjunto de datos en Machine Learning Studio
* [**Base de datos de SQL Server local** ](use-data-from-an-on-premises-sql-server.md): use los datos de una base de datos de SQL Server local sin tener que copiar manualmente los datos

> [!NOTE]
> Existe una gran variedad de conjuntos de datos de ejemplo disponibles en Machine Learning Studio que puede usar como datos de aprendizaje. Para obtener información al respecto, consulte [Uso de los conjuntos de datos de ejemplo en Azure Machine Learning Studio](use-sample-datasets.md).
> 
> 

En este tema de introducción también se explica cómo hacer que los datos estén preparados para usarlos en Machine Learning Studio y se describe qué tipos y formatos de datos se admiten.

## <a name="get-data-ready-for-use-in-azure-machine-learning-studio"></a>Preparación de los datos para usarlos en Azure Machine Learning Studio
Machine Learning Studio está diseñado para trabajar con datos rectangulares o tabulares, como datos de texto delimitados o datos estructurados de una base de datos, aunque en algunas circunstancias, es posible usar datos no rectangulares.

Se recomienda que los datos estén relativamente limpios. Es decir, querrá ocuparse de problemas como las cadenas sin comillas antes de cargar los datos en su experimento.

Sin embargo, hay módulos disponibles en Machine Learning Studio que le permitirán manipular levemente los datos en el experimento. Dependiendo de los algoritmos de aprendizaje automático que usará, es posible que deba decidir cómo controlar los problemas estructurales de los datos, como valores que faltan y datos esparcidos y existen módulos que pueden ayudar en esto. Observe la sección **Transformación de datos** de la paleta de módulos para los módulos que realizan estas funciones.

En cualquier momento del experimento puede ver o descargar los datos que genera un módulo con un clic en el puerto de salida. Dependiendo del módulo, es posible que haya distintas opciones de descarga disponibles, o bien que se puedan visualizar los datos dentro del explorador web en Machine Learning Studio.

## <a name="data-formats-and-data-types-supported"></a>Tipos y formatos de datos admitidos
Puede importar diversos tipos de datos al experimento, dependiendo del mecanismo que usa para importar los datos y de dónde provienen estos:

* Texto sin formato (.txt)
* Valores separados por coma (CSV) con un encabezado (.csv) o sin encabezado (.nh.csv)
* Valores separados con tabulaciones (TSV) con un encabezado (.tsv) o sin encabezado (.nh.tsv)
* Archivo de Excel
* Tabla de Azure
* Tabla de Hive
* Tabla de Base de datos SQL
* Valores de OData
* Datos SVMLight (.svmlight) (consulte la [definición de SVMLight](http://svmlight.joachims.org/) para obtener más información sobre el formato)
* Datos de formato de archivo con relación de atributo (ARFF) (.arff) (consulte la [definición de ARFF](http://weka.wikispaces.com/ARFF) si desea ver información sobre el formato)
* Archivo ZIP (.zip)
* Archivo de área de trabajo u objeto de R (.RData)

Si importa datos en un formato distinto de ARFF que incluyan metadatos, Machine Learning Studio usa estos metadatos para definir el tipo de datos y encabezado de cada columna.

Si importa datos en formato TSV o CSV que no incluyan estos metadatos, Machine Learning Studio infiere el tipo de datos de cada columna tomando una muestra de los mismos. Si los datos no tienen encabezados de columna, Machine Learning Studio proporciona nombres predeterminados.

Puede especificar o cambiar explícitamente los encabezados y los tipos de datos de las columnas usando el módulo [Editar metadatos][edit-metadata].

Machine Learning Studio reconoce los siguientes **tipos de datos** :

* string
* Entero
* Doble
* boolean
* Datetime
* timespan

Machine Learning Studio usa un tipo de datos interno llamado ***Tabla de datos*** para pasar datos entre los módulos. Puede convertir explícitamente sus datos en formato de tabla de datos con el módulo [Convertir al conjunto de datos][convert-to-dataset].

Todo módulo que acepta formatos distintos de Tabla de datos convertirá los datos a Tabla de datos de manera silenciosa antes de pasarlos al módulo siguiente.

En caso de ser necesario, puede convertir el formato Tabla de datos de vuelta al formato CSV, TSV, ARFF o SVMLight mediante el uso de otros módulos de conversión.
Consulte la sección **Conversiones de formatos de datos** de la paleta de módulos para ver los módulos que realizan estas funciones.

<!-- Module References -->
[convert-to-dataset]: https://msdn.microsoft.com/library/azure/72bf58e0-fc87-4bb1-9704-f1805003b975/
[edit-metadata]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
