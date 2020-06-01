---
title: O que é MSIX?
description: Este artigo apresenta os conceitos básicos do formato de empacotamento MSIX, uma experiência moderna para todos os aplicativos do Windows.
ms.date: 12/02/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 2cd4de777c7fc45a62e511d76916d61a46d5492a
ms.sourcegitcommit: 7a52883434aa05272c15d033d85b67e2dd1e8c75
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84107334"
---
# <a name="what-is-msix"></a>O que é MSIX?

O MSIX é um formato de pacote de aplicativo do Windows que oferece uma experiência de empacotamento moderna para todos os aplicativos do Windows. O formato de pacote MSIX preserva a funcionalidade de pacotes de aplicativo existentes e/ou de arquivos de instalação, além de habilitar recursos de empacotamento e implantação novos e modernos para aplicativos Win32, WPF e Windows Forms.

O MSIX possibilita que as empresas permaneçam atualizadas e garantam que os aplicativos estejam sempre atualizados. Ele permite que os profissionais e os desenvolvedores de TI forneçam uma solução voltada para o usuário ao mesmo tempo que reduz o custo de propriedade de aplicativos por meio da diminuição da necessidade de reempacotamento.

## <a name="key-features"></a>Principais recursos

* **Confiabilidade.** O MSIX oferece uma instalação confiável, com uma taxa de sucesso de 99,96% em milhões de instalações com desinstalação garantida.
* **Otimização da largura de banda da rede.** O MSIX reduz o impacto na largura de banda da rede por meio do download apenas do bloco 64k. Isso é feito aproveitando o arquivo AppxBlockMap.xml contido no pacote do aplicativo MSIX (veja abaixo para obter mais detalhes). O MSIX é projetado para sistemas modernos e para a nuvem.
* **Otimizações do espaço em disco.** Com o MSIX não há duplicação de arquivos entre aplicativos e o Windows gerencia os arquivos compartilhados entre os aplicativos. Os aplicativos ainda são independentes uns dos outros, então as atualizações não vão afetar outros aplicativos que compartilham o arquivo. Uma desinstalação limpa é garantida, mesmo que a plataforma gerencie arquivos compartilhados entre aplicativos.

## <a name="highlights"></a>Destaques

* **Empacotar aplicativos existentes do Windows.** Use a [Ferramenta de Empacotamento de MSIX](packaging-tool/mpt-overview.md) para criar um pacote MSIX para qualquer aplicativo do Windows, antigo ou novo. A Ferramenta de Empacotamento MSIX simplifica a experiência de empacotamento, oferecendo uma interface do usuário interativa ou uma linha de comando para converter e empacotar aplicativos do Windows.
* **Instalar pacotes de aplicativo do MSIX.** Use o [Instalador de Aplicativo](app-installer/app-installer-root.md) para instalar ou atualizar qualquer pacote de aplicativo MSIX que esteja localmente disponível ou em qualquer rede de distribuição de conteúdo.
* **Aplicar correções de tempo de execução para aplicativos empacotados.** A [Estrutura de Suporte do Pacote](psf/package-support-framework-overview.md) é um kit de software livre que ajuda a aplicar correções a seu aplicativo da área de trabalho existente quando não há acesso ao código-fonte, para que ele possa ser executado em um contêiner do MSIX.
* **Usar o MSIX em qualquer lugar.** Com o [SDK do MSIX](msix-sdk/sdk-overview.md) de software livre, os pacotes MSIX são mais versáteis e independentes de plataforma. O SDK fornece todas as APIs necessárias para verificar, validar e desempacotar um pacote de aplicativo em qualquer plataforma, incluindo plataformas do Windows 10 e plataformas que não são do Windows 10.

## <a name="introduction-video-to-msix-and-resources"></a>Vídeo de introdução ao MSIX e recursos

Este vídeo apresenta as principais maneiras pelas quais o empacotamento MSIX pode ajudar você a simplificar e melhorar os fluxos de trabalho de implantação e instalação de seu aplicativo.

<br/>

> [!VIDEO https://www.youtube.com/embed/phrD081sMWc]

Visite a página [MSIX Tech Community](https://aka.ms/msixcommunity) para ver discussões e as informações mais recentes sobre o MSIX. Para obter recursos adicionais sobre como aprender o MSIX, veja [este artigo](resources.md).

## <a name="inside-an-msix-package"></a>Dentro de um pacote MSIX

![Diagrama do pacote MSIX](package/images/msixpackage.png)

### <a name="app-payload"></a>Carga do aplicativo

Os arquivos de carga são os arquivos de código e ativos de aplicativo criados durante a compilação do aplicativo.

### <a name="appxblockmapxml"></a>AppxBlockMap.xml

O arquivo de mapa de blocos do pacote é um documento XML que contém uma lista dos arquivos do aplicativo juntamente com os índices e hashes criptográficos de cada bloco de dados armazenado no pacote. O próprio arquivo do mapa de blocos é verificado e protegido por uma assinatura digital quando o pacote é assinado. O arquivo do mapa de blocos permite que os pacotes MSIX sejam baixados e validados de forma incremental e também funciona para oferecer suporte a atualizações diferenciais de arquivos do aplicativo depois da instalação.

### <a name="appxmanifestxml"></a>AppxManifest.xml

O manifesto do pacote é um documento XML que contém as informações de que o sistema precisa para implantar, exibir e atualizar um aplicativo MSIX. Essas informações incluem a identidade do pacote, as dependências do pacote, os recursos necessários, os elementos visuais e os pontos de extensibilidade.

### <a name="appxsignaturep7x"></a>AppxSignature.p7x

O AppxSignature.p7x é gerado quando o pacote é autenticado. Todos os pacotes MSIX precisam ser autenticados antes da instalação. Com o AppxBlockmap.xml, a plataforma consegue instalar o pacote e validá-lo.

## <a name="supported-platforms"></a>Plataformas compatíveis

Consulte [este artigo](supported-platforms.md) para obter uma lista completa das plataformas que oferecem suporte a MSIX.

## <a name="benefits-of-app-containers"></a>Benefícios de contêineres de aplicativo

Os aplicativos empacotados usando MSIX são executados em um contêiner leve de aplicativo. O processo do aplicativo MSIX e seus processos filhos são executados no contêiner e isolados usando a virtualização do sistema de arquivos e do Registro. Todos os aplicativos MSIX podem ler o Registro global. Um aplicativo MSIX grava em seu próprio Registro virtual e na pasta de dados do aplicativo, e esses dados serão excluídos quando o aplicativo for desinstalado ou redefinido. Outros aplicativos não têm acesso ao Registro virtual ou ao sistema de arquivos virtual de um aplicativo MSIX.
