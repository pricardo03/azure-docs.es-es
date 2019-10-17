---
title: Rol de administrador de clústeres de Red Hat OpenShift en Azure | Microsoft Docs
description: Asignación y uso del rol de administrador de clústeres de Azure Red Hat OpenShift
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 1c676b2671b73084a2b4ae8908acb83c23a59b7b
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936927"
---
# <a name="azure-red-hat-openshift-customer-administrator-role"></a>Rol de administrador de clientes de Red Hat OpenShift en Azure

Usted es el administrador de un clúster de Azure Red Hat OpenShift. Su cuenta tiene más permisos y dispone de acceso a todos los proyectos creados por usuarios.

Cuando su cuenta lleva enlazado el rol de autorización osa-customer-admin, puede administrar automáticamente un proyecto.

> [!Note] 
> El rol de clúster osa-customer-admin no es el mismo que cluster-admin.


Por ejemplo, puede ejecutar acciones asociadas a un conjunto de verbos (`create`) para operar en un conjunto de nombres de recursos (`templates`). Para ver los detalles de estos roles y sus conjuntos de verbos y recursos, ejecute el siguiente comando:

`$ oc describe clusterrole/osa-customer-admin`

Los nombres de verbo no tienen por qué estar asignados directamente a los comandos `oc`. Suelen equivaler a los tipos de operaciones de la CLI que puede realizar. 

Por ejemplo, tener el verbo `list` significa que puede mostrar una lista de todos los objetos de un nombre de recurso (`oc get`). El verbo `get` significa que puede mostrar los detalles de un objeto concreto si conoce su nombre (`oc describe`).

## <a name="configure-the-customer-administrator-role"></a>Configuración del rol de administrador de clientes

El rol de administrador de clientes solo se puede configurar durante la creación del clúster cuando se proporciona la marca `--customer-admin-group-id`. Para más información sobre cómo configurar Azure Active Directory y el grupo de administradores, consulte [Integración de Azure Active Directory para Red Hat OpenShift en Azure](howto-aad-app-configuration.md).

## <a name="next-steps"></a>Pasos siguientes

Configuración del rol osa-customer-admin:
> [!div class="nextstepaction"]
> [Integración de Azure Active Directory con Red Hat OpenShift en Azure](howto-aad-app-configuration.md)
