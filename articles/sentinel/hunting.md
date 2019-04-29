---
title: Capacidades de búsqueda en la versión preliminar de Azure Sentinel | Microsoft Docs
description: Este artículo describe cómo usar las capacidades de búsqueda Centinela de Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 6aa9dd27-6506-49c5-8e97-cc1aebecee87
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: adedc8bc1f574ae089f2a11033fab4f390c57a9a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60714868"
---
# <a name="hunt-for-threats-with-in-azure-sentinel-preview"></a>Buscando amenazas con en versión preliminar de Centinela de Azure

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Si eres un investigator que quieran ser proactivo sobre buscando amenazas de seguridad, Azure Sentinel herramientas de búsqueda y consulta de búsqueda eficaces buscando amenazas de seguridad a través de orígenes de datos de su organización. Pero los sistemas y dispositivos de seguridad generan montañas de datos que pueden ser difíciles de analizar y filtrar los eventos significativos. Para ayudar a la seguridad de los analistas buscar proactivamente nuevas anomalías que no se han detectado por las aplicaciones de seguridad, Azure Sentinel' consultas de búsqueda integradas ayudará a hacer las preguntas adecuadas para encontrar problemas en los datos que ya tiene en su red. 

Por ejemplo, una consulta integrada proporciona datos sobre los procesos más habitual que se ejecutan en su infraestructura: no desearía una alerta sobre cada vez que se ejecutan, pueden ser totalmente inofensivo, pero desea Eche un vistazo a la consulta en alguna ocasión para ver si th ere's nada inusual. 



Con Azure Sentinel caza, puede aprovechar las capacidades siguientes:

- Consultas integradas: Para ayudarle a comenzar, una página de inicio proporciona ejemplos de consultas precargadas diseñados para ayudarle a trabajar y familiarizarse con las tablas y el lenguaje de consulta. Estas consultas de búsqueda integradas están desarrolladas por investigadores de seguridad de Microsoft de forma continua, agregar nuevas consultas, y ajuste preciso existente consultas para proporcionarle un punto de entrada para buscar nuevas detecciones y averiguar dónde empezar a buscar el comienzos de nuevos ataques. 

- Lenguaje de consulta eficaz con IntelliSense: Se basa en un lenguaje de consulta que le ofrece la flexibilidad que necesita para realizar la búsqueda al siguiente nivel.

- Cree sus propios marcadores: Durante el proceso de búsqueda, que puede encontrarse coincidencias o las conclusiones, paneles o actividades que tienen un aspecto sospechosa o inusual. Con el fin de marcar esos elementos, por lo que puede volver a ellas en el futuro, utilice la funcionalidad de marcador. Marcadores permiten guardar los elementos para su uso posterior, que se usará para crear un caso de investigación. Para obtener más información acerca de los marcadores, consulte usar [marcadores de búsqueda].

- Utilizar blocs de notas para automatizar la investigación: Blocs de notas son como guías paso a paso que se pueden compilar para recorrer los pasos de una investigación y de captura.  Cuadernos de encapsulan todos los pasos de búsqueda de un guión de procedimientos reutilizable que se pueden compartir con otras personas de su organización. 
- Consulta los datos almacenados: Los datos son accesibles en las tablas para que pueda consultarlos. Por ejemplo, puede consultar la creación del proceso, los eventos DNS y muchos otros tipos de eventos.

- Vínculos a la Comunidad: Aproveche la eficacia de la mayor comunidad para encontrar los orígenes de datos y consultas adicionales.
 
## <a name="get-started-hunting"></a>Introducción a la caza

1. En el portal de Azure Sentinel, haga clic en **caza**.
  ![Centinela Azure comienza la búsqueda](media/tutorial-hunting/hunting-start.png)

2. Al abrir el **caza** página, todas las consultas de búsqueda se muestran en una sola tabla. La tabla enumeran todas las consultas escritas por el equipo de Microsoft de analistas de seguridad, así como cualquier consulta adicional creado o modificado. Cada consulta proporciona una descripción de lo que busca para y qué tipo de datos que se ejecuta. Estas plantillas se agrupan por sus diversas tácticas: los iconos de la derecha clasificación el tipo de amenaza, por ejemplo, el acceso inicial, la persistencia y filtración. Puede filtrar estas plantillas de consulta de búsqueda mediante cualquiera de los campos. Puede guardar cualquier consulta a sus favoritos. Al guardar una consulta a sus favoritos, automáticamente ejecuta la consulta cada vez que el **caza** se tiene acceso a la página. Puede crear sus propias consultas de búsqueda o un clon y personalizar una plantilla de consulta de búsqueda existente. 
 
2. Haga clic en **Ejecutar consulta** en la búsqueda de la página de detalles de la consulta para ejecutar cualquier consulta sin salir de la página de búsqueda.  Se muestra el número de coincidencias dentro de la tabla. Revise la lista de las consultas de búsqueda y sus coincidencias. Consulte qué fase de la cadena de interrupción está asociada a la coincidencia.

