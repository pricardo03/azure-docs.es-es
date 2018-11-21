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
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 2e4406a75ea1d9f1968d994ae2294b39ca7613d5
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2018
ms.locfileid: "51623866"
---
# <a name="risky-sign-ins-report-in-the-azure-active-directory-portal"></a>Informe de inicios de sesión poco seguros del portal de Azure Active Directory

Azure Active Directory (Azure AD) detecta acciones sospechosas relacionadas con las cuentas de usuario. Para cada acción detectada, se crea un registro denominado **evento de riesgo**. Para más información, consulte [Eventos de riesgo de Azure AD](concept-risk-events.md). 

Puede acceder a los informes de seguridad desde [Azure Portal](https://portal.azure.com); para ello, seleccione la hoja **Azure Active Directory** y, luego, vaya a la sección **Seguridad**. 

Es posible calcular dos informes de seguridad distintos en función de los eventos de riesgo:

- **Inicios de sesión peligrosos**: un inicio de sesión peligroso es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.

- **Usuarios marcados en riesgo**: un usuario en peligro es un indicador de una cuenta de usuario que puede haber estado en peligro. 

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/10.png)

Para aprender a configurar las directivas que desencadenan estos eventos de riesgo, consulte [Procedimiento de configuración de la directiva de riesgo del usuario](../identity-protection/howto-user-risk-policy.md).  

## <a name="who-can-access-the-risky-sign-ins-report"></a>¿Quién puede acceder al informe de inicios de sesión de riesgo?

Los informes de inicios de sesión de riesgo están disponibles para los usuarios con los siguientes roles:

- Administrador de seguridad
- Administrador global
- Lector de seguridad

Para obtener información sobre cómo asignar roles administrativos a un usuario en Azure Active Directory, consulte [Ver y asignar roles de administrador en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-manage-roles-portal).

## <a name="what-azure-ad-license-do-you-need-to-access-a-security-report"></a>¿Qué licencia de Azure AD se necesita para acceder a un informe de seguridad?  

Todas las ediciones de Azure AD le proporcionan informes sobre inicios de sesión de riesgo. Sin embargo, el nivel de granularidad del informe varía según la edición: 

- En las **ediciones Azure Active Directory Free y Basic**, puede obtener una lista de inicios de sesión de riesgo. 

- Además, la edición **Azure Active Directory Premium 1** le permite examinar algunos de los eventos de riesgo subyacentes que se han detectado en cada informe. 

- La edición **Azure Active Directory Premium 2** le proporciona la información más detallada acerca de todos los eventos de riesgo subyacentes y también le permite configurar directivas de seguridad que responden automáticamente a los niveles de riesgo configurados.

## <a name="risky-sign-ins-report-for-azure-ad-free-and-basic-edition"></a>Informe de inicios de sesión de riesgo de las ediciones Free y Basic de Azure AD

Las ediciones Free y Basic de Azure AD proporcionan una lista de inicios de sesión de riesgo que se han detectado para los usuarios. Cada registro contiene los siguientes atributos:

- **Usuario**: el nombre del usuario que se utilizó durante la operación de inicio de sesión
- **IP**: la dirección IP del dispositivo que se usó para conectarse a Azure Active Directory
- **Ubicación**: la ubicación utilizada para conectarse a Azure Active Directory
- **Hora de inicio de sesión**: la hora en que se realizó el inicio de sesión
- **Estado**: el estado del inicio de sesión

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/01.png)

Con los datos derivados de la investigación del inicio de sesión de riesgo, puede proporcionar información a Azure AD en forma de las siguientes acciones:

- Resolver
- Marcar como falso positivo
- Ignorar
- Reactivar

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/21.png)

Este informe también proporciona una opción para:

- Búsqueda de recursos
- Descargar los datos de informe

![Inicios de sesión no seguros](./media/concept-risky-sign-ins/93.png)


## <a name="risky-sign-ins-report-for-azure-ad-premium-editions"></a>Informe de inicios de sesión de riesgo para las ediciones de Azure AD Premium

El informe de inicios de sesión de riesgo de las ediciones Azure AD Premium proporciona lo siguiente:

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

- [Procedimiento de configuración de la directiva de riesgo del usuario](../identity-protection/howto-user-risk-policy.md)
- [Procedimiento de configuración de la directiva de corrección de riesgo del usuario](../identity-protection/howto-user-risk-policy.md)
- [Tipos de evento de riesgo](concept-risk-events.md)