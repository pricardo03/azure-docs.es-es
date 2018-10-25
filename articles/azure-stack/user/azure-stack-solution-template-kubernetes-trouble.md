---
title: Solucionar problemas de la implementación de Kubernetes (K8) en Azure Stack | Microsoft Docs
description: Obtenga información acerca de cómo solucionar problemas de la implementación de Kubernetes (K8) en Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2018
ms.author: mabrigg
ms.reviewer: waltero
ms.openlocfilehash: fbb51d8dc3b1ea4c6b34120e8fe35474ae949cf2
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116919"
---
# <a name="troubleshoot-your-deployment-to-kubernetes-to-azure-stack"></a>Solucionar problemas de implementación de Kubernetes en Azure Stack

*Se aplica a: sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

> [!Note]  
> Kubernetes en Azure Stack está en versión preliminar.

En el siguiente artículo se analiza la resolución de problemas del clúster de Kubernetes. Puede revisar la alerta y el estado de la implementación mediante los elementos necesarios para la implementación. Es posible que deba recopilar los registros de implementación de Azure Stack o las máquinas virtuales de Linux que hospedan Kubernetes. Además, deberá trabajar con el administrador de Azure Stack para recuperar los registros de un punto de conexión administrativo.

## <a name="overview-of-deployment"></a>Información general de la implementación

Antes de pasar a los pasos para solucionar problemas del clúster, puede que quiera revisar el proceso de implementación del clúster de Azure Stack Kubernetes. La implementación usa una plantilla de solución de Azure Resource Manager para crear las máquinas virtuales e instala ACS Engine para el clúster.

### <a name="deployment-workflow"></a>Flujo de trabajo de implementación

En el siguiente diagrama se muestra el proceso general para implementar el clúster.

![Implementación del proceso Kubernetes](media/azure-stack-solution-template-kubernetes-trouble/002-Kubernetes-Deploy-Flow.png)

### <a name="deployment-steps"></a>Pasos de implementación

1. Recopila los parámetros de entrada del elemento de Marketplace.

    Escribe los valores necesarios para configurar el clúster de Kubernetes que incluye:
    -  **Nombre de usuario**: nombre de usuario de las instancias de Linux Virtual Machines que forman parte del clúster de Kubernetes y DVM.
    -  **Clave pública SSH**: clave que se usa para la autorización en todas las máquinas Linux creadas como parte del clúster de Kubernetes y DVM.
    -  **Entidad de servicio**: id. que usa el proveedor de nube de Azure Kubernetes. El id. de cliente identificado como identificador de la aplicación cuando se creó la entidad de servicio. 
    -  **Secreto de cliente**: la clave que se estableció al crear la entidad de servicio.

2. Crea la VM de implementación y la extensión de script personalizada.
    -  Crea la VM Linux de implementación mediante la imagen de Linux de Marketplace, **Ubuntu Server 16.04-LTS**.
    -  Descargue y ejecute la extensión de script del cliente de Marketplace. El script es **Script personalizado para Linux 2.0**.
    -  Se ejecuta el script personalizado de DVM. El script:
        1. Obtiene el punto de conexión de la galería del punto de conexión de los metadatos de Azure Resource Manager.
        2. Obtiene el id. del recurso del directorio activo del punto de conexión de los metadatos de Azure Resource Manager.
        3. Carga el modelo de API para ACS Engine.
        4. Implementa ACS Engine en el clúster de Kubernetes y guarda el perfil de la nube de Azure Stack en `/etc/kubernetes/azurestackcloud.json`.
3. Crea máquinas virtuales maestras.

    Descarga y ejecuta la extensión del script de cliente.

