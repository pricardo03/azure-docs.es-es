---
title: 'Almacenamiento inmutable de blob: Azure Storage'
description: Azure Storage ofrece compatibilidad WORM (escribir una vez, leer muchas) con el almacenamiento de Blob (objeto), lo que permite a los usuarios almacenar datos en un estado no modificable durante un intervalo de tiempo especificado.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: tamram
ms.reviewer: hux
ms.subservice: blobs
ms.openlocfilehash: 92bfa4f13467763fd88b9ae993554aef69355d75
ms.sourcegitcommit: 428fded8754fa58f20908487a81e2f278f75b5d0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2019
ms.locfileid: "74555229"
---
# <a name="store-business-critical-blob-data-with-immutable-storage"></a>Almacenamiento de datos de blobs críticos para la empresa con almacenamiento inmutable

El almacenamiento inmutable para Azure Blob Storage permite a los usuarios almacenar objetos de datos críticos para la empresa en un estado WORM (escribir una vez, leer muchas). En este estado, los usuarios no pueden borrar ni modificar los datos durante el intervalo de tiempo especificado por el usuario. Durante el intervalo de retención los blobs se pueden crear y leer, pero no modificar ni eliminar. El almacenamiento inmutable está disponible para cuentas de almacenamiento de uso general v2 y de Blob Storage en todas las regiones de Azure.

Para obtener información sobre cómo establecer y borrar las suspensiones legales o crear una directiva de retención basada en el tiempo mediante Azure Portal, PowerShell o la CLI de Azure, vea [Establecimiento y administración de directivas de inmutabilidad para el almacenamiento de blobs](storage-blob-immutability-policies-manage.md).

## <a name="about-immutable-blob-storage"></a>Acerca del almacenamiento de blobs inmutable

El almacenamiento inmutable ayuda a los centros de atención médica, las instituciones financieras y los sectores relacionados &mdash;sobre todo las organizaciones de agente-distribuidor&mdash; a almacenar los datos de forma segura. También puede usarse en cualquier escenario para proteger datos críticos e impedir que se eliminen o modifiquen.

Las aplicaciones típicas son:

