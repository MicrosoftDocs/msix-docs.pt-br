---
title: Criar um pacote de aplicativo com a ferramenta MakeAppx.exe
description: MakeAppx.exe cria, criptografa, descriptografa e extrai arquivos de pacotes e lotes de apps.
ms.date: 07/29/2019
ms.topic: article
keywords: windows 10, uwp, empacotamento
ms.assetid: 7c1c3355-8bf7-4c9f-b13b-2b9874b7c63c
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 7720d585d56199dc89616b7220ef72e0d1a559e4
ms.sourcegitcommit: de0c711ce28851ef6976a71dd1317291f278b4d1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96612735"
---
# <a name="create-an-msix-package-or-bundle-with-makeappxexe"></a>Criar um pacote ou grupo MSIX com MakeAppx.exe

**MakeAppx.exe** cria os pacotes de aplicativos (. msix ou. AppX) e os pacotes de pacote de aplicativo (. msixbundle ou. appxbundle). Além disso, a **MakeAppx.exe** extrai arquivos de um pacote ou lote de aplicativo e criptografa ou descriptografa pacotes e lotes de aplicativos. Essa ferramenta está incluída no SDK do Windows 10 e pode ser usada em um prompt de comando ou um arquivo de script.

Para obter informações sobre como usar **MakeApp.exe** para empacotar um aplicativo de área de trabalho, consulte [empacotar um aplicativo de área de trabalho manualmente](../desktop/desktop-to-uwp-manual-conversion.md).

> [!IMPORTANT]
> Se você usou o Visual Studio para desenvolver seu app, é recomendável que você use o Assistente do Visual Studio para criar e assinar seu pacote de app. Para obter mais informações, consulte [empacotar um aplicativo UWP com o Visual Studio](packaging-uwp-apps.md) e [empacotar um aplicativo de desktop do código-fonte usando o Visual Studio](../desktop/desktop-to-uwp-packaging-dot-net.md).

