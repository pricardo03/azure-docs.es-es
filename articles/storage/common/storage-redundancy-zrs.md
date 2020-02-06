---
title: Creación de aplicaciones de alta disponibilidad con el almacenamiento con redundancia de zona (ZRS)
titleSuffix: Azure Storage
description: Almacenamiento con redundancia de zona (ZRS) ofrece un modo sencillo de crear aplicaciones de alta disponibilidad. ZRS protege frente a errores de hardware en el centro de datos y algunos desastres regionales.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 0e6b87ff34d6555fda50518198f9ae3839aa56e6
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719099"
---
# <a name="build-highly-available-applications-with-zone-redundant-storage-zrs"></a>Creación de aplicaciones de alta disponibilidad con el almacenamiento con redundancia de zona (ZRS)

[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-zrs.md)]

## <a name="support-coverage-and-regional-availability"></a>Cobertura de soporte técnico y disponibilidad regional

ZRS actualmente admite los tipos de cuenta de almacenamiento de uso general v2 estándar, FileStorage y BlockBlobStorage. Para más información acerca de los tipos de cuentas de almacenamiento, consulte la [Introducción a la cuenta de Azure Storage](storage-account-overview.md).

Las cuentas ZRS de uso general v2 admiten blobs en bloques, blobs en páginas que no son de disco, recursos compartidos estándar, tablas y colas.

Para las cuentas de uso general v2 estándar, ZRS está disponible con carácter general en las regiones siguientes:

- Sudeste de Asia
- Norte de Europa
- Oeste de Europa
- Centro de Francia
- Japón Oriental
- Norte de Sudáfrica
- Sur de Reino Unido 2
- Centro de EE. UU.
- Este de EE. UU.
- Este de EE. UU. 2
- Oeste de EE. UU. 2

En el caso de las cuentas de FileStorage (recursos compartidos premium) y de BlockBlobStorage (blobs en bloques premium), ZRS está disponible con carácter general en las regiones siguientes:

- Oeste de Europa
- Este de EE. UU.

