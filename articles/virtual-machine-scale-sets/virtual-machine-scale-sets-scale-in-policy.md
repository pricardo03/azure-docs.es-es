---
title: Uso de directivas personalizadas de reducción horizontal con conjuntos de escalado de máquinas virtuales de Azure
description: Aprenda a usar directivas de reducción horizontal personalizadas con conjuntos de escalado de máquinas virtuales de Azure que utilizan la configuración de escalabilidad automática para administrar el recuento de instancias
author: avverma
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: avverma
ms.openlocfilehash: 8e51ebab36d75d1c9512446ee0370f7359a72551
ms.sourcegitcommit: 5397b08426da7f05d8aa2e5f465b71b97a75550b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2020
ms.locfileid: "76271760"
---
# <a name="preview-use-custom-scale-in-policies-with-azure-virtual-machine-scale-sets"></a>Vista previa: Uso de directivas personalizadas de reducción horizontal con conjuntos de escalado de máquinas virtuales de Azure

Una implementación de conjuntos de escalado de máquinas virtuales se puede escalar horizontalmente o reducir horizontalmente en función de una matriz de métricas, incluidas las métricas personalizadas de la plataforma y las definidas por el usuario. Mientras que una escalabilidad horizontal crea nuevas máquinas virtuales basadas en el modelo de conjuntos de escalado, una reducción horizontal afecta a las máquinas virtuales en ejecución que pueden tener diferentes configuraciones y/o funciones a medida que evoluciona la carga de trabajo del conjunto de escalado. 

La característica de la directiva de reducción horizontal proporciona a los usuarios una manera de configurar el orden en el que se reducen horizontalmente las máquinas virtuales. La versión preliminar presenta tres configuraciones de reducción horizontal: 

1. Valor predeterminado
2. NewestVM
3. OldestVM

***Esta característica en vista previa (GB) se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción.***

### <a name="default-scale-in-policy"></a>Directiva predeterminada de reducción horizontal

De forma predeterminada, el conjunto de escalado de máquinas virtuales aplica esta directiva para determinar qué instancias se van a reducir horizontalmente. Con la directiva *Predeterminada*, se seleccionan las máquinas virtuales para la reducción horizontal en el orden siguiente:

1. Equilibrar máquinas virtuales entre zonas de disponibilidad (si el conjunto de escalado se implementa en una configuración de zona).
2. Equilibrar máquinas virtuales entre dominios de error (mejor opción).
3. Eliminar la máquina virtual con el identificador de instancia más alto.

No es necesario que los usuarios especifiquen una directiva de reducción horizontal si solo quieren seguir el orden predeterminado.

Tenga en cuenta que al equilibrar entre las zonas de disponibilidad o los dominios de error, no mueven las instancias entre las zonas de disponibilidad o los dominios de error. El equilibrio se logra mediante la eliminación de máquinas virtuales de las zonas de disponibilidad o los dominios de error desequilibrados hasta que se equilibra la distribución de máquinas virtuales.

### <a name="newestvm-scale-in-policy"></a>Directiva NewestVM de reducción horizontal

Esta directiva eliminará la máquina virtual creada más reciente en el conjunto de escalado, después de equilibrar las máquinas virtuales entre las zonas de disponibilidad (para implementaciones de zonas). La habilitación de esta directiva requiere un cambio de configuración en el modelo de conjuntos de escalado de máquinas virtuales.

### <a name="oldestvm-scale-in-policy"></a>Directiva OldestVM de reducción horizontal

Esta directiva eliminará la máquina virtual creada más antigua en el conjunto de escalado, después de equilibrar las máquinas virtuales entre las zonas de disponibilidad (para implementaciones de zonas). La habilitación de esta directiva requiere un cambio de configuración en el modelo de conjuntos de escalado de máquinas virtuales.

## <a name="enabling-scale-in-policy"></a>Habilitación de la directiva de reducción horizontal

Una directiva de reducción horizontal se define en el modelo de conjuntos de escalado de máquinas virtuales. Como se ha indicado en las secciones anteriores, se necesita una definición de directiva de reducción horizontal cuando se usan las directivas "NewestVM" y "OldestVM". El conjunto de escalado de máquinas virtuales usará automáticamente la directiva de reducción horizontal "Predeterminada" si no hay ninguna definición de directiva de reducción horizontal en el modelo de conjuntos de escalado. 

Se puede definir una directiva de reducción horizontal en el modelo de conjuntos de escalado de máquinas virtuales de las siguientes maneras:

### <a name="using-api"></a>Uso de la API

Ejecute PUT en el conjunto de escalado de máquinas virtuales mediante la API 2019-03-01:

```
PUT
https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<myRG>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVMSS>?api-version=2019-03-01

{ 
"location": "<VMSS location>", 
    "properties": { 
        "scaleInPolicy": {  
            "rules": ["OldestVM"]  
        } 
    }    
} 
```

