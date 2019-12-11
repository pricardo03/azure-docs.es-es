---
title: Configuración del nuevo agente de aprovisionamiento en la nube de Azure AD Connect
description: En este tema se describe cómo instalar el aprovisionamiento en la nube.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 12/02/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 956962503dcb3a6355d455ff96744143c5965d1e
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795471"
---
# <a name="azure-ad-connect-cloud-provisioning-new-configuration"></a>Configuración del nuevo aprovisionamiento en la nube de Azure AD Connect

Una vez instalado el agente, debe iniciar sesión en Azure Portal y configurar el aprovisionamiento.  Para habilitar el agente, realice los siguientes pasos.

## <a name="configure-provisioning"></a>Configuración del aprovisionamiento
Para configurar el aprovisionamiento, siga estos pasos:

1.  En el portal de Azure AD, haga clic en **Azure Active Directory**.
2.  Haga clic en **Azure AD Connect**.
3.  Seleccione **Administración del aprovisionamiento (versión preliminar)** 
![](media/how-to-configure/manage1.png).

4.  Haga clic en **Nueva configuración**.
5.  En la pantalla de configuración, el dominio local se ha rellenado previamente.
6. Escriba un **correo electrónico de notificación de alerta**. Se notificará a este correo electrónico 
7. cuando el aprovisionamiento no sea correcto.  
8. Mueva el selector a **Habilitar** y haga clic en **Guardar**.
![](media/tutorial-single-forest/configure2.png)

## <a name="scoping-provisioning-to-specific-users-and-groups"></a>Aprovisionamiento del ámbito para usuarios y grupos específicos
Es posible limitar el ámbito del agente para sincronizar solo usuarios y grupos específicos. Puede usar el ámbito mediante grupos de AD locales o unidades organizativas. No se pueden configurar grupos ni unidades organizativas dentro de una configuración. 

1.  En el portal de Azure AD, haga clic en **Azure Active Directory**.
2.  Haga clic en **Azure AD Connect**.
3.  Seleccione **Administración del aprovisionamiento (versión preliminar)** .
4.  En **Configuración**, haga clic en su configuración.  
![](media/how-to-configure/scope1.png)

5.  En **Configurar**, seleccione **Todos los usuarios** para cambiar el ámbito de la regla de configuración.
![](media/how-to-configure/scope2.png)

6. A la derecha, puede cambiar el ámbito para que incluya solo los grupos de seguridad; para ello, especifique el nombre distintivo del grupo y haga clic en **Agregar**.
![](media/how-to-configure/scope3.png)

7. O bien cámbielo para que incluya solo unidades organizativas específicas. Haga clic en **Listo** y en **Guardar**.
![](media/how-to-configure/scope4.png)


## <a name="restart-provisioning"></a>Reinicio del aprovisionamiento 
Si no desea esperar a la siguiente ejecución programada, puede desencadenar la ejecución del aprovisionamiento mediante el botón para reiniciar el aprovisionamiento. 
1.  En el portal de Azure AD, haga clic en **Azure Active Directory**.
2.  Haga clic en **Azure AD Connect**.
3.  Seleccione **Administración del aprovisionamiento (versión preliminar)** .
4.  En **Configuración**, haga clic en su configuración.  
![](media/how-to-configure/scope1.png)

5.  En la parte superior, haga clic en **Reiniciar aprovisionamiento**.

## <a name="removing-a-configuration"></a>Eliminación de una configuración
Si desea eliminar una configuración, puede seguir estos pasos.

1.  En el portal de Azure AD, haga clic en **Azure Active Directory**.
2.  Haga clic en **Azure AD Connect**.
3.  Seleccione **Administración del aprovisionamiento (versión preliminar)** .
4.  En **Configuración**, haga clic en su configuración.  
![](media/how-to-configure/scope1.png)

5.  En la parte superior, haga clic en **Eliminar**.
![](media/how-to-configure/remove1.png)

>[!IMPORTANT]
>No hay confirmación antes de eliminar una configuración. Asegúrese de que se trata de la acción que desea realizar antes de hacer clic en **Eliminar**.


## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
