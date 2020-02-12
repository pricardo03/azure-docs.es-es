---
title: 'Administración de cuentas: portal de Azure VMware Solutions (AVS)'
description: En este artículo se describe cómo administrar cuentas en el portal de Azure VMware Solutions (AVS).
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1fd7cb1c55fb664828448cef0b67ea9b16323bdf
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025374"
---
# <a name="manage-accounts-on-the-azure-vmware-solutions-avs-portal"></a>Administración de cuentas en el portal de Azure VMware Solutions (AVS)

Cuando se crea el servicio de AVS, se crea una cuenta en AVS. La cuenta se asocia a su suscripción de Azure donde se encuentra el servicio. Todos los usuarios con roles de propietario y de colaborador en la suscripción tienen acceso al portal de AVS. El identificador de suscripción de Azure y el identificador de inquilino asociados con el servicio de AVS se encuentran en la página Cuentas.

Para administrar las cuentas en el portal de AVS, [acceda al portal](access-cloudsimple-portal.md) y seleccione **Cuenta** en el menú lateral.

Seleccione **Resumen** para ver información sobre la configuración de AVS de su empresa. Se mostrará la capacidad actual de la configuración de la nube, incluido el número de nubes privadas de AVS, el almacenamiento total, la configuración del clúster de vSphere, el número de nodos y el número de núcleos de proceso. Se incluye un vínculo para comprar nodos adicionales si la configuración actual no satisface todas sus necesidades.

## <a name="email-alerts"></a>Alertas por correo electrónico

Puede agregar direcciones de correo electrónico de las personas a las que quiere notificar los cambios en la configuración de la nube privada de AVS.

1. En el área **Additional email alerts** (Alertas de correo electrónico adicionales), haga clic en **Agregar nueva**.
2. Escriba la dirección de correo electrónico.
3. Presione ENTRAR.  

Para quitar una entrada, haga clic en **X**.

## <a name="avs-operator-access"></a>Acceso de operador de AVS

Con la configuración de acceso de operador, AVS le ayuda a solucionar problemas, ya que permite que un ingeniero de soporte técnico inicie sesión en el portal de AVS. Esta opción está habilitada de forma predeterminada. Todas las acciones que el ingeniero de soporte técnico lleve a cabo cuando haya iniciado sesión en su cuenta de cliente se registrarán y estarán disponibles para su revisión en la página **Actividad** > **Auditoría**.

Haga clic en el botón de alternancia **AVS operator access enabled** (Acceso de operador de AVS habilitado) para habilitar o deshabilitar el acceso.
