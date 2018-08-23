---
title: Uso del Explorador de Azure Cosmos DB para administrar los datos | Microsoft Docs
description: El Explorador de Azure Cosmos DB es una interfaz independiente basada en web que permite ver y administrar los datos almacenados en Azure Cosmos DB.
services: cosmos-db
author: deborahc
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: dech
ms.openlocfilehash: 8d1bd0d4331937e37307140e17e5aed1a6e3b0ff
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2018
ms.locfileid: "42144434"
---
# <a name="use-azure-cosmos-db-explorer-to-manage-your-data"></a>Uso del Explorador de Azure Cosmos DB para administrar los datos 

El Explorador de Azure Cosmos DB es una interfaz independiente basada en web que permite ver y administrar los datos almacenados en Azure Cosmos DB. El Explorador de Azure Cosmos DB es equivalente a la pestaña **Explorador de datos** existente que está disponible en Azure Portal cuando crea una cuenta de Azure Cosmos DB. Las principales ventajas del Explorador de Azure Cosmos DB sobre el Explorador de datos existente son las siguientes:

* Tiene un estado real de pantalla completa para ver los datos, ejecutar las consultas, los procedimientos almacenados, los desencadenadores y ver sus resultados.  

* Puede proporcionar acceso de lectura o de lectura y escritura temporal o permanente a la cuenta de base de datos y sus colecciones a otros usuarios que no tienen acceso a Azure Portal ni a la suscripción.  

* Puede compartir los resultados de la consulta con otros usuarios que no tienen acceso a Azure Portal ni a la suscripción.  

## <a name="access-azure-cosmos-db-explorer"></a>Acceso al Explorador de Azure Cosmos DB

1. Inicie sesión en [Azure Portal](https://portal.azure.com/). 

2. En **Todos los recursos**, busque y vaya a la cuenta de Azure Cosmos DB, seleccione Claves y copie la **cadena de conexión principal**.  

3. Vaya a https://cosmos.azure.com/, pegue la cadena de conexión y seleccione **Conectar**. Con la cadena de conexión, puede acceder al Explorador de Azure Cosmos DB sin límite de tiempo.  

   Si quiere proporcionar a otros usuarios acceso temporal a su cuenta de Azure Cosmos DB, puede hacerlo con las direcciones URL de acceso de lectura y escritura o de acceso de lectura. 

4. Abra la hoja **Explorador de datos**, seleccione **Open Full Screen** (Abrir pantalla completa). En el cuadro de diálogo emergente, puede ver dos direcciones URL de acceso: **Read-Write** (Lectura y escritura) y **Read** (Lectura). Estas direcciones URL le permiten compartir temporalmente su cuenta de Azure Cosmos DB con otros usuarios. El acceso a la cuenta expira en 24 horas. Después, puede reconectarse con una dirección URL de acceso nueva o mediante la cadena de conexión. 

   **Read-Write** (Lectura y escritura): cuando comparte la dirección URL de lectura y escritura con otros usuarios, estos pueden ver y modificar las bases de datos, las colecciones, las consultas y otros recursos asociados con esa cuenta específica.

   **Read** (Lectura): cuando comparte la dirección URL de solo lectura con otros usuarios, estos pueden ver las bases de datos, las colecciones, las consultas y otros recursos asociados con esa cuenta específica. Por ejemplo, si quiere compartir los resultados de una consulta con sus compañeros de equipo que no tienen acceso a Azure Portal o a su cuenta de Azure Cosmos DB, puede proporcionarles esta dirección URL.

   Elija el tipo de acceso con el que quisiera abrir la cuenta y haga clic en **Abrir**. Después de abrir el explorador, la experiencia es igual que la que tendría con la pestaña Explorador de datos en Azure Portal.   

   ![Apertura del Explorador de Azure Cosmos DB](./media/data-explorer/open-data-explorer-with-access-url.png)

## <a name="known-issues"></a>Problemas conocidos

Actualmente, la experiencia de **Open Full Screen** (Abrir pantalla completa) que permite compartir acceso temporal de lectura y escritura o de lectura todavía no es compatible con cuentas de Azure Cosmos DB Gremlin y Table API. Puede seguir viendo sus cuentas de Gremlin y Table API si pasa la cadena de conexión al Explorador de Azure Cosmos DB. 

## <a name="next-steps"></a>Pasos siguientes
Ya aprendió a empezar a trabajar con el Explorador de Azure Cosmos DB para administrar los datos y continuación podrá:

* Empezar a definir [consultas](sql-api-sql-query-reference.md) con la sintaxis de SQL y realizar [programación del lado servidor](programming.md) mediante el uso de procedimientos almacenados, UDF y desencadenadores. 