---
title: Criar um pacote de aplicativos com todos os outros instaladores
description: Criar um pacote MSIX com nossa ferramenta de conversão com todos os outros instaladores
ms.date: 04/04/2019
ms.topic: article
keywords: MSIX, MPT, Ferramenta de Empacotamento MSIX
ms.localizationpriority: medium
ms.openlocfilehash: 9ee45813396a0342a0ea2ad3a9c1784820f7dbdc
ms.sourcegitcommit: 3b780f7599f1777e99ba66baf66e42e2c8dd2d97
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72561203"
---
# <a name="create-an-msix-package-with-all-other-installer-types"></a>Criar um pacote MSIX com todos os outros tipos de instaladores

Use a Ferramenta de Empacotamento MSIX para criar um pacote de aplicativos MSIX com base em outros tipos de instaladores. O dispositivo ou a VM que você está usando para converter os aplicativos em MSIX precisa atender aos seguintes critérios:

- Ele precisa ser configurado para receber comandos remotos (execute o comando Enable-PSRemoting na VM).
- Ele precisa executar o Windows 10, versão 1809 ou uma versão posterior do Windows.
- Quando a ferramenta for iniciada pela primeira vez, você deverá fornecer consentimento para enviar dados telemétricos. É importante observar que os dados de diagnóstico que você compartilha são provenientes apenas do aplicativo e nunca são usados para identificá-lo ou contatá-lo. Isso apenas nos ajuda a corrigir as coisas mais rapidamente para você.

## <a name="choose-the-installer-you-want-to-package"></a>Escolher o instalador que você deseja empacotar

Em **Escolher o instalador que você deseja empacotar**, localize o instalador no computador. Caso não tenha um instalador, ignore esta etapa. Recomendamos que você assine o pacote.

- Marque a caixa em **Assinar pacote**, procure e selecione o arquivo de certificado .pfx. Se o certificado for protegido por senha, digite a senha na caixa de senha.
- Marque a caixa em **Usar argumentos do instalador** e insira o argumento desejado no campo fornecido. Isso é opcional. Esse campo aceita qualquer cadeia de caracteres.

## <a name="packaging-method"></a>Método de compactação

- Selecione o método de empacotamento e clique em **Avançar**.

## <a name="package-information"></a>Informações do pacote

Depois de optar por empacotar o aplicativo em uma máquina virtual existente, você precisará fornecer informações sobre o aplicativo. A ferramenta tentará preencher automaticamente esses campos com base nas informações disponíveis no instalador (se aplicável). Você sempre terá uma opção para atualizar as entradas, conforme necessário. Se o campo tiver um asterisco *, ele será obrigatório. A ajuda embutida será fornecida se a entrada não for válida.

- Nome do pacote:
  - Obrigatório; corresponde ao Nome de identidade do pacote no manifesto para descrever o conteúdo do pacote.
  - Precisa ser correspondente às informações da entidade do Nome do certificado usado para assinar um pacote.
  - Não é mostrado ao usuário final.
  - Diferencia maiúsculas de minúsculas e não pode ter um espaço.
  - Pode aceitar uma cadeia de caracteres entre 3 e 50 caracteres que consiste em caracteres alfanuméricos, ponto e traço.
  - Não é possível terminar com um ponto e ser um destes: "CON", "PRN", "AUX", "NUL", "COM1", "COM2", "COM3", "COM4", "COM5", "COM6", "COM7", "COM8", "COM9", "LPT1", "LPT2", "LPT3", "LPT4", "LPT5", "LPT6", "LPT7", "LPT8" e "LPT9".

- Nome de exibição do pacote:
  - Obrigatório; corresponde ao pacote no manifesto para exibir um nome de pacote amigável para o usuário no menu Iniciar e nas páginas de configurações.
  - O campo aceita uma cadeia de caracteres entre 1 e 256 caracteres e é localizável.

- Nome do fornecedor:
  - Obrigatório; corresponde ao pacote que descreve as informações do fornecedor.
  - O atributo Publisher precisa corresponder às informações da entidade do fornecedor do certificado usado para assinar um pacote.
  - Este campo aceita uma cadeia de caracteres entre 1 e 8.192 caracteres que ajusta a expressão regular de um nome diferenciado: "(CN | L | O | OU | E | C | S | STREET | T | G | I | SN | DC | SERIALNUMBER | Description | PostalCode | POBox | Phone | X21Address | dnQualifier | (OID.(0 | [1-9][0-9])(.(0 | [1-9][0-9]))+))=(([^,+="<>#;])+ | ".")(, ((CN | L | O | OU | E | C | S | STREET | T | G | I | SN | DC | SERIALNUMBER | Description | PostalCode | POBox | Phone | X21Address | dnQualifier | (OID.(0 | [1-9][0-9])(.(0 | [1-9][0-9]))+))=(([^,+="<>#;])+ | ".")))*".

