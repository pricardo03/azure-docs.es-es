---
title: Requisitos del sistema de Microsoft Azure Data Box Disk | Microsoft Docs
description: Obtenga más información sobre los requisitos de software y red de Azure Data Box Disk
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/10/2018
ms.author: alkohli
ms.openlocfilehash: 7138fa70c8b5615ad84196703f3bd76009ba5811
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39011493"
---
# <a name="azure-data-box-disk-system-requirements-preview"></a>Requisitos del sistema de Azure Data Box Disk (versión preliminar)

En este artículo se describen los requisitos del sistema importantes de la solución Microsoft Azure Data Box Disk y de los clientes de que se conectan a Data Box Disk. Es recomendable que revise cuidadosamente la siguiente información antes de implementar Data Box Disk y que luego la consulte según sea necesario durante la implementación y el funcionamiento posterior.

> [!IMPORTANT]
> Data Box Disk se encuentra en versión preliminar. Revise los [términos de uso para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solución. 

Los requisitos del sistema incluyen las plataformas admitidas para los clientes que se conectan a discos, cuentas de almacenamiento admitidas y tipos de almacenamiento.


## <a name="supported-operating-systems-for-clients"></a>Sistemas operativos compatibles para clientes

Aquí se proporciona una lista de los sistemas operativos compatibles para la operación de desbloqueo de discos y copia de datos a través de los clientes conectados a Data Box Disk.

| **Sistema operativo o plataforma** | **Versiones** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows |7, 8, 10 |
| Windows PowerShell |4.0 |
| .NET Framework |4.5. |

> [!NOTE] 
> BitLocker debe habilitarse en los clientes que ejecutan la herramienta de desbloqueo de discos y se usan para copiar los datos.


## <a name="supported-storage-accounts"></a>Cuentas de almacenamiento admitidas

Aquí se proporciona una lista de los tipos de almacenamiento compatibles para Data Box Disk.

| **Cuenta de almacenamiento** | **Notas** |
| --- | --- |
| Clásico | Estándar |
| Uso general  |Estándar; se admiten V1 y V2. Se admiten niveles frecuentes y esporádicos. |


## <a name="supported-storage-types"></a>Tipos de almacenamiento admitidos

Aquí se proporciona una lista de los tipos de almacenamiento compatibles para Data Box Disk.

| **Formato de archivo** | **Notas** |
| --- | --- |
| Blob en bloques de Azure | |
| Blob en páginas de Azure  | |


## <a name="next-step"></a>Paso siguiente

* [Implementación de Azure Data Box Disk](data-box-disk-deploy-ordered.md)

