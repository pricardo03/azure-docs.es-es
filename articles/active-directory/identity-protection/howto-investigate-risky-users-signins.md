---
title: Cómo investigar usuarios e inicios de sesión de riesgo en Azure Active Directory Identity Protection (actualizado) | Microsoft Docs
description: Aprenda a investigar usuarios e inicios de sesión de riesgo en Azure Active Directory Identity Protection (actualizado).
services: active-directory
keywords: azure active directory identity protection, detección de aplicaciones en la nube, administración de aplicaciones, seguridad, riesgo, nivel de riesgo, punto vulnerable, directiva de seguridad
documentationcenter: ''
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.assetid: e7434eeb-4e98-4b6b-a895-b5598a6cccf1
ms.service: active-directory
ms.subservice: identity-protection
ms.date: 01/25/2019
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 646119f223067fac5ee36574fb10273819251cce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60296051"
---
# <a name="how-to-investigate-risky-users-and-sign-ins"></a>Instrucciones: Investigación sobre usuarios e inicios de sesión de riesgo 


Al usar los informes de inicio de sesión y de usuarios de riesgo, puede investigar y obtener información sobre los riesgos en su entorno. Gracias a la capacidad de filtrar y clasificar los inicios de sesión y los usuarios de riesgo, puede comprender mejor las posibles intrusiones que pueden producirse en su organización. 


## <a name="risky-users-report"></a>Informe de usuarios de riesgo

Con la información que proporciona el informe de usuarios de riesgo, puede encontrar respuestas a preguntas tales como:

- ¿Qué usuarios son de alto riesgo?
- ¿Qué usuarios tienen un estado de riesgo corregido?



El primer punto de entrada de este informe es la sección **Investigar** de la página de seguridad.

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/01.png)


El informe de usuarios de riesgo tiene una vista predeterminada que muestra lo siguiente:

- Name

- Estado de riesgo

- Nivel de riesgo

- Detalle de riesgo

- Última actualización de riesgo

- Type

- Status
 

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/03.png)


Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/04.png)

Este cuadro de diálogo con columnas le permite mostrar los campos adicionales o quitar los campos que ya se están mostrando.

Si hace clic en un elemento de la vista de lista, puede obtener todos los detalles disponibles sobre él en una vista horizontal.

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/05.png)


La vista de detalles muestra:

- Información básica

- Inicios de sesión de riesgo recientes

- Eventos de riesgo que no están vinculados a ningún inicio de sesión

- Historial de riesgos



Además, puede:

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/08.png)

- Ver todos los accesos directos de inicios de sesión para ver el informe de inicios de sesión del usuario.

- Ver todos los inicios de sesión de riesgo para poder ver todos los inicios de sesión del usuario que se marcaron como de riesgo.

- Restablecer una contraseña de usuario si cree que se ha puesto en peligro la identidad del usuario.

- Descartar el riesgo de usuario si cree que los eventos de riesgo activos de un usuario son falsos positivos. Para obtener más información, consulte [how to improve the detection accuracy](howto-improve-detection-accuracy.md) (cómo mejorar la precisión de la detección).



### <a name="filter-risky-users"></a>Filtrar los usuarios de riesgo

Para restringir los datos del informe a un nivel que se adapte a sus necesidades, puede filtrar los datos de los usuarios de riesgo con los siguientes campos predeterminados:

- Name

- Nombre de usuario

- Estado de riesgo

- Nivel de riesgo

- Type

- Status

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/06.png)



El filtro **Nombre** le permite especificar el nombre o el nombre principal de usuario (UPN) que le interese.


El filtro **Estado de riesgo** le permite seleccionar:

- En riesgo
- Corregido
- Descartado


El filtro **Nivel de riesgo** le permite seleccionar:

- Alto
- Mediano
- Bajo


El filtro **Tipo** le permite seleccionar:

- Invitado
- Member

El filtro **Estado** le permite seleccionar:

- Deleted
- Active


### <a name="download-risky-users-data"></a>Descargar los datos de los usuarios de riesgo

Puede descargar los datos de usuarios de riesgo si desea trabajar con ellos fuera del portal de Azure. Al hacer clic en descarga crea un archivo CSV de los registros de 2.500 más recientes. 

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/07.png)


Puede personalizar la vista de lista si hace clic en la opción Columnas de la barra de herramientas.
 
