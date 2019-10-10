---
title: Administración de restricciones del contexto de seguridad en Red Hat OpenShift en Azure | Microsoft Docs
description: 'Administrador de clústeres de Red Hat OpenShift en Azure: administración de restricciones del contexto de seguridad'
services: container-service
author: troy0820
ms.author: jzim
ms.service: container-service
ms.topic: article
ms.date: 09/25/2019
ms.openlocfilehash: 8e85ac98683487c6b18be7f502f28cad9a0c2251
ms.sourcegitcommit: d4c9821b31f5a12ab4cc60036fde00e7d8dc4421
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71712843"
---
# <a name="overview"></a>Información general 

Las restricciones del contexto de seguridad permiten a los administradores controlar los permisos para los pods. Para más información sobre este tipo de API, consulte la documentación sobre la arquitectura de las [restricciones del contexto de seguridad](https://docs.openshift.com/container-platform/3.11/architecture/additional_concepts/authorization.html) (SCC). Puede administrar las SCC en su instancia como objetos de API normales mediante la CLI.

## <a name="listing-security-context-constraints"></a>Enumeración de restricciones del contexto de seguridad

Para obtener una lista actual de SCC: 

```bash
$ oc get scc

NAME               PRIV      CAPS      SELINUX     RUNASUSER          FSGROUP     SUPGROUP    PRIORITY   READONLYROOTFS   VOLUMES
anyuid             false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    10         false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
hostaccess         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath persistentVolumeClaim secret]
hostmount-anyuid   false     []        MustRunAs   RunAsAny           RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir hostPath nfs persistentVolumeClaim secret]
hostnetwork        false     []        MustRunAs   MustRunAsRange     MustRunAs   MustRunAs   <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
nonroot            false     []        MustRunAs   MustRunAsNonRoot   RunAsAny    RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
privileged         true      [*]       RunAsAny    RunAsAny           RunAsAny    RunAsAny    <none>     false            [*]
restricted         false     []        MustRunAs   MustRunAsRange     MustRunAs   RunAsAny    <none>     false            [configMap downwardAPI emptyDir persistentVolumeClaim secret]
```

## <a name="examining-a-security-context-constraints-object"></a>Examen de un objeto de restricciones del contexto de seguridad

Para examinar un SCC determinado, use `oc get`, `oc describe` o `oc edit`.  Por ejemplo, para examinar el SCC **restricted**:
```bash
$ oc describe scc restricted
Name:                   restricted
Priority:               <none>
Access:
  Users:                <none>
  Groups:               system:authenticated
Settings:
  Allow Privileged:         false
  Default Add Capabilities:     <none>
  Required Drop Capabilities:       KILL,MKNOD,SYS_CHROOT,SETUID,SETGID
  Allowed Capabilities:         <none>
  Allowed Seccomp Profiles:     <none>
  Allowed Volume Types:         configMap,downwardAPI,emptyDir,persistentVolumeClaim,projected,secret
  Allow Host Network:           false
  Allow Host Ports:         false
  Allow Host PID:           false
  Allow Host IPC:           false
  Read Only Root Filesystem:        false
  Run As User Strategy: MustRunAsRange
    UID:                <none>
    UID Range Min:          <none>
    UID Range Max:          <none>
  SELinux Context Strategy: MustRunAs
    User:               <none>
    Role:               <none>
    Type:               <none>
    Level:              <none>
  FSGroup Strategy: MustRunAs
    Ranges:             <none>
  Supplemental Groups Strategy: RunAsAny
    Ranges:             <none>
```
## <a name="next-steps"></a>Pasos siguientes
Configuración del rol osa-customer-admin:
> [!div class="nextstepaction"]
> [Integración de Azure Active Directory con Red Hat OpenShift en Azure](howto-aad-app-configuration.md) 