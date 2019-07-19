---
title: Creación y administración de experimentos en el portal
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo crear y administrar experimentos de aprendizaje automático automatizado en el portal
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: tsikiksr
author: tsikiksr
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 05/02/2019
ms.openlocfilehash: 714283628e1b2ac445d36d0b07fe299b589a1cf0
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312814"
---
# <a name="create-and-explore-automated-machine-learning-experiments-in-the-azure-portal-preview"></a>Creación y exploración de experimentos de aprendizaje automático automatizado en Azure Portal (versión preliminar)

 En este artículo, aprenderá a crear, ejecutar y explorar experimentos de aprendizaje automático automatizado en Azure Portal sin una sola línea de código. El aprendizaje automático automatizado automatiza el proceso de selección del mejor algoritmo que se usará para datos específicos, por lo que puede generar un modelo de Machine Learning rápidamente. [Más información sobre el aprendizaje automático automatizado](concept-automated-ml.md).

 Si prefiere una experiencia más basada en código, también puede [configurar los experimentos de aprendizaje automático automatizado de Python](how-to-configure-auto-train.md) con el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="prerequisites"></a>Requisitos previos

* Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

* Un área de trabajo de Azure Machine Learning. Consulte [Creación de un área de trabajo de Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/setup-create-workspace).

## <a name="get-started"></a>Primeros pasos

Navegue hasta el panel izquierdo del área de trabajo. Seleccione Aprendizaje automático automatizado en la sección Creación (versión preliminar).

![Panel de navegación de Azure Portal](media/how-to-create-portal-experiments/nav-pane.png)

 Si es la primera vez que se realiza cualquier experimento con Aprendizaje automático automatizado, verá lo siguiente:

![Página de aterrizaje de experimento de Azure Portal](media/how-to-create-portal-experiments/landing-page.png)

En caso contrario, verá el panel de Aprendizaje automático automatizado con una introducción de todos sus experimentos de aprendizaje automático automatizado, incluidos aquellos creados con el SDK. Aquí puede filtrar y explorar las ejecuciones por la fecha, el nombre de experimento y el estado de la ejecución.

![Panel de experimento de Azure Portal](media/how-to-create-portal-experiments/dashboard.png)

## <a name="create-an-experiment"></a>Creación de un experimento

Seleccione el botón Create Experiment (Crear experimento) para rellenar el formulario siguiente.

![Formulario para crear un experimento](media/how-to-create-portal-experiments/create-exp-name-compute.png)

1. Escriba el nombre del experimento.

1. Seleccione un proceso para la generación de perfiles de los datos y el trabajo de entrenamiento. Hay una lista de los procesos existentes disponible en la lista desplegable. Para crear un nuevo proceso, siga las instrucciones del paso 3.

1. Seleccione el botón Create a new compute (Crear un nuevo proceso) para abrir el panel que figura a continuación y configurar el contexto del proceso para este experimento.

    ![Creación de un nuevo proceso para el experimento](media/how-to-create-portal-experiments/create-new-compute.png)

    Campo|DESCRIPCIÓN
    ---|---
    Nombre del proceso| Escriba un nombre único que identifique el contexto del proceso.
    Tamaño de la máquina virtual| Seleccione el tamaño de la máquina virtual para el proceso.
    Configuración adicional| *Min node* (Nodos mín.): escriba el número mínimo de nodos para el proceso. El número mínimo de nodos para el proceso de AML es 0. Para habilitar la generación de perfiles de datos, debe tener 1 o más nodos. <br> *Max node* (Nodos máx.): escriba el número máximo de nodos para el proceso. El valor predeterminado es seis nodos para un proceso de AML.

      Para iniciar la creación del nuevo proceso, seleccione **Crear**. Esta operación puede tardar unos segundos.

      >[!NOTE]
      > Su nombre de proceso indicará si el proceso que selecciona o crea *admite la generación de perfiles* . (Consulte el punto 7b para más detalles sobre la generación de perfiles de los datos).

1. Seleccione una cuenta de almacenamiento para los datos. La versión preliminar pública solo admite cargas de archivos locales y cuentas de Azure Blob Storage.

