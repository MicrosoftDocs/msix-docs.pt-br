---
title: Desenvolvendo com os pacotes de ativo e dobramento de pacote
description: Saiba como organizar de forma eficiente seu aplicativo com pacotes de ativo e dobramento de pacote.
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, empacotamento, layout do pacote, pacote do ativo
ms.localizationpriority: medium
ms.openlocfilehash: 5cf7855e57c04bd496341580bb029b53868c11b8
ms.sourcegitcommit: cb145c63d700446e6aec1be8f9e6d2ae7481fb6b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011584"
---
# <a name="developing-with-asset-packages-and-package-folding"></a>Desenvolvendo com os pacotes de ativo e dobramento de pacote 

> [!IMPORTANT]
> Se você pretende enviar seu aplicativo para a Store, você precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) e receber aprovação para usar pacotes de ativo e dobramento de pacote.

Pacotes de ativo podem diminuir o tamanho dos pacotes globais e o tempo de publicação dos seus aplicativos para a Store. Você pode saber mais sobre pacotes de ativo e como eles podem acelerar suas iterações de desenvolvimento em [Introdução aos pacotes de ativo](asset-packages.md).

Se você estiver pensando em usar pacotes de ativo para o seu aplicativo ou já sabe que deseja usá-los, então, está provavelmente imaginando sobre como pacotes de ativo mudarão seu processo de desenvolvimento. Resumindo, o desenvolvimento de aplicativos para você permanece o mesmo, isso é possível graças ao dobramento de pacote para pacotes de ativo.

## <a name="file-access-after-splitting-your-app"></a>Acesso a arquivos depois de separar seu aplicativo

Para entender como o dobramento de pacote não afeta seu processo de desenvolvimento, vamos voltar ao inicio para entender o que acontece quando você separa seu aplicativo em vários pacotes (com pacotes de ativo ou pacotes de recursos). 

Em geral, ao separar alguns dos arquivos do seu aplicativo em outros pacotes (que não são pacotes de arquitetura), você não conseguirá acessar esses arquivos diretamente em relação ao local onde seu código é executado. Isso ocorre porque esses pacotes são instalados em diretórios diferentes de onde o pacote de arquitetura está instalado. Por exemplo, se você estiver fazendo um jogo e seu jogo estiver localizado em francês e alemão e tiver criado para computadores x86 e x64, você deverá ter esses arquivos de pacote do aplicativo no pacote de aplicativos do seu jogo:

-   MyGame_1.0_x86.appx
-   MyGame_1.0_x64.appx
-   MyGame_1.0_language-fr.appx
-   MyGame_1.0_language-de.appx

Quando o jogo é instalado no computador de um usuário, cada arquivo de pacote de aplicativo terá sua própria pasta no diretório **WindowsApps** . Portanto, para um usuário em francês executando o Windows de 64 bits, seu jogo terá a seguinte aparência:

```example
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   `-- …
|-- MyGame_1.0_language-fr
|   `-- …
`-- …(other apps)
```

Observe que os arquivos de pacote do aplicativo que não são aplicáveis ao usuário não serão instalados (os pacotes x86 e alemão). 

Para este usuário, o executável principal do jogo estará dentro da pasta **MyGame_1.0_x64** e será executado a partir daí e ele terá acesso, normalmente, apenas aos arquivos dentro dessa pasta. Para acessar os arquivos na pasta **MyGame_1.0_language-fr**, você precisaria usar as APIs de MRT ou as APIs PackageManager. As APIs do MRT podem selecionar automaticamente o arquivo mais apropriado dos idiomas instalados. você pode saber mais sobre as APIs do MRT em [Windows. ApplicationModel. Resources. Core](/uwp/api/windows.applicationmodel.resources.core). Como alternativa, você pode encontrar o local de instalação do pacote de idiomas francês usando a [Classe PackageManager](/uwp/api/Windows.Management.Deployment.PackageManager). Você nunca deve assumir o local de instalação dos pacotes do seu aplicativo, pois isso pode mudar e pode variar entre os usuários. 

## <a name="asset-package-folding"></a>Dobramento do pacote de ativo

Então como você pode acessar os arquivos nos seus pacotes de ativo? Bem, você pode continuar a usar as APIs de acesso à arquivos que você está usando para acessar qualquer outro arquivo no pacote de arquitetura. Isso ocorre porque os arquivos do pacote de ativo serão dobrados em seu pacote de arquitetura quando for instalado por meio do processo de dobramento do pacote. Além disso, como os arquivos do pacote de ativo originalmente devem ser arquivos dentro dos seus pacotes de arquitetura, isso significa que você não precisa alterar o uso da API quando você move da implantação de arquivos soltos para implantação empacotada em seu processo de desenvolvimento. 

Para saber mais sobre como funciona o dobramento de pacote, vamos começar com um exemplo. Se você tiver um projeto de jogo com a seguinte estrutura de arquivo:

```example
MyGame
|-- Audios
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Videos
|   |-- Level1
|   |   `-- ...
|   `-- Level2
|       `-- ...
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
```

