---
title: Notas de versão MSIX SDK versão 1.6
description: Notas de versão do MSIX SDK versão 1.6
author: c-don
ms.author: cdon
ms.date: 02/06/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: b964c8942d3da85d85cbff87f78ef8c9399ab142
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58900318"
---
# <a name="msix-sdk-16-update"></a>Atualização de MSIX SDK 1.6

Com o lançamento do SDK (1.6), podemos ouviu os comentários de nossos parceiros e adicionados mais APIs para fornecer aos desenvolvedores mais opções e flexibilidade no tratamento de pacotes MSIX. 

## <a name="utf8-api-variants"></a>UTF8 Variantes de API

Nesta versão do SDK, podemos adicionar aproximadamente 14 novas variantes de API UTF8 para chamadas de API existentes. Com a inclusão dessas novas APIs, desenvolvedores podem optar por usar a variante Utf8 para manipulação de cadeia de caracteres de acordo com seu ambiente/plataforma. Assim como acontece com APIs AppxPackaging, o chamador é responsável por desalocando a memória usada pelo LPSTR * parâmetros de saída.

Estas são as novas interfaces UTF8:
- IAppxBlockMapFileUtf8
- IAppxBlockMapReaderUtf8
- IAppxBundleManifestPackageInfoUtf8
- IAppxBundleReaderUtf8
- IAppxFactoryUtf8
- IAppxFileUtf8
- IAppxManifestApplicationUtf8
- IAppxManifestPackageDependencyUtf8
- IAppxManifestPackageIdUtf8
- IAppxManifestPropertiesUtf8
- IAppxManifestQualifiedResourceUtf8
- IAppxManifestResourcesEnumeratorUtf8
- IAppxManifestTargetDeviceFamilyUtf8
- IAppxPackageReaderUtf8


## <a name="override-language-selection"></a>Substituir seleção de idioma 

Por padrão, ao lidar com pacotes de aplicativos, o SDK MSIX retorna o pacote de idiomas é aplicável, selecionando o idioma que também é padrão no sistema. Essa API permite que o aplicativo enumerar os pacotes de idiomas que estão disponíveis e substituem o pacote de idiomas que será retornado ao manipular pacotes de aplicativos. 

## <a name="other-updates-and-improvements"></a>Outras atualizações e aprimoramentos

Nesta atualização, 
- Atualizar a dependência de lib do OpenSSL para 1.0.2q
- Corrigido como lidar com caracteres internacionais 

Você pode obter o SDK mais recente no GitHub. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.6" data-linktype="external">MSIX SDK no GitHub</a></p></div>

