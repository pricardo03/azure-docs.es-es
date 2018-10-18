---
title: Configuración de la seguridad para acceder a Time Series Insights y administrarlo | Microsoft Docs
description: En este artículo se describe cómo configurar la seguridad y los permisos como directivas de acceso de administración y directivas de acceso a datos para proteger Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/15/2017
ms.openlocfilehash: c9bddf8e20524433b31793e277efd954a5d1320e
ms.sourcegitcommit: 42405ab963df3101ee2a9b26e54240ffa689f140
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47423381"
---
# <a name="grant-data-access-to-a-time-series-insights-environment-using-azure-portal"></a>Concesión de acceso a los datos de un entorno de Time Series Insights mediante Azure Portal

En este artículo se tratan los dos tipos de directivas de acceso de Time Series Insights.

## <a name="video"></a>Vídeo: 

### <a name="in-this-video-we-cover-creating-and-managing-access-policies-within-time-series-insights-br"></a>En este vídeo se trata la creación y administración de directivas de acceso en Time Series Insights. </br>

> [!VIDEO https://www.youtube.com/embed/5zTIdyHMJW8]

Los entornos de Time Series Insights tienen dos tipos independientes de directivas de acceso:

* Directivas de acceso de administración
* Directivas de acceso a datos

Ambas directivas conceden a las entidades de Azure Active Directory (usuarios y aplicaciones) distintos permisos en un entorno concreto. Las entidades de seguridad (usuarios y aplicaciones) deben pertenecer al directorio activo (conocido como inquilino de Azure) asociado a la suscripción que contiene el entorno.

Las directivas de acceso de administración conceden permisos relacionados con la configuración del entorno, tales como:
*   La creación y eliminación del entorno, orígenes de eventos, conjuntos de datos de referencia.
*   Administración de las directivas de acceso a datos.

Las directivas de acceso a datos conceden permisos para emitir consultas de datos, manipular datos de referencia en el entorno y compartir consultas guardadas y perspectivas asociadas con el entorno.

Los dos tipos de directivas permiten una separación clara entre el acceso a la administración del entorno y el acceso a los datos dentro del entorno. Por ejemplo, es posible configurar un entorno de modo que su propietario o creador no disponga de acceso a los datos. Además, los usuarios y servicios que tienen permiso para leer los datos del entorno, podrían no tener acceso a la configuración del entorno.

## <a name="grant-data-access"></a>Concesión de acceso a datos
Siga estos pasos para conceder acceso a datos a una entidad de seguridad de usuario:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque el entorno de Time Series Insights. Escriba **Time Series** en el cuadro de **búsqueda**. Seleccione **Entorno de Time Series** en los resultados de búsqueda. 

3. Seleccione el entorno de Time Series Insights de la lista.
   
4. Seleccione **Data Access Policies** (Directivas de acceso a datos) y, luego, **+ Agregar**.
  ![Administración del entorno del origen de Time Series Insights: entorno](media/data-access/getstarted-grant-data-access1.png)

5. Seleccione **Seleccionar usuario**.  Busque el nombre de usuario o la dirección de correo para encontrar al usuario que quiere agregar. Haga clic en **Seleccionar** para confirmar la selección. 

   ![Administración del origen de Time Series Insights: agregar](media/data-access/getstarted-grant-data-access2.png)

6. Seleccione **Seleccionar rol**. Elija el rol de acceso adecuado para el usuario:
   - Seleccione **Colaborador** si desea permitir al usuario cambiar los datos de referencia y compartir las consultas almacenadas y las perspectivas con otros usuarios del entorno. 
   - En caso contrario, seleccione **Lector** para permitir al usuario consultar los datos del entorno y guardar consultas personales (no compartidas) en el entorno.

   Seleccione **Aceptar** para confirmar la elección del rol.

   ![Administración del origen de Time Series Insights: seleccionar usuario](media/data-access/getstarted-grant-data-access3.png)

8. Seleccione **Aceptar** en la página **Select User Role** (Seleccionar rol de usuario).

   ![Administración del origen de Time Series Insights: seleccionar rol](media/data-access/getstarted-grant-data-access4.png)

9. En la página **Data Access Policies** (Directivas de acceso a datos) se muestran los usuarios y los roles de cada uno.

   ![Administración del origen de Time Series Insights: resultados](media/data-access/getstarted-grant-data-access5.png)

## <a name="provide-guest-access-to-a-user-from-another-aad-tenant"></a>Proporcionar acceso de invitado a un usuario desde otro inquilino de AAD

"Invitado" no es un rol de administración; es un término que se usa para una cuenta a la que se ha invitado desde un inquilino a otro. Una vez que la cuenta de invitado ha sido invitada al directorio del inquilino, se le puede aplicar el mismo control de acceso que a cualquier otra cuenta, ya sea concederle acceso de administración a un entorno de TSI mediante la hoja Control de acceso (IAM), o concederle acceso a los datos del entorno a través de la hoja Directivas de acceso a datos. Para obtener más información sobre el acceso de invitado de inquilino de AAD, vea este [documento](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Siga estos pasos para conceder acceso de invitado a un entorno de Time Series Insights a un usuario de AAD desde otro inquilino:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Busque el entorno de Time Series Insights. Escriba **Time Series** en el cuadro de **búsqueda**. Seleccione **Entorno de Time Series** en los resultados de búsqueda.

3. Seleccione el entorno de Time Series Insights de la lista.

4. Seleccione **Directivas de acceso a datos** y luego + **Invitar**.

    ![Administración del origen de Time Series Insights: invitar usuario](media/data-access/getstarted-grant-data-access6.png)

5. Proporcione el correo electrónico del usuario al que quiere invitar. Tenga en cuenta que debe tratarse de un correo electrónico asociado a AAD. Opcionalmente puede incluir un mensaje personal con la invitación.

    ![Administración del origen de Time Series Insights: seleccionar usuario](media/data-access/getstarted-grant-data-access7.png)

6. Debería ver una burbuja de confirmación en la pantalla.

    ![Administración del origen de Time Series Insights: confirmar usuario](media/data-access/getstarted-grant-data-access8.png)

7. Seleccione **Seleccionar usuario**. Busque la dirección de correo electrónico del usuario invitado que acaba de invitar para encontrar al usuario que quiere agregar. Haga clic en **Seleccionar** para confirmar la selección.
  
    ![Administración del origen de Time Series Insights: confirmar usuario](media/data-access/getstarted-grant-data-access9.png)

8. Seleccione **Seleccionar rol**. Elija el rol de acceso adecuado para el usuario invitado:

    * Seleccione **Colaborador** si quiere permitir al usuario cambiar los datos de referencia y compartir las consultas y las perspectivas guardadas con otros usuarios del entorno.

    * En caso contrario, seleccione **Lector** para permitir al usuario consultar los datos del entorno y guardar consultas personales (no compartidas) en el entorno.

    Seleccione **Aceptar** para confirmar la elección del rol.

    ![Administración del origen de Time Series Insights: seleccionar rol](media/data-access/getstarted-grant-data-access10.png)

9. Seleccione **Aceptar** en la página **Select User Role** (Seleccionar rol de usuario).

10. En la página **Directivas de acceso a datos** ahora aparecen el usuario invitado y los roles de cada usuario invitado.

    ![Administración del origen de Time Series Insights: confirmar rol](media/data-access/getstarted-grant-data-access11.png)

11. Ahora el usuario invitado debe realizar ciertos pasos para acceder al entorno que se encuentra en el inquilino de Azure al que se le acaba de invitar. En primer lugar, tiene que aceptar la invitación que acaba de enviarle. Esta invitación se envía por correo electrónico a la dirección de correo electrónico invitada en el paso 5. Debe hacer clic en "Comenzar" para aceptar.

    ![Administración del origen de Time Series Insights: invitar usuario](media/data-access/getstarted-grant-data-access12.png)

12. Luego, el usuario invitado debe aceptar los permisos asociados a la organización del administrador.

    ![Administración del origen de Time Series Insights: aceptar permisos](media/data-access/getstarted-grant-data-access13.png)

13. Cuando el usuario invitado inicia sesión en la dirección de correo electrónico invitada y acepta la invitación, va a insights.azure.com. Una vez allí, debe hacer clic en el avatar situado junto a su correo electrónico en la esquina superior derecha de la pantalla. 

    ![Administración del origen de Time Series Insights: aceptar permisos](media/data-access/getstarted-grant-data-access14.png)

14. Luego, el usuario invitado selecciona el inquilino de Azure en el menú desplegable del directorio. Se trata del inquilino al que se le ha invitado. 

    ![Administración del origen de Time Series Insights: aceptar permisos](media/data-access/getstarted-grant-data-access15.png)

15. Por último, cuando el usuario invitado selecciona el inquilino, ve el entorno de Time Series Insights al que le acaba de proporcionar acceso. Ahora debe tener todas las funcionalidades asociadas al rol que le ha proporcionado en el paso 8.

## <a name="next-steps"></a>Pasos siguientes
* Aprenda a [agregar un origen del evento del centro de eventos al entorno de Azure Time Series Insights](time-series-insights-how-to-add-an-event-source-eventhub.md).
* Realice el [envío de eventos](time-series-insights-send-events.md) al origen del evento.
* Vea el entorno en el [explorador de Time Series Insights](https://insights.timeseries.azure.com).
