---
title: Como pré-instalar aplicativos empacotados
description: Este artigo apresenta uma visão geral dos aplicativos pré-instalados
ms.date: 11/30/2020
ms.topic: article
author: dianmsft
ms.author: diahar
keywords: Windows 10, msix, UWP, pacotes opcionais, conjunto relacionado, extensão do pacote, Visual Studio, DISM, pré-instalação, pré-instalação, aplicativos empacotados, nome completo do pacote, pfun
ms.localizationpriority: medium
ms.openlocfilehash: 2a3eacd19444df5df553ea6550d134d636b1c4a1
ms.sourcegitcommit: de0c711ce28851ef6976a71dd1317291f278b4d1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/04/2020
ms.locfileid: "96612084"
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
* **[Get-ProvisionedAppxPackages](/powershell/module/dism/get-appxprovisionedpackage?view=win10-ps)** Isso listará todos os aplicativos pré-instalados na imagem.
* **[Add-ProvisionedAppxPackage](/powershell/module/dism/add-appxprovisionedpackage?view=win10-ps)** Isso prepara o pacote appx e o configura para pré-instalação. Todas as dependências também precisam ser fornecidas, as quais podem ser encontradas no SDK ou nos pacotes baixados da Microsoft Store.
* **[Remove-ProvisionedAppxPackage](/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps)** Isso pode ser usado para remover um aplicativo pré-instalado. Observe que isso não remove o aplicativo caso ele já esteja registrado para algum usuário; apenas remove o comportamento de registro automático para que ele não seja atualizado automaticamente para os novos usuários.  Caso nenhum usuário tenha instalado o aplicativo, esse comando também removerá os arquivos preparados.

Usando os cmdlets do PowerShell do MSIX, para pré-instalar ou provisionar um aplicativo empacotado do MSIX em um dispositivo, você deve usar o nome completo do pacote do aplicativo MSIX. O nome completo do pacote é o nome completo do pacote que contém o nome do pacote, a versão, a arquitetura e as informações do Publicador. Veja a seguir um exemplo de um nome completo do pacote: `Contoso.ContosoApp_44.20231.1000.0_neutral__8wekyb3d8bbwe`

## <a name="licensing"></a>Licenciamento
O licenciamento se aplica somente ao provisionar um aplicativo da Windows Store. Qualquer outro aplicativo pode ser provisionado sem uma licença. Se um aplicativo for da loja a um computador, a licença também deverá ser fornecida quando o aplicativo for provisionado. Neste momento, todos os aplicativos da Windows Store de pré-instalação devem ser aplicativos gratuitos e configurados para serem pré-instalados por meio do Windows Store Partner Center. Depois de configurado, o pacote pré-instalado e a licença podem ser baixados e provisionados em qualquer imagem.
