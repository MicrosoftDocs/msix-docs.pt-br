---
title: Visão geral da ferramenta de empacotamento MSIX
description: Documento de visão geral sobre como começar com a ferramenta de empacotamento Msix
author: mcleanbyron
ms.author: mcleans
ms.date: 02/19/2019
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 6432a4b33b93a6340acabfd6284f5f530b2fe8b9
ms.sourcegitcommit: 5669d59a0979a9de1dead4949f44d1544fd45988
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65795440"
---
# <a name="msix-packaging-tool"></a>Ferramenta de Empacotamento MSIX 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>

A ferramenta de empacotamento MSIX permite que você reempacotar os aplicativos Win32 existentes para o formato MSIX. Ele oferece uma interface do usuário interativo e uma linha de comando para conversões e lhe dá a habilidade de converter um aplicativo sem a necessidade de código-fonte. Queremos permitir que os profissionais de TI converter seus ativos existentes para MSIX, para fornecer-lhes uma maneira melhor de fazer o empacotamento e gerenciamento de aplicativo.

Ferramenta de empacotamento MSIX agora está disponível da Microsoft Store. Você pode executar seus instaladores da área de trabalho por essa ferramenta e obter um pacote MSIX que podem ser instalados em seu computador.

Interessado ser insider uma ferramenta de empacotamento MSIX, clique em [aqui](packaging-tool/insider-program.md) para obter mais detalhes.

## <a name="prerequisites"></a>Pré-requisitos

- Windows 10, versão 1809 (ou posterior)
- Participação no programa Windows Insider (se você estiver usando um build do Insider)
- Um alias MSA (conta) da Microsoft válido para acessar o aplicativo da Microsoft Store 
- Privilégios de administrador no seu computador para executar a ferramenta
 
 ## <a name="install"></a>Instalar
 
Para instalar a ferramenta de empacotamento MSIX da Microsoft Store, acesse [aqui](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf), tornando-se de que você está conectado com a MSA que é usada para o seu programa do Windows Insider. Em seguida, vá para a página de descrição de produto e clique no ícone de instalação para iniciar a instalação.

Ferramenta de empacotamento MSIX também pode ser baixada para uso offline na empresa da Microsoft Store para empresas [portal da web](https://businessstore.microsoft.com/). Você pode aprender mais sobre a distribuição off-line [aqui](https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

 
## <a name="latest-public-version---120194020"></a>Versão mais recente do público - 1.2019.402.0

### <a name="new-features"></a>Novos recursos:

- Capacidade de converter em uma máquina remota - [obter mais informações](packaging-tool/remote-conversion-setup.md)
- Melhor experiência de gerenciamento no editor de pacote
    - Recomendações de controle de versão automático ao salvar no editor de pacote
    - Agora dá suporte à adição de pasta existente para o pacote no VFS
- Usuário pode especificar códigos de saída de válidos conhecidos para conversões de CLI
- Adicionada a capacidade para o carimbo de data / hora seu pacote assinado em todos os fluxos de trabalho em que a assinatura está disponível no momento 
    - Você pode especificar sua URL de carimbo de data / hora padrão e o tipo de servidor de carimbo de data / hora na página de configurações de ferramenta
- Atualizado [lógica de geração de AppID](packaging-tool/release-notes/history.md#appid-generation-logic)e adicionou uma validação adicional para o aplicativo e o nome do pacote 

Você pode encontrar o histórico completo das notas de versão da ferramenta de empacotamento MSIX [aqui](packaging-tool/release-notes/history.md).

 ## <a name="tasks"></a>Tarefas
 
Aqui está o que você pode esperar ser capaz de fazer com essa ferramenta:
 
- Empacotar o aplicativo favorito (msi, exe, App-V 5. x e scripts personalizados) no formato MSIX iniciando a ferramenta e selecionando o **pacote de aplicativo** ícone.
- Criar um pacote de modificação de um pacote MSIX iniciando a ferramenta e selecionando o **pacote de modificação** ícone. 
- Abra seu pacote MSIX para exibir e editar suas propriedades/conteúdo selecionando o **editor de pacote** ícone, navegando até o pacote MSIX e selecionando **Abrir pacote**.

## <a name="try-it-out"></a>Experimente 

Os artigos a seguir estão os tutoriais sobre como usar a ferramenta de empacotamento MSIX para converter os aplicativos da área de trabalho: 

| Artigo | Descrição |
|-------|-------------|
| [Criar pacote MSIX de um arquivo MSI/App-V](packaging-tool/create-app-package-MSI-VM.md) | Este tutorial percorrer como usar a interface de usuário da ferramenta de empacotamento MSIX para converter seus aplicativos de área de trabalho (particularmente os instaladores, como MSI, EXE ou App-V) para um pacote MSIX. |
| [Criar pacote MSIX de outro tipo de instalador](packaging-tool/create-other-installer.md) | Este tutorial passarão a utilização da interface do usuário da ferramenta de empacotamento MSIX para converter seu aplicativo da área de trabalho (instaladores como scripts em lotes, o PowerShell etc) para um pacote MSIX. |
| [Crie um pacote MSIX usando a interface de linha de comando da ferramenta de empacotamento MSIX](packaging-tool/package-conversion-cli.md) | Este tutorial percorrer como usar a interface de linha de comando da ferramenta de empacotamento MSIX para converter seu aplicativo da área de trabalho a um pacote MSIX. |
| [Automatizar a conversão de pacote MSIX](packaging-tool/automate-conversion.md) | Este tutorial sobre como você pode usar a interface de linha de comando para automatizar a conversão de aplicativos da área de trabalho em pacotes MSIX. |
| [Criar pacote MSIX em um dispositivo remoto](packaging-tool/remote-conversion-setup.md) | Este artigo fornece as instruções necessárias para executar a conversão de aplicativos da área de trabalho em pacotes MSIX em um dispositivo remoto. |