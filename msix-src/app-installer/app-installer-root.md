---
author: mcleanbyron
title: Instalar aplicativos do Windows 10 com o instalador de aplicativo
description: Esta seção contém ou é ligada a artigos sobre o Instalador de Aplicativo e como usar seus recursos.
ms.author: mcleans
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, instalador de aplicativo, AppInstaller, sideload, conjunto relacionado, pacotes opcionais
ms.localizationpriority: medium
ms.custom: RS5, seodec18
ms.openlocfilehash: 3578d993f2d88cdcfea4781748015ac9ecba509b
ms.sourcegitcommit: 92e034ce942cf3df1ea243b03e7b38ed78af4d43
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58900438"
---
# <a name="install-windows-10-apps-with-app-installer"></a>Instalar aplicativos do Windows 10 com o instalador de aplicativo

## <a name="purpose"></a>Finalidade
Esta seção contém ou é ligada a artigos sobre o Instalador de Aplicativo e como usar seus recursos.

O instalador de aplicativo permite que aplicativos do Windows 10 ser instalado clicando duas vezes o pacote do aplicativo. Isso significa que os usuários não precisam usar o PowerShell ou outras ferramentas de desenvolvedor para implantar aplicativos do Windows 10. O Instalador de Aplicativo também pode instalar um aplicativo da Web, pacotes opcionais e conjuntos relacionados. Para saber como usar o Instalador de Aplicativo para instalar seu aplicativo, consulte os tópicos na tabela.

| Tópico | Descrição |
|-------|-------------|
| [Visão geral de arquivo do instalador de aplicativo](app-installer-file-overview.md) | Saiba mais sobre o conteúdo de arquivos do instalador do aplicativo e como eles funcionam. |
| [Criar um arquivo do instalador do aplicativo no Visual Studio](create-appinstallerfile-vs.md)| Saiba como usar o Visual Studio para permitir atualizações automáticas usando o arquivo .appinstaller. |
| [Criar um arquivo do instalador do aplicativo manualmente](how-to-create-appinstaller-file.md)| Saiba como criar um arquivo appinstaller manualmente. Isso é particularmente útil para a instalação de um conjunto relacionado que contém um pacote principal e pacotes opcionais. |
| [Definir configurações de atualização no arquivo de instalador de aplicativo](update-settings.md)  |  Saiba como configurar atualizações de aplicativo usando o arquivo do instalador do aplicativo. |
| [Instalar um aplicativo do Windows 10 da web](installing-windows10-apps-web.md) | Nesta seção, analisaremos as etapas que você precisa executar para permitir que os usuários instalem seus aplicativos diretamente a partir da página da Web. |
| [Pacotes opcionais e conjuntos relacionados](install-related-set.md) | Saiba mais sobre conjuntos relacionados que contêm um pacote principal e os pacotes opcionais relacionados.  |
| [Solucionar problemas de instalação com o arquivo do Instalador de Aplicativo](troubleshoot-appinstaller-issues.md) | Problemas e soluções comuns ao fazer sideload de aplicativos com o arquivo do Instalador de Aplicativo. |
| [Documentação relacionada](app-installer-documentation.md) | Fornece links para documentação relacionada, incluindo APIs que você pode usar para modificar pacotes por meio de arquivos do instalador do aplicativo ou para recuperar informações sobre os aplicativos com uma associação de instalador de aplicativo.  |
| [Referência de arquivo do Instalador de Aplicativo (.appinstaller)](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file?context=/windows/msix/render) | Exibir o esquema XML completo para o arquivo do Instalador de Aplicativo. |

## <a name="tutorials"></a>Tutoriais

Siga estes tutoriais e saiba como hospedar e instalar um aplicativo do Windows 10 em várias plataformas de distribuição. Estes tutoriais são úteis para empresas e desenvolvedores que não desejam ou precisam publicar seus aplicativos na Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10.

| Tutorial | Descrição |
|----------|-------------|
| [Instalar um aplicativo do Windows 10 de um aplicativo Web do Azure](web-install-azure.md) | Criar um aplicativo Web do Azure e usá-lo para hospedar e distribuir seu pacote de aplicativos do Windows 10. |
| [Instalar um aplicativo do Windows 10 de um servidor IIS](web-install-IIS.md) | Configure um servidor IIS, verifique se o aplicativo Web pode hospedar pacotes de aplicativos e use o Instalador de aplicativo de maneira eficaz. |
| [Instalam de pacotes de aplicativos no AWS para web hospedando o Windows 10](web-install-aws.md) | Saiba como configurar a Amazon Simple Storage Service para hospedar seu pacote de aplicativos do Windows 10 de um site da web. |
