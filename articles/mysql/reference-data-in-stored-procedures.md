---
title: Procedimientos almacenados de Data-in Replication de Azure Database for MySQL
description: En este artículo se detallan todos los procedimientos almacenados que se utilizan para Data-in Replication.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 08/31/2018
ms.openlocfilehash: a3c88953eea95871529e8ab257f52b694db443a9
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53544862"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>Procedimientos almacenados de Data-in Replication de Azure Database for MySQL

Data-in Replication permite sincronizar los datos de un servidor de MySQL que se ejecuta de forma local, en máquinas virtuales o servicios de base de datos hospedados por otros proveedores de nube en el servicio de Azure Database for MySQL.

Los siguientes procedimientos almacenados se utilizan para establecer o quitar Replicación de datos de entrada entre un servidor maestro y de réplica.

|**Nombre del procedimiento almacenado**|**Parámetros de entrada**|**Parámetros de salida**|**Nota sobre el uso**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_host<br/>master_log_file<br/>master_log_file<br/>master_ssl_ca|N/D|Para transferir los datos con el modo SSL, transfiera el contexto del certificado de entidad de certificación en el parámetro master_ssl_ca. </br><br>Para transferir datos sin SSL, transfiera una cadena vacía en el parámetro master_ssl_ca.|
|*mysql.az_replication _start*|N/D|N/D|Inicia la replicación.|
|*mysql.az_replication _stop*|N/D|N/D|Detiene la replicación.|
|*mysql.az_replication _remove_master*|N/D|N/D|Quita la relación de replicación entre el servidor maestro y de réplica.|
|*mysql.az_replication_skip_counter*|N/D|N/D|Emite un error de replicación.|

Para configurar la replicación de datos de entrada entre un servidor maestro y un servidor de réplica en Azure Database for MySQL, consulte [Configuración de la replicación de datos internos de Azure Database for MySQL](howto-data-in-replication.md).
