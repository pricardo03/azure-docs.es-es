---
title: 'Frecuente, esporádico y de archivo de los niveles de acceso para blobs: Azure Storage'
description: Frecuente, esporádico y de archivo de los niveles de acceso para las cuentas de almacenamiento de Azure.
services: storage
author: Xansky
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: mhopkins
ms.subservice: blobs
ms.openlocfilehash: 424ca1cccd1b82c26a801af3f85f41eb2991e2d7
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58370132"
---
# <a name="azure-blob-storage-hot-cool-and-archive-access-tiers"></a>Almacenamiento de blobs de Azure: frecuente, esporádico y niveles de acceso de archivo

Azure storage ofrece niveles de acceso diferentes, lo que permite almacenar los datos de objeto de blob de la manera más rentable. Los niveles de acceso disponibles incluyen:

- **Hot** : optimizada para almacenar datos que se accede con frecuencia.
- **Genial** : optimizada para almacenar los datos que se accede con poca frecuencia y se almacenan durante al menos 30 días.
- **Archivo** : optimizada para almacenar los datos que se accede con poca frecuencia y se almacenan durante al menos 180 días con requisitos de latencia flexible (del orden de horas).

Las siguientes consideraciones se aplican a los niveles de acceso diferentes:

- El nivel de acceso de archivo está disponible sólo en el nivel de blob y no en el nivel de cuenta de almacenamiento.
- En el nivel de acceso esporádico tolera una disponibilidad ligeramente inferior, pero aun así requiere gran durabilidad y características similares de rendimiento y tiempo para tener acceso a datos de acceso frecuente. Para datos de acceso esporádico, un contrato de nivel de servicio de disponibilidad ligeramente inferior (SLA) y acceso mayores costos en comparación con datos activos son aceptables ventajas y desventajas para disminuir los costos de almacenamiento.
- El almacenamiento de archivo se realiza sin conexión y ofrece los menores costos de almacenamiento, pero los mayores costos de acceso.
- Solo los niveles de acceso frecuente y esporádico pueden establecerse en el nivel de cuenta.
- Acceso frecuente, esporádico y archivo se puede establecer en el nivel de objeto.

Los datos almacenados en la nube están creciendo a un ritmo exponencial. Para administrar los costos de las crecientes necesidades de almacenamiento, resulta útil organizar los datos en función de atributos como frecuencia de acceso y el período de retención planeado para optimizar costos. Datos almacenados en la nube pueden ser diferentes en cuanto a cómo se genera, procesar y tener acceso a través de su ciclo de vida. A algunos datos se accede y se modifican activamente a lo largo de su duración. A algunos datos se accede con frecuencia al principio de su duración, mientras que el acceso cae drásticamente a medida que envejecen los datos. Algunos datos permanecen inactivos en la nube y rara vez, si alguna vez, tiene acceso a una vez que se almacena.

Cada uno de estos escenarios de acceso de datos se beneficia de un nivel de acceso diferente que está optimizado para un patrón de acceso concreto. Con acceso frecuente, esporádico y los niveles de acceso de archivo, direcciones de almacenamiento de blobs de Azure esta necesidad de niveles de acceso diferenciado con modelos de precios independientes.

## <a name="storage-accounts-that-support-tiering"></a>Cuentas de almacenamiento que admiten niveles

Sólo puede disponer los datos de almacenamiento de objetos a acceso frecuente, esporádico o archivo en Blob storage y de uso General v2 (GPv2) cuentas. Las cuentas de General Purpose v1 (GPv1) no admiten niveles. Sin embargo, puede convertir fácilmente cuentas de almacenamiento de GPv1 o de Blob existentes a cuentas de GPv2 mediante un proceso con un solo clic en el portal de Azure. GPv2 proporciona una nueva estructura de precios para blobs, archivos, colas y acceso a una variedad de otras nuevas características de almacenamiento. A partir de ahora, se ofrecerán algunas nuevas características y reducciones de precio solo en las cuentas de GPv2. Por lo tanto, debe evaluar el uso de cuentas de GPv2 pero utilícelas solo después de revisar el precio de todos los servicios. Algunas cargas de trabajo pueden ser más caras en GPv2 que en GPv1. Para más información, vea [Introducción a las cuentas de Azure Storage](../common/storage-account-overview.md).

