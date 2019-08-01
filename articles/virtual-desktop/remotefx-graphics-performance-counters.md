---
title: Diagnóstico de problemas de rendimiento de gráficos en Escritorio remoto - Azure
description: Este artículo describe cómo usar contadores de gráficos de RemoteFX en sesiones del protocolo de escritorio remoto para diagnosticar problemas de rendimiento con gráficos en Windows Virtual Desktop.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 05/23/2019
ms.author: v-chjenk
ms.openlocfilehash: 0b4113f1e0024415135aa99d1fb4e881efe448a3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66499270"
---
# <a name="diagnose-graphics-performance-issues-in-remote-desktop"></a>Diagnóstico de problemas de rendimiento de gráficos en Escritorio remoto

Cuando el sistema no ofrece el rendimiento previsto, es importante identificar el origen del problema. Este artículo le ayuda a identificar y corregir cuellos de botella de rendimiento relacionados con gráficos durante sesiones del protocolo de escritorio remoto (RDP).

## <a name="find-your-remote-session-name"></a>Búsqueda del nombre de la sesión remota

Necesitará el nombre de la sesión remota para identificar los contadores de rendimiento de gráficos. Siga las instrucciones de esta sección para identificar el nombre de la sesión remota de la versión preliminar de Windows Virtual Desktop.

1. Abra el símbolo del sistema de Windows desde la sesión remota.
2. Ejecute el comando **qwinsta**.
    - Si la sesión se hospeda en una máquina virtual de sesión múltiple: El sufijo de cada nombre de contador es el mismo sufijo en el nombre de su sesión, como "rdp-tcp 37".
    - Si la sesión se hospeda en una máquina virtual compatible con unidades de procesamiento gráfico virtuales (vGPU): Los contadores se almacenan en el servidor en lugar de en la máquina virtual. Las instancias de contador incluyen el nombre de la máquina virtual en lugar del número en el nombre de la sesión, como "Win8 Enterprise VM".

>[!NOTE]
> Aunque los contadores tienen RemoteFX en el nombre, también incluyen gráficos de escritorio remoto en escenarios de vGPU.

## <a name="access-performance-counters"></a>Acceso a los contadores de rendimiento

Los contadores de rendimiento de los gráficos de RemoteFX permiten detectar cuellos de botella al ayudarle a realizar un seguimiento de aspectos como el tiempo de codificación de marcos y los fotogramas omitidos.

Una vez determinado el nombre de la sesión remota, siga estas instrucciones para recopilar los contadores de rendimiento de los gráficos de RemoteFX para su sesión remota.

1. Seleccione **Inicio** > **Herramientas administrativas** > **Monitor de rendimiento**.
2. En el cuadro de diálogo **Monitor de rendimiento**, expanda **Herramientas de supervisión**, seleccione **Monitor de rendimiento**y, a continuación, seleccione **Agregar**.
3. En el cuadro de diálogo **Agregar contadores**, en la lista **Contadores disponibles**, expanda el objeto de contador de rendimiento para los gráficos de RemoteFX.
4. Seleccione los contadores que desea supervisar.
5. En la lista **Instancias del objeto seleccionado**, seleccione las instancias específicas que desea supervisar para los contadores seleccionados y, a continuación, seleccione **Agregar**. Para seleccionar todas las instancias de contadores disponibles, seleccione **Todas las instancias**.
6. Después de agregar los contadores, seleccione **Aceptar**.

Los contadores de rendimiento seleccionados aparecerán en la pantalla Monitor de rendimiento.

>[!NOTE]
>Cada sesión activa en un host tiene su propia instancia de cada contador de rendimiento.

## <a name="diagnosis"></a>Diagnóstico

Por lo general, los problemas de rendimiento relacionados con gráficos se dividen en cuatro categorías:

- Baja velocidad de fotogramas
- Pausas aleatorias
- Alta latencia de entrada
- Baja calidad de fotograma

Solucione primero los problemas de baja velocidad de fotogramas, pausas aleatorias y alta latencia de entrada. En la siguiente sección se indica qué contadores de rendimiento miden cada categoría.

### <a name="performance-counters"></a>contadores de rendimiento

Esta sección le ayuda a identificar cuellos de botella.

