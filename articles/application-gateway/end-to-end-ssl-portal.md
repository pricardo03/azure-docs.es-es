---
title: 'Inicio rápido: configurar el cifrado de SSL de extremo a otro con Azure Application Gateway mediante Azure portal | Microsoft Docs'
description: Obtenga información sobre cómo usar el portal de Azure para crear una instancia de Azure Application Gateway con el cifrado SSL de extremo a otro.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/30/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: bd165f81b45e3ae0c121fb8876ed88e68d493195
ms.sourcegitcommit: ed66a704d8e2990df8aa160921b9b69d65c1d887
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64946796"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-the-portal"></a>Configurar SSL de extremo a extremo con Application Gateway con el portal

Este artículo muestra cómo usar el portal de Azure para configurar el cifrado SSL de extremo a extremo con application gateway SKU v1.  

> [!NOTE]
> SKU de v2 Application Gateway requiere certificados raíz de confianza para habilitar configuración de extremo a otro. Soporte técnico del portal para agregar certificados raíz de confianza aún no está disponible. Por lo tanto, en el caso de v2 SKU vea [configurar SSL de extremo a otro mediante PowerShell](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

Para configurar SSL de un extremo a otro con una puerta de enlace de aplicaciones, hacen falta certificados para la puerta de enlace y los servidores back-end. El certificado de puerta de enlace se usa para derivar una clave simétrica según la especificación del protocolo SSL. A continuación, se usa la clave simétrica para cifrar y descifrar el tráfico enviado a la puerta de enlace. Para el cifrado SSL de un extremo a otro, el back-end debe estar en la lista de permitidos junto con la puerta de enlace de aplicaciones. Para ello, cargue el certificado público de los servidores back-end, también se conocen como certificados de autenticación, en la puerta de enlace de la aplicación. Al agregar el certificado, se garantiza que la puerta de enlace de aplicaciones solo se comunique con instancias back-end conocidas. Esto protege aún más la comunicación de un extremo a otro.

Para obtener más información, consulte [SSL SSL de extremo a extremo y terminación](https://docs.microsoft.com/azure/application-gateway/ssl-overview).

## <a name="create-a-new-application-gateway-with-end-to-end-ssl"></a>Crear una nueva instancia de application gateway con SSL de extremo a extremo

Para crear una nueva instancia de application gateway con el cifrado SSL de extremo a otro, deberá habilitar primero la terminación SSL durante la creación de una nueva instancia de application gateway. Esto le permitirá el cifrado SSL para la comunicación entre la puerta de enlace de cliente y la aplicación. A continuación, necesitará certificados de la lista blanca para servidores back-end en la configuración de HTTP para habilitar el cifrado SSL para la comunicación entre los servidores de puerta de enlace y el back-end de aplicación, para realizar el cifrado SSL de extremo a otro.

### <a name="enable-ssl-termination-while-creating-a-new-application-gateway"></a>Habilitar la terminación SSL durante la creación de una nueva instancia de application gateway

Consulte este artículo para comprender cómo [habilitar la terminación SSL durante la creación de una nueva instancia de application gateway](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

### <a name="whitelist-certificates-for-backend-servers"></a>Certificados de la lista blanca para servidores back-end

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione **configuración HTTP** en el menú izquierdo. Azure crea automáticamente una configuración de HTTP de forma predeterminada, **appGatewayBackendHttpSettings**, al crear la puerta de enlace de la aplicación. 

3. Seleccione **appGatewayBackendHttpSettings**.

4. En **protocolo**, seleccione **HTTPS**. Un panel para **certificados de autenticación de back-end** aparecerá. 

5. En **certificados de autenticación de back-end**, elija **crear nuevo**.

6. Escriba adecuado **nombre**.

7. Cargue el certificado usando el **certificado CER cargar** cuadro.![ addcert](./media/end-to-end-ssl-portal/addcert.png)

   > [!NOTE]
   > El certificado proporcionado en este paso debe ser la clave pública del certificado .pfx presente en el back-end. Exporte el certificado (no el certificado raíz) instalado en el servidor back-end en formato Afirmación, Evidencia y Razonamiento (CER) y utilícelo en este paso. En este paso se coloca el back-end en la lista de permitidos con la puerta de enlace de aplicaciones.

8. Seleccione **Guardar**.

## <a name="enable-end-to-end-ssl-for-existing-application-gateway"></a>Habilitar SSL de extremo a otro para la puerta de enlace de aplicaciones existente

Para configurar una puerta de enlace de aplicaciones existentes con el cifrado SSL de extremo a otro, deberá terminación de SSL habilite primero en el agente de escucha. Esto le permitirá el cifrado SSL para la comunicación entre la puerta de enlace de cliente y la aplicación. A continuación, necesitará certificados de la lista blanca para servidores back-end en la configuración de HTTP para habilitar el cifrado SSL para la comunicación entre los servidores de puerta de enlace y el back-end de aplicación, para realizar el cifrado SSL de extremo a otro.

Deberá usar un agente de escucha con el protocolo HTTPS y certificados para habilitar la terminación SSL. No se puede cambiar el protocolo de un agente de escucha existente. Por lo tanto, puede elegir usar un agente de escucha existente con el certificado y el protocolo HTTPS, o crear un nuevo agente de escucha. Si elige la primera de ellas, puede omitir el mencionados a continuación los pasos para **terminación habilitar SSL en application gateway existente** y pasar directamente a **certificados de la lista blanca para servidores back-end** sección. Si elige esto último, siga estos pasos.

### <a name="enable-ssl-termination-in-existing-application-gateway"></a>Habilitar la terminación SSL en application gateway existente

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione **los agentes de escucha** en el menú izquierdo.

3. Elija entre **básica** y **multisitio** escucha según sus necesidades.

4. En **protocolo**, seleccione **HTTPS**. Un panel para **certificado** aparecerá.

5. Cargue el certificado PFX que se va a utilizar para la terminación SSL entre la puerta de enlace de cliente y la aplicación.

   > [!NOTE]
   > Con fines de prueba, puede usar un certificado autofirmado. No se debe usar un certificado autofirmado para cargas de trabajo de producción. Obtenga información sobre cómo [crear un certificado autofirmado](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal#create-a-self-signed-certificate).

6. Agregar otros valores necesarios para la **escucha** según sus necesidades.

7. Seleccione **Aceptar** para guardar.

### <a name="whitelist-certificates-for-backend-servers"></a>Certificados de la lista blanca para servidores back-end

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione **configuración HTTP** en el menú izquierdo. Ambos certificados lista blanca en un HTTP de back-end existente puede establecer o crear una nueva configuración de HTTP. En el siguiente paso, se realizará la lista blanca certificado para la configuración de HTTP de forma predeterminada, **appGatewayBackendHttpSettings**.

3. Seleccione **appGatewayBackendHttpSettings**.

4. En **protocolo**, seleccione **HTTPS**. Un panel para **certificados de autenticación de back-end** aparecerá. 

5. En **certificados de autenticación de back-end**, elija **crear nuevo**.

6. Escriba adecuado **nombre**.

7. Cargue el certificado usando el **certificado CER cargar** cuadro.![ addcert](./media/end-to-end-ssl-portal/addcert.png)

   > [!NOTE]
   > El certificado proporcionado en este paso debe ser la clave pública del certificado .pfx presente en el back-end. Exporte el certificado (no el certificado raíz) instalado en el servidor back-end en formato Afirmación, Evidencia y Razonamiento (CER) y utilícelo en este paso. En este paso se coloca el back-end en la lista de permitidos con la puerta de enlace de aplicaciones.

8. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Administrar el tráfico web con Application Gateway mediante la CLI de Azure](./tutorial-manage-web-traffic-cli.md)
