---
title: Como a Política de Grupo funciona com aplicativos empacotados como MSIX
description: Descreve como a Política de Grupo funciona com aplicativos convertidos em MSIX.
ms.date: 04/12/2019
ms.topic: article
keywords: MSIX
ms.localizationpriority: medium
ms.openlocfilehash: d7c5bec4b088bd5bf9f003b3936d14529f53197c
ms.sourcegitcommit: 6243b7aca6f52f007f4571c835f580f433c31769
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84812739"
---
# <a name="group-policy-and-msix-packaged-apps"></a>Política de Grupo e aplicativos empacotados como MSIX

Os desenvolvedores que usam o MSIX podem aproveitar a Política de Grupo de modo semelhante a como se beneficiam de outros tipos de instaladores.

Se você empacotar o aplicativo Win32 em um MSIX (ou se criou o aplicativo usando a Ponte de Desktop), o aplicativo terá a funcionalidade de confiança total habilitada. Isso permitirá que você faça a leitura das chaves do Registro da Política de Grupo. Durante o tempo de execução, o aplicativo terá a mesma exibição do registro da Política de Grupo que teria se tivesse sido instalado usando um método diferente. Do Windows 10 versão 1809 em diante, se o aplicativo for um aplicativo da UWP (Plataforma Universal do Windows), ele poderá acessar as mesmas chaves da Política de Grupo. Para obter mais informações sobre como criar a Política de Grupo, consulte [este artigo](https://docs.microsoft.com/openspecs/windows_protocols/ms-gpreg/834da877-264f-4589-9b80-b6b012c8edc3).

Se você estiver convertendo um instalador existente em MSIX usando a [Ferramenta de Empacotamento MSIX](mpt-overview.md), não precisará fazer nenhuma alteração para que o aplicativo dê suporte à Política de Grupo. Continue gerenciando as Políticas de Grupo como normalmente faria para o instalador original. Os aplicativos convertidos para MSIX ainda poderão fazer a leitura por meio das chaves do Registro da Política de Grupo existente. 

A Política de Grupo não tem suporte nativo para a instalação de aplicativos MSIX. 

## <a name="policies-for-blocking-microsoft-store-and-msix"></a>Políticas para bloqueio da Microsoft Store e do MSIX 

Você pode ter seus próprios requisitos sobre como deseja configurar as atualizações do aplicativo Microsoft Store. O aplicativo Store dispara as atualizações para aplicativos de terceiros e também para aplicativos próprios, como a Calculadora e o aplicativo de Fotos. Se o aplicativo Store for removido de um computador, isso poderá resultar na não atualização dos aplicativos desse computador.

Aqui está a lista de políticas do Store e como ele afeta os pacotes MSIX.

### <a name="turn-off-automatic-download-and-install-of-updates"></a>Desabilitar Download e Instalação Automáticos de Atualizações

Essa política habilita ou desabilita o download automático e a instalação de atualizações de aplicativos. Se você habilitar esta configuração, o download e a instalação automáticos das atualizações do aplicativo serão desativados. Se você desabilitar esta configuração, o download e a instalação automática de atualizações de aplicativos serão ligados. Se você não definir essa configuração, o download e a instalação automática de atualizações de aplicativos serão determinados por uma configuração do registro que o usuário poderá alterar usando as **Configurações** do Store.

* **GPO:** `Computer Configuration\Administrative Templates\Windows Components\Store`
* **Registro:** `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\WindowsStore AutoDownload REG_DWORD` (Data: enable = 2 = os aplicativos não serão atualizados, disable = 4 = o aplicativo será atualizado automaticamente)
* **Atualizações de aplicativos:** Se habilitado, o download e a instalação automática de atualizações de aplicativos serão desligados. Se desabilitado, o download e a instalação automática de atualizações de aplicativos serão ligados. 

### <a name="turn-off-store-application"></a>Desligar o aplicativo Store

Esta política nega ou permite acesso ao aplicativo Store. Se você habilitar essa configuração, o acesso ao aplicativo da Windows Store é negado. O acesso ao Store é necessário para instalar atualizações de aplicativos. Se você desabilitar ou não definir esta configuração, o acesso ao aplicativo Store será permitido.

* **GPO:** `Computer Configuration\Administrative Templates\Windows Components\Store` ou `User Configuration\Administrative Templates\Windows Components\Store`
* **Registro:** `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\WindowsStoreRemoveWindowsStore REG_DWORD` ou `HKEY_CURRENT_USER\Software\Policies\Microsoft\WindowsStoreRemoveWindowsStore REG_DWORD`
* **Atualizações de aplicativos:** Se estiver configurada no contexto do computador, essa política desligará as atualizações de aplicativos.
