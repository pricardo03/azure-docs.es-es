---
title: Administración y supervisión de copias de seguridad del agente de MARS
description: Aprenda a administrar y supervisar las copias de seguridad del agente de Microsoft Azure Recovery Services (MARS) con el servicio Azure Backup.
ms.reviewer: srinathv
ms.topic: conceptual
ms.date: 10/07/2019
ms.openlocfilehash: edb672f97f467378176bf6cdf04fe8e22cc5e51b
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74173079"
---
# <a name="manage-microsoft-azure-recovery-services-mars-agent-backups-by-using-the-azure-backup-service"></a>Administración de copias de seguridad del agente de Microsoft Azure Recovery Services (MARS) con el servicio Azure Backup

En este artículo se describe cómo administrar archivos y carpetas de los que se ha realizado una copia de seguridad con el agente de Microsoft Azure Recovery Services.

## <a name="create-a-backup-policy"></a>Creación de una directiva de copia de seguridad

La directiva de copia de seguridad especifica cuándo tomar instantáneas de los datos para crear puntos de recuperación y cuánto tiempo se conservan los puntos de recuperación. La directiva de copia de seguridad se configura mediante el agente de MARS.

Cree una directiva de la siguiente manera:

1. Después de descargar y registrar el agente de MARS, inicie la consola del agente. Para encontrarlo, busque **Microsoft Azure Backup**en la máquina.  
2. En **Acciones**, haga clic en **Programar copia de seguridad**.

    ![Programar una copia de seguridad de Windows Server](./media/backup-configure-vault/schedule-first-backup.png)
3. En el Asistente para programar copias de seguridad, en **Introducción**, haga clic en **Siguiente**.
4. En la página **Seleccionar elementos de los que realizar copia de seguridad**, haga clic en **Agregar elementos**.

    ![Seleccionar elementos de los que realizar copia de seguridad](./media/backup-azure-manage-mars/select-item-to-backup.png)

5. En **Seleccionar elementos**, seleccione lo que desee incluir en la copia de seguridad y haga clic en **Aceptar**.

    ![Elementos seleccionados para realizar una copia de seguridad](./media/backup-azure-manage-mars/selected-items-to-backup.png)

6. En la página **Seleccionar elementos de los que realizar copia de seguridad**, haga clic en **Siguiente**.
7. En la página **Especifique la programación de copia de seguridad**, especifique cuándo desea realizar copias de seguridad diarias o semanales. A continuación, haga clic en **Siguiente**.

    - Cuando se realiza una copia de seguridad, se crea un punto de recuperación.
    - El número de puntos de recuperación creados en el entorno depende de la programación de copia de seguridad.

8. Puede programar copias de seguridad diarias, hasta tres veces al día. Por ejemplo, en la captura de pantalla se muestran dos copias de seguridad diarias, una a medianoche y otra a las 6:00 p. m.

    ![Programación diaria](./media/backup-configure-vault/day-schedule.png)

9. También puede ejecutar copias de seguridad semanales. Por ejemplo, en la captura de pantalla se muestran copias de seguridad tomadas cada domingo y miércoles alternos, a las 9:30 a. m. y a la 1:00 a. m.

    ![Programación semanal](./media/backup-configure-vault/week-schedule.png)

10. En la página **Seleccionar directiva de retención**, especifique cómo desea almacenar las copias históricas de los datos. A continuación, haga clic en **Siguiente**.

    - La configuración de retención especifica qué puntos de recuperación deben almacenarse y durante cuánto tiempo.
    - Por ejemplo, cuando se establece una configuración de retención diaria, se indica que, en el momento especificado para la retención diaria, se conservará el punto de recuperación más reciente durante el número de días especificado. También puede especificar una directiva de retención mensual para indicar que el punto de recuperación creado el 30 de cada mes debe almacenarse durante 12 meses.
    - La retención diaria y semanal del punto de recuperación normalmente coincide con la programación de copia de seguridad. Es decir, cuando se desencadena la copia de seguridad de acuerdo con la programación, el punto de recuperación creado por la copia de seguridad se almacena durante el tiempo indicado en la directiva de retención diaria o semanal.
    - En la siguiente captura de pantalla se ofrece un ejemplo: - Las copias de seguridad diarias a medianoche y a las 6:00 p. m. se conservan durante siete días.
            - Las copias de seguridad realizadas un sábado a medianoche y a las 6:00 p. m. se conservan durante cuatro semanas.
            - Las copias de seguridad tomadas el sábado de la última semana del mes a medianoche y a las 6:00 p. m. se conservan durante 12 meses.
            - Las copias de seguridad tomadas el sábado de la última semana de marzo se conservan durante 10 años.

    ![Ejemplo de retención](./media/backup-configure-vault/retention-example.png)

