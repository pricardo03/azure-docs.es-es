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
ms.openlocfilehash: a37b313bd808ee0441d84ac92050b087eba7ac9d
ms.sourcegitcommit: cd70273f0845cd39b435bd5978ca0df4ac4d7b2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71097165"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-the-portal"></a>Configuración de SSL de un extremo a otro con Application Gateway mediante el portal

En este artículo se muestra cómo usar Azure Portal para configurar el cifrado SSL de un extremo a otro con una SKU de Application Gateway v1.  

> [!NOTE]
> La SKU de Application Gateway v2 requiere certificados raíz de confianza para habilitar configuración de un extremo a otro.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar

Para configurar SSL de un extremo a otro con una instancia de Application Gateway, hacen falta certificados para la puerta de enlace y para los servidores back-end. El certificado de puerta de enlace se usa para derivar una clave simétrica según la especificación del protocolo SSL. A continuación, la clave simétrica se usa para cifrar y descifrar el tráfico que se envía a la puerta de enlace. Para el cifrado SSL de un extremo a otro, los servidores back-end correctos se deben permitir en Application Gateway. Para ello, cargue el certificado público de los servidores back-end, también conocidos como certificados de autenticación (v1) o certificados raíz de confianza (v2), en Application Gateway. Al agregar el certificado, se garantiza que Application Gateway solo se comunique con instancias back-end conocidas. Esto protege aún más la comunicación de un extremo a otro.

