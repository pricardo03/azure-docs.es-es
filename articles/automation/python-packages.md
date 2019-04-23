---
title: Administrar paquetes de Python 2 en Azure Automation
description: En este artículo se describe cómo administrar paquetes de Python 2 en Azure Automation.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/25/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: b53e07d6086f2a02fd1bbd158ffc09dc95b0c377
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59796386"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Administrar paquetes de Python 2 en Azure Automation

Azure Automation le permite ejecutar runbooks de Python 2 en Azure y en Hybrid Runbook Worker de Linux. Para simplificar los runbooks, puede usar los paquetes de Python para importar los módulos que necesite. En este artículo se describe cómo administrar y usar paquetes de Python en Azure Automation.

## <a name="import-packages"></a>Importar paquetes

En la cuenta de Automation, seleccione **Python 2 packages** (Paquetes de Python 2) en **Recursos compartidos**. Haga clic en **+ Add a Python 2 package** (+ Agregar un paquete de Python 2).

![Agregar un paquete de Python](media/python-packages/add-python-package.png)

En la página **Agregar un paquete de Python 2**, seleccione un paquete local para cargar. El paquete puede ser un archivo `.whl` o `.tar.gz`. Cuando seleccione esta opción, haga clic en **Aceptar** para cargar el paquete.

![Agregar un paquete de Python](media/python-packages/upload-package.png)

Una vez que se ha importado un paquete, se muestra en el **paquetes de Python 2** página en su cuenta de Automation. Si necesita quitar un paquete, seleccione el paquete y elija **Eliminar** en la página del paquete.

![Lista de paquetes](media/python-packages/package-list.png)

## <a name="import-packages-with-dependencies"></a>Importar paquetes con dependencias

Azure automation no resuelve las dependencias de paquetes de python durante el proceso de importación. Hay dos formas de importar un paquete con todas sus dependencias. Solo uno de los pasos siguientes debe utilizarse para importar los paquetes en su cuenta de Automation.

### <a name="manually-download"></a>Descargar manualmente

En Windows 64 bits máquina con [python2.7](https://www.python.org/downloads/release/latest/python2) y [pip](https://pip.pypa.io/en/stable/) instalado, ejecute el siguiente comando para descargar un paquete y todas sus dependencias:

```cmd
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

Una vez que se descargan los paquetes, puede importarlos a su cuenta de automation.

### <a name="runbook"></a>Runbook

Importar el runbook de python [paquetes de Python 2 de importación desde pypi en la cuenta de Azure Automation](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509) desde la galería en su cuenta de Automation. Asegúrese de que se establecen los parámetros de ejecución en **Azure** e iniciar el runbook con los parámetros. El runbook requiere una cuenta de ejecución para la cuenta de Automation para que funcione. Para cada parámetro, asegúrese iniciarlo con el modificador tal como se muestra en la lista y la imagen siguiente:

* -s \<subscriptionId\>
* -g \<resourceGroup\>
* -a \<automationAccount\>
* -m \<modulePackage\>

![Lista de paquetes](media/python-packages/import-python-runbook.png)

El runbook le permite especificar qué paquete para descargar, por ejemplo, `Azure` (el cuarto parámetro) descargará todos los módulos de Azure y todas sus dependencias, que es aproximadamente 105.

Cuando se haya completado el runbook puede comprobar el **paquetes de Python 2** página **recursos compartidos** en su cuenta de Automation para comprobar que empaquetan se importó correctamente.

## <a name="use-a-package-in-a-runbook"></a>Usar un paquete en un runbook

Una vez que ha importado un paquete, ahora puede usar en un runbook. En el ejemplo siguiente se usa el [ paquete de utilidades de Azure Automation](https://github.com/azureautomation/azure_automation_utility). Este paquete facilita el uso de Python con Azure Automation. Para usar el paquete, siga las instrucciones del repositorio de GitHub y agréguelo al runbook mediante `from azure_automation_utility import get_automation_runas_credential`; por ejemplo, para importar la función para recuperar la cuenta de tipo RunAs.

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>Desarrollar y probar runbooks sin conexión

Para desarrollar y probar los runbooks de Python 2 sin conexión, puede usar el módulo [Azure Automation python emulated assets](https://github.com/azureautomation/python_emulated_assets) (Recursos emulados de Python de Azure Automation) en GitHub. Este módulo le permite hacer referencia a recursos compartidos como credenciales, variables, conexiones y certificados.

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con runbooks de Python 2, consulte [My first Python 2 runbook](automation-first-runbook-textual-python2.md) (Mi primer runbook de Python 2).