11. En **Elija el tipo de copia de seguridad inicial** decida si desea realizar la copia de seguridad inicial a través de la red o si usar la copia de seguridad sin conexión (para más información sobre la copia de seguridad sin conexión, consulte este [artículo](backup-azure-backup-import-export.md)). Para realizar la copia de seguridad inicial a través de la red, seleccione **Automáticamente por encima de la red** y haga clic en **Siguiente**.

    ![Tipo de copia de seguridad inicial](./media/backup-azure-manage-mars/choose-initial-backup-type.png)

12. En **Confirmación**, revise la información y, luego, haga clic en **Finalizar**.
    ![Confirmación del tipo de copia de seguridad](./media/backup-azure-manage-mars/confirm-backup-type.png)

13. Cuando el asistente termine de crear la programación de copia de seguridad, haga clic en **Cerrar**.
  ![Confirmación de modificación del proceso de copia de seguridad](./media/backup-azure-manage-mars/confirm-modify-backup-process.png)

Debe crear una directiva en cada máquina en la que esté instalado el agente.

## <a name="modify-a-backup-policy"></a>Modificación de una directiva de copia de seguridad

Al modificar la directiva de copia de seguridad, puede agregar nuevos elementos, quitar elementos existentes de la copia de seguridad o excluir archivos de la copia de seguridad mediante la configuración de exclusión.

- **Agregar elementos**: use esta opción solo para agregar nuevos elementos a la copia de seguridad. Para quitar elementos existentes, use **Quitar elementos** o la opción **Configuración de exclusión**.  
- **Quitar elementos**: utilice esta opción para quitar elementos de la copia de seguridad.
  - Para quitar todos los elementos de un volumen, utilice **Configuración de exclusión** en lugar de **Quitar elementos**.
  - Si se borran todas las selecciones de un volumen, las copias de seguridad anteriores de los elementos se conservarán en función de la configuración de retención establecida en el momento de la última copia de seguridad, sin posibilidad de modificación.
  - Si vuelve a seleccionar estos elementos, se realizará una primera copia de seguridad completa y los cambios en la directiva nueva no se aplicarán a las copias de seguridad anteriores.
  - Si se anula la selección del volumen completo, se conservará la copia de seguridad anterior sin posibilidad de modificar la directiva de retención.
- **Configuración de exclusión**: utilice esta opción para excluir elementos específicos de la copia de seguridad.
  
### <a name="add-new-items-to-existing-policy"></a>Adición de nuevos elementos a la directiva existente

1. En **Acciones**, haga clic en **Programar copia de seguridad**.

    ![Programar una copia de seguridad de Windows Server](./media/backup-configure-vault/schedule-first-backup.png)

2. En la pestaña **Seleccionar el elemento de directiva**, seleccione **Modificar una programación de copia de seguridad de los archivos y carpetas** y haga clic en **Siguiente**.

    ![Selección de elementos de la directiva](./media/backup-azure-manage-mars/select-policy-items.png)

3. En la pestaña **Modificar o detener una copia de seguridad programada**, seleccione **Cambiar la hora o las horas de las copias de seguridad** y haga clic en **Siguiente**.

    ![Modificación o programación de una copia de seguridad](./media/backup-azure-manage-mars/modify-schedule-backup.png)

4. En la pestaña **Seleccionar elementos de los que realizar copia de seguridad**, haga clic en **Agregar elementos** para agregar los elementos de los que desea realizar una copia de seguridad.

    ![Modificación o programación de una copia de seguridad para agregar elementos](./media/backup-azure-manage-mars/modify-schedule-backup-add-items.png)

5. En la ventana **Seleccionar elementos**, seleccione los archivos o carpetas que desea agregar y haga clic en **Aceptar**.

    ![Selección de los elementos](./media/backup-azure-manage-mars/select-item.png)

6. Complete los pasos siguientes y haga clic en **Finalizar** para completar la operación.

### <a name="add-exclusion-rules-to-existing-policy"></a>Adición de reglas de exclusión a la directiva existente

Puede agregar reglas de exclusión para omitir archivos y carpetas que no desea incluir en la copia de seguridad. Esto puede hacerse al definir una nueva directiva o al modificar una directiva existente.

