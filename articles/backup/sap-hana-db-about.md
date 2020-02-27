---
title: Acerca de la copia de seguridad de bases de datos SAP HANA en máquinas virtuales de Azure
description: En este artículo obtendrá información sobre cómo realizar copias de seguridad de bases de datos SAP HANA que se ejecutan en máquinas virtuales de Azure.
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: a1be572d6462ed8f8a86db0f5cbfeaaa37c219ab
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77586570"
---
# <a name="about-sap-hana-database-backup-in-azure-vms"></a>Acerca de la copia de seguridad de bases de datos SAP HANA en máquinas virtuales de Azure

Las bases de datos de SAP HANA son cargas de trabajo críticas que requieren un objetivo de punto de recuperación (RPO) bajo y un objetivo de tiempo de recuperación (RTO) rápido. Ahora puede [hacer una copia de seguridad de las bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db) mediante [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview).

Azure Backup tiene [certificación Backint](https://www.sap.com/dmc/exp/2013_09_adpd/enEN/#/d/solutions?id=8f3fd455-a2d7-4086-aa28-51d8870acaa5) de SAP, para proporcionar compatibilidad con copias de seguridad nativas aprovechando las API nativas de SAP HANA. Esta oferta de Azure Backup se alinea con el mantra de copias de seguridad de **infraestructura cero**, lo que elimina la necesidad de implementar y administrar la infraestructura de copia de seguridad. Ahora puede realizar copias de seguridad de bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure (ya se admiten las [de la serie M](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)) y restaurarlas, y aprovechar las funciones de administración empresarial que proporciona Azure Backup.

## <a name="added-value"></a>Valor agregado

El uso de Azure Backup para realizar copias de seguridad de bases de datos de SAP HANA y restaurarlas, ofrece las ventajas siguientes:

* **Objetivo de punto de recuperación (RPO) de 15 minutos**: ahora se pueden recuperar datos críticos hasta un máximo de 15 minutos.
* **Restauraciones a un momento dado con un clic**: se ha facilitado la restauración de los datos de producción en servidores HANA alternativos. Azure administra en segundo plano el encadenamiento de copias de seguridad y catálogos para realizar restauraciones.
* **Retención a largo plazo**: para satisfacer las necesidades rigurosas de cumplimiento y auditoría. Conserve las copias de seguridad durante años, en función de la duración de la retención, más allá de la cual los puntos de recuperación se eliminarán de forma automática mediante la función integrada de administración del ciclo de vida.
* **Administración de copias de seguridad desde Azure**: use las funciones de administración y supervisión de Azure Backup para mejorar la experiencia de administración. También se admite la CLI de Azure.

Para ver los escenarios de copia de seguridad y restauración que se admiten en la actualidad, consulte la [matriz de compatibilidad de escenarios de SAP HANA](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support).

## <a name="backup-architecture"></a>Arquitectura de copia de seguridad

![Diagrama de la arquitectura de copia de seguridad](./media/sap-hana-db-about/backup-architecture.png)

* El proceso de copia de seguridad comienza por la [creación de un almacén de Recovery Services](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#create-a-recovery-service-vault) en Azure. Este almacén se usará para almacenar las copias de seguridad y los puntos de recuperación creados con el tiempo.
* La máquina virtual de Azure que ejecuta el servidor SAP HANA se registra con el almacén y se [detectan](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#discover-the-databases) las bases de datos de las que se va a realizar la copia de seguridad. A fin de habilitar el servicio Azure Backup para que detecte las bases de datos, se debe ejecutar un [script de preregistro](https://aka.ms/scriptforpermsonhana) en el servidor HANA como usuario raíz.
* Este script crea el usuario de base de datos **AZUREWLBACKUPHANAUSER** y una clave correspondiente con el mismo nombre en **hdbuserstore**. Consulte la [sección Configuración de permisos](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#setting-up-permissions) para obtener más información sobre lo que hace el script.
* Ahora el servicio Azure Backup instala el **complemento de Azure Backup para HANA** en el servidor SAP HANA registrado.
* El **complemento de Azure Backup para HANA** utiliza el usuario de base de datos **AZUREWLBACKUPHANAUSER** creado por el script de preregistro para realizar todas las operaciones de copia de seguridad y restauración. Si intenta configurar la copia de seguridad de las bases de datos de SAP HANA sin ejecutar este script, es posible que reciba el error siguiente: **UserErrorHanaScriptNotRun**.
* Para [configurar la copia de seguridad](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#configure-backup) en las bases de datos que se han detectado, elija la directiva de copia de seguridad necesaria y habilite las copias de seguridad.

* Una vez que se haya configurado la copia de seguridad, el servicio Azure Backup configura los parámetros de Backint siguientes en el nivel de base de datos del servidor de SAP HANA protegido:
  * [catalog_backup_using_backint:true]
  * [enable_accumulated_catalog_backup:false]
  * [parallel_data_backup_backint_channels:1]
  * [log_backup_timeout_s:900)]
  * [backint_response_timeout:7200]

>[!NOTE]
>Asegúrese de que estos parámetros *no* estén presentes en el nivel de host. Los parámetros en el nivel de host invalidarán estos parámetros y pueden causar un comportamiento inesperado.
>

* El **complemento de Azure Backup para HANA** mantiene todas las programaciones de copia de seguridad y los detalles de la directiva. Desencadena las copias de seguridad programadas y se comunica con el **motor de copia de seguridad de HANA** a través de las API de Backint.
* El **motor de copia de seguridad de HANA** devuelve una secuencia de Backint con los datos de los que se va a realizar la copia de seguridad.
* Todas las copias de seguridad programadas y las copias de seguridad a petición (desencadenadas desde Azure Portal) que son completas o diferenciales se inician mediante el **complemento de Azure Backup para HANA**. Pero el propio **motor de copia de seguridad de HANA** administra y desencadena las copias de seguridad de registros.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [restaurar bases de datos de SAP HANA que se ejecutan en una máquina virtual de Azure](https://docs.microsoft.com/azure/backup/sap-hana-db-restore).
* Obtenga información acerca de cómo [administrar bases de datos de SAP HANA de las que se realizan copias de seguridad mediante Azure Backup](https://docs.microsoft.com/azure/backup/sap-hana-db-manage).
