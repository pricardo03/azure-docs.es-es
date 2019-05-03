---
title: 'Regresión de red neuronal: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de regresión de red neuronal en el servicio Azure Machine Learning para crear un modelo de regresión usando un algoritmo de red neuronal personalizable...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bc6a7505ab09e929e5add61eea687f871aedf242
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029316"
---
# <a name="neural-network-regression-module"></a>Módulo de regresión de red neuronal

*Crea un modelo de regresión usando un algoritmo de red neuronal*  
  
 Categoría: Machine Learning / inicializar modelo / regresión
  
## <a name="module-overview"></a>Información general del módulo  

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para crear un modelo de regresión usando un algoritmo de red neuronal personalizable.
  
 Aunque las redes neuronales son ampliamente conocidas para su uso en problemas complejos, como el reconocimiento de imágenes de modelado y aprendizaje profundo, se adaptan fácilmente a los problemas de regresión. Cualquier clase de modelos estadísticos puede denominarse una red neuronal si utiliza pesos adaptables y puede aproximar funciones no lineales de sus entradas. Por lo tanto regresión de red neuronal es adecuada para que un modelo de regresión más tradicional no logra una solución de problemas.
  
 Regresión de red neuronal es un método de aprendizaje supervisado y, por lo tanto, requiere una *etiquetados dataset*, que incluye una columna de etiqueta. Dado que un modelo de regresión predice un valor numérico, la columna de etiqueta debe ser un tipo de datos numéricos.  
  
 Puede entrenar el modelo, ya que proporciona el modelo y el conjunto de datos etiquetado como entrada para [entrenar modelo](./train-model.md). El modelo entrenado, a continuación, puede utilizarse para predecir valores para los nuevos ejemplos de entrada.  
  
## <a name="configure-neural-network-regression"></a>Configurar la regresión de red neuronal 

Las redes neuronales se pueden personalizar ampliamente. Esta sección describe cómo crear un modelo mediante dos métodos:
  
+ [Crear un modelo de red neuronal con la arquitectura predeterminada](#bkmk_DefaultArchitecture)  
  
    Si acepta la arquitectura de red neuronal de forma predeterminada, use el **propiedades** panel para establecer los parámetros que controlan el comportamiento de la red neuronal, como el número de nodos de la capa oculta, velocidad de aprendizaje y la normalización.

    Comience aquí si está familiarizado con las redes neuronales. El módulo admite muchas personalizaciones, así como modelo de optimización, sin un conocimiento profundo de las redes neuronales. 

+ Definir una arquitectura para una red neuronal personalizada 

    Use esta opción si desea agregar niveles ocultos adicionales o personalizar completamente las funciones de arquitectura, sus conexiones y la activación de la red.
    
    Esta opción es mejor si ya es un poco familiarizado con las redes neuronales. Use el lenguaje Net # para definir la arquitectura de red.  

##  <a name="bkmk_DefaultArchitecture"></a> Crear un modelo de red neuronal con la arquitectura predeterminada
  
1.  Agregar el **regresión de red neuronal** módulo al experimento en la interfaz. Puede encontrar este módulo en **Machine Learning**, **inicializar**, en el **regresión** categoría. 
  
2. Indicar cómo desea que el modelo se entrene, estableciendo el **crear modo de entrenador** opción.  
  
    -   **Único parámetro**: Elija esta opción si ya sabe cómo desea configurar el modelo.  

3.  En **oculta la especificación de niveles**, seleccione **completamente conectado caso**. Esta opción crea un modelo mediante la arquitectura de red neuronal de forma predeterminada, lo que, para un modelo de regresión de red neuronal tiene estos atributos:  
  
    + La red tiene exactamente una capa oculta.
    + La capa de salida está completamente conectada a la capa oculta y está conectada por completo la capa oculta a la capa de entrada.
    + Se puede establecer el número de nodos de la capa oculta por el usuario (valor predeterminado es 100).  
  
    Dado que el número de nodos en el nivel de entrada viene determinada por el número de características de los datos de entrenamiento, en un modelo de regresión puede haber solo un nodo del nivel de salida.  
  
4. Para **número de nodos ocultos**, escriba el número de nodos ocultos. El valor predeterminado es una capa oculta con 100 nodos. (Esta opción no está disponible si se define una arquitectura personalizada con Net #).
  
5.  Para **velocidad de aprendizaje**, escriba un valor que define el paso llevadas a cabo en cada iteración, antes de la corrección. Un valor mayor para la velocidad de aprendizaje puede hacer que el modelo converja con mayor rapidez, pero puede pasar por alto los mínimos locales.

6.  Para **número de iteraciones de aprendizaje**, especifique el número máximo de veces que el algoritmo procesa los casos de entrenamiento.

7.  Para ** diámetro, escriba un valor que determina los pesos de nodo al principio del proceso de aprendizaje de los pesos del aprendizaje inicial.

8.  Para **el impulso**, escriba un valor que se aplican durante el aprendizaje como un peso en nodos de iteraciones anteriores.

10. Seleccione la opción **ordenar ejemplos aleatoriamente**, para cambiar el orden de los casos entre iteraciones. Si desactiva esta opción, se procesan casos en exactamente el mismo orden cada vez que ejecute el experimento.
  
11. Para **valor de inicialización aleatorio número**, también puede escribir un valor que se usará como valor de inicialización. Especificar un valor de inicialización valor es útil cuando desea asegurar la repetibilidad entre ejecuciones del mismo experimento.
  
13. Conectar un conjunto de datos de entrenamiento y uno de los [módulos de formación](module-reference.md): 
  
    -   Si establece **crear modo de entrenador** a **único parámetro**, utilice [Train Model](./train-model.md).  
  
   
14. Ejecute el experimento.  

## <a name="results"></a>Results

Una vez completada la formación:

+ Para ver un resumen de los parámetros del modelo, junto con la característica ponderaciones aprendidas de aprendizaje y otros parámetros de la red neuronal, haga clic en la salida de [Train Model](./train-model.md)y seleccione **visualizar**.  

+ Para guardar una instantánea del modelo entrenado, haga clic en el **calificado modelo** de salida y seleccione **Guardar como modelo entrenado**. Este modelo no se actualiza en las ejecuciones sucesivas del mismo experimento.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 