---
title: Atualizações de pacote do aplicativo
description: Saiba como os aplicativos são atualizados correção diferencial.
author: mcleanbyron
ms.author: mcleans
ms.date: 09/10/2018
ms.topic: article
keywords: Windows 10, uwp, o pacote de aplicativo, atualização do aplicativo, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 1d7fc6a2d899638a8d38c47a477653173f2c5c9d
ms.sourcegitcommit: b6887e8f66e1d4a49870b933efa25d539eabfcaf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2019
ms.locfileid: "65882605"
---
# <a name="app-package-updates"></a>Atualizações de pacote do aplicativo

Atualizar pacotes de aplicativos modernos do Windows é otimizado para garantir que somente os bits alterados essenciais do aplicativo são baixados para atualizar um aplicativo existente do Windows.

## <a name="metadata-in-the-appxblockmapxml-file"></a>Metadados no arquivo appxblockmap. XML

Em um alto nível, durante a criação de pacote, uma parte dos metadados é criada e armazenada no arquivo de pacote do aplicativo (. AppX ou .msix) que permite que partes do pacote a ser identificada exclusivamente pelo Windows. Ao atualizar um pacote do aplicativo, o Windows usa o arquivo de metadados para comparar o pacote antigo para o novo pacote e determinar o que precisa ser baixado para o dispositivo.

Como os metadados permitem que partes do pacote a ser identificado exclusivamente, isso significa que o mecanismo de atualização diferencial totalmente funções de qualquer versão de um pacote para qualquer outra versão de um pacote (supondo que o pacote de origem tem uma versão inferior de destino pacote). 

Os metadados está contido no arquivo appxblockmap. XML (os metadados mencionada anteriormente). O arquivo Appxblockmap XML é um arquivo XML que contém um dois dimensional lista informações sobre arquivos no pacote. A primeira dimensão apresenta detalhes de alto nível sobre o arquivo (por exemplo, nome e tamanho) e a segunda dimensão fornece representações de hash de 256 SHA2 de cada fatia KB 64 do arquivo (o "bloco").

Aqui está um exemplo de um arquivo appxblockmap. XML.

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

O primeiro arquivo (asset1.jpg) tem dois hashes de bloco. O primeiro hash representa o primeiro bloco de 64KB de arquivo e o representa hash segundo o KB 35 restante - considerando o arquivo é 101188 bytes.

Durante uma atualização, se o segundo bloco desse arquivo é modificado, o hash também é atualizado para refletir isso. O componente de download puxa o segundo bloco e reutiliza o primeiro bloco inalterado do pacote antigo.

Em uma escala maior, se um arquivo inteiro não é alterado (determinado por um conjunto completo de blocos não alterar), esse arquivo pode ser reutilizado do pacote existente, economizando tempo e recursos.

## <a name="app-update-constraints"></a>Restrições de atualização de aplicativo

#### <a name="updates-are-performed-within-the-same-package-family"></a>As atualizações são executadas dentro da mesma família de pacote
A família de pacote é composta do nome do pacote e do publicador. Para ser capaz de atualizar, os novos metadados de pacote precisa ser o mesmo que o pacote instalado anteriormente. 

#### <a name="app-updates-must-increment-to-a-higher-version"></a>As atualizações de aplicativo devem incrementar para uma versão superior
As atualizações de aplicativo é geral exigirá a versão do novo pacote para ser maior do que o atual. O processo de atualização do aplicativo geral não permitirá que os pacotes com versões anteriores para ser instalado por padrão. Iniciando atualização do Windows 10 1809 *'Reverter'* foi introduzido. Ele permite que os pacotes de versão inferiores ser instalado quando uma opção de substituição é fornecida como parte dos argumentos de atualização. Ele está disponível atualmente no PowerShell usando a opção ForceUpdateFromAnyVersion e no [arquivo AppInstaller](https://docs.microsoft.com/en-us/windows/msix/app-installer/update-settings).  

#### <a name="app-update-package-can-have-a-different-architecture"></a>Pacote de atualização do aplicativo pode ter uma arquitetura diferente
O pacote de atualização para o pacote de aplicativo atualmente instalado pode ser de uma arquitetura diferente, desde que a nova arquitetura é compatível com o sistema operacional no qual ele está sendo implantado. Por exemplo:  Se você tiver x86 versão do MyFavApp(v1.0.0.0) instalada em um x64, o dispositivo Windows 10 e a atualização package(v2.0.0.0) é x64 versão: MyFavApp(1.0.0.0) será atualizado para MyFavApp(v2.0.0.0) com êxito. 

#### <a name="packages-can-update-from-an-msix-to-an-msixbundle"></a>Pacotes podem atualizar de uma MSIX para um MSIXbundle
Um pacote de atualização pode ir de pacote MSIX para um pacote de MSIXbundle, mas não vice-versa. Quando um MSIXbundle é instalado, a atualização do pacote precisará permanecer um pacote. 

## <a name="optimize-differential-update-technology"></a>Otimizar a tecnologia de atualização diferencial
    
Há algumas maneiras de garantir que a tecnologia de atualização diferencial é otimizada para o máximo.

- Manter arquivos no pacote pequeno - Isso garantirá que, se uma alteração é necessária que afetaria o arquivo completo, a atualização ainda será pequeno.
- As alterações nos arquivos devem ser aditivas se possível - alterações aditivas garantirá que os dispositivos do usuário final baixem apenas os blocos alterados.
- As alterações nos arquivos deverão estar contidas aos blocos de 64KB se possível - se seu aplicativo tiver arquivos grandes e requer alterações no meio de um arquivo que contém as alterações a um conjunto de blocos ajudarão significativamente.
 

