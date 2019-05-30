---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 93f4f74d435cc14130668da102d1246c5fad5872
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66238234"
---
| Identificador de límites | Límite | Comentarios |
| --- | --- | --- |
| Número máximo de credenciales de la cuenta de almacenamiento |64 | |
| Número máximo de contenedores de volúmenes |64 | |
| Número máximo de volúmenes |255 | |
| Número máximo de programaciones por plantilla de ancho de banda |168 |Una programación para cada hora, cada día de la semana. |
| Tamaño máximo de un volumen en capas en dispositivos físicos |64 TB para StorSimple 8100 y StorSimple 8600 |StorSimple 8100 y StorSimple 8600 son dispositivos físicos. |
| Tamaño máximo de un volumen en capas en dispositivos virtuales de Azure |30 TB para 8010 StorSimple <br></br> 64 TB para 8020 de StorSimple |StorSimple 8010 y 8020 de StorSimple son dispositivos virtuales de Azure que utilizan el almacenamiento estándar y Premium storage, respectivamente. |
| Tamaño máximo de un volumen anclado localmente en dispositivos físicos |9 TB para StorSimple 8100 <br></br> 24 TB para StorSimple 8600 |StorSimple 8100 y StorSimple 8600 son dispositivos físicos. |
| Número máximo de conexiones iSCSI |512 | |
| Número máximo de conexiones de iSCSI de iniciadores |512 | |
| Número máximo de registros de control de acceso por dispositivo |64 | |
| Número máximo de volúmenes por directiva de copia de seguridad |24 | |
| Número máximo de copias de seguridad por directiva de copia de seguridad |64 | |
| Número máximo de programaciones por directiva de copia de seguridad |10 | |
| Número máximo de instantáneas de cualquier tipo que se pueden retener por volumen |256 |Esta cantidad incluye instantáneas locales y de instantáneas en la nube. |
| Número máximo de instantáneas que pueden estar presentes en cualquier dispositivo |10 000 | |
| Número máximo de volúmenes que se pueden procesar en paralelo para copia de seguridad, restauración o clonación |16 |<ul><li>Si hay más de 16 volúmenes, se procesan secuencialmente como las ranuras de procesamiento estén disponibles.</li><li>No se puede producir nuevas copias de seguridad de un clonado o un volumen en capas restaurado hasta que finalice la operación. Para un volumen local, se permiten las copias de seguridad después de que el volumen está en línea.</li></ul> |
| Tiempo de recuperación de la restauración y la clonación |< 2 minutos |<ul><li>El volumen está disponible en dos minutos de una operación de clonación, independientemente del tamaño del volumen o de restauración.</li><li>El rendimiento del volumen inicialmente podría ser más lento que la normal, ya que la mayoría de los datos y metadatos todavía residen en la nube. Puede aumentar el rendimiento como flujos de datos desde la nube al dispositivo de StorSimple.</li><li>El tiempo total para descargar los metadatos depende del tamaño del volumen asignado. Los metadatos se traen automáticamente al dispositivo en segundo plano a una velocidad de 5 minutos por cada TB de datos de volumen asignado. Esta velocidad puede verse afectada por el ancho de banda de Internet a la nube.</li><li>La operación de restauración o clonación se completará cuando todos los metadatos estén en el dispositivo.</li><li>Operación de clonación completada completamente o no se puede realizar operaciones de copia de seguridad hasta que la restauración. |
| Restauración del tiempo de recuperación de volúmenes anclados localmente |< 2 minutos |<ul><li>El volumen estará disponible en dos minutos de operación de restauración, independientemente del tamaño del volumen.</li><li>El rendimiento del volumen inicialmente podría ser más lento que la normal, ya que la mayoría de los datos y metadatos todavía residen en la nube. Puede aumentar el rendimiento como flujos de datos desde la nube al dispositivo de StorSimple.</li><li>El tiempo total para descargar los metadatos depende del tamaño del volumen asignado. Los metadatos se traen automáticamente al dispositivo en segundo plano a una velocidad de 5 minutos por cada TB de datos de volumen asignado. Esta velocidad puede verse afectada por el ancho de banda de Internet a la nube.</li><li>A diferencia de los volúmenes en capas, si no hay volúmenes anclados localmente, los datos del volumen también se descargan localmente en el dispositivo. La operación de restauración finaliza cuando todos los datos de volúmenes se transfieren al dispositivo.</li><li>Las operaciones de restauración pueden ser largos y el tiempo total para completar la restauración dependerá del tamaño del volumen local aprovisionado, el ancho de banda de Internet y los datos existentes en el dispositivo. Se permiten las operaciones de copia de seguridad en el volumen anclado localmente mientras la operación de restauración está en curso. |
| Disponibilidad de restauración fina |Última conmutación por error | |
| Rendimiento de lectura y escritura máximas de cliente, cuando se atiende desde la capa SSD * |920/720 MB/s con una única interfaz de red Ethernet de 10 gigabits |Hasta dos veces con MPIO y dos interfaces de red. |
| Rendimiento de lectura y escritura máximas de cliente, cuando se atiende desde la capa HDD * |120/250 MB/seg. | |
| Rendimiento de lectura y escritura máximas de cliente, cuando se atiende desde la capa de nube * |11/41 MB/seg. |El rendimiento de lectura depende de que los clientes generen y mantengan una profundidad de cola de E/S suficiente. |

&#42;Rendimiento máximo por tipo de E/S se midió con escenarios de escritura del 100% y lectura del 100 por ciento. El rendimiento real puede ser inferior y depende de E/S mezclar y las condiciones de red.

