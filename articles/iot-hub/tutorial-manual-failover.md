---
title: Conmutación por error manual de un centro de IoT de Azure | Microsoft Docs
description: Muestra cómo realizar una conmutación por error manual de un centro de IoT de Azure
author: robinsh
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 07/11/2018
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: f0e8bf922f142b795dd1a2ded4b3ec265c43481a
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39250060"
---
# <a name="tutorial-perform-manual-failover-for-an-iot-hub-public-preview"></a>Tutorial: Conmutación por error manual de un centro de IoT (versión preliminar pública)

La conmutación por error manual es una característica del servicio IoT Hub que permite a los clientes [realizar la conmutación por error](https://en.wikipedia.org/wiki/Failover) de las operaciones de su centro desde una región primaria a la región de Azure emparejada geográficamente correspondiente. La conmutación por error manual se puede realizar si se produce un desastre regional o una interrupción prolongada del servicio. También se puede realizar una conmutación por error planeada para probar las funcionalidades de recuperación ante desastres, aunque se recomienda usar un centro de IoT de prueba, en lugar del centro que se ejecuta en producción. La característica de conmutación por error manual se ofrece a los clientes sin costo adicional.

En este tutorial se realizan las siguientes tareas:

> [!div class="checklist"]
> * Crear un centro de IoT mediante Azure Portal. 
> * Realizar una conmutación por error. 
> * Ver el centro que se ejecutan en la ubicación secundaria.
> * Realizar una conmutación por recuperación para devolver las operaciones del centro de IoT a la ubicación principal. 
> * Confirmar que el concentrador se ejecuta adecuadamente en la ubicación correcta.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="create-an-iot-hub"></a>Crear un centro de IoT

1. Inicie sesión en [Azure Portal](https://portal.azure.com). 

2. Haga clic en **+ Crear un recurso**, seleccione **Internet de las cosas** y, después, **Centro de IoT**.

   ![Captura de pantalla que muestra cómo se crea un centro de IoT](./media/tutorial-manual-failover/create-hub-01.png)

3. Seleccione la pestaña **Datos básicos**. Rellene los siguientes campos.

    **Suscripción**: seleccione la suscripción de Azure que desea usar.

    **Grupo de recursos**: haga clic en **Crear nuevo** y especifique **ManlFailRG** como nombre del grupo de recursos.

    **Región**: seleccione una región cercana que forme parte de la versión preliminar. En este tutorial se usa `westus2`. Las conmutaciones por error solo se pueden realizar entre regiones de Azure emparejadas geográficamente. La región emparejada geográficamente con westus2 es WestCentralUS.
    
   > [!NOTE]
   > La conmutación por error manual se encuentra actualmente en versión preliminar pública y *no* está disponible en las siguientes regiones de Azure: Este de EE. UU., Oeste de EE. UU, Europa del Norte, Europa Occidental, Sur de Brasil y Centro-sur de EE. UU.

   **IoT Hub Name** (Nombre del centro de IoT): especifique el nombre del centro de IoT. Dicho nombre debe ser único globalmente. 

   ![Captura de pantalla que muestra el panel Basics (Datos básicos) para crear un centro de IoT](./media/tutorial-manual-failover/create-hub-02-basics.png)

   Haga clic en **Revisar + crear** (usa los valores predeterminados para el tamaño y la escala). 

4. Revise la información y haga clic en **Create** (Crear) para crear el centro de IoT. 

   ![Captura de pantalla que muestra el paso final para crear un centro de IoT](./media/tutorial-manual-failover/create-hub-03-create.png)

## <a name="perform-a-manual-failover"></a>Realización de una conmutación por error manual

Tenga en cuenta que hay un límite de dos conmutaciones por error y dos conmutaciones por recuperación al día en un centro de IoT.

1. Haga clic en **Grupos de recursos** y seleccione el grupo **ManlFailRG**. En la lista de recursos, haga clic en su centro. 

2. En **Resiliency** (Resistencia), en el panel IoT Hub, haga clic en **Manual failover (preview)** (Conmutación por error manual [versión preliminar]). Tenga en cuenta que si el centro no se ha configurado en una región válida, la opción de conmutación por error manual estará deshabilitada.

   ![Captura de pantalla que muestra el panel de propiedades del centro de IoT](./media/tutorial-manual-failover/trigger-failover-01.png)

3. En el panel Manual failover (Conmutación por error manual), se ven las opciones **IoT Hub Primary Location** (Ubicación principal del centro de IoT) y **IoT Hub Secondary Location** (Ubicación secundaria del centro de IoT). En principio, la ubicación principal que aparece seleccionada es la que especificó al crear el centro de IoT y siempre indica la ubicación en la que el centro está activo actualmente. La ubicación secundaria es la [región emparejada geográficamente de Azure](../best-practices-availability-paired-regions.md) estándar que está emparejada con la ubicación primaria. Los valores de las ubicaciones no se pueden cambiar. En este tutorial, la ubicación principal es `westus2`, mientras que la ubicación secundaria es `WestCentralUS`.

   ![Captura de pantalla que muestra el panel Conmutación por error manual](./media/tutorial-manual-failover/trigger-failover-02.png)

3. En la parte superior del panel Conmutación por error manual, haga clic en  **Iniciar conmutación por error**. Verá el panel de **confirmación de la conmutación por error manual**. Escriba el nombre del centro de IoT para confirmar que es el que desea usar para la conmutación por error. A continuación, para iniciar la conmutación por error, haga clic en **Aceptar**.

   La cantidad de tiempo que tarda en realizarse la conmutación por error manual es proporcional al número de dispositivos registrados en el centro. Por ejemplo, si tiene 100 000, podría tardar 15 minutos, pero si tiene 5 000 000, podría tardar una hora, o incluso más.

4. En el panel **Confirm manual failover** (Confirmar la conmutación por error manual), escriba el nombre del centro de IoT para confirmar que es el que desea usar para la conmutación por error. Luego, para iniciar la conmutación por error, haga clic en Aceptar. 

   ![Captura de pantalla que muestra el panel Conmutación por error manual](./media/tutorial-manual-failover/trigger-failover-03-confirm.png)

   Mientras se ejecuta el proceso de conmutación por error manual, hay un banner en el panel Conmutación por error manual que indica que hay una conmutación por error manual en curso. 

   ![Captura de pantalla que muestra una conmutación por error manual en curso](./media/tutorial-manual-failover/trigger-failover-04-in-progress.png)

   Si cierra el panel de IoT Hub y lo abre de nuevo haciendo clic en él en el panel Resource Group (Grupo de recursos), verá un banner que le indica que el centro no está activo. 

   ![Captura de pantalla que muestra el centro de IoT inactivo](./media/tutorial-manual-failover/trigger-failover-05-hub-inactive.png)

   Una vez finalizada, se invierten las regiones principal y secundaria en la página Conmutación por error manual y el centro vuelve a estar activo. En este tutorial, la ubicación principal es ahora `WestCentralUS`, mientras que la ubicación secundaria es `westus2`. 

   ![Captura de pantalla que muestra la conmutación por error finalizada](./media/tutorial-manual-failover/trigger-failover-06-finished.png)

## <a name="perform-a-failback"></a>Realización de una conmutación por recuperación 

Después de haber realizado una conmutación por error manual, puede devolver las operaciones del centro a la región primaria original (lo que se conoce como conmutación por recuperación). Si acaba de realizar una conmutación por error, tendrá que esperar aproximadamente una hora para poder solicitar una conmutación por recuperación. Si intenta realizar la conmutación por recuperación en un período más corto, aparecerá un mensaje de error.

Las conmutaciones por recuperación se realizan de la misma forma que las conmutación por error manuales. Estos son los pasos necesarios: 

1. Para realizar una conmutación por recuperación, vuelva al panel IoT Hub de su centro de IoT.

2. En **Resiliency** (Resistencia), en el panel IoT Hub, haga clic en **Manual failover (preview)** (Conmutación por error manual [versión preliminar]). 

3. En la parte superior del panel Conmutación por error manual, haga clic en  **Iniciar conmutación por error**. Verá el panel de **confirmación de la conmutación por error manual**. 

4. En el panel **Confirm manual failover** (Confirmar la conmutación por error manual), escriba el nombre del centro de IoT para confirmar que es el que desea usar para la conmutación por recuperación. Para iniciar la conmutación por recuperación, haga clic en Aceptar. 

   ![Captura de pantalla de solicitud de conmutación por recuperación manual](./media/tutorial-manual-failover/trigger-failback-01-regions.png)

   Los banners se muestran como se explica en la sección acerca de la [realización de una conmutación por error](#perform-a-failover). Cuando la conmutación por recuperación termina, vuelve a mostrar `westus2` como ubicación principal y `WestCentralUS` como ubicación secundaria, como se estableció originalmente.

## <a name="clean-up-resources"></a>Limpieza de recursos 

Para quitar los recursos que ha creado para este tutorial, elimine el grupo de recursos. Esta acción elimina también todos los recursos del grupo. En este caso, quita el centro de IoT y el propio grupo de recursos. 

1. Haga clic en **Grupos de recursos**. 

2. Busque y seleccione el grupo de recursos **ManlFailRG**. Haga clic en ella para abrirla. 

3. Haga clic en **Eliminar grupo de recursos**. Cuando se le pida, escriba el nombre del grupo de recursos y haga clic en **Eliminar** para confirmarlo. 

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido a configurar y realizar una conmutación por error manual y a solicitar una conmutación por recuperación mediante la realización de las siguientes tareas:

> [!div class="checklist"]
> * Crear un centro de IoT mediante Azure Portal. 
> * Realizar una conmutación por error. 
> * Ver el centro que se ejecutan en la ubicación secundaria.
> * Realizar una conmutación por recuperación para devolver las operaciones del centro de IoT a la ubicación principal. 
> * Confirmar que el concentrador se ejecuta adecuadamente en la ubicación correcta.

En el siguiente tutorial aprender a administrar el estado de un dispositivo IoT. 

> [!div class="nextstepaction"]
[Administración del estado de un dispositivo IoT](tutorial-device-twins.md)