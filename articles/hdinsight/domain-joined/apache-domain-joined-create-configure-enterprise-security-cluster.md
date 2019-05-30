---
title: Crear y configurar Enterprise Security Package clústeres en HDInsight de Azure
description: Obtenga información sobre cómo crear y configurar Enterprise Security Package clústeres en HDInsight de Azure
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: howto
ms.date: 05/09/2019
ms.openlocfilehash: 5b5b83fe0028e43ca35bf883b29cb71bad6ca2c8
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66253699"
---
# <a name="create-and-configure-enterprise-security-package-clusters-in-azure-hdinsight"></a>Crear y configurar Enterprise Security Package clústeres en HDInsight de Azure

Enterprise Security Package de HDInsight de Azure proporciona acceso a la autenticación basada en Active Directory, compatibilidad con varios usuario y control de acceso basado en roles para los clústeres de Apache Hadoop en Azure. Clústeres de HDInsight ESP permiten a las organizaciones, que cumplen las directivas de seguridad estricta de la empresa, para procesar datos confidenciales de forma segura.

El objetivo de esta guía se ha configurado correctamente los recursos necesarios, por lo que en el entorno local a los usuarios pueden iniciar sesión en una ESP habilitado el clúster de HDInsight. En este artículo se describe los pasos necesarios para crear un clúster de HDInsight de Azure Enterprise Security Package habilitado. Los pasos se tratará la creación de una máquina virtual de IaaS de Windows con Active Directory & habilitado Servicios de nombres de dominio (DNS). Este servidor actúe como un reemplazo para su **real** entorno local y le permitirá continuar con los pasos de instalación y configuración para que se puede repetir más adelante en su propio entorno. Esta guía también le ayudará a crear un entorno de identidad híbrida con sincronización de hash de contraseñas con Azure Active Directory.

Esta guía está pensada para complementar [Use Enterprise Security Package en HDInsight](apache-domain-joined-architecture.md)

Antes de usar este proceso en su propio entorno, configure Active Directory y servicios de nombres de dominio (DNS). Además, habilite Azure Active Directory y sincronización de cuentas de usuario en el entorno local a Azure Active Directory.

![diagrama de arquitectura](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image002.png)

## <a name="create-on-premises-environment"></a>Crear el entorno local

Información general: En esta sección, usará una plantilla de implementación rápida de Azure para crear nuevas máquinas virtuales, configurar servicios de nombres de dominio (DNS) y un nuevo bosque de AD.

