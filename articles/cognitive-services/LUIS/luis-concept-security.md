---
title: 'Descripción del acceso a las aplicaciones de LUIS: Azure | Microsoft Docs'
description: Obtenga información sobre cómo obtener acceso a la creación con LUIS.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: 44380e12e6d095e8d40675af0b6b2fddc5e4c4e9
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264274"
---
# <a name="authoring-and-endpoint-user-access"></a>Acceso de usuario a la creación y los puntos de conexión
El acceso a la creación está disponible para los propietarios y colaboradores. Para una aplicación privada, el acceso a los puntos de conexión está disponible para los propietarios y colaboradores. Para una aplicación pública, el acceso al punto de conexión está disponible para todos los usuarios que tengan su propia cuenta de LUIS y el identificador de la aplicación pública. 

## <a name="access-to-authoring"></a>Acceso a la creación
El acceso a la aplicación desde el sitio web de [LUIS][LUIS] o las [API de creación](https://aka.ms/luis-authoring-apis) está controlado por el propietario de la aplicación. 

El propietario y todos los colaboradores tienen acceso a la creación de la aplicación. 

|El acceso de creación incluye|Notas|
|--|--|
|Agregar o quitar claves del punto de conexión||
|Exportar la versión||
|Exportar los registros de punto de conexión||
|Importar la versión||
|Hacer pública la aplicación|Cuando una aplicación es pública, cualquiera que tenga una clave de creación o del punto de conexión puede consultar la aplicación.|
|Modificar el modelo|
|Publicar|
|Revisar las expresiones de punto de conexión para un [aprendizaje activo](label-suggested-utterances.md)|
|Entrenar|

## <a name="access-to-endpoint"></a>Acceder al punto de conexión
El acceso al punto de conexión para consultar LUIS está controlado mediante el valor **Público** de la aplicación en la página **Configuración**. La consulta del punto de conexión de una aplicación la comprueba una clave autorizada con los restantes accesos de cuota. La consulta del punto de conexión de una aplicación pública también tiene que proporcionar una clave del punto de conexión (de la persona que realiza la consulta) que también se comprueba para los accesos de cuota restantes. 

La clave del punto de conexión se pasa en la cadena de consulta de la solicitud GET o en el encabezado de la solicitud POST.

![Configurar la aplicación como pública](./media/luis-concept-security/set-application-as-public.png)

### <a name="private-app-endpoint-security"></a>Seguridad del punto de conexión de aplicaciones privadas
El punto de conexión de una aplicación privada solo está disponible para lo siguiente:

|Clave y usuario|Explicación|
|--|--|--|
|Clave de creación del propietario| Hasta 1000 visitas del punto de conexión|
|Claves de creación de colaboradores| Hasta 1000 visitas del punto de conexión|
|Las claves del punto de conexión agregadas desde la página **[Publicar](publishapp.md)**|El propietario y los colaboradores pueden agregar claves del punto de conexión|

Otras claves de creación o del punto de conexión **no** tienen acceso.

### <a name="public-app-endpoint-access"></a>Acceso al punto de conexión de la aplicación pública
Configure la aplicación como **pública** en la página **Configuración** de la aplicación. Una vez que una aplicación se ha configurado como pública, _cualquier_ clave de creación de LUIS válida o clave del punto de conexión de LUIS puede consultar la aplicación, siempre y cuando la clave no haya alcanzado el límite de cuota del punto de conexión.

Un usuario que no sea propietario ni colaborador solo podrá acceder a una aplicación pública si se le proporciona el identificador de la aplicación. LUIS no tiene un _mercado_ público u otro medio para buscar una aplicación pública.  

## <a name="microsoft-user-accounts"></a>Cuentas de usuario de Microsoft
Los autores y colaboradores pueden agregar claves para LUIS en la página Publicar. La cuenta de usuario de Microsoft que crea la clave de LUIS en Azure Portal debe ser del propietario de la aplicación o de un colaborador de la aplicación. 

<!--
### Individual consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then you can provide individual consent as part of the login process. 

### Administrator consent
If the Microsoft user account is part of an Azure Active Directory (AAD), and the active directory doesn't allow users to give consent, then the administrator can give individual consent via the method discussed in this [blog](https://blogs.technet.microsoft.com/tfg/2017/10/15/english-tips-to-manage-azure-ad-users-consent-to-applications-using-azure-ad-graph-api/). 
-->
## <a name="securing-the-endpoint"></a>Proteger el punto de conexión 
Puede controlar quién puede ver su clave del punto de conexión de LUIS llamándola en un entorno de servidor a servidor. Si usa LUIS desde un bot, la conexión entre el bot y LUIS ya es segura. Si está llamando al punto de conexión de LUIS directamente, debe crear una API de servidor (por ejemplo, una [función](https://azure.microsoft.com/services/functions/) de Azure) con acceso controlado (como [AAD](https://azure.microsoft.com/services/active-directory/)). Cuando se llama a la API de servidor y se comprueba la autenticación y la autorización, pase la llamada a LUIS. Aunque esta estrategia no impide los ataques de tipo "Man in the middle", ofusca el punto de conexión de los usuarios, le permite realizar un seguimiento de acceso y agregar un registro de respuesta del punto de conexión (como [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Seguridad y cumplimiento
LUIS superó con éxito la auditoría ISO 27001:2013 e ISO 27018:2014 con cero disconformidades (hallazgos) en el informe de auditoría. Además, LUIS también obtuvo la certificación más alta de STAR de CSA con el nivel Gold por la valoración de la capacidad de madurez. Azure es el único proveedor de servicios en la nube público que ha conseguido esta certificación. Para obtener más detalles, puede encontrar el LUIS incluido en la instrucción de ámbito actualizada en el documento de [introducción al cumplimiento](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) de Azure mencionado en las páginas de ISO del [Centro de confianza](https://www.microsoft.com/en-us/trustcenter/compliance/iso-iec-27001).  

## <a name="next-steps"></a>Pasos siguientes

Vea [Best practices](luis-concept-best-practices.md) (Prácticas recomendadas) para obtener información sobre cómo usar las intenciones y entidades para obtener las mejores predicciones.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website