1. Seleccione un contenedor de almacenamiento.

1. Seleccione un archivo de datos en el contenedor de almacenamiento o cargue un archivo desde el equipo local al contenedor.

    ![Selección del archivo de datos para el experimento](media/how-to-create-portal-experiments/select-file.png)

1. Use las pestañas Vista previa y Perfil para definir más configuraciones para los datos para este experimento.

    1. En la pestaña Vista previa, indique si los datos incluyen encabezados y seleccione las características (columnas) para el entrenamiento utilizando los conmutadores **Incluido** de cada columna de característica.

        ![Vista previa de los datos](media/how-to-create-portal-experiments/data-preview.png)

    1. En la pestaña Perfil, puede ver el [perfil de los datos](#profile) por característica, así como la distribución, el tipo y las estadísticas de resumen (media, mediana, máximo y mínimo, etc.) de cada una.

        ![Pestaña Perfil de los datos](media/how-to-create-portal-experiments/data-profile.png)

        >[!NOTE]
        > El siguiente mensaje de error aparecerá si el contexto del proceso **no** admite la generación de perfiles: *Data profiling is only available for compute targets that are already running* (La generación de perfiles de los datos solo está disponible para destinos de proceso que ya se están ejecutando).

1. Seleccione el tipo de trabajo de entrenamiento: clasificación, regresión o previsión.

1. Seleccione la columna de destino. La columna sobre la que le gustaría hacer predicciones.

1. Para la previsión:
    1. Seleccione la columna de tiempo: esta columna contiene los datos de tiempo que desea usar.
    1. Seleccione el horizonte de previsión: Indique cuántas unidades de tiempo (minutos, horas, días, semanas, meses o años) será capaz predecir el modelo en el futuro. Cuanto más se exija al modelo que prediga en el futuro, menos preciso será. [Más información sobre la previsión y el horizonte de previsión](how-to-auto-train-forecast.md).

1. (Opcional) Configuración avanzada: configuración adicional que puede usar para controlar mejor el trabajo de entrenamiento.

    Configuración avanzada|DESCRIPCIÓN
    ------|------
    Métrica principal| Métrica principal usada para puntuar el modelo. [Más información sobre las métricas del modelo](how-to-configure-auto-train.md#explore-model-metrics).
    Exit criteria (Criterios de salida)| Cuando se cumple cualquiera de estos criterios, el trabajo de entrenamiento termina antes de la finalización completa. <br> *Training job time (minutes)* (Tiempo de trabajo de entrenamiento [minutos]): cantidad de tiempo para permitir que el trabajo de entrenamiento se ejecute.  <br> *Max number of iterations* (Número máximo de iteraciones): número máximo de canalizaciones (iteraciones) para probar en el trabajo de entrenamiento. El trabajo no ejecutará más iteraciones que el número especificado de ellas. <br> *Metric score threshold* (Umbral de puntuación de métrica):  puntuación mínima de métrica para todas las canalizaciones. Esto garantiza que si tiene una métrica objetivo definida que desee alcanzar, no dedicará más tiempo en el trabajo de entrenamiento que el necesario.
    Preprocessing (Preprocesamiento)| Seleccione esta opción para habilitar o deshabilitar el preprocesamiento que el aprendizaje automático automatizado realiza. El preprocesamiento incluye la limpieza, preparación y transformación automáticas de los datos para generar características sintéticas. [Más información sobre el preprocesamiento](#preprocess).
    Validación| Seleccione una de las opciones de validación cruzada en el trabajo de entrenamiento. [Más información sobre la validación cruzada](how-to-configure-auto-train.md).
    Simultaneidad| Seleccione los límites de varios núcleos que le gustaría usar cuando se usa un proceso de varios núcleos.
    Blocked algorithms (Algoritmos bloqueados)| Seleccione los algoritmos que desea excluir del trabajo de entrenamiento.

   ![Formulario de configuración avanzada](media/how-to-create-portal-experiments/advanced-settings.png)

> [!NOTE]
> Para más información sobre los campos, haga clic en el icono de información sobre herramientas.

<a name="profile"></a>

### <a name="data-profiling"></a>Generación de perfiles de los datos

Puede obtener una gran variedad de estadísticas de resumen en el conjunto de datos para comprobar si dicho conjunto está listo para ML. Para las columnas no numéricas, solo incluyen estadísticas básicas, como mínimo, máximo y recuento de errores. Para las columnas numéricas, también puede revisar sus momentos estadísticos y los cuantiles estimados. En concreto, nuestro perfil de datos incluye:

* **Característica**: nombre de la columna que se está resumiendo.

* **Perfil**: una visualización en línea según el tipo inferido. Por ejemplo, las cadenas, los tipos booleanos y las fechas tendrán recuentos de valores, mientras que los tipos decimales (valores numéricos) tendrán histogramas aproximados. Esto le permite obtener una descripción rápida de la distribución de los datos.

* **Tipo de distribución**: un recuento de valor en línea de los tipos dentro de una columna. Los valores Null son su propio tipo, por lo que esta visualizaicón es útil para detectar los valores impares o que faltan.

* **Tipo**: el tipo inferido de la columna. Los valores posibles incluyen: cadenas, valores booleanos, fechas y decimales.

* **Mín.** : el valor mínimo de la columna. Aparecen entradas en blanco para características cuyo tipo no tiene una ordenación inherente (por ejemplo, valores booleanos).

* **Máx.** : el valor máximo de la columna. Al igual que "Mínimo", aparecen entradas en blanco para las características con tipos irrelevantes.

* **Recuento**: el número total de entradas que faltan y que no faltan en la columna.

* **No falta el recuento**: el número de entradas de la columna que no faltan. Tenga en cuenta que las cadenas vacías y los errores se tratan como valores, por lo que no contribuirán a "No falta el recuento".

* **Cuantiles** (en intervalos de 0,1, 1, 5, 25, 50, 75, 95, 99 y 99,9 %): los valores aproximados en cada cuantil para proporcionar una idea de la distribución de los datos. Aparecen entradas en blanco para las características con tipos irrelevantes.

* **Media**: la media aritmética de la columna. Aparecen entradas en blanco para las características con tipos irrelevantes.

* **Desviación estándar**: la desviación estándar de la columna. Aparecen entradas en blanco para las características con tipos irrelevantes.

* **Varianza**: la varianza de la columna. Aparecen entradas en blanco para las características con tipos irrelevantes.

* **Asimetría**: la asimetría de la columna. Aparecen entradas en blanco para las características con tipos irrelevantes.

* **Curtosis**: la curtosis de la columna. Aparecen entradas en blanco para las características con tipos irrelevantes.

<a name="preprocess"></a>

### <a name="advanced-preprocessing"></a>Preprocesamiento avanzado

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

Para ejecutar el experimento, haga clic en Iniciar. El proceso de preparación del experimento tarda unos minutos.

### <a name="view-experiment-details"></a>Visualización de los detalles del experimento

Una vez que se realiza la fase de preparación del experimento, aparece la pantalla Run Detail (Detalles de la ejecución). Esto proporciona una lista completa de los modelos creados. De forma predeterminada, el modelo que puntúa más alto en función de los parámetros aparece en la parte superior de la lista. A medida que el trabajo de entrenamiento prueba más modelos, se agregan a la lista de iteración y al gráfico. Para obtener una comparación rápida de las métricas para los modelos generados hasta ahora, utilice el gráfico de iteración.

Los trabajos de entrenamiento pueden tardar un tiempo para que cada canalización termine de ejecutarse.

![Ejecución del panel de detalles](media/how-to-create-portal-experiments/run-details.png)

### <a name="view-training-run-details"></a>Ver detalles de ejecución del entrenamiento

Explorar en profundidad en cualquiera de los modelos de salida para ver los detalles de ejecución del entrenamiento, como las métricas de rendimiento y los gráficos de distribución. [Más información sobre los gráficos](how-to-track-experiments.md#understanding-automated-ml-charts).

![Detalles de la iteración](media/how-to-create-portal-experiments/iteration-details.png)

## <a name="deploy-model"></a>Implementación del modelo

Una vez que tenga a mano el mejor modelo, es el momento de implementarlo como un servicio web para predecir los datos nuevos.

ML automatizado le ayuda a implementar el modelo sin escribir código:

1. Tiene unas cuantas opciones para implementar. 
    1. Si desea implementar el mejor modelo según los criterios de las métricas establecidos para el experimento, seleccione **Deploy Best Model** (Implementar el mejor modelo) en la página **Run Detail** (Detalles de la ejecución).

        ![Botón de implementación del modelo](media/how-to-create-portal-experiments/deploy-model-button.png)

    1. Si desea implementar una iteración de modelo específica, explore en profundidad el modelo para abrir su página de detalles de ejecución específicos y seleccione **Deploy Model** (Implementar modelo).

        ![Botón de implementación del modelo](media/how-to-create-portal-experiments/deploy-model-button2.png)

1. El primer paso consiste en registrar el modelo en el servicio. Seleccione "Register model" (Registrar modelo) y espere a que el proceso de registro se complete.

    ![Hoja de implementación del modelo](media/how-to-create-portal-experiments/deploy-model-blade.png)

1. Una vez que se registra el modelo, podrá descargar el script de puntuación (scoring.py) y el script de entorno (condaEnv.yml) que se usarán durante la implementación.

1. Cuando se descarguen los script de puntuación y de entorno, vaya a la hoja **Activos** del panel de navegación izquierdo y seleccione **Modelos**.

    ![Modelos del panel de navegación](media/how-to-create-portal-experiments/nav-pane-models.png)

1. Seleccione el modelo que registró y seleccione "Crear imagen".

    Puede identificar el modelo por su descripción, que incluirá el identificador de ejecución y el número de iteración con el siguiente formato: *<Identificador_de_ejecución> _ <Número_de_iteración>_Model*

    ![Modelos: Crear imagen](media/how-to-create-portal-experiments/model-create-image.png)

1. Escriba un nombre para la imagen. 
1. Seleccione el botón **Examinar** situado junto al cuadro "Archivo de puntuación" para cargar el archivo de puntuación (scoring.py) que descargó previamente.

1. Seleccione el botón **Examinar** situado junto al cuadro "Archivo de Conda" para cargar el archivo de entorno (condaEnv.yml) que descargó previamente.

    Puede usar su propio script de puntuación y el archivo de Conda, así como cargar archivos adicionales. [Más información sobre el script de puntuación](how-to-deploy-and-where.md#script).

      >[!Important]
      > Los nombres de archivo deben tener menos de 32 caracteres y deben comenzar y terminar con caracteres alfanuméricos. Puede incluir guiones, caracteres de subrayado, puntos y caracteres alfanuméricos. No se permiten espacios.

    ![Crear imagen](media/how-to-create-portal-experiments/create-image.png)

1. Seleccione el botón "Crear" para iniciar la creación de la imagen. Esto tardará unos minutos en completarse. Una vez hecho, aparecerá un mensaje en la barra superior.
1. Vaya a la pestaña "Imágenes", active la casilla situada junto a la imagen que desea implementar y seleccione "Crear implementación". [Más información sobre la implementación](how-to-deploy-and-where.md).

    Hay 2 opciones para la implementación.
     + Azure Container Instances (ACI): se usa más para realizar pruebas en lugar de una implementación operativa a escala. Asegúrese de rellenar los valores para al menos un núcleo para _Capacidad de reserva de CPU_ y al menos un gigabyte (GB) para _Capacidad de reserva de memoria_.
     + Azure Kubernetes Service (AKS): esta opción es para la implementación a escala. Debe tener un proceso basado en AKS preparado.

     ![Imágenes: Creación de una implementación](media/how-to-create-portal-experiments/images-create-deployment.png)

1. Cuando haya terminado, seleccione **Crear**. La implementación del modelo puede tardar varios minutos para que cada canalización termine de ejecutarse.

1. Eso es todo. Tiene un servicio web operativo para generar predicciones.

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre el aprendizaje automático automatizado](concept-automated-ml.md) y Azure Machine Learning.
* [Obtenga información sobre cómo consumir un servicio web](https://docs.microsoft.com/azure/machine-learning/service/how-to-consume-web-service).
