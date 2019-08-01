---
title: Criação do pacote com o layout de empacotamento
description: O layout de empacotamento é um documento único que descreve a estrutura de empacotamento do aplicativo. Ele especifica os pacotes de um aplicativo (principal e opcional), os pacotes no lote e os arquivos nos pacotes.
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, empacotamento, layout do pacote, pacote do ativo
ms.localizationpriority: medium
ms.openlocfilehash: 50284e2878a649380c842f82d03d6530aa493c99
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68689955"
---
# <a name="package-creation-with-the-packaging-layout"></a>Criação do pacote com o layout de empacotamento  

Com a introdução de pacotes de ativo, os desenvolvedores agora têm as ferramentas para criar mais pacotes além de mais tipos de pacotes. Conforme um aplicativo fica maior e mais complexo, ele geralmente será composto de mais pacotes e a dificuldade de gerenciar esses pacotes aumentará (especialmente se você estiver criando fora do Visual Studio e usando arquivos de mapeamento). Para simplificar o gerenciamento da estrutura de empacotamento do aplicativo, você pode usar o layout de empacotamento com suporte pelo MakeAppx.exe. 

O layout de empacotamento é um documento XML único que descreve a estrutura de empacotamento do aplicativo. Ele especifica os pacotes de um aplicativo (principal e opcional), os pacotes no lote e os arquivos nos pacotes. Arquivos podem ser selecionados de locais de rede, unidades e pastas diferentes. Curingas podem ser usados para selecionar ou excluir arquivos.

Depois que o layout de empacotamento de um aplicativo tiver sido configurado, ele é usado com o MakeAppx.exe para criar todos os pacotes para um aplicativo em uma única chamada de linha de comando. O layout de empacotamento pode ser editado para alterar a estrutura de pacote para atender às suas necessidades de implantação. 


## <a name="simple-packaging-layout-example"></a>Exemplo de layout de empacotamento simples

Aqui está um exemplo da aparência de um layout de empacotamento simples:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>
    
    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>

  </PackageFamily>
