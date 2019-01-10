---
title: Introducción a Azure Table Storage
description: Almacene datos estructurados en la nube con el Almacenamiento de tablas de Azure, un almacén de datos NoSQL.
ms.service: cosmos-db
ms.subservice: cosmosdb-table
ms.devlang: dotnet
ms.topic: overview
ms.date: 11/03/2017
author: wmengmsft
ms.author: wmeng
ms.reviewer: sngun
ms.openlocfilehash: 69d2eb1627d42c12a0af024bf1c80e787ed5eca7
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54044508"
---
# <a name="azure-table-storage-overview"></a>Introducción a Azure Table Storage

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Table Storage es un servicio que almacena datos NoSQL estructurados en la nube y proporciona un almacén de claves y atributos con un diseño sin esquema. Como Almacenamiento de tablas carece de esquema, es fácil adaptar los datos a medida que evolucionan las necesidades de la aplicación. El acceso a los datos de Table Storage es rápido y rentable para muchos tipos de aplicaciones y, por lo general, el costo es normalmente menor que con el SQL tradicional para volúmenes parecidos de datos.

Table Storage se puede usar para almacenar conjuntos de datos flexibles, como datos de usuarios para aplicaciones web, libretas de direcciones, información de dispositivos u otros tipos de metadatos que el servicio requiera. Una tabla puede almacenar un número cualquiera de entidades y una cuenta de almacenamiento puede incluir un número cualquiera de tablas, hasta alcanzar el límite de capacidad de este tipo de cuenta.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a>Pasos siguientes

* El [Explorador de Microsoft Azure Storage](../vs-azure-tools-storage-manage-with-storage-explorer.md) es una aplicación independiente y gratuita de Microsoft que permite trabajar visualmente con los datos de Azure Storage en Windows, macOS y Linux.

* [Introducción a Azure Table Storage mediante .NET](table-storage-how-to-use-dotnet.md)

* Consulte la documentación de referencia de Table service para obtener información detallada acerca de las API disponibles:

    * [Referencia de la biblioteca de clientes de almacenamiento para .NET](https://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

    * [Referencia de API de REST](https://msdn.microsoft.com/library/azure/dd179355)
