---
title: Como pré-instalar aplicativos empacotados
description: Este artigo apresenta uma visão geral dos aplicativos pré-instalados
ms.date: 04/02/2020
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: windows 10, msix, uwp, pacotes opcionais, conjunto relacionado, extensão de pacote, visual studio, dism, pré-instalar, pré-instalação, aplicativos empacotados
ms.localizationpriority: medium
ms.openlocfilehash: be5c13d0374ed058934c920053f52a1fdc39df43
ms.sourcegitcommit: ccfd90b4a62144f45e002b3ce6a2618b07510c71
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2020
ms.locfileid: "81614001"
---
# <a name="preinstalling-packaged-apps"></a>Como pré-instalar aplicativos empacotados
Há várias ferramentas que podem ser usadas para instalar um aplicativo empacotado MSIX em um dispositivo para todos os usuários:

- DISM (Gerenciamento e Manutenção de Imagens de Implantação)
- Pacotes de provisionamento
- PowerShell

Este artigo apresentará uma visão geral de como os arquivos pré-instalados funcionam e como as licenças e o provisionamento funcionam com os aplicativos pré-instalados. 

## <a name="overview"></a>Visão geral
A pré-instalação de instalações de aplicativos empacotados pode ser dividida em duas etapas: 
1. Preparando
1. Registro

### <a name="staging"></a>Preparando
O preparo de um aplicativo empacotado em um dispositivo é o ato de armazenar uma cópia do aplicativo empacotado no sistema de arquivos local. Um aplicativo empacotado precisa ser preparado apenas uma vez e pode ser executado sem nenhuma conta de usuário existente no dispositivo.

O preparo de um aplicativo empacotado pode ser feito em uma imagem offline (.wim, .vhd ou .vhdx) ou em um sistema operacional ativo online. 

### <a name="registration"></a>Registro
Depois que um aplicativo empacotado é preparado, o aplicativo pode ser registrado para os usuários no dispositivo. O registro ocorre por usuário e começa quando um usuário do dispositivo faz logon. O sistema operacional carregará o pacote do aplicativo empacotado pré-instalado criando dados de aplicativo específicos do usuário, criará associações de tipo de arquivo e blocos de aplicativo no menu Iniciar. Isso é feito pelo ARS (Serviço de Preparação do Aplicativo), que reconhece todos os aplicativos pré-instalados. 

## <a name="dism"></a>DISM
O DISM é uma ferramenta de linha de comando que pode ser usada para fazer a manutenção de imagens do Windows e prepará-las, incluindo aquelas usadas para o WinPE (Ambiente de Pré-Instalação do Windows), o WinRE (Ambiente de Recuperação do Windows) e a Instalação do Windows. O DISM pode ser usado para fazer a manutenção de uma imagem do Windows (.wim) ou de discos rígidos virtuais (.vhd ou .vhdx).

## <a name="provisioning-packages"></a>Pacotes de provisionamento
Todo o provisionamento de aplicativos é encapsulado na ferramenta DISM e realiza tanto a preparação quanto a configuração do ARS. Para fazer o provisionamento, o profissional de TI precisa de um pacote do aplicativo (.msix, .msixbundle, .appx ou .appxbundle) e quaisquer pacotes de dependência. 

No Windows 10, versão 1809 em diante, os profissionais de TI podem fazer a pré-instalação por meio do provisionamento. Os aplicativos provisionados serão instalados em uma localização central %ProgramFiles%\WindowsApps e ficarão imediatamente disponíveis para os usuários registrados. Somente os usuários com o pacote do aplicativo MSIX registrado na conta terão acesso ao aplicativo.

No Windows 10, versão 2004, um aplicativo empacotado provisionado será reinstalado durante o novo provisionamento. As versões anteriores do Windows 10 impedirão a reinstalação desses aplicativos empacotados se o usuário tiver desinstalado anteriormente o aplicativo empacotado.

## <a name="powershell"></a>PowerShell
Lista de comandos relevantes do PowerShell
* **[Get-ProvisionedAppxPackages](https://docs.microsoft.com/powershell/module/dism/get-appxprovisionedpackage?view=win10-ps)** Isso listará todos os aplicativos pré-instalados na imagem.
* **[Add-ProvisionedAppxPackage](https://docs.microsoft.com/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps)** Isso prepara o pacote appx e o configura para pré-instalação. Todas as dependências também precisam ser fornecidas, as quais podem ser encontradas no SDK ou nos pacotes baixados da Microsoft Store.
* **[Remove-ProvisionedAppxPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps)** Isso pode ser usado para remover um aplicativo pré-instalado. Observe que isso não remove o aplicativo caso ele já esteja registrado para algum usuário; apenas remove o comportamento de registro automático para que ele não seja atualizado automaticamente para os novos usuários.  Caso nenhum usuário tenha instalado o aplicativo, esse comando também removerá os arquivos preparados.

## <a name="licensing"></a>Licenciamento
O licenciamento se aplica somente ao provisionamento de um aplicativo da Windows Store. Qualquer outro aplicativo pode ser provisionado sem uma licença. Se um aplicativo for da Store, uma licença de computador também deverá ser fornecida quando o aplicativo for provisionado. Nesse momento, todos os aplicativos pré-instalados da Windows Store precisam ser gratuitos e configurados como pré-instaláveis pelo Partner Center da Windows Store. Depois de configurados, o pacote pré-instalável e a licença podem ser baixados e provisionados em qualquer imagem.

