---
title: Realizar un seguimiento de los datos durante una búsqueda en Azure Sentinel, versión preliminar, con marcadores de búsqueda | Microsoft Docs
description: En este artículo se describe cómo usar los marcadores de búsqueda de Azure Sentinel para llevar un seguimiento de los datos.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 320ccdad-8767-41f3-b083-0bc48f1eeb37
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: aec04c4b9fd56b79a92c2774a48fd55f2f6a9d7a
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/07/2019
ms.locfileid: "67620207"
---
# <a name="keep-track-of-data-during-hunting"></a>Realizar un seguimiento de los datos durante una búsqueda

> [!IMPORTANT]
> Azure Sentinel se encuentra actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
La búsqueda de amenazas suele conllevar revisar una infinidad de datos de registro en busca de pruebas que evidencien comportamientos malintencionados. Durante este proceso, los investigadores dan con eventos que quieren recordar, volver a ver y analizar como parte de la validación de posibles hipótesis y para entender la historia completa de un riesgo.
La búsqueda de marcadores sirve para eso precisamente, ya que conserva las consultas que se han ejecutado en Log Analytics, así como los resultados de consulta que el usuario considere relevantes. También puede registrar las observaciones realizadas dentro de un contexto y hacer referencia a sus hallazgos agregando notas y etiquetas. Los datos marcados están visibles tanto para usted como para sus compañeros de equipo para, así, colaborar de forma más sencilla.   

Puede volver a ver los datos marcados en cualquier momento en la pestaña **Bookmarks** (Marcadores) de la página **Hunting** (Búsqueda). Puede usar opciones de filtrado y de búsqueda para encontrar rápidamente datos concretos de la investigación actual. Si lo desea, también puede ver los datos marcados directamente en la tabla **HuntingBookmark** de Log Analytics. Esto le permite filtrar, resumir y combinar los datos marcados con otros orígenes de datos, lo que facilita el hallazgo de pruebas definitivas.

También puede visualizar los datos marcados haciendo clic en **Investigate** (Investigar). Esto inicia la experiencia de investigación, en la que puede ver, investigar y comunicar visualmente sus hallazgos con un diagrama gráfico de entidades y una escala de tiempo interactivos.


## <a name="run-a-log-analytics-query-from-azure-sentinel"></a>Ejecutar una consulta de Log Analytics desde Azure Sentinel

1. En el portal de Azure Sentinel, haga clic en **Hunting** (Búsqueda) para ejecutar consultas en busca de comportamientos sospechosos y anómalos.

1. Para ejecutar una campaña de búsqueda, seleccione una de las consultas de búsqueda y, a la izquierda, revise los resultados. 

1. Haga clic en **View query results** (Ver resultados de la consulta) en la página **Details** (Detalles) de la consulta de búsqueda para ver los resultados de la consulta en Log Analytics. Este es un ejemplo de lo que se ve si se ejecuta una consulta personalizada de ataque por fuerza bruta de SSH.
  
   ![Mostrar resultados](./media/bookmarks/ssh-bruteforce-example.png)

## <a name="add-a-bookmark"></a>Agregar un marcador

1. En la lista de resultados de consulta de Log Analytics, expanda la fila que contiene la información que sea de su interés.

4. Seleccione los puntos suspensivos (...) al final de la fila y seleccione **Add hunting bookmarks** (Agregar marcadores de búsqueda).
5. A la derecha, en la página **Details** (Detalles), actualice el nombre y agregue etiquetas y notas que le ayuden a recordar por qué ese elemento es de su interés.
6. Haga clic en **Save** (Guardar) para confirmar los cambios. Todos los datos marcados se comparten con otros investigadores, y es un primer paso hacia una experiencia de investigación colaborativa.

   ![Mostrar resultados](./media/bookmarks/add-bookmark-la.png)

 
> [!NOTE]
> Los marcadores también se pueden utilizar con consultas de Log Analytics arbitrarias iniciadas desde la página de registros de Log Analytics de Azure Sentinel, o con consultas creadas sobre la marcha en la página de Log Analytics que se han abierto desde la página de búsqueda. Un marcador no se podrá agregar si Log Analytics se inicia desde fuera de Azure Sentinel. 

## <a name="view-and-update-bookmarks"></a>Ver y actualizar marcadores 

1. En el portal de Azure Sentinel, haga clic en **Hunting** (Búsqueda). 
2. Haga clic en la pestaña **Bookmarks** (Marcadores) en el centro de la página para ver la lista de marcadores.
3. Use las opciones de filtro o el cuadro de búsqueda para encontrar un marcador específico.
4. Seleccione marcadores individuales en la cuadrícula de abajo para ver detalles sobre ese marcador en el panel de detalles de la derecha.
5. Para actualizar las etiquetas y las notas, haga clic en los cuadros de texto editables y, después, en **Save** (Guardar) para conservar los cambios.

   ![Mostrar resultados](./media/bookmarks/view-update-bookmarks.png)

## <a name="view-bookmarked-data-in-log-analytics"></a>Ver datos marcados en Log Analytics 

Existen varias formas de ver los datos marcados en Log Analytics. 

La forma más fácil de ver consultas y resultados marcados o historiales de marcadores consiste en seleccionar el marcador que quiera en la tabla **Bookmarks** (Marcadores) y usar los vínculos que aparecen en el panel de detalles. Las opciones incluyen: 
- Haga clic en **View query** (Ver consulta) para ver la consulta de origen en Log Analytics.  
- Haga clic en **View bookmark history** (Ver historial de marcadores) para ver todos los metadatos de marcadores, como quién realizó la actualización, los valores actualizados y la hora en que se produjo la actualización. 

- También puede ver los datos de marcador sin procesar de todos los marcadores; para ello, haga clic en **Bookmark logs** (Registros de marcadores) encima de la cuadrícula de marcadores. Esta vista mostrará todos los marcadores de la tabla de marcadores de búsqueda con los metadatos asociados. Puede usar consultas KQL para filtrar hasta encontrar la versión más reciente del marcador específico que está buscando.  


> [!NOTE]
> Puede haber un retraso importante (medido en minutos) desde que un marcador se crea hasta que aparece en la tabla **HuntingBookmark**. Se recomienda crear los marcadores primero y analizarlos después de que los datos se hayan ingerido. 

## <a name="delete-a-bookmark"></a>Eliminar un marcador
Haga lo siguiente si quiere eliminar un marcador: 
1.  Abra la pestaña **Hunting bookmark** (Marcador de búsqueda). 
2.  Seleccione el marcador que quiera eliminar.
3.  Seleccione los puntos suspensivos (...) al final de la fila y seleccione **Delete bookmark** (Eliminar marcador).
    
Al eliminar el marcador, este se quita de la lista de la pestaña **Bookmarks** (Marcadores).  La tabla "HuntingBookmark" de Log Analytics seguirá conteniendo entradas de marcadores anteriores, pero la entrada más reciente cambiará el valor de **SoftDelete** a true, lo que permite excluir marcadores antiguos del filtro.  Cuando un marcador se elimina, no se quitan las entidades de la experiencia de investigación que estén relacionadas con otros marcadores o alertas. 


## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a realizar una investigación de búsqueda usando marcadores en Azure Sentinel. Para más información sobre Azure Sentinel, consulte los siguientes artículos:


- [Búsqueda de amenazas con Azure Sentinel, versión preliminar](hunting.md)
- [Uso de cuadernos de Jupyter Notebook para buscar amenazas de seguridad](notebooks.md)