1. En el panel Acciones, haga clic en **Programar copia de seguridad**. Vaya a **Seleccionar elementos de los que realizar copia de seguridad** y haga clic en **Configuración de exclusión**.

    ![Selección de los elementos](./media/backup-azure-manage-mars/select-exclusion-settings.png)

2. En **Configuración de exclusión**, haga clic en **Agregar exclusión**.

    ![Selección de los elementos](./media/backup-azure-manage-mars/add-exclusion.png)

3. En **Seleccionar elementos que excluir**, explore los archivos y carpetas, seleccione los elementos que desee excluir y haga clic en **Aceptar**.

    ![Selección de los elementos](./media/backup-azure-manage-mars/select-items-exclude.png)

4. De forma predeterminada, se excluirán todas las **subcarpetas** de las carpetas seleccionadas. Puede seleccionar **Sí** o **No** para cambiar esto. Puede editar y especificar los tipos de archivo que excluir como se muestra a continuación:

    ![Selección de los elementos](./media/backup-azure-manage-mars/subfolders-type.png)

5. Complete los pasos siguientes y haga clic en **Finalizar** para completar la operación.

### <a name="remove-items-from-existing-policy"></a>Quitar elementos de la directiva existente

1. En el panel Acciones, haga clic en **Programar copia de seguridad**. Vaya a **Seleccionar elementos de los que realizar copia de seguridad**. En la lista, seleccione los archivos y las carpetas que desea quitar de la programación de copia de seguridad y haga clic en **Quitar elementos**.

    ![Selección de los elementos](./media/backup-azure-manage-mars/select-items-remove.png)

> [!NOTE]
> Proceda con precaución al quitar completamente un volumen de la directiva.  Si necesita volver a agregarlo, se tratará como un nuevo volumen. La siguiente copia de seguridad programada realizará una copia de seguridad inicial (copia de seguridad completa) en lugar de una copia de seguridad incremental. Si necesita quitar y agregar temporalmente elementos más adelante, se recomienda usar **Configuración de exclusión** en lugar de **Quitar elementos** para garantizar una copia de seguridad incremental en lugar de una copia de seguridad completa.

2. Complete los pasos siguientes y haga clic en **Finalizar** para completar la operación.

## <a name="stop-protecting-files-and-folder-backup"></a>Dejar de proteger la copia de seguridad de carpetas y archivos

Hay dos formas de detener la protección de una copia de seguridad de archivos y carpetas:

- **Detener la protección y conservar los datos de copia de seguridad**.
  - Esta opción detendrá la protección para todos los trabajos de copia de seguridad futuros.
  - El servicio Azure Backup conservará los puntos de recuperación de los que se ha realizado una copia de seguridad conforme a la directiva de retención.
  - Podrá restaurar los datos de copia de seguridad de los puntos de recuperación que no hayan expirado.
  - Si decide reanudar la protección, podrá usar la opción *Vuelva a habilitar la programación de copia de seguridad*. Después de eso, los datos se conservarán de acuerdo con la nueva directiva de retención.
- **Detener la protección y eliminar los datos de copia de seguridad**.
  - Esta opción hará que todos los trabajos de copia de seguridad futuros dejen de proteger los datos y eliminará todos los puntos de recuperación.
  - Recibirá un correo electrónico de alerta de eliminación de datos de copia de seguridad con el mensaje *Your data for this Backup item has been deleted. This data will be temporarily available for 14 days, after which it will be permanently deleted* (Los datos de este elemento de copia de seguridad se han eliminado. Estos datos estarán disponibles temporalmente durante un período de 14 días, tras el cual se eliminarán de forma permanente) y la acción recomendada *Reprotect the Backup item within 14 days to recover your data* (Vuelva a proteger el elemento de copia de seguridad en el plazo de 14 días para recuperar los datos).
  - Para reanudar la protección, vuelva a proteger el elemento en el plazo de 14 días a partir de la operación de eliminación.

### <a name="stop-protection-and-retain-backup-data"></a>Detener la protección y conservar los datos de copia de seguridad

1. Abra la consola de administración de MARS, vaya al **panel Acciones** y **seleccione Programar copia de seguridad**.
    ![Modificación o detención de una copia de seguridad programada.](./media/backup-azure-manage-mars/mars-actions.png)
