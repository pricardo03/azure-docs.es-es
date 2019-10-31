---
title: Registros de aprovisionamiento en el portal de Azure Active Directory (versión preliminar) | Microsoft Docs
description: Introducción a los informes de actividad de aprovisionamiento en el portal de Azure Active Directory
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 07/29/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d48aa3ead28ab0b0a22478a0c4183995483058a
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "70983503"
---
# <a name="provisioning-reports-in-the-azure-active-directory-portal-preview"></a>Informes de aprovisionamiento en el portal de Azure Active Directory (versión preliminar)

La arquitectura de los informes de Azure Active Directory (Azure AD) consta de los siguientes componentes:

- **Actividad** 
    - **Inicios de sesión**: información sobre el uso de aplicaciones administradas y actividades de inicio de sesión de usuario.
    - **Registros de auditoría**: los [registros de auditoría](concept-audit-logs.md) proporcionan información de la actividad del sistema sobre la administración de usuarios y grupos, aplicaciones administradas y actividades de directorio. - 
    - **Registros de aprovisionamiento**: proporcionan la actividad del sistema sobre el usuario, los grupos y los roles aprovisionados por el servicio de aprovisionamiento de Azure AD. 

- **Seguridad** 
    - **Inicios de sesión de riesgo**: un [inicio de sesión de riesgo](concept-risky-sign-ins.md) es un indicador de un intento de inicio de sesión que puede haber realizado alguien que no es el propietario legítimo de una cuenta de usuario.
    - **Usuarios marcados en riesgo**: un [usuario en riesgo](concept-user-at-risk.md) es un indicador de una cuenta de usuario que puede haber estado en peligro.

Este tema ofrece una visión general del informe de aprovisionamiento.

## <a name="prerequisites"></a>Requisitos previos

### <a name="who-can-access-the-data"></a>¿Quién puede acceder a los datos?
* Los usuarios con los roles Administrador de seguridad, Lector de seguridad, Lector de informes, Administrador de aplicaciones y Administrador de aplicaciones en la nube.
* Administradores globales


### <a name="what-azure-ad-license-do-you-need-to-access-provisioning-activities"></a>¿Qué licencia de Azure AD se necesita para acceder a las actividades de aprovisionamiento?

El inquilino debe tener una licencia de Azure AD Premium asociada para ver el informe de actividades de aprovisionamiento al completo. Consulte [Introducción a Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md) para actualizar la edición de Azure Active Directory. 

## <a name="provisioning-logs"></a>Registros de aprovisionamiento

Los registros de aprovisionamiento proporcionan respuestas a las siguientes preguntas:

* ¿Qué grupos se han creado correctamente en ServiceNow?
* ¿Cómo se han importado los roles de Amazon Web Services?
* ¿Qué usuarios no se han creado correctamente en DropBox?

