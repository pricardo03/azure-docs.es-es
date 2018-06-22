---
title: Procedimientos almacenados de Data-in Replication de Azure Database for MySQL
description: En este artículo se detallan todos los procedimientos almacenados que se utilizan para Data-in Replication.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 05/18/2018
ms.openlocfilehash: 2d62cd693d7a67faf836c645f8bd33de9afca949
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266115"
---
# <a name="azure-database-for-mysql-data-in-replication-stored-procedures"></a>Procedimientos almacenados de Data-in Replication de Azure Database for MySQL

Data-in Replication permite sincronizar los datos de un servidor de MySQL que se ejecuta de forma local, en máquinas virtuales o servicios de base de datos hospedados por otros proveedores de nube en el servicio de Azure Database for MySQL.

Los siguientes procedimientos almacenados se utilizan para establecer o quitar Data-in Replication entre un servidor principal y la réplica.

|**Nombre del procedimiento almacenado**|**Parámetros de entrada**|**Parámetros de salida**|**Nota sobre el uso**|
|-----|-----|-----|-----|
|*mysql.az_replication_change_primary*|master_host<br/>master_user<br/>master_password<br/>master_host<br/>master_log_file<br/>master_log_file<br/>master_ssl_ca|N/D|Para transferir los datos con el modo SSL, transfiera el contexto del certificado de entidad de certificación en el parámetro master_ssl_ca. </br><br>Para transferir datos sin SSL, transfiera una cadena vacía en el parámetro master_ssl_ca.|
|*mysql.az_replication _start*|N/D|N/D|Inicia la replicación.|
|*mysql.az_replication _stop*|N/D|N/D|Detiene la replicación.|
|*mysql.az_replication _remove_primary*|N/D|N/D|Quita la relación de replicación entre el servidor principal y Réplica.|
|*mysql.az_replication_skip_counter*|N/D|N/D|Emite un error de replicación.|

Para configurar la replicación de datos entre un servidor principal y un servidor Réplica en Azure Database for MySQL, consulte [cómo configurar la replicación de datos internos](howto-data-in-replication.md).