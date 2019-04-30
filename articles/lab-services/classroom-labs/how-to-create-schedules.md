---
title: Creación de programaciones para laboratorios educativos en Azure Lab Services | Microsoft Docs
description: Aprenda a crear programaciones para laboratorios educativos en Azure Lab Services de forma que las máquinas virtuales de los laboratorios se inicien y apaguen a una hora determinada.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2019
ms.author: spelluru
ms.openlocfilehash: 34bc8263053cd4a701c16ee1832cf1b27340a345
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60696041"
---
# <a name="create-and-manage-schedules-for-classroom-labs-in-azure-lab-services"></a>Creación y administración de programaciones para laboratorios educativos en Azure Lab Services 
Las programaciones le permiten configurar un laboratorio educativo de forma que las máquinas virtuales que contiene se inicien y apaguen automáticamente a una hora determinada. Puede definir una programación puntual o periódica. Los procedimientos siguientes le proporcionan los pasos para crear y administrar programaciones para un laboratorio educativo: 

> [!IMPORTANT]
> La hora de ejecución programada de las máquinas virtuales no cuentan en la [cuota asignada a un usuario](how-to-configure-student-usage.md#set-quotas-per-user). La cuota es el tiempo fuera del horario programado que un alumno pasa en las máquinas virtuales. 

## <a name="add-a-schedule-once"></a>Agregar una programación (una vez)

1. Cambie a la página **Schedules** (Programaciones) y seleccione **Add schedule** (Agregar programación) en la barra de herramientas. 

    ![Botón Add schedule (Agregar programación) en la página Schedules (Programaciones)](../media/how-to-create-schedules/add-schedule-button.png)
2. En la página **Add schedule** (Agregar programación), confirme que está seleccionada la opción **Once** (Una vez) que se encuentra en la parte superior. Si no es así, selecciónela. 
3. En **Schedule date (required)** (Fecha de programación [obligatorio]), escriba la fecha o seleccione el icono de calendario para especificarla. 
4. En **Start time** (Hora de inicio), seleccione la hora a la que quiere que se inicie la máquina virtual. La hora de inicio es obligatoria si no se establece la hora de finalización. Seleccione **Remove start event** (Quitar evento de inicio) si solo quiere especificar la hora de finalización. Si **Start time** (Hora de inicio) está deshabilitada, seleccione **Add start event** (Agregar evento de inicio) junto a la lista desplegable para habilitarla. 
5. En **Stop time** (Hora de finalización), seleccione la hora a la que quiere que se apaguen las máquinas virtuales. La hora de finalización es obligatoria si no se establece la hora de inicio. Seleccione **Remove stop event** (Quitar evento de finalización) si solo quiere especificar la hora de inicio. Si **Stop time** (Hora de finalización) está deshabilitada, seleccione **Add stop event** (Agregar evento de finalización) junto a la lista desplegable para habilitarla.
6. En **Time zone (required)** (Zona horaria [obligatorio]), seleccione la zona horaria de las horas de inicio y finalización que ha especificado. 
7. En **Notes** (Notas), escriba alguna descripción o nota para la programación. 
8. Seleccione **Guardar**. 

    ![Programación puntual](../media/how-to-create-schedules/add-schedule-page.png)

## <a name="add-a-recurring-schedule-weekly"></a>Agregar una programación periódica (semanal)

1. Cambie a la página **Schedules** (Programaciones) y seleccione **Add schedule** (Agregar programación) en la barra de herramientas. 

    ![Botón Add schedule (Agregar programación) en la página Schedules (Programaciones)](../media/how-to-create-schedules/add-schedule-button.png)
2. En la página **Add schedule** (Agregar programación), cambie a **Weekly** (Semanal) en la parte superior. 
3. En **Schedule days (required)** (Días de programación [obligatorio]), seleccione los días en los que quiere que se aplique la programación. En el ejemplo siguiente, está seleccionado Monday-Friday (Lunes a viernes). 
4. En el campo **From** (De), escriba la **fecha de inicio de la programación** o use el botón de **calendario** para seleccionar una fecha. Este campo es obligatorio. 
5. En **Schedule end date** (Fecha de finalización de programación), escriba o seleccione una fecha de finalización en la que se apagarán las máquinas virtuales. 
6. En **Start time** (Hora de inicio), seleccione la hora a la que se iniciarán las máquinas virtuales. La hora de inicio es obligatoria si no se establece la hora de finalización. Seleccione **Remove start event** (Quitar evento de inicio) si solo quiere especificar la hora de finalización. Si **Start time** (Hora de inicio) está deshabilitada, seleccione **Add start event** (Agregar evento de inicio) junto a la lista desplegable para habilitarla. 
7. En **Stop time** (Hora de finalización), seleccione la hora a la que se apagarán las máquinas virtuales. La hora de finalización es obligatoria si no se establece la hora de inicio. Seleccione **Remove stop event** (Quitar evento de finalización) si solo quiere especificar la hora de inicio. Si **Stop time** (Hora de finalización) está deshabilitada, seleccione **Add stop event** (Agregar evento de finalización) junto a la lista desplegable para habilitarla.
8. En **Time zone (required)** (Zona horaria [obligatorio]), seleccione la zona horaria de las horas de inicio y finalización que ha especificado.  
9. En **Notes** (Notas), escriba alguna descripción o nota para la programación. 
10. Seleccione **Guardar**. 

    ![Programación semanal](../media/how-to-create-schedules/add-schedule-page-weekly.png)

## <a name="view-schedules-in-calendar"></a>Visualización de las programaciones en el calendario
Puede ver las fechas y horas programadas resaltadas en la vista de calendario, como se muestra en la imagen siguiente:

![Programaciones en la vista de calendario](../media/how-to-create-schedules/schedules-in-calendar.png)

Seleccione el botón **Today** (Hoy) en la esquina superior derecha para cambiar a la fecha actual en el calendario. Seleccione la **flecha izquierda** para cambiar a la semana anterior y la **flecha derecha** para cambiar a la semana siguiente en el calendario. 

## <a name="edit-a-schedule"></a>Edición de una programación
Al hacer doble clic en una programación resaltada en el calendario o seleccionar el **lápiz** situado en la barra de herramientas, verá la página **Edit schedule** (Editar programación). Actualizar la configuración de esta página es lo mismo que actualizar la configuración de la página **Add schedule** (Agregar programación), como se describe en la sección [Agregar una programación periódica](#add-a-recurring-schedule-weekly). 

![Página Edit schedule (Editar programación)](../media/how-to-create-schedules/edit-schedule-page.png)

## <a name="delete-a-schedule"></a>Eliminación de una programación

1. Para eliminar una programación, seleccione el botón de papelera (eliminar) en la barra de herramientas, como se muestra en la siguiente imagen:

    ![Botón de eliminación en la barra de herramientas](../media/how-to-create-schedules/delete-schedule-button.png)

    Puede usar el botón de eliminación con cualquiera de las fechas y horas programadas en el calendario. 
2. En la página **Delete schedules** (Eliminar programaciones), seleccione **Yes** (Sí).

    ![Eliminar confirmación de programaciones](../media/how-to-create-schedules/delete-schedules-confirmation.png)




## <a name="next-steps"></a>Pasos siguientes
Consulte los artículos siguientes:

- [Como administrador, crear y administrar cuentas de laboratorio](how-to-manage-lab-accounts.md)
- [Como propietario del laboratorio, crear y administrar laboratorios](how-to-manage-classroom-labs.md)
- [Como propietario del laboratorio, configurar y controlar el uso de un laboratorio](how-to-configure-student-usage.md)
- [Como usuario del laboratorio, obtener acceso a laboratorios educativos](how-to-use-classroom-lab.md)
