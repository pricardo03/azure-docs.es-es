---
title: Rol de administrador de clústeres de Red Hat OpenShift en Azure | Microsoft Docs
description: Asignación y uso del rol de administrador de clústeres de Red Hat OpenShift en Azure
services: container-service
author: mjudeikis
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 6d7aacea5e34e21513452880448227a1ca5f9b67
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71712839"
---
# <a name="azure-red-hat-openshift-customer-administrator-role"></a>Rol de administrador de clientes de Red Hat OpenShift en Azure

Como administrador de clústeres de ARO (Red Hat OpenShift en Azure) de un clúster de OpenShift, su cuenta ha aumentado los permisos y el acceso a todos los proyectos creados por el usuario.

Cuando su cuenta lleva enlazado el rol de autorización osa-customer-admins, puede administrar automáticamente un proyecto.

> [!Note] 
> osa-customer-admin clusterrole no es lo mismo que cluster-admin clusterrole


Por ejemplo, puede ejecutar acciones asociadas a un conjunto de verbos (`create`) para operar en un conjunto de nombres de recursos (`templates`). Para ver los detalles de estos roles y sus conjuntos de verbos y recursos, ejecute el siguiente comando:

`$ oc describe clusterrole/osa-customer-admin`

Los nombres de verbos no se asignan todos necesariamente a comandos oc, sino que se equiparan más generalmente con los tipos de operaciones de la CLI que puede realizar. Por ejemplo, tener el verbo `list` significa que puede mostrar una lista de todos los objetos de un nombre de recurso dado (`oc get`), mientras que el verbo `get` significa que puede mostrar los detalles de un objeto concreto si conoce su nombre (`oc describe`).

## <a name="how-to-configure-customer-administrator-role"></a>Configuración del rol de administrador de clientes

El rol de administrador de clientes solo se puede configurar durante la creación del clúster cuando se proporciona la marca `--customer-admin-group-id`. Guía de procedimientos para configurar Azure Active Directory y el grupo de administradores: [Integración de Azure Active Directory con Red Hat OpenShift en Azure](howto-aad-app-configuration.md)

## <a name="next-steps"></a>Pasos siguientes

Configuración del rol osa-customer-admin:
> [!div class="nextstepaction"]
> [Integración de Azure Active Directory con Red Hat OpenShift en Azure](howto-aad-app-configuration.md)