BLOB storage y exponer las cuentas de GPv2 el **Access Tier** atributo en el nivel de cuenta. Este atributo permite especificar el nivel de acceso predeterminado como frecuente o esporádico para cualquier blob de la cuenta de almacenamiento que no tiene un nivel explícito establecido en el nivel de objeto. En el caso de objetos con el nivel establecido en el nivel de objeto, el nivel de cuenta no se aplica. El nivel de archivo se puede aplicar solo a nivel de objeto. Puede cambiar entre estos niveles de acceso en cualquier momento.

## <a name="premium-performance-block-blob-storage"></a>Almacenamiento de blobs de bloque de rendimiento Premium

Almacenamiento de blobs de bloque de rendimiento Premium hace que los datos de acceso frecuente disponible a través de hardware de alto rendimiento. Datos en este nivel de rendimiento se almacenan en unidades de estado sólido (SSD), que están optimizadas para una latencia baja y coherente. Las SSD ofrecen mayores tasas de transaccionales y el rendimiento en comparación con las unidades de disco duro tradicionales.

Almacenamiento de blobs de bloque de rendimiento Premium es ideal para cargas de trabajo que requieren tiempos de respuesta rápida y coherente. Es mejor para cargas de trabajo que realizan muchas transacciones pequeñas, como capturar los datos de telemetría, mensajería y transformación de datos. Datos que implica a los usuarios finales, como la edición de vídeo interactivo, contenido web estático y las transacciones en línea también son buenos candidatos.

Almacenamiento de blobs de bloque de rendimiento Premium está disponible solo mediante el tipo de cuenta de almacenamiento de blobs de bloque y no admite actualmente la organización en niveles para frecuente, esporádico o niveles de acceso de archivo.

## <a name="hot-access-tier"></a>Nivel de acceso frecuente

El nivel de acceso frecuente tiene mayores costos de almacenamiento de los niveles de acceso esporádico y archivo, pero menores costos de acceso. Escenarios de uso de ejemplo para el nivel de acceso frecuente se incluyen:

- Datos que están en uso o espera que se acceda (lean y se escriban) con frecuencia.
- Datos que se almacenan provisionalmente para el procesamiento y la eventual migración a la capa de acceso esporádico.

## <a name="cool-access-tier"></a>Nivel de acceso esporádico

El nivel de acceso esporádico tiene menores costos de almacenamiento y los costos de acceso mayores en comparación con el almacenamiento de acceso frecuente. Este nivel está diseñado para datos que van a permanecer en el nivel de acceso esporádico durante al menos 30 días. Escenarios de uso de ejemplo para el nivel de acceso esporádico se incluyen:

- Conjuntos de datos de copia de seguridad y recuperación ante desastres a corto plazo.
- Contenido multimedia antiguo que no se ve con frecuencia, pero que se espera que esté disponible de inmediato cuando se acceda a él.
- Conjuntos de datos grandes que se deben almacenar de forma rentable mientras se recopilan más datos para el procesamiento futuro. (*Por ejemplo,*, almacenamiento a largo plazo de datos científicos, datos de telemetría sin procesar de una instalación de fabricación)

## <a name="archive-access-tier"></a>Nivel de acceso de archivo

El nivel de acceso del archivo tiene el menor costo de almacenamiento y mayores costos de recuperación de datos en comparación con los niveles de acceso frecuente y esporádico. Este nivel está destinado a los datos que pueden tolerar varias horas de latencia de recuperación y que permanecerán en el nivel de archivo durante un mínimo de 180 días.

Mientras un blob está en almacenamiento de archivo, los datos de blob están sin conexión y no se puede leer copiado, sobrescribe o modificadas. No se puede tomar instantáneas de un blob en almacenamiento de archivo. Sin embargo, los metadatos de blob quedan en línea y se mantienen disponibles, lo que permite enumerar el blob y sus propiedades. Para los blobs en el archivo, las únicas operaciones válidas son GetBlobProperties, GetBlobMetadata, ListBlobs, SetBlobTier y DeleteBlob.

Escenarios de uso de ejemplo para el nivel de acceso de archivo incluyen:

- Copia de seguridad a largo plazo, copia de seguridad secundaria y conjuntos de datos de archivado
- Datos originales (sin procesar) que deben conservarse, incluso después de que se han procesado en un formato útil final. (*Por ejemplo,*, archivos multimedia sin procesar tras la transcodificación en otros formatos)
- Datos de cumplimiento y archivado que se deben almacenar durante un largo período de tiempo y a los que casi nunca se accede. (*Por ejemplo*, llamadas de grabaciones de cámaras de seguridad, radiografías o resonancias antiguas para sanitarios, grabaciones de audio y transcripciones de cliente para servicios financieros)

### <a name="blob-rehydration"></a>Rehidratación de blobs

Para leer datos de un almacenamiento de archivo, primero debe cambiar el nivel del blob a acceso frecuente o esporádico. Este proceso se conoce como rehidratación y puede tardar en completarse hasta 15 horas. Para un rendimiento óptimo, se recomienda el uso de tamaños de blob grandes. La rehidratación de varios blobs pequeños a la vez puede suponer tiempo adicional.

Durante la rehidratación, puede comprobar la propiedad de blob **Archive Status** para confirmar si el nivel ha cambiado. El estado indica "rehydrate-pending-to-hot" (rehidratación pendiente para acceso frecuente) o "rehydrate-pending-to-cool" (rehidratación pendiente para acceso esporádico), según el nivel de destino. Al finalizar, se quita la propiedad de blob archive status y la propiedad de blob **Access Tier** refleja el nuevo nivel de acceso frecuente o esporádico.  

## <a name="blob-level-tiering"></a>Almacenamiento por niveles de blob

El almacenamiento por niveles de blob permite cambiar el nivel de los datos en el nivel de objeto mediante una única operación denominada [Set Blob Tier](/rest/api/storageservices/set-blob-tier) (establecimiento de nivel de blob). Puede cambiar fácilmente el nivel de acceso de un blob entre el nivel de archivo, esporádico o frecuente a medida que cambien los patrones de uso, sin tener que mover los datos entre las cuentas. Todos los cambios de nivel se realizan de inmediato. Sin embargo, al rehidratar un blob de un nivel de acceso de archivo puede tardar varias horas.

La hora del último cambio de nivel de blob se expone a través de la propiedad de blob **access tier change time**. Si un blob está en el nivel de archivo, no puede sobrescribirse, por lo que no se permite la carga del mismo blob en este escenario. Puede sobrescribir un blob en un nivel de acceso frecuente o esporádico, en el que caso el nuevo blob hereda el nivel del blob que se ha sobrescrito.

Los blobs en todos los niveles de acceso tres pueden coexistir en la misma cuenta. Los blobs que no tienen un nivel asignado explícitamente infieren el nivel de la configuración del nivel de acceso de la cuenta. Si el nivel de acceso se infiere desde la cuenta, verá que la propiedad de blob **access tier inferred** se establece en "true", y que la propiedad de blob **access tier** coincide con el nivel de cuenta. En el portal de Azure, puede inferir el nivel de acceso se muestra la propiedad con el nivel de acceso de blob (por ejemplo, **activa (inferido)** o **esporádico (inferido)**).

> [!NOTE]
> El almacenamiento de archivo y el almacenamiento por niveles de blob solo admiten blobs en bloques. Tampoco se puede cambiar el nivel de un blob en bloques que tiene instantáneas.

> [!NOTE]
> Datos almacenados en una cuenta de almacenamiento de blobs de bloque no se pueden colocar actualmente para frecuente, esporádico o archivo con [Set Blob Tier](/rest/api/storageservices/set-blob-tier) o mediante la administración del ciclo de vida de almacenamiento de blobs de Azure.
> Para mover datos, en forma sincrónica debe copiar blobs de la cuenta de almacenamiento de blobs de bloque para el nivel de acceso frecuente en una cuenta diferente utilizando el [colocar bloque de la API de dirección URL](/rest/api/storageservices/put-block-from-url) o una versión de AzCopy que es compatible con esta API.
> *Put Block From URL* API copia datos de forma asíncrónica en el servidor, lo que significa que la llamada solo se completa una vez que todos los datos se han movido de la ubicación original del servidor a la ubicación de destino.

### <a name="blob-lifecycle-management"></a>Administración del ciclo de vida de blobs

