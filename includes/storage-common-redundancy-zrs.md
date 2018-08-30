---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 5f7477671e7be4e6e8fd736ab94fa7e7b1e99a6e
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43184411"
---
El almacenamiento con redundancia de zona (ZRS) replica los datos de manera sincrónica en tres clústeres de almacenamiento en una sola región. Cada clúster de almacenamiento está separado físicamente de los demás y reside en su propia zona de disponibilidad (AZ). Cada zona de disponibilidad, y el clúster ZRS dentro de ella, es autónoma, con distintas herramientas y funcionalidades de red.

Almacenar datos en una cuenta de ZRS garantiza que podrá acceder a los datos y administrarlos en caso de que una zona deje de estar disponible. ZRS proporciona un excelente rendimiento y baja latencia. ZRS ofrece los mismos [objetivos de escalabilidad](../articles/storage/common/storage-scalability-targets.md) que el [almacenamiento con redundancia local (LRS)](../articles/storage/common/storage-redundancy-lrs.md).

Considere a ZRS para los escenarios que requieren coherencia máxima, durabilidad sólida y alta disponibilidad, incluso si una interrupción o un desastre natural hace que un centro de datos de zona deje de estar disponible. ZRS proporciona a los objetos de almacenamiento una durabilidad de al menos el 99,9999999999 % (doce nueves) durante un año determinado.

