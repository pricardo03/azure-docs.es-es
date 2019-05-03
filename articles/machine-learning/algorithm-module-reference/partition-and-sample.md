---
title: 'Crear particiones y muestrear: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo partición y ejemplo de servicio de Azure Machine Learning para realizar un muestreo en un conjunto de datos o para crear particiones del conjunto de datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 72f9e11e3582d804eecc7479ea079276564bd12f
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029286"
---
# <a name="partition-and-sample-module"></a>Módulo partición y ejemplo

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para realizar un muestreo en un conjunto de datos o para crear particiones del conjunto de datos.

El muestreo es una herramienta importante en aprendizaje automático porque permite reducir el tamaño de un conjunto de datos manteniendo la misma proporción de valores. Este módulo admite varias tareas relacionadas que son importantes en machine learning: 

- Dividir los datos en varias subsecciones del mismo tamaño. 

    Puede usar las particiones para la validación cruzada o para asignar casos a grupos aleatorios.

- Al separar los datos en grupos y, a continuación, trabajar con datos de un grupo específico. 

    Después de asignar aleatoriamente los casos a grupos diferentes, deberá modificar las características que están asociadas con un único grupo.

- Muestreo. 

    Puede extraer un porcentaje de los datos, aplicar un muestreo aleatorio o elegir una columna a usar para equilibrar el conjunto de datos y realizar el muestreo estratificado de sus valores.

- Creación de un conjunto de datos más pequeño para las pruebas. 

    Si tiene una gran cantidad de datos, desea utilizar solo el primer *n* filas al configurar el experimento y, a continuación, cambie para usar el conjunto de datos completa al compilar el modelo. También puede usar muestreo para crear s de conjunto de datos más pequeño para su uso en desarrollo.

## <a name="configure-partition-and-sample"></a>Configuración de partición y ejemplo

Este módulo admite varios métodos para dividir los datos en particiones o para el muestreo. Elija el método primero y, a continuación, establecer las opciones adicionales requieren por el método.

- Principal
- muestreo
- Asignar a subconjuntos
- Seleccionar subconjunto

### <a name="get-top-n-rows-from-a-dataset"></a>Obtener filas TOP N de un conjunto de datos

Utilice este modo para obtener sólo la primera *n* filas. Esta opción es útil si desea probar un experimento en un pequeño número de filas y no necesita los datos con equilibrio o muestreados en ningún modo.

1. Agregar el **partición y ejemplo** módulo al experimento en la interfaz y conecte el conjunto de datos.  

2. **Modo de partición o ejemplo**: Establezca esta opción en **Head**.

3. **Número de filas para seleccionar**: Escriba el número de filas que se va a devolver.

    El número de filas que especifique debe ser un entero no negativo. Si el número de filas seleccionadas es superior al número de filas del conjunto de datos, se devuelve el conjunto de datos completo.

4. Ejecute el experimento.

El módulo genera un único conjunto de datos que contiene solo el número especificado de filas. Las filas siempre se leen desde la parte superior del conjunto de datos.

### <a name="create-a-sample-of-data"></a>Crear una muestra de datos

Esta opción es compatible con un muestreo aleatorio simple o un muestreo aleatorio estratificado. Esto es útil si desea crear un conjunto de datos más pequeño muestra representativa para las pruebas.

1. Agregar el **partición y ejemplo** módulo para el experimento y conecte el conjunto de datos.

2. **Modo de partición o ejemplo**: Establezca esta opción en **muestreo**.

3. **Tasa de muestreo**: Escriba un valor entre 0 y 1. Este valor especifica el porcentaje de filas del conjunto de datos de origen que deben incluirse en el conjunto de datos de salida.

    Por ejemplo, si desea que sólo la mitad del conjunto de datos original, escriba `0.5` para indicar que la frecuencia de muestreo debe ser un 50%.

    Las filas del conjunto de datos de entrada se ordenan aleatoriamente y se colocan selectivamente en el conjunto de datos de salida, según la relación especificada.

4. **Valor de inicialización aleatorio para el muestreo**: Opcionalmente, escriba un número entero que se usará como un valor de inicialización.

    Esta opción es importante si desea que las filas que se va a dividir la misma manera cada vez. El valor predeterminado es 0, el significado que se genera un valor de inicialización inicial según el reloj del sistema. Esto puede conducir a resultados ligeramente diferentes cada vez que ejecute el experimento.

5. **División estratificada para el muestreo**: Seleccione esta opción si es importante que las filas del conjunto de datos deben dividirse uniformemente por alguna columna clave antes de muestreo.

    Para **columna de clave de estratificación para el muestreo**, seleccione una sola *columna de estratos* utilizar al dividir el conjunto de datos. Las filas del conjunto de datos, a continuación, se dividen como sigue:

    1. Todas las filas de entrada se agrupan (estratificadas) por los valores de la columna de estratos especificada.

    2. Las filas se ordenan aleatoriamente dentro de cada grupo.

    3. Cada grupo se agrega de forma selectiva al conjunto de datos de salida para cumplir con la relación especificada.


6. Ejecute el experimento.

    Con esta opción, el módulo genera un único conjunto de datos que contiene una muestra representativa de los datos. La parte sin muestreo restante del conjunto de datos no es de salida. 

