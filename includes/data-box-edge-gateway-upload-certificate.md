---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 03/05/2019
ms.author: alkohli
ms.openlocfilehash: 216380cf7069468d13c4e533fc90b2596aa211c4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58115649"
---
Un certificado SSL correcto garantiza que está enviando información cifrada al servidor correcto. Además de cifrado, el certificado también se permite para la autenticación. Puede cargar su propio certificado SSL de confianza mediante la interfaz de PowerShell del dispositivo.

1. [Conectarse a la interfaz de PowerShell](#connect-to-the-powershell-interface).
2. Use el `Set-HcsCertificate` cmdlet para cargar el certificado. Cuando se le solicite, proporcione los parámetros siguientes:

   - `CertificateFilePath` -Ruta de acceso al recurso compartido que contiene el archivo de certificado en *.pfx* formato.
   - `CertificatePassword` -Una contraseña para proteger el certificado.
   - `Credentials` -El nombre de usuario y contraseña para acceder al recurso compartido que contiene el certificado.

     El ejemplo siguiente muestra el uso de este cmdlet:

     ```
     Set-HcsCertificate -Scope LocalWebUI -CertificateFilePath "\\myfileshare\certificates\mycert.pfx" -CertificatePassword "mypassword" -Credentials "Username/Password"
     ```