La administración del ciclo de vida de Blob Storage (versión preliminar) ofrece una directiva enriquecida basada en reglas que se puede usar para realizar la transición de los datos al nivel de acceso mejorado y para hacer que los datos expiren cuando finalice su ciclo de vida. Para más información, consulte [Administración del ciclo de vida de Azure Blob Storage](storage-lifecycle-management-concepts.md).  

### <a name="blob-level-tiering-billing"></a>Facturación del almacenamiento por niveles de blob

Cuando un blob se mueve a un nivel de almacenamiento de acceso más esporádico (frecuente -> esporádico, frecuente -> archivo o esporádico -> archivo), la operación se factura como una operación de escritura del nivel de destino, donde se aplican los cargos de la operación de escritura (por 10 000) y de la escritura de datos (por GB) del nivel de destino.

Cuando se mueve un blob a un nivel menos esporádico (archivo -> esporádico, archivo -> frecuente o esporádico -> frecuente), la operación se factura como lectura desde el nivel de origen, donde se aplican la operación de lectura (por 10 000) y los cargos de recuperación (por GB) de datos de la capa de origen. La tabla siguiente resume cómo se facturan los cambios de nivel:

| | **Cargos de escritura (operación + acceso)** | **Cargos de lectura (operación + acceso)**
| ---- | ----- | ----- |
| **Dirección de SetBlobTier** | frecuente -> esporádico, frecuente -> archivo, esporádico -> archivo | archivo -> esporádico, archivo -> frecuente, esporádico -> frecuente

Si cambia el nivel de acceso de la cuenta de frecuente a esporádico, solo se le cobrarán las operaciones de escritura (por 10 000) de todos los blobs sin un nivel establecido en las cuentas de GPv2. En las cuentas de Blob Storage no se realiza ningún cargo por este cambio. Si cambia el nivel de acceso de su cuenta de Blob Storage o de GPv2 de esporádico a frecuente, se le cobran tanto las operaciones de lectura (por 10 000) como las de recuperación de datos (por GB). También podrían aplicarse cargos por la eliminación temprana de algún blob que se haya trasladado desde el nivel de acceso esporádico o de archivo.

### <a name="cool-and-archive-early-deletion"></a>Eliminación temprana en los niveles de acceso esporádico y de archivo

Además de los cargos por GB y por mes, los blobs que se hayan incorporado al nivel de acceso esporádico (solo cuentas de GPv2) están sujetos a un período de eliminación temprana de este nivel de 30 días, y cualquier blob que se haya incorporado al nivel de acceso de archivo está sujeto a un período de eliminación temprana de este nivel de 180 días. Este cargo se prorratea. Por ejemplo, si un blob se mueve al nivel de acceso de archivo y, a continuación, se elimina o se mueve al nivel de acceso frecuente al cabo de 45 días, se le cobrará una cuota de eliminación temprana equivalente a 135 (180 menos 45) días a partir del almacenamiento de ese blob en el nivel de acceso de archivo.

