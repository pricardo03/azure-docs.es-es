---
title: 'Azure Backup: Cargas de trabajo de copia de seguridad de Azure Monitor protegidas'
description: Supervisar las cargas de trabajo de copia de seguridad de Azure mediante Azure portal
services: backup
author: pvrk
manager: shivamg
keywords: Copia de seguridad de Azure; Alertas;
ms.service: backup
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: pullabhk
ms.assetid: 86ebeb03-f5fa-4794-8a5f-aa5cbbf68a81
ms.openlocfilehash: 8d3e3257f16fe4e0f846c2268bfefc2771387de6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60809076"
---
# <a name="monitoring-azure-backup-workloads"></a>Supervisión de las cargas de trabajo de copia de seguridad de Azure

Azure Backup proporciona varias soluciones de copia de seguridad según la topología de requisito y la infraestructura de copia de seguridad (en el entorno local frente a Azure). Cualquier usuario de copia de seguridad o administrador debería ver lo que está ocurriendo en todas las soluciones y espera que se notificará en escenarios importantes. En este artículo se detalla las funcionalidades de supervisión y notificación proporcionadas por el servicio de copia de seguridad de Azure.

## <a name="backup-jobs-in-recovery-services-vault"></a>Trabajos de copia de seguridad en el almacén de Recovery Services

Azure Backup proporciona integrados supervisión y generación de alertas para cargas de trabajo que está protegidas por Azure Backup. En los servicios de recuperación del almacén de configuración, el **supervisión** sección proporciona en generan trabajos y alertas.

![RS de almacén de supervisión integradas](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltmonitoring.png)

Los trabajos se generan cuando se realizan operaciones tales como la configuración de copia de seguridad, copia de seguridad, restauración, eliminar copia de seguridad y así sucesivamente.

Aquí se muestran los trabajos de las siguientes soluciones de copia de seguridad de Azure:

  - Copia de seguridad de máquina virtual de Azure
  - Azure copia de seguridad de archivos
  - Copia de seguridad de Azure de la carga de trabajo como SQL
  - Agente de copia de seguridad de Azure (AM)

NO se muestran los trabajos de System Center Data Protection Manager (SC DPM), Microsoft Azure Backup Server (MABS).

> [!NOTE]
> Las cargas de trabajo de Azure, como copias de seguridad SQL dentro de máquinas virtuales de Azure tienen gran número de trabajos de copia de seguridad. Por ejemplo, pueden ejecutar las copias de seguridad del registro para cada 15 minutos. Por lo tanto, para estas cargas de trabajo de la base de datos, se muestran las operaciones de usuario solo desencadenada. NO se muestran las operaciones de copia de seguridad programadas.

## <a name="backup-alerts-in-recovery-services-vault"></a>Alertas de copia de seguridad en el almacén de Recovery Services

Las alertas son principalmente los escenarios donde los usuarios reciben notificaciones para que puedan tomar las acciones pertinentes. El **alertas de copia de seguridad** sección muestra las alertas generadas por el servicio Azure Backup. Estas alertas se definen mediante el servicio y el usuario no personalizadas crear todas las alertas.

### <a name="alert-scenarios"></a>Escenarios de alerta
Los escenarios siguientes se definen por servicio como escenarios genera alertas.

  - Errores de copia de seguridad y restauración
  - Copia de seguridad correcta con advertencias
  - Detener la protección con conservar datos y detenga la protección con eliminación de datos

### <a name="exceptions-when-an-alert-is-not-raised"></a>Excepciones cuando no se genera una alerta
Hay algunas excepciones cuando no se genera una alerta en un caso de error, son:

  - Explícitamente, el usuario canceló el trabajo en ejecución
  - Se produce un error en el trabajo porque otro trabajo de copia de seguridad está en curso (nada para actuar sobre aquí, puesto que sólo tenemos que esperar para que finalice el trabajo anterior)
  - Se produce un error en el trabajo de copia de seguridad de máquina virtual porque ya no existe la máquina virtual de Azure de copia de seguridad

Las excepciones anteriores están diseñadas desde la asunción de que el resultado de estas operaciones (principalmente desencadenada el usuario) se muestra inmediatamente en los clientes de PS/portal/CLI. Por lo tanto, el usuario conoce inmediatamente y no necesita una notificación.

### <a name="alerts-from-the-following-azure-backup-solutions-are-shown-here"></a>Aquí se muestran las alertas de las siguientes soluciones de copia de seguridad de Azure:

  - Copias de seguridad de Azure VM
  - Azure copias de seguridad de archivos
  - Copias de seguridad de cargas de trabajo de Azure, como SQL
  - Agente de copia de seguridad de Azure (AM)

> [!NOTE]
> Las alertas de System Center Data Protection Manager (SC DPM), Microsoft Azure Backup Server (MABS) no se muestran aquí.

### <a name="alert-types"></a>Tipos de alerta
Según la gravedad de alerta, se pueden definir alertas en tres tipos:

  - **Crítico**: En principio, cualquier copia de seguridad o recuperación error (programados o desencadenados de usuario) daría lugar a la generación de una alerta y se mostraría como una alerta crítica y también las operaciones destructivas como eliminar copia de seguridad.
  - **Advertencia**: Si la operación de copia de seguridad se realiza correctamente, pero con algunas advertencias que aparecen como alertas de advertencia.
  - **Informativo**: En este momento, ninguna alerta informativa se genera mediante el servicio Azure Backup.

## <a name="notification-for-backup-alerts"></a>Notificación de alertas de copia de seguridad

> [!NOTE]
> Configuración de notificación se puede realizar únicamente a través del Portal de Azure. No se admite la compatibilidad con plantillas de PS, CLI o REST API o Azure Resource Manager.

Una vez que se genera una alerta, se notificación a los usuarios. Azure Backup proporciona un mecanismo integrado de notificación por correo electrónico. Uno puede especificar direcciones de correo electrónico individuales o listas de distribución para recibir una notificación cuando se genera una alerta. También puede elegir si desea recibir una notificación para cada alerta individual o agruparlos en un resumen cada hora y, a continuación, recibir una notificación.

![Notificación por correo electrónico almacén RS integradas](media/backup-azure-monitoring-laworkspace/rs-vault-inbuiltnotification.png)

Cuando se configura la notificación, recibirá un correo electrónico de bienvenida o de introducción. Esto confirma que Azure Backup puede enviar mensajes de correo electrónico a estas direcciones cuando se genera una alerta.<br>

Si la frecuencia se estableció en un resumen cada hora y se generó una alerta y se puede resolver dentro de una hora, no será una parte de la próxima resumen cada hora.

> [!NOTE]
> 
> * Si una operación destructiva como **detener la protección con eliminación de datos** es realiza, se genera una alerta y se envía un correo electrónico a los propietarios de suscripciones, los administradores y coadministradores incluso si no se configuran las notificaciones para el servicio de recuperación almacén.
> * Para configurar la notificación para trabajos realizados correctamente, use [Log Analytics](backup-azure-monitoring-use-azuremonitor.md#using-log-analytics-workspace).

## <a name="next-steps"></a>Pasos siguientes

[Supervisar Azure dichas cargas de trabajo mediante Azure Monitor](backup-azure-monitoring-use-azuremonitor.md)
