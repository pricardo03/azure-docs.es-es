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
ms.date: 09/06/2018
ms.author: alkohli
ms.openlocfilehash: aaa4e4bb24ca42adb9d283e6286dbef879bcb1ea
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44299855"
---
# <a name="azure-data-box-disk-system-requirements-preview"></a>Requisitos del sistema de Azure Data Box Disk (versión preliminar)

En este artículo se describen los requisitos del sistema importantes de la solución Microsoft Azure Data Box Disk y de los clientes de que se conectan a Data Box Disk. Es recomendable que revise cuidadosamente la siguiente información antes de implementar Data Box Disk y que luego la consulte según sea necesario durante la implementación y el funcionamiento posterior.

> [!IMPORTANT]
> Data Box Disk se encuentra en versión preliminar. Revise los [términos de uso para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solución. 

Los requisitos del sistema incluyen las plataformas admitidas para los clientes que se conectan a discos, cuentas de almacenamiento admitidas y tipos de almacenamiento.


## <a name="supported-operating-systems-for-clients"></a>Sistemas operativos compatibles para clientes

Aquí se proporciona una lista de los sistemas operativos compatibles para la operación de desbloqueo de discos y copia de datos a través de los clientes conectados a Data Box Disk.

| **Sistema operativo** | **Versiones probadas** |
| --- | --- |
| Windows Server |2008 R2 SP1 <br> 2012 <br> 2012 R2 <br> 2016 |
| Windows |7, 8, 10 |
|Linux <br> <li> Ubuntu </li><li> Debian </li><li> Red Hat Enterprise Linux (RHEL) </li><li> CentOS| <br>14.04, 16.04, 18.04 <br> 8.11, 9 <br> 7.0 <br> 6.5, 6.9, 7.0, 7.5 |  

## <a name="other-required-software-for-windows-clients"></a>Otro software necesario para los clientes Windows

Para el cliente Windows, también se debe instalar lo siguiente.

| **Software**| **Versión** |
| --- | --- |
| Windows PowerShell |5.0 |
| .NET Framework |4.5.1 |
| Windows Management Framework |5.0|
| BitLocker| - |

## <a name="other-required-software-for-linux-clients"></a>Otro software necesario para los clientes Linux

Para el cliente Linux, el Conjunto de herramientas de Data Box Disk instala el siguiente software necesario:

- dislocker
- OpenSSL

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

