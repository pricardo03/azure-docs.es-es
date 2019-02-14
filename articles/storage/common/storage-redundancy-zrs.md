---
title: Creación de aplicaciones de Azure Storage de alta disponibilidad en almacenamiento con redundancia de zona (ZRS) | Microsoft Docs
description: Almacenamiento con redundancia de zona (ZRS) ofrece un modo sencillo de crear aplicaciones de alta disponibilidad. ZRS protege frente a errores de hardware en el centro de datos y algunos desastres regionales.
services: storage
author: tolandmike
ms.service: storage
ms.topic: article
ms.date: 10/24/2018
ms.author: jeking
ms.subservice: common
ms.openlocfilehash: 11891153f1ffce438597dc4f2799a9f25d76c2f5
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992609"
---
# <a name="zone-redundant-storage-zrs-highly-available-azure-storage-applications"></a>Almacenamiento con redundancia de zona (ZRS): aplicaciones de Azure Storage de alta disponibilidad
[!INCLUDE [storage-common-redundancy-ZRS](../../../includes/storage-common-redundancy-zrs.md)]

## <a name="support-coverage-and-regional-availability"></a>Cobertura de soporte técnico y disponibilidad regional
ZRS actualmente admite los tipos de cuenta de uso general v2 estándar. Para más información acerca de los tipos de cuentas de almacenamiento, consulte la [Introducción a la cuenta de Azure Storage](storage-account-overview.md).

ZRS está disponible para blobs en bloques, blobs en páginas que no son de disco, archivos, tablas y colas.

ZRS está disponible con carácter general en las regiones siguientes:

- Sudeste de Asia
- Oeste de Europa
- Norte de Europa
- Centro de Francia
- Este de Japón
- Sur de Reino Unido 2
- Este de EE. UU.
- Este de EE. UU. - 2
- Oeste de EE. UU. 2
- Centro de EE. UU.

