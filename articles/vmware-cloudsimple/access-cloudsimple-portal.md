---
title: 'Acceso a Azure VMware Solutions (AVS): portal'
description: Se describe cómo acceder a Azure VMware Solutions (AVS) desde Azure Portal.
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/04/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c0b0601b9d2cf505aa5b25e10875338352cab771
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015957"
---
# <a name="access-azure-vmware-solutions-avs-from-the-azure-portal"></a>Acceso a Azure VMware Solutions (AVS) desde Azure Portal

El inicio de sesión único se admite para acceder al portal de AVS. Después de iniciar sesión en Azure Portal, puede acceder al portal de AVS sin volver a iniciar sesión. La primera vez que acceda al portal de AVS, se le solicitará que autorice la aplicación [Autorización del servicio de AVS](#consent-to-avs-service-authorization-application). La autorización es una acción que solo se realiza una vez.

## <a name="before-you-begin"></a>Antes de empezar

Los usuarios con los roles **Propietario** y **Colaborador** integrados pueden acceder al portal de AVS. Los roles deben configurarse en el grupo de recursos donde está implementado el servicio AVS. Los roles también pueden configurarse en el objeto de servicio AVS. Para obtener más información sobre la comprobación de su rol, consulte el artículo [Visualización de asignaciones de roles](https://docs.microsoft.com/azure/role-based-access-control/check-access). Los usuarios con los roles **Propietario** y **Colaborador** integrados son los únicos que pueden acceder al portal de AVS. Los roles deben configurarse en la suscripción. Para obtener más información sobre la comprobación de su rol, consulte el artículo [Visualización de asignaciones de roles](https://docs.microsoft.com/azure/role-based-access-control/check-access).

Si utiliza roles personalizados, el rol debe tener cualquiera de las operaciones que se especifican debajo de ```Actions```.  Para obtener información acerca de los roles personalizados, consulte [Roles personalizados en los recursos de Azure](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). Si alguna de las operaciones es una parte de ```NotActions```, el usuario no puede acceder al portal de AVS. 

```
Microsoft.VMwareCloudSimple/*
Microsoft.VMwareCloudSimple/*/write
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*
Microsoft.VMwareCloudSimple/dedicatedCloudServices/*/write
```

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Inicie sesión en Azure Portal en [https://portal.azure.com](https://portal.azure.com).

## <a name="access-the-avs-portal"></a>Acceso al portal de AVS

1. Seleccione **Todos los servicios**.

2. Busque **AVS Services** (Servicios de AVS).

3. Seleccione el servicio de AVS en el que quiere crear la nube privada de AVS.

4. En la página **Información general**, haga clic en **Go to the AVS portal** (Ir al portal de AVS). Si accede al portal de AVS desde Azure Portal por primera vez, se le solicitará que autorice la aplicación [Autorización del servicio de AVS](#consent-to-avs-service-authorization-application). 

    ![Inicio del portal de AVS](media/launch-cloudsimple-portal.png)

> [!NOTE]
> Si selecciona una operación de nube privada (como la creación o expansión de una nube privada) directamente desde Azure Portal, el portal de AVS se abre en la página indicada.

En el portal de AVS, seleccione **Inicio** en el menú lateral para mostrar información de resumen sobre su nube privada de AVS. Se muestran los recursos y la capacidad de su nube privada de AVS, junto con las alertas y las tareas que requieren atención. Para las tareas comunes, haga clic en los iconos con nombre en la parte superior de la página.

![Página principal](media/cloudsimple-portal-home.png)

## <a name="consent-to-avs-service-authorization-application"></a>Consentimiento para la aplicación Autorización del servicio de AVS

Al iniciar el portal de AVS desde Azure Portal por primera vez, se requiere su consentimiento para la aplicación Autorización del servicio de AVS. Seleccione **Aceptar** para conceder los permisos solicitados y acceder al portal de AVS.

![Consentimiento para Autorización del servicio de AVS: administradores](media/cloudsimple-azure-consent.png)

Si tiene el privilegio de administrador global, puede dar el consentimiento para su organización. Seleccione **Consentimiento en nombre de la organización**.

![Consentimiento para Autorización del servicio de AVS: administrador global](media/cloudsimple-azure-consent-global-admin.png)

Si sus permisos no permiten acceder al portal de AVS, póngase en contacto con el administrador global del inquilino para que le conceda los permisos necesarios. Un administrador global puede dar consentimiento en nombre de la organización.

![Consentimiento para Autorización del servicio de AVS: requiere administradores](media/cloudsimple-azure-consent-requires-administrator.png)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [crear una nube privada](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Más información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)
