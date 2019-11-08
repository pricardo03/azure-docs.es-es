---
title: Administración de áreas de trabajo
titleSuffix: ML Studio (classic) Azure
description: Administración del acceso a las áreas de trabajo de Azure Machine Learning Studio (clásico) e implementación y administración de servicios web de la API de Machine Learning
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 02/27/2017
ms.openlocfilehash: 0ec619e765dde39f6d4c24cdadaab8bff4729f79
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73684760"
---
# <a name="manage-an-azure-machine-learning-studio-classic-workspace"></a>Administración de un área de trabajo de Azure Machine Learning Studio (clásico)

> [!NOTE]
> Para obtener información acerca de cómo administrar servicios web en el portal Servicios web Machine Learning, consulte [Administración de un servicio web mediante el portal Servicios web Azure Machine Learning](manage-new-webservice.md).
> 
> 

Puede administrar áreas de trabajo de Machine Learning Studio (clásico) en Azure Portal.



## <a name="use-the-azure-portal"></a>Uso de Azure Portal

Para administrar un área de trabajo de Studio en Azure Portal (clásico):

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) mediante una cuenta de administrador de la suscripción de Azure.
2. En el cuadro de búsqueda de la parte superior de la página, escriba "áreas de trabajo de Machine Learning Studio (clásico)" y, después, seleccione **Áreas de trabajo de Machine Learning Studio (clásico)** .
3. Haga clic en el área de trabajo que desea administrar.

Además de la información de administración de recursos estándar y de las opciones disponibles, puede:

- Ver **Propiedades**: esta página muestra la información del área de trabajo y de los recursos, y puede cambiar la suscripción y el grupo de recursos con el que esta área de trabajo está conectado.
- **Resincronizar las claves de almacenamiento**: el área de trabajo mantiene las claves de la cuenta de almacenamiento. Si la cuenta de almacenamiento cambia las claves, puede hacer clic en **Resincronizar claves** para sincronizar las claves con el área de trabajo.

Para administrar los servicios web asociados a esta área de trabajo de Studio (clásico), use el Portal de servicios web Machine Learning. Para obtener una información más completa, consulte [Administración de un servicio web mediante el portal Servicios web Azure Machine Learning](manage-new-webservice.md).

> [!NOTE]
> Para implementar o administrar nuevos servicios web, es preciso que se le haya asignado un rol de colaborador o administrador en la suscripción en la que se implementa el servicio web. Si invita a otro usuario a un área de trabajo de Machine Learning Studio (clásico), debe asignarle un rol de colaborador o administrador en la suscripción para que pueda implementar o administrar servicios web. 
> 
>Para más información acerca de cómo establecer los permisos de acceso, consulte [Administración del acceso mediante RBAC y Azure Portal](../../role-based-access-control/role-assignments-portal.md).

## <a name="next-steps"></a>Pasos siguientes
* Más información sobre cómo [implementar Machine Learning con plantillas de Azure Resource Manager](deploy-with-resource-manager-template.md). 
