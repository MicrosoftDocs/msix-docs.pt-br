---
description: Como usar a Extensão de Empacotamento do MSIX.
title: Extensão de empacotamento do MSIX
ms.date: 11/5/2020
ms.topic: article
keywords: msix, extensão do DevOps, empacotamento, CI/CD, Azure DevOps
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 53d63fec204fad166f8de4548f1ae401a5c1556a
ms.sourcegitcommit: 0b5b7bfc2985f2b420f0ba9f2edb25c5843f8ce6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384918"
---
# <a name="msix-packaging-extension"></a>Extensão de empacotamento do MSIX

A Extensão de *Empacotamento do MSIX* é uma extensão do Azure DevOps que ajuda a criar, empacotar e assinar os aplicativos do Windows usando o formato de pacote MSIX.

Os fluxos de trabalho de CI/CD se tornaram parte integrante do processo de desenvolvimento para melhorar a eficiência e a qualidade, ao mesmo tempo reduzindo o custo e o tempo de colocação no mercado. A solução de CI/CD da Microsoft, Pipelines do Azure DevOps, é amplamente adotada e popular, mas o processo atual de integração de fluxos de trabalho de compilação e implantação para aplicativos que precisam ser empacotados como MSIX no Azure Pipelines usando arquivos YAML é entediante, especificamente para pessoas que não são especialistas no Azure Pipelines e nem no MSIX. Essa extensão do Azure DevOps oferece uma solução simples, intuitiva e baseada na interface do usuário, facilitando a automatização do processo de compilação e implantação de aplicativos que estão sendo empacotados como MSIX e para aplicativos com fluxos de trabalho de CI/CD existentes para mover para o MSIX sem interromper os mecanismos de compilação e implantação.

A Extensão de *Empacotamento do MSIX* contém as seguintes tarefas que você pode usar para criar um pipeline personalizado de acordo com os requisitos:

1. **Compilação e empacotamento do MSIX** – para compilar e empacotar aplicativos do Windows usando o formato de pacote MSIX
2. **Assinatura de pacote MSIX** – Para assinar pacotes MSIX usando um certificado confiável
3. **Arquivo do instalador do aplicativo para MSIX** – Para criar ou atualizar um arquivo .appinstaller para os aplicativos MSIX
4. **Criar pacote para a anexação de aplicativo MSIX** – Para criar um pacote VHDX para a anexação de aplicativo MSIX

## <a name="install-the-extension"></a>Instalar a extensão

Procure no Marketplace do Azure DevOps e busque o nome da Extensão de *Empacotamento do MSIX*.

![Procure no marketplace](images/msix-packaging-ext/browse-marketplace.png)

## <a name="create-a-pipeline"></a>Criar um pipeline

Crie um pipeline para o projeto do Azure DevOps.

![selecionar pipeline](images/msix-packaging-ext/select-pipeline.png)

![Novo pipeline](images/msix-packaging-ext/new-pipeline.png)

Selecione a opção de *Usar o editor clássico para criar um pipeline sem YAML*.

![Usar o editor clássico](images/msix-packaging-ext/classic-editor.png)

Selecione o sistema de controle de versão e forneça os detalhes do repositório e do branch padrão.

![Configurar .vcs de origem](images/msix-packaging-ext/vcs.png)

Quando for solicitado que você *selecione um modelo*, clique em *iniciar com um **Trabalho vazio** _.

![Começar um trabalho vazio](images/msix-packaging-ext/empty-job.png)

Altere a seleção do _Agente de Especificação* para ***windows-2019**_, uma vez que a extensão do MSIX é executada apenas em um agente do Windows.

![Janelas de especificação de agente](images/msix-packaging-ext/agent-specification.png)

Você deve ver _Trabalho do agente 1* por padrão no pipeline. Clique no símbolo de adição para *Adicionar uma tarefa ao Trabalho do agente 1*.

Pesquise por ***MSIX** _ na barra de pesquisa _Adicionar tarefas* e você deverá ver as tarefas mencionadas anteriormente na Extensão de *Empacotamento do MSIX*. Você pode criar o pipeline de maneira personalizada adicionando as tarefas necessárias de acordo com os requisitos. Mas nós demonstraremos como configurar todas as quatro tarefas nesta página.

![Adicionar uma tarefa](images/msix-packaging-ext/add-task.png)

### <a name="msix-build-and-package"></a>Compilação e empacotamento do MSIX

![Tarefa de compilação e empacotamento](images/msix-packaging-ext/build-and-package.png)

