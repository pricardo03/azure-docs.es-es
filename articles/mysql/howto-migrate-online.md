---
title: Migración con tiempo de inactividad mínimo a Azure Database for MySQL
description: En este artículo se describe cómo realizar una migración con tiempo de inactividad mínimo de una base de datos de MySQL a una base de datos de Azure Database for MySQL mediante Azure Database Migration Service.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 10/11/2018
ms.openlocfilehash: cb2e1a34748875de9c7f8dd59be27c408ec82818
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49090338"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migración con tiempo de inactividad mínimo a Azure Database for MySQL
Puede realizar migraciones de MySQL a Azure Database for MySQL con un tiempo de inactividad mínimo mediante la **funcionalidad de sincronización continua** recién introducida para el [servicio Azure Database Migration Service](https://aka.ms/get-dms) (DMS). Esta funcionalidad limita la cantidad de tiempo de inactividad en el que incurre la aplicación.

## <a name="overview"></a>Información general
DMS realiza una carga inicial de la base de datos local en Azure Database for MySQL y, a continuación, realiza la sincronización continua de todas las transacciones nuevas de Azure mientras la aplicación se siga ejecutando. Una vez que los datos se ponen al día en el destino de Azure, puede detener la aplicación por un breve período de tiempo (tiempo de inactividad mínimo), esperar al último lote de datos (desde el momento en que detiene la aplicación hasta el momento en que la aplicación deja de estar disponible efectivamente para aceptar cualquier tráfico nuevo) para ponerse al día en el destino y, a continuación, actualizar la cadena de conexión para que apunte a Azure. Cuando haya terminado, la aplicación estará funcionando en Azure.

![Sincronización continua con Azure Database Migration Service](./media/howto-migrate-online/ContinuousSync.png)

- La migración de DMS de orígenes de MySQL se encuentra actualmente en versión preliminar pública. Si desea probar el servicio para migrar las cargas de trabajo de MySQL, continúe en el portal. Sus comentarios son muy valiosos para ayudar a mejorar aún más el servicio.

## <a name="next-steps"></a>Pasos siguientes
- Vea el vídeo sobre [migración fácil de aplicaciones de MySQL/PostgreSQL a Azure Managed Service](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), que contiene una demostración que muestra cómo migrar aplicaciones de MySQL a Azure Database for MySQL.
- Consulte el tutorial [Migración de MySQL a Azure Database for MySQL en línea mediante DMS](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).
