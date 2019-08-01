---
ms.assetid: ff2523cb-8109-42be-9dfc-cb5d09002574
title: Criar e converter um mapa de grupo de conteúdo de origem
description: Para preparar seu aplicativo para a instalação de streaming, você precisará criar um mapa de grupo de conteúdo. Este artigo irá ajudá-lo com as especificidades de criar e converter um mapa de grupo de conteúdo, fornecendo algumas dicas e truques ao longo do caminho.
ms.date: 07/02/2019
author: dianmsft
ms.author: diahar
ms.topic: article
keywords: Windows 10, msix, UWP, mapa de grupo de conteúdo, instalação de streaming, instalação de streaming de aplicativo, mapa de grupo de conteúdo de origem
ms.localizationpriority: medium
ms.openlocfilehash: 4908c03e9c300b56ac21b7f8b268b78ee27af915
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689885"
---
# <a name="create-and-convert-a-source-content-group-map"></a>Criar e converter um mapa de grupo de conteúdo de origem

Para preparar seu aplicativo para a instalação de streaming, você precisará criar um mapa de grupo de conteúdo. Este artigo irá ajudá-lo com as especificidades de criar e converter um mapa de grupo de conteúdo, fornecendo algumas dicas e truques ao longo do caminho.

## <a name="creating-the-source-content-group-map"></a>Criar o mapa de grupo de conteúdo de origem

Você precisará criar um arquivo `SourceAppxContentGroupMap.xml` e, em seguida, usar o Visual Studio ou a ferramenta **MakeAppx.exe** para converter esse arquivo para a versão final:`AppxContentGroupMap.xml`. É possível pular uma etapa criando o `AppxContentGroupMap.xml` do zero, mas é recomendável (e geralmente mais fácil) criar o `SourceAppxContentGroupMap.xml` e convertê-lo, uma vez que caracteres curinga não são permitidos no `AppxContentGroupMap.xml` (e eles são muito úteis). 

Vamos examinar um cenário simples em que a instalação do streaming de aplicativos é benéfica. 

Digamos que você criou um jogo, mas o tamanho do seu aplicativo final é de mais de 100 GB. Isso vai levar muito tempo para baixar da Microsoft Store, o que pode ser inconveniente. Se você optar por usar a instalação de streaming de aplicativo, poderá especificar a ordem na qual os arquivos do aplicativo serão baixados. Ao dizer à loja que faça o download de arquivos essenciais primeiro, o usuário poderá se envolver com seu aplicativo mais cedo, enquanto outros arquivos não essenciais serão baixados em segundo plano.

> [!NOTE]
> Usar a instalação do streaming de aplicativos depende muito da organização de arquivos do seu aplicativo. É recomendável que você pense no layout do conteúdo do aplicativo em relação à instalação do streaming de aplicativos assim que possível para tornar o segmento dos arquivos do aplicativo mais simples.

Primeiro, vamos criar um arquivo `SourceAppxContentGroupMap.xml`.

Antes de entrarmos em detalhes, veja aqui um exemplo de um arquivo simples e completo `SourceAppxContentGroupMap.xml`:

```xml
<?xml version="1.0" encoding="utf-8"?>  
<ContentGroupMap xmlns="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap" 
                 xmlns:s="http://schemas.microsoft.com/appx/2016/sourcecontentgroupmap"> 
    <Required>
        <ContentGroup Name="Required">
            <File Name="StreamingTestApp.exe"/>
        </ContentGroup>
    </Required>
    <Automatic>
        <ContentGroup Name="Level2">
            <File Name="Assets\Level2\*"/>
        </ContentGroup>
        <ContentGroup Name="Level3">
            <File Name="Assets\Level3\*"/>
        </ContentGroup>
    </Automatic>
</ContentGroupMap>
```

Existem dois componentes principais para um mapa de grupo de conteúdo: a seção **necessária**, que contém o grupo de conteúdo exigido e a seção **automática**, que pode conter vários grupos de conteúdo automático.

### <a name="required-content-group"></a>Grupo de conteúdo necessário

