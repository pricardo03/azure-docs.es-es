---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 12/26/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: a67ad4c5010cf93ff55123013a35c697ce5971f8
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597887"
---
Los recursos compartidos de archivos de Azure se implementan en *cuentas de almacenamiento*, que son objetos de nivel superior que representan un grupo compartido de almacenamiento. Este grupo de almacenamiento se puede usar para implementar varios recursos compartidos de archivos, así como otros recursos de almacenamiento, tales como contenedores de blobs, colas o tablas. Todos los recursos de almacenamiento que se implementan en una cuenta de almacenamiento comparten los límites que se aplican a esa cuenta de almacenamiento. Para ver los límites actuales de una cuenta de almacenamiento, consulte [Objetivos de escalabilidad y rendimiento de Azure Files](../articles/storage/files/storage-files-scale-targets.md).

Hay dos tipos principales de cuentas de almacenamiento que se usarán para las implementaciones de Azure Files: 
- **Cuentas de almacenamiento de uso general, versión 2 (GPv2)** : Las cuentas de almacenamiento de GPv2 permiten implementar recursos compartidos de archivos de Azure en hardware estándar o basado en disco duro (HDD). Además de almacenar recursos compartidos de archivos de Azure, las cuentas de almacenamiento de GPv2 pueden almacenar otros recursos de almacenamiento, como contenedores de blobs, colas o tablas. 
- **Cuentas de almacenamiento FileStorage**: Las cuentas de almacenamiento FileStorage permiten implementar recursos compartidos de archivos de Azure en hardware prémium o basado en unidades de estado sólido (SSD). Las cuentas FileStorage solo se pueden usar para almacenar recursos compartidos de archivos de Azure. No se puede implementar ningún otro recurso de almacenamiento (contenedores de blobs, colas, tablas, etc.) en una cuenta FileStorage.

Existen otros tipos de cuenta de almacenamiento que puede encontrar en el Azure Portal, PowerShell o la CLI. Dos tipos de cuentas de almacenamiento, BlockBlobStorage y BlobStorage, no pueden contener recursos compartidos de archivos de Azure. Los otros dos tipos de cuenta de almacenamiento que puede ver son las cuentas de almacenamiento clásico y de uso general de la versión 1 (GPv1), y ambas pueden contener recursos compartidos de archivos de Azure. Aunque las cuentas de almacenamiento clásico y de GPv1 pueden contener recursos compartidos de archivos de Azure, la mayoría de las nuevas características de Azure Files solo están disponibles en las cuentas de almacenamiento de GPv2 y FileStorage. Por lo tanto, se recomienda usar solo las cuentas de almacenamiento de GPv2 y FileStorage para las nuevas implementaciones, así como actualizar las cuentas de almacenamiento de GPv1 y clásico si ya existen en su entorno.  