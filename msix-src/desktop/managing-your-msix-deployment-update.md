---
Description: Este artigo oferece detalhes relacionados às opções disponíveis durante a atualização de um aplicativo MSIX.
title: Atualizações diferenciais do pacote do aplicativo MSIX
ms.date: 2/3/2020
ms.topic: article
keywords: windows 10, implantação, msix
ms.assetid: ''
ms.localizationpriority: medium
ms.openlocfilehash: 80258b0b63b16ed02ac12a0b103ac1a4c8949c8e
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89091234"
---
# <a name="differential-updates-for-msix-app-packages"></a>Atualizações diferenciais para pacotes do aplicativo MSIX

## <a name="understanding-msix-app-package-updates"></a>Noções básicas sobre atualizações de pacote do aplicativo MSIX
Quando um pacote do aplicativo MSIX é criado, um arquivo de manifesto é gerado contendo detalhes relacionados aos arquivos incluídos no pacote do aplicativo MSIX. Durante a criação do pacote, uma parte dos metadados é criada e armazenada no pacote .msix ou .msixbundle, que permite que partes do pacote sejam identificadas de forma exclusiva pelo Windows. Posteriormente, durante a atualização, o Windows poderá usar esse arquivo de metadados para comparar o pacote antigo com o novo pacote e determinar o que precisa ser baixado no dispositivo. Já que esses metadados permitem que partes do pacote sejam identificadas de forma exclusiva, isso significa que o mecanismo de atualização diferencial funciona totalmente de qualquer versão do pacote para qualquer outra versão (supondo-se que o pacote de origem seja de uma versão anterior à do pacote de destino).

Tudo começa no arquivo AppxBlockMap.xml (os metadados mencionados acima). O arquivo AppxBlockMap.xml é um documento XML que contém uma lista bidimensional de todas as informações sobre os arquivos no pacote. A primeira dimensão apresenta detalhes de alto nível no arquivo (por exemplo, nome e tamanho) e a segunda dimensão oferece representações de hash SHA2-256 de cada fatia de 64KB do arquivo (ou seja, o "bloco").

O primeiro hash representa o primeiro bloco de 64 KB do arquivo e o segundo hash representa os 35 KB restantes – considerando que o arquivo tenha 101.188 bytes.

Durante uma atualização, se o segundo bloco do arquivo tiver sido modificado, o hash também será atualizado para refletir isso. O componente de download entende isso e extrairá apenas o segundo bloco e reutilizará o primeiro bloco inalterado do pacote antigo.

Além disso, caso um arquivo inteiro não tenha sido alterado (o que é determinado pelo conjunto de blocos completo inalterado) o arquivo poderá ser reutilizado do pacote existente, resultando em uma grande economia para os usuários do Windows 10

### <a name="upgrading-to-newer-versions"></a>Atualização para versões mais recentes
Quando uma versão mais recente do pacote do aplicativo MSIX é instalada, o arquivo de manifesto é comparado e os blocos de arquivo modificados são identificados. Conforme o pacote do aplicativo MSIX é atualizado para a versão mais recente, somente os arquivos modificados são recuperados, reduzindo o consumo de largura de banda caso os aplicativos atualizados estejam em um compartilhamento de rede ou armazenados de forma externa da organização.

### <a name="upgrading-to-older-versions"></a>Atualização para versões anteriores
Quando uma versão anterior do pacote do aplicativo MSIX é instalada, o arquivo de manifesto é comparado e os blocos de arquivo modificados são identificados. Conforme o pacote do aplicativo MSIX é atualizado para a versão mais antiga, somente os arquivos modificados são recuperados, reduzindo o consumo de largura de banda caso os aplicativos atualizados estejam em um compartilhamento de rede ou armazenados de forma externa da organização.

## <a name="optimizing-upgrade-experiences"></a>Otimização das experiências de atualização
A distribuição ou a instalação de um pacote do aplicativo MSIX em um dispositivo pode ser configurada para melhorar a experiência dos usuários. Quando um aplicativo é implantado, o dispositivo pode ser configurado para atualizar o aplicativo depois que o usuário o fecha ou forçar o fechamento do aplicativo e atualizá-lo obrigatoriamente.

### <a name="powershell"></a>PowerShell
A instalação de um pacote do aplicativo MSIX em um dispositivo usando o PowerShell utiliza o cmdlet [add-appxpackage](/powershell-msix-cmdlets.md). Esse cmdlet contém os seguintes parâmetros que alteram a instalação do pacote do aplicativo MSIX ou atualizam a experiência do usuário.

|||
|-|-|
| -DeferRegistrationWhenPackagesAreInUse | Indica que esse cmdlet impedirá que o pacote do aplicativo MSIX seja atualizado enquanto ele estiver aberto. |
| -ForceApplicationShutdown | Indica que esse cmdlet força todos os processos ativos associados ao pacote ou às dependências dele a serem desligados. |
| -ForceUpdateFromAnyVersion | Indica que o pacote do aplicativo MSIX forçará uma versão específica de um pacote a ser preparada/registrada, independentemente de uma versão mais recente já ter sido preparada/registrada. |
| -InstallAllResources | Indica que o cmdlet força a implantação de todos os pacotes de recursos especificados em um argumento de grupo. Isso substitui a verificação de aplicabilidade do recurso pelo mecanismo de implantação e força a preparação de todos os pacotes de recursos. |
| -RetainFilesOnFailure | Caso haja falha na implantação, se a opção estiver definida como True, os arquivos criados no computador de destino durante o processo de instalação não serão removidos. |
| -Update | Especifica que o pacote sendo adicionado é uma atualização de pacote de dependência. Um pacote de dependência é removido quando o aplicativo primário é removido. Se não for especificado, o pacote não será removido quando o aplicativo primário for removido. |

Para obter uma lista completa dos parâmetros disponíveis para este cmdlet, visite o artigo PowerShell em [add-appxpackage](/powershell/module/appx/add-appxpackage?view=win10-ps).