---
title: Asignación de una estructura de carpetas a una topología de Azure File Sync
description: Asigne una estructura de archivos y carpetas existente a recursos compartidos de archivos de Azure para su uso con Azure File Sync. Un bloque de texto común, compartido entre los documentos de migración.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 44fb95de88cc86c802e5ba72f0b5379b8708c506
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209323"
---
En este paso, está evaluando cuántos recursos compartidos de archivos de Azure necesita. Una sola instancia de Windows Server (o clúster) puede sincronizar hasta 30 recursos compartidos de archivos de Azure.

Es posible que tenga más carpetas en StorSimple que actualmente comparte localmente como recursos compartidos de SMB en sus usuarios y aplicaciones. Lo más sencillo sería prever un recurso compartido local para una asignación 1:1 a un recurso compartido de archivos de Azure. Si este número es pequeño, es decir, un valor inferior a 30 para una única instancia de Windows Server, o si tiene pensado tener dos instancias de Windows Server (60), etc., se recomienda una asignación 1:1.

Si tiene más de 30 recursos compartidos, a menudo no es necesario asignar un recurso compartido local 1:1 a un recurso compartido de archivos de Azure.
Considere las opciones siguientes:

#### <a name="share-grouping"></a>Agrupación de recursos compartidos

Por ejemplo, si el Departamento de RR. HH. tiene un total de 15 recursos compartidos, podría considerar la posibilidad de almacenar todos los datos de recursos humanos en un solo recurso compartido de archivos de Azure. Almacenar varios recursos compartidos locales en un recurso compartido de archivos de Azure no le impide crear los 15 recursos compartidos de SMB habituales en la instancia local de Windows Server. Solo significa que organiza las carpetas raíz de estos 15 recursos compartidos como subcarpetas en una carpeta común. A continuación, sincronizará esta carpeta común con un recurso compartido de archivos de Azure. De este modo, solo se necesita un único recurso compartido de archivos de Azure en la nube para este grupo de recursos compartidos locales.

#### <a name="volume-sync"></a>Sincronización de volúmenes

Azure File Sync admite la sincronización de la raíz de un volumen con un recurso compartido de archivos de Azure.
Si sincroniza la carpeta raíz, todas las subcarpetas y archivos terminarán en el mismo recurso compartido de archivos de Azure.

#### <a name="other-best-practices-to-consider"></a>Otros procedimientos recomendados a tener en cuenta

Aparte del límite de sincronización de 30 recursos compartidos de archivos de Azure por servidor, la principal consideración es la eficacia de la sincronización.

Cuando hay varios recursos compartidos en el servidor, donde cada uno se sincroniza con su propio recurso compartido de archivos de Azure, la sincronización puede funcionar en paralelo para todos ellos. El vector de escala no es el tamaño de todos los archivos en un ámbito de sincronización, es el número de elementos (archivos y carpetas) que necesitan procesamiento.

Un solo recurso compartido de archivos de Azure puede contener hasta 100 TiB.
Azure File Sync admite la sincronización de hasta 100 millones elementos por recurso compartido de archivos de Azure.

La sincronización del volumen raíz no siempre será la mejor respuesta. La sincronización de varias ubicaciones presenta ventajas: ayuda a mantener un menor número de elementos por ámbito de sincronización. Configurar Azure File Sync con un número menor de elementos no es solo importante para la sincronización, sino que también beneficia la restauración en la nube a partir de copias de seguridad, así como aumenta la velocidad de la recuperación ante desastres en caso de que pierda el servidor y aprovisione uno nuevo que se conecte a los mismos recursos compartidos de archivos de Azure.

Use una combinación de los conceptos anteriores para ayudar a determinar cuántos recursos compartidos de archivos de Azure necesita, y qué partes de los datos de StorSimple existentes terminarán en cuál recurso compartido de archivos de Azure.

Cree una lista donde registre sus pensamientos, de modo que pueda consultarla en el paso siguiente. Mantenerse organizado durante este procedimiento es importante, ya que puede ser fácil perder detalles del plan de asignación al aprovisionar muchos recursos a la vez.
