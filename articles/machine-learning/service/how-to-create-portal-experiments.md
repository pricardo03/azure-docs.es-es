---
title: Uso de la interfaz de ML automatizado de Azure para entrenar e implementar modelos
titleSuffix: Azure Machine Learning
description: Cree, administre e implemente experimentos de aprendizaje automático automatizado en la página de aterrizaje del área de trabajo de Azure Machine Learning (versión preliminar).
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 09/09/2019
ms.openlocfilehash: 45207eb1cdc62f2468d8b0c052723337c18d5021
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350554"
---
# <a name="create-explore-and-deploy-automated-machine-learning-experiments-with-azure-machine-learnings-workspace-landing-page-preview"></a>Cree, explore e implemente experimentos de aprendizaje automático automatizado con la página de aterrizaje del área de trabajo de Azure Machine Learning (versión preliminar).

 En este artículo, aprenderá a crear, explorar e implementar experimentos de aprendizaje automático automatizado en la página de aterrizaje del área de trabajo de Machine Learning sin una sola línea de código. El aprendizaje automático automatizado automatiza el proceso de selección del mejor algoritmo que se usará para datos específicos, por lo que puede generar un modelo de Machine Learning rápidamente. [Más información sobre el aprendizaje automático automatizado](concept-automated-ml.md).

 Si prefiere una experiencia más basada en código, también puede [configurar los experimentos de aprendizaje automático automatizado de Python](how-to-configure-auto-train.md) con el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Un área de trabajo de Azure Machine Learning. Consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

## <a name="get-started"></a>Primeros pasos

