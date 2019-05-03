---
title: 'Limpiar datos que faltan: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo de limpiar datos que faltan en el servicio de Azure Machine Learning para quitar, reemplazar o deducir los valores que faltan.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: de81204219a102734f1820258a3c32e59a64c685
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028791"
---
# <a name="clean-missing-data-module"></a>Módulo de limpiar datos que faltan

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para quitar, reemplazar o deducir los valores que faltan. 

Los científicos de datos a menudo comprobación los datos para los valores que faltan y, a continuación, realizan varias operaciones para corregir los datos o insertar nuevos valores. El objetivo de estas operaciones de limpieza es evitar problemas causados por falta de datos que pueden surgir al entrenar un modelo. 

Este módulo admite varios tipo de operaciones para "limpiar" valores que faltan, incluidos:

+ Reemplace los valores que faltan con un marcador de posición, promedio u otro valor
+ Eliminación completa de las filas y columnas que tienen valores ausentes
+ Deducir los valores en función de los métodos estadísticos


Con este módulo no cambia el conjunto de datos de origen. En su lugar, crea un nuevo conjunto de datos en el área de trabajo que puede usar en el flujo de trabajo posterior. También puede guardar el conjunto de datos nuevo y limpio para reutilizarlo.

Este módulo también genera una definición de la transformación que usó para limpiar los valores que faltan. Puede volver a usar esta transformación en otros conjuntos de datos que tengan el mismo esquema, mediante el [aplicar transformación](./apply-transformation.md) módulo.  

## <a name="how-to-use-clean-missing-data"></a>Uso de limpiar datos que faltan

Este módulo le permite definir una operación de limpieza. También puede guardar la operación de limpieza para que pueda aplicar adelante a los nuevos datos. Vea los siguientes vínculos para obtener una descripción de cómo crear y guardar un proceso de limpieza: 
 
+ Para reemplazar los valores que faltan
  
+ Para aplicar una transformación de limpieza a los nuevos datos
 
> [!IMPORTANT]
> El método de limpieza que usa para controlar los valores que faltan puede afectar drásticamente a los resultados. Se recomienda que experimente con diferentes métodos. Tenga en cuenta la justificación para su uso de un método concreto y la calidad de los resultados.

### <a name="replace-missing-values"></a>Reemplace los valores que faltan  

Cada vez que se aplica los [Clean Missing Data](./clean-missing-data.md) módulo a un conjunto de datos, la misma operación de limpieza se aplica a todas las columnas que seleccione. Por lo tanto, si tiene que limpiar mediante distintos métodos de distintas columnas, utilice instancias independientes del módulo.

1.  Agregar el [Clean Missing Data](./clean-missing-data.md) módulo para el experimento y conecte el conjunto de datos que tiene valores que faltan.  
  
2.  Para **columnas que desea limpiar**, elija las columnas que contienen los valores que faltan que desee cambiar. Puede elegir varias columnas, pero debe usar el mismo método de reemplazo en todas las columnas seleccionadas. Por lo tanto, normalmente deberá limpiar las columnas de cadena y las columnas numéricas por separado.

    Por ejemplo, para comprobar si hay valores que faltan en todas las columnas numéricas:

    1. Abra el Selector de columnas y seleccione **con reglas**.
    2. Para **comenzar con**, seleccione **columnas n**.

        Puede también iniciar con todas las columnas y, a continuación, excluir columnas. Inicialmente, las reglas no se muestran si haga clic primero en **todas las columnas**, pero puede hacer clic en **columnas n** y, a continuación, haga clic en **todas las columnas** nuevo para comenzar con todas las columnas y, a continuación, filtrar las columnas de (excluir) según el nombre, tipo de datos, o índice de columnas.

    3. Para **Include**, seleccione **tipo de columna** desde la lista desplegable y, a continuación, seleccione **numérico**, o un tipo numérico más específico. 
  
    Cualquier método de limpieza o de reemplazo que elija debe ser aplicable a **todas** columnas de la selección. Si los datos de cualquier columna no están compatibles con la operación especificada, el módulo devuelve un error y detiene el experimento.
  
3.  Para **mínimo valor proporción que faltan**, especifique el número mínimo de valores que faltan necesarios para que realizar la operación.  
  
    Use esta opción en combinación con **máximo valor proporción que faltan** para definir las condiciones en las que se realiza una operación de limpieza en el conjunto de datos. Si hay demasiados o muy pocas filas que son valores que faltan, no se puede realizar la operación. 
  
    El número que especifique representa el **proporción** de valores que faltan para todos los valores de la columna. De forma predeterminada, el **proporción de valores que faltan mínimo** propiedad está establecida en 0. Esto significa que los valores que faltan se limpian incluso si hay solo un valor que falta. 

    > [!WARNING]
    > Esta condición debe cumplirse por cada columna en orden para la operación especificada aplicar. Por ejemplo, supongamos que tres de las columnas seleccionadas y, a continuación, establezca la relación mínima de valores que faltan.2 (20%), pero en realidad, solo una columna tiene 20% de valores que faltan. En este caso, la operación de limpieza se aplicaría solo a la columna con más del 20% los valores que faltan. Por lo tanto, las demás columnas sería sin cambios.
    > 
    > Si tiene alguna duda sobre si han cambiado los valores que faltan, seleccione la opción **generar columna de indicador de valor que falta**. Una columna se anexa al conjunto de datos para indicar si cada columna cumplen los criterios especificados para los intervalos mínimos y máximo.  
  
