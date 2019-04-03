---
title: Atualizações de pacote de aplicativo
description: Saiba como os aplicativos são atualizados correção diferencial.
author: mcleanbyron
ms.author: mcleans
ms.date: 09/10/2018
ms.topic: article
keywords: Windows 10, uwp, o pacote de aplicativo, atualização do aplicativo, msix, appx
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 786dd7236b8d5c8600dd36e5d3e507576df79ae7
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900508"
---
# <a name="app-package-updates"></a>Atualizações de pacote de aplicativo

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

Há algumas maneiras de utilizar essa tecnologia de atualização de diferencial.

- Manter arquivos no pacote pequeno - Isso garantirá que, se uma alteração é necessária que afetaria o arquivo completo, a atualização ainda será pequeno.
- Modificações em arquivos devem ser aditivas se possível - alterações aditivas garantirá que os dispositivos do usuário final baixem apenas os blocos alterados.
- Modificações em arquivos deverão estar contidas aos blocos de 64KB se possível - se seu aplicativo tiver arquivos grandes e requer alterações no meio de um arquivo que contém as alterações a um conjunto de blocos ajudarão significativamente.
