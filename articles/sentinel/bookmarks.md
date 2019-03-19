---
title: Realizar un seguimiento de los datos durante la búsqueda en Azure Preview Sentinel con marcadores de búsqueda | Microsoft Docs
description: En este artículo se describe cómo usar los marcadores de búsqueda Centinela de Azure para realizar un seguimiento de los datos.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 320ccdad-8767-41f3-b083-0bc48f1eeb37
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: 9a7ceeab6d2ad761752f778038692256bd87624b
ms.sourcegitcommit: ad019f9b57c7f99652ee665b25b8fef5cd54054d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2019
ms.locfileid: "57242843"
---
# <a name="keep-track-of-data-during-hunting"></a>Realizar un seguimiento de los datos durante la búsqueda

> [!IMPORTANT]
> Sentinel Azure está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
Búsqueda de amenazas normalmente requiere revisión montañas de datos de registro para buscar evidencia de comportamientos malintencionados. Durante este proceso, investigadores de buscar eventos que desean Recuerde, revisar y analizar como parte de la validación posibles hipótesis y comprender la historia completa de un compromiso.
Marcadores de búsqueda le ayudará a hacerlo, conservando las consultas que ejecutó en Log Analytics, junto con los resultados de consulta que se consideran relevantes. También puede registrar las observaciones contextuales y hacen referencia a sus hallazgos mediante la adición de etiquetas y las notas. Marcadores datos son visibles para usted y sus compañeros de equipo para una colaboración simple.   

Puede revisar los datos de marcadores en cualquier momento en el **marcador** pestaña de la **caza** página. Puede usar el filtrado y las opciones para encontrar rápidamente los datos específicos para la investigación actual de búsqueda. Como alternativa, puede ver los datos de marcadores directamente en el **HuntingBookmark** tabla en Log Analytics. Esto le permite filtrar, resumir y combinar datos de marcadores con otros orígenes de datos, lo que facilita buscar evidencia donde se confirmen.

También puede visualizar los datos de marcadores, haciendo clic en **investigar**. Esto inicia la experiencia de investigación en el que puede ver, investigar y comunicar visualmente los hallazgos con un diagrama de gráfico interactivo de la entidad y la escala de tiempo.


## <a name="run-a-log-analytics-query-from-azure-sentinel"></a>Ejecutar una consulta de Log Analytics desde Azure Sentinel

1. En el portal de Azure Sentinel, haga clic en **caza** para ejecutar consultas de comportamiento sospechoso y anómala.

1. Para ejecutar una campaña de búsqueda, seleccione una de las consultas de búsqueda y en la izquierda, revise los resultados. 

1. Haga clic en **ver resultados de la consulta** en la consulta de búsqueda **detalles** resultados de la página para ver la consulta de Log Analytics. Este es un ejemplo de lo que ve si se ejecutaba una consulta personalizada de ataque de bruteforce SSH.
  
   ![Mostrar resultados](./media/bookmarks/ssh-bruteforce-example.png)

## <a name="add-a-bookmark"></a>Agregar un marcador

1. En la lista de resultados de consulta de Log Analytics, expanda la fila que contiene la información que encuentre interesantes.

4. Seleccione el botón de puntos suspensivos (...) al final de la fila y seleccione **agregar marcadores de búsqueda**.
5. A la derecha, en el **detalles** página, actualice el nombre y agregar etiquetas y las notas que le ayudarán a identificar lo que era interesante sobre el elemento.
6. Haga clic en **guardar** para confirmar los cambios. Todos los datos de marcadores se comparte con otros investigadores y es un primer paso hacia una experiencia de investigación de colaboración.

   ![Mostrar resultados](./media/bookmarks/add-bookmark-la.png)

 
> [!NOTE]
> También puede utilizar marcadores con consultas de Log Analytics arbitrarias iniciadas desde la página de registros de análisis de registro Centinela de Azure o las consultas creadas en el Volar desde la página de Log Analytics y se abrirá en la página de búsqueda. No podrá agregar un marcador si inicia Log Analytics desde fuera de Azure Sentinel. 

## <a name="view-and-update-bookmarks"></a>Ver y actualizar los marcadores 

1. En el portal de Azure Sentinel, haga clic en **caza**. 
2. Haga clic en el **marcadores** ficha en el medio de la página para ver la lista de marcadores.
3. Use las opciones de filtro o de cuadro de búsqueda para encontrar un marcador específico.
4. Seleccione sus marcadores individuales a la cuadrícula inferior para ver los detalles de marcador en el panel de detalles de la derecha.
5. Para actualizar las etiquetas y las notas, haga clic en los cuadros de texto editable y **guardar** para conservar los cambios.

   ![Mostrar resultados](./media/bookmarks/view-update-bookmarks.png)

## <a name="view-bookmarked-data-in-log-analytics"></a>Ver datos de marcadores en Log Analytics 

Existen varias opciones para ver los datos de marcadores de Log Analytics. 

Es la manera más fácil de ver consultas de marcado, los resultados o historial seleccionando el marcador deseado en el **marcadores** de tabla y use los vínculos proporcionados en el panel de detalles. Las opciones incluyen: 
- Haga clic en **ver consulta** para ver la consulta de origen en Log Analytics.  
- Haga clic en **ver el historial de marcador** para ver todos los marcadores de metadatos, como: quién realizó la actualización, los valores actualizados y la hora en que se produjo la actualización. 

- También puede ver los datos sin procesar del marcador de todos los marcadores haciendo clic en **marcador registros** encima de la cuadrícula de marcador. Esta vista mostrará todos los marcadores en la tabla de marcador de búsqueda con los metadatos asociados. Puede usar consultas KQL para filtrar hasta encontrar la versión más reciente del marcador específico que está buscando.  


> [!NOTE]
> Puede haber un retraso importante (medido en minutos) entre la creación de un marcador y cuando se muestra en el **HuntingBookmark** tabla. Se recomienda crear los marcadores en primer lugar, a continuación, analizarlos después de que se ingieren los datos. 

## <a name="delete-a-bookmark"></a>Eliminar marcador
Si desea eliminar un realice marcador lo siguiente: 
1.  Abra th **marcador caza** ficha. 
2.  Seleccione el marcador de destino.
3.  Seleccione los puntos suspensivos (...) al final de la fila y seleccione **Eliminar marcador**.
    
Eliminando el marcador, quita el marcador de la lista en el **marcador** ficha.  Log Analytics "HuntingBookmark" tabla seguirá conteniendo las entradas de marcador anteriores, pero la entrada más reciente se cambiará el **SoftDelete** valor en true, lo que facilita filtrar los antiguos marcadores.  Eliminación de un marcador no quita todas las entidades de la experiencia de investigación que están asociados con otros marcadores o alertas. 


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido cómo ejecutar una investigación caza uso de marcadores en Centinela de Azure. Para obtener más información acerca de Centinela de Azure, consulte los artículos siguientes:


- [Captura de forma proactiva en busca de amenazas](hunting.md)
- [Uso de cuadernos para ejecutar campañas caza automatizadas](notebooks.md)