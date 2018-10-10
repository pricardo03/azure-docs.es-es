---
title: Procedimientos almacenados de Replicación de datos de entrada de Azure Database for MariaDB
description: En este artículo se detallan todos los procedimientos almacenados que se utilizan para Data-in Replication.
author: ajlam
ms.author: andrela
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: reference
ms.date: 09/24/2018
ms.openlocfilehash: 87c6fa783964c019841810ec38f342a5a44c0ee3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46959094"
---
# <a name="azure-database-for-mariadb-data-in-replication-stored-procedures"></a>Procedimientos almacenados de Replicación de datos de entrada de Azure Database for MariaDB

La característica Replicación de datos de entrada permite sincronizar los datos de un servidor de MariaDB que se ejecuta de forma local, de máquinas virtuales o de servicios de base de datos hospedados por otros proveedores de nube con el servicio Azure Database for MariaDB.

Los siguientes procedimientos almacenados se utilizan para establecer o quitar Replicación de datos de entrada entre un servidor maestro y de réplica.

|**Nombre del procedimiento almacenado**|**Parámetros de entrada**|**Parámetros de salida**|**Nota sobre el uso**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_master*|master_host<br/>master_user<br/>master_password<br/>master_host<br/>master_log_file<br/>master_log_file<br/>master_ssl_ca|N/D|Para transferir los datos con el modo SSL, transfiera el contexto del certificado de entidad de certificación en el parámetro master_ssl_ca. </br><br>Para transferir datos sin SSL, transfiera una cadena vacía en el parámetro master_ssl_ca.|
|*mysql.az_replication _start*|N/D|N/D|Inicia la replicación.|
|*mysql.az_replication _stop*|N/D|N/D|Detiene la replicación.|
|*mysql.az_replication _remove_master*|N/D|N/D|Quita la relación de replicación entre el servidor maestro y de réplica.|
|*mysql.az_replication_skip_counter*|N/D|N/D|Emite un error de replicación.|

Para configurar la replicación de datos de entrada entre una instancia maestra y una réplica de Azure Database for MySQL, consulte [Configuración de la replicación de datos de entrada de Azure Database for MariaDB](howto-data-in-replication.md).