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
ms.openlocfilehash: 3f969fb346ce341e694458dcc38bf193f012226b
ms.sourcegitcommit: b050c7e5133badd131e46cab144dd5860ae8a98e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72792656"
---
Una canalización se ejecuta en un destino de proceso que es un recurso de proceso asociado al área de trabajo. Una vez creado un destino de proceso, puede volver a utilizarlo para futuras ejecuciones.

1. Seleccione **Ejecutar** en la parte superior del lienzo para ejecutar la canalización.

1. Cuando aparezca el panel **Configuración**, seleccione **Seleccionar destino de proceso**.

    Si ya tiene un destino de proceso disponible, puede seleccionarlo para ejecutar esta canalización.

    > [!NOTE]
    > La interfaz visual solo puede ejecutar experimentos en destinos de proceso de Machine Learning. Otros destinos de proceso no aparecerán.

1. Proporcione un nombre para el recurso de proceso.

1. Seleccione **Guardar**.

    ![Configuración del destino de proceso](./media/aml-ui-create-training-compute/set-compute.png)

1. Seleccione **Run** (Ejecutar).

1. En el cuadro de diálogo **Set up pipeline run** (Configurar ejecución de canalización), seleccione **+ Nuevo experimento** para el **Experimento**.

    > [!NOTE]
    > Los experimentos agrupan ejecuciones de canalización. Si ejecuta una canalización varias veces, puede seleccionar el mismo experimento para ejecuciones sucesivas.

    * Escriba un **Nombre de experimento descriptivo**

    * Seleccione **Ejecutar**.
    
    Puede ver el estado y los detalles de la ejecución en la parte superior derecha del lienzo.

    > [!NOTE]
    > Se tardan 5 minutos aproximadamente en crear un recurso de proceso. Una vez creado el recurso, puede volver a usarlo y eliminar este tiempo de espera en futuras ejecuciones.
    >
    > El recurso de proceso realizará el escalado automático a 0 nodos cuando esté inactivo para ahorrar costos.  Cuando lo vuelva a usar después de un retraso, puede que experimente de nuevo unos 5 minutos de tiempo de espera mientras se realiza el escalado vertical nuevamente.