4. Ejecuta el script principal.

    El script:
    - Instala los recursos de etcd, Docker y Kubernetes como kubelet. etcd es un almacén de pares valor-clave distribuido que proporciona una manera de almacenar datos en un clúster de máquinas. Docker es compatible con virtualizaciones básicas a nivel del sistema operativo, conocidas como contenedores. Kubelet es el agente de nodo que se ejecuta en cada nodo de Kubernetes.
    - Configura el servicio etcd.
    - Configura el servicio Kubelet.
    - Inicia kubelet. Esto implica los pasos siguientes:
        1. Inicia el Servicio de API.
        2. Inicia el servicio Controller.
        3. Inicia el servicio Scheduler.
5. Crea máquinas virtuales de agente.

    Descarga y ejecuta la extensión del script del cliente.

6. Ejecuta el script del agente. El script personalizado del agente:
    - Instale etcd.
    - Configure el servicio de Kubelet.
    - Una el clúster de Kubernetes.

## <a name="steps-for-troubleshooting"></a>Pasos para solucionar los problemas

Puede recopilar registros en máquinas virtuales que admiten el clúster de Kubernetes. También puede revisar el registro de implementación. También es posible que tenga que hablar con el administrador de Azure Stack para comprobar qué versión está usando y para obtener registros de Azure Stack relacionados con la implementación.

