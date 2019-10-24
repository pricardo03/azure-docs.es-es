---
title: Vista de los registros de Azure Monitor para contenedores en tiempo real | Microsoft Docs
description: En este artículo se describe la vista en tiempo real de los registros de contenedor (stdout/stderr) sin usar kubectl con Azure Monitor para contenedores.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 07/12/2019
ms.openlocfilehash: 25cfe10ec192f874d050bca22ce1b85c2d1afbb4
ms.sourcegitcommit: ae461c90cada1231f496bf442ee0c4dcdb6396bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2019
ms.locfileid: "72554089"
---
# <a name="how-to-view-logs-and-events-in-real-time-preview"></a>Visualización de registros y eventos en tiempo real (versión preliminar)
Azure Monitor para contenedores incluye una característica (actualmente en versión preliminar) que proporciona una vista en vivo de los registros de contenedor (stdout y stderr) y los eventos de Azure Kubernetes Service (AKS) sin tener que ejecutar los comandos de kubectl. Cuando se selecciona cualquiera de las opciones, aparece un nuevo panel debajo de la tabla de datos de rendimiento en la vista **Nodos**, **Controladores** y **Contenedores**. En este panel se muestran el registro y los eventos en vivo generados por el motor de contenedores para ayudar a solucionar problemas en tiempo real.

>[!NOTE]
>Esta característica está disponible en todas las regiones de Azure, incluida Azure China. Actualmente no está disponible en Azure Gobierno de EE. UU.

