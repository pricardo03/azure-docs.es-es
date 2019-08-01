---
title: 'Azure Backup: Creación de almacenes de Recovery Services'
description: Creación de almacenes de Recovery Services que almacena las copias de seguridad y los puntos de recuperación
ms.reviewer: sogup
author: dcurwin
manager: carmonm
keywords: Almacén de Recovery Services; copia de seguridad de máquina virtual de Azure; restauración de máquina virtual de Azure;
ms.service: backup
ms.topic: conceptual
ms.date: 05/30/2019
ms.author: dacurwin
ms.openlocfilehash: d13aff40d735e98e7745358f8caddcd8cdc20727
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688554"
---
# <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

Un almacén de Recovery Services es una entidad que almacena las copias de seguridad y los puntos de recuperación creados a lo largo del tiempo. También contiene las directivas de copia de seguridad asociadas con las máquinas virtuales protegidas.

Para crear un almacén de Recovery Services:

1. Inicie sesión en su suscripción en [Azure Portal](https://portal.azure.com/).

2. En el menú izquierdo, seleccione **Todos los servicios**.

    ![Seleccionar Todos los servicios](./media/backup-create-rs-vault/click-all-services.png)

3. En el cuadro de diálogo **Todos los servicios**, escriba **Recovery Services**. La lista de recursos se filtra en función de lo que especifique. En la lista de recursos, seleccione **Almacenes de Recovery Services**.

    ![Escribir y elegir Almacenes de Recovery Services](./media/backup-create-rs-vault/all-services.png)

    Aparece la lista de almacenes de Recovery Services de la suscripción.

4. En el panel **Almacenes de Recovery Services**, seleccione **Agregar**.

    ![Agregar un almacén de Recovery Services](./media/backup-create-rs-vault/add-button-create-vault.png)

    Se abre el cuadro de diálogo **Almacén de Recovery Services**. Especifique los valores de **Nombre**, **Suscripción**, **Grupo de recursos** y **Ubicación**.

    ![Configurar el almacén de Recovery Services](./media/backup-create-rs-vault/create-new-vault-dialog.png)

   - **Nombre**: escriba un nombre descriptivo que identifique el almacén. El nombre debe ser único para la suscripción de Azure. Especifique un nombre que tenga entre 2 y 50 caracteres. El nombre debe comenzar por una letra y consta solo de letras, números y guiones.
   - **Suscripción**: elija la suscripción que desee usar. Si es miembro de una sola suscripción, verá solo ese nombre. Si no está seguro de qué suscripción va a utilizar, use la predeterminada (sugerida). Solo hay varias opciones si la cuenta profesional o educativa está asociada a más de una suscripción de Azure.
   - **Grupo de recursos**: Use un grupo de recursos existente o cree uno. Para ver la lista de grupos de recursos disponibles en una suscripción, seleccione **Usar existente** y, después, seleccione un recurso en el cuadro de lista desplegable. Para crear un grupo de recursos, seleccione **Crear nuevo** y escriba el nombre. Para más información acerca de los grupos de recursos, consulte [Introducción a Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
   - **Ubicación**: seleccione la región geográfica del almacén. Para crear un almacén para proteger máquinas virtuales, el almacén **debe** estar en la misma región que las máquinas virtuales.

      > [!IMPORTANT]
      > Si no está seguro de la ubicación de la máquina virtual, cierre el cuadro de diálogo. Vaya a la lista de máquinas virtuales en el portal. Si tiene máquinas virtuales en varias regiones, cree un almacén de Recovery Services en cada una de ellas. Cree el almacén en la primera ubicación, antes de crear el almacén de otra. No es preciso especificar cuentas de almacenamiento para almacenar los datos de la copia de seguridad. Tanto el almacén de Recovery Services como el de Azure Backup lo controlan automáticamente.
      >
      >

5. Cuando esté listo para crear el almacén de Recovery Services, seleccione **Crear**.

    ![Crear almacén de Recovery Services](./media/backup-create-rs-vault/click-create-button.png)

    La creación del almacén de Recovery Services puede tardar unos minutos. Supervise las notificaciones de estado en el área de **notificaciones** de la parte superior derecha del portal. Tras crear el almacén, se ve en la lista de almacenes de Recovery Services. Si no lo ve, haga clic en **Actualizar**.

     ![Actualizar lista de almacenes de Backup](./media/backup-create-rs-vault/refresh-button.png)

## <a name="set-storage-redundancy"></a>Establecimiento de la redundancia de almacenamiento

Azure Backup administra automáticamente el almacenamiento para el almacén. Debe especificar cómo se replica ese almacenamiento.

1. En la hoja **Almacenes de Recovery Services**, haga clic en el almacén nuevo. En la sección **Configuración**, haga clic en **Propiedades**.
2. En **Propiedades**, en **Configuración de copia de seguridad**, haga clic en **Actualizar**.

3. Seleccione el tipo de replicación de almacenamiento y haga clic en **Guardar**.

     ![Establecimiento de la configuración de almacenamiento del nuevo almacén](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

   - Se recomienda que, si usa Azure como punto de conexión del almacenamiento de copia de seguridad principal, siga utilizando la configuración **con redundancia geográfica** predeterminada.
   - Si no utiliza Azure como punto de conexión de almacenamiento de copia de seguridad principal, elija **Redundancia local** para reducir los costes de almacenamiento de Azure.
   - Obtenga más información sobre la redundancia [geográfica](../storage/common/storage-redundancy-grs.md) y [local](../storage/common/storage-redundancy-lrs.md).

> [!NOTE]
> El cambio del **tipo de replicación de almacenamiento** (con redundancia local o con redundancia geográfica) para un almacén de Recovery Services debe realizarse antes de configurar las copias de seguridad en el almacén. Una vez configure la copia de seguridad, la opción para modificar está deshabilitada y no puede cambiar el **tipo de replicación del almacenamiento**. 

## <a name="next-steps"></a>Pasos siguientes

[Más información](backup-azure-recovery-services-vault-overview.md) sobre los almacenes de Recovery Services.
[Más información](backup-azure-delete-vault.md) sobre la eliminación de los almacenes de Recovery Services.
