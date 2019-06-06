---
title: Solución de acceso VMware Azure CloudSimple - Portal
description: Describe cómo tener acceso a la solución VMware CloudSimple portal de Azure portal
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 30882899e5be4101ae3d77f9840d8bdef567e53f
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66676989"
---
# <a name="accessing-the-vmware-solution-by-cloudsimple-portal-from-azure-portal"></a>Obtener acceso a la solución VMware CloudSimple portal de Azure portal

Inicio de sesión único es compatible con acceso al portal CloudSimple. Después de iniciar sesión el portal de Azure, puede acceder al portal de CloudSimple sin volver a iniciar sesión. La primera vez accede el portal CloudSimple se le pedirá que autorice el [CloudSimple servicio autorización](#consent-to-cloudsimple-service-authorization-application) aplicación.  La autorización es una acción única.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-cloudsimple-portal"></a>Acceso al portal de CloudSimple

1. Seleccione **Todos los servicios**.

2. Busque **CloudSimple servicios**.

3. Seleccione el servicio CloudSimple en el que desea crear su propia nube privada.

4. En el **Introducción** página, haga clic en **ir al portal CloudSimple**.  Si tiene acceso al portal CloudSimple desde el portal de Azure por primera vez, se le pedirá autorizar el [CloudSimple servicio autorización](#consent-to-cloudsimple-service-authorization-application) aplicación. 

    ![Iniciar CloudSimple portal](media/launch-cloudsimple-portal.png)

> [!TIP]
> Si selecciona una operación de nube privada (como la creación o expansión de una nube privada) directamente desde el portal de Azure, el portal CloudSimple se abre en la página indicada.

En el portal CloudSimple, seleccione **inicio** en el menú lateral para mostrar información de resumen sobre sus nubes privadas. Se muestran los recursos y la capacidad de sus nubes privadas, junto con alertas y las tareas que requieren atención. Para tareas comunes, haga clic en los iconos con nombre en la parte superior de la página.

![Página principal](media/cloudsimple-portal-home.png)

## <a name="consent-to-cloudsimple-service-authorization-application"></a>Dar su consentimiento a la aplicación de autorización de servicio CloudSimple

Iniciar el portal de CloudSimple desde el portal de Azure por primera vez, requiere su consentimiento para la aplicación CloudSimple autorización del servicio.  Seleccione **Accept** para conceder los permisos solicitados y acceder al portal CloudSimple. 

![Dar su consentimiento a la autorización de servicio CloudSimple - los administradores](media/cloudsimple-azure-consent.png)

Si tiene privilegios de administrador global, puede dar su consentimiento para su organización.  Seleccione **dar su consentimiento en nombre de su organización**.

![Dar su consentimiento a la autorización de servicio CloudSimple - administrador global](media/cloudsimple-azure-consent-global-admin.png)

Si los permisos no permitan el acceso al portal CloudSimple, póngase en contacto con el administrador global del inquilino para conceder los permisos necesarios.  Un administrador global puede dar su consentimiento en nombre de su organización.

![Dar su consentimiento a la autorización de servicio CloudSimple: requiere que los administradores](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear una nube privada](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Obtenga información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)