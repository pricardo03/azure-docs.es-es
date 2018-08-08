---
title: Configuración de entornos de ciencia de datos en Azure | Microsoft Docs
description: Configure entornos de ciencia de datos de Azure para utilizarse en el proceso de ciencia de datos en equipos.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 481cfa6a-7ea3-46ac-b0f9-2e3982c37153
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2017
ms.author: deguhath
ms.openlocfilehash: 6c28a64830afeb19c6a9264888b296c3b99990d1
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39262572"
---
# <a name="set-up-data-science-environments-for-use-in-the-team-data-science-process"></a>Configuración de entornos de ciencia de datos para utilizarse en el proceso de ciencia de datos en equipos
El proceso de ciencia de datos en equipos usa una variedad de entornos de ciencia de datos para almacenar, procesar y analizar datos. Incluye Azure Blob Storage, varios tipos de máquinas virtuales de Azure, clústeres de HDInsight (Hadoop) y áreas de trabajo de Azure Machine Learning. La decisión sobre el entorno que se debe usar depende del tipo y de la cantidad de datos que se quieran modelar y del destino de los datos en la nube. 

* Para obtener orientación sobre las cuestiones que se deben tener en cuenta al tomar esta decisión, consulte [Planear su entorno de ciencia de datos de Azure Machine Learning](plan-your-environment.md). 
* Para ver un catálogo de algunos de los escenarios que se pueden encontrar al realizar el análisis avanzado, consulte [Escenarios para análisis avanzado en Aprendizaje automático de Azure](plan-sample-scenarios.md)

Este menú redirige a temas en los que se describe cómo configurar los diversos entornos de ciencia de datos que usa el proceso de ciencia de datos en equipos.

[!INCLUDE [data-science-environment-setup](../../../includes/cap-setup-environments.md)]

**Microsoft Data Science Virtual Machine (DSVM)** también está disponible como una imagen de máquina virtual (VM) de Azure. Esta máquina virtual se preinstala y configura con varias herramientas populares que se usan habitualmente para el análisis de datos y el aprendizaje automático. DSVM está disponible en Windows y Linux. Para más información, consulte [Introducción a Data Science Virtual Machine basada en la nube para Linux y Windows](../data-science-virtual-machine/overview.md).

Aprenda a crear:

- [Windows DSVM](../data-science-virtual-machine/provision-vm.md)
- [Ubuntu DSVM](../data-science-virtual-machine/dsvm-ubuntu-intro.md)
- [CentOS DSVM](../data-science-virtual-machine/linux-dsvm-intro.md)
- [Máquina virtual de aprendizaje profundo](../data-science-virtual-machine/provision-deep-learning-dsvm.md)
