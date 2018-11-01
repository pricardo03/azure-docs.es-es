---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: fe726986b3f93ab3bb447b8973727a658ac1c706
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132862"
---
### <a name="prerequisites"></a>Requisitos previos
* Una cuenta de [GoogleDrive](https://www.google.com/drive/).  

Antes de poder usar la cuenta de GoogleDrive en una aplicación lógica, debe autorizar a la aplicación lógica a conectarse a dicha cuenta. Por suerte, esto se puede hacer fácilmente dentro de la aplicación lógica en Azure Portal.  

Aquí se explica cómo autorizar a la aplicación lógica para conectarse a su cuenta de GoogleDrive:  

1. Para crear una conexión a GoogleDrive, en el diseñador de Logic Apps, seleccione **Mostrar API administradas por Microsoft** en la lista desplegable y, luego, escriba *GoogleDrive* en el cuadro de búsqueda. Seleccione el desencadenador o la acción que quiera usar:   
   ![paso de creación de conexión de GoogleDrive](./media/connectors-create-api-googledrive/googledrive-1.png)  
2. Si no ha creado ninguna conexión a GoogleDrive antes, se le pedirá que indique sus credenciales de GoogleDrive. Estas credenciales se usarán para autorizar a la aplicación lógica a conectarse y acceder a los datos de su cuenta de GoogleDrive:  
   ![paso de creación de conexión de GoogleDrive](./media/connectors-create-api-googledrive/googledrive-2.png)  
3. Proporcione su dirección de correo electrónico de GoogleDrive:  
   ![paso de creación de conexión de GoogleDrive](./media/connectors-create-api-googledrive/googledrive-3.png)  
4. Proporcione la contraseña de GoogleDrive para autorizar a la aplicación lógica:  
   ![paso de creación de conexión de GoogleDrive](./media/connectors-create-api-googledrive/googledrive-4.png)
5. Permita la conexión a GoogleDrive:  
   ![paso de creación de conexión de GoogleDrive](./media/connectors-create-api-googledrive/googledrive-5.png)  
6. Observe que la conexión se ha creado y que puede continuar sin problemas con el resto de pasos en la aplicación lógica:   
   ![paso de creación de conexión de GoogleDrive](./media/connectors-create-api-googledrive/googledrive-6.png)  

