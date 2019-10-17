---
title: Seguimiento del progreso de proyectos del proceso de ciencia de datos en equipo
description: Cómo pueden realizar un seguimiento del progreso de un proyecto de ciencia de datos los administradores de grupos de ciencia de datos, los responsables de equipos y los responsables de proyectos.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 09/26/2019
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 28bd3e558294c000ba65a1c60fe227bbae7e82dd
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72244076"
---
# <a name="track-the-progress-of-data-science-projects"></a>Seguimiento del progreso de los proyectos de ciencia de datos

Los administradores de grupos de ciencia de datos, los responsables de equipos y los de proyectos pueden realizar el seguimiento del progreso de sus proyectos, qué trabajo se ha realizado en ellos, quién lo ha hecho y el trabajo restante. 

## <a name="azure-devops-dashboards"></a>Paneles de Azure DevOps

Si usa Azure DevOps, puede crear paneles para hacer el seguimiento de las actividades y de los elementos de trabajo asociados a un proyecto ágil determinado. Para más información sobre los paneles, consulte [Dashboards, reports, and widgets](/azure/devops/report/dashboards/) (Paneles, informes y widgets).

Para obtener instrucciones sobre cómo crear y personalizar los paneles y widgets en Azure DevOps, consulte los siguientes inicios rápidos:

- [Agregar y administrar paneles](/azure/devops/report/dashboards/dashboards).
- [Agregar widgets a un panel](/azure/devops/report/dashboards/add-widget-to-dashboard)

## <a name="example-dashboard"></a>Panel de ejemplo

Este es un panel de ejemplo sencillo que hace el seguimiento de las actividades de sprint de un proyecto de ciencia de datos ágil, así como también la cantidad de confirmaciones en los repositorios asociados. 

- El icono de **cuenta atrás** muestra el número de días que quedan en el sprint actual. 

- Los dos **iconos de código** muestran el número de confirmaciones en los dos repositorios de proyectos de los últimos siete días. 

- Los **elementos de trabajo para el proyecto de TDSP del cliente** muestran los resultados de una consulta de todos los elementos de trabajo y su estado. 

- Un **diagrama de flujo acumulado** (CFD) muestra el número de elementos de trabajo cerrados y activos.

- En el **gráfico de evolución** se muestra el trabajo que falta por completar en relación con el tiempo restante del sprint.

- El **gráfico de evolución ascendente** muestra el trabajo completado en comparación con la cantidad total de trabajo del sprint.

![panel](./media/track-progress/dashboard.png)

## <a name="next-steps"></a>Pasos siguientes

[Tutoriales para ejecutar el proceso de ciencia de datos en equipo en Azure](walkthroughs.md) indica los tutoriales que muestran todos los pasos del proceso para escenarios específicos, con vínculos y descripciones en miniatura. Los escenarios vinculados muestran cómo combinar servicios y herramientas en la nube y locales en flujos de trabajo o canalizaciones para crear aplicaciones inteligentes. 
