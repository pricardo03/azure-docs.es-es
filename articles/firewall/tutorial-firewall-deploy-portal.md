---
title: 'Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal'
description: En este tutorial, aprenderá a implementar y configurar Azure Firewall mediante Azure Portal.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 11/6/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 4873da97b790df98b6d10ae8b7a57fc39b534755
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51278589"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-using-the-azure-portal"></a>Tutorial: Implementación y configuración de Azure Firewall mediante Azure Portal

El control del acceso de red saliente es una parte importante de un plan de seguridad de red de ámbito general. Por ejemplo, puede que quiera limitar el acceso a sitios web, o las direcciones IP de salida y los puertos a los que se puede acceder.

Una manera de controlar el acceso de red saliente desde una subred de Azure es con Azure Firewall. Con Azure Firewall, puede configurar:

* Reglas de aplicación que definen los nombres de dominio completos (FQDN) a los que se puede acceder desde una subred.
* Reglas de red que definen la dirección de origen, el protocolo, el puerto de destino y la dirección de destino.

El tráfico está sujeto a las reglas de firewall configuradas cuando enruta el tráfico al firewall como puerta de enlace predeterminada de la subred.

En este tutorial, creará una red virtual única simplificada con tres subredes para facilitar la implementación. En el caso de las implementaciones de producción, se recomienda un [modelo tipo hub-and-spoke](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke), en el que el firewall está en su propia red virtual y los servidores de las cargas de trabajo están en redes virtuales emparejadas de la misma región con una o más subredes.

- **AzureFirewallSubnet**: el firewall está en esta subred.
- **Workload-SN**: el servidor de carga de trabajo está en esta subred. El tráfico de red de esta subred va a través del firewall.
- **Jump-SN**: el servidor de "salto" está en esta subred. El servidor de salto tiene una dirección IP pública a la que puede conectarse mediante Escritorio remoto. Desde allí se puede conectar posteriormente al servidor de carga de trabajo (mediante otro Escritorio remoto).