1. Inicie sesión en la [página de aterrizaje del área de trabajo](https://ml.azure.com/workspaceportal/). 

1. Seleccione su suscripción y área de trabajo. 

1. Navegue al panel izquierdo. Seleccione **Automated ML** (ML automatizado) en la sección **Creación**.

[![Panel de navegación de Azure Portal](media/how-to-create-portal-experiments/nav-pane.png)](media/how-to-create-portal-experiments/nav-pane-expanded.png)

 Si es la primera vez que realiza algún experimento, verá la pantalla **Introducción**. 

En caso contrario, verá el panel de **Machine Learning automatizado** con una introducción de todos sus experimentos de aprendizaje automático, incluidos aquellos creados con el SDK. Aquí puede filtrar y explorar las ejecuciones por la fecha, el nombre de experimento y el estado de la ejecución.

## <a name="create-and-run-experiment"></a>Creación y ejecución de un experimento

1. Seleccione **Create Experiment** (Crear experimento) y rellene el formulario.

1. Escriba un nombre único para el experimento.

1. Seleccione un proceso para la generación de perfiles de los datos y el trabajo de entrenamiento. Hay una lista de los procesos existentes disponible en la lista desplegable. Para crear un nuevo proceso, siga las instrucciones del paso 4.

1. Seleccione **Create a new compute** (Crear un proceso) para configurar el contexto del proceso de este experimento.

    Campo|DESCRIPCIÓN
    ---|---
    Nombre del proceso| Escriba un nombre único que identifique el contexto del proceso.
    Tamaño de la máquina virtual| Seleccione el tamaño de la máquina virtual para el proceso.
    Número máximo y mínimo de nodos (en Configuración avanzada)| Para generar perfiles de datos, debe especificar uno o más nodos. escriba el número máximo de nodos para el proceso. El valor predeterminado es seis nodos para un proceso de AML.
    
    Seleccione **Crear**. La creación de un nuevo proceso puede tardar unos minutos.

    >[!NOTE]
    > Su nombre de proceso indicará si el proceso que selecciona o crea *admite la generación de perfiles* . (Consulte la sección sobre la [generación de perfiles de los datos](#profile) para más detalles ).

1. Seleccione un conjunto de datos en el contenedor de almacenamiento o cree uno cargando un archivo desde el equipo local al contenedor. La versión preliminar pública solo admite cargas de archivos locales y cuentas de Azure Blob Storage.

    >[!Important]
    > Requisitos para los datos de entrenamiento:
    >* Los datos deben estar en formato tabular.
    >* El valor que quiere predecir (columna de destino) debe estar presente en los datos.

    1. Para crear un nuevo conjunto de datos a partir de un archivo en el proceso local, seleccione **Examinar** y seleccione el archivo. 

    1. Asigne un nombre único al conjunto de datos y proporcione una descripción opcional. 

    1. Seleccione **Siguiente** para cargarlo en el contenedor de almacenamiento predeterminado que se crea automáticamente con el área de trabajo o elija un contenedor de almacenamiento que quiera usar para el experimento. 

    1. Revise el formulario **Settings and preview** (Configuración y vista previa) para ver que todo está correcto. El formulario se rellena de forma inteligente según el tipo de archivo. 

        Campo| DESCRIPCIÓN
        ----|----
        Formato de archivo| Define el diseño y el tipo de datos almacenados en un archivo.
        Delimitador| Uno o más caracteres para especificar el límite entre regiones independientes en texto sin formato u otros flujos de datos.
        Encoding| Identifica qué tabla de esquema de bit a carácter se va a usar para leer el conjunto de elementos.
        Encabezados de columna| Indica cómo se tratarán los encabezados del conjunto de datos, si existen.
        Omitir filas | Indica el número de filas, si hay alguna, que se omiten en el conjunto de datos.
    
        Seleccione **Next** (Siguiente).

    1. El formulario **Esquema** se rellena de forma inteligente en función de las selecciones realizadas en el formulario **Settings and preview** (Configuración y vista previa). Aquí se configura el tipo de datos para cada columna, se revisan los nombres de columna y se seleccionan las columnas que **no se van a incluir** en el experimento. 
            
        Seleccione **Siguiente**.

1. Seleccione el tipo de trabajo de entrenamiento: clasificación, regresión o previsión.

1. Seleccione una columna de destino; esta es la columna en la que realizará las predicciones.

1. Para la previsión:
    1. Seleccione la columna de tiempo: esta columna contiene los datos de tiempo que desea usar.

    1. Seleccione el horizonte de previsión: Indique cuántas unidades de tiempo (minutos, horas, días, semanas, meses o años) será capaz predecir el modelo en el futuro. Cuanto más se exija al modelo que prediga en el futuro, menos preciso será. [Más información sobre la previsión y el horizonte de previsión](how-to-auto-train-forecast.md).

1. (Opcional) Configuración avanzada: configuración adicional que puede usar para controlar mejor el trabajo de entrenamiento. De lo contrario, los valores predeterminados se aplican en función de la selección y los datos del experimento. 

    Configuración avanzada|DESCRIPCIÓN
    ------|------
    Métrica principal| Métrica principal usada para puntuar el modelo. [Más información sobre las métricas del modelo](how-to-configure-auto-train.md#explore-model-metrics).
    Exit criteria (Criterios de salida)| Cuando se cumple alguno de estos criterios, se detiene el trabajo de entrenamiento. <br> *Training job time (minutes)* (Tiempo de trabajo de entrenamiento [minutos]): cantidad de tiempo para permitir que el trabajo de entrenamiento se ejecute.  <br> *Max number of iterations* (Número máximo de iteraciones): número máximo de canalizaciones (iteraciones) para probar en el trabajo de entrenamiento. El trabajo no ejecutará más iteraciones que el número especificado de ellas. <br> *Metric score threshold* (Umbral de puntuación de métrica):  puntuación mínima de métrica para todas las canalizaciones. Esto garantiza que si tiene una métrica objetivo definida que desee alcanzar, no dedicará más tiempo en el trabajo de entrenamiento que el necesario.
    Preprocessing (Preprocesamiento)| Seleccione esta opción para habilitar o deshabilitar el preprocesamiento que el aprendizaje automático automatizado realiza. El preprocesamiento incluye la limpieza, preparación y transformación automáticas de los datos para generar características sintéticas. [Más información sobre el preprocesamiento](#preprocess).
    Validación| Seleccione una de las opciones de validación cruzada en el trabajo de entrenamiento. [Más información sobre la validación cruzada](how-to-configure-auto-train.md).
    Simultaneidad| Seleccione los límites de varios núcleos que le gustaría usar cuando se usa un proceso de varios núcleos.
    Blocked algorithms (Algoritmos bloqueados)| Seleccione los algoritmos que desea excluir del trabajo de entrenamiento.

<a name="profile"></a>

## <a name="data-profiling--summary-stats"></a>Perfiles de datos y estadísticas de resumen

Puede obtener una gran variedad de estadísticas de resumen en el conjunto de datos para comprobar si dicho conjunto está listo para ML. Para las columnas no numéricas, solo incluyen estadísticas básicas, como mínimo, máximo y recuento de errores. Para las columnas numéricas, también puede revisar sus momentos estadísticos y los cuantiles estimados. En concreto, nuestro perfil de datos incluye:

>[!NOTE]
> Aparecen entradas en blanco para las características con tipos irrelevantes.

Estadísticas|DESCRIPCIÓN
------|------
Característica| Nombre de la columna que se está resumiendo.
Perfil| Visualización en línea según el tipo inferido. Por ejemplo, las cadenas, los tipos booleanos y las fechas tendrán recuentos de valores, mientras que los tipos decimales (valores numéricos) tendrán histogramas aproximados. Esto le permite obtener una descripción rápida de la distribución de los datos.
Distribución de tipo| Recuento de valor en línea de los tipos dentro de una columna. Los valores Null son su propio tipo, por lo que esta visualizaicón es útil para detectar los valores impares o que faltan.
type|Tipo inferido de la columna. Los valores posibles incluyen: cadenas, valores booleanos, fechas y decimales.
Min| Valor mínimo de la columna. Aparecen entradas en blanco para características cuyo tipo no tiene una ordenación inherente (por ejemplo, valores booleanos).
max| Valor máximo de la columna. 
Count| Número total de entradas que faltan y que no faltan en la columna.
No falta el recuento| Número de entradas de la columna que no faltan. Las cadenas vacías y los errores se tratan como valores, por lo que no contribuirán a la lista "No falta el recuento".
Cuantiles| Valores aproximados en cada cuantil para proporcionar una idea de la distribución de los datos.
Media| Media aritmética o promedio de la columna.
Desviación estándar| Medida de la cantidad de dispersión o variación de los datos de esta columna.
Variance| Medida de la diferencia de los datos de esta columna con respecto a su valor medio. 
Asimetría| Medida de la diferencia entre los datos de esta columna y la distribución normal.
Curtosis| La medida de la cantidad de datos en cola de esta columna se compara con una distribución normal.

<a name="preprocess"></a>

## <a name="advanced-preprocessing-options"></a>Opciones de preprocesamiento avanzado

Al configurar los experimentos, puede habilitar la configuración avanzada `Preprocess`. Si lo hace, los pasos siguientes de caracterización y preprocesamiento de los datos se realizarán automáticamente.

|Pasos de&nbsp;preprocesamiento| DESCRIPCIÓN |
| ------------- | ------------- |
|Se quitan las características de cardinalidad alta o sin variación|Quítelas de los conjuntos de entrenamiento y validación, incluidas las características en las faltan todos los valores, que tienen el mismo valor en todas las filas o que tienen una cardinalidad muy alta (por ejemplo, hashes, identificadores o GUID).|
|Atribución de valores que faltan|Para las características numéricas, se atribuyen con el promedio de los valores de la columna.<br/><br/>Para las características de categorías, se atribuyen con el valor más frecuente.|
|Generación de características adicionales|Para las características de fecha y hora: año, mes, día, día de la semana, día del año, trimestre, semana del año, hora, minuto, segundo.<br/><br/>Para las características de texto: Frecuencia de término basada en unigramas, bigramas y trigramas.|
|Transformar y codificar |Las características numéricas con pocos valores únicos se transforman en características de categorías.<br/><br/>La codificación "one-hot" se realiza para características de categorías de cardinalidad baja; para una cardinalidad alta, se lleva a cabo la codificación "one-hot-hash".|
|Inserciones de palabras|Caracterizador de texto que convierte los vectores de tokens de texto en vectores de oración mediante un modelo previamente entrenado. El vector de inserción de cada palabra en un documento se agrega para producir un vector de característica de documento.|
|Codificaciones de destino|Para características de categorías, se asigna a cada categoría con el valor de destino promedio para los problemas de regresión, y a la probabilidad de clase para cada clase para problemas de clasificación. La ponderación basada en la frecuencia y la validación cruzada de k iteraciones se aplican para reducir el ajuste excesivo de la asignación y el ruido causado por categorías de datos dispersos.|
|Codificación de destino de texto|Para la entrada de texto, se usa un modelo lineal apilado con bolsa de palabras para generar la probabilidad de cada clase.|
|Peso de la evidencia (WoE)|Calcula WoE como una medida de la correlación de las columnas de categorías para la columna de destino. Se calcula como el registro de la relación de las probabilidades dentro de la clase frente a las probabilidades fuera de la clase. Este paso genera una columna de característica numérica por clase y quita la necesidad de atribuir de forma explícita los valores que faltan y el tratamiento de valores atípicos.|
|Distancia del clúster|Entrena un modelo de agrupamiento k-medias en todas las columnas numéricas.  Genera k nuevas características, una nueva característica numérica por grupo, que contiene la distancia de cada muestra hasta el centroide de cada grupo.|

## <a name="run-experiment-and-view-results"></a>Ejecución del experimento y visualización de los resultados

Seleccione **Start** (Iniciar) para ejecutar el experimento. El proceso de preparación del experimento tarda unos minutos.

### <a name="view-experiment-details"></a>Visualización de los detalles del experimento

Una vez que se realiza la fase de preparación del experimento, verá que la pantalla Run Detail (Detalles de la ejecución) empieza a llenarse. Esta pantalla proporciona una lista completa de los modelos creados. De forma predeterminada, el modelo que puntúa más alto en función de las métricas seleccionadas aparece en la parte superior de la lista. A medida que el trabajo de entrenamiento prueba más modelos, se agregan a la lista de iteración y al gráfico. Para obtener una comparación rápida de las métricas para los modelos generados hasta ahora, utilice el gráfico de iteración.

Los trabajos de entrenamiento pueden tardar un tiempo para que cada canalización termine de ejecutarse.

[![Panel de detalles de ejecución](media/how-to-create-portal-experiments/run-details.png)](media/how-to-create-portal-experiments/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Ver detalles de ejecución del entrenamiento

Explorar en profundidad en cualquiera de los modelos de salida para ver los detalles de ejecución del entrenamiento, como las métricas de rendimiento y los gráficos de distribución. [Más información sobre los gráficos](how-to-understand-automated-ml.md).

[![Detalles de la iteración](media/how-to-create-portal-experiments/iteration-details.png)](media/how-to-create-portal-experiments/iteration-details-expanded.png)

## <a name="deploy-your-model"></a>Implementación del modelo

Una vez que tenga a mano el mejor modelo, es el momento de implementarlo como un servicio web para predecir los datos nuevos.

ML automatizado le ayuda a implementar el modelo sin escribir código:

1. Tiene unas par de opciones de implementación. 

    + Opción 1: Para implementar el mejor modelo (según los criterios de métrica que definió), seleccione Deploy Best Model (Implementar el mejor modelo) en la página Run Detail (Detalles de ejecución).

    + Opción 2: Si quiere implementar una iteración de modelo específica de este experimento, explore en profundidad el modelo para abrir su página de detalles de ejecución específicos y seleccione Deploy Model (Implementar modelo).

1. Rellene el panel **Deploy Model** (Implementar modelo).

    Campo| Valor
    ----|----
    Nombre de implementación| Escriba un nombre único para la implementación.
    Descripción de implementación| Escriba una descripción para saber mejor para qué sirve esta implementación.
    Script de puntuación| Genere automáticamente o cargue su propio archivo de puntuación. [Más información sobre el script de puntuación](how-to-deploy-and-where.md#script).
    Script del entorno| Genere automáticamente o cargue su propio archivo de entorno.
    >[!Important]
    > Los nombres de archivo deben tener menos de 32 caracteres y deben comenzar y terminar con caracteres alfanuméricos. Puede incluir guiones, caracteres de subrayado, puntos y caracteres alfanuméricos. No se permiten espacios.

1. Seleccione **Implementar**. La implementación puede tardar unos 20 minutos en completarse.

    El siguiente mensaje aparece cuando la implementación se completa correctamente.

    ![Implementación completada](media/tutorial-1st-experiment-automated-ml/deploy-complete-status.png) 

Ya tiene un servicio web operativo para generar predicciones.

## <a name="next-steps"></a>Pasos siguientes

* Pruebe el [tutorial de un extremo a otro para crear su primer experimento de ML automatizado con Azure Machine Learning](tutorial-first-experiment-automated-ml.md). 
* [Más información sobre el aprendizaje automático automatizado](concept-automated-ml.md) y Azure Machine Learning.
* [Descripción de los resultados de aprendizaje automático automatizado](how-to-understand-automated-ml.md).
* [Obtenga información sobre cómo consumir un servicio web](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
