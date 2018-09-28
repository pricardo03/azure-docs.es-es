---
title: 'Tutorial: crear un registro de alias de Azure DNS para admitir nombres de dominio de vértice con Traffic Manager'
description: Este tutorial muestra cómo configurar un registro de alias de Azure DNS para admitir el uso de su nombre de dominio apex con Traffic Manager.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 4a5d8968861f6f2938e605d2f7106529ca401df4
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46967445"
---
# <a name="tutorial-configure-an-alias-record-to-support-apex-domain-names-with-traffic-manager"></a>Tutorial: configurar un registro de alias de para admitir nombres de dominio de vértice con Traffic Manager 

Puede crear un registro de alias para que el vértice de nombre de dominio (por ejemplo, contoso.com) haga referencia a un perfil de Traffic Manager. En lugar de un servicio de redirección para esto, puede configurar Azure DNS para hacer referencia a un perfil de Traffic Manager directamente desde la zona. 


En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una infraestructura de red y máquina virtual de host
> * Crear un perfil de Traffic Manager
> * Crear un registro de alias
> * Probar el registro de alias


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos
Debe tener un nombre de dominio disponible que pueda hospedar en Azure DNS para realizar las pruebas. Debe tener control total de este dominio, incluyendo la capacidad de establecer los registros de nombres de servidor (NS) para el dominio.

Para que obtener instrucciones para hospedar el dominio en DNS de Azure, consulte [Tutorial: Hospedaje del dominio en Azure DNS](dns-delegate-domain-azure-dns.md).

El dominio en el ejemplo que se usa en este tutorial es contoso.com, pero debe usar su propio nombre de dominio.

## <a name="create-the-network-infrastructure"></a>Creación de la infraestructura de red
En primer lugar, cree una red virtual y una subred en la que colocar los servidores web.
1. Inicie sesión en Azure Portal en http://portal.azure.com.
2. En la parte superior izquierda en el portal, haga clic en **Crear un recurso**, escriba *grupo de recursos* en el cuadro de búsqueda y cree un grupo de recursos denominado **RG-DNS-Alias-TM**.
3. Haga clic en **Crear un recurso**, haga clic en **Redes** y después en **Red virtual**.
4. Cree una red virtual denominada **VNet-Servers**, colóquela en el grupo de recursos **RG-DNS-Alias-TM** y asigne a la subred el nombre **SN-Web**.

## <a name="create-two-web-server-virtual-machines"></a>Crear dos máquinas virtuales de servidor web
1. Haga clic en **Crear un recurso** y después en **Máquina virtual de Windows Server 2016**.
2. Escriba **Web-01** para el nombre y coloque la máquina virtual en el grupo de recursos **RG-DNS-Alias-TM**. Escriba un nombre de usuario y una contraseña y haga clic en **Aceptar**.
3. Para **Tamaño**, elija una SKU con 8 GB de RAM.
4. Para **Configuración**, seleccione la red virtual **VNet-Servers** y la subred **SN-Web**.
5. Haga clic en **Dirección IP pública** y, en **Asignación**, haga clic en **Estático** y después en **Aceptar**.
6. Para los puertos de entrada públicos, seleccione **HTTP**, **HTTPS** y **RDP (3389)**, y después haga clic en **Aceptar**.
7. Una página Resumen, haga clic en **Crear**.

   Este procedimiento tarda unos minutos en completarse.
6. Repita este procedimiento para crear otra máquina virtual denominada **Web-02**.

### <a name="add-a-dns-label"></a>Agregar una etiqueta DNS
Las direcciones IP públicas necesitan una etiqueta DNS para funcionar con el Traffic Manager.
1. En el grupo de recursos **RG-DNS-Alias-TM**, haga clic en la dirección IP pública **Web-01-ip**.
2. En **Configuración** haga clic en **Configuración**.
3. En el cuadro de texto de etiqueta de nombre DNS escriba **web01pip**.
4. Haga clic en **Save**(Guardar).