1. En la página **Seleccionar el elemento de directiva**, seleccione **Modificar una programación de copia de seguridad de los archivos y carpetas** y haga clic en **Siguiente**.
    ![Modificación o detención de una copia de seguridad programada.](./media/backup-azure-manage-mars/select-policy-item-retain-data.png)
1. En la página **Modificar o detener una copia de seguridad programada**, seleccione **Detener la programación de copias de seguridad, pero conservar las copias de seguridad almacenadas hasta que se vuelva a activar una programación**. Después, seleccione **Siguiente**.  
    ![Modificación o detención de una copia de seguridad programada.](./media/backup-azure-manage-mars/stop-schedule-backup.png)
1. En **Pausar copias de seguridad programadas**, revise la información y haga clic en **Finalizar**. ![Modificación o detención de una copia de seguridad programada.](./media/backup-azure-manage-mars/pause-schedule-backup.png)
1. En **Modify backup process** (Modificar el proceso de copia de seguridad), compruebe si el estado de pausa de la copia de seguridad programada es correcto y haga clic en **Cerrar** para finalizar.

### <a name="stop-protection-and-delete-backup-data"></a>Detener la protección y eliminar los datos de copia de seguridad

1. Abra la consola de administración de MARS, vaya al panel **Acciones** y seleccione **Programar copia de seguridad**.
2. En la página **Modificar o detener una copia de seguridad programada**, seleccione **Dejar de utilizar esta programación de copias de seguridad y eliminar todas las copias de seguridad almacenadas**. Después, seleccione **Siguiente**.

    ![Modificación o detención de una copia de seguridad programada.](./media/backup-azure-delete-vault/modify-schedule-backup.png)

3. En la página **Detención de una copia de seguridad programada**, seleccione **Finalizar**.

    ![Detención de una copia de seguridad programada.](./media/backup-azure-delete-vault/stop-schedule-backup.png)
4. Se le pedirá que escriba un PIN (número de identificación personal) de seguridad, que debe generar manualmente. Para ello, primero inicie sesión en Azure Portal.
5. Vaya a **Almacén de Recovery Services** > **Configuración** > **Propiedades**.
6. En **PIN de seguridad**, seleccione **Generar**. Copie este código PIN. Este PIN solo es válido durante cinco minutos.
7. En la consola de administración, pegue el código PIN y seleccione **Aceptar**.

    ![Generación de un PIN de seguridad.](./media/backup-azure-delete-vault/security-pin.png)

8. En la página **Modificar progreso de la copia de seguridad**, aparece el mensaje siguiente: *Los datos de copia de seguridad eliminados se conservarán durante 14 días. Tras ese período, se eliminarán permanentemente los datos de copia de seguridad.*  

    ![Eliminación de la infraestructura de copia de seguridad.](./media/backup-azure-delete-vault/deleted-backup-data.png)

Después de eliminar los elementos de copia de seguridad locales, siga los pasos siguientes desde el portal.

## <a name="re-enable-protection"></a>Volver a habilitar la protección

Si ha detenido la protección y conservado los datos y, después, decide reanudar la protección, puede volver a habilitar la programación de copia de seguridad mediante la modificación de la directiva de copia de seguridad.

1. En **Acciones**, seleccione **Programar copia de seguridad**.
1. Seleccione **Vuelva a habilitar la programación de copia de seguridad. También puede modificar las horas o los elementos de la copia de seguridad** y haga clic en **Siguiente**.
    ![Eliminación de la infraestructura de copia de seguridad.](./media/backup-azure-manage-mars/re-enable-policy-next.png)
1. En **Seleccionar elementos de los que realizar copia de seguridad**, haga clic en **Siguiente**.
    ![Eliminación de la infraestructura de copia de seguridad.](./media/backup-azure-manage-mars/re-enable-next.png)
1. En **Especifique la programación de copia de seguridad**, indique la programación de copia de seguridad y haga clic en **Siguiente**.
1. En **Seleccionar directiva de retención**, especifique la duración de la retención y haga clic en **Siguiente**.
1. Por último, en la pantalla **Confirmación**, revise los detalles de la directiva y haga clic en **Finalizar**.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre los escenarios admitidos y las limitaciones, consulte la [matriz de compatibilidad para MARS](https://docs.microsoft.com/azure/backup/backup-support-matrix-mars-agent).
- Obtenga más información acerca del [comportamiento de retención de la directiva de copia de seguridad a petición](backup-configure-vault.md#on-demand-backup-policy-retention-behavior).
