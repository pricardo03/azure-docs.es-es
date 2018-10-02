---
title: 'Tutorial: Creación de un registro de alias de Azure DNS para hacer referencia a una dirección IP pública de Azure.'
description: En este tutorial se muestra cómo configurar un registro de alias de Azure DNS para hacer referencia a una dirección IP pública de Azure.
services: dns
author: vhorne
ms.service: dns
ms.topic: tutorial
ms.date: 9/25/2018
ms.author: victorh
ms.openlocfilehash: 842015d853790e3ac78214cca6a70becb7f9fadf
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46991239"
---
# <a name="tutorial-configure-an-alias-record-to-refer-to-an-azure-public-ip-address"></a>Tutorial: Configuración de un registro de alias para hacer referencia a una dirección IP pública de Azure 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una infraestructura de red
> * Crear una máquina virtual de servidor web
> * Crear un registro de alias
> * Probar el registro de alias


Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos
Debe tener un nombre de dominio disponible que pueda hospedar en Azure DNS para realizar las pruebas. Debe tener control total de este dominio, incluida la capacidad de establecer los registros de nombres de servidor (NS) para el dominio.

Para que obtener instrucciones para hospedar el dominio en DNS de Azure, consulte [Tutorial: Hospedaje del dominio en Azure DNS](dns-delegate-domain-azure-dns.md).

El dominio en el ejemplo que se usa en este tutorial es contoso.com, pero debe usar su propio nombre de dominio.

## <a name="create-the-network-infrastructure"></a>Creación de la infraestructura de red
En primer lugar, cree una red virtual y una subred en la que colocar los servidores web.
1. Inicie sesión en Azure Portal en http://portal.azure.com.
2. En la parte superior izquierda en el portal, haga clic en **Crear un recurso**, escriba *grupo de recursos* en el cuadro de búsqueda y cree un grupo de recursos denominado **RG-DNS-Alias-pip**.
3. Haga clic sucesivamente en **Crear un recurso**, **Redes** y **Red virtual**.
4. Cree una red virtual denominada **VNet-Server**, colóquela en el grupo de recursos **RG-DNS-Alias-pip** y asigne a la subred el nombre **SN-Web**.

## <a name="create-a-web-server-virtual-machine"></a>Crear una máquina virtual de servidor web
1. Haga clic en **Crear un recurso** y después en **Máquina virtual de Windows Server 2016**.
2. Escriba **Web-01** para el nombre y coloque la máquina virtual en el grupo de recursos **RG-DNS-Alias-TM**. Escriba un nombre de usuario y una contraseña y haga clic en **Aceptar**.
3. Para **Tamaño**, elija una SKU con 8 GB de RAM.
4. En **Configuración**, seleccione la red virtual **VNet-Servers** y la subred **SN-Web**. Para los puertos de entrada públicos, seleccione **HTTP**, **HTTPS** y **RDP (3389)**, y después haga clic en **Aceptar**.
5. Una página Resumen, haga clic en **Crear**.

   Este procedimiento tarda unos minutos en completarse.

### <a name="install-iis"></a>Instalación de IIS

Instale IIS en **Web-01**.

1. Conéctese a **Web-01** e inicie sesión.
2. En el panel Administrador del servidor haga clic en **Agregar roles y características**.
3. Haga clic en **Siguiente** tres veces y, en la página **Roles de servidor**, seleccione **Servidor Web (IIS)**.
4. Haga clic en **Agregar características** y después en **Siguiente**.
5. Haga clic en **Siguiente** cuatro veces y después en **Instalar**.

   Esta operación tardará algunos minutos en completarse.
6. Una vez completada la instalación, haga clic en **Cerrar**.
7. Abra un explorador web y vaya a **localhost** para comprobar que aparece la página web de IIS de forma predeterminada.

## <a name="create-an-alias-record"></a>Crear un registro de alias

Cree un registro de alias que apunte a la dirección IP pública.

1. Haga clic en la zona de Azure DNS para abrirla.
2. Haga clic en **Conjunto de registros**.
3. En el cuadro de texto **Nombre**, escriba **web01**.
4. Deje el **Tipo** como un registro **D**.
5. Haga clic en la casilla **Alias Record Set** (Conjunto de registros de alias).
6. Haga clic en **Choose Azure service** (Elegir servicio de Azure) y seleccione la dirección IP pública **Web-01-ip**.

## <a name="test-the-alias-record"></a>Probar el registro de alias

1. En el grupo de recursos **RG-DNS-Alias-pip**, haga clic en la máquina virtual **Web-01**. Anote la dirección IP pública.
1. Desde un explorador web, busque el nombre de dominio completo de la máquina virtual Web01-01. Por ejemplo: **web01.contoso.com**. Verá la página web predeterminada de IIS.
2. Cierre el explorador web.
3. Detenga la máquina virtual **Web-01** y, a continuación, reiníciela.
4. Cuando se reinicie, anote la nueva dirección IP pública de la máquina virtual.
5. Abra un nuevo explorador y busque de nuevo el nombre de dominio completo de la máquina virtual Web01-01. Por ejemplo: **web01.contoso.com**.
6. Como usó un registro de alias para apuntar al recurso de dirección IP pública y no un registro D estándar, este nombre debería aún funcionar.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no sea necesario, puede eliminar el grupo de recursos **RG-DNS-Alias-pip** para eliminar todos los recursos creados en este tutorial.


## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado un registro de alias para hacer referencia a una dirección IP pública de Azure. Para más información acerca de Azure DNS y sobre las aplicaciones web, continúe con el tutorial sobre estas.

> [!div class="nextstepaction"]
> [Creación de registros DNS para una aplicación web en un dominio personalizado](./dns-web-sites-custom-domain.md)