Microsoft sigue habilitando ZRS en otras regiones de Azure. Revise periódicamente la página [Actualizaciones de servicios Azure](https://azure.microsoft.com/updates/) para información sobre las regiones nuevas.

## <a name="what-happens-when-a-zone-becomes-unavailable"></a>¿Qué ocurre cuando una zona deja de estar disponible?
Los datos son accesibles para las operaciones de escritura y lectura incluso si una zona deja de estar disponible. Microsoft recomienda seguir procedimientos recomendados para el control de errores transitorios. Estos procedimientos incluyen la implementación de directivas de reintento con retroceso exponencial.

Cuando una zona no está disponible, Azure realiza actualizaciones de red, como el redireccionamiento de DNS. Estas actualizaciones pueden afectar a la aplicación si se accede a los datos antes de que se completen.

ZRS podría no proteger los datos frente a un desastre regional en el que varias zonas resulten afectadas permanentemente. En lugar de eso, ZRS ofrece resistencia de los datos si dejan de estar disponibles de manera temporal. Para la protección frente a desastres regionales, Microsoft recomienda usar el almacenamiento con redundancia geográfica (GRS). Para obtener más información acerca de GRS, vea [Almacenamiento con redundancia geográfica (GRS): replicación entre regiones para Azure Storage](storage-redundancy-grs.md).

## <a name="converting-to-zrs-replication"></a>Conversión a la replicación de ZRS
Migrar a o desde LRS, GRS y RA-GRS es sencillo. Use Azure Portal o la API del proveedor de recursos de almacenamiento para cambiar el tipo de redundancia de la cuenta. Azure, a continuación, replicará los datos según corresponda. 

Migrar datos desde o hacia ZRS requiere una estrategia diferente. La migración de ZRS implica el movimiento físico de los datos desde un sello de almacenamiento único a varios sellos dentro de una región.

Hay dos opciones principales para la migración desde o hacia ZRS: 

- Copie o mueva los datos de manera manual a una cuenta de ZRS nueva desde una cuenta existente.
- Solicite una migración en vivo.

Microsoft recomienda encarecidamente que realice una migración manual. Una migración manual proporciona más flexibilidad que una migración en vivo. Con una migración manual, puede controlar el tiempo.

Para realizar una migración manual, tiene diferentes opciones:
- Use las herramientas existentes como AzCopy, una de las bibliotecas cliente de Azure Storage o herramientas de terceros de confianza.
- Si está familiarizado con Hadoop o HDInsight, adjunte la cuenta de origen y destino (ZRS) al clúster. Después, realice un paralelismo del proceso de copia de datos con una herramienta como DistCp.
- Cree sus propias herramientas mediante una de las bibliotecas cliente de Azure Storage.

Una migración manual puede provocar tiempos de inactividad de la aplicación. Si la aplicación requiere alta disponibilidad, Microsoft también proporciona una opción de migración en vivo. Una migración en vivo es una migración en contexto. 

Durante una migración en vivo, puede usar la cuenta de almacenamiento mientras se migran los datos entre las marcas de almacenamiento de origen y destino. Durante el proceso de migración, seguirá teniendo el mismo nivel de durabilidad y el SLA de disponibilidad como lo haría normalmente.

Tenga en cuenta las siguientes restricciones en la migración en vivo:

- Si bien Microsoft controla la solicitud de migración en vivo rápidamente, no hay ninguna garantía respecto de cuándo se completará una migración en vivo. Si necesita migrar los datos a ZRS en una fecha concreta, Microsoft recomienda que, en su lugar, realice una migración manual. En general, cuantos más datos tenga en su cuenta, más tiempo se tardará en migrar esos datos. 
- La migración en vivo se admite solo para las cuentas de almacenamiento que utilizan la replicación de LRS o GRS. Si su cuenta usa RA-GRS, debe cambiar primero el tipo de replicación de la cuenta a LRS o GRS antes de continuar. Este paso intermedio quita el punto de conexión de solo lectura secundario proporcionado por RA-GRS antes de la migración.
- La cuenta debe tener datos.
- Solo puede migrar datos en la misma región. Si quiere migrar los datos a una cuenta de ZRS ubicada en una región distinta de la región de la cuenta de origen, debe hacer una migración manual.
- Solo los tipos de cuenta de almacenamiento estándar admiten la migración en vivo. Las cuentas de Premium Storage deben migrarse manualmente.

Puede solicitar la migración en vivo mediante el [Portal de soporte técnico de Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). En el portal, seleccione la cuenta de almacenamiento que quiere convertir a ZRS.
1. Seleccione **Nueva solicitud de soporte técnico**.
2. Complete los **aspectos básicos** según la información de su cuenta. En la sección **Servicio**, seleccione **Administración de cuentas de almacenamiento** y el recurso que desea convertir a ZRS. 
3. Seleccione **Next** (Siguiente). 
4. Especifique los siguientes valores en la sección **Problema**: 
    - **Gravedad**: deje el valor predeterminado tal cual.
    - **Tipo de problema**: seleccione **Migración de datos**.
    - **Categoría**: seleccione **Migración a ZRS dentro de una región**.
    - **Título**: escriba un título descriptivo, por ejemplo, **migración de cuentas de ZRS**.
    - **Detalles**: escriba información adicional en el cuadro **Detalles**; por ejemplo, me gustaría migrar a ZRS desde [LRS, GRS] en la región \_\_. 
5. Seleccione **Next** (Siguiente).
6. Compruebe que la información de contacto sea correcta en la hoja **Información de contacto**.
7. Seleccione **Crear**.

Un responsable de soporte técnico se pondrá en contacto con usted para proporcionarle la asistencia que necesite. 

## <a name="zrs-classic-a-legacy-option-for-block-blobs-redundancy"></a>ZRS clásico: opción heredada para la redundancia de los blobs en bloques
> [!NOTE]
> Microsoft dejará de usar y migrará las cuentas de ZRS clásico el 31 de marzo de 2021. Se enviarán más detalles a los clientes de ZRS clásico antes de que estas cuentas dejen de usarse. 
>
> Una vez que ZRS esté [disponible con carácter general](#support-coverage-and-regional-availability) en una región, los clientes ya no podrán crear una cuenta de ZRS clásico desde el portal de dicha región. El uso de Microsoft PowerShell y la CLI de Azure para crear cuentas de ZRS clásico se admite como opción hasta que ZRS clásico esté en desuso.

ZRS clásico replica asincrónicamente datos en centros de datos dentro de una o dos regiones. Puede que los datos replicados no estén disponibles hasta que Microsoft inicie la conmutación por error al elemento secundario. Una cuenta de ZRS clásico no puede convertirse a o desde LRS, GRS o RA-GRS. Las cuentas de ZRS clásico no admiten ni las métricas ni el registro.

ZRS clásico solo está disponible para **blobs en bloques** de cuentas de almacenamiento de uso general V1 (GPv1). Para más información sobre las cuentas de almacenamiento, vea [Introducción a las cuentas de Azure Storage](storage-account-overview.md).

Para migrar manualmente datos de cuentas de ZRS desde o hacia una cuenta de LRS, ZRS clásico, GRS o RA-GRS, use alguna de las siguientes herramientas: AzCopy, Explorador de Azure Storage, Azure PowerShell o la CLI de Azure. También puede compilar su propia solución de migración con una de las bibliotecas cliente de Azure Storage.

## <a name="see-also"></a>Otras referencias
- [Replicación de Azure Storage](storage-redundancy.md)
- [Almacenamiento con redundancia local (LRS): redundancia de datos de bajo costo para Azure Storage](storage-redundancy-lrs.md)
- [Almacenamiento con redundancia geográfica (GRS): replicación entre regiones para Azure Storage](storage-redundancy-grs.md)
