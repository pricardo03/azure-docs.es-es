---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: sdgilley
ms.service: machine-learning
ms.author: sgilley
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 05/06/2019
ms.openlocfilehash: cf35651f7dd839e8792029851b9bfe278036624c
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028521"
---
Un experimento se ejecuta en un destino de proceso, un recurso de proceso que está conectado al área de trabajo.  Una vez que cree un destino de proceso, puede volver a usar para futuras ejecuciones.

1. Seleccione **ejecutar** en la parte inferior para ejecutar el experimento.

     ![Ejecutar experimento](./media/aml-ui-create-training-compute/run-experiment.png)

1. Cuando el **destinos de proceso de instalación** aparece el cuadro de diálogo, si el área de trabajo ya tiene un recurso de proceso, puede seleccionarlo ahora.  En caso contrario, seleccione **crear nuevo**.

    > [!NOTE]
    > La interfaz visual solo puede ejecutar experimentos en Machine Learning Compute destinos. No se mostrarán otros destinos de proceso.

1. Proporcione un nombre para el recurso de proceso.

1. Seleccione **Run** (Ejecutar).

    ![Destino de proceso del programa de instalación](./media/aml-ui-create-training-compute/set-compute.png)

    Ahora se creará el recurso de proceso. Ver el estado en la esquina superior derecha del experimento. 

    > [!NOTE]
    > Tarda aproximadamente 5 minutos para crear un recurso de proceso. Una vez creado el recurso, puede volver a usarla y omitir este tiempo de espera para futuras ejecuciones.
    >
    > El recurso de proceso realizará el escalado automático a 0 nodos cuando esté inactivo para ahorrar costos.  Cuando use nuevo tras un retraso, podría experimentar de nuevo aproximadamente 5 minutos de tiempo de espera mientras se escala hacia atrás.