Microsoft sigue habilitando ZRS en otras regiones de Azure. Revise periódicamente la página [Actualizaciones de servicios Azure](https://azure.microsoft.com/updates/) para información sobre las regiones nuevas.

**Limitaciones conocidas**

- El nivel de archivo no se admite actualmente en las cuentas de ZRS. Consulte [Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) para más información.
- Los discos administrados no admiten ZRS. Puede almacenar instantáneas e imágenes de discos administrados SSD estándar en almacenamiento HDD estándar y [elegir entre las opciones LRS y ZRS](https://azure.microsoft.com/pricing/details/managed-disks/).

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>¿Qué ocurre cuando una zona deja de estar disponible?

Los datos son accesibles para las operaciones de escritura y lectura incluso si una zona deja de estar disponible. Microsoft recomienda seguir procedimientos recomendados para el control de errores transitorios. Estos procedimientos incluyen la implementación de directivas de reintento con retroceso exponencial.

Cuando una zona no está disponible, Azure realiza actualizaciones de red, como el redireccionamiento de DNS. Estas actualizaciones pueden afectar a la aplicación si se accede a los datos antes de que se completen.

ZRS podría no proteger los datos frente a un desastre regional en el que varias zonas resulten afectadas permanentemente. En lugar de eso, ZRS ofrece resistencia de los datos si dejan de estar disponibles de manera temporal. Para la protección frente a desastres regionales, Microsoft recomienda usar el almacenamiento con redundancia geográfica (GRS). Para obtener más información acerca de GRS, vea [Almacenamiento con redundancia geográfica (GRS): replicación entre regiones para Azure Storage](storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Conversión a la replicación de ZRS

Migrar a o desde LRS, GRS y RA-GRS es sencillo. Use Azure Portal o la API del proveedor de recursos de almacenamiento para cambiar el tipo de redundancia de la cuenta. Azure, a continuación, replicará los datos según corresponda. 

Para migrar datos a ZRS se requiere una estrategia diferente. La migración de ZRS implica el movimiento físico de los datos desde un sello de almacenamiento único a varios sellos dentro de una región.

Hay dos opciones principales para la migración a ZRS: 

- Copie o mueva los datos de manera manual a una cuenta de ZRS nueva desde una cuenta existente.
- Solicite una migración en vivo.

> [!IMPORTANT]
> La migración en vivo no se admite actualmente para recursos compartidos de archivos Prémium. Actualmente solo se admite copiar o mover datos manualmente.

Si necesita que la migración se complete en una fecha determinada, considere la posibilidad de realizar una migración manual. Una migración manual proporciona más flexibilidad que una migración en vivo. Con una migración manual, puede controlar el tiempo.

Para realizar una migración manual, tiene diferentes opciones:
- Use las herramientas existentes como AzCopy, una de las bibliotecas cliente de Azure Storage o herramientas de terceros de confianza.
- Si está familiarizado con Hadoop o HDInsight, adjunte la cuenta de origen y destino (ZRS) al clúster. Después, realice un paralelismo del proceso de copia de datos con una herramienta como DistCp.
- Cree sus propias herramientas mediante una de las bibliotecas cliente de Azure Storage.

Una migración manual puede provocar tiempos de inactividad de la aplicación. Si la aplicación requiere alta disponibilidad, Microsoft también proporciona una opción de migración en vivo. Una migración en vivo es una migración en contexto sin tiempo de inactividad. 

Durante una migración en vivo, puede usar la cuenta de almacenamiento mientras se migran los datos entre las marcas de almacenamiento de origen y destino. Durante el proceso de migración, tiene el mismo nivel de Acuerdo de Nivel de Servicio de durabilidad y disponibilidad que habitualmente.

Tenga en cuenta las siguientes restricciones en la migración en vivo:

- Si bien Microsoft controla la solicitud de migración en vivo rápidamente, no hay ninguna garantía respecto de cuándo se completará una migración en vivo. Si necesita migrar los datos a ZRS en una fecha concreta, Microsoft recomienda que, en su lugar, realice una migración manual. En general, cuantos más datos tenga en su cuenta, más tiempo se tardará en migrar esos datos. 
- La migración en vivo solo se admite para las cuentas de almacenamiento que utilizan la replicación de LRS. Si su cuenta usa GRS o RA-GRS, debe cambiar primero el tipo de replicación a LRS antes de continuar. Este paso intermedio quita el punto de conexión secundario proporcionado por GRS/RA-GRS.
- La cuenta debe tener datos.
- Solo puede migrar datos en la misma región. Si quiere migrar los datos a una cuenta de ZRS ubicada en una región distinta de la región de la cuenta de origen, debe hacer una migración manual.
- Solo los tipos de cuenta de almacenamiento estándar admiten la migración en vivo. Las cuentas de Premium Storage deben migrarse manualmente.
- No se admite la migración en vivo de ZRS a LRS, GRS o RA-GRS. Deberá mover manualmente los datos a una cuenta de almacenamiento nueva o existente.
- Los discos administrados solo están disponible para LRS y no se pueden migrar a ZRS. Puede almacenar instantáneas e imágenes de discos administrados SSD estándar en almacenamiento HDD estándar y [elegir entre las opciones LRS y ZRS](https://azure.microsoft.com/pricing/details/managed-disks/). Para conseguir la integración con conjuntos de disponibilidad, consulte [Integración a los discos administrados de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#integration-with-availability-sets).
- Las cuentas LRS o GRS con datos de archivo no se pueden migrar a ZRS.

Puede solicitar la migración en vivo mediante el [Portal de soporte técnico de Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). En el portal, seleccione la cuenta de almacenamiento que quiere convertir a ZRS.
1. Seleccione **Nueva solicitud de soporte técnico**.
2. Complete los **aspectos básicos** según la información de su cuenta. En la sección **Servicio**, seleccione **Administración de cuentas de almacenamiento** y el recurso que desea convertir a ZRS. 
3. Seleccione **Next** (Siguiente). 
4. Especifique los siguientes valores en la sección **Problema**: 
    - **Gravedad**: deje el valor predeterminado tal cual.
    - **Tipo de problema**: seleccione **Migración de datos**.
    - **Categoría**: seleccione **Migración a ZRS**.
    - **Título**: escriba un título descriptivo, por ejemplo, **migración de cuentas de ZRS**.
    - **Detalles**: escriba información adicional en el cuadro **Detalles**; por ejemplo, me gustaría migrar a ZRS desde [LRS, GRS] en la región \_\_. 
5. Seleccione **Next** (Siguiente).
6. Compruebe que la información de contacto sea correcta en la hoja **Información de contacto**.
7. Seleccione **Crear**.

Un responsable de soporte técnico se pondrá en contacto con usted para proporcionarle la asistencia que necesite.

## <a name="live-migration-to-zrs-faq"></a>Preguntas frecuentes sobre la migración en vivo a ZRS

**¿Debo planear un tiempo de inactividad durante la migración?**

La migración no ocasiona tiempo de inactividad. Durante una migración en vivo, puede seguir usando su cuenta de almacenamiento mientras se migran los datos entre las marcas de almacenamiento de origen y destino. Durante el proceso de migración, tiene el mismo nivel de Acuerdo de Nivel de Servicio de durabilidad y disponibilidad que habitualmente.

**¿Hay pérdida de datos asociada con la migración?**

No hay pérdida de datos asociada con la migración. Durante el proceso de migración, tiene el mismo nivel de Acuerdo de Nivel de Servicio de durabilidad y disponibilidad que habitualmente.

**¿Se necesitan actualizaciones en la aplicación una vez finalizada la migración?**

Una vez finalizada la migración, el tipo de replicación de las cuentas cambia a "almacenamiento con redundancia de zona (ZRS)". Los puntos de conexión de servicio, las claves de acceso, la SAS y cualquier otra opción de configuración permanecen sin cambios.

**¿Se puede solicitar una migración en vivo de las cuentas de uso general v1 a ZRS?**

ZRS solo admite cuentas de uso general v2, por lo que antes de enviar una solicitud para una migración en vivo a ZRS, asegúrese de actualizar las cuentas a uso general v2. Consulte [Información general acerca de la cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-overview) y [Actualización a una cuenta de almacenamiento de uso general v2](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) para más información.

**¿Puedo solicitar una migración en vivo de mis cuentas de almacenamiento con redundancia geográfica o con redundancia geográfica de acceso de lectura (GRS/RA-GRS) a ZRS?**

La migración en vivo solo se admite para las cuentas de almacenamiento que utilizan la replicación de LRS. Si su cuenta usa GRS o RA-GRS, debe cambiar primero el tipo de replicación a LRS antes de continuar. Este paso intermedio quita el punto de conexión secundario proporcionado por GRS/RA-GRS. Además, antes de enviar una solicitud para una migración en vivo a ZRS, asegúrese de que las aplicaciones o cargas de trabajo ya no requieren acceso al punto de conexión de solo lectura secundario y cambie el tipo de replicación de las cuentas de almacenamiento a almacenamiento con redundancia local (LRS). Consulte [Cambio de estrategia de replicación](https://docs.microsoft.com/azure/storage/common/storage-redundancy#changing-replication-strategy) para más información.

**¿Se puede solicitar una migración en vivo de mis cuentas de almacenamiento a ZRS en otra región?**

Si quiere migrar los datos a una cuenta de ZRS ubicada en una región distinta de la región de la cuenta de origen, debe hacer una migración manual.

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS clásico: opción heredada para la redundancia de los blobs en bloques
> [!NOTE]
> Microsoft dejará de usar y migrará las cuentas de ZRS clásico el 31 de marzo de 2021. Se enviarán más detalles a los clientes de ZRS clásico antes de que estas cuentas dejen de usarse. 
>
> Una vez que ZRS esté [disponible con carácter general](#support-coverage-and-regional-availability) en una región, los clientes ya no podrán crear cuentas de ZRS clásico desde el portal en esa región. El uso de Microsoft PowerShell y la CLI de Azure para crear cuentas de ZRS clásico se admite como opción hasta que ZRS clásico esté en desuso.

ZRS clásico replica asincrónicamente datos en centros de datos dentro de una o dos regiones. Puede que los datos replicados no estén disponibles hasta que Microsoft inicie la conmutación por error al elemento secundario. Una cuenta de ZRS clásico no puede convertirse a o desde LRS, GRS o RA-GRS. Las cuentas de ZRS clásico no admiten ni las métricas ni el registro.

ZRS clásico solo está disponible para **blobs en bloques** de cuentas de almacenamiento de uso general V1 (GPv1). Para más información sobre las cuentas de almacenamiento, vea [Introducción a las cuentas de Azure Storage](storage-account-overview.md).

Para migrar manualmente datos de cuentas de ZRS desde o hacia una cuenta de LRS, ZRS clásico, GRS o RA-GRS, use alguna de las siguientes herramientas: AzCopy, Explorador de Azure Storage, Azure PowerShell o la CLI de Azure. También puede compilar su propia solución de migración con una de las bibliotecas cliente de Azure Storage.

También puede actualizar las cuentas de ZRS clásico a ZRS en el portal o usar Azure PowerShell o la CLI de Azure en las regiones donde ZRS está disponible. Para actualizar a ZRS en Azure Portal, navegue a la sección de **Configuración** de la cuenta y elija **Actualizar**:

![Actualización de ZRS clásico a ZRS en el portal](media/storage-redundancy-zrs/portal-zrs-classic-upgrade.png)

Para actualizar a ZRS mediante PowerShell, llame al siguiente comando:
```powershell
Set-AzStorageAccount -ResourceGroupName <resource_group> -AccountName <storage_account> -UpgradeToStorageV2
```

Para actualizar a ZRS mediante la CLI, llame al comando siguiente:
```cli
az storage account update -g <resource_group> -n <storage_account> --set kind=StorageV2
```

## <a name="see-also"></a>Consulte también
- [Replicación de Azure Storage](storage-redundancy.md)
- [Almacenamiento con redundancia local (LRS): redundancia de datos de bajo costo para Azure Storage](storage-redundancy-lrs.md)
- [Almacenamiento con redundancia geográfica (GRS): replicación entre regiones para Azure Storage](storage-redundancy-grs.md)
