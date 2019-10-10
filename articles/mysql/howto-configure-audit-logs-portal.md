---
title: 'Configuración de los registros de auditoría y acceso a ellos: Azure Database for MySQL'
description: En este artículo se describe cómo configurar los registros de auditoría de Azure Database for MySQL, y acceder a ellos, mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 06/24/2019
ms.openlocfilehash: 2a2d2a697f0e41fb296c61c01909a814678f8277
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350397"
---
# <a name="configure-and-access-audit-logs-for-azure-database-for-mysql-in-the-azure-portal"></a>Configuración de los registros de auditoría de Azure Database for MySQL y acceso a ellos en Azure Portal

Puede configurar los [registros de auditoría de Azure Database for MySQL](concepts-audit-logs.md) y realizar un diagnóstico de la configuración desde Azure Portal.

> [!IMPORTANT]
> La funcionalidad del registro de auditoría se encuentra actualmente en versión preliminar.

## <a name="prerequisites"></a>Requisitos previos

Para seguir esta guía, necesitará:

- [Servidor de Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="configure-audit-logging"></a>Configuración del registro de auditoría

Habilite y configure el registro de auditoría.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

1. Seleccione un servidor de Azure Database for MySQL.

1. En la sección **Configuración**, seleccione **Parámetros del servidor**.
    ![Parámetros del servidor](./media/howto-configure-audit-logs-portal/server-parameters.png)

1. Actualice el parámetro **audit_log_enabled** a ACTIVADO.
    ![Habilitar los registros de auditoría](./media/howto-configure-audit-logs-portal/audit-log-enabled.png)

1. Seleccione los [tipos de eventos](concepts-audit-logs.md#configure-audit-logging) que se registrarán actualizando el parámetro **audit_log_events**.
    ![Eventos del registro de auditoría](./media/howto-configure-audit-logs-portal/audit-log-events.png)

1. Agregue a cualquier usuario de MySQL que se excluya del registro actualizando el parámetro **audit_log_exclude_users**. Especifique los usuarios proporcionando su nombre de usuario de MySQL.
    ![Excluir usuarios del registro de auditoría](./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png)

1. Cuando haya cambiado los parámetros, haga clic en **Guardar**. O bien puede **descartar** los cambios.
    ![Guardar](./media/howto-configure-audit-logs-portal/save-parameters.png)

## <a name="set-up-diagnostic-logs"></a>Configuración de registros de diagnósticos

1. En la sección **Supervisión** de la barra lateral, seleccione **Configuración de diagnóstico**.

1. Haga clic en "+ Agregar configuración de diagnóstico" ![Agregar configuración de diagnóstico](./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png)

1. Proporcione un nombre de configuración de diagnóstico.

1. Especifique qué datos se enviarán para enviar los registros de auditoría (cuenta de almacenamiento, centro de eventos y área de trabajo de Log Analytics).

1. Seleccione "MySqlAuditLogs" como el tipo de registro.
![Configurar las opciones de diagnóstico](./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png)

1. Una vez que haya configurado los receptores de datos para canalizar los registros de auditoría, puede hacer clic en **Guardar**.
![Guardar la configuración de diagnóstico](./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png)

1. Acceda a los registros de auditoría mediante los receptores de datos que configuró. Los registros pueden tardar hasta 10 minutos en aparecer.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre los [registros de auditoría](concepts-audit-logs.md) en Azure Database for MySQL.