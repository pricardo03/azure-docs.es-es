---
title: Agrupación de máquinas para su evaluación con Azure Migrate | Microsoft Docs
description: En este artículo se describe cómo agrupar máquinas antes de ejecutar una evaluación con el servicio Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: ccab88c0195a7ca459c8579b7870d121dfd0fe1d
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231413"
---
# <a name="group-machines-for-assessment"></a>Agrupación de máquinas para su evaluación

En este artículo se describe cómo crear un grupo de máquinas para su evaluación mediante [Azure Migrate](migrate-overview.md). Azure Migrate evalúa máquinas del grupo para comprobar si son adecuadas para su migración a Azure, y proporciona estimaciones de ajuste de tamaño y costo con relación a la ejecución de la máquina en Azure.


## <a name="create-a-group"></a>Creación de un grupo

1. En **Información general** del proyecto de Azure Migrate, en Administrar, haga clic en  **Grupos** > **+Grupo** un grupo.
2. Agregue una o variad máquinas al grupo y haga clic en  **Crear**. 
3. También puede elegir ejecutar una nueva evaluación para el grupo. 

    ![Creación de un grupo](./media/how-to-create-a-group/create-group.png)

Una vez creado el grupo, puede modificarlo seleccionándolo en la página **Grupos**, y agregando o quitando máquinas.

## <a name="next-steps"></a>Pasos siguientes

- Aprenda a usar la [asignación de dependencias de máquina](how-to-create-group-machine-dependencies.md) para crear grupos de confianza alta.
- [Obtenga más información](concepts-assessment-calculation.md) sobre cómo se calculan las evaluaciones.