## <a name="split-data-into-partitions"></a>Dividir los datos en particiones

Use esta opción si desea dividir el conjunto de datos en subconjuntos de los datos. Esta opción también es útil cuando desea crear un número personalizado de pliegues para la validación cruzada o dividir filas en varios grupos.

1. Agregar el **partición y ejemplo** módulo para el experimento y conecte el conjunto de datos.

2. Para **partición o en modo de ejemplo**, seleccione **asignar a pliegues**.

3. **Usar reemplazo en la creación de particiones**: Seleccione esta opción si desea que la fila muestreada se vuelva a poner en el grupo de filas para poder reutilizarse. Como resultado, la misma fila podría asignarse a varios subconjuntos.

    Si no utiliza el reemplazo (opción predeterminada), la fila muestreada no se coloca en el grupo de filas para poder reutilizarse. Como resultado, cada fila puede asignarse a solo un subconjunto.

4. **División aleatoria**:  Seleccione esta opción si desea que las filas que se asignarán aleatoriamente a subconjuntos.

    Si no selecciona esta opción, las filas se asignan a los pliegues con el método round-robin.

5. **Valor de inicialización aleatorio**: Opcionalmente, escriba un número entero que se usará como el valor de inicialización. Esta opción es importante si desea que las filas que se va a dividir la misma manera cada vez. En caso contrario, el valor predeterminado de 0 significa que uno aleatorio se usará el valor de inicialización.

6. **Especifique el método del particionador**: Indicar cómo desea que los datos que se distribuyen en cada partición, con estas opciones:

    - **Partición uniforme**: Use esta opción para colocar un número igual de filas en cada partición. Para especificar el número de particiones de salida, escriba un número entero en el **especificar el número de subconjuntos para dividir uniformemente en** cuadro de texto.

    - **Partición con proporciones personalizadas**: Use esta opción para especificar el tamaño de cada partición como una lista separada por comas.

        Por ejemplo, si desea crear tres particiones, con la primera partición que contiene el 50% de los datos y las dos particiones restantes cada con un 25% de los datos, haga clic en el **lista de proporciones separadas por comas** cuadro de texto, y Escriba estos números: `.5, .25, .25`

        Debe agregar la suma de todos los tamaños de partición hasta exactamente 1.

        - Si escribe números que se suman a **menos de 1**, se crea una partición adicional para contener las filas restantes. Por ejemplo, si escribe se crea la partición de los valores.2 y.3, un tercero que contiene el 50 por ciento restante de todas las filas.

        - Si escribe números que se suman a **más de 1**, se produce un error al ejecutar el experimento.

7. **División estratificada**: Seleccione esta opción si desea que las filas se estratifiquen al dividir y, a continuación, elija el _columna de estratos_.

8. Ejecute el experimento.

    Con esta opción, el módulo genera varios conjuntos de datos con particiones mediante las reglas que especificó.

### <a name="use-data-from-a-predefined-partition"></a>Usar datos de una partición predefinida  

Esta opción se usa cuando se divide un conjunto de datos en varias particiones y ahora desea cargar cada partición a su vez para su posterior análisis o procesamiento.

1. Agregar el **partición y ejemplo** módulo en el experimento.

2. Conectarse a la salida de una instancia anterior de **partición y ejemplo**. Esa instancia debe haber usado el **asignar a pliegues** opción para generar un número de particiones.

3. **Modo de partición o ejemplo**: Seleccione **Seleccionar subconjunto**.

4. **Especificar el subconjunto del que para se va a muestrear**: Seleccione una partición que se utilizará para ello, escriba su índice. Índices de partición están basadas en 1. Por ejemplo, si divide el conjunto de datos en tres partes, las particiones tendría los índices de 1, 2 y 3.

    Si escribe un valor de índice no válido, se produce un error en tiempo de diseño: "Error 0018: Conjunto de datos contiene datos no válidos."

    Además del conjunto de datos de agrupación por subconjuntos, puede separar el conjunto de datos en dos grupos: un subconjunto de destino y todo lo demás. Para ello, escriba el índice de un subconjunto único y, a continuación, seleccione la opción **seleccionar complemento del subconjunto seleccionado**, para obtener todo excepto los datos en el subconjunto especificado.

5. Si trabaja con varias particiones, debe agregar instancias adicionales de la **partición y ejemplo** módulo para controlar cada partición.

    Por ejemplo, supongamos que a los pacientes con particiones previamente en subconjuntos de cuatro mediante age. Para trabajar con cada subconjunto individual, necesita cuatro copias de la **partición y ejemplo** módulo, y en cada uno, seleccione un subconjunto diferente, como se muestra a continuación. No es correcto usar la **asignar a pliegues** directamente de salida.  

    [![Partición y ejemplo](./media/partition-and-sample/partition-and-sample.png)](./media/partition-and-sample/partition-and-sample-lg.png#lightbox)

5. Ejecute el experimento.

    Con esta opción, el módulo genera un único conjunto de datos que contiene solo las filas que se asigna a ese subconjunto.

> [!NOTE]
>  No se puede ver las designaciones de plegamiento directamente; están presentes solo en los metadatos.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 