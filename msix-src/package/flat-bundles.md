---
title: Pacotes do lote simples de aplicativo
description: Descreve como criar um lote simples de pacote para empacotar seus arquivos de pacote .appx com referências aos pacotes de aplicativos.
ms.date: 07/02/2019
author: dianmsft
ms.author: diahar
ms.topic: article
keywords: Windows 10, msix, empacotamento, configuração de pacote, pacote simples
ms.localizationpriority: medium
ms.openlocfilehash: a1e06484476c82a856da6ad4713ef6bdcac0e051
ms.sourcegitcommit: 8a75eca405536c5f9f7c4fd35dd34c229be7fa3e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68690015"
---
# <a name="flat-bundle-app-packages"></a>Pacotes do lote simples de aplicativo 

> [!IMPORTANT]
> Se você pretender enviar seu aplicativo para a Store, precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) para obter aprovação para usar lotes simples.

Os pacotes simples são uma maneira aprimorada de agrupar os arquivos de pacote do aplicativo. Um arquivo de pacote de aplicativo do Windows típico usa uma estrutura de empacotamento de vários níveis na qual os arquivos de pacote do aplicativo precisam estar contidos no pacote, os pacotes simples removem essa necessidade apenas referenciando os arquivos de pacote do aplicativo, permitindo que eles estejam fora do pacote de aplicativos. Como os arquivos de pacote do aplicativo não estão mais contidos no pacote, eles podem ser processados em paralelo, o que resulta em tempo de carregamento reduzido, publicação mais rápida (já que cada arquivo de pacote de aplicativo pode ser processado ao mesmo tempo) e desenvolvimento mais rápido iterações.

![Diagrama de lote simples](images/bundle-combined.png)

Outra vantagem de lotes simples é a necessidade de criar menos pacotes. Como os arquivos de pacote do aplicativo são referenciados apenas, duas versões do aplicativo podem referenciar o mesmo arquivo de pacote se o pacote não fosse alterado nas duas versões. Assim, você precisa criar apenas os pacotes de aplicativo que foram alterados ao compilar os pacotes para a próxima versão do seu aplicativo.
Por padrão, os pacotes simples referenciarão os arquivos de pacote do aplicativo dentro da mesma pasta. No entanto, essa referência pode ser alterada para outros caminhos (caminhos relativos, compartilhamentos de rede e locais http). Para fazer isso, você deve fornecer diretamente um **BundleManifest** durante a criação de lote simples. 

## <a name="how-to-create-a-flat-bundle"></a>Como criar um lote simples

Um lote simples pode ser criado usando a ferramenta MakeAppx.exe ou usando o layout de empacotamento para definir a estrutura do seu lote.

### <a name="using-makeappxexe"></a>Usando a MakeAppx.exe

Para criar um pacote simples usando MakeAppx. exe, use o comando "pacote de MakeAppx. exe" como de costume, mas com a opção/FB para gerar o arquivo de pacote de aplicativo simples (que será extremamente pequeno, já que ele faz referência apenas aos arquivos de pacote do aplicativo e não contém nenhuma carga real ). 

Eis um exemplo da sintaxe do comando:

```syntax
MakeAppx bundle [options] /d <content directory> /fb /p <output flat bundle name>
```

Para obter mais informações sobre como usar MakeAppx.exe, consulte [Criar um pacote de aplicativos com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md).

### <a name="using-packaging-layout"></a>Usando o layout de empacotamento

Como alternativa, você pode criar um lote simples usando o layout de empacotamento. Para fazer isso, defina o atributo **FlatBundle** como **true** no elemento **PackageFamily** do manifesto do lote de aplicativo. Para saber mais sobre o layout de empacotamento, consulte [Criação do pacote com o layout de empacotamento](packaging-layout.md).

## <a name="how-to-deploy-a-flat-bundle"></a>Como implantar um lote simples 

Antes que um pacote simples possa ser implantado, cada um dos pacotes de aplicativos (além do pacote de aplicativos) deve ser assinado com o mesmo certificado. Isso ocorre porque todos os arquivos de pacote do aplicativo (. Appx/. msix) agora são arquivos independentes e não estão mais contidos no arquivo de pacote do aplicativo (. appxbundle/. msixbundle).

Depois que os pacotes são assinados, você pode instalar o aplicativo por meio de uma destas opções:
* Clique duas vezes no arquivo de pacote de aplicativo para instalar com o instalador do aplicativo.
* Use o [cmdlet Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) no PowerShell e aponte para o arquivo de pacote de aplicativo (supondo que os pacotes de aplicativo sejam onde o pacote de aplicativo espera que sejam). 

Você não pode implantar os pacotes. Appx/. msix individuais de um pacote simples por si só. Eles devem ser implantados por meio de. appxbundle/. msixbundle. No entanto, você pode atualizar pacotes. Appx/. msix individuais de um pacote simples após a instalação inicial. 

Por exemplo, se seu pacote simples v1 é composto de um. msixbundle, um x86. msix, um x64. msix e um Asset. msix, e você sabe que o seu pacote V2 tem apenas alterações no pacote de ativos, você só precisa criar o. msixbundle e o Asset. msix para poder instalar o th atualização e. Você deve compilar o. msixbundle para v2 porque o pacote mantém o controle de todas as versões de seus pacotes. msix. Ao aumentar a versão do Asset. msix para a v2, você precisa de um New. msixbundle que tenha essa nova referência. O v2. msixbundle pode conter referências para o v1 x86. msix e x64. msix; os pacotes. msix de um pacote simples não precisam ter o mesmo número de versão.  
