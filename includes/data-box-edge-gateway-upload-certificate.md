---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 04/23/2019
ms.author: alkohli
ms.openlocfilehash: d7a9923d5bd9e357bcd75fae6e0a7d1bcd437a53
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186772"
---
Un certificado SSL correcto garantiza que envía la información cifrada al servidor correcto. Además del cifrado, el certificado también permite la autenticación. Puede cargar su propio certificado SSL de confianza mediante la interfaz de PowerShell del dispositivo.

1. [Conéctese a la interfaz de PowerShell](#connect-to-the-powershell-interface).
2. Use el cmdlet `Set-HcsCertificate` para cargar el certificado. Cuando se le solicite, proporcione los siguientes parámetros:

   - `CertificateFilePath`: ruta de acceso al recurso compartido que contiene el archivo de certificado en formato *.pfx*.
   - `CertificatePassword`: contraseña usada para proteger el certificado.
   - `Credentials`: nombre de usuario y contraseña para acceder al recurso compartido que contiene el certificado.

     En el ejemplo siguiente se muestra el uso de este cmdlet:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credential "Username/Password"
     ```