1. Vaya a [crear una máquina virtual de Azure con un nuevo bosque de AD](https://azure.microsoft.com/resources/templates/active-directory-new-domain/), para ver la plantilla de implementación rápida.

1. Haga clic en **implementar en Azure**.
1. Inicie sesión en la suscripción de Azure.
1. En el **crear una máquina virtual de Azure con un nuevo bosque de AD** pantalla, complete los pasos siguientes:
    1. Seleccione la suscripción donde desea que los recursos implementados desde el **suscripción** lista desplegable.
    1. Seleccione **crear nuevo** junto a **grupo de recursos** y escriba el nombre **OnPremADVRG**
    1. Escriba la siguiente información para el resto de los campos de plantilla:

        * **Ubicación**: Centro de EE. UU.
        * **Nombre de usuario administrador**: HDIFabrikamAdmin
        * **Contraseña de administrador**: < Neptuno >
        * **Dominio**: HDIFabrikam.com
        * **Prefijo de DNS**: hdifabrikam

        ![Creación de plantilla de máquina virtual de Azure y el bosque de AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-azure-vm-ad-forest.png)

    1. Haga clic en **compra**
    1. Supervisar la implementación y espere a que finalice.
    1. Confirme que los recursos se crean en el grupo de recursos correcto `OnPremADVRG`.

## <a name="configure-users-and-groups-for-cluster-access"></a>Configurar usuarios y grupos de acceso al clúster

Información general: En esta sección, creará los usuarios que tendrán acceso al clúster de HDInsight al final de esta guía.

1. Conectar con el controlador de dominio mediante Escritorio remoto.
    1. Si ha usado la plantilla se ha mencionado al principio, el controlador de dominio es una máquina virtual denominada **adVM** en el `OnPremADVRG` grupo de recursos.
    1. Vaya a Azure portal > **grupos de recursos** > **OnPremADVRG** > **adVM** > **Connect**.
    1. Haga clic en el **RDP** pestaña y, a continuación, haga clic en **Descargar archivo RDP**.
    1. Guarde el archivo en el equipo y ábralo.
    1. Cuando se le solicite las credenciales, utilice `HDIFabrikam\HDIFabrikamAdmin` como el nombre de usuario y, a continuación, escriba la contraseña que eligió para la cuenta de administrador.

1. Una vez que la sesión de escritorio remoto se abre en la máquina virtual de controlador de dominio, inicie **equipos y usuarios de Active Directory** desde el **administrador del servidor** panel. Haga clic en **herramientas** en la esquina superior derecha y, a continuación, **equipos y usuarios de Active Directory** en la lista desplegable.

    ![Administración de Active Directory abierto de administrador del servidor](./media/apache-domain-joined-create-configure-enterprise-security-cluster/server-manager-active-directory-screen.png)

1. Cree dos nuevos usuarios, **HDIAdmin**, **HDIUser**. Estos dos usuarios se usará para iniciar sesión en los clústeres de HDInsight.

    1. En el **equipos y usuarios de Active Directory** pantalla, haga clic en **acción** > **New** > **usuario**.

        ![Crear nuevo usuario de Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-user.png)

    1. En el **nuevo objeto - usuario** pantalla, escriba `HDIUser` como el **nombre de inicio de sesión de usuario** y haga clic en **siguiente**.

        ![Crear el primer usuario administrador](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image020.png)

    1. En el menú emergente que aparece, escriba la contraseña deseada para la nueva cuenta. Active la casilla que dice **contraseña nunca expira**. HDIClick **Aceptar**.
    1. Haga clic en **finalizar** para crear la nueva cuenta.
    1. Cree otro usuario `HDIAdmin`.

        ![Crear el segundo usuario de administrador](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image024.png)

1. En el **equipos y usuarios de Active Directory** pantalla, haga clic en **acción** > **New** > **grupo**. Crear `HDIUserGroup` como un nuevo grupo.

    ![Crear nuevo grupo de Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-group.png)

    ![Crear nuevo grupo2](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image028.png)

1. Agregar el **HDIUser** creado en el paso anterior para el **HDIUserGroup** como miembro.

    1. Haga clic con el botón derecho en el **HDIUserGroup** y haga clic en **propiedades**.
    1. Vaya a **miembros** ficha y haga clic en **agregar**.
    1. ENTRAR `HDIUser` en el cuadro etiquetado **escriba los nombres de objeto para seleccionar** y haga clic en **Aceptar**.
    1. Repita los pasos anteriores para la otra cuenta `HDIAdmin`

        ![Agregar miembros al grupo](./media/apache-domain-joined-create-configure-enterprise-security-cluster/active-directory-add-users-to-group.png)

Ahora ha creado el entorno de Active Directory, junto con dos usuarios y un grupo de usuarios para acceder al clúster de HDInsight.

Estos usuarios se sincronizarán con Azure AD.

### <a name="create-a-new-azure-active-directory"></a>Crear una nueva Azure Active Directory

1. Inicie sesión en el Portal de Azure.
1. Haga clic en **crear un recurso** y tipo **directory**. Seleccione **Azure Active Directory** > **crear**.
1. Escriba **HDIFabrikam** en **nombreDeOrganización**.
1. Escriba **HDIFabrikamoutlook** en **nombre de dominio inicial**.
1. Haga clic en **Create**(Crear).
1. A la izquierda en Azure portal, haga clic en **Azure Active Directory**.
1. Si es necesario, haga clic en **Cambiar directorio** para cambiar al directorio nuevo que ha creado **HDIFabrikamoutlook**.
1. En **administrar** haga clic en **nombres de dominio personalizados** > **Agregar dominio personalizado**.
1. Escriba **HDIFabrikam.com** en **nombre de dominio personalizado** y haga clic en **Agregar dominio**.

![crear una nueva azure active directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-directory.png)

![crear un nuevo dominio personalizado](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-custom-domain.png)

## <a name="configure-your-azure-ad-tenant"></a>Configurar el inquilino de Azure AD

Información general: Ahora configurará su inquilino de Azure AD para que pueda sincronizar los usuarios y grupos locales de AD a la nube.

1. Crear un administrador de inquilinos de AD.
    1. Inicie sesión en el portal de Azure y seleccione el inquilino de Azure AD **HDIFabrikam**
    1. Seleccione **usuarios** en **administrar** y, a continuación, **nuevo usuario**.
    1. Escriba la siguiente información para el nuevo usuario:

        * Name: fabrikamazureadmin
        * Nombre de usuario: fabrikamazureadmin@hdifabrikam.com
        * Contraseña: contraseña segura de su elección

    1. Haga clic en el **grupos** sección, busque **AAD DC Administrators**y haga clic en **seleccione**.

        ![Grupos](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image038.png)

    1. Haga clic en el **rol del directorio** sección y seleccione **administrador Global** en el lado derecho. Haga clic en **Aceptar**.

        ![Rol del directorio](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image040.png)

    1. Escriba una contraseña para el usuario. Haga clic en **Create**(Crear).

1. Si desea cambiar la contraseña para el usuario recién creado <fabrikamazureadmin@hdifabrikam.com>. Inicie sesión en Azure portal mediante que la identidad y, a continuación, se le pedirá que cambie la contraseña.

## <a name="sync-on-premises-users-to-azure-ad"></a>Sincronización de usuarios locales a Azure AD

### <a name="download-and-install-microsoft-azure-active-directory-connect"></a>Descargue e instale Microsoft Azure Active Directory connect

1. [Descarga de Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594).

1. Instalación de Microsoft Azure Active Directory connect en el controlador de dominio.
    1. Abra el archivo ejecutable que descargó en el paso anterior y acepte los términos de licencia. Haga clic en **Continue**.

        ![Azure AD Connect](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image052.png)

    1. Haga clic en **usar configuración rápida** y completar la instalación.

        ![Usar configuración rápida](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image054.png)

### <a name="configure-sync-with-on-premises-domain-controller"></a>Configurar la sincronización con el controlador de dominio local

1. En el **conectar con Azure AD** pantalla, escriba el nombre de usuario y la contraseña del administrador global de Azure AD. Haga clic en **siguiente**. Este es el nombre de usuario `fabrikamazureadmin@hdifabrikam.com` que creó al configurar el inquilino de AD.
    ![Conectarse a Azure AD](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image058.png)
1. En el **conectar con Active Directory Domain Services** pantalla, escriba el nombre de usuario y la contraseña para una cuenta de administrador de empresa. Haga clic en **siguiente**. Este es el nombre de usuario `HDIFabrikam\HDIFabrikamAdmin` y su correspondiente contraseña que creó anteriormente.

   ![Conectarse a servicios de dominio de Active Directory](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image060.png)
1. En el **configuración de inicio de sesión de Azure AD** página, haga clic en **siguiente**.
    ![Configuración de inicio de sesión de AD Azure](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image062.png)
1. En la pantalla listo para configurar, haga clic en **instalar**.
    ![install](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image064.png)
1. Cuando el **completar configuración** se muestra la pantalla, haga clic en **Exit**.
    ![Configuración completada](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image078.png)

1. Una vez completada la sincronización confirme si los usuarios que ha creado en Active Directory de IAAS se sincronizan con Azure Active Directory.
    1. Inicie sesión en el Portal de Azure.
    1. Seleccione **Azure Active Directory** > **HDIFabrikam** > **usuarios**.

### <a name="create-an-user-assigned-managed-identity"></a>Crear una identidad administrada asignada por el usuario

Crear una identidad administrada asignada por el usuario que se usará para configurar Azure Active Directory Domain Services (Azure AD-DS). Para obtener más información sobre cómo crear una identidad administrada asignada por el usuario, consulte [Create, list, eliminar o asignar un rol a una identidad administrada asignada por el usuario mediante el portal de Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

1. Inicie sesión en el Portal de Azure.
1. Haga clic en **crear un recurso** y tipo **identidad administrada**. Seleccione **usuario asignado identidad administrada** > **crear**.
1. Escriba **HDIFabrikamManagedIdentity** como el **nombre de recurso**.
1. Seleccione su suscripción.
1. En **grupo de recursos** haga clic en **crear nuevo** y escriba **HDIFabrikam CentralUS**.
1. Seleccione **EE. UU** en **ubicación**.
1. Haga clic en **Create**(Crear).

![Cree una nueva identidad administrada asignada por el usuario](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image082.png)

### <a name="enable-azure-active-directory-domain-services"></a>Habilitación de Azure Active Directory Domain Services

Para obtener más información, consulte [Habilitación de Azure Active Directory Domain Services mediante Azure Portal](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started).

1. Cree la red Virtual al host de Azure AD DS. Ejecute el siguiente código de powershell.

    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Set-AzContext -Subscription 'SUBSCRIPTION_ID'
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS' -Location 'Central US' -Name 'HDIFabrikam-AADDSVNET' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'AADDS-subnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Inicie sesión en el Portal de Azure.
1. Haga clic en **crear recurso**, escriba **servicios de dominio** y seleccione **Azure AD Domain Services**.
1. En el **Fundamentos** pantalla complete los pasos siguientes:
    1. En **nombre del directorio** seleccione Azure Active Directory creado en este tutorial, **HDIFabrikam**.
    1. Escriba un **nombre de dominio DNS** de **HDIFabrikam.com**.
    1. Seleccione su suscripción.
    1. Especifique el grupo de recursos **HDIFabrikam CentralUS** y **ubicación** de **EE. UU**.

        ![detalles básicos de Azure ad ds](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image084.png)

1. En el **red** de pantalla completa, seleccione la red (**HDIFabrikam VNET**) y la subred (**AADDS subred**) que ha creado con el script de powershell anterior. O bien puede usar el **crear nuevo** opción para crear una red virtual ahora.

    ![Seleccione la red](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image086.png)

1. En el **grupo administrador** pantalla, verá una notificación de que un grupo denominado **AAD DC Administrators** ya se ha creado para administrar este grupo. Opcionalmente, puede modificar la pertenencia a este grupo, pero no es necesario para los pasos de este tutorial. Haga clic en **OK**.

    ![grupo de administradores de la vista](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image088.png)

1. En el **sincronización** pantalla, habilitar la sincronización completa seleccionando **todas** y, a continuación, haga clic en **Aceptar**.

    ![habilitar la sincronización](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image090.png)

1. En el **resumen** pantalla, compruebe los detalles de Azure AD-DS y haga clic en **Aceptar**.

    ![Compruebe los detalles](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image092.png)

1. Después de habilitar Azure AD DS, se ejecuta un servidor local del Servicio de nombres de dominio (DNS) en las máquinas virtuales (VM) de AD.

### <a name="configure-your-azure-ad-ds-virtual-network"></a>Configurar la red virtual de Azure AD DS

Los pasos descritos en esta sección le ayudará a configurar la red virtual de Azure AD DS (**HDIFabrikam AADDSVNET**) para usar los servidores DNS personalizados.

1. Busque las direcciones IP de los servidores DNS personalizados. Haga clic en el **HDIFabrikam.com** recursos de AD DS, haga clic en **propiedades** en **administrar**   y examine las direcciones IP mostradas en **IP Dirección de red Virtual**.

    ![Busque las direcciones IP de DNS personalizadas para Azure AD DS](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image096.png)

1. Configurar **HDIFabrikam AADDSVNET** a direcciones IP personalizada `10.0.0.4` y `10.0.0.5`.

    1. Seleccione **servidores DNS** bajo el **configuración** categoría. a continuación, haga clic en el botón de radio junto a **personalizado**, escriba la primera dirección IP (10.0.0.4) en el cuadro de texto siguiente y haga clic en **guardar**.
    1. Agregar más direcciones IP (10.0.0.5) siguiendo los mismos pasos.

1. En nuestro escenario de Azure AD DS se configuró para usar direcciones IP 10.0.0.4 y 10.0.0.5, establecer la misma dirección IP en la red virtual AADDS como se muestra en la imagen siguiente.

    ![ver los servidores dns personalizados](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image098.png)

## <a name="securing-ldap-traffic"></a>Protección del tráfico de LDAP

Protocolo de acceso de directorio ligero (LDAP) se utiliza para leer y escribir en Active Directory. Puede realizar LDAP tráfico confidencialidad y la seguridad mediante capa de Sockets seguros (SSL) o tecnología de capa de transporte (TLS). Puede habilitar LDAP a través de SSL (LDAPS) mediante la instalación de un certificado con el formato correcto.

Para obtener más información sobre LDAP seguro, consulte [LDAP seguro configurar (LDAPS) para un Azure AD Domain Services managed domain](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap).

En esta sección, cree un certificado autofirmado, descargue el certificado y configurar LDAP seguro (LDAPS) para el **hdifabrikam** el dominio administrado de Azure AD DS.

El script siguiente crea un certificado para hdifabrikam. El certificado se guarda en la ruta de acceso "LocalMachine".

> [!Note] 
> Cualquier aplicación que crea un PKCS válido o la utilidad \#10 puede usarse para formar la solicitud de certificado SSL de la solicitud.

```powershell
$lifetime = Get-Date
New-SelfSignedCertificate -Subject hdifabrikam.com `
-NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
-Type SSLServerAuthentication -DnsName *.hdifabrikam.com, hdifabrikam.com
```

Compruebe que el certificado está instalado en el equipo\'almacén Personal s. Complete los siguientes pasos:

1. Inicie Microsoft Management Console (MMC).
1. Agregue el complemento certificados que administra los certificados en el equipo local.
1. Expanda **certificados (equipo Local)** , expanda **Personal**y, a continuación, expanda **certificados**. Un nuevo certificado debe existir en el almacén Personal. Este certificado se emite para el nombre de host completo.

    ![comprobar la creación de certificados](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image102.png)

1. En el panel derecho, haga clic en el certificado que creó en el paso anterior, elija **todas las tareas**y, a continuación, haga clic en **exportar**.

1. En el **exportar la clave privada** página, haga clic en **Sí, exportar la clave privada,** . Falta la clave privada para los mensajes cifrados para leerse desde el equipo donde se importará la clave.

    ![exportar clave privada](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image103.png)

1. En el **Exportar formato de archivo** página, deje la configuración predeterminada y, a continuación, haga clic en **siguiente**. 
1. En el **contraseña** página, escriba una contraseña para la clave privada, seleccione **TripleDES SHA1** para **cifrado** y haga clic en **siguiente**.
1. En el **archivo para exportar** página, escriba la ruta de acceso y el nombre del archivo de certificado exportado y, a continuación, haga clic en **siguiente**.
1. El nombre de archivo tiene que ser extensión .pfx, este archivo está configurado en Azure portal para establecer una conexión segura.
1. Habilitar LDAP seguro (LDAPS) para un dominio administrado de Azure AD Domain Services.
    1. Seleccione el dominio **HDIFabrikam.com** desde el portal de Azure.
    1. Haga clic en **LDAP seguro** en **administrar**.
    1. En el **LDAP seguro** pantalla, haga clic en **habilitar** en **LDAP seguro**.
    1. Busque el archivo de certificado .pfx que exportó en el equipo.
    1. Escriba la contraseña del certificado.

    ![Habilitación de ldap seguro](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image113.png)

1. Ahora que ha habilitado el protocolo LDAP seguro, asegúrese de que sea accesible al habilitar el puerto 636.
    1. Haga clic en el grupo de seguridad de red **AADDS-HDIFabrikam.com-NSG** en el **HDIFabrikam CentralUS** grupo de recursos.
    1. En **configuración** haga clic en **reglas de seguridad de entrada** > **agregar**.
    1. En el **Agregar regla de seguridad de entrada** pantalla, especifique las siguientes propiedades y haga clic en **agregar**:

        | Propiedad | Valor |
        |---|---|
        | Origen | Cualquiera |
        | Intervalos de puertos de origen | * |
        | Destino | Cualquiera |
        | Intervalo de puertos de destino | 636 |
        | Protocol | Cualquiera |
        | . | PERMITIR |
        | Prioridad | <Desired Number> |
        | NOMBRE | Port_LDAP_636 |

    ![regla de seguridad de entrada](./media/apache-domain-joined-create-configure-enterprise-security-cluster/add-inbound-security-rule.png)

1. `HDIFabrikamManagedIdentity` es la asignada por el usuario de la identidad administrada, el rol de colaborador de servicios de dominio de HDInsight está habilitado para la identidad administrada que le permitirán esta identidad leer, crear, modificar y eliminar operaciones de servicios de dominio.

    ![Crear la identidad administrada asignada por el usuario](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image117.png)

## <a name="creating-enterprise-security-package-enabled-hdinsight-cluster"></a>Crear Enterprise Security Package habilitado para el clúster de HDInsight

Este paso requiere los siguientes requisitos previos:

1. Crear un nuevo grupo de recursos `HDIFabrikam-WestUS` en la ubicación `West US`.
1. Crear una virtual network que hospedará ESP habilitado el clúster de HDInsight.

    ```powershell
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS' -Location 'West US' -Name 'HDIFabrikam-HDIVNet' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'SparkSubnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Crear una relación del mismo nivel entre la red Virtual que hospeda AADDS (`HDIFabrikam-AADDSVNET`) y la red Virtual que va a hospedar la ESP habilitado el clúster de HDInsight (`HDIFabrikam-HDIVNet `). Use el siguiente código de powershell para emparejar estas dos redes virtuales.

    ```powershell
    Add-AzVirtualNetworkPeering -Name 'HDIVNet-AADDSVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS')

    Add-AzVirtualNetworkPeering -Name 'AADDSVNet-HDIVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS')
    ```

1. Crear una nueva cuenta de Azure Data Lake Storage Gen2, **Hdigen2store**, que se configura con la identidad del usuario administrado **HDIFabrikamManagedIdentity**. Para obtener más información sobre la creación de cuentas de Data Lake Storage Gen2 habilitadas con usuario identidades administradas, vea [clústeres uso Azure Data Lake Storage Gen2 con Azure HDInsight](../hdinsight-hadoop-use-data-lake-storage-gen2.md).

1. Configuración de DNS personalizado en el **HDIFabrikam AADDSVNET** red virtual.
    1. Vaya a Azure portal > **grupos de recursos** > **OnPremADVRG** > **HDIFabrikam AADDSVNET**  >   **Servidores DNS**.
    1. Seleccione **personalizado** y escriba `10.0.0.4` y `10.0.0.5`.
    1. Haga clic en **Save**(Guardar).

        ![guardar la configuración de dns personalizado](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image123.png)

1. Crear un nuevo clúster habilitado para ESP HDInsight Spark.
    1. Haga clic en **personalizado (tamaño, configuración, aplicaciones)** .
    2. Escriba los detalles deseados de la sección 1 **Fundamentos**. Asegúrese de que el **tipo de clúster** es **Spark 2.3 (HDI 3.6)** y **grupo de recursos** es **HDIFabrikam CentralUS**

    1. En la sección 2 **seguridad + redes**, complete los pasos siguientes:
        1. Haga clic en **habilitado** en **Enterprise Security Package**.
        1. Haga clic en **usuario del Administrador de clúster** y seleccione el **HDIAdmin** cuenta que creó anteriormente como el usuario de administrador local. Haga clic en **Seleccionar**.

        1. Haga clic en **grupo de acceso de clúster** y, a continuación, seleccione **HDIUserGroup**. Cualquier usuario que agregue a este grupo en el futuro podrá obtener acceso a los clústeres de HDInsight.

            ![Seleccione el grupo de acceso de clúster](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image129.jpg)

    1. Complete los demás pasos de la configuración del clúster y compruebe los detalles en el **resumen del clúster**. Haga clic en **Create**(Crear).

1. Inicie sesión en la UI de Ambari para el clúster recién creado en `https://CLUSTERNAME.azurehdinsight.net` utilizando su nombre de usuario administrador `hdiadmin@hdifabrikam.com` y la contraseña.

    ![Inicie sesión en Ambari](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image135.jpg)

1. Haga clic en **Roles** desde el panel del clúster.
1. En el **Roles** , escriba el grupo **hdiusergroup** para asignarla a la **el Administrador de clústeres** rol bajo **asignar roles a estos**.

    ![Asigne el rol de administrador de clúster para hdiusergroup](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image137.jpg)

1. Abra su cliente SSH y el inicio de sesión en el clúster con el **hdiuser** que creó anteriormente en Active Directory local.

    ![Inicie sesión en el clúster con SSH](./media/apache-domain-joined-create-configure-enterprise-security-cluster/image139.jpg)

Si es posible iniciar sesión con esta cuenta, ha configurado el clúster ESP correctamente para la sincronización con su Active Directory local.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a la seguridad de Apache Hadoop con Enterprise Security Package](apache-domain-joined-introduction.md)