En primer lugar, compruebe el contador Fotogramas de salida/segundo. Este contador mide el número de fotogramas que se ponen a disposición del cliente. Si este valor es inferior al del contador Fotogramas de entrada/segundo, se estarán omitiendo fotogramas. Para identificar el cuello de botella, use los contadores Fotogramas omitidos/segundo.

Hay tres tipos de contadores Fotogramas omitidos/segundo:

- Fotogramas omitidos/segundo: recursos de red insuficientes
- Fotogramas omitidos/segundo: recursos de cliente insuficientes
- Fotogramas omitidos/segundo: recursos de servidor insuficientes

Un valor alto de cualquiera de estos contadores implica que el problema está relacionado con el recurso que supervisa el contador. Por ejemplo, si el cliente no descodifica y presenta fotogramas a la misma velocidad que el servidor los proporciona, el contador Fotogramas omitidos/segundo: recursos de cliente insuficientes tendrá un valor elevado.

Si el contador Fotogramas de salida/segundo coincide con el contador Fotogramas de entrada/segundo pero aún experimenta retrasos o pausas inusuales, el problema puede ser el promedio de tiempo de codificación. La codificación es un proceso sincrónico que se produce en el servidor en un escenario de sesión única (vGPU) y en la máquina virtual en un escenario de sesión múltiple. El promedio de tiempo de codificación debe ser inferior a 33 ms. Si el promedio de tiempo de codificación es inferior a 33 ms pero aún tiene problemas de rendimiento, puede haber un problema con la aplicación o el sistema operativo que está usando.

Para más información acerca de cómo diagnosticar problemas relacionados con la aplicación, consulte [Contadores de rendimiento de retraso de entrada del usuario](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters).

Dado que RDP admite un promedio de tiempo de codificación de 33 ms, también admite una velocidad de fotogramas de entrada de hasta 30 fotogramas por segundo. Tenga en cuenta que 33 ms es la velocidad de fotogramas máxima permitida. En muchos casos, la velocidad de fotogramas experimentada por el usuario será menor, en función de la frecuencia con la que el origen proporciona un fotograma a RDP. Por ejemplo, las tareas como ver un vídeo requieren una velocidad de fotogramas de entrada completa de 30 fotogramas por segundo, pero otras tareas menos intensivas para los recursos, como la esporádica edición de un documento de Word, no requieren una alta velocidad de fotogramas de entrada por segundo para ofrecer una buena experiencia de usuario.

Utilice el contador Calidad de fotograma para diagnosticar problemas de calidad de fotograma. Este contador expresa la calidad del fotograma de salida como un porcentaje de la calidad del fotograma de origen. La pérdida de calidad puede deberse a RemoteFX o puede ser inherente al origen de los gráficos. Si RemoteFX es la causa de la pérdida de calidad, el problema puede ser una falta de recursos de red o de servidor para enviar contenido de una mayor fidelidad.

## <a name="mitigation"></a>Mitigación

Si los recursos del servidor son la causa del cuello de botella, pruebe una de las siguientes acciones para mejorar el rendimiento:

- Reduzca el número de sesiones por host.
- Aumente la memoria y los recursos de proceso en el servidor.
- Baje la resolución de la conexión.

Si los recursos de red son la causa del cuello de botella, pruebe una de las siguientes acciones para mejorar la disponibilidad de la red por sesión:

- Reduzca el número de sesiones por host.
- Baje la resolución de la conexión.
- Utilice una red con mayor ancho de banda.

Si los recursos del cliente son la causa del cuello de botella, realice al menos una de las siguientes acciones para mejorar el rendimiento:

- Instale el cliente de Escritorio remoto más reciente.
- Aumente la memoria y los recursos de proceso en la máquina cliente.

> [!NOTE]
> Actualmente no se admite el contador Fotogramas de origen/segundo. Por el momento, este contador siempre estará establecido en 0.

## <a name="next-steps"></a>Pasos siguientes

- Para crear una máquina virtual de Azure optimizada para GPU, consulte [Configuración de la aceleración de la unidad de procesamiento de gráficos (GPU) para Windows Virtual Desktop, versión preliminar](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu).
- Para obtener información general sobre cómo solucionar problemas y las pistas de escalación, consulte [Información general sobre solución de problemas, comentarios y soporte técnico](https://docs.microsoft.com/azure/virtual-desktop/troubleshoot-set-up-overview).
- Para obtener más información sobre el servicio en versión preliminar, consulte [Entorno de versión preliminar de Windows Desktop](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