</PackagingLayout>
```

Vamos dividir este exemplo para entender como ele funciona.

### <a name="packagefamily"></a>PackageFamily

Esse layout de empacotamento criará um único arquivo simples de pacote de aplicativo com um pacote de arquitetura x64 e um pacote de ativos de "mídia". 

O elemento **PackageFamily** é usado para definir um lote de aplicativo. Você deve usar o atributo **ManifestPath** para fornecer um **AppxManifest** para o pacote, o **AppxManifest** deve corresponder ao **AppxManifest** para o pacote de arquitetura do pacote. O atributo **ID** também deve ser fornecido. Isso é usado com MakeAppx.exe durante a criação de pacote para que você possa criar apenas esse pacote se quiser, e isso será o nome do arquivo do pacote criado. O atributo **FlatBundle** é usado para descrever o tipo de pacote que você deseja criar, **true** para um pacote simples (que você pode ler mais sobre aqui), e **false** para um pacote clássico. O atributo **ResourceManager** é usado para especificar se os pacotes de recursos dentro esse pacote usarão MRT para acessar os arquivos. Por padrão, é **true**, mas a partir do Windows 10, versão 1803, isso ainda não está definido, portanto, esse atributo deve ser definido como **false**.

### <a name="package-and-assetpackage"></a>Pacote e AssetPackage

Dentro do **PackageFamily**, os pacotes que contêm o lote de aplicativo ou referências são definidos. Aqui, o pacote de arquitetura (também chamado de pacote principal) é definido com o elemento **Package** e o pacote de ativo é definido com o elemento **AssetPackage**. O pacote de arquitetura deve especificar para qual arquitetura é o pacote, "x64", "x86", "arm" ou "neutra". Você pode fornecer também diretamente (opcionalmente) uma **AppxManifest** especificamente para esse pacote usando o atributo **ManifestPath** novamente. Se um **AppxManifest** não for fornecido, um será gerado automaticamente com o **AppxManifest** fornecido para o **PackageFamily**. 

Por padrão um **AppxManifest** será gerado para cada pacote dentro do lote. Para o pacote de ativo, você também pode definir o atributo **AllowExecution**. Configurá-lo como **false** (padrão), ajuda a reduzir o tempo de publicação para o seu aplicativo, visto que pacotes que não precisam ser executados não têm a verificação de vírus bloqueada no processo de publicação (você pode saber mais sobre isso em [Introdução aos pacotes de ativo](asset-packages.md)). 

### <a name="files"></a>Arquivos

Dentro de cada definição de pacote, você pode usar o elemento **File** para selecionar arquivos a serem incluídos neste pacote. O atributo **SourcePath** é onde os arquivos estão localmente. Você pode selecionar arquivos de pastas diferentes (fornecendo caminhos relativos), unidades diferentes (fornecendo caminhos absolutos) ou até mesmo os compartilhamentos de rede (fornecendo algo como `\\myshare\myapp\*`). O **DestinationPath** é onde os arquivos acabarão dentro do pacote, relativo à raiz do pacote. **ExcludePath** pode ser usado (em vez de outros dois atributos) para selecionar arquivos a serem excluídos daqueles selecionado por outros atributos **SourcePath** dos elementos **File** dentro do mesmo pacote.

Cada elemento **File** pode ser usado para selecionar vários arquivos usando curingas. Em geral, um único curinga (`*`) pode ser usado em qualquer lugar dentro do caminho quantas vezes quiser. No entanto, um único curinga corresponderá somente aos arquivos dentro de uma pasta e não a todas as subpastas. Por exemplo, `C:\MyGame\*\*` pode ser usado no **SourcePath** para selecionar os arquivos `C:\MyGame\Audios\UI.mp3` e `C:\MyGame\Videos\intro.mp4`, mas não pode selecionar `C:\MyGame\Audios\Level1\warp.mp3`. O curinga duplo (`**`) também pode ser usado no lugar da pastas de ou nomes de arquivo para corresponder algo recursivamente (mas não pode ser ao lado de nomes parciais). Por exemplo, `C:\MyGame\**\Level1\**` pode selecionar `C:\MyGame\Audios\Level1\warp.mp3` e `C:\MyGame\Videos\Bonus\Level1\DLC1\intro.mp4`. Curingas também podem ser usados para alterar diretamente nomes de arquivo como parte do processo de empacotamento, se os caracteres curinga forem usados em locais diferentes entre a origem e destino. Por exemplo, tendo `C:\MyGame\Audios\*` para **SourcePath** e `Sound\copy_*` para **DestinationPath** pode selecionar `C:\MyGame\Audios\UI.mp3` e fazer com que ele apareça no pacote como `Sound\copy_UI.mp3`. Em geral, o número de curingas únicos e duplos devem ser o mesmo para **SourcePath** e **DestinationPath** de um único elemento **File**.

## <a name="advanced-packaging-layout-example"></a>Exemplo de layout de empacotamento avançado

Aqui está um exemplo de um layout de empacotamento mais complicado:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <!-- Main game -->
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>

    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>
    
    <!-- English resource package -->
    <ResourcePackage ID="en">
      <Files>
        <File DestinationPath="english\**" SourcePath="C:\mygame\english\**"/>
      </Files>
      <Resources Default="true">
        <Resource Language="en"/>
      </Resources>
    </ResourcePackage>

    <!-- French resource package -->
    <ResourcePackage ID="fr">
      <Files>
        <File DestinationPath="french\**" SourcePath="C:\mygame\french\**"/>
      </Files>
      <Resources>
        <Resource Language="fr"/>
      </Resources>
    </ResourcePackage>
  </PackageFamily>

  <!-- DLC in the related set -->
  <PackageFamily ID="DLC" Optional="true" ManifestPath="C:\DLC\appxmanifest.xml">
    <Package ID="DLC.x86" Architecture="x86">
      <Files>
        <File DestinationPath="**" SourcePath="C:\DLC\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- DLC not part of the related set -->
  <PackageFamily ID="Themes" Optional="true" RelatedSet="false" ManifestPath="C:\themes\appxmanifest.xml">
    <Package ID="Themes.main" Architecture="neutral">
      <Files>
        <File DestinationPath="**" SourcePath="C:\themes\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- Existing packages that need to be included/referenced in the bundle -->
  <PrebuiltPackage Path="C:\prebuilt\DLC2.appxbundle" />

</PackagingLayout>
```

Este exemplo é diferente do exemplo simples com a adição dos elementos **ResourcePackage** e **Optional**.

