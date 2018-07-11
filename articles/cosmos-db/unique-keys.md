---
title: Claves únicas en Azure Cosmos DB | Microsoft Docs
description: Obtenga información acerca de cómo usar las claves únicas en la base de datos de Azure Cosmos DB.
services: cosmos-db
keywords: restricción de clave única, infracción de restricción de clave única
author: rafats
manager: kfile
editor: monicar
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: rafats
ms.openlocfilehash: d12109efbb157b1e0c15b1a4c0d005fa98c44858
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35261107"
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Claves únicas en Azure Cosmos DB

Las claves únicas proporcionan a los desarrolladores la capacidad de agregar una capa de integridad de datos a su base de datos. Al crear una directiva de clave única al crear un contenedor, se garantiza la unicidad de uno o más valores por [clave de partición](partition-data.md). Una vez creado un contenedor con una directiva de clave única, impide la creación de cualquier elemento nuevo o actualizado con valores que dupliquen los valores especificados por la restricción de clave única.   

> [!NOTE]
> Las claves únicas se admiten en las versiones más recientes de los SDK de SQL de [.NET](sql-api-sdk-dotnet.md) y [.NET Core](sql-api-sdk-dotnet-core.md) y de la [API de MongoDB](mongodb-feature-support.md#unique-indexes). Table API y Graph API no admiten claves únicas en este momento. 
> 
>

## <a name="use-case"></a>Caso de uso

Como ejemplo, veamos cómo una base de datos de usuario asociada a una [aplicación social](use-cases.md#web-and-mobile-applications) podría beneficiarse de tener una directiva de clave única en las direcciones de correo electrónico. Al hacer que la dirección de correo electrónico del usuario sea una clave única, se asegura de que cada registro tenga una dirección de correo electrónico única y que no se puedan crear nuevos registros con direcciones de correo electrónico duplicadas. 

Si desea que los usuarios puedan crear varios registros con la misma dirección de correo electrónico, pero no con el mismo nombre, apellidos y dirección de correo electrónico, puede añadir otras rutas de acceso a la directiva de claves únicas. Por lo tanto, en lugar de crear una clave única basada en una dirección de correo electrónico, puede crear una clave única que sea una combinación de nombre, apellido y correo electrónico. En este caso, se permite cada combinación única de las tres rutas de acceso, por lo que la base de datos podría contener elementos que tengan los siguientes valores de ruta de acceso. Cada uno de estos registros aprobaría la directiva de clave única.  

**Valores permitidos para la clave única de nombre, apellidos y correo electrónico**

|Nombre|Apellidos|Dirección de correo electrónico|
|---|---|---|
|Gaby|Duperre|gaby@contoso.com |
|Gaby|Duperre|gaby@fabrikam.com|
|Ivan|Duperre|gaby@fabrikam.com|
|    |Duperre|gaby@fabrikam.com|
|    |       |gaby@fabraikam.com|

Si ha intentado insertar otro registro con cualquiera de las combinaciones enumeradas en la tabla anterior, podría recibir un error que indica que no se ha cumplido la restricción de clave única. El error que Azure Cosmos DB devolvió es "Ya existe un recurso con el id. o nombre especificado". O "Ya existe un recurso con el id., nombre o índice único especificado". 

## <a name="using-unique-keys"></a>Uso de claves únicas

Las claves únicas deben definirse cuando se crea el contenedor y la clave única se explora en la clave de partición. Para crear sobre el ejemplo anterior, si realiza una partición basada en el código postal, podría tener duplicados los registros de la tabla en cada partición.

No puede actualizar un contenedor existente para que use claves únicas.

Cuando se crea un contenedor con una directiva de clave única, esta directiva no se puede cambiar a menos que se vuelva a crear el contenedor. Si tiene datos existentes sobre los que desea implementar claves únicas, cree el nuevo contenedor y utilice después la herramienta de migración de datos apropiada para mover los datos al nuevo contenedor. Para los contenedores de SQL, use la [Herramienta de migración de datos](import-data.md). Para los contenedores de MongoDB, utilice [mongoimport.exe o mongorestore.exe](mongodb-migrate.md).

En cada clave única se puede incluir un máximo de 16 valores de ruta de acceso (por ejemplo /firstName, /lastName, /address/zipCode, etc.). 

Cada directiva de clave única puede tener un máximo de 10 combinaciones o restricciones de clave única, y las rutas de acceso combinadas para todas las propiedades de índice únicas no deben superar los 60 caracteres. Por lo tanto, el ejemplo anterior que usa el nombre, el apellido y la dirección de correo electrónico es solo una restricción, y utiliza tres de las 16 posibles rutas de acceso disponibles. 

Los cargos por unidad de solicitud para crear, actualizar y eliminar un elemento son ligeramente superiores cuando hay una directiva de clave única en el contenedor. 

No se admiten las claves únicas dispersas. Si faltan valores para algunas rutas de acceso únicas, se tratan como un valor nulo especial, que participa en la restricción de unicidad.

## <a name="sql-api-sample"></a>Ejemplo de API de SQL

En el ejemplo de código siguiente se muestra cómo crear un nuevo contenedor de SQL con dos restricciones de clave única. La primera restricción es la del nombre, apellido y correo electrónico descrita en el ejemplo anterior. La segunda restricción es la dirección y el código postal de los usuarios. Un archivo JSON de ejemplo que utiliza las rutas de acceso en esta directiva de clave única sigue el ejemplo de código. 

```csharp
// Create a collection with two separate UniqueKeys, one compound key for /firstName, /lastName,
// and /email, and another for /address/zipCode.
private static async Task CreateCollectionIfNotExistsAsync(string dataBase, string collection)
{
    try
    {
        await client.ReadDocumentCollectionAsync(UriFactory.CreateDocumentCollectionUri(dataBase, collection));
    }
    catch (DocumentClientException e)
    {
        if (e.StatusCode == System.Net.HttpStatusCode.NotFound)
        {
            DocumentCollection myCollection = new DocumentCollection();
            myCollection.Id = collection;
            myCollection.PartitionKey.Paths.Add("/pk");
            myCollection.UniqueKeyPolicy = new UniqueKeyPolicy
            {
                UniqueKeys =
                new Collection<UniqueKey>
                {
                    new UniqueKey { Paths = new Collection<string> { "/firstName" , "/lastName" , "/email" }}
                    new UniqueKey { Paths = new Collection<string> { "/address/zipcode" } },
          }
            };
            await client.CreateDocumentCollectionAsync(
                UriFactory.CreateDatabaseUri(dataBase),
                myCollection,
                new RequestOptions { OfferThroughput = 2500 });
        }
        else
        {
            throw;
        }
    }
```

Documento JSON de ejemplo.

```json
{
    "id": "1",
    "pk": "1234",
    "firstName": "Gaby",
    "lastName": "Duperre",
    "email": "gaby@contoso.com",
    "address": 
        {            
            "line1": "100 Some Street",
            "line2": "Unit 1",
            "city": "Seattle",
            "state": "WA",
            "zipcode": 98012
        }
    
}
```
> [!NOTE]
> Observe que el nombre de clave único distingue mayúsculas de minúsculas. Como se muestra en el ejemplo anterior, el nombre único está establecido para /dirección/código postal. Si los datos tendrán Código Postal, insertará "null" en la clave única porque código postal no es lo mismo que Código Postal. Debido a esta distinción entre mayúsculas y minúsculas, todos los demás registros con Código Postal no se podrán insertar porque un valor "null" duplicado infringirá la restricción de clave única.

## <a name="mongodb-api-sample"></a>Ejemplo de API de MongoDB

El ejemplo de comando siguiente muestra cómo crear un índice único en los campos de nombre, apellido y correo electrónico de la colección de usuarios para la API de MongoDB. Esto garantiza la unicidad de una combinación de los tres campos en todos los documentos de la colección. Para las colecciones de la API de MongoDB, se crea el índice único después de crear la colección, pero antes de rellenarla.

```
db.users.createIndex( { firstName: 1, lastName: 1, email: 1 }, { unique: true } )
```
## <a name="configure-unique-keys-by-using-azure-portal"></a>Configuración de claves únicas mediante Azure Portal

En las secciones anteriores encontrará ejemplos de código que mostrarán cómo puede definir restricciones de clave única cuando se crea una colección mediante la API de MongoDB o la API de SQL. También es posible definir claves únicas cuando se crea una colección a través de la interfaz de usuario web en Azure Portal. 

- Vaya al **Explorador de datos** de la cuenta de Cosmos DB
- Haga clic en **Nueva colección**
- En la sección Claves únicas, puede agregar las restricciones de clave única que desea si hace clic en **Add unique key** (Agregar clave única)

![Definición de claves únicas en el Explorador de datos](./media/unique-keys/unique-keys-azure-portal.png)

- Si quiere crear una restricción de clave única en la ruta de acceso lastName, agregue `/lastName`.
- Si quiere crear una restricción de clave única para la combinación lastName firstName, agregue `/lastName,/firstName`

Cuando termine, haga clic en **Aceptar** para crear la colección.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha aprendido a crear claves únicas para los elementos de una base de datos. Si está creando un contenedor por primera vez, consulte [Partición y escalado en Azure Cosmos DB](partition-data.md) pues las claves únicas y las claves de partición dependen unas de otras. 