Repita este procedimiento para la dirección IP pública **Web-02-ip** usando **web02pip** para la etiqueta de nombre DNS.

### <a name="install-iis"></a>Instalación de IIS

Instale IIS en **Web-01** y **Web 02**.

1. Conéctese a **Web-01** e inicie sesión.
2. En el panel Administrador del servidor haga clic en **Agregar roles y características**.
3. Haga clic en **Siguiente** tres veces y, en la página **Roles de servidor**, seleccione **Servidor Web (IIS)**.
4. Haga clic en **Agregar características** y después en **Siguiente**.
5. Haga clic en **Siguiente** cuatro veces y después en **Instalar**.

   Esta operación tardará algunos minutos en completarse.
6. Una vez completada la instalación, haga clic en **Cerrar**.
7. Abra un explorador web y vaya a **localhost** para comprobar que aparece la página web IIS de forma predeterminada.

Repita este proceso para instalar IIS en **Web-02**.


## <a name="create-a-traffic-manager-profile"></a>Crear un perfil de Traffic Manager

Due 

1. Abra el grupo de recursos **RG-DNS-Alias-TM** y haga clic en la dirección IP pública **Web-01-ip**. Escriba la dirección IP para su uso posterior. Repita el proceso para la dirección IP pública **Web-02-ip**.
1. Haga clic en **crear un recurso**, haga clic en **Redes** y después en **Perfil de Traffic Manager**.
2. Escriba el nombre **TM-alias-test** y colóquelo en el grupo de recursos **RG-DNS-Alias-TM**.
3. Haga clic en **Create**(Crear).
4. Cuando finalice la implementación, haga clic en **Ir al recurso**.
5. En la página de perfiles de Traffic Manager, en **Configuración**, haga clic en **Extremos**.
6. Haga clic en **Agregar**.
7. Para **Tipo** seleccione **Punto de conexión externo** y para **Nombre** escriba **EP-Web01**.
8. En el cuadro de texto **Nombre de dominio completo (FQDN) o dirección IP** escriba la dirección IP de **Web-01-ip** que anotó anteriormente.
9. Seleccione la misma **Ubicación** que la de los otros recursos y después haga clic en **Aceptar**.

Repita este procedimiento para agregar el punto de conexión **Web-02** mediante la dirección IP que anotó anteriormente para **Web-02-ip**.

## <a name="create-an-alias-record"></a>Crear un registro de alias

Cree un registro de alias que señale al perfil de Traffic Manager.

1. Haga clic en la zona de Azure DNS para abrir la zona.
2. Haga clic en **Conjunto de registros**.
3. Deje el cuadro de texto **Nombre** vacío para representar el vértice de nombre de dominio (por ejemplo, contoso.com).
4. Deje el **Tipo** como un registro **A**.
5. Haga clic en la casilla de verificación **Alias del conjunto de registros**.
6. Haga clic en **Elegir servicio de Azure** y seleccione el **TM-alias-test** perfil de Traffic Manager.

## <a name="test-the-alias-record"></a>Probar el registro de alias

1. Desde un explorador web, vaya al vértice de nombre de dominio (por ejemplo, contoso.com). Debería ver la página web predeterminada de IIS. Cierre el explorador web.
2. Apague la máquina virtual **Web-01** y espere unos minutos para que se apague por completo.
3. Abra un nuevo explorador web y busque el vértice de nombre de dominio de nuevo.
4. Debería volver a ver la página predeterminada de IIS, dado que Traffic Manager controla la situación y dirige el tráfico a **Web-02**.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sea necesario, puede eliminar el grupo de recursos **RG-DNS-Alias-TM** para eliminar todos los recursos creados en este tutorial.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial ha creado un registro de alias para usar el nombre de dominio de vértice para hacer referencia a un perfil de Traffic Manager. Para más información acerca de Azure DNS y sobre las aplicaciones web, continúe con el tutorial sobre estas.

> [!div class="nextstepaction"]
> [Creación de registros DNS para una aplicación web en un dominio personalizado](./dns-web-sites-custom-domain.md)
