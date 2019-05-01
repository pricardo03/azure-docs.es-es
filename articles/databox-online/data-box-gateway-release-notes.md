---
title: Notas de la versión en Disponibilidad General de puerta de enlace de datos de cuadro de Azure | Microsoft Docs
description: Describe los problemas críticos y las soluciones para la puerta de enlace de cuadro de datos de Azure, ejecuta la versión de disponibilidad general.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/26/2019
ms.author: alkohli
ms.openlocfilehash: f4ee3a5bd754335ab1c7f124671e9c37307a6a28
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60754206"
---
# <a name="azure-data-box-edgeazure-data-box-gateway-general-availability-release-notes"></a>Notas de la versión de disponibilidad de datos cuadro de borde o Azure Data cuadro puerta de enlace General de Azure

## <a name="overview"></a>Información general

Las notas siguientes identifican los problemas críticos abiertos y los problemas resueltos para Disponibilidad General (GA) de la versión para el borde del cuadro de datos de Azure y la puerta de enlace de cuadro de datos de Azure.

Las notas de la versión se actualizan continuamente y se van agregando a medida que se descubren problemas críticos que requieren una solución alternativa. Antes de implementar la puerta de enlace de datos de cuadro de Edge/Data cuadro, revise cuidadosamente la información contenida en las notas.

Esta versión corresponde a las versiones de software:

- **Puerta de enlace de datos cuadro 1903 (1.5.814.447)**
- **Borde del cuadro datos 1903 (1.5.814.447)**


## <a name="whats-new"></a>Novedades

- **Nuevas imágenes de disco virtual** -nuevo disco VHDX y VMDK ahora están disponibles en el portal de Azure. Descargar estas imágenes para aprovisionar, configurar e implementar nuevos dispositivos de puerta de enlace de datos cuadro GA. Los dispositivos de Data Box Gateway creados en las versiones preliminares anteriores se pueden actualizar a esta versión. Para más información, consulte [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md).
- **Compatibilidad con NFS** -compatibilidad con NFS está actualmente en versión preliminar y está disponible para v3.0 y v4.1 los clientes que tienen acceso a los dispositivos de puerta de enlace de datos de cuadro de borde del cuadro de datos.
- **Resistencia de almacenamiento** -dispositivo de su borde del cuadro de datos puede resistir el error de un disco de datos con la característica de resistencia de almacenamiento. Esta funcionalidad actualmente está en su versión preliminar. Puede habilitar la resistencia de almacenamiento seleccionando la **resistentes** opción el **configuración de almacenamiento** local de interfaz de usuario web.


## <a name="known-issues-in-ga-release"></a>Problemas conocidos de la versión de GA

En la tabla siguiente proporciona un resumen de los problemas conocidos para la puerta de enlace de datos de cuadro ejecutando la versión.

| No. | Característica | Problema | Soluciones alternativas o comentarios |
| --- | --- | --- | --- |
| **1.** |Tipos de archivo | No se admiten los siguientes tipos de archivo: archivos, bloquear los archivos, sockets, canalizaciones, los vínculos simbólicos de caracteres.  |Al copiar estos archivos, se crean archivos de longitud 0 en el recurso compartido NFS. Estos archivos permanecen en estado de error y también se notifican en *error.xml*. <br> Como resultado de los vínculos simbólicos a directorios, estos nunca se marcan como sin conexión. Por este motivo, no puede ver la cruz gris que indica que los directorios están disponibles sin conexión y que todo el contenido asociado se cargó por completo en Azure. |
| **2.** |Eliminación | Debido a un error en esta versión, si se elimina un recurso compartido NFS, es posible que el recurso compartido no se pueda eliminar. El estado del recurso compartido será *Deleting* (Eliminando).  |Esto solo se produce cuando el recurso compartido usa un nombre de archivo no admitido. |
| **3.** |Copiar | La copia de datos produce un error con el mensaje Error:  no se pudo completar la operación solicitada por una limitación del sistema de archivos.  |No se admite el Stream de datos alternativa (ADS) asociada con el tamaño de archivo mayor que 128 KB.   |


## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md)
- [Prepararse para implementar Azure datos de cuadro de borde](data-box-edge-deploy-prep.md).
