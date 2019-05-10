---
title: 'Importar desde dirección URL Web a través de HTTP: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar la importación de dirección URL Web a través del módulo HTTP en el servicio de Azure Machine Learning para leer datos de una página Web pública para su uso en un experimento de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: bff913efb38c9e5589c795386dfbbc480d799a37
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65411435"
---
# <a name="import-from-web-url-via-http-module"></a>Importar desde dirección URL Web a través del módulo HTTP

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para leer datos de una página Web pública para su uso en un experimento de aprendizaje automático.

Las siguientes restricciones se aplican a los datos publicados en una página web:

- Datos deben estar en uno de los formatos admitidos: CSV, TSV, ARFF o SvmLight. Otros datos provocará errores.
- Sin autenticación es necesaria o compatible. Datos deben estar disponibles públicamente. 

Hay dos maneras de obtener datos: usar el Asistente para configurar el origen de datos o configurarla manualmente.

## <a name="use-the-data-import-wizard"></a>Use el Asistente para importación de datos

1. Agregar el **importar datos** módulo al experimento. Puede encontrar el módulo en la interfaz, en el **datos de entrada y salida** categoría.

2. Haga clic en **iniciar el Asistente para importar datos** y seleccione la dirección URL Web a través de HTTP.

3. Pegue la dirección URL y seleccione un formato de datos.

4. Cuando la configuración está completa.

Para editar una conexión de datos existente, vuelva a iniciar al asistente. El asistente carga todos los detalles de configuración anterior para que no tiene que volver a empezar desde cero

## <a name="manually-set-properties-in-the-import-data-module"></a>Establecer manualmente las propiedades en el módulo importar datos

Los pasos siguientes describen cómo configurar manualmente el origen de importación.

1. Agregar el [importar datos](import-data.md) módulo al experimento. Puede encontrar el módulo en la interfaz, en el **datos de entrada y salida** categoría.

2. Para **origen de datos**, seleccione **dirección URL Web a través de HTTP**.

3. Para **URL**, escriba o pegue la dirección URL completa de la página que contiene los datos que desea cargar.

    La dirección URL debe incluir la dirección URL del sitio y la ruta de acceso completa, con el nombre de archivo y extensión a la página que contiene los datos que se va a cargar.

    Por ejemplo, la página siguiente contiene el conjunto de datos de Iris desde el repositorio de machine learning de la Universidad de California, Irvine:

    `https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data`

4. Para **formato de datos**, seleccione uno de los datos compatibles formatos de la lista.

    Se recomienda que compruebe siempre los datos con antelación para determinar el formato. La página de UC Irvine utiliza el formato CSV. Otros formatos de datos admitidos son TSV, ARFF y SvmLight.

5. Si los datos están en formato CSV o TSV, use el **archivo tiene fila de encabezado** opción para indicar si el origen de datos incluye una fila de encabezado. La fila de encabezado se utiliza para asignar nombres de columna.

6. Seleccione el **usar en caché de resultados** opciones si no espera que los datos que se va a cambiar mucho o si desea evitar volver a cargar los datos cada vez que ejecutan el experimento.

    Cuando se selecciona esta opción, el experimento carga el tiempo de datos la primera se ejecuta el módulo y, a partir de entonces usa una versión almacenada en caché del conjunto de datos.

    Si desea volver a cargar el conjunto de datos en cada iteración del experimento de conjunto de datos, anule la selección de la **usar en caché de resultados** opción. También se vuelven a cargar los resultados si hay algún cambio a los parámetros de [importar datos](import-data.md).

7. Ejecute el experimento.

## <a name="results"></a>Resultados

Cuando haya terminado, haga clic en el conjunto de datos de salida y seleccione **visualizar** para ver si los datos se importan correctamente.


## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 