---
title: 'Almacenamiento de Azure Stack: Diferencias y consideraciones | Microsoft Docs'
description: Comprender las diferencias entre el almacenamiento de Azure Stack Storage y Azure Storage, junto con las consideraciones de implementación de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/05/2018
ms.author: mabrigg
ms.reviwer: xiaofmao
ms.openlocfilehash: 14e32bdfcde6969b820c0950d59bd5cf946a51e6
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48802328"
---
# <a name="azure-stack-storage-differences-and-considerations"></a>Almacenamiento de Azure Stack: diferencias y consideraciones

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

El almacenamiento de Azure Stack es el conjunto de servicios de almacenamiento en la nube de Microsoft Azure Stack. El almacenamiento de Azure Stack proporciona blobs, tablas, colas y funcionalidad de administración de cuentas con una semántica coherente con Azure.

En este artículo se resumen las diferencias entre Azure Stack Storage y los servicios de Azure Storage. También se muestran los aspectos que se deben tener en cuenta al implementar Azure Stack. Para información sobre las diferencias generales entre Azure global y Azure Stack, consulte el tema [Consideraciones clave](azure-stack-considerations.md).

## <a name="cheat-sheet-storage-differences"></a>Hoja de referencia rápida: Diferencias de almacenamiento

| Característica | Azure (global) | Azure Stack |
| --- | --- | --- |
|File Storage|Recursos compartidos de archivos SMB basado en la nube admitidos|Todavía no se admite
|Cifrado del servicio Azure Storage para datos en reposo|Cifrado de AES de 256 bits|Cifrado AES de 128 bits de BitLocker
|Tipo de cuenta de almacenamiento|Cuentas de Azure Blob Storage y de uso general|Solo para uso general.
|Opciones de replicación|Almacenamiento con redundancia local, almacenamiento con redundancia geográfica, almacenamiento con redundancia geográfica con acceso de lectura y almacenamiento con redundancia de zona|Almacenamiento con redundancia local.
|Premium Storage|Totalmente compatible|Se pueden aprovisionar, pero no hay límite de rendimiento o garantía.
|Discos administrados|Premium y estándar admitidos|Se admite cuando se usa la versión 1808 o posterior.
|Nombre de blob|1 024 caracteres (2 048 bytes)|880 caracteres (1 760 bytes)
|Tamaño máximo de blob en bloque|4,75 TB (100 MB x 50 000 bloques)|4,75 TB (100 MB x 50 000 bloques) para la actualización 1802 o versiones más recientes. 50 000 x 4 MB (aproximadamente 195 GB) para las versiones anteriores.
|Copia de instantáneas incrementales del blob de página|Blobs en páginas de Azure estándar y premium admitidos|Todavía no se admite.
|Niveles de almacenamiento para Blob Storage|Niveles de almacenamiento de acceso frecuente, de acceso esporádico y de acceso de archivo.|Todavía no se admite.
Eliminación temporal para Blob Storage|Vista previa|Todavía no se admite.
|Tamaño máximo de blob en página|8 TB|1 TB
|Tamaño de página de blob en página|512 bytes|4 KB
|Clave de partición de tabla y tamaño de clave de fila|1 024 caracteres (2 048 bytes)|400 caracteres (800 bytes)
|Instantánea de blob|No se limita el número máximo de instantáneas de un blob.|El número máximo de instantáneas de un blob es 1 000.|

También hay diferencias en las métricas de almacenamiento:

* Los datos de transacción de las métricas de almacenamiento no distinguirán el ancho de banda de red interna o externa.
* Los datos de transacción de las métricas de almacenamiento no incluyen el acceso de la máquina virtual a los discos montados.

## <a name="api-version"></a>Versión de API

Solo las siguientes versiones son compatibles con Azure Stack Storage:

API de los servicios de Azure Storage:

Actualización 1802 o versiones más recientes:

 - [2017-04-17](https://docs.microsoft.com/rest/api/storageservices/version-2017-04-17)
 - [2016-05-31](https://docs.microsoft.com/rest/api/storageservices/version-2016-05-31)
 - [2015-12-11](https://docs.microsoft.com/rest/api/storageservices/version-2015-12-11)
 - [2015-07-08](https://docs.microsoft.com/rest/api/storageservices/version-2015-07-08)
 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

Versiones anteriores:

 - [2015-04-05](https://docs.microsoft.com/rest/api/storageservices/version-2015-04-05)

API de administración de los servicios de Azure Storage:

 - [2015-05-01-preview](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2015-06-15](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)
 - [2016-01-01](https://docs.microsoft.com/rest/api/storagerp/?redirectedfrom=MSDN)

## <a name="sdk-versions"></a>Versiones del SDK

Las siguientes bibliotecas de cliente son compatibles con el almacenamiento de Azure Stack:

| Biblioteca de cliente | Versión compatible de Azure Stack | Vínculo                                                                                                                                                                                                                                                                                                                                     | Especificación de punto de conexión       |
|----------------|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| .NET           | De la 6.2.0 a la 8.7.0.          | Paquete NuGet:<br>https://www.nuget.org/packages/WindowsAzure.Storage/<br> <br>Versión de GitHub:<br>https://github.com/Azure/azure-storage-net/releases                                                                                                                                                                                    | archivo app.config              |
| Java           | De la 4.1.0 a la 6.1.0           | Paquete Maven:<br>http://mvnrepository.com/artifact/com.microsoft.azure/azure-storage<br> <br>Versión de GitHub:<br>https://github.com/Azure/azure-storage-java/releases                                                                                                                                                                    | Configuración de la cadena de conexión      |
| Node.js        | De la 1.1.0 a la 2.7.0           | Vínculo NPM:<br>https://www.npmjs.com/package/azure-storage<br>(Por ejemplo: ejecute "npm install azure-storage@2.7.0")<br> <br>Versión de GitHub:<br>https://github.com/Azure/azure-storage-node/releases                                                                                                                                         | Declaración de instancia de servicio |
| C++            | De la 2.4.0 a la 3.1.0           | Paquete NuGet:<br>https://www.nuget.org/packages/wastorage.v140/<br> <br>Versión de GitHub:<br>https://github.com/Azure/azure-storage-cpp/releases                                                                                                                                                                                          | Configuración de la cadena de conexión      |
| PHP            | De la 0.15.0 a la 1.0.0          | Versión de GitHub:<br>https://github.com/Azure/azure-storage-php/releases<br> <br>Instalación a través de Composer (consulte los detalles a continuación)                                                                                                                                                                                                                  | Configuración de la cadena de conexión      |
| Python         | De la 0.30.0 a la 1.0.0          | Versión de GitHub:<br>https://github.com/Azure/azure-storage-python/releases                                                                                                                                                                                                                                                                | Declaración de instancia de servicio |
| Ruby           | De la 0.12.1 a la 1.0.1          | Paquete de RubyGems:<br>Común:<br>https://rubygems.org/gems/azure-storage-common/<br>Blob: https://rubygems.org/gems/azure-storage-blob/<br>Queue: https://rubygems.org/gems/azure-storage-queue/<br>Table: https://rubygems.org/gems/azure-storage-table/<br> <br>Versión de GitHub:<br>https://github.com/Azure/azure-storage-ruby/releases | Configuración de la cadena de conexión      |

## <a name="next-steps"></a>Pasos siguientes

* [Empezar a trabajar con herramientas de desarrollo de Azure Stack Storage](azure-stack-storage-dev.md)
* [Introducción a Azure Stack Storage](azure-stack-storage-overview.md)
