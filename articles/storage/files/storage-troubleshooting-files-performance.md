---
title: Rendimiento de los archivos de Azure Guía de solución de problemas
description: Problemas de rendimiento con recursos compartidos de archivos de Azure premium (versión preliminar) y soluciones asociadas conocidos.
services: storage
author: gunjanj
ms.service: storage
ms.topic: article
ms.date: 04/25/2019
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: 5ae0bb736a7cc0bbc38df5905abc5d8a71f60eb9
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190047"
---
# <a name="troubleshoot-azure-files-performance-issues"></a>Solucionar problemas de rendimiento de Azure Files

En este artículo se enumera algunos problemas comunes relacionados con los recursos compartidos de archivos de Azure premium (versión preliminar). Proporciona posibles causas y soluciones alternativas cuando se producen estos problemas.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Alta latencia, rendimiento bajo y problemas de rendimiento general

### <a name="cause-1-share-experiencing-throttling"></a>Causa 1: Compartir la limitación está experimentando

La cuota predeterminada en un recurso compartido es 100 GB, que proporciona la línea base 100 IOPS (con un potencial que aumentar el rendimiento de hasta 300 durante una hora). Para obtener más información sobre el aprovisionamiento y su relación con la e/s por segundo, vea el [aprovisionar recursos compartidos](storage-files-planning.md#provisioned-shares) sección de la Guía de planeación.

Para confirmar si se está limitando el recurso compartido, puede aprovechar las métricas de Azure en el portal.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

1. Seleccione **todos los servicios** y, a continuación, busque **métricas**.

1. Seleccione **Métricas**.

1. Seleccione la cuenta de almacenamiento como el recurso.

1. Seleccione **archivo** como el espacio de nombres de métrica.

1. Seleccione **transacciones** como la métrica.

1. Agregar un filtro para **ResponseType** y para comprobar si las solicitudes tienen un código de respuesta de **SuccessWithThrottling**.

![Opciones de las métricas de recursos compartidos de archivos de premium](media/storage-troubleshooting-premium-fileshares/metrics.png)

### <a name="solution"></a>Solución

- Aumento comparten la capacidad aprovisionada mediante la especificación de una cuota mayor en el recurso compartido.

### <a name="cause-2-metadatanamespace-heavy-workload"></a>Causa 2: Cargas de trabajo pesadas y espacio de nombres de metadatos

Si la mayoría de las solicitudes es metadatos centrado en (por ejemplo, createfile/openfile/closefile/queryinfo/querydirectory), la latencia será peor en comparación con las operaciones de lectura y escritura.

Para confirmar que si la mayoría de las solicitudes son centrado en metadatos, puede usar los mismos pasos que antes. Excepto en lugar de agregar un filtro para **ResponseType**, agregar un filtro para **nombre de la API**.

![Filtrar por nombre de la API en las métricas](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>Solución alternativa

- Compruebe si se puede modificar la aplicación para reducir el número de operaciones de metadatos.

### <a name="cause-3-single-threaded-application"></a>Causa 3: Aplicación de un único subproceso

Si la aplicación se usa el cliente es un único subproceso, esto puede dar lugar IOPS/rendimiento significativamente menor que el máximo posible según el tamaño del recurso compartido aprovisionado.

### <a name="solution"></a>Solución

- Aumentar el paralelismo de la aplicación si aumenta el número de subprocesos.
- Cambiar a las aplicaciones donde es posible paralelismo. Por ejemplo, para las operaciones de copia, los clientes podrían utilizar AzCopy o RoboCopy desde clientes de Windows o el **paralelo** comando en los clientes de Linux.

## <a name="very-high-latency-for-requests"></a>Latencia muy alta para las solicitudes

### <a name="cause"></a>Causa

Se encontró la máquina virtual de cliente en una región distinta a recurso compartido de archivos de premium.

### <a name="solution"></a>Solución

- Ejecute la aplicación desde una máquina virtual que se encuentra en la misma región que el recurso compartido de archivos premium.

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>No se puede lograr un rendimiento máximo compatible con la red de cliente

Una posible causa de esto es una falta fo SMB multicanal soporte. Actualmente, los recursos compartidos de archivos de Azure solo admiten canal único, por lo que hay solo una conexión desde la máquina virtual del cliente al servidor. Esta conexión solo se tiende a un solo núcleo en el cliente de máquina virtual, por lo que el rendimiento máximo alcanzable desde una máquina virtual está limitado por un solo núcleo.

### <a name="workaround"></a>Solución alternativa

- Obtención de una máquina virtual con un núcleo más grande puede ayudar a mejorar el rendimiento.
- Ejecutar la aplicación cliente desde varias máquinas virtuales, aumentará el rendimiento.
- Siempre que sea posible, use las API de REST.

## <a name="throughput-on-linux-clients-is-significantly-lower-when-compared-to-windows-clients"></a>Rendimiento en los clientes de Linux es significativamente menor en comparación con los clientes de Windows.

### <a name="cause"></a>Causa

Se trata de un problema conocido con la implementación de cliente SMB en Linux.

### <a name="workaround"></a>Solución alternativa

- Distribuir la carga entre varias máquinas virtuales
- En la misma máquina virtual, use varios puntos de montaje con **nosharesock** opción y propagación de la carga entre estos puntos de montaje.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Latencias altas para metadatos cargas de trabajo que implican las operaciones de apertura y cierre extenso.

### <a name="cause"></a>Causa

Falta de compatibilidad para las concesiones de directorio.

### <a name="workaround"></a>Solución alternativa

- Si es posible, evite excesivo apertura o cierre del identificador en el mismo directorio en un breve período de tiempo.
- Las máquinas virtuales de Linux, aumentar el tiempo de espera de caché de entrada de directorio mediante la especificación de **actimeo =<sec>**  como una opción de montaje. De forma predeterminada, es un segundo, por lo que podría ayudar un valor mayor, como tres o cinco.
- Las máquinas virtuales de Linux, actualice el kernel a 4.20 o superior.

## <a name="low-iops-on-centosrhel"></a>Baja tasa de IOPS en RHEL/CentOS

### <a name="cause"></a>Causa

Profundidad de E/S mayor que uno no se admite en RHEL/CentOS.

### <a name="workaround"></a>Solución alternativa

- Actualización a 8 de CentOS o RHEL 8.
- Cambie a Ubuntu.

## <a name="jitterysaw-tooth-pattern-for-iops"></a>Patrón nerviosos/Sierra de e/s por segundo

### <a name="cause"></a>Causa

Aplicación cliente supera constantemente la línea de base de e/s por segundo. Actualmente, no hay ningún lado del servicio de suavizado de la carga de solicitudes, por lo que si el cliente supera la línea de base de e/s por segundo, se verá limitada por el servicio. Limitación puede provocar que el cliente experimenta un patrón de e/s por segundo nerviosos/Sierra. En este caso, el promedio IOPS que se logra mediante el cliente podría ser inferior a la línea de base de e/s por segundo.

### <a name="workaround"></a>Solución alternativa

- Reducir la carga de solicitud de la aplicación cliente, por lo que el recurso compartido no se verá limitado.
- Aumente la cuota del recurso compartido para que el recurso compartido no se verá limitado.

## <a name="excessive-directoryopendirectoryclose-calls"></a>Un exceso de llamadas DirectoryOpen/DirectoryClose

### <a name="cause"></a>Causa

Si el número de llamadas DirectoryOpen/DirectoryClose se encuentre entre las llamadas de API principales y no espera a que el cliente para realizar muchas llamadas, podrían ser un problema con el antivirus instalado en la máquina virtual de cliente de Azure.

### <a name="workaround"></a>Solución alternativa

- Una corrección para este problema está disponible en el [abril Platform Update para Windows](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform).

## <a name="file-creation-is-slower-than-expected"></a>Creación de archivos es más lenta de lo esperado

### <a name="cause"></a>Causa

Las cargas de trabajo que se basan en la creación de un gran número de archivos no verán una diferencia importante entre el rendimiento de premium recursos compartidos de archivos y recursos compartidos de archivos estándar.

### <a name="workaround"></a>Solución alternativa

- Ninguno.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Rendimiento lento de Windows 8.1 o Server 2012 R2

### <a name="cause"></a>Causa

Mayor que espera la latencia de acceso a los archivos de Azure para cargas de trabajo intensivas de E/S.

### <a name="workaround"></a>Solución alternativa

- Instalar el disponibles [revisión](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1).