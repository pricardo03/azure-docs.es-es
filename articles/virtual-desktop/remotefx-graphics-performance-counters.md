---
title: 'Diagnosticar problemas de rendimiento de gráficos en el escritorio remoto: Azure'
description: Este artículo describe cómo usar contadores de gráficos de RemoteFX en las sesiones del protocolo de escritorio remoto para diagnosticar problemas de rendimiento con gráficos en el escritorio Virtual de Windows.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshoot
ms.date: 05/23/2019
ms.author: v-chjenk
ms.openlocfilehash: 0b4113f1e0024415135aa99d1fb4e881efe448a3
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499270"
---
# <a name="diagnose-graphics-performance-issues-in-remote-desktop"></a>Diagnosticar problemas de rendimiento de gráficos en el escritorio remoto

Cuando el sistema no realiza según lo previsto, es importante identificar el origen del problema. En este artículo le ayuda a identificar y corregir cuellos de botella de rendimiento relacionados con gráficos durante las sesiones del protocolo de escritorio remoto (RDP).

## <a name="find-your-remote-session-name"></a>Busque el nombre de la sesión remota

Necesitará el nombre de la sesión remota para identificar los contadores de rendimiento de gráficos. Siga las instrucciones de esta sección para identificar el nombre de la sesión remota de Windows Vista previa del escritorio Virtual.

1. Abra el símbolo del sistema de Windows desde la sesión remota.
2. Ejecute el **qwinsta** comando.
    - Si la sesión se hospeda en una máquina virtual de multisesión (VM): El sufijo para cada nombre de contador es el mismo sufijo en su nombre de la sesión, como "rdp-tcp 37."
    - Si la sesión se hospeda en una máquina virtual que admite a unidades de procesamiento de gráficos (vGPU) virtual: Los contadores se almacenan en el servidor en lugar de en la máquina virtual. Las instancias de contador incluyen el nombre de la máquina virtual en lugar del número en el nombre de la sesión, como "Win8 Enterprise VM."

>[!NOTE]
> Mientras que los contadores tienen RemoteFX en sus nombres, en escenarios de vGPU también incluyen gráficos de escritorio remotos.

## <a name="access-performance-counters"></a>Contadores de rendimiento de acceso

Contadores de rendimiento en los gráficos de RemoteFX ayudar a detectar los cuellos de botella al ayudarle a realizar un seguimiento de las cosas como marco de tiempo de codificación y omiten los fotogramas.

Una vez que haya determinado el nombre de la sesión remota, siga estas instrucciones para recopilar los contadores de rendimiento de gráficos de RemoteFX para la sesión remota.

1. Seleccione **iniciar** > **herramientas administrativas** > **Monitor de rendimiento**.
2. En el **Monitor de rendimiento** cuadro de diálogo, expanda **las herramientas de supervisión**, seleccione **Monitor de rendimiento**y, a continuación, seleccione **agregar**.
3. En el **agregar contadores** cuadro de diálogo desde el **contadores disponibles** lista, expanda el objeto de contador de rendimiento para los gráficos de RemoteFX.
4. Seleccione los contadores que desea supervisar.
5. En el **instancias del objeto seleccionado** lista, seleccione las instancias específicas que desea supervisar para los contadores seleccionados y, a continuación, seleccione **agregar**. Para seleccionar todas las instancias de contador disponibles, seleccione **todas las instancias**.
6. Después de agregar los contadores, seleccione **Aceptar**.

Los contadores de rendimiento seleccionado aparecerá en la pantalla del Monitor de rendimiento.

>[!NOTE]
>Cada sesión activa en un host tiene su propia instancia de cada contador de rendimiento.

## <a name="diagnosis"></a>Diagnóstico

Problemas de rendimiento relacionadas con gráficos generalmente se dividen en cuatro categorías:

- Velocidad de fotogramas baja
- Obstrucciones aleatorios
- Alta latencia de entrada
- Calidad de fotograma deficiente

Iniciar al tratar de velocidad de fotogramas baja, detenciones aleatorios y alta latencia de entrada. La siguiente sección le indicará qué contadores de rendimiento miden cada categoría.

### <a name="performance-counters"></a>contadores de rendimiento

En esta sección le ayuda a identificar cuellos de botella.