Se puede tener acceso a los registros de aprovisionamiento si se selecciona **Registros de aprovisionamiento** en la sección **Supervisión** de la hoja **Azure Active Directory** en [Azure Portal](https://portal.azure.com). Algunos registros de aprovisionamiento pueden tardar hasta dos horas en aparecer en el portal.

![Registros de aprovisionamiento](./media/concept-provisioning-logs/access-provisioning-logs.png "Registros de aprovisionamiento")


Un registro de aprovisionamiento tiene una vista de lista predeterminada que muestra:

- La identidad
- La acción
- El sistema de origen
- El sistema de destino
- El estado
- La fecha


![Columnas predeterminadas](./media/concept-provisioning-logs/default-columns.png "Columnas predeterminadas")

Puede personalizar la vista de lista, haga clic en **Columnas** en la barra de herramientas.

![Selector de columnas](./media/concept-provisioning-logs/column-chooser.png "Selector de columnas")

Esto le permite mostrar los campos adicionales o quitar los campos que ya se están mostrando.

![Columnas disponibles](./media/concept-provisioning-logs/available-columns.png "Columnas disponibles")

Seleccione un elemento de la vista de lista para obtener información más detallada.

![Información detallada](./media/concept-provisioning-logs/steps.png "Filtrar")


## <a name="filter-provisioning-activities"></a>Filtrado de las actividades de aprovisionamiento

Para restringir los datos del informe a un nivel que se adapte a sus necesidades, puede filtrar los datos de aprovisionamiento con los campos predeterminados que se indican a continuación. Tenga en cuenta que los valores de los filtros se rellenan dinámicamente en función del inquilino. Por ejemplo, si no tiene ningún evento de creación en el inquilino, no habrá una opción de filtro para la creación.

- Identidad
- .
- Sistema de origen
- Sistema de destino
- Status
- Date


![Filter](./media/concept-provisioning-logs/filter.png "Filtrar")

El filtro **Identidad** le permite especificar el nombre o la identidad que le interesa. Esta identidad podría ser un usuario, un grupo, un rol u otro objeto. Puede buscar por nombre o por identificador de objeto. El identificador varía según el escenario. Por ejemplo, al aprovisionar un objeto de Azure AD en SalesForce, el identificador de origen es el identificador de objeto del usuario en Azure AD, mientras que el identificador de destino es el identificador del usuario en Salesforce. Al aprovisionar de Workday a Active Directory, el identificador de origen es el identificador de empleado del trabajador de Workday. Tenga en cuenta que el nombre del usuario puede no estar siempre presente en la columna de identidad. Siempre habrá un identificador. 

El filtro **Sistema de origen** le permite especificar desde dónde se aprovisiona la identidad. Por ejemplo, al aprovisionar un objeto de Azure AD a ServiceNow, el sistema de origen es Azure AD. 

El filtro **Sistema de destino** le permite especificar en dónde se aprovisiona la identidad. Por ejemplo, al aprovisionar un objeto de Azure AD a ServiceNow, el sistema de destino es ServiceNow. 

El filtro **Estado** le permite seleccionar:

- Todo
- Correcto
- Error
- Skipped

El filtro **Acción** permite filtrar:

- Crear 
- Actualizar
- Eliminar
- Disable
- Otros

El filtro **Fecha** le permite definir un período de tiempo para los datos devueltos.  
Los valores posibles son:

- 1 mes
- 7 días
- 30 días
- 24 horas
- Intervalo de tiempo personalizado

Cuando se selecciona un período de tiempo personalizado, puede configurar una fecha de inicio y una fecha de finalización.


Además de los campos predeterminados, cuando se selecciona, también puede incluir los siguientes campos en el filtro:

- **Id. de trabajo**: es un identificador de trabajo único que está asociado a cada aplicación para la que se ha habilitado el aprovisionamiento.   

- **Id. de ciclo**: identifica de forma única el ciclo de aprovisionamiento. Puede compartir este identificador para permitir buscar el ciclo en el que se ha producido este evento.

- **Id. de cambio**: identificador único para el evento de aprovisionamiento. Puede compartir este identificador para permitir buscar el evento de aprovisionamiento.   



  

## <a name="provisioning-details"></a>Detalles de aprovisionamiento 

Al seleccionar un elemento en la vista de lista de aprovisionamiento, obtendrá más información sobre dicho elemento.
Los detalles se agrupan en función de las categorías siguientes:

- Pasos

- Solución de problemas y recomendaciones

- Propiedades modificadas

- Resumen


![Filter](./media/concept-provisioning-logs/provisioning-tabs.png "Pestañas")



### <a name="steps"></a>Pasos

En la pestaña **Pasos** se describen los pasos necesarios para aprovisionar un objeto. El aprovisionamiento de un objeto puede constar de cuatro pasos: 

- Importación del objeto
- Determinación de si el objeto está en el ámbito
- Coincidencia del objeto entre el origen y el destino
- Aprovisionamiento del objeto (realización de una acción, como crear, actualizar, eliminar o deshabilitar)



![Filter](./media/concept-provisioning-logs/steps.png "Filtrar")


### <a name="troubleshoot-and-recommendations"></a>Solución de problemas y recomendaciones


La pestaña **Solución de problemas y recomendaciones** proporciona el código de error y el motivo. La información del error solo está disponible en caso de error. 


### <a name="modified-properties"></a>Propiedades modificadas

En la pestaña **Propiedades modificadas** se muestran el valor anterior y el nuevo. En los casos en los que no hay ningún valor anterior, la columna con este valor está en blanco. 


### <a name="summary"></a>Resumen

En la pestaña **Resumen** se proporciona información general sobre lo que sucedió y los identificadores del objeto en el sistema de origen y de destino. 

## <a name="what-you-should-know"></a>Qué debería saber

- Azure Portal almacena durante 30 días los datos de aprovisionamiento detectados si tiene una edición Premium y durante siete días si tiene una edición gratuita.

- Puede usar el atributo Id. de cambio como identificador único. Esto es útil, por ejemplo, al hablar con el soporte técnico del producto.

- Actualmente no hay ninguna opción para descargar los datos de aprovisionamiento.

- Actualmente no se presta soporte técnico para el análisis de registros.

- Al acceder a los registros de aprovisionamiento desde el contexto de una aplicación, los eventos no se filtran automáticamente según la aplicación específica, como sucede en el caso de los registros de auditoría.

## <a name="next-steps"></a>Pasos siguientes

* [Comprobación del estado de aprovisionamiento de usuarios](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user)
* [Problema al configurar el aprovisionamiento de usuarios para una aplicación de la galería de Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-config-problem)


