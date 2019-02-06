---
title: Requisitos del sistema de Microsoft Azure Data Box | Microsoft Docs
description: Obtenga información acerca de los requisitos de software y de red de su dispositivo Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 01/23/2019
ms.author: alkohli
ms.openlocfilehash: 7d52af9e3948f40936795efab5b6671c3f71007a
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206754"
---
# <a name="azure-data-box-system-requirements"></a>Requisitos del sistema de Azure Data Box

En este artículo se describen los requisitos del sistema importantes tanto de Microsoft Azure Data Box como de los clientes de que se conectan a Data Box. Se recomienda leer detenidamente la siguiente información antes de implementar Data Box y volver a consultarla según sea necesario durante la implementación y el posterior funcionamiento.

Los requisitos del sistema incluyen:

* **Requisitos de software de los hosts que se conectan a Data Box**: describe las plataformas compatibles, los exploradores de la interfaz de usuario web local, los clientes SMB y los requisitos adicionales de los hosts que se pueden conectar a Data Box.
* **Requisitos de red de Data Box**: proporciona información acerca de los requisitos de red para el funcionamiento del óptimo de Data Box.


## <a name="software-requirements"></a>Requisitos de software

Los requisitos de software incluyen información acerca de los sistemas operativos y exploradores compatibles con la interfaz de usuario web local y los clientes SMB.

### <a name="supported-operating-systems-for-clients"></a>Sistemas operativos compatibles para clientes

Aquí se proporciona una lista de los sistemas operativos compatibles para la operación de copia de datos a través de los clientes conectados al dispositivo Data Box.

| **Sistema operativo** | **Versiones** | 
| --- | --- | 
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 | 
|  Windows |7, 8, 10 | 
|Linux    |         |

### <a name="supported-file-systems-for-linux-clients"></a>Sistemas de archivos compatibles para clientes de Linux

| **Protocolos** | **Versiones** | 
| --- | --- | 
| SMB |2.X y posterior |
| NFS | Todas las versiones hasta la 4.1, inclusive|

### <a name="supported-storage-accounts"></a>Cuentas de almacenamiento admitidas

Aquí se proporciona una lista de los tipos de cuentas de almacenamiento compatibles y de almacenamiento para el dispositivo Data Box. Para una lista completa de todos los tipos distintos de cuentas de almacenamiento y todas sus funcionalidades, consulte [Tipos de cuentas de almacenamiento](/azure/storage/common/storage-account-overview#types-of-storage-accounts).

| **Tipos de cuenta de almacenamiento/almacenamiento compatible** | **Blob en bloques** |**Blob en páginas*** |**Archivos de Azure** |**Notas**|
| --- | --- | -- | -- | -- |
| Estándar clásico | Y | Y | Y |
| Estándar de uso general v1  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v1  |  | Y| | |
| Estándar de uso general v2  | Y | Y | Y | Se admiten frecuentes y esporádicos.|
| Premium de uso general v2  |  |Y | | |
| Estándar de Blob Storage |Y | | |Se admiten frecuentes y esporádicos. |

\* *- Los datos cargados en blobs en páginas deben tener 512 bytes alineados como discos duros virtuales.*

>[!NOTE]
> No se admiten las cuentas de Azure Data Lake Storage Gen 2.


### <a name="supported-storage-types"></a>Tipos de almacenamiento admitidos

Aquí se proporciona una lista de los tipos de almacenamiento compatibles con el dispositivo Data Box.

| **Formato de archivo** | **Notas** |
| --- | --- |
| Blob en bloques de Azure | |
| Blob en páginas de Azure  | Los datos deben ser de 512 bytes alineados.|
| Archivos de Azure | |


### <a name="supported-web-browsers"></a>Exploradores web compatibles

Presentamos una lista de los exploradores web compatibles con la interfaz de usuario web local.

| **Browser** | **Versiones** | **Requisitos/notas adicionales** |
| --- | --- | --- |
| Google Chrome |La versión más reciente |Probado con Chrome|
| Microsoft Edge |La versión más reciente | |
| FireFox | La versión más reciente | Probado con FireFox|
| Internet Explorer |La versión más reciente |Si no puede iniciar sesión, compruebe si están habilitados tanto Javascript como las cookies. Para habilitar el acceso de la interfaz de usuario, agregue la dirección IP del dispositivo a **Acciones de privacidad**, con el fin de que el dispositivo pueda acceder a las cookies. |


## <a name="networking-requirements"></a>Requisitos de red

El centro de datos debe tener una red de alta velocidad. Es muy recomendable tener una conexión de 10 GbE como mínimo. Si no hay disponible una conexión 10 GbE, se puede usar un vínculo de datos de 1 GbE para copiar datos, pero la velocidad de copia resultará afectada.

## <a name="next-step"></a>Paso siguiente

* [Implementación de Azure Data Box](data-box-deploy-ordered.md)