Para más información, consulte [Terminación SSL y SSL de un extremo a otro](https://docs.microsoft.com/azure/application-gateway/ssl-overview).

## <a name="create-a-new-application-gateway-with-end-to-end-ssl"></a>Crear una nueva instancia de Application Gateway con SSL de un extremo a otro

Para crear una nueva instancia de Application Gateway con el cifrado SSL de un extremo a otro, deberá habilitar primero la terminación SSL durante la creación de una nueva instancia de Application Gateway. Esto permitirá el cifrado SSL para la comunicación entre el cliente y la puerta de enlace de aplicaciones. Luego, tendrá que aprobar los certificados para los servidores back-end en la configuración HTTP para habilitar el cifrado SSL para la comunicación entre l puerta de enlace de aplicaciones y los servidores back-end, para lograr el cifrado SSL de un extremo a otro.

### <a name="enable-ssl-termination-while-creating-a-new-application-gateway"></a>Habilitar la terminación SSL durante la creación de una nueva instancia de Application Gateway

Consulte este artículo para comprender cómo [habilitar la terminación SSL durante la creación de una nueva instancia de Application Gateway](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal).

### <a name="add-authenticationroot-certificate-of-back-end-servers"></a>Incorporación de certificado de autenticación o raíz de los servidores back-end

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione **Configuración HTTP** en el menú izquierdo. Azure creó automáticamente una configuración HTTP predeterminada, **appGatewayBackendHttpSettings** cuando creó la puerta de enlace de aplicaciones. 

3. Seleccione **appGatewayBackendHttpSettings**.

4. En **Protocolo**, seleccione **HTTPS**. Aparece un panel para los **certificados de autenticación o certificados raíz de confianza back-end**. 

5. Elija **Crear nuevo**.

6. Escriba un nombre adecuado en **Nombre**.

7. Seleccione el archivo de certificado mediante el cuadro **Cargar certificado CER**.

   En el caso de instancias de Application Gateway estándar y WAF (V1), debe cargar la clave pública del certificado de servidor back-end en formato .cer.

   ![addcert](./media/end-to-end-ssl-portal/addcert.png)

   En el caso de instancias de Application Gateway Standard_v2 y WAF_v2, debe cargar el **certificado raíz** del certificado de servidor back-end en formato .cer. Si el certificado de back-end lo emite una entidad de certificación conocida, puede activar la casilla "Usar el certificado de una entidad de certificación reconocida" y no es necesario cargar un certificado.

   ![addtrustedrootcert](./media/end-to-end-ssl-portal/trustedrootcert-portal.png)

   ![rootcert](./media/end-to-end-ssl-portal/trustedrootcert.png)

8. Seleccione **Guardar**.

## <a name="enable-end-to-end-ssl-for-existing-application-gateway"></a>Habilitar SSL de un extremo a otro para la puerta de enlace de aplicaciones existente

Para configurar una puerta de enlace de aplicaciones existente con cifrado SSL de un extremo a otro, deberá habilitar primero la terminación SSL en el agente de escucha. Esto permitirá el cifrado SSL para la comunicación entre el cliente y la puerta de enlace de aplicaciones. Luego, tendrá que aprobar los certificados para los servidores back-end en la configuración HTTP para habilitar el cifrado SSL para la comunicación entre l puerta de enlace de aplicaciones y los servidores back-end, para lograr el cifrado SSL de un extremo a otro.

Deberá usar un agente de escucha con el certificado y protocolo HTTPS para habilitar la terminación SSL. Por lo tanto, puede elegir usar un agente de escucha existente con el certificado y protocolo HTTPS, o crear un nuevo agente de escucha. Si elige la primera opción, puede omitir los pasos que se mencionan a continuación para **Habilitar la terminación SSL en una puerta de enlace de aplicaciones existente** y pasar directamente a la sección **Incorporación de certificados de autenticación o raíz de confianza de servidores back-end**. Si elige esto último, siga estos pasos.

### <a name="enable-ssl-termination-in-existing-application-gateway"></a>Habilitar la terminación SSL en una puerta de enlace de aplicaciones existente

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione **Agentes de escucha** en el menú izquierdo.

3. Elija entre agente de escucha **Básico** y **Multisitio** según sus necesidades.

4. En **Protocolo**, seleccione **HTTPS**. Aparecerá un panel para **Certificado**.

5. Cargue el certificado PFX que se va a usar para la terminación SSL entre el cliente y la puerta de enlace de aplicaciones.

   > [!NOTE]
   > Para fines de prueba, puede usar un certificado autofirmado. Pero no se recomienda para las cargas de trabajo de producción, ya que es más difícil de administrar y no es completamente seguro. Obtenga información sobre cómo [crear un certificado autofirmado](https://docs.microsoft.com/azure/application-gateway/create-ssl-portal#create-a-self-signed-certificate).

6. Agregue otros opciones necesarias para el **Agente de escucha** según sus requisitos.

7. Seleccione **Aceptar** para guardar.

### <a name="add-authenticationtrusted-root-certificates-of-back-end-servers"></a>Incorporación de certificados de autenticación o raíz de confianza de servidores back-end

1. Seleccione **Todos los recursos** y, después, seleccione **myAppGateway**.

2. Seleccione **Configuración HTTP** en el menú izquierdo. Puede aprobar los certificados en una configuración HTTP de back-end existente o crear una nueva configuración HTTP. En el siguiente paso, se aprobará el certificado para la configuración HTTP predeterminada, **appGatewayBackendHttpSettings**.

3. Seleccione **appGatewayBackendHttpSettings**.

4. En **Protocolo**, seleccione **HTTPS**. Aparece un panel para los **certificados de autenticación o certificados raíz de confianza back-end**. 

5. Elija **Crear nuevo**.

6. Escriba el **Nombre** adecuado.

7. Seleccione el archivo de certificado mediante el cuadro **Cargar certificado CER**.

   En el caso de instancias de Application Gateway estándar y WAF (V1), debe cargar la clave pública del certificado de servidor back-end en formato .cer.

   ![addcert](./media/end-to-end-ssl-portal/addcert.png)

   En el caso de instancias de Application Gateway Standard_v2 y WAF_v2, debe cargar el **certificado raíz** del certificado de servidor back-end en formato .cer. Si el certificado de back-end lo emite una entidad de certificación conocida, puede activar la casilla "Usar el certificado de una entidad de certificación reconocida" y no es necesario cargar un certificado.

   ![addtrustedrootcert](./media/end-to-end-ssl-portal/trustedrootcert-portal.png)

8. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Administrar el tráfico web con Application Gateway mediante la CLI de Azure](./tutorial-manage-web-traffic-cli.md)
