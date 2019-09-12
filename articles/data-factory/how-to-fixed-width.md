---
title: Procesamiento de archivos de texto de longitud fija con Mapping Data Flow en Azure Data Factory
description: Aprenda a procesar archivos de texto de longitud fija en Azure Data Factory mediante Mapping Data Flows.
services: data-factory
author: balakreshnan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 8/18/2019
ms.author: makromer
ms.openlocfilehash: e1ba09f459152616941071c23f7a6545ec2a8b73
ms.sourcegitcommit: d470d4e295bf29a4acf7836ece2f10dabe8e6db2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2019
ms.locfileid: "70210510"
---
# <a name="process-fixed-length-text-files-using-data-factory-mapping-data-flows"></a>Procesamiento de archivos de texto de longitud fija con Data Factory Mapping Data Flow

Data Factory Mapping Data Flow admite datos de transformación procedentes de archivos de texto de ancho fijo. Definirá un conjunto de datos para un archivo de texto sin un delimitador y, a continuación, configurará divisiones de subcadenas basadas en la posición ordinal.

## <a name="create-a-pipeline"></a>Crear una canalización

1. Vaya a **+Nueva canalización** para iniciar una nueva canalización.

2. Agregue una actividad de flujo de datos que se usará para el procesamiento de archivos de ancho fijo

  ![Canalización de ancho fijo](media/data-flow/fwpipe.png)

3. En la actividad de Data Flow, seleccione New Mapping Data Flow (Nueva actividad de Mapping Data Flow).

4. Agregue una transformación de origen, una columna derivada, una transformación de selección y otra de receptor.

  ![Flujo de datos de ancho fijo](media/data-flow/fw2.png)

5. Configure la transformación de origen para que use un nuevo conjunto de datos que será del tipo texto delimitado.

6. No establezca ningún delimitador de columna ni ningún encabezado.

Vamos a establecer simplemente puntos iniciales y longitudes de campo para el contenido de este archivo:

```
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
1234567813572468
```

7. En la pestaña Proyección de la transformación de origen, debería ver una columna de cadena denominada "Column_1".

8. Ahora, en la columna derivada, cree una nueva columna.

9. Asignaremos a las columnas nombres sencillos como col1

10. A continuación, en el generador de expresiones, escriba:

  ```substring(Column_1,1,4)```

  ![columna derivada](media/data-flow/fwderivedcol1.png)

10. Repita este paso para todas las columnas que necesita analizar.

12. Haga clic en la pestaña Inspeccionar para ver las nuevas columnas que se van a generar.

  ![inspeccionar](media/data-flow/fwinspect.png)

13. Use la transformación de selección para quitar todas las columnas que no necesite para la transformación.

  ![seleccionar transformación](media/data-flow/fwselect.png)

14. Por último, use la transformación de receptor para generar los datos en una carpeta:

  ![receptor de ancho fijo](media/data-flow/fwsink.png)

  La salida tendrá un aspecto similar al siguiente:

  ![salida de ancho fijo](media/data-flow/fxdoutput.png)

  Los datos de ancho fijo se dividen ahora con cuatro caracteres cada uno y se asignan a Col1, Col2, Col3, Col4,... Según el ejemplo anterior, estoy dividiendo los datos en 4 columnas.

## <a name="next-steps"></a>Pasos siguientes

* Compile el resto de la lógica del flujo de datos mediante [transformaciones](concepts-data-flow-overview.md) de Mapping Data Flow
