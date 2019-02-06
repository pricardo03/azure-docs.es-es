---
title: Preguntas más frecuentes (P+F) sobre Machine Learning Studio
titleSuffix: Azure Machine Learning Studio
description: 'Azure Machine Learning Studio: preguntas más frecuentes sobre facturación, funcionalidades y limitaciones de un servicio en la nube para un modelado de predicción optimizado.'
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: ericlicoding
ms.author: amlstudiodocs
ms.custom: seodec18
ms.date: 06/02/2017
ms.openlocfilehash: 8c381ca02dcfb194b2807735415ef894d72f74fa
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462286"
---
# <a name="azure-machine-learning-studio-faq-capabilities-and-limitations"></a>Preguntas más frecuentes sobre Azure Machine Learning Studio: funcionalidades y limitaciones
Estas son algunas de las preguntas más frecuentes (P+F) y las respuestas correspondientes sobre Azure Machine Learning, un servicio en la nube para el desarrollo de modelos predictivos y la aplicación de soluciones mediante servicios web. 

## <a name="general-questions"></a>Preguntas generales
**¿Qué es Machine Learning Studio?**

Machine Learning Studio es un entorno de lienzo con la funcionalidad de arrastrar y colocar al que se accede mediante un explorador web. Machine Learning Studio hospeda una gran paleta de módulos en una interfaz de composición visual que ayuda a crear un flujo de trabajo completo de ciencia de los datos en forma de experimento.

Para más información sobre Machine Learning Studio, consulte [¿Qué es Machine Learning Studio?](what-is-ml-studio.md)

**¿Qué es el servicio API de Machine Learning?**

El servicio API de Machine Learning le permite implementar modelos predictivos, igual que los que se crean en Machine Learning Studio, como servicios escalables y tolerantes a errores. Los servicios web creados con el servicio API de Machine Learning son varias API de REST que proporcionan una interfaz para la comunicación entre las aplicaciones externas y los modelos de análisis predictivo.

Para más información, consulte [Consumo de servicios web de Azure Machine Learning](consume-web-services.md).

**¿Dónde se muestran mis servicios web clásicos? ¿Dónde se muestran mis nuevos servicios web basados Azure Resource Manager?**