### <a name="using-template"></a>Uso de una plantilla

En la plantilla, en "properties", agregue lo siguiente:

```json
"scaleInPolicy": {  
      "rules": ["OldestVM"]  
}
```

Los bloques anteriores especifican que el conjunto de escalado de máquinas virtuales eliminará la máquina virtual más antigua en un conjunto de escalado con equilibrio de zona, cuando se desencadene una reducción horizontal (mediante el escalado automático o la eliminación manual).

Cuando un conjunto de escalado de máquinas virtuales no tenga equilibrio de zona, el conjunto de escalado eliminará primero las máquinas virtuales en las zonas desequilibradas. Dentro de las zonas desequilibradas, el conjunto de escalado usará la directiva de reducción horizontal especificada anteriormente para determinar qué máquina virtual se va a reducir horizontalmente. En este caso, dentro de una zona desequilibrada, el conjunto de escalado seleccionará la máquina virtual más antigua de esa zona para su eliminación.

En el caso de un conjunto de escalado de máquinas virtuales no de zona, la directiva selecciona la máquina virtual más antigua del conjunto de escalado para su eliminación.

El mismo proceso se aplica cuando se usa "NewestVM" en la directiva de reducción horizontal anterior.

## <a name="modifying-scale-in-policies"></a>Modificación de directivas de reducción horizontal

La modificación de la directiva de reducción horizontal sigue el mismo proceso que la aplicación de la directiva de reducción horizontal. Por ejemplo, si en el ejemplo anterior desea cambiar la directiva de "OldestVM" a "NewestVM", puede hacerlo de la siguiente manera:

### <a name="using-api"></a>Uso de la API

Ejecute PUT en el conjunto de escalado de máquinas virtuales mediante la API 2019-03-01:

```
PUT
https://management.azure.com/subscriptions/<sub-id>/resourceGroups/<myRG>/providers/Microsoft.Compute/virtualMachineScaleSets/<myVMSS>?api-version=2019-03-01 

{ 
"location": "<VMSS location>", 
    "properties": { 
        "scaleInPolicy": {  
            "rules": ["NewestVM"]  
        } 
    }    
}
```

### <a name="using-template"></a>Uso de una plantilla

En la plantilla, en "properties", modifique la plantilla como se indica a continuación y vuelva a implementarla: 

```json
"scaleInPolicy": {  
      "rules": ["NewestVM"]  
} 
```

Se aplicará el mismo proceso si decide cambiar "NewestVM" por "Default" u "OldestVM".

## <a name="instance-protection-and-scale-in-policy"></a>Protección de instancias y directiva de reducción horizontal

