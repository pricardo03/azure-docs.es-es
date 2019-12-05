---
title: Solución de problemas de errores de copia de seguridad de bases de datos de SAP HANA
description: En este artículo se describe cómo se solucionan los errores comunes que pueden producirse al usar Azure Backup para realizar copias de seguridad de bases de datos de SAP HANA.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: b4c39c631963a358dcdc9d1eafe954a85a9499ad
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2019
ms.locfileid: "74554854"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA en Azure

En este artículo se proporciona información para la solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA en Azure Virtual Machines. Para más información sobre los escenarios de copia de seguridad de SAP HANA actualmente compatibles, consulte [Compatibilidad con los escenarios](sap-hana-backup-support-matrix.md#scenario-support).

## <a name="prerequisites-and-permissions"></a>Requisitos previos y permisos

Consulte las secciones sobre los [requisitos previos](tutorial-backup-sap-hana-db.md#prerequisites) y los [permisos de configuración](tutorial-backup-sap-hana-db.md#setting-up-permissions) antes de configurar las cpias de seguridad.

## <a name="common-user-errors"></a>Errores de usuario comunes

###  <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection 

| Mensaje de error      | No se pudo conectar con el sistema HANA                             |
| ------------------ | ------------------------------------------------------------ |
| Causas posibles    | La instancia de SAP HANA puede estar fuera de servicio.<br/>No se establecen los permisos necesarios para que la copia de seguridad de Azure interactúe con la base de datos de HANA. |
| Acción recomendada | Compruebe si la base de datos de SAP HANA está activa. Si la base de datos está en funcionamiento, compruebe si se establecieron todos los permisos necesarios. Si falta alguno de los permisos, ejecute el [script de registro previo](https://aka.ms/scriptforpermsonhana) para agregar los permisos que faltan. |

###  <a name="usererrorhanainstancenameinvalid"></a>UserErrorHanaInstanceNameInvalid 

| Mensaje de error      | La instancia de SAP HANA especificada no es válida ni se puede encontrar. |
| ------------------ | ------------------------------------------------------------ |
| Causas posibles    | No se puede realizar una copia de seguridad de varias instancias de SAP HANA en una máquina virtual de Azure única. |
| Acción recomendada | Ejecute el [script de registro previo](https://aka.ms/scriptforpermsonhana) en la instancia de SAP HANA de la que quiere hacer una copia de seguridad. Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft. |

###  <a name="usererrorhanaunsupportedoperation"></a>UserErrorHanaUnsupportedOperation 

| Mensaje de error      | No se admite la operación de SAP HANA especificada.             |
| ------------------ | ------------------------------------------------------------ |
| Causas posibles    | La copia de seguridad de Azure para SAP HANA no es compatible con la copia de seguridad incremental ni con las acciones realizadas en clientes nativos de SAP HANA (Studio/Cockpit/DBA Cockpit). |
| Acción recomendada | Para más información, consulte [aquí](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support). |

###  <a name="usererrorhanapodoesnotsupportbackuptype"></a>UserErrorHANAPODoesNotSupportBackupType 

| Mensaje de error      | Esta base de datos de SAP HANA no admite el tipo de copia de seguridad solicitada. |
| ------------------ | ------------------------------------------------------------ |
| Causas posibles    | La copia de seguridad de Azure no es compatible con la copia de seguridad incremental ni con la copia de seguridad mediante instantáneas. |
| Acción recomendada | Para más información, consulte [aquí](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support). |

###  <a name="usererrorhanalsnvalidationfailure"></a>UserErrorHANALSNValidationFailure 

| Mensaje de error      | La cadena de registros de copia de seguridad se interrumpió.                                   |
| ------------------ | ------------------------------------------------------------ |
| Causas posibles    | Es posible que el destino de la copia de seguridad de registros se haya actualizado desde BackInt al sistema de archivos o puede que el ejecutable de BackInt se haya modificado. |
| Acción recomendada | Desencadene una copia de seguridad completa para resolver el problema.                   |

###  <a name="usererrorincomaptiblesrctargetsystsemsforrestore"></a>UserErrorIncomaptibleSrcTargetSystsemsForRestore 

| Mensaje de error      | Los sistemas de origen y destino para la restauración no son compatibles.   |
| ------------------ | ------------------------------------------------------------ |
| Causas posibles    | El sistema de destino para la restauración no es compatible con el origen. |
| Acción recomendada | Consulte la nota SAP [1642148](https://launchpad.support.sap.com/#/notes/1642148) para información sobre los tipos de restauración compatibles actualmente. |

###  <a name="usererrorsdctomdcupgradedetected"></a>UserErrorSDCtoMDCUpgradeDetected 

| Mensaje de error      | Actualización de SDC a MDC detectada.                                  |
| ------------------ | ------------------------------------------------------------ |
| Causas posibles    | La instancia de SAP HANA se actualizó de SDC a MDC. Las copias de seguridad presentarán un error después de la actualización. |
| Acción recomendada | Siga los pasos que aparecen en la sección [Actualización de SAP HANA 1.0 a 2.0](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#upgrading-from-sap-hana-10-to-20) para resolver el problema. |

###  <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration 

| Mensaje de error      | Configuración de BackInt no válida detectada.                       |
| ------------------ | ------------------------------------------------------------ |
| Causas posibles    | Los parámetros de BackInt están incorrectamente especificados para Azure Backup. |
| Acción recomendada | Compruebe si se establecieron estos parámetros (BackInt):<br/>\* [catalog_backup_using_backint:true]<br/>\* [enable_accumulated_catalog_backup:false]<br/>\* [parallel_data_backup_backint_channels:1]<br/>\* [log_backup_timeout_s:900)]<br/>\* [backint_response_timeout:7200]<br/>Si los parámetros basados en BackInt están presentes en el host, quítelos. Si los parámetros no están presentes en el nivel del host, pero se han modificado manualmente en el nivel de la base de datos, reviértalos a los valores apropiados como se describió anteriormente. O bien, ejecute [Detener la protección y conservar los datos de copia de seguridad](https://docs.microsoft.com/azure/backup/sap-hana-db-manage#stop-protection-for-an-sap-hana-database) en Azure Portal y, después, seleccione **Reanudar copia de seguridad**. |

## <a name="restore-checks"></a>Comprobaciones de restauración

### <a name="single-container-database-sdc-restore"></a>Restauración de una base de datos de contenedor único (SDC)

Ocúpese de las entradas mientras se realiza la restauración de una base de datos de contenedor único (SDC) para HANA a otra máquina de SDC. El nombre de la base de datos debe estar en minúscula con "sdc" anexado entre corchetes. La instancia de HANA se mostrará en mayúsculas.

Supongamos que hay una copia de seguridad de una instancia "H21" de HANA de SDC. En la página de elementos de copia de seguridad aparecerá el nombre del elemento de copia de seguridad como **"h21(sdc)"** . Si intenta restaurar esta base de datos en otra SDC de destino, por ejemplo, H11, es necesario proporcionar las entradas siguientes.

![Entradas de restauración de SDC](media/backup-azure-sap-hana-database/hana-sdc-restore.png)

Tenga en cuenta los siguientes puntos:

- De manera predeterminada, el nombre de la base de datos restaurada se rellenará con el nombre del elemento de copia de seguridad, es decir, h21(sdc).
- Si selecciona el destino como H11, el nombre de la base de datos restaurada NO CAMBIARÁ de manera automática. **Se debe editar para cambiarlo a h11(sdc)** . Con respecto a SDC, el nombre de la base de datos restaurada será el identificador de la instancia de destino en minúsculas más "sdc" entre corchetes.
- Como SDC solo puede tener una base de datos única, también debe hacer clic en la casilla para permitir reemplazar los datos existentes de la base de datos por los datos del punto de recuperación.
- Linux distingue mayúsculas de minúsculas. Por lo tanto, tiene que tener cuidado y respetar su uso.

### <a name="multiple-container-database-mdc-restore"></a>Restauración de base de datos de varios contenedores (MDC)

En las bases de datos de varios contenedores para HANA, la configuración estándar es SYSTEMDB + 1 o más bases de datos de inquilino. Restaurar toda una instancia de SAP HANA significa restaurar tanto SYSTEMDB como las bases de datos de inquilino. Primero se restaura SYSTEMDB y, luego, la base de datos de inquilino. Básicamente, restaurar la base de datos del sistema significa reemplazar la información del sistema del destino seleccionado. Esta restauración también reemplaza la información relacionada con BackInt en la instancia de destino. Por lo tanto, una vez que la base de datos del sistema se restaura en una instancia de destino, es necesario volver a ejecutar el script de registro previo. Solo entonces las restauraciones de bases de datos de inquilino subsiguientes se realizarán correctamente.

## <a name="upgrading-from-sap-hana-10-to-20"></a>Actualización de SAP HANA 1.0 a 2.0

Si va a proteger bases de datos SAP HANA 1.0 y quiere actualizarlas a la versión 2.0, siga los pasos que se describen a continuación:

- [Detenga la protección](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) con la conservación de datos para la base de datos SDC antigua.
- Vuelva a ejecutar el [script de registro previo](https://aka.ms/scriptforpermsonhana) con los detalles correctos de (sid y mdc).
- Vuelva a registrar la extensión (Backup [Copia de seguridad] -> View details [Ver detalles] -> Select the relevant Azure VM [Seleccionar la máquina virtual de Azure pertinente] -> Re-register [Volver a registrar]).
- Haga clic en Rediscover DBs (Volver a detectar bases de datos) para la misma máquina virtual. Esta acción debería mostrar las nuevas bases de datos en el paso 2 con los detalles correctos (SYSTEMDB y base de datos de inquilino, no SDC).
- Proteja estas nuevas bases de datos.

## <a name="upgrading-without-an-sid-change"></a>Actualización sin un cambio de identificador de seguridad

Las actualizaciones del sistema operativo o SAP HANA que no causan un cambio de SID se pueden controlar tal como se describe a continuación:

- [Detenga la protección](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) con la conservación de datos para la base de datos.
- Vuelva a ejecutar el [script de registro previo](https://aka.ms/scriptforpermsonhana).
- [Reanude la protección](sap-hana-db-manage.md#resume-protection-for-an-sap-hana-database) de la base de datos nuevamente.

## <a name="next-steps"></a>Pasos siguientes

- Revise las [preguntas más frecuentes](https://docs.microsoft.com/azure/backup/sap-hana-faq-backup-azure-vm) sobre cómo hacer copias de seguridad de las bases de datos SAP HANA en VM de Azure.
