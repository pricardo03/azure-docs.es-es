---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: d90ef1737f0107e5e323c1e90e3f9021efd4e4f7
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "58114792"
---
### <a name="prerequisites"></a>Requisitos previos
* Una cuenta de [SMTP](https://wikipedia.org/wiki/Simple_Mail_Transfer_Protocol)  

Antes de poder usar la cuenta de SMTP en una aplicación lógica, debe autorizar a la aplicación lógica a conectarse a dicha cuenta. Por suerte, esto se puede hacer fácilmente dentro de la aplicación lógica en Azure Portal.  

Aquí se explica cómo autorizar a la aplicación lógica a conectarse a su cuenta de SMTP:  

1. Para crear una conexión a SMTP, en el diseñador de Logic Apps, seleccione **Mostrar las API administradas por Microsoft** en la lista desplegable y, luego, escriba *SMTP* en el cuadro de búsqueda. Seleccione el desencadenador o la acción que quiera usar:   
   ![](./media/connectors-create-api-smtp/smtp-1.png)  
2. Si no ha creado ninguna conexión a SMTP antes, se le pedirá que indique sus credenciales de SMTP. Estas credenciales se usarán para autorizar a la aplicación lógica a conectarse y acceder a los datos de su cuenta de SMTP:  
   ![](./media/connectors-create-api-smtp/smtp-2.png)  
3. Observe que la conexión se ha creado y que puede continuar sin problemas con el resto de pasos en la aplicación lógica:  
   ![](./media/connectors-create-api-smtp/smtp-3.png)  

