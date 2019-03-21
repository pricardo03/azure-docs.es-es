---
title: Certificados necesarios para la creación de listas blancas back-ends en Azure Application Gateway
description: En este artículo se proporciona ejemplos de cómo se puede convertir un certificado SSL para el certificado de autenticación y certificado raíz de confianza que son necesarios para la creación de listas blancas instancias de back-end de Azure Application Gateway
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 3/14/2019
ms.author: absha
ms.openlocfilehash: 72ee9123ad959c0c7240d4f7a906adc1a4dd1a93
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260604"
---
# <a name="create-certificates-for-whitelisting-backend-with-azure-application-gateway"></a>Creación de certificados para la creación de listas blancas back-end con Azure Application Gateway

Para llevar a cabo SSL de extremo a extremo, application gateway requiere que aparece en la lista mediante la carga de certificados raíz de confianza de autenticación o las instancias de back-end. En el caso de SKU v1, se requieren certificados de autenticación, mientras que en el caso de SKU v2, de confianza de certificados raíz son necesarios para la creación de listas blancas los certificados

En este artículo, aprenderá a:

> [!div class="checklist"]
>
> - Exportar el certificado de autenticación de un certificado de back-end (para SKU v1)
> - Exportar el certificado raíz de confianza de un certificado de back-end (para SKU v2)

## <a name="prerequisites"></a>Requisitos previos

Requiere un certificado existente de back-end para generar los certificados de autenticación o los certificados raíz de confianza necesarios para las instancias de back-end de inclusión en lista blanca con application gateway. El certificado de back-end puede ser igual que el certificado SSL o diferentes para una mayor seguridad. Puerta de enlace de aplicación no proporciona ningún mecanismo para crear o adquirir un certificado SSL. Con fines de prueba, puede crear un certificado autofirmado, pero no debe usarlo para cargas de trabajo de producción. 

## <a name="export-authentication-certificate-for-v1-sku"></a>Exportar el certificado de autenticación (para SKU v1)

Certificado de autenticación es necesaria para las instancias de back-end de lista blanca en la versión 1 de puerta de enlace de aplicación SKU. Certificado de autenticación es la clave pública de los certificados de servidor de back-end en Base64 codificado X.509 (. Formato CER). En este ejemplo, se usará un certificado SSL para el certificado de back-end y exportar su clave pública que se usará como certificados de autenticación. Además, en este ejemplo, se usará la herramienta Administrador de certificados de Windows para exportar los certificados necesarios. Puede usar cualquier otra herramienta según su comodidad.

En el certificado SSL, exportar el archivo .cer de clave pública (no la clave privada). Los siguientes pasos ayuda exportar .cer X.509 codificado de archivo en Base 64 (. Formato CER) para el certificado:

1. Para obtener un archivo .cer del certificado, abra **Administrar certificados de usuario**. Busque el certificado, normalmente en "Certificates - Current User\Personal\Certificates" y haga clic en. Haga clic en **Todas las tareas** y, luego, en **Exportar**. Se abre el **Asistente para exportar certificados**. Si no encuentra el certificado en Usuario actual\Personal\Certificados, puede que haya abierto de forma accidental "Certificados – equipo Local", en lugar de "Certificados - Usuario actual"). Si desea abrir el Administrador de certificados en el ámbito del usuario actual mediante PowerShell, escriba *certmgr* en la ventana de la consola.

   ![Exportación](./media/certificates-for-backend-authentication/export.png)

2. En el asistente, haga clic en **Siguiente**.

   ![Exportación de certificado](./media/certificates-for-backend-authentication/exportwizard.png)

3. Seleccione **No exportar la clave privada** y, después, haga clic en **Siguiente**.

   ![No exportar la clave privada](./media/certificates-for-backend-authentication/notprivatekey.png)

4. En la página **Formato de archivo de exportación**, seleccione **X.509 codificado base 64 (.CER)** y, luego, haga clic en **Siguiente**.

   ![Codificado con Base 64](./media/certificates-for-backend-authentication/base64.png)