![Infraestructura de red del tutorial](media/tutorial-firewall-rules-portal/Tutorial_network.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar un entorno de red de prueba
> * Implementar un firewall
> * Crear una ruta predeterminada
> * Configuración de una aplicación para permitir el acceso a github.com
> * Configuración de una regla de red para permitir el acceso a los servidores DNS externos
> * Probar el firewall

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="set-up-the-network"></a>Configuración de la red

En primer lugar, cree un grupo de recursos para que contenga los recursos necesarios para implementar el firewall. A continuación, cree una red virtual, subredes y pruebe los servidores.

### <a name="create-a-resource-group"></a>Crear un grupo de recursos

El grupo de recursos contiene todos los recursos necesarios para el tutorial.

1. Inicie sesión en Azure Portal en [http://portal.azure.com](http://portal.azure.com).
2. En la página principal de Azure Portal, haga clic en **Grupos de recursos** > **Agregar**.
3. En **Nombre del grupo de recursos**, escriba **Test-FW-RG**.
4. En **Suscripción**, seleccione la suscripción.
5. En **Ubicación del grupo de recursos**: seleccione una ubicación. Todos los recursos posteriores que cree deben estar en la misma ubicación.
6. Haga clic en **Create**(Crear).

### <a name="create-a-vnet"></a>Creación de una red virtual

Esta red virtual contiene tres subredes.

1. En la página principal de Azure Portal, haga clic en **Todos los servicios**.
2. En **Redes**, haga clic en **Redes virtuales**.
3. Haga clic en **Agregar**.
4. En **Nombre**, escriba **Test-FW-VN**.
5. En **Espacio de direcciones**, escriba **10.0.0.0/16**.
6. En **Suscripción**, seleccione la suscripción.
7. En **Grupo de recursos**, seleccione **Usar existente** > **Test-FW-RG**.
8. En **Ubicación**, seleccione la misma ubicación que usó anteriormente.
9. En **Subred**, como **Nombre** escriba **AzureFirewallSubnet**. El firewall estará en esta subred y el nombre de la subred **debe** ser AzureFirewallSubnet.
10. En **Intervalo de direcciones**, escriba **10.0.1.0/24**.
11. Utilice los otros valores predeterminados y, a continuación, haga clic en **Crear**.

> [!NOTE]
> El tamaño mínimo de la subred AzureFirewallSubnet es /25.

### <a name="create-additional-subnets"></a>Creación de subredes adicionales

A continuación, cree las subredes para el servidor de salto y una subred para los servidores de cargas de trabajo.

1. En la página principal de Azure Portal, haga clic en **Grupos de recursos** > **Test-FW-RG**.
2. Haga clic en la red virtual **Test-FW-VN**.
3. Haga clic en **Subredes** > **+Subred**.
4. En **nombre**, escriba **Workload-SN**.
5. En **Intervalo de direcciones**, escriba **10.0.2.0/24**.
6. Haga clic en **OK**.

Cree otra subred denominada **Jump-SN**, con un intervalo de direcciones **10.0.3.0/24**.

### <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Ahora cree las máquinas virtuales de salto y de cargas de trabajo, y colóquelas en las subredes adecuadas.

1. En la página principal de Azure Portal, haga clic en **Todos los servicios**.
2. En **Compute**, haga clic en **Máquinas virtuales**.
3. Haga clic en **Agregar** > **Windows Server** > **Windows Server 2016 Datacenter** > **Crear**.

**Aspectos básicos**

1. En **Nombre**, escriba **Srv-Jump**.
5. Escriba un nombre de usuario y una contraseña.
6. En **Suscripción**, seleccione la suscripción.
7. En **Grupo de recursos**, haga clic en **Usar existente** > **Test-FW-RG**.
8. En **Ubicación**, seleccione la misma ubicación que usó anteriormente.
9. Haga clic en **OK**.

**Tamaño**

1. Elija un tamaño apropiado para una máquina virtual de prueba que ejecute Windows Server. Por ejemplo, **B2ms** (8 GB de RAM, 16 GB de almacenamiento).
2. Haga clic en **Seleccionar**.

**Configuración**

1. En **Red**, como **Red virtual**, seleccione **Test-FW-VN**.
2. En **Subred**, seleccione **Jump-SN**.
3. En **Seleccionar puertos de entrada públicos**, seleccione **RDP (3389)**. 

    Desea limitar el acceso a la dirección IP pública, pero deberá abrir el puerto 3389 para que pueda conectar un escritorio remoto al servidor de salto. 
2. Deje los demás valores predeterminados y haga clic en **Aceptar**.

**Resumen**

Revise el resumen y luego haga clic en **Crear**. Esta operación tardará algunos minutos en completarse.

Repita este proceso para crear otra máquina virtual denominada **Srv-Work**.

Use la información de la tabla siguiente para configurar la **configuración** de la máquina virtual Srv-Work. El resto de la configuración es la misma que la de la máquina virtual Srv-Jump.

|Configuración  |Valor  |
|---------|---------|
|Subred|Workload-SN|
|Dirección IP pública|None|
|Seleccionar puertos de entrada públicos|No hay puertos de entrada públicos|

## <a name="deploy-the-firewall"></a>Implementación del firewall

Implemente el firewall en la red virtual.

1. En la página principal del portal, haga clic en **Crear un recurso**.
2. Haga clic en **Redes** y, después, en **Destacados**, haga clic en **Ver todo**.
3. Haga clic en **Firewall** > **Crear**. 
4. En la página **Creación de un firewall**, utilice la tabla siguiente para configurar el firewall:
   
   |Configuración  |Valor  |
   |---------|---------|
   |NOMBRE     |Test-FW01|
   |Subscription     |\<su suscripción\>|
   |Grupos de recursos     |**Usar existente**: Test-FW-RG |
   |Ubicación     |Seleccione la misma ubicación que usó anteriormente.|
   |Elegir una red virtual     |**Usar existente**: Test-FW-VN|
   |Dirección IP pública     |**Cree uno nuevo**. La dirección IP pública tiene que ser del tipo de SKU estándar.|

2. Haga clic en **Revisar + crear**.
3. Revise el resumen y luego haga clic en **Crear** para crear el firewall.

   La implementación tardará varios minutos.
4. Una vez finalizada la implementación, vaya al grupo de recursos **Test-FW-RG** y haga clic en el firewall **Test-FW01**.
6. Anote la dirección IP privada. Se usará más adelante al crear la ruta predeterminada.

## <a name="create-a-default-route"></a>Crear una ruta predeterminada

En la subred **Workload-SN**, configure la ruta predeterminada de salida que pase por el firewall.

1. En la página principal de Azure Portal, haga clic en **Todos los servicios**.
2. En **Redes**, haga clic en **Tablas de rutas**.
3. Haga clic en **Agregar**.
4. En **Nombre**, escriba **Firewall-route**.
5. En **Suscripción**, seleccione la suscripción.
6. En **Grupo de recursos**, seleccione **Usar existente** y, después, **Test-FW-RG**.
7. En **Ubicación**, seleccione la misma ubicación que usó anteriormente.
8. Haga clic en **Create**(Crear).
9. Haga clic en **Actualizar**y, a continuación, haga clic en la tabla de rutas **Firewall-route**.
10. Haga clic en **Subredes** > **Asociar**.
11. Haga clic en **Red virtual** > **FW-Test-VN**.
12. En **Subred**, haga clic en **Workload-SN**. Asegúrese de seleccionar únicamente la subred **Workload-SN** para esta ruta, o el firewall no funcionará correctamente.

13. Haga clic en **OK**.
14. Haga clic en **Rutas** > **Agregar**.
15. En **Nombre de ruta**, escriba **FW-DG**.
16. En **Prefijo de dirección** escriba **0.0.0.0/0**.
17. En **Tipo del próximo salto**, seleccione **Aplicación virtual**.

    Azure Firewall es realmente un servicio administrado, pero una aplicación virtual funciona en esta situación.
18. En **Dirección del próximo salto**, escriba la dirección IP privada del firewall que anotó anteriormente.
19. Haga clic en **OK**.

## <a name="configure-an-application-rule"></a>Configuración de una regla de aplicación

Se trata de la regla de aplicación que permite el acceso saliente a github.com.

1. Abra **Test-FW-RG**y haga clic en el firewall **Test-FW01**.
2. En la página **Test-FW01**, en **Configuración**, haga clic en **Reglas**.
3. Haga clic en **Agregar una colección de reglas de aplicación**.
4. En **Nombre**, escriba **App-Coll01**.
5. En **Priority**, escriba **200**.
6. En **Acción**, seleccione **Permitir**.
7. En **Reglas**, como **Nombre**, escriba **AllowGH**.
8. En **Direcciones de origen**, escriba **10.0.2.0/24**.
9. En **Protocolo:Puerto**, escriba **http, https**.
10. En **FQDN de destino**, escriba **github.com**
11. Haga clic en **Agregar**.

Azure Firewall incluye una colección de reglas integradas para FQDN de infraestructura que están permitidos de forma predeterminada. Estos FQDN son específicos para la plataforma y no se pueden usar para otros fines. Para más información, consulte [Nombres de dominio completos de infraestructura](infrastructure-fqdns.md).

## <a name="configure-a-network-rule"></a>Configurar una regla de red

Se trata de la regla de red que permite el acceso saliente a dos direcciones IP en el puerto 53 (DNS).

1. Haga clic en **Agregar una colección de reglas de red**.
2. En **Nombre**, escriba **Net-Coll01**.
3. En **Priority**, escriba **200**.
4. En **Acción**, seleccione **Permitir**.

6. En **Reglas**, como **Nombre**, escriba **AllowDNS**.
8. En **Protocolo**, seleccione **UDP**.
9. En **Direcciones de origen**, escriba **10.0.2.0/24**.
10. Como dirección de destino, escriba **209.244.0.3,209.244.0.4**
11. En **Puertos de destino**, escriba **53**.
12. Haga clic en **Agregar**.

### <a name="change-the-primary-and-secondary-dns-address-for-the-srv-work-network-interface"></a>Cambio de la dirección DNS principal y secundaria para la interfaz de red **Srv-Work**

Con fines de prueba para este tutorial, configure las direcciones DNS principal y secundaria. Esto no es un requisito general de Azure Firewall.

1. En Azure Portal, abra el grupo de recursos **Test-FW-RG**.
2. Haga clic en la interfaz de red de la máquina virtual **Srv-Work**.
3. En **Configuración**, haga clic en **Servidores DNS**.
4. En **Servidores DNS**, haga clic en **Personalizado**.
5. Escriba **209.244.0.3** en el cuadro de texto **Agregar servidor DNS** y **209.244.0.4** en el siguiente cuadro de texto.
6. Haga clic en **Save**(Guardar). 
7. Reinicie la máquina virtual **Srv-Work**.

## <a name="test-the-firewall"></a>Probar el firewall

Ahora, pruebe el firewall para confirmar que funciona según lo previsto.

1. En Azure Portal, revise la configuración de red de la máquina virtual **Srv-Work** y anote la dirección IP privada.
2. Conecte un escritorio remoto a la máquina virtual **Srv-Jump** y desde allí abra una conexión de escritorio remoto a la dirección IP privada **Srv-Work**.

5. Abra Internet Explorer y vaya a http://github.com.
6. Haga clic en **Aceptar** > **Cerrar** en las alertas de seguridad.

   Debería ver la página principal de GitHub.

7. Vaya a http://www.msn.com.

   El firewall debería bloquearle.

Con ello, ha comprobado que funcionan las reglas de firewall:

- Puede navegar al FQDN permitido pero no a ningún otro.
- Puede resolver nombres DNS con el servidor DNS externo configurado.

## <a name="clean-up-resources"></a>Limpieza de recursos

Puede conservar los recursos relacionados con el firewall para el siguiente tutorial o, si ya no los necesita, eliminar el grupo de recursos **Test-FW-RG** para eliminarlos todos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial: Supervisión de los registros de Azure Firewall](./tutorial-diagnostics.md)
