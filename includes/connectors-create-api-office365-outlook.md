---
ms.service: logic-apps
ms.topic: include
author: ecfan
ms.author: estfan
ms.date: 11/03/2016
ms.openlocfilehash: 635a69fc8e75e3a6249728dc1df31cf839b345c6
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74789606"
---
## <a name="prerequisites"></a>Requisitos previos

* Una cuenta de Azure; puede crear una [gratuita](https://azure.microsoft.com/free)
* Una cuenta de [Office 365](https://office365.com).  

Antes de usar su cuenta de Office 365 en una aplicación lógica, autorice a la aplicación lógica a conectarse a su cuenta de Office 365. Esto lo puede hacer fácilmente dentro de la aplicación lógica en el Portal de Azure.  

Autorice a la aplicación lógica a conectarse a su cuenta de Office 365 con los pasos siguientes:

1. Cree una aplicación lógica. En el diseñador de Logic Apps, seleccione **Mostrar API administradas por Microsoft** en la lista desplegable y, luego, escriba "office 365" en el cuadro de búsqueda. Seleccione uno de los desencadenadores o una de las acciones:  
    ![paso de creación de conexión de Office 365](./media/connectors-create-api-office365-outlook/office365-sendemail.png)  
2. Si no ha creado anteriormente ninguna conexión a Office 365, se le pedirá que inicie sesión con sus credenciales de Office 365:  
    ![Paso de creación de una conexión a Office 365](./media/connectors-create-api-office365-outlook/office365-signin.png)  
3. Seleccione **Iniciar sesión** y escriba su nombre de usuario y contraseña. Seleccione **Iniciar sesión**:  
    ![paso de creación de conexión de Office 365](./media/connectors-create-api-office365-outlook/office365-usernamepassword.png)
   
    Estas credenciales se usan para autorizar a la aplicación lógica a conectarse y acceder a su cuenta de Office 365. 
4. Observe que la conexión se ha creado en el portal. Ahora, continúe con el resto de los pasos en la aplicación lógica:   
    ![Paso de creación de una conexión a Office 365](./media/connectors-create-api-office365-outlook/office365-sendemailproperties.png)  