> [!IMPORTANT]
> Observe que **MakeAppx.exe** não cria um [arquivo de carregamento de pacote de aplicativo (. appxupload ou. msixupload)](packaging-uwp-apps.md#types-of-app-packages), que é o tipo recomendado de pacote de aplicativo válido para [envios para o Partner Center](/windows/uwp/publish/upload-app-packages). O arquivo de upload do pacote do aplicativo normalmente é [criado como parte do processo de empacotamento do Visual Studio](packaging-uwp-apps.md#generate-an-app-package-upload-file-for-store-submission), embora também possa ser criado manualmente.

## <a name="using-makeappxexe"></a>Usando a MakeAppx.exe

Dependendo do seu caminho de instalação do SDK, a **MakeAppx.exe** é instalada no computador Windows 10 nos seguintes locais:
- x86: C:\Arquivos de programas (x86) \Windows Kits\10\bin \\ &lt; número de Build &gt;\x86\makeappx.exe
- x64: C:\Arquivos de programas (x86) \Windows Kits\10\bin \\ &lt; número de Build &gt;\x64\makeappx.exe

Não há nenhuma versão ARM dessa ferramenta.

### <a name="makeappxexe-syntax-and-options"></a>Sintaxe e opções da MakeAppx.exe

Sintaxe geral da **MakeAppx.exe**:

``` Usage
MakeAppx <command> [options]      
```

A tabela a seguir descreve os comandos da **MakeAppx.exe**.

| **Comando**   | **Descrição**                       |
|---------------|---------------------------------------|
| pack          | Cria um pacote.                    |
| unpack        | Extrai todos os arquivos do pacote especificado para o diretório de saída especificado. |
| bundle        | Cria um pacote.                     |
| unbundle      | Descompacta todos os pacotes em um subdiretório no caminho de saída especificado, nomeados após o pacote ou o nome completo. |
| encrypt       | Cria um pacote ou lote de aplicativo criptografado a partir do pacote/lote de entrada no pacote/lote de saída especificado. |
| descriptografar       | Cria um pacote ou lote de aplicativo descriptografado a partir do pacote/lote de aplicativo de entrada no pacote/lote de saída especificado. |


Esta lista de opções se aplica a todos os comandos:

| **Opção**    | **Descrição**                       |
|---------------|---------------------------------------|
| /d            | Especifica a entrada, a saída ou o diretório de conteúdo. |
| /l            | Usado para pacotes localizados. A validação padrão é bloqueada em pacotes localizados. Essas opção desabilita apenas aquela validação específica, sem exigir que todas as validações sejam desabilitadas. |
| /kf           | Criptografa ou descriptografa o pacote ou lote usando a chave do arquivo de chave especificado. Essa opção não pode ser usada com /kt. |
| /kt           | Criptografa ou descriptografa o pacote ou lote usando a chave de teste global. Essa opção não pode ser usada com /kf. |
| /no           | Impede uma substituição do arquivo de saída, se houver. Se você não especificar essa opção ou a opção /o, será perguntado ao usuário se ele deseja substituir o arquivo. |
| /nv           | Ignora a validação semântica. Se você não especificar essa opção, a ferramenta executará uma validação completa do pacote. |
| /o            | Substitui o arquivo de saída, se houver. Se você não especificar essa opção ou a opção /no, será perguntado ao usuário se ele deseja substituir o arquivo. |
| /p            | Especifica o pacote ou lote de aplicativo.  |
| /v            | Habilita a saída do log detalhado no console. |
| /?            | Exibe o texto da Ajuda.                   |


A lista a seguir contém argumentos possíveis:

| **Argument**                          | **Descrição**                       |
|---------------------------------------|---------------------------------------|
| &lt;output package name&gt;           | O nome do pacote criado. Esse é o nome do arquivo anexado com. msix ou. Appx. |
| &lt;encrypted output package name&gt; | O nome do pacote criptografado criado. Esse é o nome do arquivo anexado com. emsix ou. eappx. |
| &lt;input package name&gt;            | O nome do pacote. Esse é o nome do arquivo anexado com. msix ou. Appx. |
| &lt;encrypted input package name&gt;  | O nome do pacote criptografado. Esse é o nome do arquivo anexado com. emsix ou. eappx. |
| &lt;output bundle name&gt;            | O nome do lote criado. Esse é o nome do arquivo anexado com. msixbundle ou. appxbundle. |
| &lt;encrypted output bundle name&gt;  | O nome do lote criptografado criado. Esse é o nome do arquivo anexado com. emsixbundle ou. eappxbundle. |
| &lt;input bundle name&gt;             | O nome do lote. Esse é o nome do arquivo anexado com. msixbundle ou. appxbundle. |
| &lt;encrypted input bundle name&gt;   | O nome do lote criptografado. Esse é o nome do arquivo anexado com. emsixbundle ou. eappxbundle. |
| &lt;content directory&gt;             | Caminho do conteúdo do pacote ou lote de aplicativo. |
| &lt;mapping file&gt;                  | Nome do arquivo que especifica a origem e o destino do pacote. |
| &lt;output directory&gt;              | Caminho do diretório de pacotes e lotes de saída. |
| &lt;key file&gt;                      | Nome do arquivo que contém uma chave de criptografia ou descriptografia. |
| &lt;algorithm ID&gt;                  | Algoritmos usados durante a criação de um mapa de blocos. Os algoritmos válidos incluem: SHA256 (padrão), SHA384, SHA512. |


### <a name="create-an-app-package"></a>Criar um pacote de aplicativo

Um pacote de aplicativo é um conjunto completo de arquivos do aplicativo empacotado em um arquivo de pacote. msix ou. Appx. Para criar um pacote de aplicativo usando o comando **pack**, você deve informar um diretório de conteúdo ou um arquivo de mapeamento para o local do pacote. Você também pode criptografar um pacote ao criá-lo. Se desejar criptografar o pacote, você deverá usar /ep e especificar se está usando um arquivo de chave (/kf) ou a tecla de teste global (/kt). Para obter mais informações sobre como criar um pacote criptografado, consulte [Criptografar ou descriptografar um pacote ou um lote](#encrypt-or-decrypt-a-package-or-bundle).

Opções específicas ao comando **pack**:

| **Opção**    | **Descrição**                       |
|---------------|---------------------------------------|
| /f            | Especifica o arquivo de mapeamento.           |
| /h            | Especifica o algoritmo de hash a ser usado ao criar o mapa de blocos. Isso pode ser usado apenas com o comando pack. Os algoritmos válidos incluem: SHA256 (padrão), SHA384, SHA512. |
| /m            | Especifica o caminho de um manifesto de aplicativo de entrada que será usado como base para gerar o pacote de aplicativo de saída ou o manifesto do pacote de recursos.  Ao usar essa opção, você também deve usar /f e incluir uma seção [ResourceMetadata] no arquivo de mapeamento para especificar as dimensões do recurso a serem incluídas no manifesto gerado.|
| /nc           | Impede a compactação dos arquivos do pacote. Por padrão, os arquivos são compactados com base no tipo de arquivo detectado. |
| /r            | Cria um pacote de recursos. Isso deve ser usado com /m e implica o uso da opção /l. |  


Os exemplos de uso a seguir mostram algumas opções de sintaxe possíveis para o comando **pack**:

``` syntax
MakeAppx pack [options] /d <content directory> /p <output package name>
MakeAppx pack [options] /f <mapping file> /p <output package name>
MakeAppx pack [options] /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /r /m <app package manifest> /f <mapping file> /p <output package name>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kf <key file>
MakeAppx pack [options] /d <content directory> /ep <encrypted output package name> /kt

```
A seguir estão alguns exemplos de linha de comando para o comando **pack**:

``` examples
MakeAppx pack /v /h SHA256 /d "C:\My Files" /p MyPackage.msix
MakeAppx pack /v /o /f MyMapping.txt /p MyPackage.msix
MakeAppx pack /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p AppPackage.msix
MakeAppx pack /r /m "MyApp\AppxManifest.xml" /f MyMapping.txt /p ResourcePackage.msix
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kf MyKeyFile.txt
MakeAppx pack /v /h SHA256 /d "C:\My Files" /ep MyPackage.emsix /kt
```

### <a name="create-an-app-bundle"></a>Criar um lote de aplicativo

Um lote de aplicativo é semelhante a um pacote de aplicativo, mas um lote pode reduzir o tamanho do app que os usuários baixam. Os lotes de aplicativos são úteis para ativos específicos ao idioma, ativos de escala de imagem variáveis ou recursos que se aplicam a versões específicas do Microsoft DirectX, por exemplo. Semelhante à criação de um pacote de aplicativo criptografado, também é possível criptografar o lote de aplicativo ao agrupá-lo. Para criptografar o lote de aplicativo, use a opção /ep e especifique se está usando um arquivo de chave (/kf) ou a tecla de teste global (/kt). Para obter mais informações sobre como criar um lote criptografado, consulte [Criptografar ou descriptografar um pacote ou um lote](#encrypt-or-decrypt-a-package-or-bundle).

Opções específicas ao comando **bundle**:

| **Opção**    | **Descrição**                       |
|---------------|---------------------------------------|
| /bv           | Especifica o número de versão do lote. O número de versão deve estar em quatro partes separadas por pontos no formato: &lt;Principal&gt;.&lt;Secundário&gt;.&lt;Compilação&gt;.&lt;Revisão&gt;. |
| /f            | Especifica o arquivo de mapeamento.           |

Observe que, se a versão do lote não for especificada ou se estiver definida como "0.0.0.0", o lote será criado com a data e a hora atuais.

Os exemplos de uso a seguir mostram algumas opções de sintaxe possíveis para o comando **bundle**:

``` syntax
MakeAppx bundle [options] /d <content directory> /p <output bundle name>
MakeAppx bundle [options] /f <mapping file> /p <output bundle name>
MakeAppx bundle [options] /d <content directory> /ep <encrypted output bundle name> /kf MyKeyFile.txt
MakeAppx bundle [options] /f <mapping file> /ep <encrypted output bundle name> /kt
```
O seguinte bloco contém exemplos do comando **bundle**:

``` examples
MakeAppx bundle /v /d "C:\My Files" /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /p MyBundle.msixbundle
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kf MyKeyFile.txt
MakeAppx bundle /v /o /bv 1.0.1.2096 /f MyMapping.txt /ep MyBundle.emsixbundle /kt
```

### <a name="extract-files-from-a-package-or-bundle"></a>Extrair arquivos de um pacote ou um lote

Além de empacotar e agrupar apps, a **MakeAppx.exe** também pode desempacotar ou desagrupar pacotes existentes. Você deve fornecer o diretório de conteúdo como um destino para os arquivos extraídos. Se você estiver tentando extrair arquivos de um pacote ou um lote criptografado, poderá descriptografar e extrair os arquivos ao mesmo tempo usando a opção /ep e especificando se ele deve ser descriptografado com um arquivo de chave (/kf) ou a chave de teste global (/kt). Para obter mais informações sobre como descriptografar um pacote ou um lote, consulte [Criptografar ou descriptografar um pacote ou um lote](#encrypt-or-decrypt-a-package-or-bundle).

Opções específicas aos comandos **unpack** e **unbundle**:

| **Opção**    | **Descrição**                       |
|---------------|---------------------------------------|
| /nd           | Não realiza a descriptografia ao descompactar ou desagrupar o pacote/lote. |
| /pfn          | Desempacota/desagrupa todos os arquivos em um subdiretório no caminho de saída especificado, nomeados após o pacote ou o nome completo de pacotes |

Os exemplos de uso a seguir mostram algumas opções de sintaxe possíveis para os comandos **unpack** e **unbundle**:

``` syntax
MakeAppx unpack [options] /p <input package name> /d <output directory>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kf <key file>
MakeAppx unpack [options] /ep <encrypted input package name> /d <output directory> /kt

MakeAppx unbundle [options] /p <input bundle name> /d <output directory>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kf <key file>
MakeAppx unbundle [options] /ep <encrypted input bundle name> /d <output directory> /kt
```

O seguinte bloco contém exemplos de uso dos comandos **unpack** e **unbundle**:

``` examples
MakeAppx unpack /v /p MyPackage.msix /d "C:\My Files"
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unpack /v /ep MyPackage.emsix /d "C:\My Files" /kt

MakeAppx unbundle /v /p MyBundle.msixbundle /d "C:\My Files"
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kf MyKeyFile.txt
MakeAppx unbundle /v /ep MyBundle.emsixbundle /d "C:\My Files" /kt
```

### <a name="encrypt-or-decrypt-a-package-or-bundle"></a>Criptografar ou descriptografar um pacote ou um lote

A ferramenta **MakeAppx.exe** também pode criptografar ou descriptografar um pacote ou um lote existente. Você deve simplesmente informar o nome do pacote, o nome do pacote de saída e se a criptografia ou descriptografia deve usar um arquivo de chave (/kf) ou a chave de teste global (/kt).

A criptografia e a descriptografia não estão disponíveis com o Assistente de empacotamento do Visual Studio.

Opções específicas aos comandos **encrypt** e **decrypt**:

| **Opção**    | **Descrição**                       |
|---------------|---------------------------------------|
| /ep           | Especifica um pacote ou lote de aplicativo criptografado. |

Os exemplos de uso a seguir mostram algumas opções de sintaxe possíveis para os comandos **encrypt** e **decrypt**:

``` syntax
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kf <key file>
MakeAppx encrypt [options] /p <package name> /ep <output package name> /kt

MakeAppx decrypt [options] /ep <package name> /p <output package name> /kf <key file>
MakeAppx decrypt [options] /ep <package name> /p <output package name> /kt
```

O seguinte bloco contém exemplos de uso dos comandos **encrypt** e **decrypt**:

``` examples
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe encrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt

MakeAppx.exe decrypt /p MyPackage.msix /ep MyEncryptedPackage.emsix /kt
MakeAppx.exe decrypt p MyPackage.msix /ep MyEncryptedPackage.emsix /kf MyKeyFile.txt
```

## <a name="key-files"></a>Arquivos de chave

Os arquivos de chave devem iniciar com uma linha contendo a cadeia de caracteres "[Keys]" seguida por linhas que descrevem as chaves com as quais criptografar cada pacote. Cada chave é representada por um par de cadeias de caracteres entre aspas, separadas por espaços ou tabulações. A primeira cadeia de caracteres representa a ID da chave de 32 bytes codificada em base64, e a segunda representa a chave de criptografia de 32 bytes codificada em base64. Um arquivo de chave deve ser um arquivo de texto simples.

Exemplo de um arquivo de chave:

``` Example
[Keys]
"OWVwSzliRGY1VWt1ODk4N1Q4R2Vqc04zMzIzNnlUREU="    "MjNFTlFhZGRGZEY2YnVxMTBocjd6THdOdk9pZkpvelc="
```

## <a name="mapping-files"></a>Arquivos de mapeamento
Os arquivos de mapeamento devem iniciar com uma linha contendo a cadeia de caracteres "[Files]" seguida por linhas que descrevem os arquivos a serem adicionados ao pacote. Cada arquivo é descrito por um par de caminhos entre aspas, separados por espaços ou tabulações. Cada arquivo representa sua origem (no disco) e seu destino (no pacote). Um arquivo de mapeamento deve ser um arquivo de texto simples.

Exemplo de um arquivo de mapeamento (sem a opção /m):

``` Example
[Files]
"C:\MyApp\StartPage.html"               "default.html"
"C:\Program Files (x86)\example.txt"    "misc\example.txt"
"\\MyServer\path\icon.png"              "icon.png"
"my app files\readme.txt"               "my app files\readme.txt"
"CustomManifest.xml"                    "AppxManifest.xml"
```

Ao usar um arquivo de mapeamento, você poderá escolher se deseja usar a opção /m. A opção /m permite que o usuário especifique os metadados do recurso no arquivo de mapeamento a serem incluídos no manifesto gerado. Se você usar a opção /m, o arquivo de mapeamento deverá conter uma seção que inicia com a linha "[ResourceMetadata]", seguida por linhas que especificam "ResourceDimensions" e "ResourceId". É possível que um pacote de aplicativo contenha várias "ResourceDimensions", mas só pode haver uma "ResourceId".

Exemplo de um arquivo de mapeamento (com a opção /m):

``` Example
[ResourceMetadata]
"ResourceDimensions"                    "language-en-us"
"ResourceId"                            "English"

[Files]
"images\en-us\logo.png"                 "en-us\logo.png"
"en-us.pri"                             "resources.pri"
```

## <a name="semantic-validation-performed-by-makeappxexe"></a>Validação semântica realizada por MakeAppx.exe

A **MakeAppx.exe** realiza a validação semântica limitada, que é projetada para capturar os erros mais comuns de implantação e ajudar a garantir que o pacote de aplicativo seja válido. Consulte a opção /nv se desejar ignorar a validação ao usar a **MakeAppx.exe**.

Essa validação garante que:
- Todos os arquivos referenciados no manifesto do pacote sejam incluídos no pacote de aplicativo.
- Um app não tenha duas chaves idênticas.
- Um app não seja registrado para um protocolo proibido desta lista: SMB, FILE, MS-WWA-WEB, MS-WWA.

Essa não é uma validação semântica completa, já que é projetada apenas para capturar erros comuns. Os pacotes compilados pela **MakeAppx.exe** não são garantidos como instaláveis.