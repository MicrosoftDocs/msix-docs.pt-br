---
Description: Uma visão geral dos tópicos sobre criação de um pacote do MSIX com base em código-fonte
title: Criar um pacote MSIX com base na visão geral do código
author: Huios
ms.date: 02/03/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ce394fa310da995a59289bfb463d643bf2b46413
ms.sourcegitcommit: 37bc5d6ef6be2ffa373c0aeacea4226829feee02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074082"
---
# <a name="building-an-msix-package-from-your-code"></a>Criar um pacote do MSIX com base em seu código 

Se o seu aplicativo da área de trabalho estiver em desenvolvimento ativo, recomendamos criar um pacote MSIX no ambiente do build em vez de gerar um instalador e executá-lo com a Ferramenta de Empacotamento MSIX. No Visual Studio 2017 versão 15.5 e posterior (incluindo Visual Studio 2019) é possível usar o Projeto de Empacotamento de Aplicativo do Windows para gerar um MSIX para seu aplicativo. Se não estiver desenvolvendo no Visual Studio, há ferramentas de linha de comando MSIX que você pode integrar ao sistema do build para empacotar os binários do aplicativo como MSIX.

Se estiver desenvolvendo um aplicativo UWP, o Visual Studio usará o MSIX como formato de empacotamento padrão do aplicativo.

|Tópico| Descrição |
|:---|:---|
|[O que saber antes de empacotar o aplicativo de área de trabalho](before-packaging-overview.md)| Histórico de requisitos de MSIX e comportamento de tempo de execução de aplicativo. É útil ter essas informações antes de criar um pacote MSIX para o aplicativo de área de trabalho. Se estiver criando um aplicativo UWP, será possível ignorar essa seção. | 
|[Empacotar o aplicativo UWP ou da área de trabalho no Visual Studio](vs-package-overview.md)| Esta seção fala sobre como empacotar o aplicativo UWP ou da área de trabalho (Windows Forms, WPF, Win32 etc.) como MSIX no Visual Studio.|
|[Pipelines de CI/CD para Builds e Implantações MSIX](azure-dev-ops.md)| Esta seção fala sobre como automatizar os fluxos de trabalho de build e implantação usando pipelines de CI/CD no Azure DevOps.|
|[Empacotamento pela linha de comando](../package/manual-packaging-root.md)| Esta seção fala sobre como empacotar o aplicativo como MSIX usando ferramentas de linha de comando.|
|[Estender o aplicativo MSIX](extend-overview.md)| Esta seção fala sobre como é possível estender o aplicativo usando extensões e pacotes opcionais.|

## <a name="add-modern-windows-10-experiences"></a>Adicionar as experiências modernas do Windows 10

Depois de criar um pacote MSIX para o aplicativo de área de trabalho, é possível usar APIs da UWP, extensões de pacote e componentes UWP para possibilitar experiências modernas e envolventes do Windows 10, como notificações e blocos dinâmicos.

### <a name="enhance-with-uwp-apis"></a>Aprimorar com as APIs da UWP

Depois de preparar o pacote do aplicativo, será possível aprimorá-lo com recursos como blocos dinâmicos e notificações por push. Alguns desses recursos podem melhorar significativamente o nível de envolvimento do seu aplicativo e podem ser adicionados rapidamente. Outros aprimoramentos exigem um pouco mais de código.

Veja [Usar as APIs da UWP em aplicativos de área de trabalho](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance).

### <a name="integrate-with-package-extensions"></a>Integrar com extensões de pacote

Caso o aplicativo precise se integrar ao sistema (por exemplo, estabelecer regras de firewall), descreva os itens no manifesto do pacote do aplicativo e o sistema vai se encarregar do resto. Não é necessário escrever códigos para a maioria dessas tarefas. Com um pouco de XML no manifesto, você pode fazer coisas como iniciar um processo quando o usuário fizer logon, integrar o aplicativo no Explorador de Arquivos e adicionar nele uma lista de destinos de impressão que aparecem em outros aplicativos.

Consulte [Integrar o aplicativo da área de trabalho com as extensões de pacote](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extensions).

### <a name="extend-with-uwp-components"></a>Estender com componentes UWP

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de aplicativo moderno. Normalmente, primeiro é necessário determinar se é possível adicionar sua experiência [aprimorando](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance) o aplicativo da área de trabalho existente com APIs da UWP. Caso seja necessário usar um componente UWP para obter a experiência, será possível adicionar um projeto UWP à solução e usar os serviços de aplicativo para fazer a comunicação entre o aplicativo da área de trabalho e o componente UWP.

Veja [Estender seu aplicativo da área de trabalho com componentes UWP](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-extend).
