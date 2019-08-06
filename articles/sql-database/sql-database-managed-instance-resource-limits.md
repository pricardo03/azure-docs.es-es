---
title: 'Límites de recursos de Azure SQL Database: instancia administrada | Microsoft Docs'
description: En este artículo se proporciona información general de los límites de recursos de Azure SQL Database para las instancias administradas.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop, sachinp, sstein
ms.date: 06/26/2019
ms.openlocfilehash: 961c0de6856a9791f76be1ec609f176f1fbbf8fb
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567329"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Introducción a los límites de recursos de instancia administrada de Azure SQL Database

En este artículo se proporciona una introducción a los límites de recursos de instancia administrada de Azure SQL Database e información sobre cómo solicitar aumentar estos límites.

> [!NOTE]
> Para conocer las diferencias en las características e instrucciones T-SQL admitidas, consulte las instrucciones [Diferencias entre las características](sql-database-features.md) y [Compatibilidad con instrucciones T-SQL](sql-database-managed-instance-transact-sql-information.md).

## <a name="instance-level-resource-limits"></a>Límites de recursos a nivel de instancia

Instancia administrada tiene características y límites de recursos que dependen de la infraestructura y la arquitectura subyacentes. Los límites dependen de la generación de hardware y del nivel de servicio.

### <a name="hardware-generation-characteristics"></a>Características de la generación de hardware

Instancia administrada de Azure SQL Database puede implementarse en dos generaciones de hardware: Gen4 y Gen5. Las generaciones de hardware tienen diferentes características, que se describen en la tabla siguiente:

|   | **Gen4** | **Gen5** |
| --- | --- | --- |
| Hardware | Procesadores Intel E5-2673 v3 (Haswell) de 2,4 GHz; núcleo virtual SSD conectado equivalente a 1 PP (núcleo físico) | Procesadores Intel E5-2673 v4 (Broadwell) de 2,3 GHz; SSD NVMe rápido, núcleo virtual equivalente a 1 LP (Hyper-Threading) |
| Número de núcleos virtuales | 8, 16, 24 núcleos virtuales | 4, 8, 16, 24, 32, 40, 64 y 80 núcleos virtuales |
| Memoria máxima (relación memoria/núcleo) | 7 GB por núcleo virtual<br/>Agregue más núcleos virtuales para obtener más memoria. | 5,1 GB por núcleo virtual<br/>Agregue más núcleos virtuales para obtener más memoria. |
| Memoria máxima de OLTP en memoria | Límite de instancia: 3 GB por núcleo virtual<br/>Límites de la base de datos:<br/> - 8 núcleos: 8 GB por base de datos<br/> - 16 núcleos: 20 GB por base de datos<br/> - 24 núcleos: 36 GB por base de datos | Límite de instancia: 2,5 GB por núcleo virtual<br/>Límites de la base de datos:<br/> - 8 núcleos: 13 GB por base de datos<br/> - 16 núcleos: 32 GB por base de datos |
| Número máximo de almacenamiento reservado de instancia |  Uso general: 8 TB<br/>Crítico para la empresa: 1 TB | Uso general: 8 TB<br/> Crítico para la empresa: 1 TB, 2 TB o 4 TB, en función del número de núcleos |

> [!IMPORTANT]
> Las nuevas bases de datos de Gen4 ya no se admiten en la región Este de Australia.

### <a name="service-tier-characteristics"></a>Características del nivel de servicios

Instancia administrada tiene dos niveles de servicio: De uso general y Crítico para la empresa. Estos niveles proporcionan funcionalidades diferentes, como se describe en la tabla siguiente:

