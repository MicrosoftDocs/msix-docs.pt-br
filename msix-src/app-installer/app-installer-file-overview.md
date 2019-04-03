---
title: Visão geral de arquivo do instalador de aplicativo
description: Saiba mais sobre o conteúdo de arquivos do instalador do aplicativo e como eles funcionam.
author: mcleanbyron
ms.author: mcleans
ms.date: 12/12/2018
ms.topic: article
keywords: Windows 10, uwp, msix
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 67fbda421f84227ed2618af5711617e406b26a7e
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900278"
---
# <a name="app-installer-file-overview"></a>Visão geral de arquivo do instalador de aplicativo

Muitas vezes, você precisa compartilhar seu aplicativo com muitos usuários. Posteriormente, você precisa atualizar o aplicativo e você deseja certificar-se de que você pode fazer isso de forma simples, mesmo para os usuários não técnicos, e fácil para você.

Para ajudar você a conseguir isso, apresentamos criado o arquivo do instalador do aplicativo. Esse é um arquivo XML que você pode criar por conta própria ou criar usando o Visual Studio (consulte as instruções do Visual Studio [aqui](create-appinstallerfile-vs.md)). O arquivo do instalador de aplicativo especifica onde o seu aplicativo está localizado e como atualizá-lo. Se você optar por usar esse método de distribuição de aplicativos, você deve compartilhar com seus usuários do arquivo instalador do aplicativo, em vez de contêiner do aplicativo real. O usuário deve, em seguida, clique no arquivo instalador do aplicativo. Neste ponto, a interface do usuário familiar do aplicativo instalador aparecerá e orientar o usuário durante a instalação.  Quando o usuário tiver instalado o aplicativo usando essas etapas, o aplicativo está associado com o arquivo do instalador do aplicativo.  

Posteriormente, quando você tem uma atualização para o aplicativo, você apenas atualiza o arquivo do instalador do aplicativo (appinstaller). Quando você atualiza o arquivo, a nova versão do aplicativo é enviada para o usuário. Isso é especialmente bom para os usuários porque eles não precisam fazer nada para obter a atualização. Eles simplesmente continuar usando o aplicativo como de costume, e a atualização será entregue a eles.

Aqui está um exemplo que mostra como isso funciona:

1. IT Pro Jorge deseja distribuir o aplicativo de recursos humanos para sua empresa.
2. Joe IT Pro coloca o aplicativo de recursos humanos em um compartilhamento e cria um arquivo do instalador de aplicativo denominado HumanResources.appinstaller. Esse arquivo de instalador de aplicativo é associado ao aplicativo.
3. Joe IT Pro coloca HumanResources.appinstaller em um compartilhamento.
4. Joe IT Pro aponta HumanResources.appinstaller os funcionários da empresa.
5. Gerenciador de Maggie clica no HumanResources.appinstaller e obtém a interface do instalador do aplicativo, que a orienta para instalar o aplicativo de recursos humanos.
6. A partir daí, no Gerenciador de dispositivo de Maggie recursos humanos é apenas outro aplicativo e ela interage com ele, pois ela faz isso com qualquer outro aplicativo. Ela pode fixá-la na barra de tarefas ou o menu Iniciar, ele aparece na sua lista de aplicativos etc.
7. Uma semana depois Joe IT pro obtém uma atualização para o aplicativo de recursos humanos. Para compartilhá-lo com os usuários, ele apenas atualiza HumanResources.appinstaller para apontar para a nova versão do aplicativo e define o tipo de atualização que ele deseja.
8. Na manhã seguinte, Manager Maggie, que não sabe nada sobre a atualização inicia o aplicativo de recursos humanos que já está na sua área de trabalho.
9. O aplicativo detecta que há uma atualização e aplica a atualização automaticamente
10. Manager Maggie está feliz que ela agora tem a versão mais recente do aplicativo e pode tirar proveito dos novos recursos.

A partir do Windows 10 Fall Creators Update (versão 1709, build 16299) e versões posteriores, o SDK do Windows também oferece várias APIs que você pode usar para modificar programaticamente pacotes por meio de arquivos do instalador do aplicativo ou recuperar informações sobre os aplicativos com um aplicativo Associação de instalador. Para obter mais informações, consulte [documentação relacionada](app-installer-documentation.md).

## <a name="contents-of-the-app-installer-file"></a>Conteúdo do arquivo instalador do aplicativo

A imagem a seguir mostra um exemplo de arquivo do instalador do aplicativo. Para obter detalhes completos sobre os elementos XML no arquivo de instalador de aplicativo, consulte o [referência de esquema de arquivo do instalador de aplicativo](https://docs.microsoft.com/uwp/schemas/appinstallerschema/schema-root). Para obter mais informações sobre como definir configurações de atualização no arquivo de instalador de aplicativo, consulte [definir configurações de atualização no arquivo de instalador de aplicativo](update-settings.md).

![Exemplo de arquivo do instalador de aplicativo com as configurações de atualização](images/App-Installer-File-Update.png)

## <a name="related-topics"></a>Tópicos relacionados

* [Criar um arquivo do Instalador de Aplicativo com o Visual Studio](create-appinstallerfile-vs.md)
* [Criar um arquivo do instalador do aplicativo manualmente](how-to-create-appinstaller-file.md)
* [Definir configurações de atualização no arquivo de instalador de aplicativo](update-settings.md)
