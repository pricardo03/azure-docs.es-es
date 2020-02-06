---
title: Creación de aplicaciones de Azure Storage de alta disponibilidad en almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar) | Microsoft Docs
description: El almacenamiento con redundancia de zona geográfica (GZRS) combina la alta disponibilidad del almacenamiento con redundancia de zona (ZRS) con protección frente a las interrupciones regionales según lo proporcionado por el almacenamiento con redundancia geográfica (GRS). Los datos de una cuenta de almacenamiento de GZRS se replican en las zonas de disponibilidad de Azure en la región primaria y también en una región geográfica secundaria para la protección frente a desastres regionales.
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 08/13/2019
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: c102941920d2b8746a49be47ef4c5613fa0bc281
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719082"
---
# <a name="build-highly-available-azure-storage-applications-with-geo-zone-redundant-storage-gzrs-preview"></a>Creación de aplicaciones de Azure Storage de alta disponibilidad en almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar)

El almacenamiento con redundancia de zona geográfica (GZRS) (versión preliminar) combina la alta disponibilidad del [almacenamiento con redundancia de zona (ZRS)](storage-redundancy-zrs.md) con protección frente a las interrupciones regionales según lo proporcionado por el [almacenamiento con redundancia geográfica (GRS)](storage-redundancy-grs.md). Los datos de una cuenta de almacenamiento de GZRS se replican en las [zonas de disponibilidad de Azure](../../availability-zones/az-overview.md) en la región primaria y también en una región geográfica secundaria para la protección frente a desastres regionales. Cada región de Azure se empareja con otra región de la misma zona geográfica, que juntas forman un emparejamiento regional. Para obtener más información y ver las excepciones, consulte la [documentación](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

Con una cuenta de almacenamiento de GZRS, puede seguir leyendo y escribiendo datos si una zona de disponibilidad deja de estar disponible o es irrecuperable. Además, los datos se mantienen en caso de un apagón regional completo o un desastre del cual la región primaria no se puede recuperar. El almacenamiento con redundancia de zona geográfica (GZRS) está diseñado para proporcionar una durabilidad mínima del 99,99999999999999 % (16 nueves) de los objetos en un año determinado. GZRS también ofrece los mismos objetivos de escalabilidad que LRS, ZRS, GRS o RA-GRS. Opcionalmente, puede habilitar el acceso de lectura a los datos de la región secundaria con almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS) si es necesario que las aplicaciones puedan leer datos en caso de que se produzca un desastre en la región primaria.

Microsoft recomienda el uso de GZRS en aplicaciones que requieran coherencia, durabilidad, alta disponibilidad, rendimiento excelente y resistencia para la recuperación ante desastres. Para la seguridad adicional de acceso de lectura a la región secundaria en caso de un desastre regional, habilite RA-GZRS para la cuenta de almacenamiento.

## <a name="about-the-preview"></a>Acerca de la versión preliminar

Solo las cuentas de almacenamiento de uso general v2 son compatibles con GZRS y RA-GZRS. Para más información acerca de los tipos de cuentas de almacenamiento, consulte la [Introducción a la cuenta de Azure Storage](storage-account-overview.md). GZRS y RA-GZRS admiten blobs en bloques y blobs en páginas que no sean discos VHD, archivos, tablas y colas.

GZRS y RA-GZRS están disponibles actualmente en las versiones preliminares de las siguientes regiones:

- Sudeste de Asia
- Norte de Europa
- Oeste de Europa
- Japón Oriental
- Sur de Reino Unido 2
- Este de EE. UU.
- Este de EE. UU. 2
- Centro de EE. UU.

