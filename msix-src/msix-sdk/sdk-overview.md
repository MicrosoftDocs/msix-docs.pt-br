---
title: MSIX SDK
description: MSIX SDK é um projeto de código-fonte aberto que permite aos desenvolvedores usar o formato de pacote MSIX universalmente em todas as plataformas.
author: mcleanbyron
ms.author: mcleans
ms.date: 09/07/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e34bccadc54960ce7a55d62b2e1c999759fecc7d
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900298"
---
# <a name="msix-sdk"></a>MSIX SDK 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://github.com/Microsoft/msix-packaging" data-linktype="external">MSIX SDK no GitHub</a></p></div>

O SDK de MSIX é um projeto de código-fonte aberto que permite aos desenvolvedores usar o formato de pacote MSIX universalmente em todas as plataformas. Isso permite que os desenvolvedores criem experiências consistentes para seus usuários em todas as plataformas e distribuir as experiências usando o mesmo pacote. O SDK fornece orientação para desenvolvedores empacotar seu conteúdo de aplicativo e criar um manifesto de pacote do aplicativo de forma que ele pode direcionar as plataformas de sua preferência. Isso permite que os desenvolvedores empacotar seu conteúdo de aplicativo em vez de ter uma vez para o pacote para cada plataforma.

O SDK fornece que as APIs necessárias para verificar, validar e descompacte o conteúdo do pacote do pacote MSIX. Usando o projeto, os desenvolvedores de aplicativos não precisam se preocupar sobre se o pacote foi violado ou se ele pode ser confiável. Ele executará verificações de validação de assinatura e de proteção de adulteração antes do conteúdo do aplicativo é descompactado.

O SDK pode ser usado por qualquer aplicativo de cliente de plataforma cruzada que permite que terceiros criem plug-ins ou extensões. Os desenvolvedores de aplicativo cliente podem usar o modelo de extensão de aplicativo que está disponível na plataforma Windows 10 e use o SDK de MSIX nas plataformas não Windows 10. Com a Ajuda do SDK, os desenvolvedores de terceiros Criando extensões de aplicativo e plug-ins para o aplicativo cliente não é necessário que criar um pacote específico para cada plataforma. Em vez disso, eles criam um pacote que tem suporte no Windows 10 e todas as outras plataformas. Com o SDK, os desenvolvedores de aplicativos podem escolher as plataformas específicas para dar suporte.

Um dos principais diferenciais do pacote MSIX é o arquivo de manifesto. O arquivo de manifesto contém todos os metadados sobre o pacote e especifica todas as informações de chave que o aplicativo cliente pode acessar para fazer escolhas apropriadas como aplicabilidade ou capacidade de suporte. O arquivo de manifesto permite que os desenvolvedores de aplicativos de cliente e os desenvolvedores de terceiros mais opções e flexibilidade para se comunicar características como requisitos, disponibilidade e suporte.

## <a name="get-more-info"></a>Obter mais informações

MSIX SDK é um [projeto de software livre no GitHub](https://github.com/Microsoft/msix-packaging). O repositório GitHub inclui o código-fonte completo e instruções sobre como criar os binários para cada plataforma.

Se você tiver comentários, envie-na [site da comunidade técnica MSIX](https://techcommunity.microsoft.com/t5/MSIX/ct-p/MSIX). Se houver problemas ou bugs identificados no SDK, você pode postá-los para o [página questões](https://github.com/Microsoft/msix-packaging/issues) para o repositório do GitHub do SDK MSIX.