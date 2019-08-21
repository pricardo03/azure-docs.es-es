---
title: Solución de problemas y errores de la replicación de Azure a Azure en Azure Site Recovery| Microsoft Docs
description: Solución de problemas y errores al replicar máquinas virtuales de Azure para la recuperación ante desastres
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: asgang
ms.openlocfilehash: 4d8ba44cdd5161a1a5ff108837cb57af4cd98835
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69034800"
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Solución de problemas de replicación de máquinas virtuales de Azure a Azure

En este artículo se describen los problemas comunes de Azure Site Recovery al replicar y recuperar máquinas virtuales de Azure de una región a otra y se explica cómo solucionarlos. Para más información sobre las configuraciones admitidas, consulte la [matriz de compatibilidad para la replicación de máquinas virtuales de Azure](site-recovery-support-matrix-azure-to-azure.md).

## <a name="list-of-errors"></a>Lista de errores
- **[Problemas de cuota de recursos de Azure (código de error 150097)](#azure-resource-quota-issues-error-code-150097)**
- **[Certificados raíz de confianza (código de error 151066)](#trusted-root-certificates-error-code-151066)**
- **[Conectividad saliente para Site Recovery (código de error 151195)](#issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br)**

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problemas de cuota de recursos de Azure (código de error 150097)
La suscripción debe estar habilitada para crear máquinas virtuales de Azure en la región de destino que se va a utilizar como región de recuperación ante desastres. Además, la suscripción debe tener habilitada suficiente cuota para crear máquinas virtuales de un tamaño específico. De forma predeterminada, Site Recovery adopta el mismo tamaño en la máquina virtual de destino que en la de origen. Si el tamaño coincidente no está disponible, se selecciona automáticamente el tamaño más aproximado posible. Si no hay ningún tamaño coincidente que admita la configuración de la máquina virtual de origen, aparece este mensaje de error:

**Código de error** | **Causas posibles:** | **Recomendación**
--- | --- | ---
150097<br></br>**Mensaje**: No se pudo habilitar la replicación para la máquina virtual VmName. | -El identificador de suscripción no puede habilitarse para crear ninguna máquina virtual en la ubicación de la región de destino.</br></br>-El identificador de suscripción no puede habilitarse o no tiene cuota suficiente para crear tamaños de máquina virtual específicos en la ubicación de la región de destino.</br></br>-No se encuentra un tamaño adecuado de máquina virtual de destino que coincida con el número de tarjetas NIC de la máquina virtual de origen (2) para el id. de suscripción en la ubicación de la región de destino.| Para habilitar la creación de máquinas virtuales con el tamaño adecuado en la ubicación de destino de la suscripción, póngase en contacto con el [soporte de facturación de Azure](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request). Una vez habilitada, vuelva a intentar la operación con error.

### <a name="fix-the-problem"></a>Corrección del problema
Para permitir que su suscripción cree máquinas virtuales de los tamaños necesarios en la ubicación de destino, puede ponerse en contacto con el [soporte de facturación de Azure](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request).

Si la ubicación de destino tiene restricciones de capacidad, deshabilite la replicación y habilítela en una ubicación diferente donde su suscripción tenga cuota suficiente para crear máquinas virtuales con los tamaños necesarios.

## <a name="trusted-root-certificates-error-code-151066"></a>Certificados raíz de confianza (código de error 151066)

Si los certificados raíz de confianza más recientes no están presentes en la máquina virtual, el trabajo de habilitar la replicación podría dar error. Sin los certificados, la autenticación y la autorización de las llamadas al servicio de Site Recovery desde la máquina virtual darán error. Aparece el mensaje de error al habilitar la replicación para el trabajo de Site Recovery:

**Código de error** | **Causa posible** | **Recomendaciones**
--- | --- | ---
151066<br></br>**Mensaje**: Error de configuración de Site Recovery. | Los certificados raíz de confianza necesarios usados para la autorización y la autenticación no están presentes en la máquina. | -En máquinas virtuales que ejecutan el sistema operativo Windows, asegúrese de que los certificados raíz de confianza estén presentes en la máquina. Para más información, consulte [Configurar raíces de confianza y certificados no permitidos](https://technet.microsoft.com/library/dn265983.aspx).<br></br>-En máquinas virtuales que ejecutan el sistema operativo Linux, siga las instrucciones relativas a los certificados raíz de confianza publicados por el distribuidor de versiones de dicho sistema operativo.

### <a name="fix-the-problem"></a>Corrección del problema
**Windows**

Instale las actualizaciones de Windows más recientes en la máquina virtual para que todos los certificados raíz de confianza están presentes en la máquina. Si se encuentra en un entorno desconectado, siga el proceso de actualización estándar de Windows en su organización para obtener los certificados. Si los certificados necesarios no están presentes en la máquina virtual, las llamadas al servicio de Site Recovery darán error por razones de seguridad.

Para obtener los certificados raíz más recientes y la lista actualizada de revocación de certificados en las máquinas virtuales, siga el proceso habitual de administración de actualizaciones de certificados o de administración de actualizaciones de Windows.

Para comprobar que el problema se haya resuelto, vaya a login.microsoftonline.com en su máquina virtual mediante un explorador.

**Linux**

Para obtener los certificados raíz de confianza y la lista de revocación de certificados más recientes en la máquina virtual, siga las instrucciones proporcionadas por su distribuidor de Linux.

Como SuSE Linux usa vínculos simbólicos para mantener una lista de certificados, siga estos pasos:

1.  Inicie sesión con un usuario raíz.

2.  Ejecute este comando para cambiar el directorio.

      ``# cd /etc/ssl/certs``

1. Compruebe si está presente el certificado raíz de CA de Symantec.

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

2. Si no se encuentra, ejecute el siguiente comando para descargar el archivo. Compruebe si hay errores y siga la acción recomendada para los errores de red.

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

3. Compruebe si está presente el certificado raíz de CA de Baltimore.

      ``# ls Baltimore_CyberTrust_Root.pem``

4. Si no se encuentra, descárguelo.  

    ``# wget https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem -O Baltimore_CyberTrust_Root.pem``

5. Compruebe si está presente el certificado DigiCert_Global_Root_CA.

    ``# ls DigiCert_Global_Root_CA.pem``

6. Si no se encuentra, ejecute los siguientes comandos para descargarlo.

    ``# wget http://www.digicert.com/CACerts/DigiCertGlobalRootCA.crt``

    ``# openssl x509 -in DigiCertGlobalRootCA.crt -inform der -outform pem -out DigiCert_Global_Root_CA.pem``

7. Ejecute el script rehash para actualizar los hashes del firmante del certificado para los certificados descargados recientemente.

    ``# c_rehash``

8.  Compruebe si los hashes del firmante, como los symlink, se crean para los certificados.

    - Get-Help

      ``# ls -l | grep Baltimore``

    - Output

      ``lrwxrwxrwx 1 root root   29 Jan  8 09:48 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      -rw-r--r-- 1 root root 1303 Jun  5  2014 Baltimore_CyberTrust_Root.pem``

    - Get-Help

      ``# ls -l | grep VeriSign_Class_3_Public_Primary_Certification_Authority_G5``

    - Output

      ``-rw-r--r-- 1 root root 1774 Jun  5  2014 VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem
      lrwxrwxrwx 1 root root   62 Jan  8 09:48 facacbc6.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

    - Get-Help

      ``# ls -l | grep DigiCert_Global_Root``

    - Output

      ``lrwxrwxrwx 1 root root   27 Jan  8 09:48 399e7759.0 -> DigiCert_Global_Root_CA.pem
      -rw-r--r-- 1 root root 1380 Jun  5  2014 DigiCert_Global_Root_CA.pem``

9.  Creación de una copia del archivo VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem con el nombre de archivo b204d74a.0

    ``# cp VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

10. Creación de una copia del archivo Baltimore_CyberTrust_Root.pem con el nombre de archivo 653b494a.0

    ``# cp Baltimore_CyberTrust_Root.pem 653b494a.0``

13. Creación de una copia del archivo DigiCert_Global_Root_CA.pem con el nombre de archivo 3513523f.0

    ``# cp DigiCert_Global_Root_CA.pem 3513523f.0``  


14. Compruebe si los archivos están presentes.  

    - Get-Help

      ``# ls -l 653b494a.0 b204d74a.0 3513523f.0``

    - Output

      ``-rw-r--r-- 1 root root 1774 Jan  8 09:52 3513523f.0
      -rw-r--r-- 1 root root 1303 Jan  8 09:52 653b494a.0
      -rw-r--r-- 1 root root 1774 Jan  8 09:52 b204d74a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Conectividad saliente para direcciones URL o intervalos IP de Site Recovery (código de error 151037 o 151072)

Para que la replicación de Site Recovery funcione, la máquina virtual debe disponer de conectividad saliente a direcciones URL o intervalos IP específicos. Si la máquina virtual está detrás de un firewall o usa reglas de grupo de seguridad de red (NSG) para controlar la conectividad saliente, puede encontrarse alguno de estos problemas.

### <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Problema 1: no se pudo registrar la máquina virtual de Azure en Site Recovery (151195) </br>
- **Causa posible** </br>
  - No se puede establecer la conexión con los puntos de conexión de Site Recovery por un error de resolución de DNS.
  - Normalmente, este problema se produce al volver a establecer la protección tras una conmutación por error de la máquina virtual y no se puede acceder al servidor DNS desde la región de recuperación ante desastres (DR).

- **Resolución**
   - Si utiliza un DNS personalizado, asegúrese de que el servidor DNS es accesible desde la región de recuperación ante desastres. Para comprobar si el DNS está personalizado, vaya a la máquina virtual > red de recuperación ante desastres > servidores DNS. Intente acceder al servidor DNS desde la máquina virtual. Si no puede, intente que esté accesible conmutando por error el servidor DNS o creando la línea del sitio entre la red de recuperación ante desastres y DNS.

    ![com-error](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)


### <a name="issue-2-site-recovery-configuration-failed-151196"></a>Problema 2: error de configuración de Site Recovery (151196)
- **Causa posible** </br>
  - No se puede establecer conexión con los puntos de conexión de autenticación e identidad IP4 de Office 365.

- **Resolución**
  - Azure Site Recovery debe tener acceso a los intervalos de direcciones IP de Office 365 para la autenticación.
    Si utiliza un proxy que actúe como firewall o reglas de grupos de seguridad de red (NSG) de Azure para controlar la conectividad de salida de la red en la máquina virtual, no olvide permitir la comunicación con los intervalos de IP de O365. Cree una regla de grupos de seguridad de red basada en la [etiqueta de servicio de Azure Active Directory (AAD)](../virtual-network/security-overview.md#service-tags) para permitir el acceso a todas las direcciones IP correspondientes a AAD.
      - Si en el futuro se agregan nuevas direcciones a Azure Active Directory (AAD), tendrá que crear nuevas reglas de grupos de seguridad de red.

> [!NOTE]
> Si las máquinas virtuales se encuentran detrás de un equilibrador de carga interno **estándar**, no tendría acceso a direcciones IP de O365, es decir, login.microsoftonline.com de manera predeterminada. Cambie a un tipo de equilibrador de carga interno **básico** o cree acceso de salida como se ha mencionado en el [artículo](https://aka.ms/lboutboundrulescli).

### <a name="issue-3-site-recovery-configuration-failed-151197"></a>Problema 3: error de configuración de Site Recovery (151197)
- **Causa posible** </br>
  - No se puede establecer conexión con los puntos de conexión del servicio Azure Site Recovery.

- **Resolución**
  - Azure Site Recovery necesita tener acceso a [intervalos de IP de Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) en función de la región. Asegúrese de que los intervalos de IP están accesibles desde la máquina virtual.


### <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Problema 4: error de replicación A2A cuando el tráfico de red pasa por el servidor proxy local (151072)
- **Causa posible** </br>
  - La configuración de proxy personalizada no es válida y el agente de Mobility Service de Azure Site Recovery no detectó automáticamente la configuración de proxy de Internet Explorer.


- **Resolución**
  1. El agente de Mobility Service detecta la configuración de proxy de Internet Explorer en Windows y /etc/environment en Linux.
  2. Si prefiere configurar el proxy solo para Mobility Service de Azure Site Recovery, puede proporcionar los detalles de este en ProxyInfo.conf en:</br>
     - ``/usr/local/InMage/config/`` en ***Linux***
     - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` en ***Windows***
  3. ProxyInfo.conf debe tener la configuración de proxy en el siguiente formato INI.</br>
                *[proxy]*</br>
                *Address=http://1.2.3.4*</br>
                *Port=567*</br>
  4. El agente de Mobility Service de Azure Site Recovery solo admite ***servidores proxy no autenticados***.


### <a name="fix-the-problem"></a>Corrección del problema
Para permitir [las direcciones URL necesarias](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) o los [intervalos IP necesarios](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges), siga los pasos del [documento de instrucciones para redes](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-the-machine-error-code-150039"></a>Disco no encontrado en la máquina (código de error 150039)

Se debe inicializar un nuevo disco asociado a la máquina virtual.

**Código de error** | **Causas posibles:** | **Recomendaciones**
--- | --- | ---
150039<br></br>**Mensaje**: El disco de datos (DiskName) (DiskUri) con el número de unidad lógica (LUN) (LUNValue) no se ha asignado a un disco correspondiente que se notifica desde la máquina virtual como que tiene el mismo valor de LUN. | -Un nuevo disco de datos se asoció a la máquina virtual, pero no se ha inicializado.</br></br>-El disco de datos dentro de la máquina virtual no está notificando correctamente el valor LUN con el que el disco se asoció a la máquina virtual.| Asegúrese de que se inicializan los discos de datos y, a continuación, vuelva a intentar la operación.</br></br>Para Windows: [adjunte e inicialice un disco nuevo](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).</br></br>Para Linux: [inicialice un nuevo disco de datos en Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

### <a name="fix-the-problem"></a>Corrección del problema
Asegúrese de que se hayan inicializado los discos de datos y, a continuación, vuelva a intentar la operación:

- Para Windows: [adjunte e inicialice un disco nuevo](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal).
- En Linux: [Agregue un nuevo disco de datos en Linux](https://docs.microsoft.com/azure/virtual-machines/linux/add-disk).

Si el problema persiste, póngase en contacto con el soporte técnico.

## <a name="one-or-more-disks-are-available-for-protectionerror-code-153039"></a>Hay uno o más discos disponibles para protección (código de error 153039)
- **Causa posible** </br>
  - si uno o más de discos se han agregado recientemente a la máquina virtual después de la protección. 
  - si uno o más discos se han inicializado después de la protección de la máquina virtual.

### <a name="fix-the-problem"></a>Corrección del problema
Puede optar por proteger los discos o pasar por alto la advertencia para que el estado de replicación de la máquina virtual sea correcto de nuevo.</br>
1. Para proteger los discos. Vaya a Elementos replicados > Máquina virtual > Discos > haga clic en disco no protegido > Habilitar replicación.
 ![add_disks](./media/azure-to-azure-troubleshoot-errors/add-disk.png)
2. Para descartar la advertencia. Vaya a Elementos replicados > Máquina virtual > Haga clic en la alerta de descarte de la sección de información general.
![dismiss_warning](./media/azure-to-azure-troubleshoot-errors/dismiss-warning.png)


## <a name="remove-the-virtual-machine-from-the-vault-completed-with-information--error-code-150225"></a>Retirada de la máquina virtual del almacén que se completó con información (código de error 150225)
A la hora de proteger la máquina virtual, Azure Site Recovery crea algunos vínculos en la máquina virtual de origen. Cuando se quite la protección o se deshabilite la replicación, Azure Site Recovery quita estos vínculos como parte del trabajo de limpieza. En caso de que la máquina virtual tenga un bloqueo de recursos, el trabajo se completa con la información. Indica que la máquina virtual se ha eliminado del almacén de los servicios de recuperación, pero algunos de los vínculos obsoletos no se pudieron limpiar de la máquina de origen.

Puede omitir esta advertencia si no pretende proteger esta máquina virtual de nuevo en el futuro. Sin embargo, si tiene que proteger esta máquina virtual más tarde, debe limpiar los vínculos como se menciona en los pasos siguientes. 

**Si no hace la limpieza, entonces:**

1.  Durante el tiempo de habilitación de la replicación a través del almacén de los servicios de recuperación, la máquina virtual no aparecerá en la lista. 
2.  Si intenta proteger la máquina virtual mediante **Máquina virtual > Configuración > Recuperación ante desastres**, se producirá el error "*No se puede habilitar la replicación debido a los vínculos de recursos obsoletos existentes en la VM*".


### <a name="fix-the-problem"></a>Corrección del problema

>[!NOTE]
>
>Azure Site Recovery no elimina la máquina virtual de origen ni la afecta de ninguna manera mientras se realizan los siguientes pasos.
>

1. Quite el bloqueo de la máquina virtual o del grupo de recursos de la máquina virtual. Por ejemplo:  Debajo del nombre de la máquina virtual "MoveDemo" tiene el bloqueo de recursos que debe eliminarse.

   ![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)
2. Descargue el script [Remove stale Azure Site Recovery configuration](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1) (Quitar la configuración obsoleta de Azure Site Recovery).
3. Ejecute el script *Cleanup-stale-asr-config-Azure-VM.ps1*.
4. Proporcione el identificador de la suscripción, el grupo de recursos de la máquina virtual y el nombre de la máquina virtual como un parámetro.
5. Si se le solicitan las credenciales de Azure, proporciónelas y compruebe que el script se ejecuta sin errores. 


## <a name="replication-cannot-be-enabled-because-of-the-existing-stale-resource-links-on-the-vm-error-code-150226"></a>La replicación no se puede habilitar debido a los vínculos de recursos obsoletos existentes en la máquina virtual (código de error 150226).

**Causa: La máquina virtual tiene una configuración obsoleta de la protección anterior de Site Recovery**.

Esta situación podría darse en los siguientes casos:

- Se ha habilitado la replicación para la máquina virtual de Azure mediante Site Recovery y después se ha deshabilitado la replicación, pero la **máquina virtual de origen tenía un bloqueo de recursos**.
- Habilitó la replicación para la máquina virtual de Azure mediante Site Recovery y luego eliminó el almacén de Site Recovery sin deshabilitar explícitamente la replicación en la máquina virtual.
- Habilitó la replicación para la máquina virtual de Azure mediante Site Recovery y luego eliminó el grupo de recursos que contenía el almacén de Site Recovery sin deshabilitar explícitamente la replicación en la máquina virtual.

### <a name="fix-the-problem"></a>Corrección del problema

>[!NOTE]
>
>Azure Site Recovery no elimina la máquina virtual de origen ni la afecta de ninguna manera mientras se realizan los siguientes pasos.


1. Quite el bloqueo de la máquina virtual o del grupo de recursos de la máquina virtual, si hay alguno. *Por ejemplo:* Debajo del nombre de la máquina virtual "MoveDemo" tiene el bloqueo de recursos que debe eliminarse.
   
   ![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)
2. Descargue el script [Remove stale Azure Site Recovery configuration](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1) (Quitar la configuración obsoleta de Azure Site Recovery).
3. Ejecute el script *Cleanup-stale-asr-config-Azure-VM.ps1*.
4. Proporcione el identificador de la suscripción, el grupo de recursos de la máquina virtual y el nombre de la máquina virtual como un parámetro.
5. Si se le solicitan las credenciales de Azure, proporciónelas y compruebe que el script se ejecuta sin errores.  

## <a name="unable-to-see-the-azure-vm-or-resource-group--for-selection-in-enable-replication"></a>No se puede ver la máquina virtual o el grupo de recursos de Azure para su selección en "Habilitar replicación"

 **Causa 1:  el grupo de recursos y la máquina virtual de origen están en ubicaciones distintas**
 
Actualmente, Azure Site Recovery requiere que el grupo de recursos y las máquinas virtuales de la región de origen estén en la misma ubicación. Si no es así, no podrá encontrar la máquina virtual o el grupo de recursos durante el momento de la protección. 

**Como solución alternativa**, puede habilitar la replicación desde la máquina virtual en lugar del almacén de Recovery Services. Vaya a la máquina virtual de origen > Propiedades > Recuperación ante desastres y habilite la replicación.

**Causa 2: el grupo de recursos no forma parte de la suscripción seleccionada**

Es posible que no pueda encontrar el grupo de recursos en el momento de la protección si no forma parte de la suscripción especificada. Asegúrese de que el grupo de recursos pertenece a la suscripción que se usa.

 **Causa 3: configuración obsoleta**
 
Si no ve la VM que quiere habilitar para la replicación, podría deberse a que se dejó una configuración obsoleta de Site Recovery en la VM de Azure. Esta situación podría darse en los siguientes casos:

- Habilitó la replicación para la máquina virtual de Azure mediante Site Recovery y luego eliminó el almacén de Site Recovery sin deshabilitar explícitamente la replicación en la máquina virtual.
- Habilitó la replicación para la máquina virtual de Azure mediante Site Recovery y luego eliminó el grupo de recursos que contenía el almacén de Site Recovery sin deshabilitar explícitamente la replicación en la máquina virtual.

- Se ha habilitado la replicación para la máquina virtual de Azure mediante Site Recovery y después se ha deshabilitado la replicación, pero la máquina virtual de origen tenía un bloqueo de recursos.

### <a name="fix-the-problem"></a>Corrección del problema

> [!NOTE]
>
> Asegúrese de actualizar el módulo "" AzureRM.Resources"" antes de usar el siguiente script. Azure Site Recovery no elimina la máquina virtual de origen ni la afecta de ninguna manera mientras se realizan los siguientes pasos.
>

1. Quite el bloqueo de la máquina virtual o del grupo de recursos de la máquina virtual, si hay alguno. *Por ejemplo:* Debajo del nombre de la máquina virtual "MoveDemo" tiene el bloqueo de recursos que debe eliminarse.

   ![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/vm-locks.png)
2. Descargue el script [Remove stale configuration](https://github.com/AsrOneSdk/published-scripts/blob/master/Cleanup-Stale-ASR-Config-Azure-VM.ps1) (Quitar la configuración obsoleta).
3. Ejecute el script *Cleanup-stale-asr-config-Azure-VM.ps1*.
4. Proporcione el identificador de la suscripción, el grupo de recursos de la máquina virtual y el nombre de la máquina virtual como un parámetro.
5. Si se le solicitan las credenciales de Azure, proporciónelas y compruebe que el script se ejecuta sin errores.

## <a name="unable-to-select-virtual-machine-for-protection"></a>No se puede seleccionar la máquina virtual para la protección
 **Causa 1:  la máquina virtual tiene alguna extensión instalada en estado Con errores o No responde** <br>
 Vaya a Máquinas virtuales > Configuración > Extensiones y compruebe si hay alguna extensión en un estado con errores. Desinstale la extensión con errores y vuelva a intentar proteger la máquina virtual.<br>
 **Causa 2:  [el estado de aprovisionamiento de la máquina virtual no es válido](#vms-provisioning-state-is-not-valid-error-code-150019)**

## <a name="vms-provisioning-state-is-not-valid-error-code-150019"></a>El estado de aprovisionamiento de la máquina virtual no es válido (código de error 150019)

Para habilitar la replicación en la máquina virtual, el estado de aprovisionamiento debe ser **Correcto**. Puede comprobar el estado de la máquina virtual con los pasos siguientes.

1.  Seleccione el **Explorador de recursos** de **todos los servicios** en Azure Portal.
2.  Expanda la lista **Suscripciones** y seleccione su suscripción.
3.  Expanda la lista **ResourceGroups** y seleccione el grupo de recursos de la máquina virtual.
4.  Expanda la lista **Recursos** lista y seleccione la máquina virtual.
5.  Compruebe el campo **provisioningState** en la vista Instancia de lado derecho.

### <a name="fix-the-problem"></a>Corrección del problema

- Si **provisioningState** es **Error**, póngase en contacto con soporte técnico con detalles para solucionar el problema.
- Si **provisioningState** es **Actualizando**, puede que se esté implementando otra extensión. Compruebe si hay operaciones en curso en la máquina virtual, espere a que finalicen y reintente el trabajo **Habilitar replicación** para la recuperación del sitio que presenta el error.

## <a name="unable-to-select-target-virtual-network---network-selection-tab-is-grayed-out"></a>No se puede seleccionar la red virtual de destino: pestaña selección de red atenuada.

**Causa 1: si la máquina virtual está conectada a una red que ya está asignada a una "red de destino".**
- Si la máquina virtual de origen forma parte de una red virtual y otra máquina virtual de la misma red virtual ya está asignada a una red del grupo de recursos de destino, se deshabilitará la lista desplegable de selección de red de manera predeterminada.

![Network_Selection_greyed_out](./media/site-recovery-azure-to-azure-troubleshoot/unabletoselectnw.png)

**Causa 2: si ha protegido previamente la máquina virtual mediante Azure Site Recovery y deshabilitado la replicación.**
 - Deshabilitar la replicación de una máquina virtual no elimina la asignación de red. Debe eliminarse desde el almacén de Recovery Services donde está protegida. </br>
 Vaya al almacén de Recovery Services > Site Recovery Infrastructure (Infraestructura de Site Recovery) > Asignaciones de red. </br>
 ![Delete_NW_Mapping](./media/site-recovery-azure-to-azure-troubleshoot/delete_nw_mapping.png)
 - La red de destino configurada durante la instalación de la recuperación ante desastres puede cambiarse después, una vez protegida la máquina virtual. </br>
 ![Modify_NW_mapping](./media/site-recovery-azure-to-azure-troubleshoot/modify_nw_mapping.png)
 - Tenga en cuenta que cambiar la asignación de red afecta a todas las máquinas virtuales protegidas que usen esa asignación de red específica.


## <a name="comvolume-shadow-copy-service-error-error-code-151025"></a>Error del servicio de instantáneas de volumen/COM+ (código de error 151025)

**Código de error** | **Causas posibles:** | **Recomendaciones**
--- | --- | ---
151025<br></br>**Mensaje**: no se pudo instalar la extensión de Site Recovery | - Servicio de "aplicación del sistema COM+" deshabilitado.</br></br>- Servicio de instantáneas de volumen desactivado.| Establezca los servicios de aplicación del sistema COM+ y de instantáneas de volumen en modo de inicio automático o manual.

### <a name="fix-the-problem"></a>Corrección del problema

Puede abrir la consola de los servicios y asegurarse de que la aplicación del sistema COM+ y el servicio de instantáneas de volumen no se encuentran en "Desactivado" en "Tipo de inicio".
  ![com-error](./media/azure-to-azure-troubleshoot-errors/com-error.png)

## <a name="unsupported-managed-disk-size-error-code-150172"></a>Tamaño del disco administrado no compatible (código de error 150172)


**Código de error** | **Causas posibles:** | **Recomendaciones**
--- | --- | ---
150172<br></br>**Mensaje**: Protection couldn't be enabled for the virtual machine as it has (DiskName) with size (DiskSize) that is lesser than the minimum supported size 1024 GB [No se pudo habilitar la protección para la máquina virtual, ya que tiene un disco (DiskName) con un tamaño (DiskSize), que es menor que el tamaño mínimo admitido de 1024 MB]. | -El tamaño del disco es menor que el tamaño admitido de 1024 MB| Asegúrese de que los tamaños de disco están dentro del intervalo de tamaño admitido y reintente la operación.

## <a name="enable-protection-failed-as-device-name-mentioned-in-the-grub-configuration-instead-of-uuid-error-code-151126"></a>No se pudo habilitar la protección debido a que el nombre del dispositivo se menciona en la configuración de GRUB en lugar del UUID (código de error 151126)

**Causa posible:** </br>
Los archivos de configuración de GRUB ("/boot/grub/menu.lst", "/boot/grub/grub.cfg", "/boot/grub2/grub.cfg" o "/etc/default/grub") pueden contener el valor de los parámetros **root** y **resume** como nombres reales del dispositivo, en lugar del UUID. Site Recovery exige el enfoque de UUID, ya que el nombre de los dispositivos puede cambiar al reiniciar la VM, debido a que la VM puede no mostrar el mismo nombre tras la conmutación por error, lo que puede generar problemas. Por ejemplo: </br>


- La línea siguiente procede del archivo GRUB **/boot/grub2/grub.cfg**. <br>
  *linux   /boot/vmlinuz-3.12.49-11-default **root=/dev/sda2**  ${extra_cmdline} **resume=/dev/sda1** splash=silent quiet showopts*


- La siguiente línea es del archivo GRUB **/boot/grub/menu.lst**
  *kernel /boot/vmlinuz-3.0.101-63-default **root=/dev/sda2** **resume=/dev/sda1** splash=silent crashkernel=256M-:128M showopts vga=0x314*

Si observa la cadena en negrita anterior, GRUB tiene nombres de dispositivos reales para los parámetros "root" y "resume", en lugar del UUID.

**Solución:**<br>
Los nombres de dispositivo se deben reemplazar con el UUID correspondiente.<br>


1. Para buscar el UUID del dispositivo, ejecute el comando "blkid \<nombre de dispositivo>". Por ejemplo:<br>
   ```
   blkid /dev/sda1
   ```<br>
   ```/dev/sda1: UUID="6f614b44-433b-431b-9ca1-4dd2f6f74f6b" TYPE="swap" ```<br>
   ```blkid /dev/sda2```<br>
   ```/dev/sda2: UUID="62927e85-f7ba-40bc-9993-cc1feeb191e4" TYPE="ext3"
   ```<br>
   ```



1. Ahora, sustituya el nombre del dispositivo por su UUID con el formato "root=UUID=\<UUID>". Por ejemplo, si reemplazamos los nombres de dispositivo por el UUID de root y reanudamos el parámetro mencionado anteriormente en los archivos "/boot/grub2/grub.cfg", "/boot/grub2/grub.cfg" o "/etc/default/grub:", las líneas de los archivos serán parecidas a las siguientes: <br>
   *kernel /boot/vmlinuz-3.0.101-63-default **root=UUID=62927e85-f7ba-40bc-9993-cc1feeb191e4** **resume=UUID=6f614b44-433b-431b-9ca1-4dd2f6f74f6b** splash=silent crashkernel=256M-:128M showopts vga=0x314*
1. Reinicie la protección

## <a name="enable-protection-failed-as-device-mentioned-in-the-grub-configuration-doesnt-existerror-code-151124"></a>No se pudo habilitar la protección debido a que el dispositivo que se menciona en la configuración de GRUB no existe (código de error 151124)
**Causa posible:** </br>
Los archivos de configuración de GRUB ("/boot/grub/menu.lst", "/boot/grub/grub.cfg", "/boot/grub2/grub.cfg" o "/etc/default/grub") pueden contener los parámetros "rd.lvm.lv" o "rd_LVM_LV" para indicar el dispositivo LVM que debe detectarse en el momento del arranque. Si estos dispositivos LVM no existen, el sistema protegido no se iniciará ni se atascará en el proceso de arranque. Se observará lo mismo con la VM de conmutación por error. Estos son algunos ejemplos:

Ejemplos: </br>

1. La línea siguiente procede del archivo GRUB **"/boot/grub2/grub.cfg"** en RHEL7. </br>
   *linux16 /vmlinuz-3.10.0-957.el7.x86_64 root=/dev/mapper/rhel_mup--rhel7u6-root ro crashkernel=128M\@64M **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet LANG=en_US.UTF-8*</br>
   La parte resaltada muestra que GRUB tiene que detectar dos dispositivos LVM con los nombres **"root"** y **"swap"** del grupo de volúmenes "rootvg".
1. La línea siguiente procede del archivo GRUB **"/etc/default/grub"** en RHEL7. </br>
   *GRUB_CMDLINE_LINUX="crashkernel=auto **rd.lvm.lv=rootvg/root rd.lvm.lv=rootvg/swap** rhgb quiet"*</br>
   La parte resaltada muestra que GRUB tiene que detectar dos dispositivos LVM con los nombres **"root"** y **"swap"** del grupo de volúmenes "rootvg".
1. La línea siguiente procede del archivo GRUB **"/boot/grub/menu.lst"** en RHEL6. </br>
   *kernel /vmlinuz-2.6.32-754.el6.x86_64 ro root=UUID=36dd8b45-e90d-40d6-81ac-ad0d0725d69e rd_NO_LUKS LANG=en_US.UTF-8 rd_NO_MD SYSFONT=latarcyrheb-sun16 crashkernel=auto rd_LVM_LV=rootvg/lv_root  KEYBOARDTYPE=pc KEYTABLE=us rd_LVM_LV=rootvg/lv_swap rd_NO_DM rhgb quiet* </br>
   La parte resaltada muestra que GRUB tiene que detectar dos dispositivos LVM con los nombres **"root"** y **"swap"** del grupo de volúmenes "rootvg".<br>

**Solución:**<br>

Si el dispositivo LVM no existe, corríjalo mediante su creación o quite el parámetro del mismo de los archivos de configuración de GRUB y, a continuación, vuelva a intentar habilitar la protección. </br>

## <a name="site-recovery-mobility-service-update-completed-with-warnings--error-code-151083"></a>Actualización del servicio de movilidad de Site Recovery completada con advertencias (código de error 151083)
El servicio de movilidad de Site Recovery tiene muchos componentes, uno de los cuales es el controlador del filtro. El controlador del filtro solamente se carga en el sistema cuando se reinicia el sistema. Siempre que haya actualizaciones del servicio de movilidad de Site Recovery que tengan cambios en el controlador del filtro, se actualiza la máquina, pero se sigue generando una advertencia de que algunas correcciones requieren un reinicio. Esto significa que las correcciones del controlador del filtro solo pueden aplicarse cuando se carga un nuevo controlador del filtro, lo que solo ocurre cuando se reinicia el sistema.<br>
**Tenga en cuenta** que solo se trata de una advertencia y que la replicación existente seguirá funcionando incluso después de que se realice la nueva actualización del agente. Puede reiniciar el sistema en cualquier momento para aprovechar las ventajas del nuevo controlador del filtro, pero, si no reinicia, el antiguo controlador seguirá funcionando. A parte del controlador del filtro, **se aplicarán todas las demás mejoras y correcciones del servicio de movilidad sin necesidad de reiniciar cuando se actualice el agente**.  


## <a name="protection-couldnt-be-enabled-as-replica-managed-disk-diskname-replica-already-exists-without-expected-tags-in-the-target-resource-group-error-code-150161"></a>No se pudo habilitar la protección porque el disco administrado de la réplica "diskname-replica" ya existe sin las etiquetas esperadas en el grupo de recursos de destino (código de error 150161)

**Causa**: Puede producirse si la máquina virtual se protegió anteriormente y durante la deshabilitación de la replicación el disco de la réplica no se limpió debido a alguna razón.</br>
**Solución:** Elimine el disco de la réplica mencionado en el mensaje de error y vuelva a reiniciar el trabajo de protección con error.

## <a name="next-steps"></a>Pasos siguientes
[Replicación de máquinas virtuales de Azure](site-recovery-replicate-azure-to-azure.md)
