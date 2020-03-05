---
title: Solución de problemas de replicación de máquinas virtuales de Azure en Azure Site Recovery
description: Solución de errores al replicar máquinas virtuales de Azure para la recuperación ante desastres.
author: rochakm
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: rochakm
ms.openlocfilehash: 32d826f3c27cea3d0993c47e8562360315b7bd2e
ms.sourcegitcommit: d4a4f22f41ec4b3003a22826f0530df29cf01073
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/03/2020
ms.locfileid: "78256043"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-errors"></a>Solución de errores de replicación de máquinas virtuales de Azure en Azure

En este artículo se describen los errores comunes de Azure Site Recovery durante la replicación y recuperación de máquinas virtuales (VM) de Azure de una región a otra. Para más información sobre las configuraciones admitidas, consulte la [matriz de compatibilidad para la replicación de máquinas virtuales de Azure](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problemas de cuota de recursos de Azure (código de error 150097)

Asegúrese de que la suscripción está habilitada para crear máquinas virtuales de Azure en la región de destino que se va a utilizar como región de recuperación ante desastres. Asegúrese también de que la suscripción tiene cuota suficiente para crear máquinas virtuales de los tamaños necesarios. De forma predeterminada, Site Recovery elige un tamaño de VM de destino que es igual que el tamaño de VM de origen. Si el tamaño coincidente no está disponible, Site Recovery elige automáticamente el tamaño disponible más cercano.

Si no hay ningún tamaño que admita la configuración de la máquina virtual de origen, aparece este mensaje:

> "No se pudo habilitar la replicación para la máquina virtual *VmName*."

### <a name="possible-causes"></a>Causas posibles

- El identificador de suscripción no está habilitado para crear ninguna máquina virtual en la ubicación de la región de destino.
- El identificador de suscripción no está habilitado o no tiene cuota suficiente para crear tamaños de máquina virtual específicos en la ubicación de la región de destino.
- No se encuentra ningún tamaño adecuado de máquina virtual de destino que coincida con el número de tarjetas de interfaz de red (NIC) de la máquina virtual de origen (2) para el id. de suscripción en la ubicación de la región de destino.

### <a name="fix-the-problem"></a>Corrección del problema

Para permitir que su suscripción cree máquinas virtuales de los tamaños necesarios en la ubicación de destino, póngase en contacto con el [soporte de facturación de Azure](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request). Vuelva a intentar la operación con error.

Si la ubicación de destino tiene una restricción de capacidad, deshabilite la replicación en ella. A continuación, habilite la replicación en una ubicación diferente donde su suscripción tenga cuota suficiente para crear máquinas virtuales de los tamaños necesarios.

## <a name="trusted-root-certificates-error-code-151066"></a>Certificados raíz de confianza (código de error 151066)

Si no todos los certificados raíz de confianza más recientes están presentes en la máquina virtual, el trabajo de Site Recovery de habilitar la replicación podría dar error. La autenticación y la autorización de las llamadas al servicio de Site Recovery desde la máquina virtual darán error sin estos certificados. 

Si el trabajo de habilitar la replicación da error, aparece este mensaje:

> "Error de configuración de Site Recovery".

### <a name="possible-cause"></a>Causa posible

Los certificados raíz de confianza necesarios para la autorización y la autenticación no están presentes en la máquina virtual.

### <a name="fix-the-problem"></a>Corrección del problema

#### <a name="windows"></a>Windows

En el caso de una máquina virtual que ejecuta el sistema operativo Windows, instale las actualizaciones de Windows más recientes en la máquina virtual para que todos los certificados raíz de confianza estén presentes en la máquina. Para obtener los certificados raíz más recientes y la lista actualizada de revocación de certificados en las máquinas virtuales, siga el proceso habitual de administración de actualizaciones de certificados o de administración de actualizaciones de Windows de la organización.

Si se encuentra en un entorno desconectado, siga el proceso de actualización estándar de Windows en su organización para obtener los certificados. Si los certificados necesarios no están presentes en la máquina virtual, las llamadas al servicio de Site Recovery darán error por razones de seguridad.

Para comprobar que el problema se haya resuelto, vaya a login.microsoftonline.com en su máquina virtual mediante un explorador.

Para más información, consulte [Configurar raíces de confianza y certificados no permitidos](https://technet.microsoft.com/library/dn265983.aspx).

#### <a name="linux"></a>Linux

Para obtener los certificados raíz de confianza y la lista de revocación de certificados más recientes en la máquina virtual, siga las instrucciones proporcionadas por el distribuidor de su versión del sistema operativo Linux.

Como SUSE Linux usa vínculos simbólicos (o *symlinks*) para mantener una lista de certificados, siga estos pasos:

1. Inicie sesión con un usuario raíz.

1. Ejecute este comando para cambiar el directorio:

    **# cd /etc/ssl/certs**

1. Compruebe si el certificado de entidad de certificación raíz de Symantec está presente:

    **# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem**

1. Si no se encuentra, ejecute el siguiente comando para descargar el archivo. Compruebe si hay errores y siga las acciones recomendadas para los errores de red.

    **# wget https://docs.broadcom.com/docs-and-downloads/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem**

1. Compruebe si el certificado de entidad de certificación raíz de Baltimore está presente:

    **# ls Baltimore_CyberTrust_Root.pem**

1. Si no se encuentra, ejecute este comando para descargar el certificado:

    **# wget https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem**

1. Compruebe si el certificado DigiCert_Global_Root_CA está presente:

    **# ls DigiCert_Global_Root_CA.pem**

1. Si no se encuentra, ejecute los siguientes comandos para descargarlo:

    **# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt**

    **# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem**

1. Ejecute el script rehash para actualizar los hashes del firmante del certificado para los certificados descargados recientemente:

    **# c_rehash**

1. Ejecute estos comandos para comprobar si los hashes del firmante como vínculos simbólicos se han creado para los certificados:

    - Comando:

        **# ls -l | grep Baltimore**

    - Salida:

        `lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem`

        `-rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem`

    - Comando:

        **# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5**

    - Salida:

        `-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem`

        `lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem`

    - Comando:

        **# ls -l | grep DigiCert_Global_Root**

    - Salida:

        `lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem`

        `-rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem`

1. Cree una copia del archivo VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem con el nombre de archivo b204d74a.0:

    **# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0**

1. Cree una copia del archivo Baltimore_CyberTrust_Root.pem con el nombre de archivo 653b494a.0:

    **# cp Baltimore_CyberTrust_Root.pem 653b494a.0**

1. Cree una copia del archivo DigiCert_Global_Root_CA.pem con el nombre de archivo 3513523f.0:

    **# cp DigiCert_Global_Root_CA.pem 3513523f.0**

1. Compruebe si los archivos están presentes:

    - Comando:

        **# ls -l 653b494a.0 b204d74a.0 3513523f.0**

    - Output

        `-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0`

        `-rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0`

        `-rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0`

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Conectividad saliente para direcciones URL o intervalos IP de Site Recovery (código de error 151037 o 151072)

Para que la replicación de Site Recovery funcione, la máquina virtual debe disponer de conectividad saliente a direcciones URL o intervalos IP específicos. Si la máquina virtual está detrás de un firewall o usa reglas de grupo de seguridad de red (NSG) para controlar la conectividad saliente, puede encontrarse alguno de estos problemas.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Problema 1: no se pudo registrar la máquina virtual de Azure en Site Recovery (código de error 151195)

#### <a name="possible-cause"></a>Causa posible 

No se puede establecer la conexión con los puntos de conexión de Site Recovery por un error de resolución de DNS.

Normalmente, este problema se produce al volver a establecer la protección tras una conmutación por error de la máquina virtual y no se puede acceder al servidor DNS desde la región de recuperación ante desastres (DR).

#### <a name="fix-the-problem"></a>Corrección del problema

Si utiliza un DNS personalizado, asegúrese de que el servidor DNS es accesible desde la región de recuperación ante desastres. Para averiguar si tiene un DNS personalizado, en la máquina virtual, vaya a *red de recuperación ante desastres* > **servidores DNS**.

![Lista de servidores DNS personalizados](./media/azure-to-azure-troubleshoot-errors/custom_dns.PNG)

Intente acceder al servidor DNS desde la máquina virtual. Si no puede, intente que esté accesible conmutando por error el servidor DNS o creando la línea del sitio entre la red de recuperación ante desastres y DNS.

### <a name="issue-2-site-recovery-configuration-failed-error-code-151196"></a>Problema 2: error de configuración de Site Recovery (código de error 151196)

#### <a name="possible-cause"></a>Causa posible

No se puede establecer conexión con los puntos de conexión de autenticación e identidad IP4 de Office 365.

#### <a name="fix-the-problem"></a>Corrección del problema

Site Recovery debe tener acceso a los intervalos de direcciones IP de Office 365 para la autenticación.
Si utiliza un proxy que actúe como firewall o reglas de NSG de Azure para controlar la conectividad de salida de la red en la máquina virtual, no olvide permitir la comunicación con los intervalos de direcciones IP de Office 365. Cree una regla de NSG basada en la [etiqueta de servicio de Azure Active Directory (Azure AD)](../virtual-network/security-overview.md#service-tags), que permite el acceso a todas las direcciones IP correspondientes a Azure AD. Si se agregan en el futuro nuevas direcciones a Azure AD, deberá crear nuevas reglas de NSG.

> [!NOTE]
> Si las máquinas virtuales se encuentran detrás de un equilibrador de carga interno *estándar*, este no tendrá acceso de forma predeterminada a intervalos de direcciones IP de Office 365, es decir, login.microsoftonline.com. Cambie el tipo de equilibrador de carga interno a *básico* o cree acceso de salida como se describe en el artículo [Configuración del equilibrio de carga y las reglas de salida](https://aka.ms/lboutboundrulescli).

### <a name="issue-3-site-recovery-configuration-failed-error-code-151197"></a>Problema 3: error de configuración de Site Recovery (código de error 151197)

#### <a name="possible-cause"></a>Causa posible

No se puede establecer la conexión a los puntos de conexión del servicio de Site Recovery.

#### <a name="fix-the-problem"></a>Corrección del problema

Site Recovery necesita tener acceso a [intervalos de direcciones IP de Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges), en función de la región. Asegúrese de que los intervalos de direcciones IP necesarios están accesibles desde la máquina virtual.

### <a name="issue-4-azure-to-azure-replication-failed-when-the-network-traffic-goes-through-an-on-premises-proxy-server-error-code-151072"></a>Problema 4: error de replicación de Azure a Azure cuando el tráfico de red pasa por el servidor proxy local (código de error 151072)

#### <a name="possible-cause"></a>Causa posible

La configuración de proxy personalizada no es válida y el agente de Mobility Service de Site Recovery no detectó automáticamente la configuración de proxy de Internet Explorer.

#### <a name="fix-the-problem"></a>Corrección del problema

El agente de Mobility Service detecta la configuración de proxy de Internet Explorer en Windows y /etc/environment en Linux.

Si prefiere configurar el servidor proxy solo para Mobility Service, puede proporcionar la información en el archivo ProxyInfo.conf de estas ubicaciones:

- **Linux**: /usr/local/InMage/config/
- **Windows**: C:\ProgramData\Microsoft Azure Site Recovery\Config

En ProxyInfo.conf, proporcione la configuración de proxy en el siguiente formato de archivo de inicialización:

> [*proxy*]

> Address= *http://1.2.3.4*

> Port=*567*


> [!NOTE]
> El agente de Mobility Service de Site Recovery solo admite *servidores proxy no autenticados*.

### <a name="more-information"></a>Más información

Para especificar las [direcciones URL requeridas](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) o los [intervalos de IP requeridos](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), siga las instrucciones en [Acerca de las redes en Azure para la replicación de Azure](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Disco no encontrado en la máquina (código de error 150039)

Se debe inicializar un nuevo disco asociado a la máquina virtual. Si no se encuentra el disco, aparece este mensaje:

> "El disco de datos *DiskName* *DiskURI* con el número de unidad lógica *LUN* *LUNValue* no se ha asignado al disco correspondiente de la máquina virtual con el mismo valor de LUN.

### <a name="possible-causes"></a>Causas posibles

- Un nuevo disco de datos se asoció a la máquina virtual, pero no se ha inicializado.
- El disco de datos dentro de la máquina virtual no está notificando correctamente el valor de número de unidad lógica (LUN) con el que el disco se asoció a la máquina virtual.

### <a name="fix-the-problem"></a>Corrección del problema

Asegúrese de que se inicializan los discos de datos y, a continuación, vuelva a intentar la operación.

- **Windows**: [adjunte e inicialice un disco nuevo](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).

- **Linux**: [inicialice un nuevo disco de datos en Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Si el problema persiste, póngase en contacto con el soporte técnico.

## <a name="one-or-more-disks-are-available-for-protection-error-code-153039"></a>Hay uno o más discos disponibles para protección (código de error 153039)

### <a name="possible-causes"></a>Causas posibles

- Uno o más discos se han agregado recientemente a la máquina virtual después de la protección.
- Uno o más discos se han inicializado después de la protección de la máquina virtual.

### <a name="fix-the-problem"></a>Corrección del problema

Para que el estado de replicación de la máquina virtual sea correcto de nuevo, puede optar por proteger los discos o pasar por alto la advertencia.

#### <a name="to-protect-the-disks"></a>Para proteger los discos

1. Vaya a **Elementos replicados** > *Nombre de la máquina virtual* > **Discos**.
1. Seleccione el disco no protegido y, a continuación, **Habilitar replicación**:

    ![Habilitación de la replicación en discos de VM](./media/azure-to-azure-troubleshoot-errors/add-disk.png)

#### <a name="to-dismiss-the-warning"></a>Para descartar la advertencia

1. Vaya a **Elementos replicados** > *Nombre de la máquina virtual*.
1. Seleccione la advertencia en la sección **Información general** y, a continuación, seleccione **Aceptar**.

    ![Descartar advertencia de nuevo disco](./media/azure-to-azure-troubleshoot-errors/dismiss-warning.png)

## <a name="remove-the-virtual-machine-from-the-vault-completed-with-information-error-code-150225"></a>Retirada de la máquina virtual del almacén que se completó con información (código de error 150225)

Al proteger la máquina virtual, Site Recovery crea algunos vínculos en la máquina virtual de origen. Cuando se quite la protección o se deshabilite la replicación, Site Recovery quita estos vínculos como parte del trabajo de limpieza. Si la máquina virtual tiene un bloqueo de recursos, el trabajo de limpieza se completa con la información. La información indica que la máquina virtual se ha eliminado del almacén de Recovery Services, pero que algunos de los vínculos obsoletos no se pudieron limpiar en la máquina de origen.

Puede omitir esta advertencia si no pretende proteger esta máquina virtual de nuevo. Sin embargo, en caso de tener que proteger esta máquina virtual posteriormente, siga los pasos en "Corrección del problema" para limpiar los vínculos.

> [!WARNING]
> Si no hace la limpieza:
>
> - Al habilitar la replicación por medio del almacén de Recovery Services, no se mostrará la máquina virtual.
> - Si intenta proteger la máquina virtual mediante **Máquina virtual** > **Configuración** > **Recuperación ante desastres**, se producirá un error en la operación con el mensaje "No se puede habilitar la replicación debido a los vínculos de recursos obsoletos existentes en la VM".

### <a name="fix-the-problem"></a>Corrección del problema

> [!NOTE]
> Site Recovery no elimina la máquina virtual de origen ni la afecta de ninguna manera mientras realiza estos pasos.

1. Quite el bloqueo de la máquina virtual o del grupo de recursos de la máquina virtual. Por ejemplo, en la siguiente imagen, debe eliminarse el bloqueo de recursos de la máquina virtual llamada "MoveDemo":

    ![Eliminación del bloqueo de la máquina virtual](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)

1. Descargue el script para [quitar una configuración obsoleta de Site Recovery](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1).
1. Ejecute el script, que se llama Cleanup-stale-asr-config-Azure-VM.ps1. Proporcione el identificador de la suscripción, el grupo de recursos de la máquina virtual y el nombre de la máquina virtual como parámetros.
1. Si se le piden las credenciales de Azure, proporciónelas. A continuación, compruebe que el script se ejecuta sin errores.

## <a name="replication-cant-be-enabled-because-of-stale-resource-links-on-the-vm-error-code-150226"></a>La replicación no se puede habilitar debido a los vínculos de recursos obsoletos de la máquina virtual (código de error 150226)

### <a name="possible-cause"></a>Causa posible

La máquina virtual tiene una configuración obsoleta de la protección anterior de Site Recovery.

Una configuración obsoleta puede producirse en una máquina virtual de Azure si habilitó la replicación para la máquina virtual de Azure mediante Site Recovery y, a continuación:

- Deshabilitó la replicación, pero la máquina virtual de origen tenía un bloqueo de recursos.
- Eliminó el almacén de Site Recovery sin deshabilitar de forma explícita la replicación en la máquina virtual.
- Eliminó el grupo de recursos que contiene el almacén de Site Recovery sin deshabilitar de forma explícita la replicación en la máquina virtual.

### <a name="fix-the-problem"></a>Corrección del problema

> [!NOTE]
> Site Recovery no elimina la máquina virtual de origen ni la afecta de ninguna manera mientras realiza estos pasos.

1. Quite el bloqueo de la máquina virtual o del grupo de recursos de la máquina virtual. Por ejemplo, en la siguiente imagen, debe eliminarse el bloqueo de recursos de la máquina virtual llamada "MoveDemo":

    ![Eliminación del bloqueo de la máquina virtual](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)

1. Descargue el script para [quitar una configuración obsoleta de Site Recovery](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1).
1. Ejecute el script, que se llama Cleanup-stale-asr-config-Azure-VM.ps1. Proporcione el identificador de la suscripción, el grupo de recursos de la máquina virtual y el nombre de la máquina virtual como parámetros.
1. Si se le piden las credenciales de Azure, proporciónelas. A continuación, compruebe que el script se ejecuta sin errores.

## <a name="unable-to-see-the-azure-vm-or-resource-group-for-the-selection-in-the-enable-replication-job"></a>No se puede ver la máquina virtual o el grupo de recursos de Azure para la selección en el trabajo "Habilitar replicación"

### <a name="cause-1-the-resource-group-and-source-virtual-machine-are-in-different-locations"></a>Causa 1: el grupo de recursos y la máquina virtual de origen están en ubicaciones distintas

Actualmente, Site Recovery requiere que el grupo de recursos y las máquinas virtuales de la región de origen estén en la misma ubicación. Si no es así, no podrá encontrar la máquina virtual o el grupo de recursos cuando intente aplicar la protección.

Como solución alternativa, puede habilitar la replicación desde la máquina virtual en lugar del almacén de Recovery Services. Vaya a **Máquina virtual de origen** > **Propiedades** > **Recuperación ante desastres** y habilite la replicación.

### <a name="cause-2-the-resource-group-is-not-part-of-the-selected-subscription"></a>Causa 2: el grupo de recursos no forma parte de la suscripción seleccionada

Es posible que no pueda encontrar el grupo de recursos en el momento de la protección si no forma parte de la suscripción seleccionada. Asegúrese de que el grupo de recursos pertenece a la suscripción que usa.

### <a name="cause-3-stale-configuration"></a>Causa 3: configuración obsoleta

Es posible que no vea la máquina virtual que quiere habilitar para la replicación si se ha dejado una configuración obsoleta de Site Recovery en la máquina virtual de Azure. Esta condición podría producirse si habilitó la replicación para la máquina virtual de Azure mediante Site Recovery y, a continuación:

- Eliminó el almacén de Site Recovery sin deshabilitar de forma explícita la replicación en la máquina virtual.
- Eliminó el grupo de recursos que contiene el almacén de Site Recovery sin deshabilitar de forma explícita la replicación en la máquina virtual.
- Deshabilitó la replicación, pero la máquina virtual de origen tenía un bloqueo de recursos.

### <a name="fix-the-problem"></a>Corrección del problema

> [!NOTE]
> Asegúrese de actualizar el módulo "AzureRM.Resources" antes de usar el script mencionado en esta sección.  Site Recovery no elimina la máquina virtual de origen ni la afecta de ninguna manera mientras realiza estos pasos.

1. Quite el bloqueo, en caso de haberlo, de la máquina virtual o del grupo de recursos de la máquina virtual. Por ejemplo, en la siguiente imagen, debe eliminarse el bloqueo de recursos de la máquina virtual llamada "MoveDemo":

    ![Eliminación del bloqueo de la máquina virtual](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)

1. Descargue el script para [quitar una configuración obsoleta de Site Recovery](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1).
1. Ejecute el script, que se llama Cleanup-stale-asr-config-Azure-VM.ps1. Proporcione el identificador de la suscripción, el grupo de recursos de la máquina virtual y el nombre de la máquina virtual como parámetros.
1. Si se le piden las credenciales de Azure, proporciónelas. A continuación, compruebe que el script se ejecuta sin errores.

## <a name="unable-to-select-a-virtual-machine-for-protection"></a>No se puede seleccionar una máquina virtual para la protección

### <a name="cause-1-the-virtual-machine-has-an-extension-installed-in-a-failed-or-unresponsive-state"></a>Causa 1: la máquina virtual tiene una extensión instalada en estado Con errores o No responde

Vaya a **Máquinas virtuales** > **Configuración** > **Extensiones** y compruebe si hay alguna extensión en un estado con errores. Desinstale cualquier extensión con errores y, a continuación, vuelva a intentar proteger la máquina virtual.

### <a name="cause-2-the-vms-provisioning-state-is-not-valid"></a>Causa 2: el estado de aprovisionamiento de la máquina virtual no es válido

Consulte los pasos de solución de problemas en [El estado de aprovisionamiento de la máquina virtual no es válido](#the-vms-provisioning-state-is-not-valid-error-code-150019), más adelante en este artículo.

## <a name="the-vms-provisioning-state-is-not-valid-error-code-150019"></a>El estado de aprovisionamiento de la máquina virtual no es válido (código de error 150019)

Para habilitar la replicación en la máquina virtual, su estado de aprovisionamiento debe ser **Correcto**. Siga estos pasos para comprobar el estado de aprovisionamiento:

1. En Azure Portal, seleccione el **Explorador de recursos** de **Todos los servicios**.
1. Expanda la lista **Suscripciones** y seleccione su suscripción.
1. Expanda la lista **ResourceGroups** y seleccione el grupo de recursos de la máquina virtual.
1. Expanda la lista **Recursos** y seleccione la máquina virtual.
1. Compruebe el campo **provisioningState** en la vista Instancia del lado derecho.

### <a name="fix-the-problem"></a>Corrección del problema

- Si **provisioningState** es **Error**, póngase en contacto con soporte técnico con detalles para solucionar el problema.
- Si **provisioningState** es **Actualizando**, puede que se esté implementando otra extensión. Compruebe si hay operaciones en curso en la máquina virtual, espere a que finalicen y reintente el trabajo "Habilitar replicación" de Site Recovery que presenta el error.

## <a name="unable-to-select-target-vm-network-selection-tab-is-unavailable"></a>No se puede seleccionar la red virtual de destino (la pestaña Selección de red no está disponible)

### <a name="cause-1-your-vm-is-attached-to-a-network-thats-already-mapped-to-a-target-network"></a>Causa 1: la máquina virtual está conectada a una red que ya está asignada a una red de destino

Si la máquina virtual de origen forma parte de una red virtual y otra máquina virtual de la misma red virtual ya está asignada a una red del grupo de recursos de destino, el cuadro de lista desplegable de selección de red no estará disponible (aparecerá atenuado) de forma predeterminada.

![Lista de selección de red no disponible](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

### <a name="cause-2-you-previously-protected-the-vm-by-using-site-recovery-and-then-you-disabled-the-replication"></a>Causa 2: ha protegido previamente la máquina virtual mediante Site Recovery y, a continuación, ha deshabilitado la replicación

Deshabilitar la replicación de una máquina virtual no elimina la asignación de red. La asignación debe eliminarse del almacén de Recovery Services donde la máquina virtual está protegida. Vaya a *Almacén de Recovery Services* > **Infraestructura de Site Recovery** > **Asignación de red**.

![Eliminación de la asignación de red](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)

La red de destino que se configuró durante la instalación de la recuperación ante desastres puede cambiarse después, una vez protegida la máquina virtual:

![Modificación de la asignación de red](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)

Tenga en cuenta que cambiar la asignación de red afecta a todas las máquinas virtuales protegidas que usen esa misma asignación de red.

## <a name="com-or-volume-shadow-copy-service-error-error-code-151025"></a>Error del servicio de instantáneas de volumen o COM+ (código de error 151025)

Al producirse este error, aparece este mensaje:

> "No se pudo instalar la extensión de Site Recovery"

### <a name="possible-causes"></a>Causas posibles

- El servicio de aplicación del sistema COM+ está deshabilitado.
- El servicio de instantáneas de volumen está deshabilitado.

### <a name="fix-the-problem"></a>Corrección del problema

Establezca los servicios de aplicación del sistema COM+ y de instantáneas de volumen en modo de inicio automático o manual.

1. Abra la consola de Servicios en Windows.
1. Asegúrese de que los servicios de aplicación del sistema COM+ y de instantáneas de volumen no se establecen en **Deshabilitado** como su **Tipo de inicio**.

    ![Comprobación de tipo de inicio de los servicios de aplicación del sistema COM+ y de instantáneas de volumen](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="unsupported-managed-disk-size-error-code-150172"></a>Tamaño del disco administrado no compatible (código de error 150172)

Al producirse este error, aparece este mensaje:

> "No se pudo habilitar la protección para la máquina virtual, ya que tiene un disco (*DiskName*) con un tamaño (*DiskSize*)* que es menor que el tamaño mínimo admitido de 1024 MB".

### <a name="possible-cause"></a>Causa posible

El disco es menor que el tamaño admitido de 1024 MB.

### <a name="fix-the-problem"></a>Corrección del problema

Asegúrese de que el tamaño de disco está dentro del intervalo de tamaño admitido y, a continuación, reintente la operación.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-error-code-151126"></a>No se habilitó la protección porque la configuración de GRUB incluye el nombre de dispositivo en lugar del UUID (código de error 151126)

### <a name="possible-cause"></a>Causa posible

Los archivos de configuración de GRUB de Linux (/boot/grub/menu.lst", /boot/grub/grub.cfg, /boot/grub2/grub.cfg o /etc/default/grub) pueden especificar los nombres de dispositivo reales en lugar de los valores UUID de los parámetros *root* y *resume*. Site Recovery requiere UUID porque los nombres de dispositivo pueden cambiar. Tras el reinicio, podría aparecer una máquina virtual con otro nombre en la conmutación por error, lo que genera problemas.

Los siguientes ejemplos son líneas de archivos de GRUB en las que aparecen nombres de dispositivo (mostrados en negrita) en lugar de los UUID necesarios:

- Archivo /boot/grub2/grub.cfg

  > linux   /boot/vmlinuz-3.12.49-11-default **root=/dev/sda2**  ${extra_cmdline} **resume=/dev/sda1** splash=silent quiet showopts

- Archivo: /boot/grub/menu.lst

  > kernel /boot/vmlinuz-3.0.101-63-default **root=/dev/sda2** **resume=/dev/sda1** splash=silent crashkernel=256M-:128M showopts vga=0x314


### <a name="fix-the-problem"></a>Corrección del problema

Reemplace cada nombre de dispositivo por el UUID correspondiente:

1. Para buscar el UUID del dispositivo, ejecute el comando **blkid** ***nombre de dispositivo***. Por ejemplo:

    ```
    blkid /dev/sda1
    /dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap"
    blkid /dev/sda2
    /dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```

1. Reemplace el nombre de dispositivo por su UUID, en los formatos **root=UUID**=*UUID* y **resume=UUID**=*UUID*. Por ejemplo, después del reemplazo, la línea de /boot/grub/menu.lst (analizada anteriormente) tendría el siguiente aspecto:

    > kernel /boot/vmlinuz-3.0.101-63-default **root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4** **resume=UUID=6f614b44-433b-431b-9ca1-4dd2f6f74f6b** splash=silent crashkernel=256M-:128M showopts vga=0x314

1. Reintente la protección.

## <a name="enable-protection-failed-because-the-device-mentioned-in-the-grub-configuration-doesnt-exist-error-code-151124"></a>No se pudo habilitar la protección debido a que el dispositivo que se menciona en la configuración de GRUB no existe (código de error 151124)

### <a name="possible-cause"></a>Causa posible

Los archivos de configuración de GRUB (/boot/grub/menu.lst, /boot/grub/grub.cfg, /boot/grub2/grub.cfg o /etc/default/grub) pueden contener los parámetros *rd.lvm.lv* o *rd_LVM_LV*. Estos parámetros identifican los dispositivos de administrador de volúmenes lógicos (LVM) que se van a detectar en tiempo de arranque. Si estos dispositivos LVM no existen, el sistema protegido no se iniciará y se atascará en el proceso de arranque. También se verá el mismo problema con la máquina virtual de conmutación por error. Estos son algunos ejemplos:

- Archivo: /boot/grub2/grub.cfg on RHEL7:

    > linux16 /vmlinuz-3.10.0-957.el7.x86_64 root=/dev/mapper/rhel_mup--rhel7u6-root ro crashkernel=128M\@64M **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet LANG=en_US.UTF-8

- Archivo: /etc/default/grub on RHEL7:

    > GRUB_CMDLINE_LINUX="crashkernel=auto **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet"

- Archivo: /boot/grub/menu.lst on RHEL6:

    > kernel /vmlinuz-2.6.32-754.el6.x86_64 ro root=UUID=36dd8b45-e90d-40d6-81ac-ad0d0725d69e rd_NO_LUKS LANG=en_US.UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=auto **rd_LVM_LV=rootvg/lv_root**  KEYBOARDTYPE=pc KEYTABLE=us **rd_LVM_LV=rootvg/lv_swap** rd_NO_DM rhgb quiet

En cada ejemplo, la parte en negrita muestra que GRUB tiene que detectar dos dispositivos LVM con los nombres "root" y "swap" del grupo de volúmenes "rootvg".

### <a name="fix-the-problem"></a>Corrección del problema

Si el dispositivo LVM no existe, créelo o quite los parámetros correspondientes de los archivos de configuración de GRUB. A continuación, vuelva a intentar habilitar la protección.

## <a name="a-site-recovery-mobility-service-update-finished-with-warnings-error-code-151083"></a>Una actualización del servicio de movilidad de Site Recovery finalizada con advertencias (código de error 151083)

El servicio de movilidad de Site Recovery tiene muchos componentes, uno de los cuales es el controlador del filtro. El controlador del filtro solamente se carga en la memoria del sistema durante el reinicio del sistema. Siempre que una actualización de Mobility Service incluye cambios en el controlador del filtro, la máquina se actualiza, pero sigue mostrándose una advertencia de que algunas correcciones requieren un reinicio. La advertencia aparece porque las correcciones del controlador del filtro pueden surtir efecto solo al cargarse el nuevo controlador del filtro, lo que ocurre únicamente durante un reinicio.

> [!NOTE]
> Esto es solo una advertencia. La replicación existente sigue funcionando incluso después de la nueva actualización del agente. Puede reiniciar el sistema siempre que desee las ventajas del nuevo controlador del filtro, pero, si no reinicia, el antiguo controlador seguirá funcionando.
>
> Además del controlador del filtro, las ventajas de cualquier otra mejora y corrección de la actualización de Mobility Service surten efecto sin requerir un reinicio.  

## <a name="protection-couldnt-be-enabled-because-the-replica-managed-disk-already-exists-without-expected-tags-in-the-target-resource-group-error-code-150161"></a>No se pudo habilitar la protección porque el disco administrado de la réplica ya existe, sin las etiquetas esperadas, en el grupo de recursos de destino (código de error 150161)

### <a name="possible-cause"></a>Causa posible

Este problema puede producirse si la máquina virtual se protegió previamente y, cuando se deshabilitó la replicación, no se limpió el disco de la réplica.

### <a name="fix-the-problem"></a>Corrección del problema

Elimine el disco de la réplica identificado en el mensaje de error y reintente el trabajo de protección con error.

## <a name="next-steps"></a>Pasos siguientes

[Replicación de máquinas virtuales de Azure](site-recovery-replicate-azure-to-azure.md)
