---
title: 'Guía de inicio rápido: Configuración del cifrado SSL de un extremo a otro con Azure Application Gateway: Azure Portal | Microsoft Docs'
description: Obtenga información sobre cómo usar Azure Portal para crear una instancia de Azure Application Gateway con el cifrado SSL de un extremo a otro.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/30/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: bd165f81b45e3ae0c121fb8876ed88e68d493195
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64946796"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-the-portal"></a>Configuración de SSL de un extremo a otro con Application Gateway mediante el portal

En este artículo se muestra cómo usar Azure Portal para configurar el cifrado SSL de un extremo a otro con una SKU de Application Gateway v1.  

> [!NOTE]
> La SKU de Application Gateway v2 requiere certificados raíz de confianza para habilitar configuración de un extremo a otro. El soporte técnico del portal para agregar certificados raíz de confianza aún no está disponible. Por lo tanto, en el caso del SKU v2, consulte la [configuración de SSL de un extremo a otro mediante PowerShell](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

Para configurar SSL de un extremo a otro con una puerta de enlace de aplicaciones, hacen falta certificados para la puerta de enlace y los servidores back-end. El certificado de puerta de enlace se usa para derivar una clave simétrica según la especificación del protocolo SSL. A continuación, la clave simétrica se usa para cifrar y descifrar el tráfico que se envía a la puerta de enlace. Para el cifrado SSL de un extremo a otro, el back-end debe estar en la lista de permitidos junto con la puerta de enlace de aplicaciones. Para ello, cargue el certificado público de los servidores back-end, también conocidos como certificados de autenticación, en la puerta de enlace de las aplicaciones. Al agregar el certificado, se garantiza que la puerta de enlace de aplicaciones solo se comunique con instancias back-end conocidas. Esto protege aún más la comunicación de un extremo a otro.

Para más información, consulte [Terminación SSL y SSL de un extremo a otro](https://docs.microsoft.com/azure/application-gateway/ssl-overview).

## <a name="create-a-new-application-gateway-with-end-to-end-ssl"></a>Crear una nueva instancia de Application Gateway con SSL de un extremo a otro

Para crear una nueva instancia de Application Gateway con el cifrado SSL de un extremo a otro, deberá habilitar primero la terminación SSL durante la creación de una nueva instancia de Application Gateway. Esto permitirá el cifrado SSL para la comunicación entre el cliente y la puerta de enlace de aplicaciones. Luego, tendrá que aprobar los certificados para los servidores back-end en la configuración HTTP para habilitar el cifrado SSL para la comunicación entre l puerta de enlace de aplicaciones y los servidores back-end, para lograr el cifrado SSL de un extremo a otro.

### <a name="enable-ssl-termination-while-creating-a-new-application-gateway"></a>Habilitar la terminación SSL durante la creación de una nueva instancia de Application Gateway

Consulte este artículo para comprender cómo [habilitar la terminación SSL durante la creación de una nueva instancia de Application Gateway](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

### <a name="whitelist-certificates-for-backend-servers"></a>Aprobar certificados para servidores back-end

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione **Configuración HTTP** en el menú izquierdo. Azure creó automáticamente una configuración HTTP predeterminada, **appGatewayBackendHttpSettings** cuando creó la puerta de enlace de aplicaciones. 

3. Seleccione **appGatewayBackendHttpSettings**.

4. En **Protocolo**, seleccione **HTTPS**. Aparece un panel para **Certificados de autenticación back-end**. 

5. En **Certificados de autenticación back-end**, elija **Crear nuevo**.

6. Escriba el **Nombre** adecuado.

7. Cargar el certificado con el cuadro **Cargar certificado CER** cuadro.![addcert](./media/end-to-end-ssl-portal/addcert.png)

   > [!NOTE]
   > El certificado proporcionado en este paso debe ser la clave pública del certificado .pfx presente en el back-end. Exporte el certificado (no el certificado raíz) instalado en el servidor back-end en formato Afirmación, Evidencia y Razonamiento (CER) y utilícelo en este paso. En este paso se coloca el back-end en la lista de permitidos con la puerta de enlace de aplicaciones.

8. Seleccione **Guardar**.

## <a name="enable-end-to-end-ssl-for-existing-application-gateway"></a>Habilitar SSL de un extremo a otro para la puerta de enlace de aplicaciones existente

Para configurar una puerta de enlace de aplicaciones existente con cifrado SSL de un extremo a otro, deberá habilitar primero la terminación SSL en el agente de escucha. Esto permitirá el cifrado SSL para la comunicación entre el cliente y la puerta de enlace de aplicaciones. Luego, tendrá que aprobar los certificados para los servidores back-end en la configuración HTTP para habilitar el cifrado SSL para la comunicación entre l puerta de enlace de aplicaciones y los servidores back-end, para lograr el cifrado SSL de un extremo a otro.

Deberá usar un agente de escucha con el certificado y protocolo HTTPS para habilitar la terminación SSL. No se puede cambiar el protocolo de un agente de escucha existente. Por lo tanto, puede elegir usar un agente de escucha existente con el certificado y protocolo HTTPS, o crear un nuevo agente de escucha. Si elige la primera opción, puede omitir los pasos que se mencionan a continuación para **Habilitar la terminación SSL en una puerta de enlace de aplicaciones existente** y pasar directamente a la sección **Aprobar certificados para servidores back-end**. Si elige esto último, siga estos pasos.

### <a name="enable-ssl-termination-in-existing-application-gateway"></a>Habilitar la terminación SSL en una puerta de enlace de aplicaciones existente

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione **Agentes de escucha** en el menú izquierdo.

3. Elija entre agente de escucha **Básico** y **Multisitio** según sus necesidades.

4. En **Protocolo**, seleccione **HTTPS**. Aparecerá un panel para **Certificado**.

5. Cargue el certificado PFX que se va a usar para la terminación SSL entre el cliente y la puerta de enlace de aplicaciones.

   > [!NOTE]
   > Para fines de prueba, puede usar un certificado autofirmado. No debe usar certificados autofirmados para cargas de trabajo de producción. Obtenga información sobre cómo [crear un certificado autofirmado](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal#create-a-self-signed-certificate).

6. Agregue otros opciones necesarias para el **Agente de escucha** según sus requisitos.

7. Seleccione **Aceptar** para guardar.

### <a name="whitelist-certificates-for-backend-servers"></a>Aprobar certificados para servidores back-end

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione **Configuración HTTP** en el menú izquierdo. Puede aprobar los certificados en una configuración HTTP de back-end existente o crear una nueva configuración HTTP. En el siguiente paso, se aprobará el certificado para la configuración HTTP predeterminada, **appGatewayBackendHttpSettings**.

3. Seleccione **appGatewayBackendHttpSettings**.

4. En **Protocolo**, seleccione **HTTPS**. Aparece un panel para **Certificados de autenticación back-end**. 

5. En **Certificados de autenticación back-end**, elija **Crear nuevo**.

6. Escriba el **Nombre** adecuado.

7. Cargar el certificado con el cuadro **Cargar certificado CER** cuadro.![addcert](./media/end-to-end-ssl-portal/addcert.png)

   > [!NOTE]
   > El certificado proporcionado en este paso debe ser la clave pública del certificado .pfx presente en el back-end. Exporte el certificado (no el certificado raíz) instalado en el servidor back-end en formato Afirmación, Evidencia y Razonamiento (CER) y utilícelo en este paso. En este paso se coloca el back-end en la lista de permitidos con la puerta de enlace de aplicaciones.

8. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Administrar el tráfico web con Application Gateway mediante la CLI de Azure](./tutorial-manage-web-traffic-cli.md)
