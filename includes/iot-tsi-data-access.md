---
title: archivo de inclusión
description: archivo de inclusión
services: time-series-insights
author: ashannon7
ms.service: time-series-insights
ms.topic: include
ms.date: 08/20/2018
ms.author: anshan
ms.custom: include file
ms.openlocfilehash: c9daa86bf36b260001d9969385b9e8a98a8ac0cf
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61443249"
---
## <a name="grant-data-access"></a>Concesión de acceso a datos

Siga estos pasos para conceder acceso a datos a una entidad de seguridad de usuario:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque el entorno de Time Series Insights. Escriba **Time Series** en el cuadro de **búsqueda**. Seleccione **Entorno de Time Series** en los resultados de búsqueda. 

3. Seleccione el entorno de Time Series Insights de la lista.

4. Seleccione **Data Access Policies** (Directivas de acceso a datos) y, luego, **+ Agregar**.
    ![Administración del entorno del origen de Time Series Insights: entorno](media/iot-tsi-data-access/getstarted-grant-data-access1.png)

5. Seleccione **Seleccionar usuario**.  Busque el nombre de usuario o la dirección de correo para encontrar al usuario que quiere agregar. Haga clic en **Seleccionar** para confirmar la selección. 

    ![Administración del origen de Time Series Insights: agregar](media/iot-tsi-data-access/getstarted-grant-data-access2.png)

6. Seleccione **Seleccionar rol**. Elija el rol de acceso adecuado para el usuario:
   - Seleccione **Colaborador** si desea permitir al usuario cambiar los datos de referencia y compartir las consultas almacenadas y las perspectivas con otros usuarios del entorno. 
   - En caso contrario, seleccione **Lector** para permitir al usuario consultar los datos del entorno y guardar consultas personales (no compartidas) en el entorno.

     Seleccione **Aceptar** para confirmar la elección del rol.

     ![Administración del origen de Time Series Insights: seleccionar usuario](media/iot-tsi-data-access/getstarted-grant-data-access3.png)

7. Seleccione **Aceptar** en la página **Select User Role** (Seleccionar rol de usuario).

    ![Administración del origen de Time Series Insights: seleccionar rol](media/iot-tsi-data-access/getstarted-grant-data-access4.png)

8. En la página **Data Access Policies** (Directivas de acceso a datos) se muestran los usuarios y los roles de cada uno.

    ![Administración del origen de Time Series Insights: resultados](media/iot-tsi-data-access/getstarted-grant-data-access5.png)