---
description: Este artigo descreve os problemas conhecidos que podem ocorrer quando você cria um pacote MSIX para seu aplicativo da área de trabalho.
title: Problemas conhecidos com aplicativos empacotados da área de trabalho
ms.date: 07/29/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: 0ec44b40dc377bbc573c40a6ec12540f7cd89dac
ms.sourcegitcommit: e3a06eccd3322053b8b498cb6343fb6f711a7a0b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84724531"
---
# <a name="known-issues-with-packaged-desktop-apps"></a>Problemas conhecidos com aplicativos empacotados da área de trabalho

Este artigo contém os problemas conhecidos que podem ocorrer quando você cria um pacote MSIX para seu aplicativo da área de trabalho.

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>Você recebe o erro MSB4018 Falha inesperada na tarefa "GenerateResource"

Isso pode acontecer quando você tenta converter assemblies satélite em arquivos de PRI (Índice de Recurso do Pacote).

Estamos cientes desse problema e trabalhando em uma solução de mais longo prazo. Como uma solução temporária, você pode desabilitar o gerador de recursos adicionando esta linha de XML ao primeiro elemento PropertyGroup no arquivo de projeto de hospedagem:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernel_security_check_failure"></a>Tela azul com código de erro 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Depois da instalação ou da inicialização de certos aplicativos da Microsoft Store, talvez o computador seja reiniciado inesperadamente com o erro: **0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)** .

Entre os aplicativos afetados conhecidos estão Kodi, JT2Go, Ear Trumpet, Teslagrad e outros.

Uma [atualização do Windows (versão 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) foi lançada em 27/10/16 incluindo correções importantes que resolvem esse problema. Se você enfrentar esse problema, atualize o computador. Se não conseguir atualizar o computador porque ele reinicia antes de você conseguir fazer logon, você deverá usar a restauração do sistema para restaurar o sistema até um ponto anterior ao momento em que instalou um dos aplicativos afetados. Para obter informações sobre como usar a restauração do sistema, consulte [Opções de recuperação no Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

Se a atualização não corrigir o problema ou se você não souber como recuperar o computador, entre em contato com o [Suporte da Microsoft](https://support.microsoft.com/contactus/).

Caso você seja desenvolvedor, talvez deseje impedir a instalação de seu aplicativo empacotado em versões do Windows que não incluam essa atualização. Observe que, ao você fazer isso, o aplicativo não estará disponível para os usuários que ainda não tiverem instalado a atualização. Para limitar a disponibilidade do aplicativo para os usuários que tenham instalado essa atualização, modifique o arquivo AppxManifest.xml da seguinte maneira:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Os detalhes a respeito do Windows Update podem ser encontrados em:
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>Erros comuns que podem aparecer quando você assina seu aplicativo

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>A incompatibilidade de publicadores e de certificados causa o erro SignTool "Erro: Falha no SignerSign()" (-2147024885/0x8007000b)

A entrada do publicador no manifesto do pacote do aplicativo do Windows deve corresponder à entidade do certificado com o qual você está assinando.  Você pode usar qualquer um dos seguintes métodos para ver o assunto do certificado.

**Opção 1: PowerShell**

Execute o seguinte comando do PowerShell: Ambos .cer ou .pfx pode ser usado como o arquivo de certificado, pois eles têm as mesmas informações de fornecedor.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Opção 2: Explorador de Arquivos**

Clique duas vezes no certificado no Explorador de Arquivos, selecione a guia *Detalhes* e depois o campo *Assunto* na lista. Em seguida, você pode copiar o conteúdo.

**Opção 3: CertUtil**

Por meio da linha de comando, execute o **certutil** no arquivo PFX e copie o campo *Entidade* da saída.

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert"></a>

### <a name="bad-pe-certificate-0x800700c1"></a>Certificado PE defeituoso (0x800700C1)

Isso pode acontecer quando seu pacote contém um binário que corrompeu o certificado. Aqui estão algumas das razões por que isso pode acontecer:

* O início do certificado não está no final de uma imagem.  

* O tamanho do certificado não é positivo.

* O início do certificado não está depois da estrutura `IMAGE_NT_HEADERS32` de um executável de 32 bits nem depois da estrutura `IMAGE_NT_HEADERS64` de um executável de 64 bits.

* O ponteiro do certificado não está adequadamente alinhado com uma estrutura WIN_CERTIFICATE.

Para localizar arquivos que contenham um certificado PE defeituoso, abra um **prompt de comando** e defina a variável de ambiente nomeada `APPXSIP_LOG` a um valor de 1.

```
set APPXSIP_LOG=1
```

Em seguida, no **prompt de comando**, assine o aplicativo novamente. Por exemplo:

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

As informações sobre os arquivos que contêm um certificado PE defeituoso aparecerão na **janela do console**. Por exemplo:

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```

## <a name="next-steps"></a>Próximas etapas

Tem dúvidas? Pergunte-nos no Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
