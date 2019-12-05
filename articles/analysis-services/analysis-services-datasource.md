---
title: Orígenes de datos admitidos en Azure Analysis Services | Microsoft Docs
description: Describe los orígenes de datos y los conectores admitidos para los modelos de datos tabulares de 1200 y superiores en Azure Analysis Services.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c92baf5c97597a0161f402cc458e90bb3e637d6c
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74170660"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Orígenes de datos admitidos en Azure Analysis Services

Los orígenes de datos y los conectores que se muestran en Obtener datos o en el Asistente para importación de Visual Studio corresponden tanto a Azure Analysis Services como a SQL Server Analysis Services. Sin embargo, no todos los orígenes de datos y los conectores que aparecen son compatibles con Azure Analysis Services. Los tipos de orígenes de datos con los que se puede establecer conexión dependen de muchos factores, como el nivel de compatibilidad del modelo, los conectores de datos disponibles, el tipo de autenticación, los proveedores y la compatibilidad con la puerta de enlace de datos local. 

## <a name="azure-data-sources"></a>Orígenes de datos de Azure

|Origen de datos  |En memoria  |DirectQuery  |
|---------|---------|---------|
|Azure SQL Database<sup>[2](#azsqlmanaged)</sup>     |   Sí      |    Sí      |
|Azure SQL Data Warehouse     |   Sí      |   Sí       |
|Azure Blob Storage<sup>[1](#tab1400a)</sup>     |   Sí       |    Sin      |
|Azure Table Storage<sup>[1](#tab1400a)</sup>    |   Sí       |    Sin      |
|Azure Cosmos DB<sup>[1](#tab1400a)</sup>     |  Sí        |  Sin        |
|Azure Data Lake Store (Gen1)<sup>[1](#tab1400a)</sup>, <sup>[4](#gen2)</sup>      |   Sí       |    Sin      |
|Azure HDInsight HDFS<sup>[1](#tab1400a)</sup>     |     Sí     |   Sin       |
|Azure HDInsight Spark<sup>[1](#tab1400a)</sup>, <sup>[3](#databricks)</sup>     |   Sí       |   Sin       |
||||

<a name="tab1400a">1</a>: Tabular 1400 y modelos posteriores solamente.   
<a name="azsqlmanaged">2</a>: Compatible con Instancia administrada de Azure SQL Database. Dado que la instancia administrada se ejecuta dentro de una red virtual de Azure con una dirección IP privada, el punto de conexión público debe estar habilitado en la instancia. Si no está habilitado, se requiere una puerta de enlace de datos local.    
<a name="databricks">3</a>: Actualmente no se admite Azure Databricks con el conector de Spark.   
<a name="gen2">4</a>: el conector ADLS Gen2 no se admite actualmente; no obstante, se puede usar el conector de Blob Storage con un origen de datos ADLS Gen2.


**Proveedor**   
Los modelos En memoria y DirectQuery que se conectan a orígenes de datos de Azure utilizan el proveedor de datos .NET Framework para SQL Server.

## <a name="other-data-sources"></a>Otros orígenes de datos

Para conectarse a los orígenes de datos locales desde el servidor Azure Analysis Services, se necesita una puerta de enlace local. Cuando se usa una puerta de enlace, se requieren proveedores de 64 bits.

### <a name="in-memory-and-directquery"></a>En memoria y DirectQuery

|Origen de datos | Proveedor en memoria | Proveedor de DirectQuery |
|  --- | --- | --- |
| SQL Server |Microsoft OLE DB Driver for SQL Server MSOLEDBSQL (recomendado), SQL Server Native Client 11.0, proveedor de datos .NET Framework para SQL Server | Proveedor de datos .NET Framework para SQL Server |
| SQL Server Data Warehouse |Microsoft OLE DB Driver for SQL Server MSOLEDBSQL (recomendado), SQL Server Native Client 11.0, proveedor de datos .NET Framework para SQL Server | Proveedor de datos .NET Framework para SQL Server |
| Oracle | Proveedor OLE DB para Oracle, Proveedor de datos de Oracle para .NET |Proveedor de datos de Oracle para .NET |
| Teradata |Proveedor OLE DB para Teradata, Proveedor de datos de Teradata para .NET |Proveedor de datos de Teradata para .NET |
| | | |

### <a name="in-memory-only"></a>Solo En memoria

|Origen de datos  |  
|---------|
|Base de datos de Access     |  
|Active Directory<sup>[1](#tab1400b)</sup>     |  
|Analysis Services     |  
|Analytics Platform System     |  
|Archivo CSV  |
|Dynamics CRM<sup>[1](#tab1400b)</sup>     |  
|Libro de Excel     |  
|Exchange<sup>[1](#tab1400b)</sup>     |  
|Carpeta<sup>[1](#tab1400b)</sup>     |
|IBM Informix<sup>[1](#tab1400b)</sup> (Beta) |
|Documento JSON<sup>[1](#tab1400b)</sup>     |  
|Líneas de archivo binario<sup>[1](#tab1400b)</sup>     | 
|Base de datos MySQL     | 
|Fuente OData<sup>[1](#tab1400b)</sup>     |  
|Consulta ODBC     | 
|OLE DB     |   
|Base de datos de PostgreSQL<sup>[1](#tab1400b)</sup>    | 
|Objetos de Salesforce<sup>[1](#tab1400b)</sup> |  
|Informes de Salesforce<sup>[1](#tab1400b)</sup> |
|SAP HANA<sup>[1](#tab1400b)</sup>    |  
|SAP Business Warehouse<sup>[1](#tab1400b)</sup>    |  
|Lista de SharePoint<sup>[1](#tab1400b)</sup>, <sup>[2](#filesSP)</sup>     |   
|Base de datos de Sybase     |  
|Archivo TXT  |
|Tabla XML<sup>[1](#tab1400b)</sup>    |  
||
 
<a name="tab1400b">1</a>: Tabular 1400 y modelos posteriores solamente.   
<a name="filesSP">2</a>: no se admite archivos en SharePoint en el entorno local.

## <a name="specifying-a-different-provider"></a>Especificación de un proveedor diferente

Los modelos de datos de Azure Analysis Services pueden requerir distintos proveedores de datos al conectarse a algunos orígenes de datos. En algunos casos, los modelos tabulares se conectan a orígenes de datos mediante proveedores nativos como SQL Server Native Client (SQLNCLI11) pueden devolver un error. Si utiliza proveedores nativos distintos de MSOLEDBSQL, puede aparecer el mensaje de error: **El proveedor "SQLNCLI11.1" no está registrado**. Por otro lado, si tiene un modelo de DirectQuery que se conecta a orígenes de datos locales y utiliza proveedores nativos, puede aparecer un mensaje de error similar a este: **Error al crear el conjunto de filas OLE DB. Sintaxis incorrecta cerca de "LIMIT"** .

Cuando se migra un modelo tabular de SQL Server Analysis Services local a Azure Analysis Services, puede ser necesario cambiar el proveedor.

**Para especificar un proveedor**

1. En Visual Studio > **Explorador de modelos tabulares** > **Orígenes de datos**, haga clic con el botón derecho en una conexión de origen de datos y luego en **Editar origen de datos**.
2. En **Editar conexión**, haga clic en **Avanzadas** para abrir la ventana de propiedades avanzadas.
3. En **Establecer propiedades avanzadas** > **Proveedores**, seleccione el proveedor adecuado.

## <a name="impersonation"></a>Suplantación
En algunos casos, puede ser necesario especificar otra cuenta de suplantación. La cuenta de suplantación se puede especificar en Visual Studio o SSMS.

Para orígenes de datos locales:

* Si se utiliza la autenticación de SQL, la suplantación debe ser la Cuenta de servicio.
* Si se utiliza la autenticación de Windows, establezca la contraseña y el usuario de Windows. Para SQL Server, se admite la autenticación de Windows con una cuenta de suplantación específica solo para los modelos de datos en memoria.

Para orígenes de datos en la nube:

* Si se utiliza la autenticación de SQL, la suplantación debe ser la Cuenta de servicio.

## <a name="oauth-credentials"></a>Credenciales de OAuth

En el caso de los modelos tabulares con el nivel de compatibilidad 1400 y superiores, Azure SQL Database, Azure SQL Data Warehouse, Dynamics 365 y la lista de SharePoint admiten las credenciales de OAuth. Azure Analysis Services administra la actualización de tokens para los orígenes de datos de OAuth para evitar los tiempos de espera con las operaciones de actualización de ejecución prolongadas. Para generar tokens válidos, establezca las credenciales mediante SSMS.

## <a name="next-steps"></a>Pasos siguientes
[Puerta de enlace local](analysis-services-gateway.md)   
[Administración del servidor](analysis-services-manage.md)   

