---
title: Migración con tiempo de inactividad mínimo a Azure Database for MySQL
description: En este artículo se describe cómo realizar una migración con tiempo de inactividad mínimo de una base de datos de MySQL a una base de datos de Azure Database for MySQL mediante Azure Database Migration Service.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: 267212d8f832b96bf66145b97c3464471e28593d
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "53014198"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migración con tiempo de inactividad mínimo a Azure Database for MySQL
Puede realizar migraciones de MySQL a Azure Database for MySQL con un tiempo de inactividad mínimo mediante la **funcionalidad de sincronización continua** recién introducida para el [servicio Azure Database Migration Service](https://aka.ms/get-dms) (DMS). Esta funcionalidad limita la cantidad de tiempo de inactividad en el que incurre la aplicación.

## <a name="overview"></a>Información general
Azure DMS realiza una carga inicial de la base de datos local en Azure Database for MySQL y luego realiza la sincronización continua de todas las transacciones nuevas de Azure mientras la aplicación se siga ejecutando. Una vez que los datos se ponen al día en el destino de Azure, puede detener la aplicación por un breve período de tiempo (tiempo de inactividad mínimo), esperar al último lote de datos (desde el momento en que detiene la aplicación hasta el momento en que la aplicación deja de estar disponible efectivamente para aceptar cualquier tráfico nuevo) para ponerse al día en el destino y, a continuación, actualizar la cadena de conexión para que apunte a Azure. Cuando haya terminado, la aplicación estará funcionando en Azure.

![Sincronización continua con Azure Database Migration Service](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Pasos siguientes
- Vea el vídeo sobre [migración fácil de aplicaciones de MySQL/PostgreSQL a Azure Managed Service](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), que contiene una demostración que muestra cómo migrar aplicaciones de MySQL a Azure Database for MySQL.
- Consulte el tutorial [Migración de MySQL a Azure Database for MySQL en línea mediante DMS](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).