O grupo de conteúdo necessário é um único grupo de conteúdo dentro do elemento `<Required>` do `SourceAppxContentGroupMap.xml`. Um grupo de conteúdo exigido deve conter todos os arquivos essenciais necessários para iniciar o aplicativo com a experiência mínima do usuário. Devido à compilação nativa do .NET, todo o código (o executável do aplicativo) deve ser parte do grupo necessário, deixando ativos e outros arquivos para os grupos automáticos.

Por exemplo, se o seu aplicativo for um jogo, o grupo necessário pode incluir arquivos usados no menu principal ou na tela inicial do jogo.

Veja o trecho do nosso arquivo de exemplo original `SourceAppxContentGroupMap.xml`: 
```xml
<Required>
    <ContentGroup Name="Required">
        <File Name="StreamingTestApp.exe"/>
    </ContentGroup>
</Required>
```

Há algumas coisas importantes a serem observadas aqui:

- O `<ContentGroup>` dentro do elemento `<Required>`**deve** ser nomeado para "Necessário." Este nome é reservado apenas para o grupo de conteúdo necessário e não pode ser usado com nenhum outro`<ContentGroup>` no mapa do grupo de conteúdo final.
- Há apenas um `<ContentGroup>`. Isso é intencional, uma vez que deve haver apenas um grupo de arquivos essenciais.
- O arquivo neste exemplo é um único arquivo `.exe`. Um grupo de conteúdo necessário não está restrito a um arquivo, pode haver vários. 

Uma maneira fácil de começar a escrever este arquivo é abrir uma nova página em seu editor de texto favorito, clicar rapidamente a função "Salvar como" do seu arquivo na pasta do projeto do seu aplicativo e nomear o arquivo recém-criado: `SourceAppxContentGroupMap.xml`.

> [!IMPORTANT]
> Se você estiver desenvolvendo um C++ aplicativo, precisará ajustar as propriedades do arquivo do seu `SourceAppxContentGroupMap.xml`. Defina a propriedade `Content` para **verdadeiro** e a propriedade `File Type` para **arquivo XML**.

