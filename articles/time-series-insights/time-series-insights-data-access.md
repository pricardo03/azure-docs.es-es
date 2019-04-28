---
title: Configuración de la seguridad para el acceso y la administración de Azure Time Series Insights | Microsoft Docs
description: En este artículo se describe cómo configurar la seguridad y los permisos como directivas de acceso de administración y directivas de acceso a datos para proteger la versión preliminar de Azure Time Series Insights.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: anshan
manager: cshankar
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.workload: big-data
ms.topic: conceptual
ms.date: 11/26/2018
ms.custom: seodec18
ms.openlocfilehash: dd4c5e1652eb4dbff66591aa4bbe74e51be3e6c0
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63759552"
---
# <a name="grant-data-access-to-an-environment"></a>Concesión de acceso a datos en un entorno

En este artículo se tratan los dos tipos de directivas de acceso de la versión preliminar de Azure Time Series Insights.

## <a name="sign-in-to-tsi"></a>Inicie sesión en TSI

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Busque el entorno de Time Series Insights. Especifique `Time Series` en el cuadro **Búsqueda**. Seleccione **Entorno de Time Series** en los resultados de búsqueda.
1. Seleccione el entorno de Time Series Insights de la lista.

## <a name="grant-data-access"></a>Concesión de acceso a datos

Siga estos pasos para conceder acceso a datos a una entidad de seguridad de usuario.

1. Seleccione **Directivas de acceso a datos** y, luego, **+ Agregar**.

    ![Acceso a datos: uno][1]

1. Elija **Seleccionar usuario**. Busque el nombre de usuario o la dirección de correo para encontrar al usuario que quiere agregar. Haga clic en **Seleccionar** para confirmar la selección.

    ![Acceso a datos: dos][2]

1. Elija **Seleccionar rol**. Elija el rol de acceso adecuado para el usuario:

    * Seleccione **Colaborador** si quiere permitir al usuario cambiar los datos de referencia y compartir las consultas y las perspectivas guardadas con otros usuarios del entorno.

    * En caso contrario, seleccione **Lector** para permitir al usuario consultar los datos del entorno y guardar consultas personales (no compartidas) en el entorno.

   Seleccione **Aceptar** para confirmar la elección del rol.

    ![Acceso a datos: tres][3]

1. Seleccione **Aceptar** en la página **Seleccionar rol de usuario**.

    ![Acceso a datos: cuatro][4]

1. Confirme que la página **Directivas de acceso a datos** enumera los usuarios y los roles de cada uno.

    ![Acceso a datos: cinco][5]

## <a name="provide-guest-access-from-another-aad-tenant"></a>Proporcionar acceso de invitados de otro inquilino AAD

`Guest` no es un rol de administración. Se trata de un término que se usa para una cuenta a la que se ha invitado desde un inquilino a otro. Después de que la cuenta de invitado se haya invitado al directorio del inquilino, se le puede aplicar el mismo control de acceso que a cualquier otra cuenta. Puede conceder acceso de administración a un entorno de Time Series Insights mediante la hoja de Control de acceso (IAM). O puede conceder acceso a los datos del entorno utilizando la hoja Directivas de acceso a datos. Para más información sobre el acceso de invitado de inquilino de Azure Active Directory (Azure AD), consulte [Incorporación de usuarios de colaboración B2B de Azure Active Directory en Azure Portal](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator).

Siga estos pasos para conceder acceso de invitado a un entorno de Time Series Insights a un usuario de Azure AD desde otro inquilino.

1. Seleccione **Directivas de acceso a datos** y, luego, **+ Invitar**.

    ![Acceso a datos: seis][6]

1. Escriba la dirección de correo electrónico del usuario que desea invitar. Esta dirección de correo electrónico tiene que estar asociada con Azure AD. Opcionalmente puede incluir un mensaje personal con la invitación.

    ![Acceso a datos: siete][7]

1. Busque la burbuja de confirmación que aparece en la pantalla.

    ![Acceso a datos: ocho][8]

1. Elija **Seleccionar usuario**. Busque la dirección de correo electrónico del usuario que ha invitado para ubicar al usuario que quiere agregar. Haga clic en **Seleccionar** para confirmar la selección.

    ![Acceso a datos: nueve][9]

1. Elija **Seleccionar rol**. Elija el rol de acceso adecuado para el usuario invitado:

    * Seleccione **Colaborador** si quiere permitir al usuario cambiar los datos de referencia y compartir las consultas y las perspectivas guardadas con otros usuarios del entorno.

    * En caso contrario, seleccione **Lector** para permitir al usuario consultar los datos del entorno y guardar consultas personales (no compartidas) en el entorno.

   Seleccione **Aceptar** para confirmar la elección del rol.

    ![Acceso a datos: diez][10]

1. Seleccione **Aceptar** en la página **Seleccionar rol de usuario**.

1. Confirme que la página **Directivas de acceso a datos** aparece el usuario invitado y los roles de cada usuario invitado.

    ![Acceso a datos: once][11]

1. Ahora el usuario invitado tiene que realizar ciertos pasos para acceder al entorno que se encuentra en el inquilino de Azure al que se le ha invitado. En primer lugar, el usuario tiene que aceptar la invitación que le ha enviado. Esta invitación se envía por correo electrónico a la dirección que utilizó en el paso 5. Para aceptar el usuario invitado selecciona **Get Started** (Inicio).

    ![Acceso a datos: doce][12]

1. A continuación el usuario invitado debe aceptar los permisos asociados a la organización del administrador.

    ![Acceso a datos: trece][13]

1. Cuando el usuario invitado inicia sesión en la dirección de correo electrónico que utilizó para invitarle y acepta la invitación, tiene que ir a insights.azure.com. Una vez allí, el usuario selecciona el avatar situado junto a su dirección de correo electrónico en la esquina superior derecha de la pantalla.

    ![Acceso a datos: catorce][14]

1. Luego, el usuario invitado selecciona su inquilino de Azure en el menú desplegable del directorio. Este inquilino es aquel al que le ha invitado.

    ![Acceso a datos: quince][15]

Una vez que el usuario invitado selecciona su inquilino, verá el entorno de Time Series Insights al que le ha proporcionado acceso. Ahora tienen todas las funcionalidades asociadas con el rol que se les proporcionó con en **paso 5**.

## <a name="next-steps"></a>Pasos siguientes

* Aprenda [cómo agregar un origen del evento de Azure Event Hubs](./time-series-insights-how-to-add-an-event-source-eventhub.md) al entorno de Time Series Insights.

* Envíe [eventos al origen del evento](./time-series-insights-send-events.md).

* Visualice [el entorno en el explorador de la versión preliminar de Time Series Insights](./time-series-insights-update-explorer.md).

<!-- Images -->
[1]: media/data-access/data-access-one.png
[2]: media/data-access/data-access-two.png
[3]: media/data-access/data-access-three.png
[4]: media/data-access/data-access-four.png
[5]: media/data-access/data-access-five.png
[6]: media/data-access/data-access-six.png
[7]: media/data-access/data-access-seven.png
[8]: media/data-access/data-access-eight.png
[9]: media/data-access/data-access-nine.png
[10]: media/data-access/data-access-ten.png
[11]: media/data-access/data-access-eleven.png
[12]: media/data-access/data-access-twelve.png
[13]: media/data-access/data-access-thirteen.png
[14]: media/data-access/data-access-fourteen.png
[15]: media/data-access/data-access-fifteen.png