Esto le permite mostrar los campos adicionales o quitar los campos que ya se están mostrando.
 
Para obtener más información acerca de un usuario de riesgo, haga clic en el cajón de detalles para expandirlo.

 



## <a name="risky-sign-ins-report"></a>Informe de inicios de sesión peligrosos

Con la información que proporciona el informe de inicios de sesión de riesgo, puede encontrar respuestas a preguntas tales como:

- ¿Cuántos inicios de sesión con eventos de riesgo de direcciones IP anónimas se realizaron correctamente en la última semana?

- ¿Qué usuarios se confirmaron en peligro en el último mes?

- ¿Qué usuarios tenían inicios de sesión de riesgo en el portal de Office 365?




El primer punto de entrada de este informe es la sección **Investigar** de la página de seguridad.

![Informe de inicios de sesión peligrosos](./media/howto-investigate-risky-users-signins/02.png)

El informe de inicios de sesión de riesgo tiene una vista predeterminada que muestra lo siguiente:

- Date

- Usuario

- Application

- Estado de inicio de sesión

- Estado de riesgo

- Nivel de riesgo (agregado)

- Nivel de riesgo (en tiempo real)

- Acceso condicional

- Se requiere MFA  
 

![Informe de inicios de sesión peligrosos](./media/howto-investigate-risky-users-signins/09.png)


Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/11.png)

Este cuadro de diálogo con columnas le permite mostrar los campos adicionales o quitar los campos que ya se están mostrando.

Si hace clic en un elemento de la vista de lista, puede obtener todos los detalles disponibles sobre él en una vista horizontal.

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/12.png)


La vista de detalles muestra:

- Información básica

- Información del dispositivo

- Información de riesgos

- Información de MFA

- Acceso condicional





Además, puede:

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/13.png)

- Confirmar que se encuentra en peligro 

- Confirmar que es seguro

Para obtener más información, consulte [how to improve the detection accuracy](howto-improve-detection-accuracy.md) (cómo mejorar la precisión de la detección).




### <a name="filter-risky-sign-ins"></a>Filtrar inicios de sesión de riesgo

Para restringir los datos del informe a un nivel que se adapte a sus necesidades, puede filtrar los datos de los usuarios de riesgo con los siguientes campos predeterminados:

- Usuario
- Application
- Estado de inicio de sesión
- Estado de riesgo
- Nivel de riesgo (agregado)
- Nivel de riesgo (en tiempo real)
- Acceso condicional
- Date
- Tipo de nivel de riesgo

![Informe de inicios de sesión peligrosos](./media/howto-investigate-risky-users-signins/14.png)



El filtro **Nombre** le permite especificar el nombre o el nombre principal de usuario (UPN) que le interese.

El filtro **Aplicación** le permite especificar a qué aplicación en la nube intentó obtener acceso el usuario.

El filtro **estado de inicio de sesión** le permite seleccionar:

- Todo
- Correcto
- Error


El filtro **Estado de riesgo** le permite seleccionar:

- En riesgo
- Confirmado (en peligro)
- Confirmado (seguro)
- Descartado
- Corregido


El filtro **Nivel de riesgo (agregado)** le permite seleccionar:

- Alto
- Mediano
- Bajo

El filtro **Nivel de riesgo (en tiempo real)** le permite seleccionar:

- Alto
- Mediano
- Bajo


El filtro **Acceso condicional** le permite seleccionar:

- Todo
- No aplicado
- Correcto
- Error


El filtro **Fecha** le permite definir un período de tiempo para los datos devueltos.
Los valores posibles son:

- Último mes
- Últimos 7 días
- Últimas 24 horas
- Intervalo de tiempo personalizado





### <a name="download-risky-sign-ins-data"></a>Descargar los datos de inicios de sesión de riesgo

Puede descargar los datos de inicios de sesión si desea trabajar con ellos fuera del portal de Azure. Al hacer clic en descarga crea un archivo CSV de los registros de 2.500 más recientes. 

![Informe de usuarios de riesgo](./media/howto-investigate-risky-users-signins/15.png)



## <a name="next-steps"></a>Pasos siguientes

Para obtener una visión general de Azure AD Identity Protection, consulte la [Introducción a Azure AD Identity Protection](overview-v2.md).