- **Nome de exibição** – Personalize o nome da tarefa
- **Caminho de saída** – Especifique o caminho de saída para o pacote MSIX que será criado por essa tarefa. O caminho do exemplo acima usa a [variável predefinida](https://docs.microsoft.com/azure/devops/pipelines/build/variables) **Build.ArtifactStagingDirectory** que é o caminho local no agente para armazenar artefatos e é usada aqui para armazenar os arquivos de saída da tarefa que podem ser publicados posteriormente usando uma tarefa de publicar artefatos.
- **Compilar solução com o MSBuild** – Selecione esta opção para compilar a solução com o MSBuild para a plataforma de destino especificada. Deixe a caixa desmarcada se você já tiver binários que precisam apenas ser empacotados. Se você deixar a caixa desmarcada, será solicitado que forneça o caminho para os binários.
- **Projeto a Compilar** – Forneça o caminho para o arquivo do projeto ou da solução que precisa ser compilado.
- **Limpar antes de Compilar** – Marque essa caixa de seleção se quiser que a tarefa execute uma limpeza antes da compilação.
- **Gerar Grupo MSIX** – Marque essa caixa de seleção para gerar um grupo MSIX em vez de um pacote. Não se esqueça de nomear o arquivo de saída na opção **Caminho de Saída** com a extensão .msixbundle, em vez de .msix.
- **Configuração** – Escolha entre as configurações de compilação *Depurar* e *Lançar*.
- **Plataforma** – Especifique a plataforma de compilação desejada, por exemplo, *x64*, *x86* ou *Qualquer CPU*.
- **Atualizar a versão do aplicativo no Manifesto** – Marque essa caixa de seleção para alterar a versão do aplicativo com a versão especificada no arquivo .appxmanifest do aplicativo. Isso não substituirá o arquivo .appxmanifest, mas vai alterar a versão do aplicativo no pacote MSIX de saída gerado. Se essa opção estiver selecionada, você será solicitado a fornecer o caminho para o arquivo de manifesto e o número da versão do aplicativo a ser definido para o aplicativo.
- **Modo de distribuição de pacotes do aplicativo** – Selecione o modo no menu suspenso para gerar um Pacote do aplicativo de armazenamento ou Pacote de aplicativo que não seja de armazenamento.
- **Opções avançadas para o MSBuild** – Personalize o MSBuild usando as opções avançadas.

### <a name="msix-package-signing"></a>Assinatura do pacote MSIX

![tarefa de assinatura](images/msix-packaging-ext/signing.png)

- **Nome de exibição** – Personalize o nome da tarefa
- **Pacote a ser assinado** – A tarefa de assinatura de pacote MSIX usa a SignTool para assinar todos os arquivos correspondentes a esse caminho, independentemente de serem pacotes ou grupos MSIX.
- **Arquivo de Certificado** – Selecione o certificado confiável para assinar o aplicativo na lista suspensa ou carregue um arquivo de certificado usando o ícone de engrenagem.
- **Variável de senha** – O nome da variável que armazena a senha usada para acessar o arquivo de certificado para assinatura. Observe que essa NÃO é a senha em si, mas o nome da variável, que pode ser definido na Biblioteca.
- **Servidor de carimbo de data/hora** – Uma URL que especifica o endereço de um servidor de carimbo de data/hora. Esse é um parâmetro opcional.

### <a name="app-installer-file-for-msix"></a>Arquivo do instalador de aplicativo MSIX

![appinstaller](images/msix-packaging-ext/app-installer.png)

- **Nome de exibição** – Personalize o nome da tarefa
- **Pacote** – Esse é o caminho para o pacote ou o grupo para o qual você deseja criar um Instalador de Aplicativo.
- **Caminho do arquivo de saída** – Este é o caminho do arquivo do Instalador de Aplicativo a ser gravado.
- **Método para criar o arquivo do Instalador de Aplicativo** – Escolha se deseja criar um arquivo do Instalador de Aplicativo ou atualizar um arquivo existente. Se optar por atualizar um existente, você será solicitado a fornecer o caminho para o arquivo do Instalador do Aplicativo existente.
- **Versão do arquivo do Instalador de Aplicativo** – O número da versão que será fornecida. Deve assumir a forma (*principal*).(*secundária*).(*build*).(*revisão*).
- **URI** – URI da Web para o arquivo do Instalador de Aplicativo redirecionado.
- **URI principal do pacote/grupo** – URI do local do pacote/grupo do aplicativo.
- **Atualizar ao iniciar** – Selecione esta configuração para configurar o aplicativo para verificar se há atualizações ao ser iniciado. Se essa caixa de seleção estiver marcada, você será solicitado a fornecer detalhes como o número de *Horas entre as verificações de atualização* para *Mostrar a interface do usuário ao atualizar* e para que a *Atualização bloqueie a ativação do aplicativo*.

### <a name="create-package-for-msix-app-attach"></a>Criar pacote para a anexação de aplicativo MSIX

![anexação de aplicativo](images/msix-packaging-ext/app-attach.png)

- **Nome de exibição** – Personalize o nome da tarefa.
- **Caminho do pacote** – Este é o caminho para o pacote/grupo MSIX.
- **Caminho de Saída do VHDX** – Esse é o caminho do arquivo VHDX que será criado pela tarefa.
- **Tamanho do VHDX** – O tamanho máximo do VHDX em MB.

Depois de configurar todas as tarefas, você pode usar uma tarefa *Publicar artefatos de compilação* a fim de remover todos os artefatos do local temporário para os artefatos do Azure Pipelines ou para o compartilhamento de arquivos de sua preferência.

## <a name="ways-to-provide-feedback"></a>Maneiras de fornecer comentários

Adoraríamos ouvir comentários sobre a Extensão de *Empacotamento do MSIX*. Entre em contato conosco por meio dos seguintes canais:

- Examinar a extensão no Marketplace do Azure DevOps
- [MSIX Tech Community](https://techcommunity.microsoft.com/t5/msix/ct-p/MSIX)
- [Projeto de software livre do GitHub](https://github.com/microsoft/msix-packaging/tree/master/tools/pipelines-tasks) – O código-fonte dessa extensão faz parte do projeto de software livre do SDK do MSIX, que agradece contribuições e sugestões.