Pacotes de recursos podem ser especificados com o elemento **ResourcePackage**. Dentro do **ResourcePackage**, o elemento **Resources** deve ser usado para especificar os qualificadores de recursos de pacotes de recursos. Os qualificadores de recursos são os recursos compatíveis com o pacote de recursos, aqui, podemos ver que existem dois pacotes de recursos definidos e cada um deles contém os arquivos específicos de inglês e francês. Um pacote de recursos pode ter mais de um qualificador, isso pode ser feito adicionando outro elemento **Resource** dentro de **Resources**. Um recurso padrão para a dimensão do recurso também deve ser especificado se a dimensão existir (dimensões sendo idioma, escala e dxfl). Aqui, vemos que o inglês é o idioma padrão, que significa que, para os usuários que não tem um idioma do sistema definido como francês, eles farão fallback para baixar o pacote de recurso inglês e exibir em inglês.

Cada pacote opcional tem seu próprio nome de família do pacote distinto e deve ser definido com elementos **PackageFamily**, ao especificar o atributo **Optional** para **true**. O atributo **RelatedSet** é usado para especificar se o pacote opcional está dentro do conjunto relacionado (por padrão isso é verdadeiro), e se o pacote opcional deve ser atualizado com o pacote principal.

O elemento **PrebuiltPackage** é usado para adicionar pacotes que não estão definidos no layout de empacotamento a ser incluído ou referenciado nos arquivos de pacote de aplicativo a serem compilados. Nesse caso, outro pacote opcional DLC está sendo incluído aqui para que o arquivo de pacote do aplicativo primário possa fazer referência a ele e fazer com que ele faça parte do conjunto relacionado.

## <a name="build-app-packages-with-a-packaging-layout-and-makeappxexe"></a>Criar pacotes de aplicativos com um layout de empacotamento e MakeAppx.exe

Quando você tiver o layout de empacotamento para seu aplicativo, você pode começar a usar o MakeAppx.exe para criar os pacotes do seu aplicativo. Para criar todos os pacotes definidos no layout de empacotamento, use o comando:

```cmd
MakeAppx.exe build /f PackagingLayout.xml /op OutputPackages\
```

No entanto, se você estiver atualizando seu aplicativo e alguns pacotes não contêm quaisquer arquivos alterados, você pode criar apenas os pacotes que foram alterados. Usando o exemplo de layout de empacotamento simples nesta página e criando pacote de arquitetura x64, isso é a aparência do nosso comando:

```cmd
MakeAppx.exe build /f PackagingLayout.xml /id "x64" /ip PreviousVersion\ /op OutputPackages\ /iv
```

O sinalizador `/id` pode ser usado para selecionar os pacotes a serem construídos com o layout de empacotamento, correspondente ao atributo do **ID** no layout. O `/ip` é usado para indicar onde a versão anterior dos pacotes estão neste caso. A versão anterior deve ser fornecida porque o arquivo de pacote de aplicativo ainda precisa fazer referência à versão anterior do pacote de **mídia** . O sinalizador `/iv` é usado para incrementar a versão dos pacotes que está sendo criado automaticamente (ao invés de alterar a versão no **AppxManifest**). Como alternativa, os switches `/pv` e `/bv` podem ser usados para fornecer diretamente uma versão do pacote (para todos os pacotes a serem criados) e uma versão do lote (para todos os lotes a serem criados), respectivamente.
Usando o exemplo de layout de empacotamento avançado nesta página, se você quiser criar somente o lote opcional **Themes** e o pacote do aplicativo **Themes.main** que faz referência a ele, você usaria esse comando:

```cmd
MakeAppx.exe build /f PackagingLayout.xml /id "Themes" /op OutputPackages\ /bc /nbp
```

O sinalizador `/bc` é usado para indicar que os filhos do lote **Themes** também devem ser compilados (neste caso **Themes.main** será compilado). O sinalizador `/nbp` é usado para indicar que o responsável do lote **Themes** não deve ser compilado. O pai de **temas**, que é um pacote de aplicativo opcional, é o pacote de aplicativo primário: **MyGame**. Geralmente para um pacote opcional em um conjunto relacionado, o lote de aplicativo principal deve também ser criado para o pacote a ser instalado, desde que o pacote opcional também seja referenciado no lote de aplicativo principal quando ele estiver em um conjunto relacionado (para garantir o controle de versão entre os pacotes principais e opcionais). A relação do responsável entre pacotes é ilustrada no diagrama a seguir:

![Diagrama do layout de empacotamento](images/packaging-layout.png)
