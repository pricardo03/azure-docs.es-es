---
title: Informe de inicios de sesión poco seguros del portal de Azure Active Directory | Microsoft Docs
description: Obtenga información acerca del informe de inicios de sesión poco seguros del portal de Azure Active Directory
services: active-directory
author: priyamohanram
manager: mtillman
ms.assetid: 7728fcd7-3dd5-4b99-a0e4-949c69788c0f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/14/2017
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 38ae18dca08b50a90102149d7e44169c956a1c0e
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "48869642"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Informe de inicios de sesión poco seguros del portal de Azure Active Directory

Con los informes de seguridad de Azure Active Directory (Azure AD), puede obtener información acerca de la probabilidad de que haya cuentas de usuario en peligro en su entorno. 

Azure AD detecta acciones sospechosas relacionadas con las cuentas de usuario. Para cada acción detectada, se crea un registro denominado *evento de riesgo*. Para más información, consulte [Azure Active Directory risk events](concept-risk-events.md) (Eventos de riesgo de Azure Active Directory). 

Los eventos de riesgo detectados se utilizan para calcular:

- **Inicios de sesión peligrosos**: un inicio de sesión peligroso es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario. Para más información, consulte [Configuración de la directiva de riesgo de inicio de sesión](../identity-protection/howto-sign-in-risk-policy.md). 

- **Usuarios marcados en riesgo**: un usuario en peligro es un indicador de una cuenta de usuario que puede haber estado en peligro. Para más información, consulte [Procedimiento de configuración de la directiva de riesgo del usuario](../identity-protection/howto-user-risk-policy.md).  

En [Azure Portal](https://portal.azure.com), puede encontrar los informes de seguridad en la hoja **Azure Active Directory** de la sección **Seguridad**. 

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/10.png)

## <a name="who-can-access-the-risky-sign-ins-report"></a>¿Quién puede acceder al informe de inicios de sesión de riesgo?

Los informes de inicios de sesión de riesgo están disponibles para los usuarios con los siguientes roles:

- Administrador de seguridad
- Administrador global
- Lector de seguridad

Para obtener información sobre cómo asignar roles administrativos a un usuario en Azure Active Directory, consulte [Ver y asignar roles de administrador en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal).

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>¿Qué licencia de Azure AD se necesita para acceder a un informe de seguridad?  

Todas las ediciones de Azure Active Directory le proporcionan informes sobre inicios de sesión de riesgo.  
Sin embargo, el nivel de granularidad del informe varía según la edición: 

- En las **ediciones Azure Active Directory Free y Basic** ya puede obtener una lista de inicios de sesión de riesgo. 

- En la edición **Azure Active Directory Premium 1** se extiende este modelo, con lo que también puede examinar algunos de los eventos de riesgo subyacentes que se han detectado para cada informe. 

- La edición **Azure Active Directory Premium 2** le proporciona la información más detallada acerca de todos los eventos de riesgo subyacentes y también le permite configurar directivas de seguridad que responden automáticamente a los niveles de riesgo configurados.

## <a name="azure-active-directory-free-and-basic-edition"></a>Edición gratuita y básica de Azure Active Directory

Las ediciones gratuita y básica de Azure Active Directory proporcionan una lista de inicios de sesión peligrosos que se han detectado para los usuarios. Este informe muestra:

- **Usuario**: el nombre del usuario que se utilizó durante la operación de inicio de sesión
- **IP**: la dirección IP del dispositivo que se usó para conectarse a Azure Active Directory
- **Ubicación**: la ubicación utilizada para conectarse a Azure Active Directory
- **Hora de inicio de sesión**: la hora en que se realizó el inicio de sesión
- **Estado**: el estado del inicio de sesión


![Inicios de sesión no seguros](./media/concept-risky-sign-ins/01.png)

Con los datos derivados de la investigación del inicio de sesión no seguro, puede proporcionar información a Azure Active Directory en forma de las siguientes acciones:

- Resolver
- Marcar como falso positivo
- Ignorar
- Reactivar

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/21.png)



Este informe proporciona una opción para:

- Búsqueda de recursos
- Descargar los datos de informe


![Inicios de sesión no seguros](./media/concept-risky-sign-ins/93.png)


## <a name="azure-active-directory-premium-editions"></a>Ediciones Azure Active Directory Premium

El informe de inicios de sesión poco seguros de las ediciones Azure Active Directory Premium proporciona lo siguiente:

- Información agregada sobre los [tipos de eventos de riesgo](concept-risk-events.md) que se han detectado

- Una opción para descargar el informe


![Inicios de sesión no seguros](./media/concept-risky-sign-ins/456.png)


Cuando selecciona un evento de riesgo, obtiene una vista detallada del informe para este evento de riesgo que le permite:

- Una opción para configurar una [directiva de corrección de riesgos de usuario](../identity-protection/howto-user-risk-policy.md)  

- Revisar la escala de tiempo de detección del evento de riesgo  

- Revisar una lista de usuarios para los que se ha detectado este evento de riesgo

- Cierre manualmente los eventos de riesgo. 


![Inicios de sesión no seguros](./media/concept-risky-sign-ins/457.png)

Cuando selecciona un usuario, obtiene una vista detallada del informe para este usuario que le permite:

- Abrir la vista de todos los inicios de sesión

- Restablezca la contraseña del usuario.

- Descartar todos los eventos

- Investigar los eventos de riesgo notificados para el usuario. 


![Inicios de sesión no seguros](./media/concept-risky-sign-ins/324.png)


Para investigar un evento de riesgo, seleccione uno de la lista.  
Se abre la hoja **Detalles** de este evento de riesgo. En la hoja **Detalles**, tiene la opción de cerrar manualmente un evento de riesgo o de volver a activar un evento de riesgo cerrado manualmente. 


![Inicios de sesión no seguros](./media/concept-risky-sign-ins/325.png)





## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de Azure Active Directory Identity Protection, consulte [Azure Active Directory Identity Protection](../active-directory-identityprotection.md).

