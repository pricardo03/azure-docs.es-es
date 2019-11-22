---
title: archivo de inclusión
description: archivo de inclusión
services: machine-learning
author: peterclu
ms.service: machine-learning
ms.author: peterlu
manager: cgronlund
ms.custom: include file
ms.topic: include
ms.date: 10/09/2019
ms.openlocfilehash: 1ee90e0c99234497b072bbee0b92d76129baea48
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73929649"
---
Una canalización se ejecuta en un destino de proceso que es un recurso de proceso asociado al área de trabajo. Después de crear un destino de proceso, puede volver a usarlo para futuras ejecuciones.

1. Seleccione **Ejecutar** en la parte superior del lienzo para ejecutar la canalización.

1. Cuando aparezca el panel **Configuración**, seleccione **Seleccionar destino de proceso**.

    Si ya tiene un destino de proceso disponible, puede seleccionarlo para ejecutar esta canalización.

    > [!NOTE]
    > El diseñador solo puede ejecutar experimentos en destino de proceso de Azure Machine Learning. Otros destinos de proceso no aparecerán.

1. Escriba un nombre para el recurso de proceso.

1. Seleccione **Guardar**.

    ![Configuración del destino de proceso](./media/aml-ui-create-training-compute/set-compute.png)

1. Seleccione **Run** (Ejecutar).

1. En el cuadro de diálogo **Configurar ejecución de canalización**, seleccione **+ Nuevo experimento** para **Experimento**.

    > [!NOTE]
    > Los experimentos agrupan ejecuciones de canalización. Si ejecuta una canalización varias veces, puede seleccionar el mismo experimento para ejecuciones sucesivas.

    1. Escriba un nombre descriptivo para **Nombre del experimento**.

    1. Seleccione **Run** (Ejecutar).
    
    Puede ver el estado y los detalles de la ejecución en la parte superior derecha del lienzo.

    > [!NOTE]
    > La creación de un recurso de proceso tarda aproximadamente cinco minutos. Una vez creado el recurso, puede volver a usarlo y eliminar este tiempo de espera en futuras ejecuciones.
    >
    > El recurso de proceso se escala automáticamente a cero nodos cuando está inactivo para ahorrar costes. Cuando se vuelve a usar después de un retraso, es posible que experimente aproximadamente cinco minutos de tiempo de espera mientras se escala la copia de seguridad.
