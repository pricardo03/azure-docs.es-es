---
title: Compilación e implementación de modelos de Machine Learning automatizados
titleSuffix: Azure Machine Learning
description: Cree, administre e implemente experimentos de aprendizaje automático automatizado en Azure Machine Learning Studio.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: nibaccam
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 02/04/2020
ms.openlocfilehash: a2bf15c8778a6ff549284b1053cf0978d182b802
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77116886"
---
# <a name="create-explore-and-deploy-automated-machine-learning-experiments-with-azure-machine-learning-studio"></a>Cree, explore e implemente experimentos de aprendizaje automático automatizado con Azure Machine Learning Studio
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

 En este artículo, aprenderá a crear, explorar e implementar experimentos de aprendizaje automático automatizado en Azure Machine Learning Studio sin una sola línea de código. El aprendizaje automático automatizado automatiza el proceso de selección del mejor algoritmo que se usará para datos específicos, por lo que puede generar un modelo de Machine Learning rápidamente. [Más información sobre el aprendizaje automático automatizado](concept-automated-ml.md).

 Si prefiere una experiencia más basada en código, también puede [configurar los experimentos de aprendizaje automático automatizado de Python](how-to-configure-auto-train.md) con el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="prerequisites"></a>Prerrequisitos

* Suscripción a Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

