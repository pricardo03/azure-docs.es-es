---
title: Información sobre cómo realizar auditorías en una máquina virtual
description: Obtenga información sobre cómo Azure Policy usa Guest Configuration para auditar la configuración dentro de una máquina virtual de Azure.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/29/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 77d99c90e65647a1f4a4efb07ff5520596fa54cf
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55295175"
---
# <a name="understand-azure-policys-guest-configuration"></a>Información sobre Guest Configuration de Azure Policy

Además de los recursos de auditoría y [corrección](../how-to/remediate-resources.md) de Azure, Azure Policy es capaz de auditar la configuración en una máquina virtual. La validación se realiza mediante el cliente y la extensión Guest Configuration. La extensión, a través del cliente, valida las distintas opciones de configuración, como la configuración del sistema operativo, la presencia o configuración de la aplicación, la configuración del entorno y mucho más.

> [!IMPORTANT]
> Actualmente, Guest Configuration solo admite las directivas **integradas**.

## <a name="extension-and-client"></a>Extensión y cliente

Para auditar la configuración dentro de una máquina virtual, se habilita una [extensión de máquina virtual](../../../virtual-machines/extensions/overview.md). La extensión descarga la asignación de directiva aplicable y la definición de configuración correspondiente.

### <a name="register-guest-configuration-resource-provider"></a>Registrar proveedor de recursos de Guest Configuration

Para poder usar Guest Configuration debe registrar el proveedor de recursos. Puede registrarse mediante el portal o a través de PowerShell.

#### <a name="registration---portal"></a>Registro: Portal

Para registrar el proveedor de recursos para Guest Configuration a través de Azure Portal, siga estos pasos:

1. Inicie Azure Portal y haga clic en **Todos los servicios**. Busque y seleccione **Suscripciones**.

1. Busque y haga clic en la suscripción que desea habilitar para Guest Configuration.

1. En el menú izquierdo de la página **Suscripción**, haga clic en **Proveedores de recursos**.

1. Filtre o desplácese hasta que encuentre **Microsoft.GuestConfiguration**, después haga clic en **Registrar** en la misma fila.

#### <a name="registration---powershell"></a>Registro: PowerShell

Para registrar el proveedor de recursos para Guest Configuration a través de PowerShell, ejecute el siguiente comando:

```azurepowershell-interactive
# Login first with Connect-AzAccount if not using Cloud Shell
Register-AzResourceProvider -ProviderNamespace 'Microsoft.GuestConfiguration'
```

### <a name="validation-tools"></a>Herramientas de validación

Dentro de la máquina virtual, el cliente de Guest Configuration usa herramientas locales para ejecutar la auditoría.

En la tabla siguiente se muestra una lista herramienta locales usada en cada sistemas operativos compatibles:

|Sistema operativo|Herramienta de validación|Notas|
|-|-|-|
| Windows|[Microsoft Desired State Configuration](/powershell/dsc) v2| |
|Linux|[Chef InSpec](https://www.chef.io/inspec/)| La extensión Guest Configuration instala Ruby y Python. |

### <a name="validation-frequency"></a>Frecuencia de validación

El cliente de Guest Configuration busca contenido nuevo cada 5 minutos.
Una vez que se recibe una asignación de invitado, se comprueban los valores en un intervalo de 15 minutos.
Los resultados se envían al proveedor de recursos de Guest Configuration tan pronto como finaliza la auditoría.
Cuando se produce una directiva del tipo [desencadenador evaluación](../how-to/get-compliance-data.md#evaluation-triggers), el estado de la máquina se escribe en el proveedor de recursos de Guest Configuration.
Esto hace que Azure Policy evalúe las propiedades de Azure Resource Manager.
Una evaluación de Policy a petición recupera el valor más reciente del proveedor de recursos de Guest Configuration.
Sin embargo, no desencadena una nueva auditoría de la configuración en la máquina virtual.

### <a name="supported-client-types"></a>Tipos de cliente admitidos

En la tabla siguiente se muestra una lista de sistemas operativos compatibles en imágenes de Azure:

|Publicador|NOMBRE|Versiones|
|-|-|-|
|Canonical|Ubuntu Server|14.04, 16.04, 18.04|
|Credativ|Debian|8, 9|
|Microsoft|Windows Server|2012 Datacenter, 2012 R2 Datacenter, 2016 Datacenter|
|OpenLogic|CentOS|7.3, 7.4, 7.5|
|Red Hat|Red Hat Enterprise Linux|7.4, 7.5|
|Suse|SLES|12 SP3|

> [!IMPORTANT]
> Guest Configuration no es compatible actualmente con las imágenes de máquinas virtuales personalizadas.

### <a name="unsupported-client-types"></a>Tipos de cliente no admitidos

En la tabla siguiente se enumeran los sistemas operativos no admitidos:

|Sistema operativo|Notas|
|-|-|
|Cliente Windows | No se admiten sistemas operativos cliente (por ejemplo, Windows 7 y Windows 10).
|Windows Server 2016 Nano Server | No compatible.|

## <a name="guest-configuration-definition-requirements"></a>Requisitos de definición de Guest Configuration

Cada auditoría ejecutada por Guest Configuration requiere dos definiciones de directiva, **DeployIfNotExists** y **Audit**. **DeployIfNotExists** se utiliza para preparar la máquina virtual con el agente de Guest Configuration y otros componentes para admitir las [herramientas de validación](#validation-tools).

La definición de directiva **DeployIfNotExists** valida y corrige los siguientes elementos:

- Comprueba que se ha asignado una configuración a la máquina virtual para evaluar. Si actualmente no hay ninguna asignación, obtiene la asignación y prepara la máquina virtual:
  - Autenticando la máquina virtual con una [identidad administrada](../../../active-directory/managed-identities-azure-resources/overview.md)
  - Instalando la versión más reciente de la extensión **Microsoft.GuestConfiguration**
  - Instalando las [herramientas de validación](#validation-tools) y dependencias, si es necesario

Una vez que **DeployIfNotExists** es compatible, la definición de directiva **Audit** usa las herramientas de validación local para determinar si la asignación de configuración asignada es compatible o no compatible. La herramienta de validación proporciona los resultados al cliente de Guest Configuration. El cliente envía los resultados a la extensión de Guest, que hace que estén disponibles a través del proveedor de recursos de Guest Configuration.

Azure Policy usa la propiedad **complianceStatus** de los proveedores de recursos de Guest Configuration para notificar el cumplimiento en el nodo **Compliance**. Para más información, vea [Obtención de datos de cumplimiento](../how-to/getting-compliance-data.md).

> [!NOTE]
> Para cada definición de Guest Configuration, deben existir las definiciones de directiva **DeployIfNotExists** y **Audit**.

Se incluyen todas las directivas integradas para Guest Configuration en una iniciativa para agrupar las definiciones para su uso en las asignaciones. La iniciativa integrada denominada *[Versión preliminar]: Configuración de seguridad de la contraseña de la auditoría dentro de máquinas virtuales Linux y Windows* contiene 18 directivas. Hay seis pares **DeployIfNotExists** y **Audit** para Windows y tres pares para Linux. En cada caso, la lógica dentro de la definición valida que solo el sistema operativo de destino se evalúa según definición de la [regla de directiva](definition-structure.md#policy-rule).

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md)
- Consulte la [Estructura de definición de directivas](definition-structure.md)
- Consulte [Descripción de los efectos de directivas](effects.md)
- Entender cómo se pueden [crear directivas mediante programación](../how-to/programmatically-create.md)
- Obtenga información sobre cómo [obtener datos de cumplimiento](../how-to/getting-compliance-data.md)
- Más información sobre cómo [corregir recursos no compatibles](../how-to/remediate-resources.md)
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/index.md), obtendrá información sobre lo que es un grupo de administración.
