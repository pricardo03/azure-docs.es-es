---
title: 'Configuración de un cliente VPN para conexiones P2S de protocolo OpenVPN: Autenticación de Azure AD| Microsoft Docs'
description: Puede usar la P2S VPN para conectarse a la red virtual con la autenticación de Azure AD
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/07/2019
ms.author: alzam
ms.openlocfilehash: 714cabc204c9ec9a0c3bbc93be287957c91d1936
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73932330"
---
# <a name="configure-a-vpn-client-for-p2s-openvpn-protocol-connections-azure-ad-authentication-preview"></a>Configuración de un cliente VPN para conexiones P2S de protocolo OpenVPN: Autenticación Azure AD (versión preliminar)

Este artículo le ayuda a configurar un cliente VPN para conectarse a una red virtual mediante una VPN de punto a sitio y la autenticación Azure Active Directory. Antes de poder conectarse y autenticarse con Azure AD, primero debe configurar el inquilino de Azure AD. Para más información, consulte [Configurar un inquilino de Azure AD](openvpn-azure-ad-tenant.md).

> [!NOTE]
> La autenticación de Azure AD solo se admite para las conexiones de protocolo OpenVPN®.
>

## <a name="profile"></a>Trabajar con perfiles de cliente

Para conectarse, es preciso descargar el cliente VPN de Azure (versión preliminar) y configurar un perfil de cliente VPN en todos los equipos que deseen conectarse a la red virtual. Puede crear un perfil de cliente en un equipo, exportarlo y, después, importarlo en otros equipos.

### <a name="to-download-the-azure-vpn-client"></a>Para descargar el cliente VPN de Azure

Use este [vínculo](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab) para descargar el cliente VPN de Azure (versión preliminar).

### <a name="cert"></a>Para crear un perfil de cliente basado en certificados

Cuando trabaje con un perfil basado en certificados, asegúrese de que los certificados adecuados están instalados en el equipo cliente. Para más información acerca de los certificados, consulte [Instalar certificados de cliente](point-to-site-how-to-vpn-client-install-azure-cert.md).

  ![cert](./media/openvpn-azure-ad-client/create/create-cert1.jpg)

### <a name="radius"></a>Para crear un perfil de cliente RADIUS

  ![radio](./media/openvpn-azure-ad-client/create/create-radius1.jpg)

### <a name="export"></a>Para exportar y distribuir un perfil de cliente

Una vez que tenga un perfil de trabajo y necesite distribuirlo a otros usuarios, puede exportarlo mediante los siguientes pasos:

1. Resalte el perfil de cliente de VPN que quiere exportar, seleccione el **...** y,luego, seleccione **Exportar**.

    ![exportar](./media/openvpn-azure-ad-client/export/export1.jpg)

2. Seleccione la ubicación en la que desea guardar este perfil, deje el nombre de archivo tal cual y, a continuación, seleccione **Guardar** para guardar el archivo xml.

    ![exportar](./media/openvpn-azure-ad-client/export/export2.jpg)

### <a name="import"></a>Para importar un perfil de cliente

1. En la página, seleccione **Importar**.

    ![importación](./media/openvpn-azure-ad-client/import/import1.jpg)

2. Busque el archivo xml de perfil y selecciónelo. Con el archivo seleccionado, seleccione **Abrir**.

    ![importación](./media/openvpn-azure-ad-client/import/import2.jpg)

3. Especifique el nombre del perfil y seleccione **Guardar**.

    ![importación](./media/openvpn-azure-ad-client/import/import3.jpg)

4. Seleccione **Conectar** para conectarse a la VPN.

    ![importación](./media/openvpn-azure-ad-client/import/import4.jpg)

5. Una vez conectado, el icono se volverá verde y dirá**Conectado**.

    ![importación](./media/openvpn-azure-ad-client/import/import5.jpg)

### <a name="delete"></a>Para eliminar un perfil de cliente

1. Seleccione los puntos suspensivos junto al perfil de cliente que desea eliminar. Después, seleccione **Quitar**.

    ![delete](./media/openvpn-azure-ad-client/delete/delete1.jpg)

2. Seleccione **Quitar** para eliminar.

    ![delete](./media/openvpn-azure-ad-client/delete/delete2.jpg)

## <a name="connection"></a>Crear una conexión

1. En la página, seleccione **+** , después **+ Agregar**.

    ![connection](./media/openvpn-azure-ad-client/create/create1.jpg)

2. Rellene la información de conexión. Si no está seguro de los valores, póngase en contacto con el administrador. Después de rellenar los valores, seleccione **Guardar**.

    ![connection](./media/openvpn-azure-ad-client/create/create2.jpg)

3. Seleccione **Conectar** para conectarse a la VPN.

    ![connection](./media/openvpn-azure-ad-client/create/create3.jpg)

4. Seleccione las credenciales adecuadas y, después, seleccione **Continuar**.

    ![connection](./media/openvpn-azure-ad-client/create/create4.jpg)

5. Una vez que se haya conectado correctamente, el icono se volverá verde y dirá **Conectado**.

    ![connection](./media/openvpn-azure-ad-client/create/create5.jpg)

### <a name="autoconnect"></a>Para conectarse automáticamente

Estos pasos le ayudarán a configurar la conexión para que se conecte automáticamente con Always-on.

1. En la página principal del cliente VPN, seleccione **Configuración de VPN**.

    ![auto](./media/openvpn-azure-ad-client/auto/auto1.jpg)

2. Seleccione **Sí** en el cuadro de diálogo cambiar aplicaciones.

    ![auto](./media/openvpn-azure-ad-client/auto/auto2.jpg)

3. Asegúrese de que la conexión que quiere establecer no está conectada y, luego, resalte el perfil y active la casilla **Conectar automáticamente**.

    ![auto](./media/openvpn-azure-ad-client/auto/auto3.jpg)

4. Seleccione **Conectar** para iniciar la conexión VPN.

    ![auto](./media/openvpn-azure-ad-client/auto/auto4.jpg)

## <a name="diagnose"></a>Diagnóstico de problemas de conexión

1. Para diagnosticar problemas de conexión, puede usar la herramienta **Diagnosticar**. Seleccione el **...** junto a la conexión VPN que quiere diagnosticar para que se muestre el menú. Después, seleccione **Diagnosticar**.

    ![diagnóstico](./media/openvpn-azure-ad-client/diagnose/diagnose1.jpg)

2. En la página **Propiedades de conexión**, seleccione **Ejecutar diagnóstico**.

    ![diagnóstico](./media/openvpn-azure-ad-client/diagnose/diagnose2.jpg)

3. Inicie sesión con sus credenciales.

    ![diagnóstico](./media/openvpn-azure-ad-client/diagnose/diagnose3.jpg)

4. Ver los resultados del diagnóstico.

    ![diagnóstico](./media/openvpn-azure-ad-client/diagnose/diagnose4.jpg)

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte [crear un inquilino de Azure Active Directory para conexiones VPN abiertas de P2S que usan Autenticación de Azure AD](openvpn-azure-ad-tenant.md).
