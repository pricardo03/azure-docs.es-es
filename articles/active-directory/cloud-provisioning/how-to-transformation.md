---
title: Transformaciones del aprovisionamiento en la nube de Azure AD Connect
description: En este documento se describe cómo usar transformaciones para modificar las asignaciones de atributos predeterminadas.
author: billmath
ms.author: billmath
manager: davba
ms.date: 12/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0d37fdb4ad0d385914aecd4ca62be498c5c0e7c5
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2019
ms.locfileid: "74795523"
---
# <a name="transformations"></a>Transformaciones

Una transformación permite cambiar el comportamiento predeterminado del modo de sincronización de un atributo con Azure AD mediante el aprovisionamiento en la nube.  

Para realizar esta tarea, debe modificar el esquema y volver a enviarlo a través de una solicitud web.

Para más información sobre los atributos de aprovisionamiento en la nube, consulte [Descripción del esquema de Azure AD](concept-attributes.md).


## <a name="retrieve-the-schema"></a>Recuperación del esquema
Para recuperar el esquema, siga los pasos descritos en [Visualización del esquema](concept-attributes.md#viewing-the-schema). 


## <a name="custom-attribute-mapping"></a>Asignación de atributos personalizados
Para agregar una asignación de atributos personalizados, siga el procedimiento siguiente.

1. Copie el esquema en un editor de texto o de código como [Visual Studio Code](https://code.visualstudio.com/).  
2. Busque el objeto que desea actualizar en el esquema ![](media/how-to-transformation/transform1.png)</br>
3. Busque el código de **ExtensionAttribute3** en el objeto del usuario.

    ```
                            {
                                "defaultValue": null,
                                "exportMissingReferences": false,
                                "flowBehavior": "FlowWhenChanged",
                                "flowType": "Always",
                                "matchingPriority": 0,
                                "targetAttributeName": "ExtensionAttribute3",
                                "source": {
                                    "expression": "Trim([extensionAttribute3])",
                                    "name": "Trim",
                                    "type": "Function",
                                    "parameters": [
                                        {
                                            "key": "source",
                                            "value": {
                                                "expression": "[extensionAttribute3]",
                                                "name": "extensionAttribute3",
                                                "type": "Attribute",
                                                "parameters": []
                                            }
                                        }
                                    ]
                                }
                            },
    ```
 4.  Edite el código para que el atributo Company se asigne a ExtensionAttribute3.
    ```
                            {
                                "defaultValue": null,
                                "exportMissingReferences": false,
                                "flowBehavior": "FlowWhenChanged",
                                "flowType": "Always",
                                "matchingPriority": 0,
                                "targetAttributeName": "ExtensionAttribute3",
                                "source": {
                                    "expression": "Trim([company])",
                                    "name": "Trim",
                                    "type": "Function",
                                    "parameters": [
                                        {
                                            "key": "source",
                                            "value": {
                                                "expression": "[company]",
                                                "name": "company",
                                                "type": "Attribute",
                                                "parameters": []
                                            }
                                        }
                                    ]
                                }
                            },
    ```
 5. Vuelva a copiar el esquema en el Probador de Graph, cambie el tipo de solicitud a PUT y **ejecute la consulta**.  
 ![](media/how-to-transformation/transform2.png)</br>
 6.  Ahora, en Azure Portal, vaya a la configuración del aprovisionamiento en la nube y **reinicie el aprovisionamiento**.
 ![](media/how-to-transformation/transform3.png)</br>
 7.  Después de unos minutos, compruebe que los atributos se están rellenando; para ello, ejecute la siguiente consulta en el Probador de Graph: `https://graph.microsoft.com/beta/users/{Azure AD user UPN}`.
 8.  Ahora debería ver el valor.
 ![](media/how-to-transformation/transform4.png)</br>

## <a name="custom-attribute-mapping-with-function"></a>Asignación de atributos personalizados con una función
Si desea una asignación más avanzada, puede utilizar funciones que le permitirán tratar los datos y crear valores para el atributo que se adapten a las necesidades de la organización.

Para realizar esta tarea, solo tiene que seguir los pasos anteriores y, a continuación, editar la función que se usa para construir el valor final.

Para más información sobre la sintaxis y ejemplos de expresiones, consulte [Escritura de expresiones para la asignación de atributos en Azure Active Directory](reference-expressions.md).


## <a name="next-steps"></a>Pasos siguientes 

- [¿Qué es el aprovisionamiento?](what-is-provisioning.md)
- [¿Qué es el aprovisionamiento en la nube de Azure AD Connect?](what-is-cloud-provisioning.md)
