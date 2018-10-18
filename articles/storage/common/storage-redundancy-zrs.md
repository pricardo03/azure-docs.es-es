---
title: Creación de aplicaciones de Azure Storage de alta disponibilidad en almacenamiento con redundancia de zona (ZRS) | Microsoft Docs
description: Almacenamiento con redundancia de zona (ZRS) ofrece un modo sencillo de crear aplicaciones de alta disponibilidad. ZRS protege frente a errores de hardware en el centro de datos y algunos desastres regionales.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 03/20/2018
ms.author: jeking
ms.component: common
ms.openlocfilehash: 0a4ebefd89414fa62ca69f2f9cc726f471694bee
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733625"
---
# <a name="zone-redundant-storage-zrs-highly-available-azure-storage-applications"></a>Almacenamiento con redundancia de zona (ZRS): aplicaciones de Azure Storage de alta disponibilidad
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-zrs.md)]

## <a name="support-coverage-and-regional-availability"></a>Cobertura de soporte técnico y disponibilidad regional
ZRS actualmente admite los tipos de cuenta de uso general v2 estándar. Para más información sobre los tipos de cuentas de almacenamiento, consulte [Información general acerca de la cuenta de Azure Storage](storage-account-overview.md).

ZRS está disponible para blobs en bloques, blobs en páginas que no son de disco, archivos, tablas y colas.

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

ZRS podría no proteger los datos frente a un desastre regional en el que varias zonas resulten afectadas permanentemente. En lugar de eso, ZRS ofrece resistencia de los datos si dejan de estar disponibles de manera temporal. Para la protección frente a desastres regionales, Microsoft recomienda usar el almacenamiento con redundancia geográfica (GRS). Para más información sobre GRS, consulte [Almacenamiento con redundancia geográfica (GRS): replicación entre regiones para Azure Storage](storage-redundancy-grs.md).

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
> Microsoft dejará de usar y migrará las cuentas de ZRS clásico el 31 de marzo de 2021. Se enviarán más detalles a los clientes de ZRS clásico antes de que estas cuentas dejen de usarse. 
>
> Cuando ZRS esté [disponible con carácter general](#support-coverage-and-regional-availability) en una región, los clientes ya no podrán crear una cuenta de ZRS clásico desde el portal de dicha región. Se admite el uso de Microsoft PowerShell y la CLI de Azure para crear cuentas de ZRS clásico hasta que ZRS clásico esté en desuso.

ZRS clásico replica asincrónicamente datos en centros de datos dentro de una o dos regiones. Es posible que una réplica no esté disponible hasta que Microsoft inicie la conmutación por error al elemento secundario. Una cuenta de ZRS clásico no se puede convertir a o desde LRS o GRS, y no tiene métricas o funcionalidad de registro.

ZRS clásico solo está disponible para **blobs en bloques** de cuentas de almacenamiento de uso general V1 (GPv1). Para más información sobre las cuentas de almacenamiento, consulte [Introducción a las cuentas de Azure Storage](storage-account-overview.md).

Las cuentas de ZRS Classic no pueden convertirse a (o desde) LRS, GRS o RA-GRS. Las cuentas de ZRS Classic no admiten ni las métricas ni el registro.

Para migrar manualmente datos de cuentas de ZRS desde o hacia una cuenta de LRS, ZRS clásico, GRS o RA-GRS, use AzCopy, Explorador de Azure Storage, Azure PowerShell o la CLI de Azure. También puede compilar su propia solución de migración con una de las bibliotecas cliente de Azure Storage.

## <a name="see-also"></a>Otras referencias
- [Replicación de Azure Storage](storage-redundancy.md)
- [Almacenamiento con redundancia local (LRS): redundancia de datos de bajo costo para Azure Storage](storage-redundancy-lrs.md)
- [Almacenamiento con redundancia geográfica (GRS): replicación entre regiones para Azure Storage](storage-redundancy-grs.md)