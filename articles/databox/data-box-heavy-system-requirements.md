---
title: Requisitos del sistema intensivo de datos de cuadro de Microsoft Azure | Microsoft Docs
description: Obtenga información sobre el software y los requisitos de red para su pesado de cuadro de datos de Azure
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: article
ms.date: 05/22/2019
ms.author: alkohli
ms.openlocfilehash: 8cd2f96954cde367eb99d89e89bcf672b53dd590
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248000"
---
# <a name="azure-data-box-heavy-system-requirements"></a>Requisitos del sistema de pesado de cuadro de datos de Azure

En este artículo se describe los requisitos de sistema importante para el dispositivo cuadro intensivo de datos de Azure y para los clientes que se conectan al dispositivo. Se recomienda que revise cuidadosamente la información antes de implementar la pesada de cuadro de datos y luego la consulte según sea necesario durante la implementación y las operaciones subsiguientes.

Los requisitos del sistema incluyen:

* **Requisitos de software para hosts que se conectan a intensivo de datos de cuadro** -se describen las plataformas compatibles, los exploradores de la interfaz de usuario web local, los clientes SMB y los requisitos adicionales para hosts que se pueden conectar a Data Box.
* **Requisitos de red para la pesada de datos de cuadro** -proporciona información sobre los requisitos de red para el funcionamiento óptimo del dispositivo cuadro intensivo de datos.

## <a name="software-requirements"></a>Requisitos de software

Los requisitos de software incluyen información acerca de los sistemas operativos y exploradores compatibles con la interfaz de usuario web local y los clientes SMB.

### <a name="supported-operating-systems-for-clients"></a>Sistemas operativos compatibles para clientes

[!INCLUDE [data-box-supported-os-clients](../../includes/data-box-supported-os-clients.md)]

### <a name="supported-file-systems-for-linux-clients"></a>Sistemas de archivos compatibles para clientes de Linux

[!INCLUDE [data-box-supported-file-systems-clients](../../includes/data-box-supported-file-systems-clients.md)]

### <a name="supported-storage-accounts"></a>Cuentas de almacenamiento admitidas

[!INCLUDE [data-box-supported-storage-accounts](../../includes/data-box-supported-storage-accounts.md)]

### <a name="supported-storage-types"></a>Tipos de almacenamiento admitidos

[!INCLUDE [data-box-supported-storage-types](../../includes/data-box-supported-storage-types.md)]

### <a name="supported-web-browsers"></a>Exploradores web compatibles

[!INCLUDE [data-box-supported-web-browsers](../../includes/data-box-supported-web-browsers.md)]

## <a name="networking-requirements"></a>Requisitos de red

El centro de datos debe tener una red de alta velocidad. Para mayor velocidad de copia, se pueden usar dos conexiones 40 GbE en paralelo (uno por nodo). Si no tiene 40-GbE disponible, se recomienda que tiene al menos dos conexiones de 10 GbE (una por nodo).

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de Azure Data Box](data-box-deploy-ordered.md)
