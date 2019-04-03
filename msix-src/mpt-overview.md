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
ms.openlocfilehash: dd04eb516e41d9de8650895a78f5eb99ccfe6653
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900448"
---
# <a name="msix-packaging-tool"></a>Ferramenta de empacotamento MSIX 

<div class="nextstepaction"><p><a class="x-hidden-focus" href="https://www.microsoft.com/en-us/p/msix-packaging-tool/9n5lw3jbcxkf" data-linktype="external">Obter a ferramenta de empacotamento MSIX</a></p></div>

A ferramenta de empacotamento MSIX permite que você reempacotar os aplicativos Win32 existentes para o formato MSIX. Ele oferece uma GUI interativa e uma linha de comando para conversões e lhe dá a habilidade de converter um aplicativo sem a necessidade de código-fonte. Queremos permitir que os profissionais de TI converter seus ativos existentes para MSIX, para fornecer-lhes uma maneira melhor de fazer o empacotamento e gerenciamento de aplicativo.

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

 
 ## <a name="whats-new"></a>Novidades
 **v1.2019.110.0**
- Tempos de empacotamento aprimorado 
- Lista de exclusão de arquivo padrão atualizado
- Incorporado a ferramenta reporting MSIExec logs de erros
- Logs atualizados para adicionar mais clareza e etapas de solução de problemas
- Suporte adicionado para capturar a instalação do ISE do PowerShell durante o empacotamento manual
- Suporte adicionado para declarar os scripts do PowerShell como um argumento na interface do usuário e o arquivo de modelo de linha de comando do instalador
- Adicionado um sinalizador de registro em log detalhado (-verbose | - v) para a interface de linha de comando
- Corrigido um problema em que os caminhos de rede na VM, às vezes, eram inacessíveis
- Corrigido um problema em que a validação de requisito de controle de versão do Store falhava ao usar a interface de linha de comando
- Corrigido um problema em que os caminhos de arquivo em cotações não foram sendo aceitas
- Corrigido um problema em que a VM não foi sejam limpos corretamente após a conversão
- Corrigido um problema no qual adicionar arquivos aos pacotes no editor de pacote não estava funcionando corretamente
- Limpeza de interface do usuário 


 ## <a name="tasks"></a>Tarefas
 
Aqui está o que você pode esperar ser capaz de fazer com essa ferramenta:
 
- Empacotar o aplicativo favorito (msi, exe, App-V 5. x e para o formato MSIX iniciando a ferramenta e selecionando o **pacote de aplicativo** ícone.
- Criar um pacote de modificação de um pacote de MSIX recém-criado do aplicativo inicie a ferramenta e selecionando o **pacote de modificação** ícone. 
- Abra seu pacote MSIX para exibir e editar suas propriedades de conteúdo/navegando até a **editor de pacote aberto** guia, navegando até o pacote MSIX e selecionando **Abrir pacote**.