Quando você estiver criando o `SourceAppxContentGroupMap.xml`, é útil tirar proveito do uso de curingas em nomes de arquivo, para obter mais informações, consulte a seção [Dicas e truques para o uso de curingas](#wildcards).

Se você desenvolveu seu aplicativo usando o Visual Studio, recomenda-se que você inclua isso no seu grupo de conteúdo necessário:

```xml
<File Name="*"/>
<File Name="WinMetadata\*"/>
<File Name="Properties\*"/>
<File Name="Assets\*Logo*"/>
<File Name="Assets\*SplashScreen*"/>
```

Adicionar o nome do arquivo curinga simples incluirá arquivos adicionados ao diretório do projeto do Visual Studio, como o executável do aplicativo ou DLLs. As pastas WinMetadata e Propriedades devem incluir as outras pastas que o Visual Studio gera. Os curingas dos ativos são para selecionar as imagens de Logo e SplashScreen necessárias para que o aplicativo seja instalado.

Note que você não pode usar o curinga duplo "**", na raiz da estrutura do arquivo para incluir todos os arquivos no projeto, pois isso irá falhar ao tentar converter `SourceAppxContentGroupMap.xml` para o `AppxContentGroupMap.xml` final.

Também é importante observar que os arquivos de pegada (AppxManifest.xml, AppxSignature.p7x, resources.pri, etc.) não devem ser incluídos no mapa do grupo de conteúdo. Se os arquivos de pegada estiverem incluídos dentro de um dos nomes de arquivo curinga que você especificar, eles serão ignorados.

### <a name="automatic-content-groups"></a>Grupos de conteúdo automáticos

Grupos de conteúdo automáticos são os recursos que são baixados em segundo plano enquanto o usuário está interagindo com os grupos de conteúdo já baixados. Estes contêm quaisquer arquivos adicionais que não são essenciais para o lançamento do aplicativo. Por exemplo, você pode dividir grupos de conteúdo automáticos em diferentes níveis, definindo cada nível como um grupo de conteúdo separado. Conforme observado na seção do grupo de conteúdo necessário: devido à compilação .NET Native, todo o código (o executável do aplicativo) deve fazer parte do grupo desejado, deixando ativos e outros arquivos para os grupos automáticos.

Vamos dar uma olhada no grupo de conteúdo automático do nosso exemplo `SourceAppxContentGroupMap.xml`:
```xml
<Automatic>
    <ContentGroup Name="Level2">
        <File Name="Assets\Level2\*"/>
    </ContentGroup>
    <ContentGroup Name="Level3">
        <File Name="Assets\Level3\*"/>
    </ContentGroup>
</Automatic>
```

O layout do grupo automático é bastante semelhante ao grupo necessário, com poucas exceções:

- Existem vários grupos de conteúdo.
- Grupos de conteúdo automáticos podem ter nomes exclusivos, exceto o nome "Necessário", que está reservado para o grupo de conteúdo desejado.
- Os grupos de conteúdo automático não podem conter **qualquer** arquivo do grupo de conteúdo necessário. 
- Um grupo de conteúdo automático pode conter arquivos que também estão em outros grupos de conteúdo automático. Os arquivos serão baixados apenas uma vez, e serão baixados com o primeiro grupo de conteúdo automático que os contém.

#### Dicas e truques para o uso de curingas<a name="wildcards"></a>

O layout do arquivo para os mapas de grupos de conteúdo é sempre relativo à sua pasta raiz do projeto.

No nosso exemplo, curingas são usados em ambos elementos `<ContentGroup>` para recuperar todos os arquivos dentro do nível de um arquivo de "Assets\Level2" ou "Assets\Level3". Se você estiver usando uma estrutura de pastas mais profunda, você pode usar o curinga duplo:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\Level2\**"/>
</ContentGroup>
```

Você também pode usar curingas com texto para nomes de arquivos. Por exemplo, se você deseja incluir todos os arquivos em sua pasta "Ativos" com um nome de arquivo que contenha "Nível 2", você pode usar algo assim:

```xml
<ContentGroup Name="Level2">
    <File Name="Assets\*Level2*"/>
</ContentGroup>
```

## <a name="convert-sourceappxcontentgroupmapxml-to-appxcontentgroupmapxml"></a>Converter SourceAppxContentGroupMap.xml em AppxContentGroupMap.xml

Para converter o `SourceAppxContentGroupMap.xml` para a versão final, `AppxContentGroupMap.xml`, você pode usar o Visual Studio 2017 ou a ferramenta **MakeAppx.exe** de linha de comando.

Para usar o Visual Studio para converter seu mapa de grupo de conteúdo:
1. Adicione o `SourceAppxContentGroupMap.xml`para sua pasta do projeto
2. Alterar a ação de compilação do `SourceAppxContentGroupMap.xml` para "AppxSourceContentGroupMap" na janela Propriedades
2. Clique com o botão direito do mouse no projeto no explorador de solução
3. Navegue até a Loja -> Converter Arquivo do Mapa de Grupo de Conteúdo

Se você não desenvolveu seu aplicativo no Visual Studio, ou se você preferir usar a linha de comando, use a ferramenta **MakeAppx.exe** para converter seu `SourceAppxContentGroupMap.xml`. 

Um simples comando **MakeAppx.exe** pode parecer com isto:
```syntax
MakeAppx convertCGM /s MyApp\SourceAppxContentGroupMap.xml /f MyApp\AppxContentGroupMap.xml /d MyApp\
```

A opção /s especifica o caminho para o `SourceAppxContentGroupMap.xml`, e /f especifica o caminho para o `AppxContentGroupMap.xml`. A opção final, /d, especifica qual diretório deve ser usado para expandir caracteres de nome de arquivo, neste caso, é o diretório do projeto do aplicativo.

Para obter mais informações sobre as opções que você pode usar com o **MakeAppx.exe**, abra um prompt de comando, navegue até **MakeAppx.exe** e digite:

```syntax
MakeAppx convertCGM /?
```

Isso é tudo o que você precisa para deixar seu `AppxContentGroupMap.xml` final pronto para seu aplicativo! Ainda há mais a fazer para que seu aplicativo esteja totalmente pronto para o Microsoft Store. Para obter mais informações sobre o processo completo de adição de instalação de streaming ao seu aplicativo, confira [esta postagem no blog](https://blogs.msdn.microsoft.com/appinstaller/2017/03/15/uwp-streaming-app-installation/).