Los conjuntos de escalado de máquinas virtuales proporcionan dos tipos de [protección de instancias](./virtual-machine-scale-sets-instance-protection.md#types-of-instance-protection):

1. Protección contra la reducción horizontal
2. Protección contra las acciones del conjunto de escalado

Una máquina virtual protegida no se elimina mediante una acción de reducción horizontal, sea cual sea la directiva de reducción horizontal aplicada. Por ejemplo, si VM_0 (la máquina virtual más antigua en el conjunto de escalado) está protegido contra la reducción horizontal y el conjunto de escalado tiene habilitada la directiva de reducción horizontal "OldestVM", VM_0 no se tendrá en cuenta para la reducción horizontal, aunque sea la máquina virtual más antigua del conjunto de escalado. 

El usuario puede eliminar manualmente una máquina virtual protegida en cualquier momento, sea cual sea la directiva de reducción horizontal habilitada en el conjunto de escalado. 

## <a name="usage-examples"></a>Ejemplos de uso 

En los siguientes ejemplos se muestra cómo un conjunto de escalado de máquinas virtuales seleccionará las máquinas virtuales que se van a eliminar cuando se desencadene un evento de reducción horizontal. Se supone que las máquinas virtuales con los identificadores de instancia más altos son las máquinas virtuales más recientes del conjunto de escalado y que las máquinas virtuales con los identificadores de instancia más bajos son las máquinas virtuales más antiguas del conjunto de escalado. 

### <a name="oldestvm-scale-in-policy"></a>Directiva OldestVM de reducción horizontal

| Evento                 | Id. de instancia en Zona 1  | Id. de instancia en Zona 2  | Id. de instancia en Zona 3  | Selección de reducción horizontal                                                                                                               |
|-----------------------|------------------------|------------------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Initial               | 3, 4, 5, 10            | 2, 6, 9, 11            | 1, 7, 8                |                                                                                                                                  |
| Reducción horizontal              | 3, 4, 5, 10            | ***2***, 6, 9, 11      | 1, 7, 8                | Elija entre Zona 1 y 2, aunque Zona 3 tenga la máquina virtual más antigua. Elimine VM2 de Zona 2, ya que es la máquina virtual más antigua de esa zona.   |
| Reducción horizontal              | ***3***, 4, 5, 10      | 6, 9, 11               | 1, 7, 8                | Elija Zona 1, aunque Zona 3 tenga la máquina virtual más antigua. Elimine VM3 de Zona 1, ya que es la máquina virtual más antigua de esa zona.                  |
| Reducción horizontal              | 4, 5, 10               | 6, 9, 11               | ***1***, 7, 8          | Las zonas están equilibradas. Elimine VM1 en Zona 3, ya que es la máquina virtual más antigua del conjunto de escalado.                                               |
| Reducción horizontal              | ***4***, 5, 10         | 6, 9, 11               | 7, 8                   | Elija entre Zona 1 y Zona 2. Elimine VM4 en Zona 1, ya que es la máquina virtual más antigua de las dos zonas.                              |
| Reducción horizontal              | 5, 10                  | ***6***, 9, 11         | 7, 8                   | Elija Zona 2, aunque Zona 1 tenga la máquina virtual más antigua. Elimine VM6 en Zona 1, ya que es la máquina virtual más antigua de esa zona.                    |
| Reducción horizontal              | ***5***, 10            | 9, 11                  | 7, 8                   | Las zonas están equilibradas. Elimine VM5 en Zona 1, ya que es la máquina virtual más antigua del conjunto de escalado.                                                |

En el caso de los conjuntos de escalado de máquinas virtuales no de zona, la directiva selecciona la máquina virtual más antigua del conjunto de escalado para su eliminación. Se omitirá la eliminación de cualquier máquina virtual "protegida".

### <a name="newestvm-scale-in-policy"></a>Directiva NewestVM de reducción horizontal

| Evento                 | Id. de instancia en Zona 1  | Id. de instancia en Zona 2  | Id. de instancia en Zona 3  | Selección de reducción horizontal                                                                                                               |
|-----------------------|------------------------|------------------------|------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Initial               | 3, 4, 5, 10            | 2, 6, 9, 11            | 1, 7, 8                |                                                                                                                                  |
| Reducción horizontal              | 3, 4, 5, 10            | 2, 6, 9, ***11***      | 1, 7, 8                | Elija entre Zona 1 y 2. Elimine VM11 de Zona 2, ya que es la máquina virtual más reciente de las dos zonas.                                |
| Reducción horizontal              | 3, 4, 5, ***10***      | 2, 6, 9                | 1, 7, 8                | Elija Zona 1, ya que tiene más máquinas virtuales que las otras dos zonas. Elimine VM10 de Zona 1, ya que es la máquina virtual más reciente de esa zona.          |
| Reducción horizontal              | 3, 4, 5                | 2, 6, ***9***          | 1, 7, 8                | Las zonas están equilibradas. Elimine VM9 en Zona 2, ya que es la máquina virtual más reciente del conjunto de escalado.                                                |
| Reducción horizontal              | 3, 4, 5                | 2, 6                   | 1, 7, ***8***          | Elija entre Zona 1 y Zona 3. Elimine VM8 en Zona 3, ya que es la máquina virtual más reciente de esa zona.                                      |
| Reducción horizontal              | 3, 4, ***5***          | 2, 6                   | 1, 7                   | Elija Zona 1, aunque Zona 3 tenga la máquina virtual más reciente. Elimine VM5 en Zona 1, ya que es la máquina virtual más reciente de esa zona.                    |
| Reducción horizontal              | 3, 4                   | 2, 6                   | 1, ***7***             | Las zonas están equilibradas. Elimine VM7 en Zona 3, ya que es la máquina virtual más reciente del conjunto de escalado.                                                |

En el caso de los conjuntos de escalado de máquinas virtuales no de zona, la directiva selecciona la máquina virtual más reciente del conjunto de escalado para su eliminación. Se omitirá la eliminación de cualquier máquina virtual "protegida". 

## <a name="troubleshoot"></a>Solución de problemas

1. Error al habilitar scaleInPolicy: si recibe un error de "BadRequest" con un mensaje de error que indica que "no se pudo encontrar el miembro 'scaleInPolicy' en el objeto de tipo 'properties'", compruebe la versión de API usada para el conjunto de escalado de máquinas virtuales. Se requiere la versión 2019-03-01 de la API o una posterior para esta versión preliminar.

2. Selección incorrecta de las máquinas virtuales para la reducción horizontal: consulte los ejemplos anteriores. Si el conjunto de escalado de máquinas virtuales es una implementación de zona, la directiva de reducción horizontal se aplica primero a las zonas desequilibradas y, luego, al conjunto de escalado una vez que logre el equilibrio de zona. Si el orden de reducción horizontal no es coherente con los ejemplos anteriores, genere una consulta para el equipo de conjuntos de escalado de máquinas virtuales a fin de resolver el problema.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo [implementar la aplicación](virtual-machine-scale-sets-deploy-app.md) en conjuntos de escalado de máquinas virtuales.