| **Característica** | **Uso general** | **Crítico para la empresa** |
| --- | --- | --- |
| Número de núcleos virtuales\* | Gen4: 8, 16, 24<br/>Gen5: 4, 8, 16, 24, 32, 40, 64 y 80 | Gen4: 8, 16, 24, 32 <br/> Gen5: 4, 8, 16, 24, 32, 40, 64 y 80 |
| Memoria máxima | Gen4: 56 GB - 168 GB (7 GB/núcleo virtual)<br/>Gen5: 40,8 GB - 408 GB (5,1GB/núcleo virtual)<br/>Agregue más núcleos virtuales para obtener más memoria. | Gen4: 56 GB - 168 GB (7 GB/núcleo virtual)<br/>Gen5: 40,8 GB - 408 GB (5,1GB/núcleo virtual)<br/>Agregue más núcleos virtuales para obtener más memoria. |
| Tamaño máximo de almacenamiento reservado de instancia | - 2 TB para 4 núcleos virtuales (solo para Gen5)<br/>- 8 TB para otros tamaños | Gen4: 1 TB <br/> Gen5: <br/>- 1 TB para 4, 8 y 16 núcleos virtuales<br/>- 2 TB para 24 núcleos virtuales<br/>- 4 TB para 32, 40, 64 y 80 núcleos virtuales |
| Tamaño máximo de base de datos | Determinado por el tamaño de almacenamiento máximo por instancia | Determinado por el tamaño de almacenamiento máximo por instancia |
| Número máximo de bases de datos por instancia | 100 | 100 |
| Número máximo de archivos de base de datos por instancia | Hasta 280 | Hasta 32 767 archivos por base de datos |
| Datos/IOPS de registro (aproximado) | 500 - 7500 por archivo<br/>\*[Aumentar el tamaño del archivo para obtener más IOPS](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)| 11 K - 110 K (1375/núcleo virtual)<br/>Agregue más núcleos virtuales para obtener un mejor rendimiento de E/S. |
| Límite de rendimiento de escritura en el registro | 3 MB/s por núcleo virtual<br/>22 MB/s por instancia como máximo | 4 MB/s por núcleo virtual<br/>48 MB/s por instancia como máximo|
| Rendimiento de datos (aproximado) | 100 - 250 MB/s por archivo<br/>\*[Aumentar el tamaño del archivo para mejorar el rendimiento de E/S](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes) | N/D |
| Latencia de E/S de almacenamiento (aproximada) | 5-10 ms | 1-2 ms |
| Tamaño máximo de tempDB | 192 GB - 1920 GB (24 GB por núcleo virtual)<br/>Agregue más núcleos virtuales para obtener más espacio para TempDB. | El límite viene dado por el tamaño máximo de almacenamiento de la instancia. El tamaño del archivo de registro de TempDB está limitado actualmente a 24 GB/núcleo virtual. |
| Número máximo de sesiones | 30000 | 30000 |

> [!NOTE]
> - Tanto los datos como el tamaño de archivo de registro en las bases de datos del usuario y las del sistema se incluyen en el tamaño de almacenamiento de la instancia que se compara con el límite de tamaño de almacenamiento máximo. Utilice la vista del sistema <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys.master_files</a> para determinar el espacio total utilizado por las bases de datos. Los registros de errores no se mantienen y no se incluyen en el tamaño. Las copias de seguridad no se incluyen en el tamaño del almacenamiento.
> - El rendimiento e IOPS también dependen del tamaño de página que no está limitado explícitamente por instancia administrada.

## <a name="supported-regions"></a>Regiones admitidas

