---
title: Implementar la oferta de Marketplace autoadministrados de plataforma de contenedor de OpenShift en Azure | Microsoft Docs
description: Implementar la oferta de Marketplace autoadministrados de plataforma de contenedor de OpenShift en Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/18/2019
ms.author: haroldw
ms.openlocfilehash: 1228c770799de37c85b8a48b1dc923ac8294eeca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60773613"
---
# <a name="configure-prerequisites"></a>Configuración de los requisitos previos

Antes de usar la oferta de Marketplace para implementar un clúster de OpenShift Container Platform autoadministrado en Azure, se deben configurar algunos requisitos previos.  Leer el [requisitos previos de OpenShift](https://docs.microsoft.com/azure/virtual-machines/linux/openshift-prerequisites) artículo para obtener instrucciones crear un ssh clave (sin una frase de contraseña), almacén de claves de Azure, el secreto de almacén de claves y una entidad de servicio.

 
## <a name="deploy-using-the-marketplace-offer"></a>La implementación con la oferta de Marketplace

La manera más sencilla de implementar un clúster de OpenShift Container Platform autoadministrado en Azure es usar el [oferta de Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Esta opción es la más sencilla, pero también tiene funcionalidades de personalización limitadas. La oferta de Marketplace de OpenShift Container Platform 3.11.82 implementa e incluye las siguientes opciones de configuración:

- **Nodos maestros**: tres (3) nodos maestros con tipo de instancia configurable.
- **Nodos de infraestructura**: tres (3) nodos de infraestructura con tipo de instancia configurable.
- **Nodos**: El número de nodos (entre 1 y 9) y el tipo de instancia es configurable.
- **Tipo de disco**: se usan discos administrados.
- **Redes**: Compatibilidad con red nueva o existente y el intervalo CIDR personalizado.
- **CNS**: se puede habilitar CNS.
- **Métricas**: se pueden habilitar métricas.
- **Registro**: se puede habilitar el registro.
- **Proveedor de soluciones en la nube de Azure**: Habilitado de forma predeterminada, se puede deshabilitar.

En la parte superior izquierda de Azure portal, haga clic en **crear un recurso**, escriba 'openshift container platform' en el cuadro de búsqueda y presione ENTRAR.

   ![Nueva búsqueda de recursos](media/openshift-marketplace-self-managed/ocp-search.png)  
<br>

Se abrirá la página de resultados con **Red Hat OpenShift Container Platform Self-Managed** en la lista. 

   ![Resultado de la búsqueda de recursos nuevo](media/openshift-marketplace-self-managed/ocp-searchresult.png)  
<br>

Haga clic en la oferta para ver los detalles de la oferta. Para implementar esta oferta, haga clic en **crear**. Aparecerá la interfaz de usuario que escriba los parámetros necesarios. Es la primera pantalla la **Fundamentos** hoja.

   ![Página de título de la oferta](media/openshift-marketplace-self-managed/ocp-titlepage.png)  
<br>

**Aspectos básicos**

Para obtener ayuda sobre cualquiera de los parámetros de entrada, mantenga el mouse sobre el ***i*** junto al nombre del parámetro.

Especifique valores para los parámetros de entrada y haga clic en **Aceptar**.

| Parámetro de entrada | Descripción del parámetro |
|-----------------------|-----------------|
| Nombre de usuario Administrador de máquina virtual | El usuario de administrador que se crearán en todas las instancias de máquina virtual |
| SSH clave pública para el usuario administrador | Clave pública SSH utilizada para iniciar sesión en una máquina virtual: no debe tener una frase de contraseña |
| Subscription | Suscripción de Azure para implementar el clúster a |
| Grupo de recursos | Crear un nuevo grupo de recursos o seleccione un grupo de recursos vacío existente para los recursos de clúster |
| Location | Región de Azure para implementar el clúster a |

   ![Hoja de datos básicos de la oferta](media/openshift-marketplace-self-managed/ocp-basics.png)  
<br>

**Configuración de infraestructura**

Especifique valores para los parámetros de entrada y haga clic en **Aceptar**.

| Parámetro de entrada | Descripción del parámetro |
|-----------------------|-----------------|
| Prefijo del nombre de clúster OCP | El usuario de administrador que se crearán en todas las instancias de máquina virtual |
| Tamaño del nodo maestro | Acepte el tamaño de máquina virtual predeterminada o haga clic en **cambiar tamaño** para seleccionar un tamaño de máquina virtual diferente.  Seleccione el tamaño de máquina virtual adecuado para su carga de trabajo |
| Tamaño de nodo de infraestructura | Acepte el tamaño de máquina virtual predeterminada o haga clic en **cambiar tamaño** para seleccionar un tamaño de máquina virtual diferente.  Seleccione el tamaño de máquina virtual adecuado para su carga de trabajo |
| Número de nodos de aplicación | Acepte el tamaño de máquina virtual predeterminada o haga clic en **cambiar tamaño** para seleccionar un tamaño de máquina virtual diferente.  Seleccione el tamaño de máquina virtual adecuado para su carga de trabajo |
| Tamaño de nodo de la aplicación | Acepte el tamaño de máquina virtual predeterminada o haga clic en **cambiar tamaño** para seleccionar un tamaño de máquina virtual diferente.  Seleccione el tamaño de máquina virtual adecuado para su carga de trabajo |
| Tamaño del Host bastión | Acepte el tamaño de máquina virtual predeterminada o haga clic en **cambiar tamaño** para seleccionar un tamaño de máquina virtual diferente.  Seleccione el tamaño de máquina virtual adecuado para su carga de trabajo |
| Red Virtual nueva o existente | Crear una nueva red virtual (valor predeterminado) o usar una red virtual existente |
| Elija la configuración predeterminada de CIDR o personalizar el intervalo de IP (CIDR) | Acepte los intervalos CIDR predeterminada o seleccione **intervalo IP personalizado** y escriba la información personalizada de CIDR.  Configuración predeterminada crear red virtual con CIDR de 10.0.0.0/14, subred maestro con 10.1.0.0/16 infra subred con 10.2.0.0/16 y proceso y cns con 10.3.0.0/16 |
| Nombre del grupo de recursos de Key Vault | El nombre del grupo de recursos que contiene el almacén de claves |
| Nombre de Key Vault | El nombre del almacén de claves que contiene el secreto con el ssh clave privada.  Solo caracteres alfanuméricos y guiones se permiten y tener entre 3 y 24 caracteres |
| Nombre del secreto | El nombre del secreto que contiene el ssh clave privada.  Se permiten solo caracteres alfanuméricos y guiones |

   ![Hoja de la oferta de infraestructura](media/openshift-marketplace-self-managed/ocp-inframain.png)  
<br>

**Cambiar tamaño**

Para seleccionar un tamaño de máquina virtual diferente, haga clic en ***cambiar tamaño***.  Se abrirá la ventana de selección de máquina virtual.  Seleccione el tamaño de máquina virtual que desee y haga clic en **seleccione**.

   ![Seleccione el tamaño de máquina virtual](media/openshift-marketplace-self-managed/ocp-selectvmsize.png)  
<br>

**Red Virtual existente**

| Parámetro de entrada | Descripción del parámetro |
|-----------------------|-----------------|
| Nombre de red Virtual existente | Nombre de la red virtual existente |
| Nombre de subred para los nodos principales | Nombre de subred existente para los nodos principales.  Debe contener al menos 16 direcciones IP y siga RFC 1918 |
| Nombre de subred para nodos de infraestructura | Nombre de existente de la subred de infraestructura nodos.  Debe contener al menos 32 direcciones IP y siga RFC 1918 |
| Nombre de subred para los nodos de proceso y cns | Nombre de subred existente para los nodos de proceso y los nombres comunes.  Debe contener al menos 32 direcciones IP y siga RFC 1918 |
| Grupo de recursos para la red Virtual existente | Nombre del grupo de recursos que contiene la red virtual existente |

   ![Red virtual de infraestructura existente de la oferta](media/openshift-marketplace-self-managed/ocp-existingvnet.png)  
<br>

**Intervalo de IP personalizado**

| Parámetro de entrada | Descripción del parámetro |
|-----------------------|-----------------|
| Intervalo de direcciones de la red Virtual | Custom CIDR para la red virtual |
| Intervalo de direcciones de la subred que contiene los nodos principales | Custom CIDR para subred principal |
| Intervalo de direcciones de la subred que contiene los nodos de infraestructura | Custom CIDR para subred de infraestructura |
| Intervalo de direcciones de subred que contiene los nodos de proceso y cns | CIDR personalizado para los nodos de proceso y cns |

   ![Intervalo IP personalizado de infraestructura de la oferta](media/openshift-marketplace-self-managed/ocp-customiprange.png)  
<br>

**OpenShift Container Platform**

Escriba valores para los parámetros de entrada y haga clic en **Aceptar**

| Parámetro de entrada | Descripción del parámetro |
|-----------------------|-----------------|
| Contraseña de usuario Administrador de OpenShift | Contraseña del usuario inicial de OpenShift.  Este usuario será también el Administrador de clúster |
| Confirme la contraseña de usuario Administrador de OpenShift | Vuelva a escribir la contraseña de usuario de administrador de OpenShift |
| Nombre de usuario de Red Hat Subscription Manager | Nombre de usuario para acceder a su suscripción de Red Hat o identificador de la organización.  Esta credencial se usa para registrar la instancia RHEL a su suscripción y no se almacenan por Microsoft o Red Hat |
| Contraseña de usuario de Red Hat Subscription Manager | Contraseña para acceder a su suscripción de Red Hat o la clave de activación.  Esta credencial se usa para registrar la instancia RHEL a su suscripción y no se almacenan por Microsoft o Red Hat |
| Id. de grupo de OpenShift de Red Hat Subscription Manager | Id. de grupo que contiene los derechos de OpenShift Container Platform. Asegúrese de que tiene suficientes derechos de OpenShift Container Platform para la instalación del clúster |
| Id. de grupo de OpenShift de Red Hat Subscription Manager de Broker / maestro de nodos | Identificador que contiene los derechos de OpenShift Container Platform para el agente del grupo o nodos maestros. Asegúrese de que tiene suficientes derechos de OpenShift Container Platform para la instalación del clúster. Si no utiliza agente / maestro de Id. de grupo, escriba el identificador del grupo de nodos de aplicación |
| Configurar el proveedor de nube de Azure | Configurar OpenShift para usar el proveedor de nube de Azure. Es necesario si se asocia con disco de Azure para los volúmenes persistentes.  Valor predeterminado es Yes |
| GUID de Id. de cliente de entidad de servicio de AD Azure | Entidad de servicio de AD cliente identificador GUID de Azure - también conocida como AppID. Solo es necesario si configura un proveedor de nube de Azure se establece en **sí** |
| Secreto de Id. de cliente de entidad de servicio de AD Azure | Secreto de Id. de cliente de entidad de servicio de AD Azure. Solo es necesario si configura un proveedor de nube de Azure se establece en **sí** |
 
   ![Hoja de OpenShift de oferta](media/openshift-marketplace-self-managed/ocp-ocpmain.png)  
<br>

**Configuración adicional**

La hoja de configuración adicional permite la configuración de nombres comunes para el almacenamiento de glusterfs, enrutador Sub, métricas y registro de dominio.  El valor predeterminado no instala cualquiera de estas opciones y nip.io se usará como el subdominio de enrutador con fines de prueba. Habilitación de CNS instalará tres nodos de proceso adicionales con tres discos conectados adicionales que se va a hospedar glusterfs pods.  

Escriba valores para los parámetros de entrada y haga clic en **Aceptar**

| Parámetro de entrada | Descripción del parámetro |
|-----------------------|-----------------|
| Configurar el almacenamiento nativo de contenedor (CNS) | CNS de instalaciones en el OpenShift del clúster y habilitarlo como almacenamiento. Será el valor predeterminado si se deshabilita el proveedor de Azure |
| Configurar el registro de clúster | Instala la funcionalidad de registro EFK en el clúster.  Cambiar el tamaño de infra nodos adecuadamente a los pods EFK de host |
| Configurar las métricas para el clúster | Instala Hawkular métricas en el clúster de OpenShift.  Cambiar el tamaño de infra nodos adecuadamente a los pods Hawkular métricas de host |
| Dominio de Sub enrutador predeterminado | Seleccione nipio para las pruebas o personalizado para escribir su propio subdominio para producción |
 
   ![Hoja de oferta adicional](media/openshift-marketplace-self-managed/ocp-additionalmain.png)  
<br>

**Configuración adicional: los parámetros adicionales**

| Parámetro de entrada | Descripción del parámetro |
|-----------------------|-----------------|
| (CNS) Tamaño de nodo | Acepte el tamaño de nodo predeterminado o seleccione **cambiar tamaño** para seleccionar un nuevo tamaño de máquina virtual |
| Escriba el subdominio personalizado | El dominio de enrutamiento personalizado que se utilizará para exponer las aplicaciones a través del enrutador en el clúster de OpenShift.  Asegúrese de crear la entrada DNS adecuada de caracteres comodín] |
 
   ![Oferta adicional cns instalar](media/openshift-marketplace-self-managed/ocp-additionalcnsall.png)  
<br>

**Resumen**

Se produce la validación en esta fase para comprobar la cuota de núcleos es suficiente para implementar el número total de máquinas virtuales seleccionadas para el clúster.  Revise todos los parámetros que se especificaron.  Si las entradas son aceptables, haga clic en **Aceptar** para continuar.

   ![Hoja de resumen de oferta](media/openshift-marketplace-self-managed/ocp-summary.png)  
<br>

**Comprar**

Confirme la información de contacto en la página de compra y haga clic en **compra** para aceptar los términos de uso y empezar la implementación del clúster de OpenShift Container Platform.

   ![Hoja de oferta de compra](media/openshift-marketplace-self-managed/ocp-purchase.png)  
<br>


## <a name="connect-to-the-openshift-cluster"></a>Conexión con el clúster de OpenShift

Cuando finalice la implementación, puede recuperar la conexión de la sección de salida de la implementación. Conectarse a la consola de OpenShift con el explorador mediante la **dirección URL de la consola de OpenShift**. También puede acceder mediante SSH al host bastión. El siguiente es un ejemplo en el que el nombre de usuario administrador es clusteradmin y el nombre de dominio completo de DNS de la dirección IP pública del host de tipo bastión es bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [az group delete](/cli/azure/group) para quitar el grupo de recursos, el clúster de OpenShift y todos los recursos relacionados.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Pasos siguientes

- [Tareas posteriores a la implementación](./openshift-post-deployment.md)
- [Solución de problemas de implementación de OpenShift en Azure](./openshift-troubleshooting.md)
- [Introducción a OpenShift Container Platform](https://docs.openshift.com/container-platform)

