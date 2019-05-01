---
ms.openlocfilehash: 3f4430631a664f81f53f9df1f46ebc27c635de36
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60860209"
---
### <a name="prerequisites"></a>Requisitos previos
* Una cuenta de [Dropbox](https://www.Dropbox.com/). 

Para poder usar su cuenta de Dropbox en una aplicación lógica, debe autorizar a la aplicación lógica para que pueda conectarse a dicha cuenta. Por suerte, esto es muy fácil de hacer desde la aplicación lógica en Azure Portal. 

Aquí se explica cómo autorizar a la aplicación lógica para conectarse a su cuenta de Dropbox:

1. Para crear una conexión a Dropbox, en el diseñador de Logic Apps, seleccione **Mostrar API administradas por Microsoft** en la lista desplegable y, luego, escriba *Dropbox* en el cuadro de búsqueda. Seleccione el desencadenador o la acción que quiera usar:   
   ![Dropbox, paso 1](./media/connectors-create-api-dropbox/dropbox-1.png)
2. Si no ha creado ninguna conexión a Dropbox antes, se le pedirá que indique sus credenciales de Dropbox. Estas credenciales se usarán para autorizar a la aplicación lógica a conectarse y tener acceso a los datos de su cuenta de Dropbox:  
   ![Dropbox, paso 2](./media/connectors-create-api-dropbox/dropbox-2.png)
3. Indique su nombre de usuario y contraseña de Dropbox para autorizar a la aplicación lógica:  
   ![Dropbox, paso 3](./media/connectors-create-api-dropbox/dropbox-3.png)   
4. Autorice a la aplicación lógica a usar la cuenta de Dropbox:  
   ![Dropbox, paso 4](./media/connectors-create-api-dropbox/dropbox-4.png)
5. Observe que la conexión se ha creado y que puede continuar sin problemas con el resto de pasos en la aplicación lógica:   
   ![Dropbox, paso 5](./media/connectors-create-api-dropbox/dropbox-5.png)   

