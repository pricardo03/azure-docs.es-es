---
title: Uso de Azure Import/Export para transferir datos a y desde Azure Storage | Microsoft Docs
description: Aprenda a crear trabajos de importación y exportación en Azure Portal para transferir datos a y desde Azure Storage.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 07/11/2018
ms.author: alkohli
ms.openlocfilehash: c435e21d85ae0ab35bc2fa99f7006e841eaecec0
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39248785"
---
# <a name="what-is-azure-importexport-service"></a>¿Qué es el servicio Azure Import/Export?

El servicio Azure Import/Export se usa para importar de forma segura grandes cantidades de datos a Azure Blob Storage y Azure Files mediante el envío de unidades de disco a un centro de datos de Azure. También se puede usar este servicio para transferir datos desde Azure Blob Storage hasta las unidades de disco y enviarlas al sitio local. Se pueden importar los datos de uno o varios discos a Azure Blob Storage o Azure Files. 

El servicio de importación y exportación de Azure requiere que proporcione sus propios discos. Si desea transferir datos con los discos proporcionados por Microsoft, puede usar discos de Azure Data Box para importar datos en Azure. Microsoft envía hasta 5 discos de estado sólido (SSD) cifrados con una capacidad de 40 TB por pedido al centro de datos mediante un operador regional. Puede configurar rápidamente los discos, copiar los datos en ellos a través de una conexión USB 3.0 y enviarlos de vuelta a Azure. Para obtener más información, vaya a [Introducción a Microsoft Azure Data Box Disk](https://docs.microsoft.com/azure/databox/data-box-disk-overview).

## <a name="azure-importexport-usecases"></a>Casos de uso de Azure Import/Export

Considere el uso del servicio de Azure Import/Export cuando la carga o descarga de datos a través de la red sea demasiado lenta o el costo de ancho de banda de red adicional resulte prohibitivo. Use este servicio en los escenarios siguientes:

* **Migración de datos a la nube**: mueva rápidamente grandes cantidades de datos a Azure de manera rápida y rentable.
* **Distribución de contenido**: envíe rápidamente datos a los sitios de cliente.
* **Copia de seguridad**: realice copias de seguridad de los datos locales para almacenarlos en Azure Storage.
* **Recuperación de datos**: recupere una gran cantidad de datos almacenados en el almacenamiento y recíbalos en su ubicación local.

## <a name="importexport-components"></a>Componentes de Import/Export

El servicio Import/Export usa los siguientes componentes:

- Servicio **Import/Export**: este servicio disponible en Azure Portal ayuda al usuario a crear y a realizar el seguimiento de los trabajos de importación y exportación.  

- **Herramienta WAImportExport**: se trata de una herramienta de línea de comandos que realiza las siguientes acciones: 
    - Prepara las unidades de disco que se envían para la importación.
    - Facilita la copia de los datos en la unidad.
    - Cifra los datos en la unidad con BitLocker.
    - Genera los archivos del diario de unidad que se usan durante la creación de la importación.
    - Ayuda a identificar los números de unidades necesarias para los trabajos de exportación.

    Esta herramienta está disponible en dos versiones, la versión 1 y la 2. Se recomienda que use:

    - La versión 1 para la importación y exportación en Azure Blob Storage. 
    - La versión 2 para la importación de datos en Azure Files.

    La herramienta WAImportExport solo es compatible con el sistema operativo de Windows de 64 bits. Para versiones específicas de sistema operativo compatibles, vaya a [requisitos de Azure Import/Export](storage-import-export-requirements.md#supported-operating-systems).

- **Discos**: puede enviar unidades de estado sólido (SSD) o unidades de disco duro (HDD) al centro de datos de Azure. Al crear un trabajo de importación, se envían las unidades de disco que contienen los datos. Al crear un trabajo de exportación, se envían las unidades vacías al centro de datos de Azure. Para tipos de disco específicos, vaya a [Tipos de disco compatibles](storage-import-export-requirements.md#supported-hardware).

## <a name="how-does-importexport-work"></a>¿Cómo funciona Import/Export?

El servicio Azure Import/Export permite la transferencia de datos en Azure Blobs y en Azure Files mediante la creación de trabajos. Use Azure Portal o la API REST de Azure Resource Manager para crear trabajos. Cada trabajo está asociado a una única cuenta de almacenamiento. 

Los trabajos pueden ser de importación o de exportación. Un trabajo de importación permite importar datos en Azure Blobs o Azure Files, mientras que el trabajo de exportación permite la exportación de los datos desde Azure Blobs. Para un trabajo de importación, se envían las unidades que contienen los datos. Al crear un trabajo de exportación, se envían las unidades vacías al centro de datos de Azure. En cada caso, puede enviar hasta 10 unidades de disco por trabajo.

> [!IMPORTANT]
> Actualmente no se admite la exportación de datos en Azure Files.

En esta sección se describen los pasos de alto nivel implicados en los trabajos de importación y exportación. 


### <a name="inside-an-import-job"></a>Dentro de un trabajo de importación

En un nivel alto, un trabajo de importación implica los siguientes pasos:

1. Determinar los datos que se van a importar, el número de unidades que necesita, la ubicación del blob de destino para los datos en Azure Storage.
2. Utilizar la herramienta WAImportExport para copiar datos en las unidades de disco. Cifrar los discos con BitLocker.
3. Crear un trabajo de importación en la cuenta de almacenamiento de destino en Azure Portal. Cargar los archivos del diario de unidad.
4. Indique el remite y el número de cuenta de la empresa de transporte que se utilizará para que se le envíen de vuelta las unidades.
5. Envíe las unidades de disco a la dirección de envío proporcionada durante la creación del trabajo.
6. Actualice el número de seguimiento de entrega en los datos del trabajo de importación y envíe dicho trabajo.
7. Las unidades se reciben y procesan en el centro de datos de Azure.
8. Las unidades se devuelven usando la cuenta de la empresa de transporte al remite indicado en el trabajo de importación.

> [!NOTE]
> Para los envíos locales (dentro del país del centro de datos), comparta una cuenta de transportista nacional 
>
> Para los envíos al extranjero (fuera del país del centro de datos), comparta una cuenta de transportista internacional

 ![Figura 1: Importación de flujos de trabajo](./media/storage-import-export-service/importjob.png)

Para obtener instrucciones detalladas sobre la importación de datos, vaya a:

- [Importación de datos en Azure Blobs](storage-import-export-data-to-blobs.md)
- [Importación de datos en Azure Files](storage-import-export-data-to-files.md)


### <a name="inside-an-export-job"></a>Dentro de un trabajo de exportación

> [!IMPORTANT]
> El servicio solo admite la exportación desde Azure Blobs. No se admite la exportación de archivos desde Azure Files.

En un nivel alto, un trabajo de exportación implica los siguientes pasos:

1. Determinar los datos que se van a exportar, el número de unidades necesarias, los blobs de origen o las rutas de acceso de contenedor de los datos de Blob Storage.
3. Crear un trabajo de exportación en la cuenta de almacenamiento de origen en Azure Portal.
4. Especificar los blobs de origen o las rutas de acceso de contenedor de los datos que se van a exportar.
5. Indique el remite y el número de cuenta de la empresa de transporte que se utilizará para que se le envíen de vuelta las unidades.
6. Envíe las unidades de disco a la dirección de envío proporcionada durante la creación del trabajo.
7. Actualice el número de seguimiento de entrega en los datos del trabajo de exportación y envíe dicho trabajo.
8. Las unidades se reciben y procesan en el centro de datos de Azure.
9. Las unidades se cifran con BitLocker y las claves están disponibles a través de Azure Portal.  
10. Las unidades se devuelven usando la cuenta de la empresa de transporte al remite indicado en el trabajo de importación.

> [!NOTE]
> Para los envíos locales (dentro del país del centro de datos), comparta una cuenta de transportista nacional 
>
> Para los envíos al extranjero (fuera del país del centro de datos), comparta una cuenta de transportista internacional
  
 ![Figura 2: Exportación de flujos de trabajo](./media/storage-import-export-service/exportjob.png)

Para obtener instrucciones detalladas sobre la exportación de datos, vaya a [Exportación de datos desde Azure Blobs](storage-import-export-data-from-blobs.md).

## <a name="region-availability"></a>Disponibilidad en regiones 

El servicio Azure Import/Export permite copiar datos en todas las cuentas de almacenamiento de Azure y desde estas. Las unidades de disco se pueden enviar a una de las ubicaciones de la lista. Si la cuenta de almacenamiento está en una ubicación de Azure que no se especifica aquí, se proporcionará una ubicación de envío alternativa cuando cree el trabajo.

### <a name="supported-shipping-locations"></a>Ubicaciones de envío admitidas


|País  |País  |País  |País  |
|---------|---------|---------|---------|
|Este de EE. UU    | Europa del Norte        | India Central        |US Gov Iowa         |
|Oeste de EE. UU.     |Europa occidental         | Sur de la India        | Departamento de Defensa de EE. UU. Este        |
|Este de EE. UU. 2    | Asia oriental        |  Oeste de la India        | Departamento de Defensa de EE. UU. Centro        |
|Oeste de EE. UU. 2     | Sudeste asiático        | Centro de Canadá        | Este de China         |
|Centro de EE. UU.     | Este de Australia        | Este de Canadá        | Norte de China        |
|Centro-Norte de EE. UU     |  Sudeste de Australia       | Sur de Brasil        | Sur de Reino Unido 2        |
|Centro-Sur de EE. UU     | Oeste de Japón        |Corea Central         | Centro de Alemania        |
|Centro occidental de EE.UU.     |  Este de Japón       | Gobierno de EE. UU. - Virginia        | Noreste de Alemania        |


## <a name="security-considerations"></a>Consideraciones sobre la seguridad

Los datos de la unidad se deben cifrar con BitLocker Drive Encryption. Este cifrado protege los datos mientras se encuentren en tránsito.

Para los trabajos de importación, las unidades se cifran de dos maneras.  


- La primera manera consiste en especificar la opción al usar el archivo *dataset.csv* mientras se ejecuta la herramienta WAImportExport durante la preparación de la unidad. 

- La segunda, en habilitar el cifrado de BitLocker manualmente en la unidad y especificar la clave de cifrado en el archivo *driveset.csv* al ejecutar la línea de comandos de la herramienta WAImportExport durante la preparación de la unidad.


En los trabajos de exportación, después de copiar los datos en las unidades, el servicio cifrará la unidad con BitLocker antes de enviársela de vuelta al usuario. La clave de cifrado se proporciona a través de Azure Portal.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]


### <a name="pricing"></a>Precios

**Cuota de manipulación de unidades**

Existe una cuota de manipulación por cada unidad procesada como parte del trabajo de importación o exportación. Consulte los detalles en [Precios de Azure Import/Export](https://azure.microsoft.com/pricing/details/storage-import-export/).

**Gastos de envío**

Al enviar unidades a Azure, pagar los gastos de envío al transportista. Cuando Azure le devuelve la unidad, los gastos de envío se le cargan en la cuenta de la empresa de transporte que indicó al crear el trabajo.

**Costos de transacción**

No hay ningún costo de transacción además de los costos de transacciones de almacenamiento estándar al importar datos en Azure Storage. Los cargos de salida estándar son aplicables cuando se exportan datos desde Blob Storage. Para más información sobre los costos de transacción, consulte [Detalles de precios de transferencias de datos](https://azure.microsoft.com/pricing/details/data-transfers/)



## <a name="next-steps"></a>Pasos siguientes

Aprenda a usar el servicio Import/Export para:
* [Importar datos en Azure Blobs](storage-import-export-data-to-blobs.md)
* [Exportar datos desde Azure Blobs](storage-import-export-data-from-blobs.md)
* [Importar datos en Azure Files](storage-import-export-data-to-files.md)

