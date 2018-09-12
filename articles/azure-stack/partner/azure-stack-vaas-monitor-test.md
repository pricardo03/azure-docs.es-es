---
title: Supervisión de una prueba con la validación como servicio de Azure Stack | Microsoft Docs
description: Supervisión de una prueba con la validación como servicio de Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/24/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 553d2a0e4bf6b23f5d8ab200f533d9245bf72d36
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286591"
---
# <a name="monitor-a-test-with-azure-stack-validation-as-a-service"></a>Supervisión de una prueba con la validación como servicio de Azure Stack

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

Se puede supervisar la ejecución de una prueba en la página **Operations** (Operaciones) de los conjuntos de pruebas que están en curso o completados. Esta página detalla el estado de la prueba y sus operaciones.

## <a name="monitor-a-test"></a>Supervisión de una prueba

1. Seleccione una solución.

2. Seleccione **Manage** (Administrar) en cualquier icono de flujo de trabajo.

3. Haga clic en un flujo de trabajo para abrir la página de resumen de la prueba.

4. Expanda el menú contextual **[...]** de cualquier instancia de conjunto de pruebas.

5. Seleccione **View Operations** (Ver operaciones)

![Texto alternativo](media\image4.png)

Para las pruebas que han terminado de ejecutarse, se pueden descargar los registros en la página de resumen de la prueba haciendo clic en **Download logs** (Descargar registros) en el menú contextual de una prueba **[...]** . Los asociados de Azure Stack pueden usar estos registros para depurar los problemas de las pruebas no superadas.

## <a name="open-the-test-pass-summary"></a>Apertura del resumen de la ejecución de la prueba

1. Abra el portal. 
2. Seleccione el nombre de una solución existente que contenga pruebas ejecutadas o programadas anteriormente.

    ![Administración de ejecuciones de pruebas](media/managetestpasses.png)

3. Seleccione **Manage** (Administrar) en el panel **Test Passes** (Ejecuciones de pruebas).
4. Seleccione la ejecución de la prueba para abrir el resumen de la misma. Puede revisar el nombre de la prueba, la fecha de creación y ejecución, cuánto tiempo tardó la prueba y el resultado (correcta o errónea).
5. Seleccione [**...** ].

### <a name="test-pass-summary"></a>Resumen de la ejecución de la prueba

| Columna | DESCRIPCIÓN |
| --- | --- |
| Nombre de la prueba | Nombre de la prueba. Hace referencia al número de validación. |
| Creado | Hora de creación de la ejecución de la prueba. |
| Started | Hora de ejecución de la prueba superada. |
| Duration | El tiempo que tardó la ejecución de la prueba. |
| Status | El resultado (correcto o erróneo) de la prueba superada. |
| Nombre del agente | El nombre de dominio completo del agente. |
| Total de operaciones | El número total de operaciones que se han intentado en la ejecución de la prueba. |
| Operaciones correctas | El número de operaciones correctas en la ejecución de la prueba. |
|  Operaciones erróneas | El número de operaciones con error. |

### <a name="group-columns-in-the-test-pass-summary"></a>Agrupación de columnas en el resumen de la ejecución de la prueba

Puede seleccionar y arrastrar una columna al encabezado para crear un grupo en el valor de la columna.

## <a name="reschedule-a-test"></a>Reprogramación de una prueba

1. [Abra el resumen de la ejecución de la prueba](#open-the-test-pass-summary).
2. Seleccione **Reschedule** (Reprogramar) para volver a programar la ejecución de la prueba.
3. Escriba la contraseña del administrador en la nube de la instancia de Azure Stack.
4. Escriba la cadena de conexión del almacenamiento de diagnósticos que definió al configurar la cuenta.
5. Vuelva a programar la prueba.

## <a name="cancel-a-test"></a>Cancelación de una prueba

1. [Abra el resumen de la ejecución de la prueba](#open-the-test-pass-summary).
2. Seleccione **Cancelar**.

## <a name="get-test-information"></a>Obtención de la información de la prueba

1. [Abra el resumen de la ejecución de la prueba](#open-the-test-pass-summary).
2. Seleccione **View information** (Ver información) para volver a programar la ejecución de la prueba.

**Información de la prueba**

| NOMBRE | DESCRIPCIÓN |
| -- | -- |
| Nombre de la prueba | El nombre de la prueba, por ejemplo, OEM Update on Azure Stack 1806 RC Validation. |
| Versión de la prueba | La versión de la prueba, por ejemplo, 5.1.4.0. |
| Publicador | El publicador de la prueba, como Microsoft. |
| Categoría | La categoría de la prueba, como **Funcional** o **Confiabilidad**. |
| Servicios de destino | Los servicios que se están probando, por ejemplo, VirtualMachines |
| DESCRIPCIÓN | Descripción de la prueba. |
| Duración estimada (minutos) | El período de tiempo en minutos que tardó la prueba en ejecutarse. |
| Vínculos | Un vínculo al rastreador de problemas de GitHub. |

## <a name="get-test-parameters"></a>Obtención de los parámetros de la prueba

1. [Abra el resumen de la ejecución de la prueba](#open-the-test-pass-summary).
2. Seleccione **View parameters** (Ver parámetros) para volver a programar la ejecución de la prueba.

**Parámetros**

| NOMBRE | DESCRIPCIÓN |
| -- | -- |
| Nombre de la prueba | El nombre de la prueba, por ejemplo, oemupdate1806test. |
| Versión de la prueba | La versión de la prueba, por ejemplo, 5.1.4.0. |
| Identificador de instancia de la prueba | Un GUID que identifica la instancia específica de la prueba, por ejemplo, 20b20645-b400-4f0d-bf6f-1264d866ada9. |
| cloudAdminUser | El nombre de la cuenta usada como administrador de la nube, por ejemplo, **cloudadmin**. |
| DiagnosticsContainerName | El identificador del contenedor de diagnósticos, por ejemplo, 04dd3815-5f35-4158-92ea-698027693080. |

## <a name="get-test-operations"></a>Obtención de las operaciones de la prueba

1. [Abra el resumen de la ejecución de la prueba](#open-the-test-pass-summary).
2. Seleccione **View operations** (Ver operaciones) para volver a programar la ejecución de la prueba. Se abre el panel de resumen de las operaciones.

## <a name="get-test-logs"></a>Obtención de los registros de la prueba

1. [Abra el resumen de la ejecución de la prueba](#open-the-test-pass-summary).
2. Seleccione **Download logs** (Descargar registros) para volver a programar la ejecución de la prueba.  
    Se descarga un archivo zip llamado ReleaseYYYY-MM-DD.zip que contiene los registros.

## <a name="next-steps"></a>Pasos siguientes

- Para más información, consulte [Validación como servicio de Azure Stack](https://docs.microsoft.com/azure/azure-stack/partner).
