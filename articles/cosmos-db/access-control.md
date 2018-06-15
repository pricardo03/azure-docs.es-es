---
title: Configuración del control de acceso en Azure Cosmos DB | Microsoft Docs
description: Obtenga información acerca de cómo establecer el control de acceso en Azure Cosmos DB.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 02/06/2018
ms.author: sngun
ms.openlocfilehash: 2dae2b6291aa7ce18cc6f612b25cd5bdcc1ba753
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34611150"
---
# <a name="access-control-in-azure-cosmos-db"></a>Control de acceso en Azure Cosmos DB

Para agregar acceso de lectura en la cuenta de Azure Cosmos DB a su cuenta de usuario, necesita que un propietario de la suscripción realice los pasos siguientes en Azure Portal.

1. Vaya a Azure Portal y seleccione su cuenta de Azure Cosmos DB.
2. Haga clic en la pestaña **Control de acceso (IAM)** y, a continuación, haga clic en **+ Agregar**.
3. En el panel **Agregar permisos**, en el cuadro **Rol**, seleccione **Rol de lector de la cuenta de Cosmos DB**.
4. En el cuadro **Asignar acceso a**, seleccione **usuario de Azure AD, grupo o aplicación**.
5. Seleccione el usuario, el grupo o la aplicación en el directorio al que desea conceder acceso.  Puede buscar en el directorio con los nombres para mostrar, dirección de correo electrónico o identificadores de objeto.
    El usuario, el grupo o la aplicación seleccionados aparecen en la lista de los miembros seleccionados.
6. Haga clic en **Save**(Guardar).

La entidad ahora puede leer los recursos de Azure Cosmos DB.
