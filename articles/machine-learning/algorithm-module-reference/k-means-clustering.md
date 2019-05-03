---
title: 'Agrupación en clústeres K-Means: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de agrupación en clústeres K-means en el servicio de Azure Machine Learning para entrenar modelos de agrupación en clústeres.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 29aeb460581655190b7c3f4256647059f4642d3e
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029706"
---
# <a name="k-means-clustering"></a>Agrupación en clústeres K-Means

En este artículo se describe cómo usar el **agrupación en clústeres K-Means** módulo en Azure Machine Learning Studio para crear un modelo de agrupación en clústeres K-means no entrenado. 
 
K-means es uno de los más sencillos y el más conocido *supervisados* algoritmos de aprendizaje y puede usarse para una variedad de tareas de aprendizaje automático, como [detectar datos anormales](https://msdn.microsoft.com/magazine/jj891054.aspx), agrupación en clústeres de texto documentos y el análisis de un conjunto de datos antes de usar otros métodos de clasificación o regresión. Para crear un modelo de agrupación en clústeres, agregar este módulo para el experimento, conecte un conjunto de datos y establecer los parámetros como el número de clústeres de esperar, la métrica de distancia debe utilizar para crear los clústeres y así sucesivamente. 
  
Después de haber configurado los hiperparámetros de módulo, conecte el modelo no entrenado a los [entrenar el modelo de agrupación en clústeres](train-clustering-model.md) porque el algoritmo K-means es un método de aprendizaje no supervisado, una columna de etiqueta es opcional. 

+ Si los datos incluyen una etiqueta, puede usar los valores de etiqueta para guiar la selección de los clústeres y optimizar el modelo. 

+ Si los datos no tienen ninguna etiqueta, el algoritmo crea clústeres que representa las categorías posibles, basadas únicamente en los datos.  
  

  
##  <a name="understanding-k-means-clustering"></a>Agrupación en clústeres k-means de descripción
 
En general, agrupación en clústeres utiliza técnicas iterativas para agrupar casos en un conjunto de datos en clústeres que contengan características similares. Estas agrupaciones son útiles para explorar datos, identificar anomalías en los datos y, finalmente, para realizar predicciones. Los modelos de clústeres también pueden ayudarle a identificar las relaciones en un conjunto de datos no podría deducir lógicamente mediante la observación de exploración o simple. Por estos motivos, agrupación en clústeres se suele usar en las primeras fases de tareas, de aprendizaje automático para explorar los datos y detectar las correlaciones inesperadas.  
  
 Cuando se configura un modelo de agrupación en clústeres mediante el método k-means, debe especificar un número de destino *k* que indica el número de *centroides* que desee en el modelo. El centroide es un punto que sea representativo de cada clúster. El algoritmo K-means asigna cada punto de datos entrantes a uno de los clústeres minimizando la suma en el clúster de cuadrados. 
 
Al procesar los datos de entrenamiento, el algoritmo K-means comienza con un conjunto inicial de randomnly elegido centroides, que sirven como puntos de partida para cada clúster y se aplica el algoritmo de Lloyd para refinar de manera iterativa las ubicaciones de los centroides. El algoritmo K-means deja de generar y refinar clústeres cuando cumplen una o varias de estas condiciones:  
  
-   Los centroides se estabilizan, lo que significa que ya no podrá cambien las asignaciones de clúster para puntos individuales y el algoritmo converge en una solución.  
  
-   El algoritmo completó la ejecución el número de iteraciones especificado.  
  
 Después de completar la fase de aprendizaje, usa el [asignar datos a clústeres](assign-data-to-clusters.md) módulo para asignar nuevos casos a uno de los clústeres que se ha encontrado el algoritmo k-means. Asignación de clúster se realiza mediante el cálculo de la distancia entre el nuevo caso y el centroide de cada clúster. Cada nuevo caso de se asigna al clúster con el centroide más cercano.  

## <a name="how-to-configure-k-means-clustering"></a>Cómo configurar la agrupación en clústeres K-Means
  
1.  Agregar el **agrupación en clústeres K-Means** módulo al experimento.  
  
2.  Especifique cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.  
  
    -   **Único parámetro**: Si conoce los parámetros exactos que desea usar en el modelo de agrupación en clústeres, puede proporcionar un conjunto específico de valores como argumentos.  
  
   
  
3.  Para **número de Centroides**, escriba el número de clústeres que desea que el algoritmo para empezar.  
  
     El modelo no se garantiza que generen exactamente este número de clústeres. El algorithn comienza con este número de puntos de datos y se recorre en iteración para buscar la configuración óptima.  
  
     
  
4.  Las propiedades **inicialización** se usa para especificar el algoritmo que se usa para definir la configuración inicial del clúster.  
  
    -   **Primeros N**: Un número de puntos de datos inicial es elegido en el conjunto de datos y se utiliza como el medio inicial.  
  
         También se denomina el *método Forgy*.  
  
    -   **Random**: El algoritmo coloca aleatoriamente un punto de datos en un clúster y, a continuación, calcula la media inicial para que sea el centroide del clúster de forma aleatoria puntos asignados.  
  
         También se denomina el *partición aleatoria* método.  
  
    -   **K-Means ++**: Este es el método predeterminado para inicializar los clústeres.  
  
         El **K-means ++** propusieron el algoritmo en 2007 David Arthur y Sergei Vassilvitskii para evitar una agrupación deficiente por el estándar de algoritmo k-means. **K-means ++** mejora K-means estándar mediante el uso de un método diferente para elegir los centros iniciales del clúster.  
  
    
5.  Para **valor de inicialización aleatorio número**, opcionalmente, escriba un valor que se usará como el valor de inicialización para la inicialización de clúster. Este valor puede tener un efecto significativo en la selección del clúster.  
  
    
  
6.  Para **métrica**, elija la función que se usará para medir la distancia entre los vectores de clúster o entre los nuevos puntos de datos y el centroide elegido al azar. Azure Machine Learning admite las siguientes métricas de distancia de clúster:  
  
    -   **Euclidiana**: La distancia Euclidiana se utiliza normalmente como una medida de dispersión de clúster para la agrupación en clústeres K-means. Esta métrica es preferible, ya que minimiza la distancia media entre los puntos y centroides.
  

  
7.  Para **iteraciones**, escriba el número de veces que el algoritmo debe recorrer en iteración los datos de entrenamiento antes de finalizar la selección de centroides.  
  
     Puede ajustar este parámetro para la precisión de equilibrio frente al tiempo de entrenamiento.  
  
8.  Para **modo de asignación de etiqueta**, elija una opción que especifica cómo una columna de etiqueta, si está presente en el conjunto de datos, se debe controlar.  
  
     Como agrupación en clústeres K-means es una método de aprendizaje de automático no supervisada, las etiquetas son opcionales. Sin embargo, si el conjunto de datos ya tiene una columna de etiqueta, puede usar esos valores para guiar la selección de los clústeres, o puede especificar que se omitan los valores.  
  
    -   **Omitir la columna de etiqueta**: Los valores de la columna de etiqueta se omiten y no se usan en la creación del modelo.
  
    -   **Rellenar los valores que faltan**: Los valores de columna de etiqueta se usan como características para ayudar a crear los clústeres. Si faltan una etiqueta de todas las filas, el valor se imputen mediante el uso de otras características.  
  
    -   **Sobrescribir desde más cercana al centro**: Los valores de columna de etiqueta se reemplazan por valores de etiqueta predicha, utilizando la etiqueta del punto más cercana al centroide del actual.  

8.  Seleccione la opción **normalizar características**, si va a normalizar características antes del entrenamiento.
  
     Si aplica la normalización, antes del entrenamiento, los puntos de datos se normalizan en `[0,1]` por MinMaxNormalizer.

10. Entrenar el modelo.  
  
    -   Si establece **crear modo de entrenador** a **único parámetro**, agregue un conjunto de datos etiquetado y entrenar el modelo usando el [entrenar el modelo de agrupación en clústeres](train-clustering-model.md) módulo.  
  

### <a name="results"></a>Results

Cuando haya terminado de configurar y entrenar el modelo, tiene un modelo que puede usar para generar puntuaciones. Sin embargo, hay varias formas para entrenar el modelo y varias maneras de ver y usar los resultados: 

#### <a name="capture-a-snapshot-of-the-model-in-your-workspace"></a>Capturar una instantánea del modelo en el área de trabajo

+ Si ha usado el [entrenar el modelo de agrupación en clústeres](train-clustering-model.md) módulo
    1. Haga clic en el [entrenar el modelo de agrupación en clústeres](train-clustering-model.md) módulo.
    2. Seleccione **calificado modelo** y, a continuación, haga clic en **Guardar como modelo entrenado**.


El modelo guardado representará los datos de entrenamiento en el momento de que guardar el modelo. Si más tarde actualiza los datos de entrenamiento utilizados en el experimento, no se actualizará el modelo guardado. 



#### <a name="see-the-clustering-result-dataset"></a>Ver el conjunto de datos de resultado de la agrupación en clústeres 

+ Si ha usado el [entrenar el modelo de agrupación en clústeres](train-clustering-model.md) módulo
    1. Haga clic en el [entrenar el modelo de agrupación en clústeres](train-clustering-model.md) módulo.
    2. Seleccione **conjunto de datos de resultados** y, a continuación, haga clic en **visualizar**.


### <a name="tips-for-generating-the-best-clustering-model"></a>Sugerencias para generar el mejor modelo de clúster  

Se sabe que el **propagación** utilizado durante la agrupación en clústeres de proceso puede afectar significativamente al modelo. La propagación, significa que la ubicación inicial de puntos en potental centroides.
 
Por ejemplo, si el conjunto de datos contiene muchos valores atípicos y se elige un valor atípico para propagar los clústeres, no hay otros puntos de datos se ajustaría bien con ese clúster y el clúster puede ser un singleton: es decir, un clúster con un solo punto.  
  
Hay varias maneras de evitar este problema:  
  
-   Cambie el número de centroides e intente varios valores de inicialización.  
  
-   Crear varios modelos, variar la métrica o la iteración más.  
  
  
En general, con los modelos de clústeres, es posible que una determinada configuración dará como resultado en un conjunto de clústeres optimizado localmente. En otras palabras, el conjunto de clústeres devueltos por el modelo se adapta a solo los puntos de datos actual y no es generalizable a otros datos. Si usa una configuración inicial diferente, el método K-means podría encontrar una configuración diferente, quizás superior. 
