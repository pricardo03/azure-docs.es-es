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
ms.openlocfilehash: eb84dc1b5bf3f756e484ef27aaa998ab6b94cc51
ms.sourcegitcommit: a3a40ad60b8ecd8dbaf7f756091a419b1fe3208e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69891699"
---
Un experimento se ejecuta en un destino de proceso que es un recurso de proceso que está asociado al área de trabajo.  Una vez creado un destino de proceso, puede volver a utilizarlo para futuras ejecuciones.

1. Seleccione **Ejecutar** en la parte inferior para ejecutar el experimento.

1. Cuando aparece el cuadro de diálogo **Setup Compute Targets** (Configurar destinos de proceso), si el área de trabajo ya tiene un recurso de proceso, ahora puede seleccionarlo.  En caso contrario, seleccione **Create new** (Crear nuevo).

    > [!NOTE]
    > La interfaz visual solo puede ejecutar experimentos en destinos de proceso de Machine Learning. Otros destinos de proceso no aparecerán.

1. Proporcione un nombre para el recurso de proceso.

1. Seleccione **Run** (Ejecutar).

    ![Configuración del destino de proceso](./media/aml-ui-create-training-compute/set-compute.png)

    Ahora se creará el recurso de proceso. Vea el estado en la esquina superior derecha del experimento. 

    > [!NOTE]
    > Se tardan 5 minutos aproximadamente en crear un recurso de proceso. Una vez creado el recurso, puede volver a usarlo y eliminar este tiempo de espera en futuras ejecuciones.
    >
    > El recurso de proceso realizará el escalado automático a 0 nodos cuando esté inactivo para ahorrar costos.  Cuando lo vuelva a usar después de un retraso, puede que experimente de nuevo unos 5 minutos de tiempo de espera mientras se realiza el escalado vertical nuevamente.