Microsoft sigue habilitando GZRS y RA-GZRS en otras regiones de Azure. Revise periódicamente la página [Actualizaciones de servicios Azure](https://azure.microsoft.com/updates/) para información sobre las regiones admitidas.

Para obtener información sobre los precios de la versión preliminar, consulte los precios de la versión preliminar de GZRS para [blobs](https://azure.microsoft.com/pricing/details/storage/blobs), [archivos](https://azure.microsoft.com/pricing/details/storage/files/), [colas](https://azure.microsoft.com/pricing/details/storage/queues/) y [tablas](https://azure.microsoft.com/pricing/details/storage/tables/).

> [!IMPORTANT]
> Microsoft recomienda no usar las características en vista previa (GB) para las cargas de trabajo de producción.

## <a name="how-gzrs-and-ra-gzrs-work"></a>Funcionamiento de GZRS y RA-GZRS

Cuando los datos se escriben en una cuenta de almacenamiento con GZRS o RA-GZRS habilitado, esos datos se replican primero de forma sincrónica en la región primaria en tres zonas de disponibilidad. Los datos se replican de forma asincrónica en una segunda región que se encuentra a cientos de kilómetros de distancia. Cuando los datos se escriben en la región secundaria, se replican de forma sincrónica tres veces dentro de esa región mediante el [almacenamiento con redundancia local (LRS)](storage-redundancy-lrs.md).

> [!IMPORTANT]
> La replicación asincrónica implica un retraso entre el momento en que se escriben los datos en la región primaria y cunado se replican en la región secundaria. En el caso de un desastre regional, los cambios que no se hayan replicado en la región secundaria pueden perderse si dichos datos no se pueden recuperar desde la región principal.

Al crear una cuenta de almacenamiento, especifique cómo se van a replicar los datos de esa cuenta, así como la región primaria de esa cuenta. La región secundaria emparejada de una cuenta con replicación geográfica se determina según la región primaria y no es posible cambiarla. Para información actualizada sobre las regiones que admite Azure, consulte [Continuidad empresarial y recuperación ante desastres (BCDR): regiones emparejadas de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions). Para más información sobre la creación de una cuenta de almacenamiento mediante GZRS o RA-GZRS, consulte [Crear una cuenta de almacenamiento](storage-account-create.md).

### <a name="use-ra-gzrs-for-high-availability"></a>Uso de RA-GZRS para lograr alta disponibilidad

Cuando se habilita RA-GZRS para la cuenta de almacenamiento, los datos se pueden leer desde el punto de conexión secundario y desde el punto de conexión principal de la cuenta de almacenamiento. El punto de conexión secundario anexa el sufijo *–secondary* al nombre de la cuenta. Por ejemplo, si el punto de conexión principal de Blob service es `myaccount.blob.core.windows.net`, el punto de conexión secundario es `myaccount-secondary.blob.core.windows.net`. Las claves de acceso de la cuenta de almacenamiento son iguales para los extremos principal y secundario.

Para aprovechar las ventajas de RA-GZRS en caso de una interrupción regional, debe diseñar la aplicación de antemano para controlar este escenario. La aplicación debe leer y escribir en el punto de conexión principal, pero cambie al uso del punto de conexión secundario en caso de que la región primaria deje de estar disponible. Para obtener instrucciones sobre cómo diseñar la alta disponibilidad con RA-GZRS, consulte [Diseño de aplicaciones de alta disponibilidad mediante RA-GZRS o RA-GRS](https://docs.microsoft.com/azure/storage/common/storage-designing-ha-apps-with-ragrs).

Dado que los datos se replican en la región secundaria de forma asincrónica, la región secundaria suele estar detrás de la región primaria. Para determinar qué operaciones de escritura se han replicado en la región secundaria, la aplicación comprueba la hora de la última sincronización de la cuenta de almacenamiento. Todas las operaciones de escritura escritas en la región primaria antes de la hora de la última sincronización se han replicado correctamente en la región secundaria, lo que significa que están disponibles para leerse desde la región secundaria. Las operaciones de escritura escritas en la región primaria después de la hora de la última sincronización puede que se hayan replicado o no en la región secundaria, lo que significa que es posible que no estén disponibles para las operaciones de lectura.

Puede consultar el valor de la propiedad **Hora de la última sincronización** mediante Azure PowerShell, la CLI de Azure o una de las bibliotecas cliente de Azure Storage. La propiedad **Hora de la última sincronización** es un valor de fecha y hora GMT.

Para obtener instrucciones adicionales sobre el rendimiento y la escalabilidad con RA-GZRS, consulte la [lista de comprobación de rendimiento y escalabilidad de Microsoft Azure Storage](storage-performance-checklist.md).

### <a name="availability-zone-outages"></a>Interrupciones en las zonas de disponibilidad

En caso de que se produzca un error que afecte a una zona de disponibilidad en la región primaria, las aplicaciones pueden seguir leyendo y escribiendo sin problemas en la cuenta de almacenamiento mediante las otras zonas de disponibilidad de esa región. Microsoft recomienda seguir procedimientos recomendados para el control de errores transitorios al usar GZRS o ZRS. Estos procedimientos incluyen la implementación de directivas de reintento con retroceso exponencial.

Cuando una zona de disponibilidad pasa a no estar disponible, Azure realiza actualizaciones de red, como el redireccionamiento de DNS. Estas actualizaciones pueden afectar a la aplicación si se accede a los datos antes de que se completen.

### <a name="regional-outages"></a>Interrupciones regionales

Si un error afecta a toda la región primaria, Microsoft intentará restaurar primero dicha región. Si no es posible la restauración, Microsoft conmutará por error a la región secundaria, de modo que dicha región se convierte en la nueva región primaria. Si la cuenta de almacenamiento tiene habilitado RA-GZRS, las aplicaciones diseñadas para este escenario pueden leer desde la región secundaria mientras esperan la conmutación por error. Si la cuenta de almacenamiento no tiene RA-GZRS habilitado, las aplicaciones no podrán leer desde la región secundaria hasta que se complete la conmutación por error.

> [!NOTE]
> GZRS y RA-GZRS se encuentran actualmente en versión preliminar solo en la región Este de EE. UU. La conmutación por error de la cuenta administrada por el cliente (versión preliminar) todavía no está disponible en el este de EE. UU. 2, por lo que los clientes no pueden administrar actualmente eventos de conmutación por error de cuenta con cuentas GZRS y RA-GZRS. Durante la versión preliminar, Microsoft administrará los eventos de conmutación por error que afecten a las cuentas GZRS y RA-GZRS.

Dado que los datos se replican en la región secundaria de forma asincrónica, un error que afecte a la región primaria puede producir la pérdida de datos si no se puede recuperar dicha región. El intervalo entre las escrituras más recientes en la región primaria y la última escritura en la región secundaria se conoce como objetivo de punto de recuperación (RPO). El RPO indica el punto en el tiempo al que se pueden recuperar los datos. Normalmente, Azure Storage tiene un RPO inferior a 15 minutos, aunque actualmente no hay ningún contrato de nivel de servicio sobre cuánto tiempo tarda la replicación de datos en la región secundaria.

El objetivo de tiempo de recuperación (RTO) es una medida que indica cuánto tiempo tarda en realizarse la conmutación por error y tener la cuenta de almacenamiento de nuevo en línea. Esta medida indica el tiempo necesario para que Azure pueda realizar la conmutación por error y cambiar las entradas DNS principales para que apunten a la ubicación secundaria.

## <a name="migrate-a-storage-account-to-gzrs-or-ra-gzrs"></a>Migración de una cuenta de almacenamiento a GZRS o RA-GZRS

Puede migrar cualquier cuenta de almacenamiento existente a GZRS o RA-GZRS. La migración de una cuenta ZRS existente a GZRS o RA-GZRS es sencilla, mientras que la migración desde una cuenta LRS, GRS o RA-GRS es más complicada. En las secciones siguientes se describe la migración en cada caso.

**Limitaciones conocidas**

- El nivel de archivo no se admite actualmente en las cuentas de (RA-)GZRS. Consulte [Azure Blob Storage: niveles de acceso frecuente, esporádico y de archivo](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers) para más información.
- Los discos administrados no admiten (RA-)GZRS. Puede almacenar instantáneas e imágenes de discos administrados SSD estándar en almacenamiento HDD estándar y [elegir entre las opciones LRS y ZRS](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="migrating-from-a-zrs-account"></a>Migración desde una cuenta ZRS

Para convertir una cuenta ZRS existente en RA-GZRS, use el cmdlet [Set-AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) para cambiar la SKU de la cuenta. Recuerde reemplazar los valores de marcador de posición por los propios:

```powershell
Set-AzStorageAccount -ResourceGroupName <resource-group> -AccountName <storage-account> -SkuName "Standard_RAGZRS"
```

### <a name="migrating-from-an-lrs-grs-or-ra-grs-account"></a>Migración desde una cuenta LRS, GRS o RA-GRS

Hay dos opciones para migrar a GZRS o RA-GZRS desde una cuenta LRS, GRS o RA-GRS:

- Puede copiar o mover los datos de manera manual a una cuenta GZRS o RA-GZRS nueva desde una cuenta existente.
- Puede solicitar una migración en vivo.

#### <a name="perform-a-manual-migration"></a>Realización de una migración manual

Si necesita que la migración se complete en una fecha determinada, considere la posibilidad de realizar una migración manual. Una migración manual proporciona más flexibilidad que una migración en vivo. Con una migración manual, puede controlar el tiempo.

Para migrar manualmente los datos de una cuenta existente a una cuenta GZRS o RA-GZRS, use una herramienta que pueda copiar datos de forma eficaz. Estos son algunos ejemplos:

- Use una utilidad como AzCopy o una herramienta de terceros de confianza. Para obtener información sobre AzCopy, consulte [Introducción a AzCopy](storage-use-azcopy-v10.md).
- Si está familiarizado con Hadoop o HDInsight, adjunte las cuentas de almacenamiento de origen y destino al clúster. Después, realice un paralelismo del proceso de copia de datos con una herramienta como DistCp.
- Cree sus propias herramientas mediante una de las bibliotecas cliente de Azure Storage.

#### <a name="perform-a-live-migration"></a>Realización de una migración en vivo

Una migración manual puede provocar tiempos de inactividad de la aplicación. Si la aplicación requiere alta disponibilidad, Microsoft también proporciona una opción de migración en vivo. Una migración en vivo es una migración en contexto sin tiempo de inactividad.

Durante una migración en vivo, puede usar la cuenta de almacenamiento mientras se migran los datos entre las cuentas de almacenamiento de origen y destino. Durante el proceso de migración en vivo, la cuenta sigue cumpliendo su contrato de nivel de servicio para la durabilidad y disponibilidad. No se produce ningún tiempo de inactividad ni pérdida de datos debido a la migración en vivo.

Solo las cuentas de uso general v2 admiten GZRS o RA-GZRS, por lo que antes de enviar una solicitud para una migración en vivo a GZRS o RA-GZRS, asegúrese de actualizar la cuenta a uso general v2. Consulte [Información general acerca de la cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-overview) y [Actualización a una cuenta de almacenamiento de uso general v2](https://docs.microsoft.com/azure/storage/common/storage-account-upgrade) para más información.

Una vez completada la migración, la configuración de replicación de la cuenta de almacenamiento se actualizará a **almacenamiento con redundancia de zona geográfica (GZRS)** o **almacenamiento con redundancia de zona geográfica con acceso de lectura (RA-GZRS)** . Los puntos de conexión de servicio, las firmas de acceso compartido, (SAS) y cualquier otra opción de configuración de la cuenta permanecen sin cambios.

Tenga en cuenta las siguientes restricciones en la migración en vivo:

- Si bien Microsoft controla la solicitud de migración en vivo rápidamente, no hay ninguna garantía respecto de cuándo se completará una migración en vivo. Si necesita migrar los datos a GZRS o RA-GZRS en una fecha dada, Microsoft recomienda que, en su lugar, realice una migración manual. En general, cuantos más datos tenga en su cuenta, más tiempo se tardará en migrar esos datos.
- La migración en vivo se admite solo en las cuentas de almacenamiento que utilizan la replicación GRS o RA-GRS. Si la cuenta usa LRS, debe cambiar primero el tipo de replicación a GRS o RA-GRS antes de continuar. Este paso intermedio agrega el punto de conexión secundario proporcionado por GRS/RA-GRS.
- La cuenta debe tener datos.
- Solo puede migrar datos en la misma región.
- Solo los tipos de cuenta de almacenamiento estándar admiten la migración en vivo. Las cuentas de Premium Storage deben migrarse manualmente.
- No se admite la migración en vivo de una cuenta GZRS o RA-GZRS a una cuenta LRS, GRS o RA-GRS. Deberá mover manualmente los datos a una cuenta de almacenamiento nueva o existente.
- Puede solicitar una migración en vivo de RA-GRS a RA-GZRS. Sin embargo, no se admite la migración de RA-GRS a GZRS. En este caso, debe solicitar una migración en vivo a RA-GZRS y, a continuación, convertir manualmente la cuenta de almacenamiento para usar GZRS.
- Los discos administrados solo admiten LRS y no se pueden migrar a GZRS ni RA-GZRS. Para conseguir la integración con conjuntos de disponibilidad, consulte [Integración a los discos administrados de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/managed-disks-overview#integration-with-availability-sets).
- Puede almacenar instantáneas e imágenes de discos administrados SSD estándar en almacenamiento HDD estándar y [elegir entre las opciones LRS, ZRS, GZRS y RA-GZRS](https://azure.microsoft.com/pricing/details/managed-disks/).
- Las cuentas que contienen recursos compartidos de archivos de gran tamaño no son compatibles con GZRS.

Para solicitar una migración en vivo, use [Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). En el portal, seleccione la cuenta de almacenamiento que va a migrar a GZRS o RA-GZRS y siga estas instrucciones:

1. Seleccione **Nueva solicitud de soporte técnico**.
2. Complete los **aspectos básicos** según la información de su cuenta. En la sección **Servicio**, seleccione **Administración de la cuenta de almacenamiento** y especifique la cuenta que se va a migrar.
3. Seleccione **Next** (Siguiente).
4. Especifique los siguientes valores en la sección **Problema**:
    - **Gravedad**: deje el valor predeterminado tal cual.
    - **Tipo de problema**: seleccione **Migración de datos**.
    - **Categoría**: seleccione **Migración a (RA-)GZRS dentro de una región**.
    - **Título**: escriba un título descriptivo; por ejemplo, **migración de cuentas (RA-)GZRS**.
    - **Detalles**: escriba información adicional en el cuadro **Detalles**; por ejemplo, "me gustaría migrar a GZRS desde [LRS, GRS] en la región \_\_". o "Me gustaría migrar a RA-GZRS desde [LRS, RA-GRS] en la región \_\_".
5. Seleccione **Next** (Siguiente).
6. Compruebe que la información de contacto sea correcta en la hoja **Información de contacto**.
7. Seleccione **Crear**.

Un representante de soporte técnico se pondrá en contacto con usted para proporcionarle ayuda.

## <a name="see-also"></a>Consulte también

- [Replicación de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy)
- [Almacenamiento con redundancia local (LRS): redundancia de datos de bajo costo para Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs)
- [Almacenamiento con redundancia de zona (ZRS): aplicaciones de Azure Storage de alta disponibilidad](https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs) 
- [Destinos de escalabilidad y rendimiento para cuentas de almacenamiento estándar](scalability-targets-standard-account.md)
