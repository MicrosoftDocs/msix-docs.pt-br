---
title: Visão geral da Ferramenta de Empacotamento MSIX
description: Documento de visão geral sobre como começar a usar a Ferramenta de Empacotamento MSIX
ms.date: 02/19/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 254fd757bd16a72f597fa0c8183d66f9562625b4
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840710"
---
# <a name="msix-packaging-tool"></a>Ferramenta de Empacotamento MSIX 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a Ferramenta de Empacotamento MSIX</a></p></div>

A Ferramenta de Empacotamento MSIX permite que você empacote novamente os aplicativos Win32 existentes para o formato MSIX. Ela oferece uma interface do usuário interativa e uma linha de comando para conversões e proporciona a capacidade de converter um aplicativo sem a necessidade do código-fonte. Queremos permitir que os Profissionais de TI convertam seus ativos existentes para MSIX, visando fornecer a eles uma maneira melhor de fazer o empacotamento e o gerenciamento de aplicativos.

A Ferramenta de Empacotamento MSIX agora está disponível na Microsoft Store. Execute os instaladores da área de trabalho por meio dessa ferramenta e obtenha um pacote MSIX que pode ser instalado no computador.

Caso esteja interessado em ser um Participante do programa Ferramenta de Empacotamento MSIX, clique [aqui](insider-program.md) para obter mais detalhes.

## <a name="prerequisites"></a>Pré-requisitos

- Windows 10, versão 1809 (ou posterior)
- Participação no Programa Windows Insider (caso esteja usando um build do Insider)
- Um alias válido da MSA (conta Microsoft) para acessar o aplicativo na Microsoft Store 
- Privilégios de administrador no computador para executar a ferramenta
 
 ## <a name="install"></a>Instalar
 
Para instalar a Ferramenta de Empacotamento MSIX por meio da Microsoft Store, acesse [aqui](https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf), verificando se você está conectado com a MSA que é usada para o Programa Windows Insider. Em seguida, acesse a página de descrição do produto e clique no ícone Instalar para iniciar a instalação.

A Ferramenta de Empacotamento MSIX também pode ser baixada para uso offline no [portal da Web](https://businessstore.microsoft.com/) da Microsoft Store para Empresas. Saiba mais sobre a distribuição offline [aqui](https://docs.microsoft.com/en-us/microsoft-store/distribute-offline-apps#download-an-offline-licensed-app).

 
## <a name="latest-public-version---120197010"></a>Última versão pública – 1.2019.701.0

### <a name="new-features"></a>Novos recursos:

- Capacidade de reiniciar durante a conversão
- Permite especificar um certificado padrão para assinar
- Permite especificar códigos de saída para reinicializações

Encontre o histórico completo das notas sobre a versão da Ferramenta de Empacotamento MSIX [aqui](release-notes/history.md).

 ## <a name="tasks"></a>Tarefas
 
Isto é o que você pode esperar estar apto a fazer com essa ferramenta:
 
- Empacotar seu aplicativo favorito (msi, exe, App-V 5.x e scripts personalizados) no formato MSIX iniciando a ferramenta e selecionando o ícone do **pacote de aplicativos**.
- Criar um pacote de modificação para um Pacote MSIX iniciando a ferramenta e selecionando o ícone do **Pacote de modificação**. 
- Abrir seu pacote MSIX para exibir e editar o conteúdo/as propriedades selecionando o ícone do **Editor de pacote**, procurando o pacote MSIX e selecionando **Abrir pacote**.

## <a name="try-it-out"></a>Experimente 

Os seguintes artigos são tutoriais sobre como usar a Ferramenta de Empacotamento MSIX para converter os aplicativos da área de trabalho: 

| Artigo | Descrição |
|-------|-------------|
| [Criar um pacote MSIX com base em um arquivo MSI/App-V](create-app-package-MSI-VM.md) | Este tutorial descreverá como usar a interface do usuário da Ferramenta de Empacotamento MSIX para converter seus aplicativos da área de trabalho (particularmente, instaladores como MSI, EXE ou App-V) em um Pacote MSIX. |
| [Criar um pacote MSIX com base em outros tipos de instalador](create-other-installer.md) | Este tutorial descreverá como usar a interface do usuário da Ferramenta de Empacotamento MSIX para converter seus aplicativos da área de trabalho (instaladores como scripts em lotes, PowerShell etc.) em um Pacote MSIX. |
| [Criar um pacote MSIX usando a interface de linha de comando da Ferramenta de Empacotamento MSIX](package-conversion-cli.md) | Este tutorial descreverá como usar a interface de linha de comando da Ferramenta de Empacotamento MSIX para converter seus aplicativos da área de trabalho em um Pacote MSIX. |
| [Automatizar a conversão de pacote MSIX](automate-conversion.md) | Este tutorial abordará como você pode usar a interface de linha de comando para automatizar a conversão de aplicativos da área de trabalho em Pacotes MSIX. |
| [Criar um pacote MSIX em um computador remoto](remote-conversion-setup.md) | Este artigo fornecerá as instruções necessárias para executar a conversão de aplicativos da área de trabalho em pacotes MSIX em um dispositivo remoto. |