5. En **Archivo que se va a exportar**, haga clic en **Examinar** para ir a la ubicación a la que desea exportar el certificado. En **Nombre de archivo**, asígnele un nombre al archivo de certificado. A continuación, haga clic en **Siguiente**.

   ![Examinar](./media/certificates-for-backend-authentication/browse.png)

6. Haga clic en **Finalizar** para exportar el certificado.

   ![Finalizar](./media/certificates-for-backend-authentication/finish.png)

7. El certificado se exportó correctamente.

   ![Correcto](./media/certificates-for-backend-authentication/success.png)

   El certificado exportado tiene un aspecto similar al siguiente:

   ![Exportado](./media/certificates-for-backend-authentication/exported.png)

8. Si abre el certificado exportado mediante el Bloc de notas, verá algo parecido a este ejemplo. La sección en azul contiene la información que se haya cargado a application gateway. Si abre el certificado con el Bloc de notas y no se parece a este, normalmente eso significa que no lo exportó mediante el formato X.509 codificado base 64 (. CER). Además, si desea utilizar un editor de texto diferente, debe comprender que algunos editores pueden introducir formatos no deseados en segundo plano. Esto puede crear problemas al cargar el texto de este certificado en Azure.

   ![Abrir con el Bloc de notas](./media/certificates-for-backend-authentication/format.png)

## <a name="export-trusted-root-certificate-for-v2-sku"></a>Exportar el certificado raíz de confianza (para SKU v2)

Certificado raíz es necesario para las instancias de back-end de lista blanca en la versión 2 de puerta de enlace de aplicaciones de confianza SKU. El certificado raíz está codificado en Base-64 X.509 (. Certificado de raíz CER) formato de los certificados de servidor back-end. En este ejemplo, se usa un certificado SSL para el certificado de back-end, exportará su clave pública y, a continuación, exportar el certificado raíz de la entidad de certificación de confianza de la clave pública en formato codificado en base64 para obtener el certificado raíz de confianza. 

Los pasos siguientes ayudarán a exportar el archivo .cer del certificado:

1. Use los pasos 1-9 mencionado en la sección **exportar el certificado de autenticación de un certificado de back-end (para SKU v1)** anterior para exportar la clave pública de su certificado de back-end.

2. Una vez que se ha exportado la clave pública, abra el archivo.

   ![Certificado de autorización abierta](./media/certificates-for-backend-authentication/openAuthcert.png)

   ![acerca de certificado](./media/certificates-for-backend-authentication/general.png)

3. Mover a la ruta de certificación para ver la entidad de certificación.

   ![información del certificado.](./media/certificates-for-backend-authentication/certdetails.png)

4. Seleccione el certificado raíz y haga clic en **Ver certificado**.

   ![ruta de acceso del certificado](./media/certificates-for-backend-authentication/rootcert.png)

   Debería poder ver los detalles del certificado raíz.

   ![información de certificado](./media/certificates-for-backend-authentication/rootcertdetails.png)

5. Mover a la **detalles** ver y haga clic en **copiar a archivo...**

   ![certificado raíz de copia](./media/certificates-for-backend-authentication/rootcertcopytofile.png)

6. En este momento, ha extraído los detalles del certificado raíz del certificado de back-end. Verá el **Asistente para exportar certificados**. Ahora use los pasos 2 a 9 mencionado en la sección **exportar el certificado de autenticación de un certificado de back-end (para SKU v1)** anteriormente para exportar la raíz de confianza certificado en Base 64 codificado X.509 (. Formato CER).

## <a name="next-steps"></a>Pasos siguientes

Ahora tiene la autenticación de certificado de confianza certificado raíz en Base64 codificado X.509 (. Formato CER). Puede agregar esto a la puerta de enlace de aplicaciones en la lista blanca los servidores back-end para el cifrado SSL de extremo a extremo. Consulte [cómo configurar el cifrado SSL de extremo a extremo](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).