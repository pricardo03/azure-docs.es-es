---
title: archivo de inclusión
description: archivo de inclusión
services: backup
author: markgalioto
ms.service: backup
ms.topic: include
ms.date: 5/14/2018
ms.author: markgal
ms.custom: include file
ms.openlocfilehash: 5590da80a1c217e7902e8e010688e40f5624898c
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2018
ms.locfileid: "34664962"
---
## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services
Un almacén de Recovery Services es una entidad que almacena las copias de seguridad y los puntos de recuperación creados a lo largo del tiempo. También contiene las directivas de copia de seguridad asociadas con las máquinas virtuales protegidas.

Para crear un almacén de Recovery Services:

1. Inicie sesión en su suscripción en [Azure Portal](https://portal.azure.com/).
2. En el menú izquierdo, seleccione **Todos los servicios**.

    ![Elija la opción Todos los servicios en el menú principal](./media/backup-create-rs-vault/click-all-services.png) <br/>

3. En el cuadro de diálogo Todos los servicios, escriba *Recovery Services*. En cuanto empiece a escribirlo, la entrada filtra la lista de recursos. Cuando vea la opción **Almacenes de Recovery Services**, selecciónela.

    ![Escriba Recovery Services en el cuadro de diálogo Todos los servicios](./media/backup-create-rs-vault/all-services.png) <br/>

    Aparece la lista de almacenes de Recovery Services de la suscripción.
4. En el menú **Almacenes de Recovery Services**, seleccione **Agregar**.

    ![Creación del almacén de Recovery Services, paso 2](./media/backup-create-rs-vault/add-button-create-vault.png)

    Se abre el menú **Almacenes de Recovery Services**. En él se le solicita que rellene los campos **Nombre**, **Suscripción**, **Grupo de recursos** y **Ubicación**.

    ![Panel "Almacenes de Recovery Services"](./media/backup-create-rs-vault/create-new-vault-dialog.png)
5. En **Nombre**, escriba un nombre descriptivo que identifique el almacén. El nombre debe ser único para la suscripción de Azure. Escriba un nombre que contenga un mínimo de 2 caracteres y un máximo de 50. El nombre debe comenzar por una letra y solo puede contener letras, números y guiones.
6. En **Suscripción**, elija la suscripción que desee usar. Si es miembro de una sola suscripción, aparecerá ese nombre. Si no está seguro de la suscripción que va a utilizar, use la predeterminada (o sugerida). Solo hay varias opciones si la cuenta profesional o educativa está asociada a varias suscripciones de Azure.
7. Como **Grupo de recursos** puede usar un grupo de recursos existente o crear uno nuevo. Seleccione **Usar existente** y haga clic en el menú desplegable para ver la lista de grupos de recursos disponibles. Para crear un nuevo grupo de recursos, seleccione **Crear nuevo** y escriba el nombre. Para más información sobre los grupos de recursos, consulte [Información general de Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md).
8. En **Ubicación**, seleccione la región geográfica del almacén. Si va a crear un almacén para proteger máquinas virtuales, el almacén *debe* estar en la misma región que las máquinas virtuales.

   > [!IMPORTANT]
   > Si no está seguro de cuál es ubicación en la que se encuentra la máquina virtual, cierre el cuadro de diálogo de creación del almacén y vaya a la lista de máquinas virtuales del portal. Si tiene máquinas virtuales en varias regiones, cree un almacén de Recovery Services en cada una de ellas. Cree el almacén en la primera ubicación antes de pasar a la siguiente ubicación. No hay necesidad de especificar cuentas de almacenamiento para almacenar los datos de la copia de seguridad. Tanto el almacén de Recovery Services como el de Azure Backup lo controlan automáticamente.
   >
   >

9. Cuando esté listo para crear el almacén de Recovery Services, haga clic en **Crear**.

    ![Lista de copias de seguridad](./media/backup-create-rs-vault/click-create-button.png)

    La creación del almacén de Recovery Services puede tardar unos minutos. Supervise las notificaciones de estado en la sección de Notificaciones (en la parte superior derecha del portal). Tras crear el almacén, este ya aparece en la lista de almacenes de Recovery Services. Si no ve el almacén, haga clic en **Actualizar**.

     ![Lista de copias de seguridad](./media/backup-create-rs-vault/refresh-button.png)