Para más información sobre las zonas de disponibilidad, consulte [Introducción a las zonas de disponibilidad](https://docs.microsoft.com/azure/availability-zones/az-overview).

## <a name="support-coverage-and-regional-availability"></a>Cobertura de soporte técnico y disponibilidad regional
ZRS actualmente admite los tipos de cuenta [de uso general v2 (GPv2)](../articles/storage/common/storage-account-options.md#general-purpose-v2-accounts) estándar. ZRS está disponible para blobs en bloques, blobs en páginas que no son de disco, archivos, tablas y colas. Además, todos los registros de [Storage Analytics](../articles/storage/common/storage-analytics.md) y [las métricas de almacenamiento](../articles/storage/common/storage-enable-and-view-metrics.md).

ZRS está disponible con carácter general en las regiones siguientes:

- Este de EE. UU.
- Este de EE. UU. - 2
- Oeste de EE. UU. 2
- Centro de EE. UU.
- Europa del Norte
- Europa occidental
- Centro de Francia
- Sudeste asiático

Microsoft sigue habilitando ZRS en otras regiones de Azure. Revise periódicamente la página [Actualizaciones de servicios Azure](https://azure.microsoft.com/updates/) para información sobre las regiones nuevas.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>¿Qué ocurre cuando una zona deja de estar disponible?

Los datos permanecen resistentes si una zona deja de estar disponible. Microsoft recomienda que continúe siguiendo los procedimientos para el control de errores transitorios, como la implementación de directivas de reintentos con retroceso exponencial. Cuando una zona no está disponible, Azure realiza actualizaciones de red, como el redireccionamiento de DNS. Estas actualizaciones pueden afectar la aplicación si se accede a los datos antes de que se completen.

ZRS podría no proteger los datos frente a un desastre regional en el que varias zonas resulten afectadas permanentemente. En lugar de eso, ZRS ofrece resistencia de los datos si dejan de estar disponibles de manera temporal. Para la protección frente a desastres regionales, Microsoft recomienda usar el almacenamiento con redundancia geográfica (GRS). Para más información sobre GRS, consulte [Almacenamiento con redundancia geográfica (GRS): replicación entre regiones para Azure Storage](../articles/storage/common/storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Conversión a la replicación de ZRS
Actualmente, puede usar Azure Portal o la API de proveedor de recursos de almacenamiento para cambiar el tipo de redundancia de la cuenta, siempre que vaya a migrar desde o hacia LRS, GRS y RA-GRS. Sin embargo, la migración no es tan sencilla con ZRS, porque implica el movimiento de datos físico desde un sello de almacenamiento único a varios sellos dentro de una región. 

Tiene dos opciones principales para la migración desde o hacia ZRS. Puede copiar o mover los datos de manera manual a una cuenta de ZRS nueva desde una cuenta existente. También puede solicitar una migración en vivo. Microsoft recomienda encarecidamente realizar una migración manual porque no hay garantía alguna de cuándo se completará una migración en vivo. Una ruta de migración manual proporciona más flexibilidad que una migración en vivo y es el usuario el que tiene el control de la sincronización de la migración.

Tiene varias opciones para realizar una migración manual:
- Use herramientas existentes como AzCopy, el SDK de almacenamiento, herramientas de terceros de confianza, etc.
- Si está familiarizado con Hadoop o HDInsight, puede conectar tanto la cuenta de origen como la cuenta de destino (ZRS) al clúster y usar algo como DistCp con el fin de paralelizar el proceso de copia de datos de manera masiva.
- Compile sus propias herramientas al aprovechar un tipo del SDK de almacenamiento.

Sin embargo, si una migración manual dará como resultado un tiempo de inactividad de la aplicación y eso no es algo que pueda amortiguar, Microsoft proporciona una opción de migración en vivo. Una migración en vivo es una migración en contexto que le permite seguir usando su cuenta de almacenamiento mientras se migran los datos entre los sellos de almacenamiento de origen y destino. Durante la migración, seguirá teniendo el mismo nivel de durabilidad y el SLA de disponibilidad como lo haría normalmente.

Pero la migración en vivo tiene ciertas restricciones, las que se muestran a continuación.

- Si bien Microsoft abordará rápidamente la solicitud de migración en vivo, no hay ninguna garantía respecto de cuándo se completará la migración. Si es necesario que los datos estén en ZRS en un momento determinado, debe realizar una migración manual. En general, cuantos más datos tenga en su cuenta, más tiempo se tardará en migrar esos datos. 
- Solo puede realizar una migración en vivo de una cuenta que usa la replicación de LRS o GRS. Si la cuenta usa RA-GRS, primero deberá migrar a uno de estos tipos de replicación antes de continuar. Este paso intermedio garantiza que el punto de conexión secundario de solo lectura que RA-GRS proporciona se quite antes de la migración.
- La cuenta debe tener datos.
- Solo se admiten migraciones dentro de una región. Si quiere migrar los datos a una cuenta de ZRS ubicada en una región distinta de la región de la cuenta de origen, debe hacer una migración manual.
- Solo se admiten los tipos de cuenta de almacenamiento estándar. No se puede migrar desde una cuenta de almacenamiento premium.

Las solicitudes de migración en vivo pasan por el Portal de soporte técnico de Azure. En el portal, seleccione la cuenta de almacenamiento que quiere convertir a ZRS.
1. Haga clic en **Nueva solicitud de soporte técnico**.
2. Compruebe los aspectos básicos. Haga clic en **Next**. 
3. En la sección **Problem** (Problema), 
    - No modifique Severity (Gravedad)
    - Problem Type (Tipo de problema) = **Migración de datos**
    - Category (Categoría) = **Migración a ZRS dentro de una región**
    - Title (Título) = **Migración de cuenta de ZRS** (o alguna descripción)
    - Details (Detalles) = Me gustaría migrar a ZRS desde [LRS, GRS] en la región ______. 
4. Haga clic en **Next**.
5. Compruebe que la información de contacto sea correcta en la hoja Información de contacto.
6. Haga clic en **Enviar**.

Una persona responsable de soporte técnico se pondrá en contacto con usted. Esa persona estará disponible para proporcionar cualquier asistencia que pueda necesitar. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS clásico: opción heredada para la redundancia de los blobs en bloques
> [!NOTE]
> Las cuentas de ZRS Classic caerán en desuso y será necesario hacer la migración antes del 31 de marzo de 2021. Microsoft enviará más detalles a los clientes de ZRS Classic antes de que estas cuentas dejen de usarse. En el futuro, los planes de Microsoft proporcionarán un proceso de migración automatizado de ZRS clásico a ZRS.

>[!NOTE]
> Una vez que ZRS esté [disponible con carácter general](#support-coverage-and-regional-availability) en una región, ya no podrá crear una cuenta de ZRS clásico desde el portal de dicha región. Sin embargo, aún puede crear una con otros medios, como Microsoft PowerShell y la CLI de Azure, es decir, hasta que ZRS clásico quede en desuso.

ZRS clásico replica asincrónicamente datos en centros de datos dentro de una o dos regiones. Es posible que una réplica no esté disponible hasta que Microsoft inicie la conmutación por error al elemento secundario. ZRS clásico solo está disponible para **blobs en bloques** de cuentas de almacenamiento [de uso general (GPv1) V1](../articles/storage/common/storage-account-options.md#general-purpose-v1-accounts). Una cuenta de ZRS clásico no se puede convertir a o desde LRS o GRS, y no tiene métricas o funcionalidad de registro.

Las cuentas de ZRS Classic no pueden convertirse a (o desde) LRS, GRS o RA-GRS. Las cuentas de ZRS Classic no admiten ni las métricas ni el registro.

Para migrar manualmente datos de cuentas de ZRS desde o hacia una cuenta de LRS, ZRS clásico, GRS o RA-GRS, use AzCopy, Explorador de Azure Storage, Azure PowerShell o la CLI de Azure. También puede compilar su propia solución de migración con una de las bibliotecas cliente de Azure Storage.
