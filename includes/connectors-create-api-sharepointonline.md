---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: 0cabc58d856c09accd9b1924fe63d6518b1cb9ef
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62130003"
---
Para poder establecer la conexión con **SharePoint Online**, tiene que proporcionar su identidad (nombre de usuario y contraseña, credenciales de tarjeta inteligente, etc.) a SharePoint Online. Una vez que se ha autenticado, podrá usar el conector de SharePoint Online en la aplicación lógica. 

Mientras se encuentre en el diseñador de la aplicación lógica, siga estos pasos para iniciar sesión en SharePoint y crear la **conexión** que se usará en la aplicación lógica:

1. Escriba SharePoint en el cuadro de búsqueda y espere a que la búsqueda devuelva todos los desencadenadores y acciones relacionados con SharePoint Online:   
   ![Configurar SharePoint][1]  
2. Seleccione el desencadenador **SharePoint Online - When a file is created** (SharePoint Online: cuando se crea un archivo).  
3. Seleccione **Inicio de sesión en SharePoint Online**:   
   ![Configurar SharePoint][2]    
4. Proporcione sus credenciales de SharePoint para iniciar sesión para autenticarse con SharePoint    
   ![Configurar SharePoint][3]     
5. Una vez completada la autenticación, se le redirigirá a la aplicación lógica. Y eso es todo, la conexión se ha creado. Observe el mensaje en la parte inferior que indica que ahora está conectado a SharePoint.  
   ![Configurar SharePoint][4]  
6. A continuación, puede agregar otros desencadenadores y acciones que necesita para completar la aplicación lógica.   

[1]: ./media/connectors-create-api-sharepointonline/connectionconfig1.png
[2]: ./media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ./media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ./media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ./media/connectors-create-api-sharepointonline/connectionconfig5.png
