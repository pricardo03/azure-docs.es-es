---
title: 'Importar desde dirección web mediante HTTP: Referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Importar desde dirección web mediante HTTP en Azure Machine Learning Service para leer datos de una página web pública para usarlos en un experimento de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ms.openlocfilehash: 59b8e2e73b9904a503c16d8891e5a5bd771fc87f
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70128751"
---
# <a name="import-from-web-url-via-http-module"></a>Módulo Importar desde dirección web mediante HTTP

En este artículo se describe un módulo de la interfaz visual (versión preliminar) de Azure Machine Learning Service.

Utilice este módulo para leer datos de una página web pública para usarlos en un experimento de aprendizaje automático.

Los datos publicados en una página web tienen las siguientes restricciones:

- Los datos deben estar en uno de los formatos admitidos: CSV, TSV, ARFF o SvmLight. Otros datos provocarán errores.
- No se requiere ni se admite la autenticación. Los datos deben estar disponibles públicamente. 

Hay dos maneras de obtener datos: usar el asistente para configurar el origen de datos o configurarlo manualmente.

## <a name="use-the-data-import-wizard"></a>Usar el Asistente para importación de datos

1. Agregue el módulo **Importar datos** al experimento. Puede encontrar el módulo en la interfaz, en la categoría **Entrada y salida de datos**.

2. Haga clic en **Iniciar el Asistente para importación de datos** y seleccione Dirección web mediante HTTP.

3. Pegue la dirección URL y seleccione un formato de datos.

4. Ya está configurado.

Para editar una conexión de datos existente, vuelva a iniciar el asistente. El asistente carga todos los detalles de configuración anteriores para que no tenga que volver a empezar desde cero

## <a name="manually-set-properties-in-the-import-data-module"></a>Establecer manualmente las propiedades en el módulo Importar datos

En los pasos siguientes se describe cómo configurar manualmente el origen de la importación.

1. Agregue el módulo [Importar datos](import-data.md) al experimento. Puede encontrar el módulo en la interfaz, en la categoría **Entrada y salida de datos**.

2. Para **Origen de datos**, seleccione **Dirección web mediante HTTP**.

3. Para **URL**, escriba o pegue la dirección URL completa de la página que contiene los datos que desea cargar.

    La dirección URL debe incluir la dirección URL del sitio y la ruta de acceso completa a la página que contiene los datos que se van a cargar, incluyendo el nombre de archivo y la extensión.

    Por ejemplo, la página siguiente contiene el conjunto de datos Iris del repositorio de aprendizaje automático de la Universidad de California, Irvine:

    `https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data`

4. Para **Formato de datos**, seleccione uno de los formatos de datos admitidos de la lista.

    Se recomienda que compruebe siempre los datos con antelación para determinar el formato. La página de la UC Irvine utiliza el formato CSV. Otros formatos de datos admitidos son TSV, ARFF y SvmLight.

5. Si los datos están en formato CSV o TSV, use la opción **El archivo tiene fila de encabezado** para indicar si el origen de datos incluye una fila de encabezado. La fila de encabezado se usa para asignar nombres de columna.

6. Seleccione la opción **Usar resultados en caché** si no prevé que los datos cambien mucho o si desea evitar tener que volver a cargar los datos cada vez que ejecute el experimento.

    Cuando se selecciona esta opción, el experimento carga los datos la primera vez que se ejecuta el módulo y, a partir de entonces, usa una versión almacenada en caché del conjunto de datos.

    Si desea volver a cargar el conjunto de datos en cada iteración del conjunto de datos del experimento, anule la selección de la opción **Usar resultados en caché**. Los resultados también se vuelven a cargar si hay algún cambio en los parámetros de [Importar datos](import-data.md).

7. Ejecute el experimento.

## <a name="results"></a>Results

Cuando haya terminado, haga clic en el conjunto de datos de salida y seleccione **Visualizar** para ver si los datos se han importado correctamente.


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 