Se você deseja separar seu jogo em 3 pacotes: um pacote de arquitetura x64, um pacote de ativo para áudios e um ativo para vídeos, seu jogo será dividido nesses pacotes:

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_1.0_Audios.appx
`-- Audios
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
MyGame_1.0_Videos.appx
`-- Videos
    |-- Level1
    |   `-- ...
    `-- Level2
        `-- ...
```

Quando você instala o seu jogo, o pacote x64 será implantado pela primeira vez. Então, os dois pacotes de ativo serão ainda implantados em suas próprias pastas, como **MyGame_1.0_language-fr** do nosso exemplo anterior. No entanto, devido ao dobramento de pacote, os arquivos do pacote de ativo também terão um link físico para constar na pasta **MyGame_1.0_x64** (assim, mesmo que os arquivos sejam exibidos em dois locais, eles não ocuparão duas vezes o espaço no disco). O local em que os arquivos do pacote de ativo aparecerão é exatamente o local onde eles estão em relação à raiz do pacote. Aqui está a aparência que o layout final do jogo implantado terá:

```example 
C:\Program Files\WindowsApps\
|-- MyGame_1.0_x64
|   |-- Audios
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Videos
|   |   |-- Level1
|   |   |   `-- ...
|   |   `-- Level2
|   |       `-- ...
|   |-- Engine
|   |   `-- ...
|   |-- XboxLive
|   |   `-- ...
|   `-- Game.exe
|-- MyGame_1.0_Audios
|   `-- Audios
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
|-- MyGame_1.0_Videos
|   `-- Videos
|       |-- Level1
|       |   `-- ...
|       `-- Level2
|           `-- ...
`-- …(other apps)
```

Ao usar o dobramento de pacote para pacotes de ativo, você ainda pode acessar os arquivos que separou em pacotes de ativo da mesma maneira (observe que a pasta de arquitetura tem a mesma estrutura da pasta de projeto original) e você pode adicionar pacotes de ativo ou mover arquivos entre os pacotes de ativo sem afetar o seu código. 

Agora, para um exemplo de dobramento de pacote mais complicado. Digamos que você deseja separar seus arquivos de acordo com o nível, e caso queira manter a mesma estrutura da pasta de projeto original, seus pacotes devem ter a seguinte aparência:

```example
MyGame_1.0_x64.appx
|-- Engine
|   `-- ...
|-- XboxLive
|   `-- ...
`-- Game.exe
MyGame_Level1.appx
|-- Audios
|   `-- Level1
|       `-- ...
`-- Videos
    `-- Level1
        `-- ...

MyGame_Level2.appx
|-- Audios
|   `-- Level2
|       `-- ...
`-- Videos
    `-- Level2
        `-- ...
```
Isso permitirá que as pastas **Level1** e os arquivos no pacote **MyGame_Level1**, e as pastas **Level2** e os arquivos no pacote **MyGame_Level2** sejam mesclados nas pastas **Audios** e **Videos** durante o dobramento do pacote. Assim, como regra geral, o caminho relativo designado para arquivos empacotados no arquivo de mapeamento ou [layout de empacotamento](packaging-layout.md) para MakeAppx.exe é o caminho que você deve usar para acessá-los após dobramento do pacote. 

Por fim, se houver dois arquivos em pacotes de ativos diferentes que têm os mesmos caminhos, isso causará um conflito durante o dobramento do pacote. Se ocorrer uma colisão, a implantação do seu aplicativo irá resultar em um erro e falhar. Além disso, devido ao dobramento do pacote tirar proveito dos links físicos, se você usar pacotes de ativo, seu aplicativo não poderá ser implantado em unidades não NTFS. Se você souber que seu aplicativo será provavelmente movido para unidades removíveis pelos usuários, então, você não deve usar pacotes de ativo.