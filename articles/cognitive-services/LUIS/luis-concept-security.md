---
title: Seguridad al colaborar
titleSuffix: Language Understanding - Azure Cognitive Services
description: El acceso a la creación está disponible para los propietarios y colaboradores. Para una aplicación privada, el acceso a los puntos de conexión está disponible para los propietarios y colaboradores.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/18/2018
ms.author: diberry
ms.openlocfilehash: 5abc2026362e19b406b34a4321e766c3229cc0b4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58894191"
---
# <a name="authoring-and-endpoint-user-access"></a>Acceso de usuario a la creación y los puntos de conexión
El acceso a la creación está disponible para los propietarios y colaboradores. Para una aplicación privada, el acceso a los puntos de conexión está disponible para los propietarios y colaboradores. Para una aplicación pública, el acceso al punto de conexión está disponible para todos los usuarios que tengan su propia cuenta de LUIS y el identificador de la aplicación pública. 

## <a name="access-to-authoring"></a>Acceso a la creación
El acceso a la aplicación desde el sitio web de [LUIS](luis-reference-regions.md#luis-website) o las [API de creación](https://aka.ms/luis-authoring-apis) está controlado por el propietario de la aplicación. 

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
|Revisar las expresiones de punto de conexión para un [aprendizaje activo](luis-how-to-review-endpoint-utterances.md)|
|Entrenar|

## <a name="access-to-endpoint"></a>Acceder al punto de conexión

El acceso para consultar el punto de conexión se controla mediante una opción de configuración de la página **Información de la aplicación** en la sección **Administrar**. 

![Configurar la aplicación como pública](./media/luis-concept-security/set-application-as-public.png)

|[Punto de conexión privado](#private-app-endpoint-security)|[Punto de conexión público](#public-app-endpoint-access)|
|:--|:--|
|Disponible para el propietario y los colaboradores|Disponible para el propietario, los colaboradores y cualquier otra persona que conozca el identificador de la aplicación|

### <a name="private-app-endpoint-security"></a>Seguridad del punto de conexión de aplicaciones privadas

El punto de conexión de una aplicación privada solo está disponible para lo siguiente:

|Clave y usuario|Explicación|
|--|--|
|Clave de creación del propietario| Hasta 1000 visitas del punto de conexión|
|Claves de creación de colaboradores| Hasta 1000 visitas del punto de conexión|
|Cualquier clave asignada a LUIS por un autor o un colaborador|Según el nivel de uso de las claves|

#### <a name="microsoft-user-accounts"></a>Cuentas de usuario de Microsoft

Los autores y colaboradores pueden asignar claves a una aplicación de LUIS privada. La cuenta de usuario de Microsoft que crea la clave de LUIS en Azure Portal debe ser del propietario de la aplicación o de un colaborador de la aplicación. No se puede asignar una clave a una aplicación privada desde otra cuenta de Azure.

Consulte [Usuario inquilino de Azure Active Directory](luis-how-to-collaborate.md#azure-active-directory-tenant-user) para más información sobre las cuentas de usuario de Active Directory. 

### <a name="public-app-endpoint-access"></a>Acceso al punto de conexión de la aplicación pública

Una vez que una aplicación se ha configurado como pública, _cualquier_ clave de creación de LUIS válida o clave del punto de conexión de LUIS puede consultar la aplicación, siempre y cuando la clave no haya alcanzado el límite de cuota del punto de conexión.

Un usuario que no sea propietario ni colaborador solo podrá acceder a una aplicación pública si se le proporciona el identificador de la aplicación. LUIS no tiene un _mercado_ público u otro medio para buscar una aplicación pública.  

Una aplicación pública se pone a disposición de los usuarios en todas las regiones para que los que tengan una clave de recurso de LUIS basada en regiones puedan acceder a la aplicación en cualquier región que esté asociada a la clave de recurso.

## <a name="microsoft-user-accounts"></a>Cuentas de usuario de Microsoft

Los autores y colaboradores pueden agregar claves para LUIS en la página Publicar. La cuenta de usuario de Microsoft que crea la clave de LUIS en Azure Portal debe ser del propietario de la aplicación o de un colaborador de la aplicación. 

Consulte [Usuario inquilino de Azure Active Directory](luis-how-to-collaborate.md#azure-active-directory-tenant-user) para más información sobre las cuentas de usuario de Active Directory. 

## <a name="securing-the-endpoint"></a>Proteger el punto de conexión 

Puede controlar quién puede ver su clave del punto de conexión de LUIS llamándola en un entorno de servidor a servidor. Si usa LUIS desde un bot, la conexión entre el bot y LUIS ya es segura. Si está llamando al punto de conexión de LUIS directamente, debe crear una API de servidor (por ejemplo, una [función](https://azure.microsoft.com/services/functions/) de Azure) con acceso controlado (como [AAD](https://azure.microsoft.com/services/active-directory/)). Cuando se llama a la API de servidor y se comprueba la autenticación y la autorización, pase la llamada a LUIS. Aunque esta estrategia no impide los ataques de tipo "Man in the middle", ofusca el punto de conexión de los usuarios, le permite realizar un seguimiento de acceso y agregar un registro de respuesta del punto de conexión (como [Application Insights](https://azure.microsoft.com/services/application-insights/)).  

## <a name="security-compliance"></a>Seguridad y cumplimiento
 
[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-security-compliance.md)]

## <a name="next-steps"></a>Pasos siguientes

Vea [Best practices](luis-concept-best-practices.md) (Prácticas recomendadas) para obtener información sobre cómo usar las intenciones y entidades para obtener las mejores predicciones.