>[!NOTE]
>Para que esta característica funcione, se requiere el acceso al recurso de clúster del **rol de usuario de Azure Kubernetes Service**. [Obtenga más información sobre el rol de usuario de clúster de Azure Kubernetes](https://docs.microsoft.com/en-us/azure/aks/control-kubeconfig-access#available-cluster-roles-permissions).

Los registros dinámicos admiten tres métodos diferentes para controlar el acceso a los registros:

1. AKS sin autorización RBAC de Kubernetes habilitada
2. AKS habilitado con autorización de RBAC de Kubernetes
3. AKS habilitado con inicio de sesión único basado en SAML de Azure Active Directory (AD)

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Clúster de Kubernetes sin RBAC habilitado
 
Si tiene un clúster de Kubernetes que no está configurado con autorización de RBAC de Kubernetes ni integrado en el inicio de sesión único de Azure AD, no es necesario seguir estos pasos. Dado que la autorización de Kubernetes usa la api de kube, se requieren permisos de solo lectura.

## <a name="kubernetes-rbac-authorization"></a>Autorización de RBAC de Kubernetes
Si ha habilitado la autorización de RBAC de Kubernetes, deberá aplicar el enlace de rol de clúster. Los pasos de ejemplo siguientes muestran cómo configurar el enlace de rol de clúster desde esta plantilla de configuración de yaml. 

1. Copie el archivo yaml, péguelo y guárdelo como LogReaderRBAC.yaml.  

    ```
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRole 
    metadata: 
       name: containerHealth-log-reader 
    rules: 
       - apiGroups: [""] 
         resources: ["pods/log", "events"] 
         verbs: ["get", "list"]  
    --- 
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRoleBinding 
    metadata: 
       name: containerHealth-read-logs-global 
    roleRef: 
        kind: ClusterRole 
        name: containerHealth-log-reader 
        apiGroup: rbac.authorization.k8s.io 
    subjects: 
       - kind: User 
         name: clusterUser 
         apiGroup: rbac.authorization.k8s.io
    ```

2. Si es la primera vez que lo configura, ejecute el comando `kubectl create -f LogReaderRBAC.yaml` para aplicar el enlace de la regla del clúster. Si habilitó anteriormente la compatibilidad con la versión preliminar de los registros dinámicos antes de que se introdujeran los registros de eventos en directo, ejecute el comando `kubectl apply -f LogReaderRBAC.yaml` para actualizar la configuración.

## <a name="configure-aks-with-azure-active-directory"></a>Configuración de AKS con Azure Active Directory

Es posible configurar AKS para que utilice Azure Active Directory (AD) para la autenticación de usuarios. Si lo va a configurar por primera vez, consulte [Integración de Azure Active Directory con Azure Kubernetes Service](../../aks/azure-ad-integration.md). Durante los pasos para crear la [aplicación cliente](../../aks/azure-ad-integration.md#create-the-client-application), especifique lo siguiente:

-  **URI de redirección**: Deben crearse dos tipos de aplicaciones **web**. El primer valor de URL base debe ser `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` y el segundo, `https://monitoring.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`.
- Después de registrar la aplicación, desde la página **Información general**, seleccione **Autenticación** en el panel izquierdo. En la página **Autenticación**, en **Configuración avanzada**, conceda implícitamente **tokens de acceso** y **tokens de identidad** y, luego, guarde su cambios.

>[!NOTE]
>Si usa esta característica en la región Azure China, el primer valor de dirección URL base debe ser `https://afd.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html` y el segundo, `https://monitoring.hosting.azureportal.chinaloudapi.cn/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`.

>[!NOTE]
>La configuración de la autenticación con Azure Active Directory para el inicio de sesión único solo puede lograrse durante la implementación inicial de un nuevo clúster de AKS. No puede configurar el inicio de sesión único en un clúster de AKS ya implementado.
  
>[!IMPORTANT]
>Si se vuelve a configurar Azure AD para la autenticación de usuarios con el URI actualizado, borre la caché del explorador para garantizar que se descarga y aplica el token de autenticación actualizado.   

## <a name="view-live-logs-and-events"></a>Visualización de registros y eventos en vivo

Puede ver eventos de registro en tiempo real cuando se generan mediante el motor de contenedores en la vista **Nodos**, **Controladores** y **Contenedores**. En el panel de propiedades, seleccione **View live data (preview)** (Ver datos activos [versión preliminar]) y aparecerá un panel debajo de la tabla de datos de rendimiento donde puede ver el registro y los eventos en una secuencia continua.

![Opción de visualización de registros en vivo del panel de propiedades del nodo](./media/container-insights-live-logs/node-properties-live-logs-01.png)  

Los mensajes de registros y eventos están limitados según el tipo de recurso seleccionado en la vista.

| Ver | Tipo de recurso | Registro o evento | Datos presentados |
|------|---------------|--------------|----------------|
| Nodos | Nodo | Evento | Cuando se selecciona un nodo, los eventos no se filtran y muestran los eventos de Kubernetes de todo el clúster. El título del panel muestra el nombre del clúster. |
| Nodos | Pod | Evento | Cuando se selecciona un pod, se filtran los eventos por su espacio de nombres. El título del panel muestra el espacio de nombres del pod. | 
| Controladores | Pod | Evento | Cuando se selecciona un pod, se filtran los eventos por su espacio de nombres. El título del panel muestra el espacio de nombres del pod. |
| Controladores | Controller | Evento | Cuando se selecciona un controlador, los eventos se filtran por su espacio de nombres. El título del panel muestra el espacio de nombres del controlador. |
| Nodos, controladores o contenedores | Contenedor | Registros | El título del panel muestra el nombre del pod con el que se agrupa el contenedor. |

Si el clúster de AKS se configura con inicio de sesión único mediante AAD, deberá autenticarse en el primer uso durante esa sesión del explorador. Seleccione la cuenta y complete la autenticación con Azure.  

Después de autenticarse correctamente, el panel de registros dinámicos aparecerá en la sección inferior del panel central. Si el indicador de estado de captura muestra una marca de verificación verde, que se encuentra en el extremo derecho del panel, significa que puede recuperar los datos.
    
  ![Datos recuperados del panel de registros dinámicos](./media/container-insights-live-logs/live-logs-pane-01.png)  

En la barra de búsqueda, puede filtrar por palabra clave para resaltar ese texto en el registro o el evento, y en el extremo derecho, se muestra cuántos resultados coinciden con el filtro.

  ![Ejemplo de filtro del panel de registros dinámicos](./media/container-insights-live-logs/live-logs-pane-filter-example-01.png)

Durante la visualización de eventos, puede limitar además los resultados mediante la cápsula **Filtro** que se encuentra a la derecha de la barra de búsqueda. Dependiendo de qué recurso haya seleccionado, la cápsula muestra un pod, un espacio de nombres o un clúster para elegir.  

Para suspender el desplazamiento automático y controlar el comportamiento del panel de forma que pueda desplazarse manualmente por lo nuevos datos leídos, haga clic en la opción **Desplazar**. Para volver a habilitar el desplazamiento automático, vuelva a hacer clic en la opción **Desplazar**. También puede pausar la recuperación de datos de registros o eventos si hace clic en la opción **Pausar**; cuando esté listo para reanudar, simplemente haga clic en **Reproducir**.  

![Vista activa de pausa en el panel de registros dinámicos](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

Puede ir a los registros de Azure Monitor para ver los registros de contenedor históricos; para ello, seleccione **Ver registros del contenedor** en la lista desplegable **Ver en Analytics**.

## <a name="next-steps"></a>Pasos siguientes

- Para continuar aprendiendo a usar Azure Monitor y supervisar otros aspectos de su clúster de AKS, consulte [Comprender el rendimiento del clúster de AKS con Azure Monitor para contenedores](container-insights-analyze.md).

- En los [ejemplos de consultas de registro](container-insights-log-search.md#search-logs-to-analyze-data) encontrará consultas predefinidas y ejemplos para evaluar o personalizar las alertas, la visualización o el análisis de los clústeres.