Para calcular la eliminación temprana, use la propiedad de blob, **creation-time**, si no ha habido cambios en el nivel de acceso. En caso contrario, puede usar el nivel de acceso se ha modificado por última vez a acceso esporádico o archivo mediante la visualización de la propiedad de blob: **acceso nivel cambio tiempo**. Para más información sobre las propiedades de blob, consulte el artículo sobre cómo [obtener las propiedades de blob](https://docs.microsoft.com/rest/api/storageservices/get-blob-properties).

## <a name="comparing-block-blob-storage-options"></a>Comparar opciones de almacenamiento de blobs de bloque

Los niveles de acceso de la siguiente tabla se muestra una comparación de almacenamiento de blobs de bloque de rendimiento premium y el acceso frecuente, esporádico y archivo.

|                                           | **Rendimiento de Premium** | **Nivel de acceso frecuente** | **Nivel de acceso esporádico** | **Nivel de archivo**
| ----------------------------------------- | ---------------- | ------------ | ----- | ----- |
| **Disponibilidad**                          | 99,9 %            | 99,9 %        | 99% | N/D |
| **Disponibilidad** <br> **(lecturas de RA-GRS)**  | N/D              | 99,99%       | 99,9 % | N/D |
| **Cargos de uso**                         | Mayores costos de almacenamiento, el menor acceso y el costo de transacción | Mayores costos de almacenamiento, menores costos de acceso y transacciones | Menores costos de almacenamiento, mayores costos de acceso y transacciones | Menores costos de almacenamiento, mayores costos de acceso y transacciones |
| **Tamaño mínimo de objeto**                   | N/D | N/D | N/D | N/D |
| **Duración mínima del almacenamiento**              | N/D | N/D | 30 días (solo GPv2) | 180 días
| **Latency** <br> **(Tiempo hasta el primer byte)** | Milisegundos de un solo dígito | milisegundos | milisegundos | Menos de 15 horas

> [!NOTE]
> Para los objetivos de escalabilidad y rendimiento, consulte [objetivos de escalabilidad y rendimiento de Azure Storage para las cuentas de almacenamiento](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

## <a name="quickstart-scenarios"></a>Escenarios de inicio rápido

En esta sección se muestran los siguientes escenarios mediante Azure Portal:

- Cambio del nivel de acceso predeterminado en una cuenta de GPv2 o de Blob Storage.
- Cambio del nivel de acceso de un blob en una cuenta de GPv2 o de Blob Storage.

### <a name="change-the-default-account-access-tier-of-a-gpv2-or-blob-storage-account"></a>Cambio del nivel de acceso predeterminado en una cuenta de GPv2 o de Blob Storage

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Para desplazarse a su cuenta de almacenamiento, seleccione Todos los recursos y, después, seleccione la cuenta de almacenamiento.

1. En la hoja Configuración, haga clic en **Configuración** para ver o cambiar la configuración de la cuenta.

1. Seleccione el nivel de derechos de acceso para sus necesidades: Establezca **Nivel de acceso** en **Esporádico** o **Frecuente**.

1. Haga clic en **Guardar** en la parte superior de la hoja.

### <a name="change-the-tier-of-a-blob-in-a-gpv2-or-blob-storage-account"></a>Cambiar el nivel de un blob en una cuenta de almacenamiento de GPv2 o Blob

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Para navegar a un blob en la cuenta de almacenamiento, seleccione **todos los recursos**, seleccione la cuenta de almacenamiento, seleccione el contenedor y, a continuación, seleccione el blob.

1. En el **propiedades del Blob** hoja, seleccione el **nivel de acceso** del menú desplegable para seleccionar el **frecuente**, **esporádico**, o **Archive**  nivel de acceso.

1. Haga clic en **Guardar** en la parte superior de la hoja.

## <a name="pricing-and-billing"></a>Precios y facturación

Todas las cuentas de Blob Storage usan un modelo de precios para el almacenamiento de blobs basado en el nivel de cada blob. Tenga en cuenta las siguientes consideraciones de facturación:

- **Costos de almacenamiento**: Además de la cantidad de datos almacenados, el costo de almacenamiento de datos varía en función del nivel de acceso. El costo por gigabyte disminuye a medida que el nivel es más esporádico.
- **Costos de acceso a datos**: los gastos de acceso a los datos aumentan a medida que el nivel es más esporádico. Para los datos en el nivel de acceso esporádico y de archivo, se cobra un cargo de acceso a datos por gigabyte para lecturas.
- **Costos de transacciones**: hay un cargo por transacción para todos los niveles, que aumenta a medida que el nivel es más esporádico.
- **Costos de transferencia de datos de replicación geográfica**: este cargo solo se aplica a las cuentas con replicación geográfica configurada, incluidas GRS y RA-GRS. La transferencia de datos de replicación geográfica incurre en un cargo por gigabyte.
- **Costos de transferencia de datos salientes**: las transferencias de datos salientes (los datos que se transfieren fuera de una región de Azure) conllevan un cargo por el uso del ancho de banda por gigabyte, lo que es coherente con las cuentas de almacenamiento de uso general.
- **Cambiar el nivel de acceso**: Cambio del nivel de acceso de cuenta de acceso esporádico a frecuente conlleva un cargo igual a la lectura de todos los datos existentes en la cuenta de almacenamiento. Sin embargo, cambiar el nivel de almacenamiento de acceso de la cuenta de frecuente a esporádico genera un cargo igual que escribir todos los datos en el nivel de acceso esporádico (solo cuentas de uso general v2).

> [!NOTE]
> Para más información acerca de los precios de las cuentas de Blob Storage, consulte la página [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/). Para más información acercas los cargos por la transferencia de datos salientes, consulte la página [Detalles de precios de ancho de banda](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="faq"></a>Preguntas más frecuentes

**¿Debo usar cuentas de Blob Storage o de GPv2 si quiero almacenar mis datos por niveles?**

Se recomienda usar cuentas de GPv2 en lugar de las de Blob Storage para el almacenamiento por niveles. GPv2 admite todas las características de las cuentas de Blob Storage y muchas más. Los precios entre Blob Storage y GPv2 son casi idénticos, pero algunas características nuevas y reducciones de precios solo están disponibles en cuentas de GPv2. Las cuentas de GPv1 no admiten niveles.

La estructura de precios entre las cuentas de GPv1 y GPv2 es diferente y los clientes deben evaluar con cuidado ambas antes de decidirse a usar las cuentas de GPv2. Puede convertir fácilmente una cuenta existente de Blob Storage o de GPv1 en GPv2 con un simple clic. Para más información, vea [Introducción a las cuentas de Azure Storage](../common/storage-account-overview.md).

**¿Puedo almacenar objetos en las tres (frecuente, esporádico y archivo) los niveles en la misma cuenta de acceso?**

Sí. El atributo **access tier** establecido en el nivel de cuenta es el nivel predeterminado que se aplica a todos los objetos de esa cuenta sin un nivel establecido explícito. Sin embargo, el almacenamiento por niveles de blob permite establecer el nivel de acceso en el nivel de objeto, independientemente de cuál sea el valor del nivel de acceso en la cuenta. Los blobs de cualquiera de los niveles de acceso de tres (frecuente, esporádico o archivo) pueden existir en la misma cuenta.

**¿Puedo cambiar el nivel de acceso predeterminada de mi cuenta de almacenamiento Blob o de GPv2?**

Sí, puede cambiar el nivel de acceso predeterminada estableciendo la **nivel de acceso** atributo en la cuenta de almacenamiento. Cambiar el nivel de acceso se aplica a todos los objetos almacenados en la cuenta que no tengan un nivel explícito establecido. Al cambiar el nivel de acceso de frecuente a esporádico incurre en las operaciones de escritura (por 10 000) para todos los blobs sin un nivel establecido en las cuentas de GPv2 y al cambiar de esporádico a frecuente incurre en operaciones de lectura (por 10 000) y cobra la recuperación de datos (por GB) para todos los blobs en Blob storage y las cuentas de GPv2.

**¿Puedo establecer el nivel de acceso de cuenta predeterminado en archivo?**

 No. Solo se pueden establecer los niveles de acceso frecuente y esporádico como el nivel de acceso de cuenta predeterminada. El nivel de acceso de archivo solo puede establecerse en el nivel de objeto.

**En el que las regiones son el acceso frecuente, esporádico y disponibles en niveles de acceso de archivo?**

Los niveles de acceso frecuente y esporádico, junto con niveles de blob están disponibles en todas las regiones. El almacenamiento de archivo solo estará disponible inicialmente en regiones seleccionadas. Para obtener una lista completa, consulte [Productos disponibles por región](https://azure.microsoft.com/regions/services/).

**¿Los blobs en el nivel de acceso esporádico se comportan de manera diferente a los que en el nivel de acceso frecuente?**

Los blobs en el nivel de acceso frecuente tienen la misma latencia que los blobs en cuentas de almacenamiento de GPv1, GPv2 y Blob. Los blobs en el nivel de acceso esporádico tienen una latencia similar (en milisegundos) como blobs en cuentas de almacenamiento de GPv1, GPv2 y Blob. Los blobs en el nivel de acceso de archivo tienen varias horas de latencia de las cuentas de almacenamiento de GPv1, GPv2 y Blob.

Los blobs en el nivel de acceso esporádico tienen un nivel de servicio disponibilidad (SLA) ligeramente inferior a los blobs almacenados en el nivel de acceso frecuente. Para más información, consulte [Contrato de nivel de servicio para Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_2/).

**¿Las operaciones entre los niveles de acceso frecuente, esporádico y de archivo son las mismas?**

Sí. Todas las operaciones entre los niveles de acceso frecuente y esporádico son coherentes al 100 %. Todas las operaciones de archivo válidas, lo que incluye la eliminación, la enumeración y la obtención de propiedades y metadatos de blobs, junto con el nivel de blob establecido, son 100 % coherentes con el acceso frecuente y esporádico. Un blob no se puede leer ni modificar mientras se encuentre en el nivel de archivo.

**Al rehidratar un blob desde el nivel de acceso de archivo al frecuente o esporádico, ¿cómo se sabe cuándo la rehidratación está completa?**

Durante la rehidratación, se puede usar la operación de obtención de propiedades de blob para sondear el atributo **Archive Status** y confirmar cuándo ha finalizado el cambio de nivel. El estado indica "rehydrate-pending-to-hot" (rehidratación pendiente para acceso frecuente) o "rehydrate-pending-to-cool" (rehidratación pendiente para acceso esporádico), según el nivel de destino. Al finalizar, se quita la propiedad de blob archive status y la propiedad de blob **Access Tier** refleja el nuevo nivel de acceso frecuente o esporádico.  

**Después de establecer el nivel de acceso de un blob, ¿cuándo comienzan a facturarme con la tarifa adecuada?**

Cada blob siempre se factura según el nivel indicado por la propiedad **access tier** del blob. Al establecer un nuevo nivel para un blob, la propiedad **Access Tier** refleja inmediatamente el nuevo nivel para todas las transiciones. Sin embargo, al rehidratar un blob desde el nivel de almacenamiento de archivo a un nivel de almacenamiento de acceso frecuente o esporádico puede tardar varias horas. En este caso, se le factura según las tarifas de archivo hasta que la rehidratación finalice, momento en el cual la propiedad **access tier** refleja el nuevo nivel. En ese momento se le cobrará dicho blob a la tarifa del nivel de almacenamiento de acceso frecuente o esporádico.

**¿Cómo determino si me aplicarán un cargo por eliminación temprana al eliminar o trasladar un blob del nivel de acceso esporádico o de archivo?**

Los blobs que se eliminan o se trasladan del nivel de acceso esporádico (solo cuentas de GPv2) o de archivo antes de 30 días y 180 días, respectivamente, generarán un cargo por eliminación temprana prorrateado. Para determinar cuánto tiempo un blob ha estado en el nivel de acceso esporádico o de archivo, compruebe la propiedad del blob **access tier change time** que proporciona una marca del último cambio del nivel de acceso. Para obtener más información, consulte [eliminación temprana de acceso esporádico y archivo](#cool-and-archive-early-deletion).

**¿Qué herramientas y SDK de Azure admiten almacenamiento por niveles de blob y almacenamiento de archivo?**

Las herramientas de Azure Portal, PowerShell y la CLI, y las bibliotecas de cliente de .NET, Java, Python y Node.js admiten todas almacenamiento por niveles de blob y almacenamiento de archivo.  

**¿Cuántos datos se pueden almacenar en los niveles de acceso frecuente, esporádico y de archivo?**

Almacenamiento de datos junto con otros límites se establecen en el nivel de cuenta y no por nivel de acceso. Por lo tanto, puede elegir usar todo el límite en un nivel o entre los tres niveles. Para más información, consulte [Objetivos de escalabilidad y rendimiento de Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="next-steps"></a>Pasos siguientes

### <a name="evaluate-hot-cool-and-archive-in-gpv2-blob-storage-accounts"></a>Evaluación de los niveles de acceso frecuente, esporádico y de archivo en cuentas de GPv2 y de Blob Storage

[Comprobación de la disponibilidad de los niveles de acceso frecuente, esporádico o de archivo por región](https://azure.microsoft.com/regions/#services)

[Administración del ciclo de vida de Azure Blob Storage](storage-lifecycle-management-concepts.md)

[Evaluación del uso de las cuentas de almacenamiento actuales mediante la habilitación de las métricas de Azure Storage](../common/storage-enable-and-view-metrics.md)

[Comprobación de los precios de los niveles de acceso frecuente, esporádico y de archivo en cuentas de Blob Storage y de GPv2 por región](https://azure.microsoft.com/pricing/details/storage/)

[Detalles de precios de Transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/)