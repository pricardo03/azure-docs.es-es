---
title: Administración de bases de datos de SAP HANA con copia de seguridad en máquinas virtuales de Azure
description: En este artículo, aprenderá las tareas comunes para administrar y supervisar las bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure.
ms.topic: conceptual
ms.date: 11/12/2019
ms.openlocfilehash: f76054c7c78c55a9754975267ee4fa3caab968a3
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74287522"
---
# <a name="manage-and-monitor-backed-up-sap-hana-databases"></a>Administración y supervisión de bases de datos de SAP HANA de las que se ha realizado copia de seguridad

En este artículo se describen las tareas comunes para administrar y supervisar bases de datos de SAP HANA que se ejecutan en una máquina virtual de Azure cuya copia de seguridad se ha realizado en un almacén de Azure Backup Recovery Services con el servicio [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview). Aprenderá cómo supervisar los trabajos y las alertas, desencadenar las copias de seguridad a petición, detener y reanudar la protección de la base de datos y anular el registro de una máquina virtual de las copias de seguridad.

Si todavía no ha configurado las copias de seguridad para las bases de datos de SAP HANA, consulte [Creación de copias de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Supervisión de trabajos de copia de seguridad manuales en el portal

Azure Backup muestra todos los trabajos desencadenados manualmente en la sección **Trabajos de copia de seguridad** de Azure Portal.

![Sección Trabajos de copia de seguridad](./media/sap-hana-db-manage/backup-jobs.png)

Los trabajos que vea en este portal incluyen operaciones de detección y registro de base de datos, y copia de seguridad y restauración. Los trabajos programados, incluidas las copias de seguridad de registros, no se muestran en esta sección. Las copias de seguridad desencadenadas manualmente desde los clientes nativos de SAP HANA (Studio/Cockpit/DBA Cockpit) tampoco se muestran aquí.

![Lista de trabajos de copia de seguridad](./media/sap-hana-db-manage/backup-jobs-list.png)

Para obtener más información sobre la supervisión, vaya a [Supervisión en Azure Portal](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) y [Supervisión con Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor).

## <a name="view-backup-alerts"></a>Visualización de alertas de copia de seguridad

Las alertas son un medio sencillo de supervisar las copias de seguridad de bases de datos de SAP HANA. Las alertas le ayudan a centrarse en los eventos que más le interesan sin perderse en la multitud de eventos que genera una copia de seguridad. Azure Backup permite establecer alertas y se pueden supervisar de la siguiente manera:

* Inicie sesión en el [Azure Portal](https://portal.azure.com/).
* En el panel del almacén, seleccione **Alertas de copia de seguridad**.

  ![Alertas de copia de seguridad en el panel del almacén](./media/sap-hana-db-manage/backup-alerts-dashboard.png)

* Podrá ver las alertas:

  ![Lista de alertas de copia de seguridad](./media/sap-hana-db-manage/backup-alerts-list.png)

* Haga clic en las alertas para ver más detalles:

  ![Detalles de alertas](./media/sap-hana-db-manage/alert-details.png)

En la actualidad, Azure Backup permite el envío de alertas por correo electrónico. Estas alertas:

* Se desencadenan para todos los errores de copia de seguridad.
* Se consolidan en el nivel de base de datos por código de error.
* Se envían solo para el primer error de copia de seguridad de una base de datos.

Para obtener más información sobre la supervisión, vaya a [Supervisión en Azure Portal](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor) y [Supervisión con Azure Monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor).

## <a name="management-operations"></a>Operaciones de administración

Azure Backup facilita la administración de una base de datos de SAP HANA de la que se hizo una copia de seguridad a través de una gran cantidad de operaciones de administración con las que es compatible. En las siguientes secciones se detalla cada una de estas operaciones.

### <a name="run-an-ad-hoc-backup"></a>Ejecute una copia de seguridad ad-hoc.

Las copias de seguridad se ejecutan según la programación de la directiva. Puede ejecutar una copia de seguridad a petición siguiendo estos pasos:

1. En el menú Almacén, haga clic en **Elementos de copia de seguridad**.
2. En **Elementos de copia de seguridad**, seleccione la máquina virtual que ejecuta la base de datos de SAP HANA y, a continuación, haga clic en **Hacer copia de seguridad ahora**.
3. En **Realizar copia de seguridad ahora**, use el control del calendario para seleccionar el último día que debería retenerse el punto de recuperación. A continuación, haga clic en **Aceptar**.
4. Supervise las notificaciones del portal. Puede supervisar el progreso del trabajo en el panel del almacén > **Trabajos de copia de seguridad** > **En curso**. Según el tamaño de la base de datos, la creación de la copia de seguridad inicial puede tardar un tiempo.

### <a name="run-sap-hana-native-client-backup-on-a-database-with-azure-backup-enabled"></a>Ejecución de una copia de seguridad del cliente nativo de SAP HANA en una base de datos con Azure Backup habilitado

Si quiere hacer una copia de seguridad local (mediante HANA Studio/Cockpit) de una base de datos de la que se está haciendo una copia de seguridad con Azure Backup, haga lo siguiente:

1. Espere a que finalicen las copias de seguridad completas o de registro de la base de datos. Compruebe el estado en SAP HANA Studio o Cockpit.
2. Inhabilite las copias de seguridad de registros y establezca el catálogo de copias de seguridad al sistema de archivos de la base de datos pertinente.
3. Para ello, haga doble clic en **systemdb** > **Configuración** > **Seleccionar base de datos** > **Filtro (registro)** .
4. Establezca **enable_auto_log_backup** en **No**.
5. Establezca **log_backup_using_backint** en **False**.
6. A continuación, haga una copia de seguridad completa ad hoc de la base de datos.
7. Espere a que finalicen la copia de seguridad completa y la copia de seguridad de catálogos.
8. Revierta la configuración anterior a las opciones de Azure:
   * Establezca **enable_auto_log_backup** en **Sí**.
   * Establezca **log_backup_using_backint** en **True**.

### <a name="edit-underlying-policy"></a>Edición de directivas subyacentes

Modifique la directiva para cambiar la frecuencia de las copias de seguridad o la duración de retención:

* En el panel del almacén, vaya a **Administrar** > **Directivas de copia de seguridad**.

  ![Directivas de copia de seguridad en el panel del almacén](./media/sap-hana-db-manage/backup-policies-dashboard.png)

* Elija la directiva que quiere editar:

  ![Lista de directivas de copia de seguridad](./media/sap-hana-db-manage/backup-policies-list.png)

  ![Detalles de la directiva de copia de seguridad](./media/sap-hana-db-manage/backup-policy-details.png)

>[!NOTE]
> Cualquier cambio en el período de retención se aplicará de manera retrospectiva a todos los puntos de recuperación más antiguos, además de los nuevos.
>
> Las directivas de copia de seguridad incrementales no se pueden usar para las bases de datos de SAP HANA. Las copias de seguridad incrementales no se admite actualmente para estas bases de datos.

### <a name="stop-protection-for-an-sap-hana-database"></a>Detención de la protección para una base de datos de SAP HANA

Puede dejar de proteger una base de datos SAP HANA de dos maneras:

* Detener todos los trabajos futuros de copia de seguridad y eliminar todos los puntos de recuperación.
* Detener todos los trabajos futuros de copia de seguridad y dejar intactos los puntos de recuperación.

Si decide dejar los puntos de recuperación, tenga en cuenta estos detalles:

* Todos los puntos de recuperación permanecerán intactos para siempre; al detenerse la protección de los datos se detendrá la eliminación de todos los puntos y se conservarán los datos.
* Se le cobrará la instancia protegida y el almacenamiento consumido. Para más información, consulte [Precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Si elimina un origen de datos sin detener las copias de seguridad, las nuevas copias de seguridad producirán errores.

Para detener la protección de una base de datos:

* En el panel del almacén, seleccione **Elementos de copia de seguridad**.
* En **Tipo de administración de copias de seguridad**, seleccione **SAP HANA en la VM de Azure**.

  ![Selección de SAP HANA en la VM de Azure](./media/sap-hana-db-manage/sap-hana-azure-vm.png)

* Seleccione la base de datos para la que quiere detener la protección:

  ![Seleccionar la base de datos en la que detener la protección](./media/sap-hana-db-manage/select-database.png)

* En el menú de la base de datos, seleccione **Detener copia de seguridad**.

  ![Seleccionar Detener copia de seguridad](./media/sap-hana-db-manage/stop-backup.png)

* En el menú **Detener copia de seguridad**, seleccione si desea conservar o eliminar los datos. También puede proporcionar una razón y un comentario.

  ![Seleccionar Conservar o eliminar datos](./media/sap-hana-db-manage/retain-backup-data.png)

* Seleccione **Detener copia de seguridad**.

### <a name="resume-protection-for-an-sap-hana-database"></a>Reanudación de la protección para una base de datos de SAP HANA

Si seleccionó la opción **Retener datos de copia de seguridad** al detener la protección de la base de datos de SAP HANA, se puede reanudar la protección más adelante. Si no conserva los datos de los que se realizó una copia de seguridad, no podrá reanudar la protección.

Para reanudar la protección para una base de datos de SAP HANA:

* Abra el elemento de copia de seguridad y seleccione **Reanudar copia de seguridad**.

   ![Seleccione Reanudar la copia de seguridad](./media/sap-hana-db-manage/resume-backup.png)

* En el menú **Directiva de copia de seguridad**, seleccione una directiva y, después, seleccione **Guardar**.

### <a name="upgrading-from-sap-hana-10-to-20"></a>Actualización de SAP HANA 1.0 a 2.0

Obtenga información sobre cómo continuar con la copia de seguridad de una base de datos de SAP HANA [después de actualizar de SAP HANA 1.0 a 2.0](backup-azure-sap-hana-database-troubleshoot.md#upgrading-from-sap-hana-10-to-20).

### <a name="upgrading-without-a-sid-change"></a>Actualización sin un cambio de Id. de seguridad

Obtenga información sobre cómo continuar con la copia de seguridad de una base de datos de SAP HANA cuyo [Id. de seguridad no haya cambiado después de la actualización](backup-azure-sap-hana-database-troubleshoot.md#upgrading-without-an-sid-change).

### <a name="unregister-an-sap-hana-database"></a>Anulación del registro de una base de datos de SAP HANA

Anule el registro de una instancia de SAP HANA después de deshabilitar la protección, pero antes de eliminar el almacén:

* En el panel del almacén, en **Administrar**, seleccione **Infraestructura de Backup**.

   ![Seleccionar Infraestructura de Backup](./media/sap-hana-db-manage/backup-infrastructure.png)

* Seleccione el **Tipo de administración de copia de seguridad** como **Carga de trabajo en la VM de Azure**.

   ![Seleccione el Tipo de administración de copia de seguridad como Carga de trabajo en la VM de Azure.](./media/sap-hana-db-manage/backup-management-type.png)

* En **Servidores protegidos**, seleccione la instancia de la que quiere anular el registro. Para eliminar el almacén, debe anular el registro de todos los servidores o instancias.

* Haga clic con el botón derecho en la instancia protegida y seleccione **Anular registro**.

   ![Seleccionar Anular registro](./media/sap-hana-db-manage/unregister.png)

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información acerca de cómo [solucionar problemas comunes al realizar copias de seguridad de bases de datos de SAP HANA](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot).

