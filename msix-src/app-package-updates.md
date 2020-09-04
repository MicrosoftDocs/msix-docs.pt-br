---
title: Atualizações de pacote do aplicativo
description: Descreve como pacotes MSIX são otimizados para garantir que somente os bits alterados essenciais do aplicativo sejam baixados para atualizar um aplicativo do Windows já existente.
ms.date: 09/10/2018
ms.topic: article
keywords: Windows 10, UWP, pacote do aplicativo, atualização do aplicativo, MSIX, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 5d088f60103543e998b4499977fc273f80f97a69
ms.sourcegitcommit: 6b1ec6420dbaa327b65c208b4cd00da87985104b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2020
ms.locfileid: "89090184"
---
# <a name="app-package-updates"></a>Atualizações de pacote do aplicativo

A atualização de pacotes de aplicativos modernos do Windows é otimizada para garantir que somente os bits alterados essenciais do aplicativo sejam baixados para atualizar um aplicativo existente do Windows.

## <a name="metadata-in-the-appxblockmapxml-file"></a>Metadados no arquivo AppxBlockMap.xml

Em um alto nível, durante a criação de pacote, uma parte dos metadados é criada e armazenada no arquivo do pacote do aplicativo (.appx ou .msix), o que permite que algumas partes do pacote sejam identificadas exclusivamente pelo Windows. Ao atualizar um pacote do aplicativo, o Windows usa o arquivo de metadados para comparar o pacote antigo com o novo pacote e determinar o que precisa ser baixado no dispositivo.

Como os metadados permitem que algumas partes do pacote sejam identificadas exclusivamente, isso significa que o mecanismo de atualização diferencial funciona totalmente de qualquer versão de um pacote para qualquer outra versão de um pacote (supondo que o pacote de origem tenha uma versão inferior do que o pacote de destino). 

Os metadados estão contidos no arquivo AppxBlockMap.xml (os metadados mencionados anteriormente). O arquivo AppxBlockMap.xml é um arquivo XML que contém uma lista bidimensional de informações sobre os arquivos do pacote. A primeira dimensão apresenta detalhes de alto nível sobre o arquivo (por exemplo, nome e tamanho) e a segunda dimensão fornece representações de hash SHA2-256 de cada fatia de 64 KB do arquivo (o "bloco").

Veja a seguir uma amostra de um arquivo AppxBlockMap.xml.

```xml
<!--?xml version="1.0" encoding="UTF-8"?-->
<blockmap hashmethod="http://www.w3.org/2001/04/xmlenc#sha256" 
          xmlns="http://schemas.microsoft.com/appx/2010/blockmap">
  <file lfhsize="66" size="101188" name="asset1.jpg">
    <block hash="2bidNE0JyaO+FjaTpRe0g8HzUCblUf/cfBcTXiZR74c="/>
    <block hash="+jeFwKrGk5gw9wSICWsWRtEQXwcLC7af4EWS7DgrAkY="/>
  </file>
  <file lfhsize="61" size="108823" name="asset2.jpg">
    <block hash="u0+5S0GOzwyAfYx54tKycZyHRBYm2ybvq27dkIKqDsQ="/>
    <block hash="F9h0FRMetL6BNCszAYB0bgyx2KWN+dO1bls4Q9m267c="/>
  </file>
  ...
</blockmap>
```

O primeiro arquivo (asset1.jpg) tem dois hashes de bloco. O primeiro hash representa o primeiro bloco de 64 KB do arquivo e o segundo hash representa os 35 KB restantes – considerando que o arquivo tenha 101.188 bytes.

Durante uma atualização, se o segundo bloco desse arquivo for modificado, o hash também será atualizado para refletir isso. O componente de download extrai o segundo bloco e reutiliza o primeiro bloco inalterado do pacote antigo.

Em uma escala maior, se um arquivo inteiro não for alterado (determinado por um conjunto completo de blocos que não é alterado), esse arquivo poderá ser reutilizado do pacote existente, economizando tempo e recursos.

## <a name="app-update-constraints"></a>Restrições da atualização de aplicativo

#### <a name="updates-are-performed-within-the-same-package-family"></a>As atualizações são executadas na mesma família de pacotes
A família de pacotes é composta pelo Nome do Pacote e pelo Fornecedor. Para conseguir fazer a atualização, os novos metadados de pacote precisarão ser os mesmos do pacote instalado anteriormente. 

#### <a name="app-updates-must-increment-to-a-higher-version"></a>As atualizações de aplicativo precisam ser incrementada para uma versão superior
No geral, as atualizações de aplicativo exigem que a versão do novo pacote sejam superiores à atual. O processo de atualização de aplicativo não permitirá que os pacotes de versões anteriores sejam instalados por padrão. A partir da versão 1809 do Windows 10, é possível usar ForceUpdateToAnyVersion para permitir que pacotes de versões anteriores sejam instalados quando se forneça uma opção de substituição como parte dos argumentos de atualização. No momento, ele está disponível no PowerShell usando a opção [ForceUpdateFromAnyVersion](/powershell/module/appx/add-appxpackage?view=win10-ps), por meio da [API do PackageManager](/uwp/api/windows.management.deployment.deploymentoptions), do [CSP do EnterpriseModernAppManagement](/windows/client-management/mdm/enterprisemodernappmanagement-csp) e no [arquivo AppInstaller](./app-installer/update-settings.md).  

> [!NOTE]
> Caso use o ForceUpdateToAnyVersion em um aplicativo do Windows Store, o Windows Update atualizará automaticamente para a versão aplicável mais recente.

#### <a name="app-update-package-can-have-a-different-architecture"></a>O pacote de atualização do aplicativo pode ter uma arquitetura diferente
O pacote de atualização para o pacote do aplicativo atualmente instalado pode ser de uma arquitetura diferente, desde que a nova arquitetura seja compatível com o sistema operacional no qual ele está sendo implantado. Por exemplo: Se você tiver a versão x86 do MyFavApp(v1.0.0.0) instalada em um x64, o dispositivo Windows 10 e o pacote de atualização (v2.0.0.0) terão a versão x64: MyFavApp(1.0.0.0) será atualizado para MyFavApp(v2.0.0.0) com êxito. 

#### <a name="packages-can-update-from-an-msix-to-an-msixbundle"></a>Os pacotes podem ser atualizados de um MSIX para um MSIXbundle
Um pacote de atualização pode ser atualizado de um pacote MSIX para um pacote MSIXbundle, mas não vice-versa. Quando um MSIXbundle for instalado, a atualização do pacote precisará permanecer sendo um pacote. 

## <a name="optimize-differential-update-technology"></a>Otimizar a tecnologia de atualização diferencial
    
Há algumas maneiras de garantir que a tecnologia de atualização diferencial seja otimizada para o máximo.

- Mantenha os arquivos no pacote pequeno – isso garantirá que, se uma alteração for necessária que afetará o arquivo completo, a atualização ainda será pequena.
- As alterações nos arquivos devem ser aditivas, se possível – as alterações aditivas garantirão que os dispositivos do usuário final baixem apenas os blocos alterados.
- As alterações nos arquivos deverão estar contidas em blocos de 64 KB, se possível – se o aplicativo tiver arquivos grandes e exigir alterações no meio de um arquivo, a contenção das alterações em um conjunto de blocos ajudará de maneira significativa.