3. Realizar una revisión rápida de la consulta subyacente en el panel de detalles de la consulta o haga clic en **ver el resultado de la consulta** para abrir la consulta de Log Analytics. En la parte inferior, revise a las coincidencias de la consulta.

4.  Haga clic en la fila y seleccione **Agregar marcador** para agregar las filas investigarse - puede hacer esto para cualquier elemento que parece sospechoso. 

5. A continuación, vuelva a la principal **caza** página y haga clic en el **marcadores** pestaña para ver todas las actividades sospechosas. 

6. Seleccione un marcador y, a continuación, haga clic en **investigar** para abrir la experiencia de investigación. Puede filtrar los marcadores. Por ejemplo, si está investigando una campaña, puede crear una etiqueta para la campaña y, a continuación, filtrar todos los marcadores en función de la campaña.

1. Una vez que detecta qué consulta de búsqueda proporciona información de gran valor sobre posibles ataques, también puede crear detección personalizada reglas basándose en la consulta y exponer dicha información como alertas en los servicios de respuesta de incidentes de seguridad.

 

## <a name="query-language"></a>Lenguaje de consulta 

Caza de Centinela de Azure se basa en el lenguaje de consulta de Azure Log Analytics. Para obtener más información sobre el lenguaje de consulta y los operadores compatibles, consulte [referencia del lenguaje de consulta](https://docs.loganalytics.io/docs/Language-Reference/).

## <a name="public-hunting-query-github-repository"></a>Repositorio de GitHub de consulta de búsqueda pública

Consulte la [repositorio de consultas de búsqueda](https://github.com/Azure/Orion). Contribuir y usar consultas de ejemplo compartidas por nuestros clientes.

 

## <a name="sample-query"></a>Consulta de ejemplo

Una consulta típica empieza con un nombre de tabla seguido por una serie de operadores separados por \|.

En el ejemplo anterior, empiece con la tabla nombre SecurityEvent y agregaremos elementos canalizados según sea necesario.

1. Definir un filtro de tiempo para revisar solo los registros de los siete días anteriores.

2. Agregar un filtro en la consulta para mostrar solo el evento 4688 de identificador.

3. Agregar un filtro en la consulta en la línea de comandos que contiene solo instancias de cscript.exe.

4. Proyecto solo las columnas que está interesado en explorar y limita los resultados a 1000 y haga clic en **Ejecutar consulta**.
5. Haga clic en el triángulo verde y ejecute la consulta. Puede probar la consulta y ejecútela para buscar comportamientos anómalos.

## <a name="useful-operators"></a>Operadores útiles

El lenguaje de consulta es eficaz y tiene numerosos operadores disponibles, se enumeran algunos operadores útiles:

**donde** -una tabla al subconjunto de filas que cumplen un predicado de filtro.

**resumir** -generar una tabla que se agrega el contenido de la tabla de entrada.

**combinación** -combinar las filas de dos tablas para formar una nueva tabla haciendo coincidir los valores de las columnas especificadas de cada tabla.

**recuento** -devuelve el número de registros en el conjunto de registros de entrada.

**Top** -registros de devuelven las primeras N ordenados por las columnas especificadas.

**límite** -devolver hasta el número especificado de filas.

**proyecto** : seleccionar las columnas que desea incluir, cambiar el nombre o quitar e insertar nuevas columnas calculadas.

**extender** : crear columnas calculadas y anexarlos al conjunto de resultados.

**makeset** -devolver una matriz de (JSON) dinámica del conjunto de valores distintos que toma la expresión en el grupo

**buscar** -buscar filas que coinciden con un predicado a través de un conjunto de tablas.

## <a name="save-a-query"></a>Almacenamiento de una consulta

Puede crear o modificar una consulta y guárdelo como su propia consulta o compartirlo con los usuarios que están en el mismo inquilino.

   ![Guardar consulta](./media/tutorial-hunting/save-query.png)

Cree una nueva consulta de búsqueda:

1. Haga clic en **nueva consulta** y seleccione **guardar**.
2. Rellene todos los campos en blanco y seleccione **guardar**.

   ![Nueva consulta](./media/tutorial-hunting/new-query.png)

Clonar y modificar una consulta de búsqueda existente:

1. Seleccione la consulta de búsqueda en la tabla que desea modificar.
2. Seleccione el botón de puntos suspensivos (...) en la línea de la consulta que desea modificar y seleccione **clonar consulta**.

   ![clonar consulta](./media/tutorial-hunting/clone-query.png)
 

3. Modificar la consulta y seleccione **crear**.

   ![consulta personalizada](./media/tutorial-hunting/custom-query.png)

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido cómo ejecutar una investigación de búsqueda con Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:


- [Uso de cuadernos para ejecutar campañas caza automatizadas](notebooks.md)
- [Uso de marcadores para guardar información interesante durante la búsqueda](bookmarks.md)