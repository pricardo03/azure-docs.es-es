---
title: 'Seguimiento del progreso de los proyectos de ciencia de datos: Team Data Science Process'
description: Cómo pueden realizar un seguimiento del progreso de un proyecto de ciencia de datos los administradores de grupo de ciencia de datos, los miembros del equipo y los responsables del proyecto.
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/28/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a9616a4f80a3105118b82ce3f4106a65fdd9ddfe
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53134310"
---
# <a name="tracking-the-progress-of-data-science-projects"></a>Seguimiento del progreso de los proyectos de ciencia de datos

Los administradores de grupo de ciencia de datos, los responsables de equipo y de proyecto tienen que realizar el seguimiento del progreso de sus proyectos, qué trabajo se ha realizado en ellos y por quién, y lo que queda en la lista de tareas pendientes. 

## <a name="azure-devops-dashboards"></a>Paneles de Azure DevOps
Si usa Azure DevOps, puede crear paneles para hacer el seguimiento de las actividades y de los elementos de trabajo asociados a un proyecto ágil determinado. 

Para más información sobre cómo crear y personalizar los paneles y widgets en Azure DevOps, consulte los conjuntos de instrucciones siguientes:

- [Agregar y administrar paneles](https://docs.microsoft.com/azure/devops/report/dashboards/dashboards).
- [Agregar widgets a un panel](https://docs.microsoft.com/azure/devops/report/dashboards/add-widget-to-dashboard).

## <a name="example-dashboard"></a>Panel de ejemplo

Este es un panel de ejemplo sencillo que se creó para hacer seguimiento de las actividades de sprint de un proyecto de ciencia de datos ágil, así como también la cantidad de confirmaciones en los repositorios asociados. En el panel en la **parte superior izquierda** se muestra:

- la cuenta regresiva del sprint actual, 
- la cantidad de confirmaciones de cada repositorio durante los últimos siete días,
- el elemento de trabajo para usuarios específicos. 

En el resto de los paneles se muestra el diagrama de flujo acumulado (CFD), la evolución y la evolución ascendente de un proyecto:

- **Inferior izquierda**:  CFD de la cantidad de trabajo con un estado determinado, donde el estado aprobado aparece en gris, el estado confirmado aparece en azul y el estado listo, en verde.
- **Parte superior derecha**: gráfico de evolución del trabajo que falta por completar en contraposición con el tiempo restante.
- **Parte inferior derecha**: gráfico de evolución ascendente del trabajo que se completó en contraposición con la cantidad de trabajo total.

![dashboard](./media/track-progress/dashboard.png)

Para una descripción de cómo crear estos gráficos, consulte las guías de inicio rápido y los tutoriales que aparecen en [Paneles](https://docs.microsoft.com/azure/devops/report/dashboards/).
 
## <a name="next-steps"></a>Pasos siguientes

También se proporcionan tutoriales que muestran todos los pasos del proceso en **escenarios concretos**. Se enumeran y enlazan con descripciones en miniatura en los [tutoriales de ejemplo](walkthroughs.md). En ellos se ilustra cómo combinar herramientas y servicios locales y en la nube en un flujo de trabajo o canalización para crear una aplicación inteligente. 
