---
title: Matriz de compatibilidad de SAP HANA
description: En este artículo, obtendrá información sobre los escenarios admitidos y las limitaciones al usar Azure Backup para realizar copias de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: 82d844385290ab0dc2953537c1f9a3387dd7b2b2
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842638"
---
# <a name="support-matrix-for-backup-of-sap-hana-databases-on-azure-vms"></a>Matriz de compatibilidad de la copia de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure

Azure Backup admite la realización de copias de seguridad de bases de datos de SAP HANA en Azure. En este artículo, se resumen los escenarios admitidos y las limitaciones presentes al usar Azure Backup para realizar copias de seguridad de bases de datos de SAP HANA en máquinas virtuales de Azure.

## <a name="onboard-to-the-public-preview"></a>Incorporación a la versión preliminar pública

Incorpórese a la versión preliminar pública de la siguiente manera:

* En el portal, registre el identificador de suscripción en el proveedor de servicios de Recovery Services [siguiendo este artículo](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors#solution-3---azure-portal).
* Para PowerShell, ejecute este cmdlet. Debería completarse como "Registrado".

```PowerShell
Register-AzProviderFeature -FeatureName "HanaBackup" –ProviderNamespace Microsoft.RecoveryServices
```

> [!NOTE]
> La frecuencia de la copia de seguridad de registros ahora puede establecerse en un mínimo de 15 minutos. Las copias de seguridad de los registros comienzan el flujo solo después de que se haya realizado correctamente una copia de seguridad completa de la base de datos.

## <a name="scenario-support"></a>Compatibilidad con los escenarios

| **Escenario**               | **Configuraciones admitidas**                                | **Configuraciones no admitidas**                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Topología**               | SAP HANA que solo se ejecuta en máquinas virtuales Linux de Azure                    | Instancias grandes de HANA (HLI)                                   |
| **Zonas geográficas**                   | **DISPONIBILIDAD GENERAL:**<br />**Europa**: Europa Occidental, Europa del Norte, Centro de Francia, Sur de Reino Unido, Oeste de Reino Unido, Norte de Alemania, Centro-oeste de Alemania, Norte de Suiza, Oeste de Suiza<br />**Asia Pacífico**: Centro de Australia, Centro de Australia 2, Este de Australia, Sudeste de Australia, Japón Oriental, Japón Occidental, Centro de Corea del Sur, Corea del Sur<br /><br>**Versión preliminar:**<br />**Américas**: Centro de EE. UU., Este de EE. UU. 2, Este de EE. UU., Centro-norte de EE. UU., Centro-sur de EE. UU., Oeste de EE. UU. 2, Centro-oeste de EE. UU., Oeste de EE. UU., Centro de Canadá, Este de Canadá <br />**Asia Pacífico**: Asia Oriental, Sudeste Asiático, Centro de la India, India del Sur | Este de China, Norte de China, Este de China 2, Norte de China 2, Oeste de la India, Centro del Norte de Suiza, Norte de Sudáfrica, Oeste de Sudáfrica, Norte de Emiratos Árabes Unidos, Centro de Emiratos Árabes Unidos, regiones de Azure Government, Sur de Francia, Sur de Brasil |
| **Versiones del SO**            | SLES 12 con SP2, SP3 o SP4                                | SLES 15, RHEL                                                |
| **Versiones de HANA**          | SDC en HANA 1.x, MDC en HANA 2.x <= SPS04 Rev 44            | -                                                            |
| **Implementaciones de HANA**       | SAP HANA en una sola máquina virtual de Azure: solo escalado vertical               | Escalado horizontal                                                    |
| **Instancias de HANA**         | Una sola instancia de SAP HANA en una sola máquina virtual de Azure: solo escalado vertical | Varias instancias de SAP HANA en una sola máquina virtual                  |
| **Tipos de base de datos HANA**    | Contenedor de base de datos única (SDC) en la versión 1.x, contenedor de varias bases de datos (MDC) en la versión 2.x | MDC en HANA 1.x                                              |
| **Tamaño de base de datos de HANA**     | Tamaño de copia de seguridad completa de 2 TB según lo indicado por HANA                   |                                                              |
| **Tipos de copia de seguridad**           | Copias de seguridad completas, diferenciales y de registros                          | Incrementales, instantáneas                                       |
| **Tipos de restauración**          | Consulte la nota SAP HANA [1642148](https://launchpad.support.sap.com/#/notes/1642148) para obtener información sobre los tipos de restauración admitidos |                                                              |
| **Límites de copia de seguridad**          | Copias de seguridad completas de hasta 2 TB por instancia de SAP HANA         |                                                              |
| **Configuraciones especiales** |                                                              | SAP HANA y Dynamic Tiering <br>  Clonación a través de LaMa        |

------

> [!NOTE]
> Actualmente no se admiten las operaciones de copia de seguridad y restauración de los clientes nativos de SAP HANA (SAP HANA Studio/Cockpit/DBA Cockpit).



## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [hacer copias de seguridad de bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database).
* Obtenga información sobre cómo [restaurar bases de datos de SAP HANA que se ejecutan en máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/sap-hana-db-restore).
* Obtenga información acerca de cómo [administrar bases de datos de SAP HANA de las que se realizan copias de seguridad mediante Azure Backup](sap-hana-db-manage.md).
* Obtenga información acerca de cómo [solucionar problemas comunes al realizar copias de seguridad de bases de datos de SAP HANA](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot).
