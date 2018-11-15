---
title: Claves únicas en Azure Cosmos DB
description: Obtenga información sobre cómo usar las claves únicas en la base de datos de Azure Cosmos DB.
author: aliuy
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/30/2018
ms.author: andrl
ms.openlocfilehash: 006d0ef28d82a7648a56b3bf871c5a3afd6a55a6
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51624427"
---
# <a name="unique-keys-in-azure-cosmos-db"></a>Claves únicas en Azure Cosmos DB

Las claves únicas proporcionan la capacidad de agregar una capa de integridad de datos a un contenedor de Cosmos. Cree una directiva de clave única al crear un contenedor de Cosmos. Con las claves únicas, se garantiza la exclusividad de uno o varios valores dentro de una partición lógica (puede garantizar la exclusividad con la [clave de partición](partition-data.md)). Una vez creado un contenedor con una directiva de clave única, impide la creación de cualquier elemento duplicado nuevo o actualizado dentro de una partición lógica, según especifique la restricción de clave única. La clave de partición combinada con la clave única garantiza la exclusividad de un elemento dentro del ámbito del contenedor.

Por ejemplo, considere un contenedor de Cosmos con la dirección de correo electrónico como restricción de clave única y `CompanyID` como clave de partición. Mediante la configuración de una clave única con la dirección de correo electrónico del usuario, se asegura de que cada elemento tiene una dirección de correo electrónico única dentro de un ámbito `CompanyID` determinado. No se pueden crear dos elementos con direcciones de correo electrónico duplicadas y con el mismo valor de clave de partición.  

Si desea proporcionar a los usuarios la capacidad de crear varios elementos con la misma dirección de correo electrónico, pero no con el mismo nombre, apellidos y dirección de correo electrónico, puede agregar otras rutas de acceso a la directiva de claves únicas. En lugar de crear una clave única basada en la dirección de correo electrónico, puede crear también una clave única con una combinación de nombre, apellido y correo electrónico (una clave única compuesta). En este caso, se permite cada combinación única de los tres valores dentro de un ámbito `CompanyID` determinado. Por ejemplo, el contenedor puede contener elementos con los valores siguientes, donde cada elemento respeta la restricción de clave única.

|CompanyID|Nombre|Apellidos|Dirección de correo electrónico|
|---|---|---|---|
|Contoso|Gaby|Duperre|gaby@contoso.com |
|Contoso|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Gaby|Duperre|gaby@fabrikam.com|
|Fabrikam|Ivan|Duperre|gaby@fabrikam.com|
|Fabrikam|   |Duperre|gaby@fabraikam.com|
|Fabrikam|   |   |gaby@fabraikam.com|

Si ha intentado insertar otro elemento con cualquiera de las combinaciones enumeradas en la tabla anterior, podría recibir un error que indica que no se ha cumplido la restricción de clave única. Como resultado, recibirá un mensaje tipo "Ya existe un recurso con el id. o nombre especificado" o "Ya existe un recurso con el id., nombre o índice único especificado".  

## <a name="defining-a-unique-key"></a>Definición de una clave única

Puede definir claves únicas solo cuando se crea un contenedor de Cosmos. Una clave única se limita a una partición lógica. En el ejemplo anterior, si crea particiones del contenedor según el código postal, al final tendrá elementos duplicados en cada partición lógica. Tenga en cuenta las siguientes propiedades al crear claves únicas:

* No puede actualizar un contenedor existente para que use una clave única distinta. En otras palabras, una vez creado un contenedor con una directiva de clave única, la directiva no se puede cambiar.

* Si desea establecer una clave única para un contenedor existente, debe crear un contenedor con la restricción de clave única y usar la herramienta de migración de datos apropiada para mover los datos del contenedor existente al nuevo. Para los contenedores de SQL, use la [Herramienta de migración de datos](import-data.md) para mover los datos. Para los contenedores de MongoDB, utilice [mongoimport.exe o mongorestore.exe](mongodb-migrate.md) para mover los datos.

* Una directiva de clave única puede tener un máximo de dieciséis valores de ruta de acceso (por ejemplo /firstName, /lastName, /address/zipCode, etc.). Cada directiva de clave única puede tener un máximo de diez combinaciones o restricciones de clave única, y las rutas de acceso combinadas para cada restricción de índice único no deben exceder los 60 bytes. En el ejemplo anterior, el nombre, el apellido y la dirección de correo electrónico de forma conjunta constituyen solo una restricción, y utiliza tres de las dieciséis posibles rutas de acceso.

* Cuando un contenedor tiene una directiva de clave única, los cargos de unidad de solicitud para crear, actualizar y eliminar un elemento son ligeramente superiores.

* No se admiten las claves únicas dispersas. Si faltan algunos valores de ruta de acceso única, se tratan como valores NULL, que participan en la restricción de exclusividad. Por lo tanto, solo puede haber un único elemento con un valor NULL para cumplir esta restricción.

* Los nombres de clave única distinguen mayúsculas de minúsculas. Por ejemplo, considere un contenedor con la restricción de clave única establecida en /address/zipcode. Si los datos tienen un campo denominado ZipCode, Cosmos DB inserta "null" como clave única porque "código postal" no es lo mismo que "Código Postal". Debido a esta distinción entre mayúsculas y minúsculas, todos los demás registros con Código Postal no se podrán insertar porque un valor "null" duplicado infringirá la restricción de clave única.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre las [particiones lógicas](partition-data.md)