Las instancias administradas solo se pueden crear en las [regiones admitidas](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Para crear una instancia administrada en una región que no se admita actualmente, puede [enviar una solicitud de soporte técnico a través de Azure Portal](#obtaining-a-larger-quota-for-sql-managed-instance).

## <a name="supported-subscription-types"></a>Tipos de suscripciones admitidos

Actualmente, instancia administrada admite la implementación solo en los siguientes tipos de suscripciones:

- [Contrato Enterprise (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Pago por uso](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Proveedor de nube (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)
- [Desarrollo/pruebas - Enterprise](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Desarrollo/pruebas - Pago por uso](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Suscripciones con crédito mensual de Azure para suscriptores de Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)

## <a name="regional-resource-limitations"></a>Limitaciones de recursos regionales

Los tipos de suscripción compatibles pueden contener un número limitado de recursos por región. Instancia administrada tiene dos límites predeterminados por región de Azure en función del tipo de suscripción:

- **Límite de subred**: el número máximo de subredes en que se implementan instancias administradas en una sola región.
- **Límite de núcleos virtuales**: el número máximo de núcleos virtuales que se pueden implementar en todas las instancias en una sola región.

> [!Note]
> Estos límites son opciones de configuración predeterminadas y no limitaciones técnicas. Los límites se pueden aumentar a petición mediante la creación de una [solicitud de soporte técnico especial en Azure Portal](#obtaining-a-larger-quota-for-sql-managed-instance) si necesita más instancias administradas en la región actual. Como alternativa, puede crear nuevas instancias administradas en otra región de Azure sin necesidad de enviar solicitudes de soporte técnico.

En la tabla siguiente se muestran los límites regionales predeterminados para las suscripciones admitidas:

|Tipo de suscripción| Número máximo de subredes de instancia administrada | Número máximo de unidades de núcleo virtual* |
| :---| :--- | :--- |
|Pago por uso|3|320|
|CSP |8 (15 en algunas regiones**)|960 (1440 en algunas regiones**)|
|Desarrollo/pruebas - Pago por uso|3|320|
|Desarrollo/pruebas - Enterprise|3|320|
|EA|8 (15 en algunas regiones**)|960 (1440 en algunas regiones**)|
|Visual Studio Enterprise|2 |64|
|Visual Studio Professional y plataformas de MSDN|2|32|

\* Al planear las implementaciones, tenga en cuenta que un núcleo virtual Crítico para la empresa (BC) (debido a la redundancia agregada) consume 4 veces más capacidad que un núcleo virtual De uso general (GP). Por tanto, para los cálculos, 1 núcleo virtual GP = 1 unidad de núcleo virtual y 1 núcleo virtual BC = 4 unidades de núcleo virtual. Para simplificar el análisis de consumo frente a los límites predeterminados, resuma las unidades de núcleo virtual de todas las subredes de la región en la que se implementan instancias administradas y compare los resultados con los límites de la unidad de instancia del tipo de suscripción. El límite **Número máximo de unidades de núcleo virtual** se aplica a cada suscripción en una región. No hay ningún límite por subredes individuales, salvo que la suma de todos los núcleos virtuales implementados en varias subredes debe ser inferior o igual al **número máximo de unidades de núcleo virtual**.

\*\* En las regiones siguientes hay más límites de subred y núcleo virtual: Este de Australia, Este de EE. UU., Este de EE. UU. 2, Norte de Europa, Centro-sur de EE. UU., Sudeste Asiático, Sur de Reino Unido, Oeste de Europa, Oeste de EE. UU. 2.

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>Obtención de una cuota mayor para instancia administrada de SQL

Si necesita más instancias administradas en sus regiones actuales, puede enviar una solicitud de soporte técnico para ampliar la cuota a través de Azure Portal.
Para iniciar el proceso de obtención de una cuota mayor:

1. Abra **Ayuda y soporte técnico** y haga clic en **Nueva solicitud de soporte técnico**.

   ![Ayuda y soporte técnico](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. En la pestaña Conceptos básicos de la nueva solicitud de soporte técnico:
   - En **Tipo de problema**, seleccione **Límites de servicio y suscripción (cuotas)** .
   - En **Suscripción**, seleccione la suscripción.
   - En **Tipo de cuota**, seleccione **Instancia administrada de SQL Database**.
   - En **Plan de soporte técnico**, seleccione un plan de soporte técnico.

     ![Cuota de tipo de problema](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. Haga clic en **Next**.
4. En la **pestaña Problema** de la nueva solicitud de soporte técnico:
   - En **Gravedad**, seleccione el nivel de gravedad del problema.
   - En **Detalles**, especifique información adicional acerca del problema, lo que incluye los mensajes de error.
   - En **Carga de archivos**, adjunte un archivo con más información (hasta 4 MB).

     ![Detalles del problema](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Una solicitud válida debe incluir:
     > - Región en la que hay que aumentar el límite de la suscripción.
     > - Número requerido de núcleos virtuales, por nivel de servicio en las subredes existentes después del aumento de la cuota (si cualquiera de las subredes existentes debe expandirse).
     > - Número requerido de nuevas subredes y número total de núcleos virtuales por nivel de servicio dentro de las nuevas subredes (si tiene que implementar instancias administradas en nuevas subredes).

5. Haga clic en **Next**.
6. En la pestaña Información de contacto de la nueva solicitud de soporte técnico, especifique el método de contacto preferido (teléfono o correo electrónico) y los detalles de contacto.
7. Haga clic en **Create**(Crear).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre instancia administrada, consulte [¿Qué es una instancia administrada?](sql-database-managed-instance.md)
- Para obtener información de precios, vea [Precios de Instancia administrada de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- Para obtener información sobre cómo crear su primera instancia administrada, consulte la [Guía de inicio rápido](sql-database-managed-instance-get-started.md).
