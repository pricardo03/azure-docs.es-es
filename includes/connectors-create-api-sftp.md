---
author: ecfan
ms.service: logic-apps
ms.topic: include
ms.date: 11/03/2016
ms.author: estfan
ms.openlocfilehash: d249a205c64f4e067f2d81c7e1068c8ad9756958
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "58115223"
---
### <a name="prerequisites"></a>Requisitos previos
* Una cuenta de [SFTP](https://en.wikipedia.org/wiki/SSH_File_Transfer_Protocol)  

Para poder usar su cuenta de SFTP en una aplicación lógica, debe autorizar a la aplicación lógica a conectarse a dicha cuenta. Por suerte, esto es muy fácil de hacer desde la aplicación lógica en Azure Portal.  

Aquí se explica cómo autorizar a la aplicación lógica a conectarse a su cuenta de SFTP:  

1. Para crear una conexión a SFTP, en el diseñador de Logic Apps, seleccione **Mostrar las API administradas por Microsoft** en la lista desplegable y, luego, escriba *SFTP* en el cuadro de búsqueda. Seleccione el desencadenador **SFTP - When a file is added or modified** (SFTP: cuando se agrega o modifica un archivo).  
   ![Imagen 1 de conexión en línea de SFTP](./media/connectors-create-api-sftp/sftp-1.png)  
2. Si no ha creado ninguna conexión a SFTP antes, se le pedirá que indique sus credenciales de SFTP. Estas credenciales se usarán para autorizar a la aplicación lógica a conectarse y acceder a los datos de su cuenta de SFTP:  
   ![Imagen 2 de conexión en línea de SFTP](./media/connectors-create-api-sftp/sftp-2.png)  
3. Observe que la conexión se ha creado y que puede continuar sin problemas con el resto de pasos en la aplicación lógica:   
   ![Imagen 3 de conexión en línea de SFTP](./media/connectors-create-api-sftp/sftp-3.png) 

