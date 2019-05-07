---
title: Control de acceso basado en roles en Azure Cosmos DB con la integración de Azure Active Directory
description: Obtenga información sobre cómo Azure Cosmos DB proporciona protección de bases de datos con la integración de Active directory (RBAC).
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mjbrown
ms.openlocfilehash: e43341e59e807fdc20033d909beadb9d582149b5
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65078946"
---
# <a name="role-based-access-control-in-azure-cosmos-db"></a>Control de acceso basado en roles en Azure Cosmos DB

Azure Cosmos DB proporciona control de acceso integrado basado en roles (RBAC) para escenarios comunes de administración de Azure Cosmos DB. Una persona que tiene un perfil en Azure Active Directory puede asignar estos roles RBAC a usuarios, grupos, entidades de servicio o administra las identidades para conceder o denegar el acceso a recursos y operaciones en recursos de Azure Cosmos DB. Las asignaciones de roles se limitan a acceso de plano de control solo, lo que incluye el acceso a las cuentas de Azure Cosmos, las bases de datos, contenedores y ofrece (rendimiento).

## <a name="built-in-roles"></a>Roles integrados

Estos son los roles integrados compatibles con Azure Cosmos DB:

|**Rol integrado**  |**Descripción**  |
|---------|---------|
|[Colaborador de cuentas de DocumentDB](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)   | Puede administrar cuentas de Azure Cosmos DB.  |
|[Lector de la cuenta de COSMOS DB](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)  | Puede leer los datos de cuentas de Azure Cosmos DB.        |
|[Operador de copia de seguridad de COSMOS](../role-based-access-control/built-in-roles.md#cosmosbackupoperator)     |  Puede enviar una solicitud de restauración de una base de datos de Azure Cosmos o un contenedor.       |
|[Operador de COSMOS DB](../role-based-access-control/built-in-roles.md#cosmos-db-operator)  | Puede aprovisionar cuentas de Azure Cosmos, las bases de datos y contenedores, pero no puede tener acceso a las claves necesarias para tener acceso a los datos.         |

> [!IMPORTANT]
> Soporte técnico RBAC en Azure Cosmos DB se aplica a solo las operaciones de plano de control. Las operaciones de plano de datos se protegen mediante las claves maestras o tokens de recursos. Para obtener más información, consulte [proteger el acceso a datos en Azure Cosmos DB](secure-access-to-data.md)

## <a name="identity-and-access-management-iam"></a>Administración de identidad y acceso (IAM)

El **control de acceso (IAM)** panel en el portal de Azure se utiliza para configurar el control de acceso basado en roles en los recursos de Azure Cosmos. Los roles se aplican a los usuarios, grupos, entidades de servicio y las identidades administradas en Active Directory. Puede usar funciones integradas o funciones personalizadas para usuarios y grupos. Captura de pantalla siguiente muestra la integración de Active Directory (RBAC) con control de acceso (IAM) en el portal de Azure:

![Control de acceso (IAM) en Azure Portal: demostración de la seguridad de bases de datos](./media/role-based-access-control/database-security-identity-access-management-rbac.png)

## <a name="custom-roles"></a>Roles personalizados

Además de los roles integrados, los usuarios también pueden crear [roles personalizados](../role-based-access-control/custom-roles.md) en Azure y se aplican estos roles a entidades de servicio en todas las suscripciones dentro de su inquilino de Active Directory. Roles personalizados proporcionan a los usuarios una manera para crear definiciones de roles RBAC con un conjunto personalizado de recurso de operaciones del proveedor. Para obtener información sobre qué operaciones están disponibles para la creación de roles personalizados para Azure Cosmos DB, consulte [las operaciones del proveedor de recursos de Azure Cosmos DB](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

## <a name="next-steps"></a>Pasos siguientes

- [¿Qué es el control de acceso basado en roles (RBAC) para recursos de Azure](../role-based-access-control/overview.md)
- [Roles personalizados en los recursos de Azure](../role-based-access-control/custom-roles.md)
- [Operaciones del proveedor de recursos de Azure Cosmos DB](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)