4. Para **máximo valor proporción que faltan**, especifique el número máximo de valores que faltan que pueden estar presentes para que se puede realizar la operación.   
  
    Por ejemplo, desea realizar la sustitución de valores ausentes solo si el 30% o menos de las filas contienen valores que faltan, pero deje los valores como-es si más del 30% de las filas tienen valores que faltan.  
  
    El número se define como la proporción de valores que faltan para todos los valores de la columna. De forma predeterminada, el **máximo valor proporción que faltan** está establecido en 1. Esto significa que los valores que faltan se limpian incluso si faltan 100% de los valores de la columna.  
  
   
  
5. Para **modo de limpieza**, seleccione una de las siguientes opciones para reemplazar o quitar faltan valores:  
  
  
    + **Valor de sustitución personalizado**: Utilice esta opción para especificar un valor de marcador de posición (por ejemplo, un valor 0 o NA) que se aplica a todos los valores que faltan. El valor que especifique como un valor de reemplazo debe ser compatible con el tipo de datos de la columna.
  
    + **Reemplazar por la Media**: Calcula la media de la columna y la media se utiliza como valor de reemplazo para cada valor que falta en la columna.  
  
        Solo se aplica a las columnas que tienen Integer, Double o tipos de datos booleanos.  
  
    + **Reemplazar por la mediana**: Calcula el valor medio de la columna y usa el valor medio como el valor de reemplazo para cualquier valor que falta en la columna.  
  
        Solo se aplica a las columnas que tienen tipos de datos Integer o Double. 
  
    + **Reemplazar por el modo**: Calcula el modo de la columna y usa el modo como el valor de reemplazo para cada valor que falta en la columna.  
  
        Se aplica a las columnas que tienen tipos de datos Integer, Double, Boolean o Categorical. 
  
    + **Quitar la fila entera**: Quita completamente cualquier fila del conjunto de datos que tiene uno o más valores que faltan. Esto es útil si el valor que falta puede considerarse como ausente de forma aleatoria.  
  
    + **Quitar toda la columna**: Quita completamente cualquier columna del conjunto de datos que tiene uno o más valores que faltan.  
  
    
  
6. La opción **valor de reemplazo** está disponible si ha seleccionado la opción **valor de sustitución personalizado**. Escriba un nuevo valor que se usará como el valor de reemplazo para todos los valores que faltan en la columna.  
  
    Tenga en cuenta que puede usar esta opción únicamente en las columnas que tienen los tipos de datos Integer, Double, Boolean o Date. Para las columnas de fecha, el valor de reemplazo también se puede introducir como el número de pasos de 100 nanosegundos desde el 1/1/0001 12:00 A.M.  
  
7. **Generar columna de indicador de valor que falta**: Seleccione esta opción si desea dar salida alguna indicación de si los valores de la columna cumplen los criterios para la limpieza de valores que faltan. Esta opción es especialmente útil cuando va a configurar una nueva operación de limpieza y desea asegurarse de que funciona según lo previsto.
  
8. Ejecute el experimento.

### <a name="results"></a>Results

El módulo devuelve dos resultados:  

-   **Conjunto de datos limpio**: Un conjunto de datos consta de las columnas seleccionadas con los valores que faltan se trata como especificado, junto con una columna de indicador, si selecciona dicha opción.  

    Las columnas no seleccionadas para la limpieza también se "pasan a través de".  
  
-  **Transformación de limpieza**: Una transformación de datos utilizada para la limpieza, que se puede guardar en el área de trabajo y aplican a los datos nuevo más tarde.

### <a name="apply-a-saved-cleaning-operation-to-new-data"></a>Una operación de limpieza guardada se aplican a los nuevos datos  

Si necesita repetir operaciones de limpieza a menudo, se recomienda que guarde la receta para la limpieza de datos como un *transformar*, para volver a usar con el mismo conjunto de datos. Guardar una transformación de limpieza es especialmente útil si debe volver a importar con frecuencia y, a continuación, limpiar datos que tiene el mismo esquema.  
      
1.  Agregar el [aplicar transformación](./apply-transformation.md) módulo al experimento.  
  
2.  Agregue el conjunto de datos que desea limpiar y conecte el conjunto de datos al puerto de entrada derecho.  
  
3.  Expanda el **transforma** grupo en el panel izquierdo de la interfaz. Busque la transformación guardada y arrástrela hasta el experimento.  
  
4.  Conecte la transformación guardada en el puerto de entrada izquierdo [aplicar transformación](./apply-transformation.md). 

    Al aplicar una transformación guardada, no puede seleccionar las columnas a la que se aplica la transformación. Eso es porque la transformación ya se ha definido y se aplica automáticamente a las columnas especificadas en la operación original.

    Sin embargo, supongamos que ha creado una transformación en un subconjunto de las columnas numéricas. Puede aplicar esta transformación a un conjunto de datos de tipos de columna mixto sin provocar un error, porque se cambian los valores que faltan sólo en las columnas numéricas coincidentes.

6.  Ejecute el experimento.  

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 