- **Cumplimiento normativo**: el almacenamiento inmutable para Azure Blob Storage ayuda a las organizaciones a cumplir SEC 17a-4(f), CFTC 1.31(d), FINRA y otras regulaciones. En una nota técnica de producto de Cohasset Associates se detalla cómo el almacenamiento inmutable aborda estos requisitos reguladores. Este documento se puede descargar en el [portal de confianza de servicios de Microsoft](https://aka.ms/AzureWormStorage). El [Centro de confianza de Azure](https://www.microsoft.com/trustcenter/compliance/compliance-overview) contiene información detallada sobre nuestras certificaciones de cumplimiento.

- **Retención segura de documentos**: el almacenamiento inmutable de Azure Blob Storage garantiza que ningún usuario puede modificar ni eliminar datos, incluidos los usuarios que tienen privilegios administrativos en la cuenta.

- **Suspensión legal**: el almacenamiento inmutable de Azure Blob Storage permite a los usuarios almacenar información confidencial crítica para procedimientos judiciales o uso empresarial en un estado a prueba de manipulación durante el tiempo deseado hasta que se elimina la suspensión. Esta característica no se limita solo a los casos de uso legales, sino que también puede considerarse una suspensión basada en eventos o un bloqueo de la empresa, donde es necesario proteger los datos basados en desencadenadores de eventos o directivas corporativas.

El almacenamiento inmutable admite las características siguientes:

- **[Compatibilidad con directivas de retención con duración definida](#time-based-retention-policies)** : los usuarios pueden establecer directivas para almacenar datos durante un intervalo especificado. Cuando se establece una directiva de retención con duración definida, se pueden crear y leer blobs, pero no se pueden modificar ni eliminar. Una vez transcurrido el período de retención, los blobs se pueden eliminar pero no sobrescribir.

- **[Compatibilidad con directivas de suspensión legal](#legal-holds)** : si el intervalo de retención no se conoce, los usuarios pueden establecer suspensiones legales para almacenar los datos inmutables hasta que estas desaparezcan.  Cuando se establece una directiva de suspensión legal, se pueden crear y leer blobs, pero no se pueden modificar ni eliminar. Cada suspensión legal está asociada a una etiqueta alfanumérica definida por el usuario que se usa como una cadena de identificación (por ejemplo, un identificador de caso, un nombre de evento, etc.). 

- **Compatibilidad con todos los niveles de blobs:** las directivas WORM son independientes del nivel de Azure Blob Storage y se aplican a todos los niveles: de archivo, frecuente y esporádico. Los usuarios pueden transferir sus datos al nivel que les ofrezca la mayor optimización de costos de acuerdo con sus cargas de trabajo sin alterar la inmutabilidad de los datos.

- **Configuración en el nivel de contenedor**: los usuarios pueden configurar las directivas de retención con duración definida y las etiquetas de suspensión legal a nivel del contenedor. Mediante valores de configuración sencillos en el nivel de contenedor, los usuarios pueden crear y bloquear las directivas de retención con duración definida, ampliar los intervalos de retención, establecer y eliminar suspensiones legales, etc. Estas directivas se aplican a todos los blobs del contenedor, tanto a los nuevos como a los existentes.

- **Compatibilidad con el registro de auditoría**: todos los contenedores incluyen un registro de auditoría de directiva. En él se muestran hasta siete comandos de retención con duración definida para las directivas de retención con duración definida bloqueadas y contiene el identificador de usuario, el tipo de comando, las marcas de tiempo y el intervalo de retención. En el caso de las suspensiones legales, el registro contiene el identificador del usuario, el tipo de comando, las marcas de tiempo y las etiquetas de suspensión legal. Este registro se conserva mientras dure la directiva, de acuerdo con las directrices de regulación SEC 17a-4(f). El [registro de actividad de Azure](../../azure-monitor/platform/activity-logs-overview.md) muestra un registro más completo de todas las actividades del plano de control, mientras que al habilitar los [registros de diagnóstico de Azure](../../azure-monitor/platform/resource-logs-overview.md) se conservan y se muestran las operaciones del plano de datos. Es responsabilidad del usuario almacenar dichos registros de forma persistente, ya que podría ser obligatorio por ley o por otros fines.

## <a name="how-it-works"></a>Cómo funciona

Almacenamiento inmutable para Azure Blob Storage admite dos tipos de directivas inmutables o WORM: retención con duración definida y suspensiones legales. Cuando se aplica una directiva de retención con duración definida o una suspensión legal a un contenedor, todos los blobs existentes pasan al estado WORM inmutable en menos de 30 segundos. Todos los nuevos blobs que se carguen en el contenedor también pasan al estado inmutable. Una vez que todos los blobs han pasado al estado inmutable, la directiva inmutable se confirma y no se permite ninguna operación de sobrescritura o eliminación de los objetos nuevos y existentes en el contenedor inmutable.

No se permite la eliminación de la cuenta de almacenamiento y el contenedor si hay blobs en el contenedor o la cuenta de almacenamiento que están protegidos mediante una directiva inmutable. La operación de eliminación de contenedor producirá un error si hay al menos un blob con una directiva de retención de duración definida o una suspensión legal. Se producirá un error en la operación de eliminación de la cuenta de almacenamiento si hay al menos un contenedor WORM con una suspensión legal o un blob con un intervalo de retención activo.

### <a name="time-based-retention-policies"></a>Directivas de retención con duración definida

> [!IMPORTANT]
> Una directiva de retención con duración definida debe estar *bloqueada* para que el blob esté en estado inmutable (protegido frente a escritura y eliminación) y, por consiguiente, se cumplan SEC 17a-4(f) y otras regulaciones. Se recomienda bloquear la directiva en un período razonable, normalmente antes de 24 horas. El estado inicial de una directiva de retención con duración definida es *desbloqueada*, lo que permite probar la característica y realizar cambios en la directiva antes de bloquearla. Aunque el estado *desbloqueada* proporciona protección de inmutabilidad, no se recomienda su uso para otros fines que no sean las evaluaciones de la característica a corto plazo. 

Cuando se aplica una directiva de retención con duración definida a un contenedor, todos los blobs de este permanecen en estado inmutable durante el período de retención *efectivo*. El período de retención efectivo para los blobs existentes es igual a la diferencia entre la hora de creación de blob y el intervalo de retención especificado por el usuario.

Para los nuevos blobs, el período de retención efectivo es igual al intervalo de retención especificado por el usuario. Como los usuarios pueden ampliar el intervalo de retención, el almacenamiento inmutable utiliza el valor más reciente del intervalo de retención especificado por el usuario para calcular el período de retención efectivo.

Por ejemplo, supongamos que un usuario crea una directiva de retención de duración definida con un intervalo de retención de cinco años. El blob que hay en ese contenedor, _testblob1_, se ha creado hace un año. El período de retención efectivo de _testblob1_ es de cuatro años. Cuando se carga un nuevo blob, _testblob2_, en el contenedor, el período de retención efectivo del nuevo blob es de cinco años.

Se recomienda una directiva de retención con duración definida desbloqueada solo para probar la característica; no obstante, una directiva debe estar bloqueada para el cumplimiento de SEC 17a-4(f) y otras normas. Una vez que se ha bloqueado una directiva de retención con duración definida, no se puede quitar la directiva y se permite un máximo de cinco aumentos al período de retención efectivo. Para obtener más información sobre cómo establecer y bloquear las directivas de retención con duración definida, vea [Establecimiento y administración de directivas de inmutabilidad para el almacenamiento de blobs](storage-blob-immutability-policies-manage.md).

Los límites siguientes se aplican a las directivas de retención:

- En el caso de una cuenta de almacenamiento, el número máximo de contenedores con directivas inmutables con duración definida bloqueadas es 1000.
- El intervalo de retención mínimo es un día. El máximo es 146 000 días (400 años).
- Para un contenedor, el número máximo de modificaciones para ampliar un intervalo de retención para las directivas inmutables con duración definida bloqueadas es 5.
- En el caso de un contenedor, se retiene un máximo de siete registros de auditoría de la directiva de retención con duración definida para una directiva bloqueada.

### <a name="legal-holds"></a>Retenciones legales

Si se establece una suspensión legal, tanto los blobs nuevos como los existentes permanecen en estado inmutable hasta que se elimine dicha suspensión. Para obtener más información sobre cómo establecer y borrar las suspensiones legales, vea [Establecimiento y administración de directivas de inmutabilidad para el almacenamiento de blobs](storage-blob-immutability-policies-manage.md).

Un contenedor puede tener una suspensión legal y una directiva de retención con duración definida al mismo tiempo. Todos los blobs de ese contenedor permanecen en estado inmutable hasta que se eliminen todas las suspensiones legales, aunque haya expirado el período de retención efectivo. Por el contrario, un blob permanece en estado inmutable hasta que expire el período de retención efectivo, aunque se hayan eliminado todas las suspensiones legales.

La tabla siguiente muestra los tipos de operaciones de almacenamiento de blobs que se deshabilitan para los diferentes escenarios de inmutabilidad. Para obtener más información, vea la documentación de la [API REST de servicios Azure Blob](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api).

|Escenario  |Estado del blob  |Operaciones de blob no permitidas  |
|---------|---------|---------|
|El intervalo de retención efectivo del blob no ha expirado todavía o se ha establecido una retención legal     |Inmutable: protegido frente a eliminación y escritura         | Put Blob<sup>1</sup>, Put Block<sup>1</sup>, Put Block List<sup>1</sup>, Delete Container, Delete Blob, Set Blob Metadata, Put Page, Set Blob Properties,  Snapshot Blob, Incremental Copy Blob, Append Block         |
|El intervalo de retención efectivo del blob ha expirado     |Protegido contra escritura únicamente (están permitidas las operaciones de eliminación)         |Put Blob<sup>1</sup>, Put Block<sup>1</sup>, Put Block List<sup>1</sup>, Set Blob Metadata, Put Page, Set Blob Properties, Snapshot Blob, Incremental Copy Blob, Append Block         |
|Todas las suspensiones legales eliminadas y sin ninguna directiva de retención basada en el tiempo establecida en el contenedor     |Mutable         |None         |
|No se crea ninguna directiva WORM (retención basada en el tiempo o suspensión legal)     |Mutable         |None         |

<sup>1</sup> la aplicación permite que estas operaciones creen un blob una vez. Todas las sucesivas operaciones de sobrescritura en una ruta de acceso de blob existente en un contenedor inmutable no se permiten.

Los límites siguientes se aplican a las suspensiones legales:

- En el caso de una cuenta de almacenamiento, el número máximo de contenedores con un valor de suspensión legal es 1.000.
- En un contenedor, el número máximo de etiquetas de suspensión legal es 10.
- La longitud mínima de una etiqueta de suspensión legal es de tres caracteres alfanuméricos. La longitud máxima es de 23 caracteres alfanuméricos.
- Para un contenedor, se conservan hasta 10 registros de auditoría de directiva de suspensión legal lo que dura la directiva.

## <a name="pricing"></a>Precios

El uso de esta característica no tiene costo adicional. El precio de los datos inmutables es el mismo que el de los datos mutables. Para obtener información detallada sobre los precios de almacenamiento de blobs de Azure, vea la [página de precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="faq"></a>Preguntas más frecuentes

**¿Se puede proporcionar documentación de cumplimiento WORM?**

Sí. Para documentar el cumplimiento, Microsoft ha usado una de las principales empresas de valoración independiente especializada en la administración de registros y la gobernanza de la información, Cohasset Associates. Dicha empresa se ha encargado de evaluar el almacenamiento de blobs inmutable y el cumplimiento con los requisitos específicos del sector de servicios financieros. Cohasset ha validado que el almacenamiento de blobs inmutable, cuando se ha usado para conservar los blobs con duración definida en un estado WORM, cumplía los requisitos de almacenamiento pertinentes de las reglas CFTC 1.31(c)-(d), FINRA 4511 y SEC 17a-4. Microsoft se centró en este conjunto de reglas, ya que representan la guía más prescriptiva globalmente para la retención de registros en las instituciones financieras. El informe de Cohasset está disponible en el [Centro de confianza de servicios de Microsoft](https://aka.ms/AzureWormStorage). Para solicitar una carta de certificación de Microsoft con respecto al cumplimiento WORM, póngase en contacto con el soporte técnico de Azure.

**¿La característica se aplica solo a los blobs en bloques o también a los blobs en páginas y a los blobs en anexos?**

El almacenamiento inmutable se puede usar con cualquier tipo de blobs cuando se establece en el nivel de contenedor, pero se recomienda usar WORM para los contenedores que almacenan principalmente blobs en bloques. A diferencia de los blobs en bloques, los nuevos blobs en páginas y los blobs en anexos se deben crear fuera de un contenedor WORM y luego se deben copiar en él. Después de copiar estos blobs en un contenedor WORM, no se permiten más *anexos* a un blob en anexos ni cambios en un blob en páginas. No se recomienda establecer una directiva de WORM en un contenedor que almacene discos duros virtuales (blobs en páginas) para cualquier máquina virtual activa, ya que bloqueará el disco de la máquina virtual.

**¿Es necesario crear una cuenta de almacenamiento para usar esta característica?**

No, puede usar el almacenamiento inmutable con cualquier cuenta de almacenamiento de uso general V2 y de blobs existente o recién creada. Esta característica está pensada para su uso con blobs en bloques en cuentas de GPv2 y Blob Storage. Las cuentas de almacenamiento de uso general v1 no se admiten, pero se pueden actualizar fácilmente a uso general v2. Para obtener información sobre la actualización de una cuenta de almacenamiento de uso general v1 existente, vea [Actualización de una cuenta de almacenamiento](../common/storage-account-upgrade.md).

**¿Se puede aplicar una suspensión legal y una directiva de retención con duración definida a la vez?**

Sí, un contenedor puede tener una suspensión legal y una directiva de retención con duración definida al mismo tiempo. Todos los blobs de ese contenedor permanecen en estado inmutable hasta que se eliminen todas las suspensiones legales, aunque haya expirado el período de retención efectivo. Por el contrario, un blob permanece en estado inmutable hasta que expire el período de retención efectivo, aunque se hayan eliminado todas las suspensiones legales.

**¿Las directivas de suspensión legal son solo para procedimientos judiciales o pueden emplearse en otros casos?**

No, la suspensión legal es simplemente el término general que se usa con una directiva de retención sin duración definida. Su uso no se aplica solamente a procedimientos judiciales. También es útil para deshabilitar la sobrescritura y las eliminaciones con el fin de proteger los datos WORM importantes de la empresa, cuando el período de retención se desconoce. Se puede usar como una directiva de empresa para proteger las cargas de trabajo WORM críticas o como una directiva de almacenamiento provisional antes de que un desencadenador de eventos personalizado requiera el uso de una directiva de retención con duración definida. 

**¿Se puede quitar una suspensión legal o una directiva de retención con duración definida _bloqueada_?**

Solo las directivas de retención con duración definida desbloqueadas se pueden quitar de un contenedor. Una vez que se ha bloqueado una directiva de retención con duración definida, no se puede quitar; solo se permiten extensiones del período de retención efectivo. Se pueden eliminar las etiquetas de suspensión legal. Cuando se eliminan todas las etiquetas legales, se quita la suspensión legal.

**¿Qué ocurre si se intenta eliminar un contenedor con una directiva de retención basada en el tiempo o una retención legal *bloqueada*?**

La operación Delete Container producirá un error si hay al menos un blob con una directiva de retención de duración definida o una retención legal. La operación Delete Container solo se ejecutará correctamente si no existe ningún intervalo de retención activo y no hay retenciones legales. Para poder eliminar el contenedor, antes debe eliminar los blobs.

**¿Qué ocurre si se intenta eliminar una cuenta de almacenamiento con un contenedor WORM que tiene una directiva de retención basada en el tiempo o una retención legal*bloqueada*?**

Se producirá un error en la eliminación de la cuenta de almacenamiento si hay al menos un contenedor WORM con una retención legal o un blob con un intervalo de retención activo. Para poder eliminar la cuenta de almacenamiento, antes debe eliminar todos los contenedores WORM. Para obtener información acerca de la eliminación de contenedores, vea la pregunta anterior.

**¿Puedo mover los datos entre distintos niveles de blob (nivel de acceso frecuente, esporádico, poco frecuente) cuando el blob está en estado inmutable?**

Sí, puede usar el comando Set Blob Tier para mover datos entre los niveles de blob y mantenerlos al mismo tiempo en estado inmutable compatible. El almacenamiento inmutable solamente se admite en los niveles de blob de acceso frecuente, de acceso esporádico y de archivo.

**¿Qué ocurre si dejo de pagar y no ha expirado el intervalo de retención?**

En caso de impago, las directivas de retención de datos normales se aplicarán como estipulado en los términos y condiciones de su contrato con Microsoft.

**¿Se ofrece una evaluación o un período de gracia para probar la característica?**

Sí. Cuando se crea por primera vez una directiva de retención con duración definida, tiene el estado *desbloqueada*. En este estado, puede realizar los cambios que desee en el intervalo de retención, como por ejemplo, aumentarlo o disminuirlo, e incluso eliminar la directiva. Una vez bloqueada la directiva, permanece en este estado hasta que expira el intervalo de retención. Esta directiva bloqueada evita la eliminación y la modificación del intervalo de retención. Se recomienda encarecidamente que utilice el estado *desbloqueado* únicamente para fines de prueba y que bloquee la directiva en un periodo de 24 horas. Estas prácticas ayudan a cumplir SEC 17a-4(f) y otras normativas.

**¿Se puede usar la eliminación temporal junto con las directivas de blob inmutables?**

Sí. La [eliminación temporal para Azure Blob Storage](storage-blob-soft-delete.md) se aplica a todos los contenedores dentro de una cuenta de almacenamiento, con independencia de que exista una directiva de retención con duración definida o una suspensión legal. Se recomienda habilitar la eliminación temporal para mayor protección antes de aplicar y confirmar las directivas WORM inmutables.

**¿Dónde está disponible la característica?**

El almacenamiento inmutable está disponible en las regiones Government, China y Azure público. Si el almacenamiento inmutable no está disponible en su región, envíe un correo al servicio de soporte técnico a la dirección azurestoragefeedback@microsoft.com.

## <a name="next-steps"></a>Pasos siguientes

[Establecimiento y administración de directivas de inmutabilidad para el almacenamiento de blobs](storage-blob-immutability-policies-manage.md)