* Un área de trabajo de Azure Machine Learning con un tipo de **Enterprise Edition**. Consulte [Creación de un área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).  Para actualizar un área de trabajo existente a Enterprise Edition, consulte [Actualización a Enterprise Edition](how-to-manage-workspace.md#upgrade).

## <a name="get-started"></a>Introducción

1. Inicie sesión en [Azure Machine Learning Studio](https://ml.azure.com). 

1. Seleccione su suscripción y área de trabajo. 

1. Navegue al panel izquierdo. Seleccione **Automated ML** (ML automatizado) en la sección **Creación**.

[![Panel de navegación de Azure Machine Learning Studio](media/how-to-create-portal-experiments/nav-pane.png)](media/how-to-create-portal-experiments/nav-pane-expanded.png)

 Si es la primera vez que realiza algún experimento, verá una lista vacía y vínculos a la documentación. 

De lo contrario, verá una lista de los experimentos de aprendizaje automático automatizado recientes, incluidos los creados con el SDK. 

## <a name="create-and-run-experiment"></a>Creación y ejecución de un experimento

1. Seleccione **+ New automated ML run** (+ Nueva ejecución de ML automatizada) y rellene el formulario.

1. Seleccione un conjunto de datos del contenedor de almacenamiento o cree un nuevo conjunto de datos. Los conjuntos de datos se pueden crear a partir de archivos locales, direcciones URL web, almacenes de datos o Azure Open Datasets. 

    >[!Important]
    > Requisitos para los datos de entrenamiento:
    >* Los datos deben estar en formato tabular.
    >* El valor que quiere predecir (columna de destino) debe estar presente en los datos.

    1. Para crear un nuevo conjunto de datos a partir de un archivo del equipo local, seleccione **Examinar** y seleccione el archivo. 

    1. Asigne un nombre único al conjunto de datos y proporcione una descripción opcional. 

    1. Seleccione **Siguiente** para abrir el formulario **Datastore and file selection** (Almacén de datos y selección de archivos). En este formulario, seleccione dónde quiere cargar el conjunto de datos: el contenedor de almacenamiento predeterminado que se crea automáticamente con el área de trabajo, o bien elija un contenedor de almacenamiento que quiera usar para el experimento. 

    1. Revise el formulario **Settings and preview** (Configuración y vista previa) para ver que todo está correcto. El formulario se rellena de forma inteligente según el tipo de archivo. 

        Campo| Descripción
        ----|----
        Formato de archivo| Define el diseño y el tipo de datos almacenados en un archivo.
        Delimitador| Uno o más caracteres para especificar el límite entre regiones independientes en texto sin formato u otros flujos de datos.
        Encoding| Identifica qué tabla de esquema de bit a carácter se va a usar para leer el conjunto de elementos.
        Encabezados de columna| Indica cómo se tratarán los encabezados del conjunto de datos, si existen.
        Omitir filas | Indica el número de filas, si hay alguna, que se omiten en el conjunto de datos.
    
        Seleccione **Next** (Siguiente).

    1. El formulario **Esquema** se rellena de forma inteligente en función de las selecciones realizadas en el formulario **Settings and preview** (Configuración y vista previa). Aquí se configura el tipo de datos para cada columna, se revisan los nombres de columna y se seleccionan las columnas que **no se van a incluir** en el experimento. 
            
        Seleccione **Siguiente**.

    1. En el formulario **Confirmar detalles** se muestra un resumen de la información que se ha rellenado anteriormente en los formularios **Información básica** y **Settings and preview** (Configuración y vista previa). También tiene la opción de crear un perfil de datos para el conjunto de datos mediante un proceso habilitado para la generación de perfiles. Más información acerca de la [generación de perfiles de datos](#profile).

        Seleccione **Next** (Siguiente).
1. Seleccione el conjunto de datos recién creado cuando aparezca. También puede ver una vista previa del conjunto de datos y las estadísticas de ejemplo. 

1. En el formulario **Ejecutar la configuración**, escriba un nombre único para el experimento.

1. Seleccione una columna de destino; esta es la columna en la que realizará las predicciones.

1. Seleccione un proceso para la generación de perfiles de los datos y el trabajo de entrenamiento. Hay una lista de los procesos existentes disponible en la lista desplegable. Para crear un nuevo proceso, siga las instrucciones del paso 7.

1. Seleccione **Create a new compute** (Crear un proceso) para configurar el contexto del proceso de este experimento.

    Campo|Descripción
    ---|---
    Nombre del proceso| Escriba un nombre único que identifique el contexto del proceso.
    Tamaño de la máquina virtual| Seleccione el tamaño de la máquina virtual para el proceso.
    Número máximo y mínimo de nodos (en Configuración avanzada)| Para generar perfiles de datos, debe especificar uno o más nodos. escriba el número máximo de nodos para el proceso. El valor predeterminado es seis nodos para un proceso de AML.
    
    Seleccione **Crear**. La creación de un nuevo proceso puede tardar unos minutos.

    >[!NOTE]
    > Su nombre de proceso indicará si el proceso que selecciona o crea *admite la generación de perfiles* . (Consulte la sección sobre la [generación de perfiles de los datos](#profile) para más detalles ).

    Seleccione **Next** (Siguiente).

1. En el formulario **Task type and settings** (Tipo de tarea y configuración), seleccione el tipo de tarea: clasificación, regresión o previsión. Para más información, consulte el artículo en el que se explica [cómo definir tipos de tareas](how-to-define-task-type.md).

    1. Para la clasificación, también puede habilitar el aprendizaje profundo, que se usa para las características de texto.

    1. Para la previsión:
        1. Seleccione la columna de tiempo: esta columna contiene los datos de tiempo que desea usar.

        1. Seleccione el horizonte de previsión: Indique cuántas unidades de tiempo (minutos, horas, días, semanas, meses o años) será capaz predecir el modelo en el futuro. Cuanto más se exija al modelo que prediga en el futuro, menos preciso será. [Más información sobre la previsión y el horizonte de previsión](how-to-auto-train-forecast.md).

1. (Opcional) Ver el apartado sobre la adición de configuraciones: opciones de configuración adicionales que puede usar para controlar mejor el trabajo de entrenamiento. De lo contrario, los valores predeterminados se aplican en función de la selección y los datos del experimento. 

    Configuraciones adicionales|Descripción
    ------|------
    Métrica principal| Métrica principal usada para puntuar el modelo. [Más información sobre las métricas del modelo](how-to-configure-auto-train.md#explore-model-metrics).
    Características automáticas| Seleccione esta opción para habilitar o deshabilitar el preprocesamiento que el aprendizaje automático automatizado realiza. El preprocesamiento incluye la limpieza, preparación y transformación automáticas de los datos para generar características sintéticas. No se admite para el tipo de tarea de predicción de series temporales. [Más información sobre el preprocesamiento](#featurization). 
    Explicación del mejor modelo | Seleccione esta opción para habilitar o deshabilitar la visualización de la explicación del mejor modelo recomendado.
    Blocked algorithms (Algoritmos bloqueados)| Seleccione los algoritmos que desea excluir del trabajo de entrenamiento.
    Criterios de exclusión| Cuando se cumple alguno de estos criterios, se detiene el trabajo de entrenamiento. <br> *Tiempo de trabajo de entrenamiento (horas)* : cantidad de tiempo para permitir que el trabajo de entrenamiento se ejecute. <br> *Metric score threshold* (Umbral de puntuación de métrica):  puntuación mínima de métrica para todas las canalizaciones. Esto garantiza que si tiene una métrica objetivo definida que desee alcanzar, no dedicará más tiempo en el trabajo de entrenamiento que el necesario.
    Validación| Seleccione una de las opciones de validación cruzada en el trabajo de entrenamiento. [Más información sobre la validación cruzada](how-to-configure-auto-train.md).
    Simultaneidad| *Número máximo de iteraciones simultáneas*: número máximo de canalizaciones (iteraciones) para probar en el trabajo de entrenamiento. El trabajo no ejecutará más iteraciones que el número especificado de ellas.

1. (Opcional) Ver el apartado sobre la configuración de características: si decide habilitar **Características automáticas** en el formulario **Additional configuration settings** (Opciones de configuración adicionales), en este formulario se especifican las columnas en las que se van a definir dichas características y se selecciona el valor estadístico que se va a usar para las imputaciones de valores que faltan.

<a name="profile"></a>

## <a name="data-profiling--summary-stats"></a>Perfiles de datos y estadísticas de resumen

Puede obtener una gran variedad de estadísticas de resumen en el conjunto de datos para comprobar si dicho conjunto está listo para ML. Para las columnas no numéricas, solo incluyen estadísticas básicas, como mínimo, máximo y recuento de errores. Para las columnas numéricas, también puede revisar sus momentos estadísticos y los cuantiles estimados. En concreto, nuestro perfil de datos incluye:

>[!NOTE]
> Aparecen entradas en blanco para las características con tipos irrelevantes.

Estadísticas|Descripción
------|------
Característica| Nombre de la columna que se está resumiendo.
Perfil| Visualización en línea según el tipo inferido. Por ejemplo, las cadenas, los tipos booleanos y las fechas tendrán recuentos de valores, mientras que los tipos decimales (valores numéricos) tendrán histogramas aproximados. Esto le permite obtener una descripción rápida de la distribución de los datos.
Distribución de tipo| Recuento de valor en línea de los tipos dentro de una columna. Los valores Null son su propio tipo, por lo que esta visualizaicón es útil para detectar los valores impares o que faltan.
Tipo|Tipo inferido de la columna. Los valores posibles incluyen: cadenas, valores booleanos, fechas y decimales.
Min| Valor mínimo de la columna. Aparecen entradas en blanco para características cuyo tipo no tiene una ordenación inherente (por ejemplo, valores booleanos).
Max| Valor máximo de la columna. 
Count| Número total de entradas que faltan y que no faltan en la columna.
No falta el recuento| Número de entradas de la columna que no faltan. Las cadenas vacías y los errores se tratan como valores, por lo que no contribuirán a la lista "No falta el recuento".
Cuantiles| Valores aproximados en cada cuantil para proporcionar una idea de la distribución de los datos.
Media| Media aritmética o promedio de la columna.
Desviación estándar| Medida de la cantidad de dispersión o variación de los datos de esta columna.
Variance| Medida de la diferencia de los datos de esta columna con respecto a su valor medio. 
Asimetría| Medida de la diferencia entre los datos de esta columna y la distribución normal.
Curtosis| La medida de la cantidad de datos en cola de esta columna se compara con una distribución normal.


<a name="featurization"></a>

## <a name="advanced-featurization-options"></a>Opciones avanzadas de caracterización

El aprendizaje automático automatizado ofrece límites de protección de datos y preprocesamiento automáticamente, para ayudarle a identificar y administrar posibles problemas con los datos. 

### <a name="preprocessing"></a>Preprocessing (Preprocesamiento)

|Pasos de&nbsp;preprocesamiento| Descripción |
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

### <a name="data-guardrails"></a>Límites de protección de datos

Los límites de protección de datos se aplican automáticamente para ayudarle a identificar posibles problemas con los datos (por ejemplo, valores faltantes o desequilibrio de clases) y a realizar acciones correctivas para mejorar los resultados. Hay muchos procedimientos recomendados disponibles y se pueden aplicar para lograr resultados confiables. 

En la tabla siguiente se describen los límites de protección de datos admitidos actualmente, así como los estados asociados que pueden producirse al enviar el experimento.

Límite de protección|Status|Condición&nbsp;para&nbsp;el desencadenador
---|---|---
Atribución de&nbsp;valores&nbsp;que faltan |**Superado** <br> <br> **Corregido**|    No falta ningún valor en ninguna de las columnas&nbsp;de entrada <br> <br> Faltan valores en algunas columnas
Validación cruzada|**Listo**|Si no se proporciona ningún conjunto de validación explícito
Detección de&nbsp;características de&nbsp;cardinalidad&nbsp;alta|  **Superado** <br> <br>**Listo**|   No se detectó ninguna característica de cardinalidad alta <br><br> Se detectaron columnas de entrada de cardinalidad alta
Detección de equilibrio de clases |**Superado** <br><br><br>**Con alertas** |Las clases están equilibradas en los datos de entrenamiento. Se considera que un conjunto de datos está equilibrado si todas las clases tienen una representación adecuada en el conjunto de datos según el número y proporción de las muestras. <br> <br> Las clases están desequilibradas en los datos de entrenamiento
Coherencia de los datos en la serie temporal|**Superado** <br><br><br><br> **Corregido** |<br> Se han analizado los valores seleccionados {horizonte, retardo y período acumulado} sin que se hayan detectado problemas potenciales de memoria insuficiente. <br> <br>Los valores seleccionados {horizonte, retardo y período acumulado} se analizaron y pueden provocar que el experimento se quede sin memoria. Se desactivó el período acumulado o el retardo.

## <a name="run-experiment-and-view-results"></a>Ejecución del experimento y visualización de los resultados

Para ejecutar el experimento, seleccione **Finalizar**. El proceso de preparación del experimento puede tardar hasta 10 minutos. Los trabajos de entrenamiento pueden tardar de 2 a 3 minutos más para que cada canalización termine de ejecutarse.

### <a name="view-experiment-details"></a>Visualización de los detalles del experimento

Se abre la pantalla **Detalles de ejecución** en la pestaña **Detalles**. En esta pantalla se muestra un resumen de la ejecución del experimento, incluida una barra de estado en la parte superior, junto al número de ejecución. 

La pestaña **Modelos** contiene una lista de los modelos creados ordenados por la puntuación de la métrica. De forma predeterminada, el modelo que puntúa más alto en función de las métricas seleccionadas aparece en la parte superior de la lista. A medida que el trabajo de entrenamiento prueba más modelos, se agregan a la lista. Utilice esto para obtener una comparación rápida de las métricas para los modelos generados hasta ahora.

[![Panel de detalles de ejecución](media/how-to-create-portal-experiments/run-details.png)](media/how-to-create-portal-experiments/run-details-expanded.png#lightbox)

### <a name="view-training-run-details"></a>Ver detalles de ejecución del entrenamiento

Explore en profundidad los modelos completados para ver los detalles de la ejecución de entrenamiento, como las métricas de ejecución en la pestaña **Detalles del modelo** o gráficos de rendimiento en la pestaña **Visualizaciones**. [Más información sobre los gráficos](how-to-understand-automated-ml.md).

[![Detalles de la iteración](media/how-to-create-portal-experiments/iteration-details.png)](media/how-to-create-portal-experiments/iteration-details-expanded.png)

## <a name="deploy-your-model"></a>Implementación del modelo

Una vez que tenga a mano el mejor modelo, es el momento de implementarlo como un servicio web para predecir los datos nuevos.

ML automatizado le ayuda a implementar el modelo sin escribir código:

1. Tiene unas par de opciones de implementación. 

    + Opción 1: Para implementar el mejor modelo (según los criterios de métrica que definió), seleccione el botón **Deploy best model** (Implementar el mejor modelo) en la pestaña **Detalles**.

    + Opción 2: Si quiere implementar una iteración de modelo específica de este experimento, explore en profundidad el modelo para abrir su pestaña **Detalles del modelo** y seleccione **Implementar modelo**.

1. Rellene el panel **Implementar modelo**.

    Campo| Value
    ----|----
    Nombre| Escriba un nombre único para la implementación.
    Descripción| Escriba una descripción para saber mejor para qué sirve esta implementación.
    Compute type (Tipo de proceso)| Seleccione el tipo de punto de conexión que desea implementar: *Azure Kubernetes Service (AKS)* o *Azure Container Instance (ACI)* .
    Nombre del proceso| *Solo se aplica a AKS:* Seleccione el nombre del clúster de AKS en que desea realizar la implementación.
    Enable authentication (Habilitar autenticación) | Seleccione esta opción para permitir la autenticación basada en token o basada en clave.
    Use custom deployment assets (Usar recursos de implementación personalizados)| Habilite esta característica si desea cargar su propio archivo de entorno y script de puntuación. [Más información sobre los scripts de puntuación](how-to-deploy-and-where.md#script).

    >[!Important]
    > Los nombres de archivo deben tener menos de 32 caracteres y deben comenzar y terminar con caracteres alfanuméricos. Puede incluir guiones, caracteres de subrayado, puntos y caracteres alfanuméricos. No se permiten espacios.

    El menú *Avanzado* ofrece características de implementación predeterminadas como la [recopilación de datos](how-to-enable-app-insights.md) y la configuración del uso de recursos. Si desea reemplazar estos valores predeterminados, hágalo en este menú.

1. Seleccione **Implementar**. La implementación puede tardar unos 20 minutos en completarse.

Ya tiene un servicio web operativo para generar predicciones. Puede probar las predicciones consultando el servicio de [soporte técnico de Azure Machine Learning de Power BI](how-to-consume-web-service.md#consume-the-service-from-power-bi).

## <a name="next-steps"></a>Pasos siguientes

* Pruebe el [tutorial de un extremo a otro para crear su primer experimento de ML automatizado con Azure Machine Learning Studio](tutorial-first-experiment-automated-ml.md). 
* [Más información sobre el aprendizaje automático automatizado](concept-automated-ml.md) y Azure Machine Learning.
* [Descripción de los resultados de aprendizaje automático automatizado](how-to-understand-automated-ml.md).
* [Obtenga información sobre cómo consumir un servicio web](https://docs.microsoft.com/azure/machine-learning/how-to-consume-web-service).
