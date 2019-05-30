---
title: Notas de versão MSIX SDK versão 1.4
description: Notas de versão do SDK MSIX versão 1.4
author: c-don
ms.author: cdon
ms.date: 09/12/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5a5e6663263300abbccbc303b2a3f67c6bfdcdfc
ms.sourcegitcommit: 67e56f5414857671c47334c65d636d531632b8f3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58900308"
---
# <a name="msix-sdk-14-update"></a>Atualização de MSIX SDK 1.4

Com o lançamento do SDK (1.4), continuamos a adicionar mais funcionalidade e fazer melhorias de desempenho em nosso SDK.  Com o lançamento do 1.4, os desenvolvedores podem usar o SDK agora descompactar e extrair pacotes de aplicativos e [simples pacotes](https://docs.microsoft.com/en-us/windows/uwp/packaging/flat-bundles?context=/windows/msix/render). 

Com o suporte de pacotes, aplicativos cliente podem agora extrair pacotes de aplicativos e apenas Baixe e extraia os pacotes dentro do pacote que são aplicáveis. Nesta versão, o suporte de pacote se estende aos pacotes simples também. Isso significa que o aplicativo cliente que consomem o pacote pode acessar o manifesto de pacote e especificar os pacotes de aplicativo que deseja extrair - deixando o escolha e controle para o desenvolvedor. O SDK chama o sistema operacional nativo para obter informações de idioma e localidade e fornece essas informações para o aplicativo cliente que ele pode usar para fazer a escolha do pacote apropriado do pacote também.

Com o novo SDK, adicionamos suporte para o MSXML6 para Windows que remove o fora de dependência de caixa e, portanto, reduz o tamanho do binário e usar a biblioteca nativa de XML. 

Junto com a remoção de dependência no Windows, também removemos a dependência zlib (biblioteca de terceiros 3ª) e usar implementações de caixa de entrada no Android(AOSP), iOS e MacOS.  Além disso, fizemos outras melhorias para reduzir o tamanho binário em todas as plataformas. 

Juntamente com os aprimoramentos de desempenho e recursos, também incluímos exemplos melhor que os desenvolvedores podem usar para começar com o SDK. Usando os exemplos, os desenvolvedores aprende como implementar algumas das interfaces públicas necessárias para ler os pacotes msix. 

Você pode obter o SDK mais recente no GitHub. 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging/tree/release_v1.4" data-linktype="external">MSIX SDK no GitHub</a></p></div>

