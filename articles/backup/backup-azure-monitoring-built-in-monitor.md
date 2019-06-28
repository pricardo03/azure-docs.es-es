---
title: 'Azure Backup: Supervisión de cargas de trabajo protegidas de Azure Backup'
description: Supervisión de cargas de trabajo de Azure Backup mediante Azure Portal
services: backup
author: pvrk
manager: shivamg
keywords: Azure Backup; Alertas;
ms.service: backup
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: pullabhk
ms.assetid: 86ebeb03-f5fa-4794-8a5f-aa5cbbf68a81
ms.openlocfilehash: ab7d2c0af4bc71733a7995b7e781f0facbfbb29f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65236443"
---
# <a name="monitoring-azure-backup-workloads"></a>Supervisión de cargas de trabajo de Azure Backup

Azure Backup proporciona varias soluciones de copia de seguridad según los requisitos de copia de seguridad y la topología de la infraestructura (local o en Azure). Cualquier usuario o administrador de copias de seguridad debería poder ver lo que está ocurriendo en todas las soluciones y espera recibir notificaciones en los escenarios importantes. En este artículo se detallan las capacidades de supervisión y notificación que presta el servicio de Azure Backup.

## <a name="backup-jobs-in-recovery-services-vault"></a>Trabajos de copia de seguridad en el almacén de Recovery Services

Azure Backup proporciona capacidades de supervisión y notificación relativas a las cargas de trabajo bajo protección de Azure Backup. En la sección **Supervisión** de la configuración del almacén de Recovery Services encontrará trabajos y alertas integrados.

![Almacén de Recovery Services frente a supervisión integrada](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltmonitoring.png)

Los trabajos se generan cuando se realizan operaciones como configurar copias de seguridad, hacer copias de seguridad, restaurar copias de seguridad, eliminar copias de seguridad, etc.

Aquí se muestran los trabajos de las siguientes soluciones de Azure Backup:

  - Copia de seguridad de máquina virtual de Azure
  - Copia de seguridad de archivos de Azure
  - Copia de seguridad de cargas de trabajo de Azure, como SQL
  - Agente de Azure Backup (MAB)

Los trabajos procedentes de System Center Data Protection Manager (SC-DPM) y Microsoft Azure Backup Server (MABS) NO se muestran.

> [!NOTE]
> Las cargas de trabajo de Azure, como las copias de seguridad de SQL dentro de las máquinas virtuales de Azure, tienen gran número de trabajos de copia de seguridad. Por ejemplo, se pueden ejecutar copias de seguridad de los registros cada 15 minutos. Por lo tanto, en el caso de estas cargas de trabajo de base de datos, solamente se muestran las operaciones desencadenadas por el usuario. NO se muestran las operaciones de copia de seguridad programadas.

## <a name="backup-alerts-in-recovery-services-vault"></a>Supervisión de alertas de copia de seguridad en el almacén de Recovery Services

Las alertas son, fundamentalmente, los escenarios donde los usuarios reciben notificaciones para que puedan tomar las medidas oportunas. La sección **Alertas de copia de seguridad** muestra las alertas generadas por el servicio de Azure Backup. Estas alertas las define el servicio; es decir, el usuario no puede crear ninguna alerta personalizada.

### <a name="alert-scenarios"></a>Escenarios de alerta
El servicio define los siguientes escenarios como escenarios susceptibles de generar alertas.

  - Errores de copia de seguridad y restauración
  - Copia de seguridad correcta con advertencias para el agente de Azure Backup (MAB)
  - Detener la protección con conservación de datos/Detener la protección con eliminación de datos

### <a name="exceptions-when-an-alert-is-not-raised"></a>Excepciones cuando una alerta no se genera
Hay algunas excepciones en las que no se genera una alerta en caso de error. Son estas:

  - Un usuario ha cancelado expresamente el trabajo en ejecución.
  - Se ha producido un error en el trabajo porque otro trabajo de copia de seguridad está en curso (no hay nada que se pueda hacer, solo hay que esperar a que el trabajo anterior finalice).
  - Se ha producido un error en el trabajo de copia de seguridad de máquina virtual porque la máquina virtual de Azure de la que se está haciendo la copia de seguridad ya no existe.

Estas excepciones están diseñadas bajo el supuesto de que el resultado de estas operaciones (desencadenadas principalmente por el usuario) se muestra inmediatamente en los clientes de Portal/PowerShell/CLI. Por lo tanto, el usuario es consciente de ello de inmediato y no necesita que se le notifique.

### <a name="alerts-from-the-following-azure-backup-solutions-are-shown-here"></a>Aquí se muestran las alertas de las siguientes soluciones de Azure Backup:

  - Copias de seguridad de máquinas virtuales de Azure
  - Copias de seguridad de archivos de Azure
  - Copias de seguridad de cargas de trabajo de Azure, como SQL
  - Agente de Azure Backup (MAB)

> [!NOTE]
> Las alertas procedentes de System Center Data Protection Manager (SC-DPM) y Microsoft Azure Backup Server (MABS) NO se muestran aquí.

### <a name="alert-types"></a>Tipos de alerta
Se pueden definir tres tipos alertas, según la gravedad:

  - **Crítico**: en principio, cualquier error en una copia de seguridad o una recuperación (sea programada o desencadenada por el usuario) daría lugar a la generación de una alerta, que se mostraría como una alerta crítica junto con operaciones de destrucción como eliminar la copia de seguridad.
  - **Advertencia**: si la operación de copia de seguridad se realiza correctamente, pero con algunas advertencias, estas se muestran como alertas de advertencia.
  - **Informativo**: a día de hoy, el servicio de Azure Backup aún no ha generado ninguna alerta informativa.

## <a name="notification-for-backup-alerts"></a>Notificación de alertas de copia de seguridad

> [!NOTE]
> La configuración de notificaciones solo se puede realizar a través de Azure Portal. No se admite PowerShell, la CLI, la API de REST ni la plantilla de Azure Resource Manager.

Después de que se genere una alerta, se notifica a los usuarios. Azure Backup proporciona un mecanismo de notificación integrado por correo electrónico. Se pueden especificar direcciones de correo electrónico individuales o listas de distribución para recibir una notificación cuando una alerta se genere. También puede optar por recibir una notificación con cada alerta individual, o bien agruparlas en un resumen cada hora y, luego, recibir la notificación correspondiente.

![Notificación por correo electrónico integrada del almacén de Recovery Services](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltnotification.png)

Cuando configure una notificación, recibirá un correo electrónico de bienvenida o de introducción. De esta forma, se confirma que Azure Backup puede enviar mensajes de correo electrónico a estas direcciones cuando se genere una alerta.<br>

Si la frecuencia se ha establecido en un resumen cada hora y una alerta se genera y resuelve en el plazo de una hora, dicha alerta no formará parte del próximo resumen de cada hora.

> [!NOTE]
>
> * Si se lleva a cabo una operación de destrucción, como **Detener la protección con eliminación de datos**, se generará una alerta y se enviará un correo electrónico a los propietarios de suscripciones, administradores y coadministradores, aun cuando las notificaciones NO estén configuradas en el almacén de Recovery Services.
> * Para configurar notificaciones que informen de los trabajos realizados correctamente, use [Log Analytics](backup-azure-monitoring-use-azuremonitor.md#using-log-analytics-workspace).

## <a name="next-steps"></a>Pasos siguientes

[Supervisión de cargas de trabajo de Azure Backup mediante Azure Monitor](backup-azure-monitoring-use-azuremonitor.md)