Los servicios web creados con el modelo de implementación clásica y los servicios web creados mediante el nuevo modelo de implementación de Azure Resource Manager se muestran en el portal de [servicios web Microsoft Azure Machine Learning](https://services.azureml.net/).

Los servicios web clásicos también se muestran en [Machine Learning Studio](http://studio.azureml.net), en la pestaña de **servicios web**.

## <a name="azure-machine-learning-questions"></a>Preguntas sobre Azure Machine Learning
**¿Qué son los servicios web Azure Machine Learning?**

Los servicios web Machine Learning proporcionan una interfaz entre una aplicación y un modelo de puntuación de flujo de trabajo de Machine Learning. Una aplicación externa puede usar Azure Machine Learning para comunicarse con un modelo de puntuación de flujo de trabajo de Machine Learning en tiempo real. Una llamada a un servicio web Machine Learning devuelve resultados de predicción a una aplicación externa. Para realizar una llamada a un servicio web Machine Learning, es necesario pasar una clave de API que se creó al implementar el servicio web. Un servicio web Machine Learning se basa en REST, una conocida opción de arquitectura para proyectos de programación web.

Azure Machine Learning tiene dos tipos de servicios web:

* Servicio de solicitud-respuesta (RRS): servicio de baja latencia y muy escalable que proporciona una interfaz con los modelos sin estado creados e implementados mediante Machine Learning Studio.
* Servicio de ejecución por lotes (BES): servicio asincrónico que puntúa un lote de registros de datos.

Hay varias maneras de usar la API de REST y acceder al servicio web. Por ejemplo, puede escribir una aplicación en C#, R o Python con el código de ejemplo que se genera automáticamente cuando se implementa el servicio web.

El código de ejemplo está disponible en:
- La página de consumo del servicio web en el portal de servicios web Azure Machine Learning.
- La página de ayuda de la API en el panel de Machine Learning Studio.

También puede usar el libro de Microsoft Excel de ejemplo que se crea automáticamente y que está disponible en el panel del servicio web en Machine Learning Studio.

**¿Cuáles son las actualizaciones principales de Azure Machine Learning?**

Puede consultar las actualizaciones más recientes en [Novedades de Azure Machine Learning](../../active-directory/fundamentals/whats-new.md).

## <a name="import-and-export-data-for-machine-learning"></a>Importación y exportación de datos en Machine Learning
**¿Qué orígenes de datos admite Machine Learning?**

Puede descargar datos en un experimento de Machine Learning Studio de tres maneras:

- Cargar un archivo local como un conjunto de datos
- Usar un módulo para importar datos de servicios de datos en la nube
- Importar un conjunto de datos guardado de otro experimento

Para más información acerca de los formatos de archivo compatibles, consulte [Importación de datos de entrenamiento en Machine Learning Studio desde varios orígenes de datos](import-data.md).

### <a id="ModuleLimit"></a>¿Cómo de grande puede ser el conjunto de datos para mis módulos?
Los módulos en Machine Learning Studio admiten conjuntos de datos de hasta 10 GB de datos numéricos densos para casos de uso comunes. Si un módulo ocupa más de una entrada, el valor de 10 GB es el total de todos los tamaños de entrada. También puede realizar el muestreo de conjuntos de datos grandes mediante consultas de Hive o Azure SQL Database, o usar el procesamiento previo de aprendizaje por recuentos antes de la ingesta.  

Los siguientes tipos de datos se pueden expandir en conjuntos de datos grandes durante la normalización de características y están limitados a menos de 10 GB:

* Dispersos
* Categorías
* Cadenas
* Datos binarios

Los siguientes módulos están limitados a conjuntos de datos de menos de 10 GB:

* Módulos de recomendación.
* Módulo Synthetic Minority Oversampling Technique (SMOTE)
* Módulos de script: R, Python y SQL
* Módulos donde el tamaño de los datos de salida puede ser mayor que el tamaño de los datos de entrada, como en la aplicación de hash de característica o unión.
* Validación cruzada, ajuste de los hiperparámetros del modelo, regresión ordinal y multiclase uno contra todos, cuando el número de iteraciones sea muy grande.

### <a id="UploadLimit"></a>¿Cuáles son los límites de carga de datos?
En el caso de conjuntos de datos que tengan más de dos gigas, cargue datos en Azure Storage o en Azure SQL Database, o use Azure HDInsight, en lugar de cargarlos directamente desde el archivo local.

**¿Se pueden leer datos de Amazon S3?**

Si tiene una pequeña cantidad de datos y quiere exponerlos mediante una dirección URL HTTP, puede usar el módulo [Import Data][import-data]. Si la cantidad de datos es mayor, transfiéralos a Azure Storage y luego use el módulo [Import Data][import-data] para incluirlos en el experimento.
<!--

<SEE CLOUD DS PROCESS>
-->

**¿Hay una capacidad integrada para usar una entrada de imagen?**

Puede obtener información sobre la funcionalidad de entrada de imágenes en la referencia de [Import Images][image-reader].

## <a name="modules"></a>Módulos
**El algoritmo, el origen de datos, el formato de datos o la operación de transformación de datos que busco no están en Azure Machine Learning Studio. ¿Qué opciones tengo?**

Puede ir al [foro de comentarios de los usuarios](https://go.microsoft.com/fwlink/?LinkId=404231) para ver solicitudes de características cuyo seguimiento estamos realizando. Agregue su voto a esta solicitud si la funcionalidad que busca ya se ha solicitado. Si la funcionalidad que busca no existe, cree una nueva solicitud. El estado de su solicitud puede verlo también en este foro. Seguimos muy de cerca esta lista y actualizamos el estado de la disponibilidad de las características con frecuencia. Además, puede usar la compatibilidad integrada con R y Python para crear transformaciones personalizadas cuando sea necesario.

**¿Puedo usar mi código existente en Machine Learning Studio?**

Sí, puede usar su código R o Python existente en Machine Learning Studio, ejecutarlo en el mismo experimento con los aprendices de Azure Machine Learning e implementar la solución como un servicio web a través de Azure Machine Learning. Para más información, consulte [Extender el experimento con R](extend-your-experiment-with-r.md) y [Ejecución de scripts de Machine Learning de Python en Azure Machine Learning Studio](execute-python-scripts.md).

**¿Se puede usar algo como [PMML](http://en.wikipedia.org/wiki/Predictive_Model_Markup_Language) para definir un modelo?**

No, no se admite el lenguaje de marcado de modelos de predicción (PMML). Puede usar código personalizado de R y Python para definir un módulo.

**¿Cuántos módulos puedo ejecutar en paralelo en mi experimento?**  

En un experimento se pueden ejecutar hasta cuatro módulos en paralelo.

## <a name="data-processing"></a>Procesamiento de datos
**¿Se pueden visualizar los datos (más allá de visualizaciones R) interactivamente en el experimento?**

Haga clic en la salida de un módulo para visualizar los datos y obtener estadísticas.

**Cuando se obtiene una vista previa de los resultados o de los datos en un explorador, el número de filas y columnas se limita. ¿Por qué?**

Dada la posibilidad de que se envíen grandes cantidades de datos a un explorador, el tamaño de los datos se limita para impedir que Machine Learning Studio funcione más lento. Para visualizar los datos o los resultados en su totalidad, es mejor descargar los datos y usar Excel u otra herramienta.

## <a name="algorithms"></a>Algoritmos
**¿Qué algoritmos existentes se admiten en Machine Learning Studio?**

Machine Learning Studio ofrece los algoritmos mas innovadores, como árboles de decisión incrementados escalables, sistemas de recomendaciones bayesianas, redes neuronales profundas y selvas de decisión, que se han desarrollado en Microsoft Research. También se incluyen paquetes de aprendizaje automático escalables de código abierto, como Vowpal Wabbit. Machine Learning Studio admite algoritmos de aprendizaje automático para clasificación, regresión y agrupación en clústeres y binarias y multiclase. Consulte la lista completa de [módulos de Machine Learning][machine-learning-modules].

**¿Se sugiere automáticamente el algoritmo de Machine Learning adecuado para utilizarlo con mis datos?**

No. Sin embargo, Machine Learning Studio ofrece varias maneras de comparar los resultados de cada algoritmo para determinar el más apropiado para su problema.

**¿Hay instrucciones sobre la elección de un algoritmo en lugar de otro de los proporcionados?**

Consulte [Elección de un algoritmo](algorithm-choice.md).

**¿Se proporcionan algoritmos escritos en R o Python?**

No. Estos algoritmos principalmente se escriben en lenguajes compilados para que ofrezcan un mayor rendimiento.

**¿Se proporcionan algunos detalles de los algoritmos?**

La documentación proporciona alguna información acerca de los algoritmos, y se describen los parámetros para optimizar el algoritmo para su uso.  

**¿Se admite el aprendizaje en línea?**

No. Actualmente solo se admite el reentrenamiento mediante programación.

**¿Puedo visualizar las capas de un modelo de red neuronal con el módulo integrado?**

 No.

**¿Puedo crear mis propios módulos en C# o algún otro lenguaje?**

Actualmente, solo puede usar R para crear nuevos módulos personalizados.

## <a name="r-module"></a>Módulo R
**¿Qué paquetes de R están disponibles en Machine Learning Studio?**

En la actualidad, Machine Learning Studio admite más de 400 paquetes de CRAN R y esta es la [lista actual](http://az754797.vo.msecnd.net/docs/RPackages.xlsx) de todos los paquetes incluidos. Consulte también [Extender el experimento con R](extend-your-experiment-with-r.md) para aprender a recuperar esta lista. Si el paquete que desea no está en la lista, especifique el nombre del paquete en el [foro de comentarios de los usuarios](https://go.microsoft.com/fwlink/?LinkId=404231).

**¿Es posible crear un módulo personalizado de R?**

Sí. Para más información, consulte [Creación de módulos R personalizados en Azure Machine Learning](custom-r-modules.md).

**¿Hay un entorno de REPL para R?**

No, no hay ningún entorno Read-Eval-Print-Loop (REPL) para R en Studio.

## <a name="python-module"></a>Módulo de Python
**¿Es posible crear un módulo personalizado de Python?**

Aunque en estos momentos no es posible, puede usar uno o varios módulos [Execute Python Script][python] para obtener el mismo resultado.

**¿Hay un entorno de REPL para Python?**

Puede usar Jupyter Notebooks en Machine Learning Studio. Para más información, consulte [Introducing Jupyter Notebooks in Azure Machine Learning Studio](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx)(Introducción a Jupyter Notebooks en Estudio de aprendizaje automático de Azure).

## <a name="web-service"></a>Servicio web

**¿Cómo reentreno los modelos de Azure Machine Learning mediante programación?**

Use las API de reentrenamiento. Para obtener más información, consulte [Volver a entrenar modelos de Machine Learning mediante programación](retrain-models-programmatically.md). También puede encontrar código de ejemplo en [Microsoft Azure Machine Learning Retraining Demo](https://azuremlretrain.codeplex.com/)(Demostración de reciclaje de Microsoft Azure Machine Learning).

**¿Puedo implementar el modelo de forma local o en una aplicación sin conexión a Internet?**

 No.

**¿Cabe esperar una latencia de línea de base para todos los servicios web?**

Consulte [Límites de la suscripción de Azure](../../azure-subscription-service-limits.md).

**¿Cuándo podría ejecutar mi modelo predictivo como servicio de ejecución de lotes en lugar de como servicio web de solicitud/respuesta?**

El servicio de solicitud-respuesta (RRS) es un servicio web de alta escala y baja latencia que se usa para proporcionar una interfaz con los modelos sin estado creados e implementados desde el entorno de experimentación. El servicio de ejecución de lotes (BES) es un servicio para la puntuación asincrónica de lotes de registros de datos. La entrada de BES es similar a la entrada de datos que se emplea en RRS. La diferencia principal radica en que BES lee un bloque de registros de varios orígenes, como Azure Blob Storage, Azure Table Storage, Azure SQL Database, HDInsight (consultas de Hive) y orígenes de HTTP. Para más información, consulte [Consumo de servicios web de Azure Machine Learning](consume-web-services.md).

**¿Cómo se actualiza el modelo para la producción del servicio implementado?**

Para actualizar un modelo predictivo para un servicio ya implementado, modifique y vuelva a ejecutar el experimento usado para crear y guardar el modelo entrenado. Una vez que disponga de una nueva versión del modelo entrenado, Machine Learning Studio le preguntará si desea actualizar el servicio web. Para más información sobre cómo actualizar un servicio web implementado, consulte [Implementar un servicio web Machine Learning](publish-a-machine-learning-web-service.md).

También puede usar las API de reciclaje.
Para obtener más información, consulte [Volver a entrenar modelos de Machine Learning mediante programación](retrain-models-programmatically.md). También puede encontrar código de ejemplo en [Microsoft Azure Machine Learning Retraining Demo](https://azuremlretrain.codeplex.com/)(Demostración de reciclaje de Microsoft Azure Machine Learning).

**¿Cómo se supervisa el servicio web implementado en producción?**

Tras implementar un modelo predictivo, se puede supervisar desde el portal de servicios web Azure Machine Learning. Cada servicio implementado cuenta con su propio panel, donde se puede ver la información de supervisión de ese servicio. Para más información sobre cómo administrar los servicios web implementados, consulte [Administración de un servicio web mediante el portal de servicios web Azure Machine Learning](manage-new-webservice.md) y [Administración de un área de trabajo de Azure Machine Learning](manage-workspace.md).

**¿Hay algún lugar donde pueda ver la salida de mi RRS/BES?**

Para RRS, en la respuesta del servicio web normalmente es donde se verá el resultado. También puede escribirlo en Azure Blob Storage. Para BES, el resultado se escribe en un blob de manera predeterminada. También puede escribir el resultado en una base de datos o en una tabla con el módulo [Export Data][export-data].

**¿Solamente puedo crear servicios web a partir de modelos creados en Machine Learning Studio?**

No. También puede crear servicios web directamente desde Jupyter Notebooks y RStudio.

**¿Dónde puedo encontrar información sobre los códigos de error?**

Consulte [Códigos de error del módulo de Machine Learning](https://msdn.microsoft.com/library/azure/dn905910.aspx) para ver una lista de códigos de error y sus descripciones.

**¿Qué es la escalabilidad del servicio web?**

En la actualidad, el punto de conexión predeterminado se ha aprovisionado con 20 solicitudes simultáneas de RRS por punto de conexión. Como se describe en [Escalado de servicios web](scaling-webservice.md), este número se puede escalar hasta alcanzar las 200 solicitudes simultáneas por punto de conexión y escalar cada servicio web hasta los 10 000 puntos de conexión por servicio web. En BES, cada punto de conexión permite el procesamiento de 40 solicitudes a la vez y el resto de solicitudes adicionales que superan este número se ponen en cola. Dichas solicitudes se ejecutan automáticamente a medida que avanza la cola.

**¿Los trabajos de R se reparten entre nodos?**

 No.  

**¿Cuántos datos se pueden usar para el entrenamiento?**

Los módulos en Machine Learning Studio admiten conjuntos de datos de hasta 10 GB de datos numéricos densos para casos de uso comunes. Si un módulo ocupa más de una entrada, el tamaño total de todas las entradas juntas es 10 GB. También puede realizar un muestreo de conjuntos de datos mayores mediante consultas de Hive o Azure SQL Database, o bien mediante el procesamiento previo con los módulos de [aprendizaje con recuentos][counts] antes de la ingesta.  

Los siguientes tipos de datos se pueden expandir en conjuntos de datos grandes durante la normalización de características y están limitados a menos de 10 GB:

* Dispersos
* Categorías
* Cadenas
* Datos binarios

Los siguientes módulos están limitados a conjuntos de datos de menos de 10 GB:

* Módulos de recomendación.
* Módulo Synthetic Minority Oversampling Technique (SMOTE)
* Módulos de script: R, Python y SQL
* Módulos donde el tamaño de los datos de salida puede ser mayor que el tamaño de los datos de entrada, como en la aplicación de hash de característica o unión.
* Validación cruzada, ajuste de los hiperparámetros del modelo, regresión ordinal y multiclase uno contra todos, cuando el número de iteraciones sea muy grande.

En el caso de conjuntos de datos que tengan más que unos cuantos gigas, cargue datos en Azure Storage o en Azure SQL Database, o use HDInsight, en lugar de cargarlos directamente desde el archivo local.

**¿Hay alguna limitación en el tamaño de los vectores?**

Las filas y las columnas están limitadas a la limitación .NET de Máx. int.: 2,147,483,647.

**¿Se puede ajustar el tamaño de la máquina virtual que ejecuta el servicio web?**

 No.  

## <a name="security-and-availability"></a>Seguridad y disponibilidad
**¿Quién puede acceder de forma predeterminada al punto de conexión http del servicio web? ¿Cómo se restringe el acceso al punto de conexión?**

Cuando se implementa un servicio web, se crea un punto de conexión predeterminado para ese servicio. El punto de conexión predeterminado puede llamarse mediante su clave de API. Se pueden agregar puntos de conexión adicionales con sus propias claves desde el portal de servicios web o mediante programación con las API de Administración del servicio web. Se necesitan claves de acceso para realizar llamadas al servicio web. Para más información, consulte [Consumo de servicios web de Azure Machine Learning](consume-web-services.md).

**¿Qué sucede si no se encuentra mi cuenta de Azure Storage?**

Machine Learning Studio depende de la cuenta de Azure Storage suministrada para guardar datos intermedios al ejecutar el flujo de trabajo. Esta cuenta de almacenamiento se proporciona a Machine Learning Studio en el momento de crear un área de trabajo. Una vez que se crea el área de trabajo, si se elimina la cuenta de almacenamiento y ya no puede encontrarse, el área de trabajo dejará de funcionar y todos los experimentos que haya en ella darán error.

Si ha eliminado accidentalmente la cuenta, vuelva a crearla con el mismo nombre y en la misma región que la eliminada. Después, vuelva a sincronizar la clave de acceso.

**¿Qué sucede si la clave de acceso de mi cuenta de almacenamiento no está sincronizada?**

Machine Learning Studio usa una cuenta de Azure Storage suministrada por el usuario para guardar los datos intermedios al ejecutar el flujo de trabajo. Esta cuenta se proporciona a Machine Learning Studio en el momento de crear un área de trabajo y las claves de acceso se asocian a dicha área de trabajo. Si se cambian las claves de acceso después de haber creado el área de trabajo, este no podrá volver a acceder a la cuenta de almacenamiento. Dejará de funcionar y se producirá un error en todos los experimentos que haya en esa área de trabajo.

Si ha cambiado las claves de acceso de la cuenta de almacenamiento, vuelva a sincronizarlas en el área de trabajo mediante Azure Portal.  


## <a name="billing-questions"></a>Preguntas sobre facturación

Para información sobre los precios y la facturación, consulte los [precios de Machine Learning](https://azure.microsoft.com/pricing/details/machine-learning/).


<!-- Module References -->
[image-reader]: https://msdn.microsoft.com/library/azure/893f8c57-1d36-456d-a47b-d29ae67f5d84/
[join]: https://msdn.microsoft.com/library/azure/124865f7-e901-4656-adac-f4cb08248099/
[machine-learning-modules]: https://msdn.microsoft.com/library/azure/6d9e2516-1343-4859-a3dc-9673ccec9edc/
[partition-and-sample]: https://msdn.microsoft.com/library/azure/a8726e34-1b3e-4515-b59a-3e4a475654b8/
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[export-data]: https://msdn.microsoft.com/library/azure/7A391181-B6A7-4AD4-B82D-E419C0D6522C
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[python]: https://msdn.microsoft.com/library/azure/CDB56F95-7F4C-404D-BDE7-5BB972E6F232
[counts]: https://msdn.microsoft.com/library/azure/dn913056.aspx