1. Revise el [estado de implementación](#review-deployment-status) y [recupere los registros](#get-logs-from-a-vm) del nodo principal en el clúster de Kubernetes.
2. Deberá usar la versión más reciente de Azure Stack. Si no está seguro de la versión de Azure Stack, póngase en contacto con el administrador de Azure Stack. El elemento Clúster de Kubernetes de Marketplace 0.3.0 requiere la versión 1808 (o superior) de Azure Stack.
3.  Revise los archivos de creación de la máquina virtual. Es posible que detecte los siguientes problemas:  
    - Es posible que la clave pública sea incorrecta. Revise la clave que ha creado.  
    - Puede que la creación de máquinas virtuales haya desencadenado un error interno o un error de creación. Los errores pueden deberse a diversos factores, como las limitaciones de capacidad de la suscripción de Azure Stack.
    - ¿El nombre de dominio completo (FQDN) de la máquina virtual comienza con un prefijo duplicado?
4.  Si la máquina virtual es **correcta**, evalúe la instancia de DVM. Si la instancia de DVM tiene un mensaje de error:

    - Es posible que la clave pública sea incorrecta. Revise la clave que ha creado.  
     - Deberá ponerse en contacto con el administrador de Azure Stack para recuperar los registros de Azure Stack mediante los puntos de conexión con privilegios. Para más información, consulte [Registros de diagnóstico de Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics).
5. Si tiene preguntas acerca de la implementación, puede publicarlas o ver si alguien ya ha respondido a las mismas preguntas en el [foro de Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack). 

## <a name="review-deployment-status"></a>Revisar el estado de implementación

Puede revisar el estado de implementación al implementar el clúster de Kubernetes para revisar los problemas.

1. Abra el [portal de Azure Stack](https://portal.local.azurestack.external).
2. Seleccione **Grupos de recursos** y, después, seleccione el nombre del grupo de recursos que usó al implementar el clúster de Kubernetes.
3. Seleccione **Implementaciones** y, después, el **Nombre de la implementación**.

    ![solución de problemas](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-kub-trouble-report.png)

4.  Consulte la ventana de solución de problemas. Cada uno de los recursos implementados ofrece la siguiente información.
    
    | Propiedad | DESCRIPCIÓN |
    | ----     | ----        |
    | Recurso | Nombre del recurso. |
    | Escriba | Proveedor de recursos y tipo de recurso. |
    | Status | Estado del elemento. |
    | TimeStamp | Marca de tiempo UTC de la hora. |
    | Detalles de la operación | Detalles de la operación, como el proveedor de recursos implicado en la operación, el punto de conexión de recursos y el nombre del recurso. |

    Cada elemento tiene un icono de estado de color verde o rojo.

## <a name="get-logs-from-a-vm"></a>Obtención de registros desde una máquina virtual

Será necesario que se conecte a la máquina virtual principal del clúster, abra un símbolo del sistema de bash y ejecute un script para generar los registros. La máquina virtual maestra se puede encontrar en el grupo de recursos del clúster y se denomina `k8s-master-<sequence-of-numbers>`. 

### <a name="prerequisites"></a>Requisitos previos

Necesitará un símbolo del sistema de bash en la máquina que usa para administrar Azure Stack. Use bash para ejecutar los scripts que tienen acceso a los registros. En un equipo Windows, puede utilizar el símbolo del sistema de bash instalado con Git. Para obtener la versión más reciente de git, consulte las [descargas de git](https://git-scm.com/downloads).

### <a name="get-logs"></a>Obtención de registros

1. Abra un símbolo del sistema de bash. Si usa git en un equipo Windows, puede abrir un símbolo del sistema de bash desde la siguiente ruta: `c:\programfiles\git\bin\bash.exe`.
2. Ejecute los comandos de bash siguientes:

    ```Bash  
    mkdir -p $HOME/kuberneteslogs
    cd $HOME/kuberneteslogs
    curl -O https://raw.githubusercontent.com/msazurestackworkloads/azurestack-gallery/master/diagnosis/getkuberneteslogs.sh
    sudo chmod 744 getkuberneteslogs.sh
    ```

    > [!Note]  
    > En Windows, no es necesario ejecutar `sudo` y puede usar simplemente `chmod 744 getkuberneteslogs.sh`.

3. En la misma sesión, ejecute el comando siguiente con los parámetros actualizados para que coincidan con su entorno.

    ```Bash  
    ./getkuberneteslogs.sh --identity-file id_rsa --user azureuser --vmdhost 192.168.102.37
    ```

    Revise los parámetros y configure los valores en función de su entorno.
    | Parámetro           | DESCRIPCIÓN                                                                                                      | Ejemplo                                                                       |
    |---------------------|------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------|
    | -i, --identity-file | El archivo de clave privada de RSA para conectar la máquina virtual maestra de kubernetes. La clave debe comenzar por `-----BEGIN RSA PRIVATE KEY-----` | C:\data\privatekey.pem                                                        |
    | -h, --host          | La dirección IP pública o el nombre de dominio completo (FQDN) de la máquina virtual maestra del clúster de Kubernetes. El nombre de la VM comienza por `k8s-master-`.                       | IP: 192.168.102.37<br><br>FQDN: k8s-12345.local.cloudapp.azurestack.external      |
    | -u, --user          | El nombre de usuario de la máquina virtual maestra del clúster de Kubernetes. Establece este nombre al configurar el elemento del Marketplace.                                                                    | azureuser                                                                     |
    | -d, --vmdhost       | La dirección IP pública o el FQDN de DVM. El nombre de la VM comienza por `vmd-`.                                                       | IP: 192.168.102.38<br><br>DNS: vmd-dnsk8-frog.local.cloudapp.azurestack.external |

   Al agregar los valores de sus parámetros, puede que su aspecto sea similar a:

    ```Bash  
    ./getkuberneteslogs.sh --identity-file "C:\secretsecret.pem" --user azureuser --vmdhost 192.168.102.37
     ```

    Una ejecución correcta que crea los registros.

    ![Registros generados](media/azure-stack-solution-template-kubernetes-trouble/azure-stack-generated-logs.png)


4. Permite recuperar los registros en las carpetas que creó el comando. El comando creará una nueva carpeta y marcará la hora.
    - KubernetesLogs*YYYY-MM-DD-XX-XX-XX-XXX*
        - Dvmlogs
        - Acsengine-kubernetes-dvm.log

## <a name="next-steps"></a>Pasos siguientes

[Implementación de Kubernetes en Azure Stack](azure-stack-solution-template-kubernetes-deploy.md).

[Agregar un clúster de Kubernetes a Marketplace (para el operador de Azure Stack)](..\azure-stack-solution-template-kubernetes-cluster-add.md)

[Kubernetes en Azure](https://docs.microsoft.com/azure/container-service/kubernetes/container-service-kubernetes-walkthrough)