- Nome de exibição do fornecedor:

  - Obrigatório; corresponde ao pacote no manifesto para exibir um nome de fornecedor amigável para o usuário no instalador do Aplicativo e nas páginas de configurações.
  - O campo aceita uma cadeia de caracteres entre 1 e 256 caracteres e é localizável.

- Versão:

  - Obrigatório; corresponde ao pacote no manifesto para descrever o número de versão do pacote.
  - Este campo aceita uma cadeia de caracteres de versão na notação quádrupla: "Major. Minor. Build. Revision".

- Local de instalação:

  - Esse é o local em que o instalador copiará o conteúdo do aplicativo (normalmente, a pasta Arquivos de Programas).
  - Esse campo é opcional, mas recomendado, especialmente quando o conteúdo do aplicativo está sendo instalado fora das pastas Arquivos de Programas.
  - Procure e selecione um caminho de pasta.
  - Verifique se esse arquivo corresponde ao local de instalação do instalador enquanto você executa a operação de instalação do aplicativo.

## <a name="prepare-computer"></a>Preparar o computador

Em seguida, a página **Preparar computador** fornece opções para preparar o computador para empacotamento.

O Driver da Ferramenta de Empacotamento MSIX é obrigatório e a ferramenta tentará automaticamente habilitá-lo se ele não estiver habilitado. A ferramenta verificará primeiro com o DISM para ver se o driver está instalado.

> [!NOTE]
> O Driver da Ferramenta de Empacotamento MSIX monitora o sistema para capturar as alterações que um instalador faz no sistema, que permite à Ferramenta de Empacotamento MSIX criar um pacote com base nessas alterações.

- [Opcional] Marque a caixa **O Windows Search está Ativo** e selecione **Desabilitar selecionado** se você optar por desabilitar o serviço de pesquisa.

  - Isso não é obrigatório, somente recomendado.
  - Quando essa opção estiver desabilitada, a ferramenta atualizará o campo de status para “desabilitado”

- [Opcional] Marque a caixa **O Windows Update está Ativo** e selecione **Desabilitar selecionado** se você optar por desabilitar o serviço de Atualização.

  - Isso não é obrigatório, somente recomendado.
  - Quando essa opção estiver desabilitada, a ferramenta atualizará o campo de status para **Desabilitado**.

- A caixa de seleção **Reinicialização pendente** está desabilitada por padrão. Você precisará reiniciar o computador manualmente e, em seguida, iniciar a ferramenta novamente caso receba um prompt informando que as operações pendentes precisam de reinicialização. Isso não é obrigatório, somente recomendado.

Quando terminar de preparar o computador, clique em **Avançar**.

## <a name="installation"></a>Instalação

Neste momento, se você não nos forneceu um instalador, siga em frente e execute todos os instaladores necessários. Isso inclui programas e scripts personalizados. A ferramenta está monitorando as alterações no dispositivo, incluindo os itens que estão sendo instalados no dispositivo.

> [!NOTE]
> Durante a conversão, os instaladores podem executar os serviços. Os serviços não são capturados durante a conversão. Como resultado, o aplicativo pode ser instalado, mas ser executado com problemas.

## <a name="manage-first-launch-tasks"></a>Gerenciar as primeiras tarefas de inicialização

Essa página mostra os executáveis do aplicativo capturados pela ferramenta. Se houver vários aplicativos, marque a caixa que corresponde ao ponto de entrada principal. Se o aplicativo .exe não for exibido aqui, procure-o manualmente e execute-o.

Clique em **Avançar** e você verá um pop-up que solicitará a confirmação de que você concluiu a instalação do aplicativo e o gerenciamento das primeiras tarefas de inicialização.

- Se você tiver concluído, clique em **Sim, continuar**.
- Se você não tiver concluído, clique em **Não, não terminei**. Você será levado novamente à última página, em que pode iniciar aplicativos, instalar ou copiar outros arquivos e DLLs/executáveis.

## <a name="create-package"></a>Criar pacote

- Forneça uma localização para salvar o pacote MSIX.
- A localização de salvamento padrão é a área de trabalho.
- Você pode definir a localização de salvamento padrão no menu Configurações.
- Caso deseje continuar para editar o conteúdo e as propriedades do pacote antes de salvar o pacote MSIX, selecione **Editor de pacote** e ir para o [editor de pacote](https://docs.microsoft.com/windows/msix/packaging-tool/package-editor)
- Clique em **Criar** para criar o pacote MSIX.

Depois que o pacote for criado, a ferramenta exibirá uma janela pop-up com o nome, o distribuidor, o local de salvamento dos logs e o local de salvamento do pacote recém-criado. Você pode fechar essa janela pop-up para ser redirecionado à página de boas-vindas. Você também pode selecionar o **Editor de pacote** para ver e modificar as propriedades e o conteúdo do pacote.
