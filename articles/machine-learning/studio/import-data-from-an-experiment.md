---
title: Importación de datos en Machine Learning Studio desde otro experimento con Azure | Microsoft Docs
description: Cómo guardar los datos de aprendizaje en Azure Machine Learning Studio y usarlo en otro experimento.
keywords: importar datos, datos, orígenes de datos, datos de entrenamiento
services: machine-learning
documentationcenter: ''
author: ericlicoding
ms.custom: previous-ms.author=deguhath, previous-author=deguhath
ms.author: amlstudiodocs
editor: cgronlun
ms.assetid: 7da9dcec-5693-4bb6-8166-15904e7f75c3
ms.service: machine-learning
ms.component: studio
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.openlocfilehash: ea6e813d78cafa4e31ea37afa2675f30bccc5c4f
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53258088"
---
# <a name="import-your-data-into-azure-machine-learning-studio-from-another-experiment"></a>Importación de datos en Azure Machine Learning Studio desde otro experimento

Habrá ocasiones en las que querrá tomar un resultado intermedio de un experimento y usarlo como parte de otro experimento. Para ello, guarde el módulo como un conjunto de datos:

1. Haga clic en la salida del módulo que desea guardar como conjunto de datos.
2. Haga clic en **Guardar como conjunto de datos**.
3. Cuando se le solicite, escriba un nombre y una descripción que le permitan identificar fácilmente el conjunto de datos.
4. Haga clic en la marca de verificación **Aceptar** .

Cuando termine de guardar, el conjunto de datos estará disponible para usarlo dentro de cualquier experimento de su área de trabajo. Puede encontrarlo en la lista **Conjuntos de datos guardados** de la paleta de módulos.

