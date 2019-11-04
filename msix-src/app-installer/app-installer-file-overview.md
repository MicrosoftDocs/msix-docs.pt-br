---
title: Visão geral do arquivo do Instalador de Aplicativo
description: Este artigo descreve o conteúdo dos arquivos do Instalador de Aplicativo e como eles funcionam para ajudar a gerenciar a distribuição e a instalação de seus aplicativos da área de trabalho.
ms.date: 12/12/2018
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 070d948417c01fbf4f278bf37959af58b368fa99
ms.sourcegitcommit: e9a890c674dd21c9a09048e2520a3de632753d27
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73328312"
---
# <a name="app-installer-file-overview"></a>Visão geral do arquivo do Instalador de Aplicativo

Muitas vezes, você precisará compartilhar seu aplicativo com muitos usuários. Posteriormente, você precisará atualizar o aplicativo e desejar ter certeza de que pode fazer isso de uma forma que seja simples, até mesmo para os usuários não técnicos, e fácil para você.

Para ajudar você a conseguir fazer isso, introduzimos o arquivo do Instalador de Aplicativo. Esse é um arquivo XML que você pode criar por conta própria ou usando o Visual Studio (confira as instruções do Visual Studio [aqui](create-appinstallerfile-vs.md)). O arquivo do Instalador de Aplicativo especifica o local em que o aplicativo se encontra e como atualizá-lo. Se optar por usar esse método de distribuição do aplicativo, você precisará compartilhar com seus usuários o arquivo do Instalador de Aplicativo, em vez do contêiner de aplicativo real. Em seguida, o usuário precisará clicar no arquivo do Instalador de Aplicativo. Neste ponto, a interface do usuário conhecida do Instalador de Aplicativo será exibida e orientará o usuário durante a instalação.  Quando o usuário tiver instalado o aplicativo usando essas etapas, o aplicativo estará associado ao arquivo do Instalador de Aplicativo.  

Posteriormente, quando você tiver uma atualização para o aplicativo, bastará atualizar o arquivo do Instalador de Aplicativo (.appinstaller). Quando você atualizar o arquivo, a nova versão do aplicativo será enviada por push ao usuário. Isso é especialmente bom para os usuários, porque eles não precisam fazer nada para obter a atualização. Eles apenas continuarão usando o aplicativo como de costume e a atualização será entregue a eles.

Este é um exemplo que mostra como isso funciona:

1. O Profissional de TI Pedro deseja distribuir o aplicativo de Recursos Humanos para sua empresa.
2. O Profissional de TI Pedro coloca o aplicativo de Recursos Humanos em um compartilhamento e cria um arquivo do Instalador de Aplicativo chamado HumanResources.appinstaller. Esse arquivo do Instalador de Aplicativo é associado ao aplicativo.
3. O Profissional de TI Pedro coloca HumanResources.appinstaller em um compartilhamento.
4. O Profissional de TI Pedro aponta os funcionários da empresa para HumanResources.appinstaller.
5. A Gerente Mila clica em HumanResources.appinstaller e obtém a interface do usuário do Instalador de Aplicativo, que a orienta para a instalação do aplicativo de Recursos Humanos.
6. Daí em diante, no dispositivo da gerente Mila, Recursos Humanos é apenas outro aplicativo e ela interage com ele como faz com qualquer outro aplicativo. Ela pode fixá-lo na barra de tarefas ou no menu Iniciar, ele é exibido na lista de aplicativos etc.
7. Uma semana depois, o profissional de TI Pedro obtém uma atualização para o aplicativo de Recursos Humanos. Para compartilhá-lo com os usuários, ele apenas atualiza HumanResources.appinstaller para que ele aponte para a nova versão do aplicativo e define o tipo de atualização desejado.
8. Na manhã seguinte, a Gerente Mila, que não sabe nada sobre a atualização, inicia o aplicativo de Recursos Humanos que já está em sua área de trabalho.
9. O aplicativo detecta que há uma atualização e a aplica automaticamente
10. A Gerente Mila está satisfeita, pois agora tem a última versão do aplicativo e pode aproveitar os novos recursos.

No Windows 10 Fall Creators Update (versão 1709, build 16299) e em versões posteriores, o SDK do Windows também oferece várias APIs que você pode usar para modificar pacotes de forma programática por meio de arquivos do Instalador de Aplicativo ou recuperar informações sobre aplicativos com uma associação do Instalador de Aplicativo. Para obter mais informações, confira [Documentação relacionada](app-installer-documentation.md).

## <a name="contents-of-the-app-installer-file"></a>Conteúdo do arquivo do Instalador de Aplicativo

A imagem a seguir mostra um arquivo de exemplo do Instalador de Aplicativo. Para obter detalhes completos sobre os elementos XML no arquivo do Instalador de Aplicativo, confira a [referência de esquema de arquivo do Instalador de Aplicativo](https://docs.microsoft.com/uwp/schemas/appinstallerschema/schema-root). Para obter mais informações sobre como definir as configurações de atualização no arquivo do Instalador de Aplicativo, confira [Definir as configurações de atualização no arquivo do Instalador de Aplicativo](update-settings.md).

![Exemplo de arquivo do Instalador de Aplicativo com as configurações de atualização](images/App-Installer-File-Update.png)

## <a name="related-topics"></a>Tópicos relacionados

* [Criar um arquivo do Instalador de Aplicativo com o Visual Studio](create-appinstallerfile-vs.md)
* [Criar um arquivo do Instalador de Aplicativo manualmente](how-to-create-appinstaller-file.md)
* [Definir as configurações de atualização no arquivo do Instalador de Aplicativo](update-settings.md)