En primer lugar, compruebe el contador de Frames/Second de salida. Mide el número de fotogramas que se ponen a disposición del cliente. Si este valor es menor que el contador Frames/Second de entrada, se omitiera marcos. Para identificar el cuello de botella, use los fotogramas omitidos por segundo contadores.

Hay tres tipos de fotogramas omitidos por segundo contadores:

- Fotogramas/segundo omitido (recursos de red insuficiente)
- Fotogramas/segundo omitido (cliente insuficientes recursos)
- Fotogramas/segundo omitido (recursos suficientes en el servidor)

Un valor alto para cualquiera de los fotogramas omitidos por segundo contadores implica que el problema está relacionado con el recurso realiza un seguimiento del contador. Por ejemplo, si el cliente no descodificar y marcos presentes en la misma velocidad que el servidor proporciona los marcos, el contador de fotogramas omitidos por segundo (cliente recursos insuficientes) será alta.

Si el contador Frames/Second salida coincide con el contador de entrada Frames/Second, aún tendrá lag inusual o estancamiento, el problema puede ser el tiempo medio de codificación. Encoding es un proceso sincrónico que aparece en el servidor en el escenario de sesión único (vGPU) y en la máquina virtual en el escenario de varias sesiones. Promedio de tiempo de codificación debe ser menos ms 33. Si el tiempo promedio de codificación es 33 ms. en pero todavía tiene problemas de rendimiento, puede haber un problema con la aplicación o el sistema operativo que está usando.

Para obtener más información acerca de cómo diagnosticar problemas relacionados con la aplicación, consulte [contadores de rendimiento retraso de entrada del usuario](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters).

Dado que RDP es compatible con un tiempo medio de codificación de 33 ms., admite una velocidad de fotogramas de entrada hasta 30 fotogramas por segundo. Tenga en cuenta que 33 ms. es la velocidad de fotogramas admitidos máximo. En muchos casos, la velocidad de fotogramas experimenta por el usuario será menor, en función de con qué frecuencia se proporciona un marco para RDP por el origen. Por ejemplo, tareas como ver un vídeo requieren una velocidad de fotogramas de entrada completa de 30 fotogramas por segundo, pero menos tareas intensivo de recursos como la edición con poca frecuencia un documento de word no requieren una alta tasa de entrada fotogramas por segundo para una buena experiencia del usuario.

Utilice el contador de calidad de fotograma para diagnosticar problemas de calidad de fotograma. Este contador expresa la calidad de la trama de salida como un porcentaje de la calidad del fotograma de origen. Puede deberse a la pérdida de calidad RemoteFX, o puede ser inherente al origen de gráficos. Si RemoteFX causó la pérdida de calidad, el problema puede ser una falta de recursos de red o el servidor para enviar contenido de una mayor fidelidad.

## <a name="mitigation"></a>Mitigación

Si los recursos del servidor están causando el cuello de botella, intente una de las siguientes acciones para mejorar el rendimiento:

- Reduzca el número de sesiones por host.
- Aumente la memoria y recursos en el servidor de proceso.
- Quitar la resolución de la conexión.

Si los recursos de red están causando el cuello de botella, intente una de las siguientes acciones para mejorar la disponibilidad de red por sesión:

- Reduzca el número de sesiones por host.
- Quitar la resolución de la conexión.
- Utilice una mayor ancho de banda red.

Si los recursos del cliente están causando el cuello de botella, realice una o ambas de las siguientes acciones para mejorar el rendimiento:

- Instalar al cliente de escritorio remoto más reciente.
- Aumente la memoria y recursos en el equipo cliente de proceso.

> [!NOTE]
> Actualmente no admitimos el contador Frames/Second de origen. Por ahora, el contador de origen Frames/Second siempre se establecerá en 0.

## <a name="next-steps"></a>Pasos siguientes

- Para crear una máquina virtual de Azure GPU optimizada, consulte [configurar la aceleración de la unidad (GPU) para el entorno de Windows Vista previa del escritorio Virtual de procesamiento de gráficos](https://docs.microsoft.com/azure/virtual-desktop/configure-vm-gpu).
- Para obtener información general de las pistas de escalamiento y solución de problemas, consulte [solución de problemas de introducción, comentarios y soporte técnico](https://docs.microsoft.com/azure/virtual-desktop/troubleshoot-set-up-overview).
- Para obtener más información sobre el servicio en versión preliminar, consulte [entorno de versión preliminar de Windows Desktop](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
