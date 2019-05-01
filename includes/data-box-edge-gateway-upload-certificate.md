---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: d7a9923d5bd9e357bcd75fae6e0a7d1bcd437a53
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64732653"
---
Un certificado SSL correcto garantiza que está enviando información cifrada al servidor correcto. Además de cifrado, el certificado también se permite para la autenticación. Puede cargar su propio certificado SSL de confianza mediante la interfaz de PowerShell del dispositivo.

1. [Conectarse a la interfaz de PowerShell](#connect-to-the-powershell-interface).
2. Use el `Set-HcsCertificate` cmdlet para cargar el certificado. Cuando se le solicite, proporcione los parámetros siguientes:

   - `CertificateFilePath` -Ruta de acceso al recurso compartido que contiene el archivo de certificado en *.pfx* formato.
   - `CertificatePassword` -Una contraseña para proteger el certificado.
   - `Credentials` -El nombre de usuario y contraseña para acceder al recurso compartido que contiene el certificado.

     El ejemplo siguiente muestra el uso de este cmdlet:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username/Password"
     ```

