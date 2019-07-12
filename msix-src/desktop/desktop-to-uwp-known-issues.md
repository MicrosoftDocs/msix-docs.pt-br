---
Description: Este artigo contém problemas conhecidos com a Ponte de Desktop.
title: Problemas conhecidos com aplicativos empacotados de área de trabalho
ms.date: 06/20/2018
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: e5abc7f51cd6b34f11c5cf063ecf2d755c56d61b
ms.sourcegitcommit: 25811dea7b2b4daa267bbb2879ae9ce3c530a44a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67828875"
---
# <a name="known-issues-with-packaged-desktop-apps"></a>Problemas conhecidos com aplicativos empacotados de área de trabalho

Este artigo contém problemas conhecidos que podem ocorrer quando você criar um pacote MSIX para seu aplicativo da área de trabalho.

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>Você recebe o erro MSB4018 A tarefa "GenerateResource" falhou inesperadamente

Isso poderá acontecer quando você estiver tentando converter assemblies de satélite em arquivos PRI (Índice de Recurso do Pacote).

Estamos cientes desse problema e estamos trabalhando em um solução de longo prazo. Como uma solução temporária, você pode desabilitar o gerador de recursos, adicionando esta linha de XML ao primeiro elemento PropertyGroup do arquivo de projeto de hospedagem:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Tela azul com código de erro 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Depois de instalar ou iniciar o Microsoft Store de determinados aplicativos, ele pode inesperadamente reiniciar com o erro: **0x139 (KERNEL\_segurança\_Verifique\_ falha)** .

Entre os aplicativos afetados conhecidos estão Kodi, JT2Go, Ear Trumpet, Teslagrad e outros.

Uma [atualização do Windows (versão 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) foi lançada em 27/10/16 incluindo correções importantes que resolvem esse problema. Se você enfrentar esse problema, atualize o computador. Se não conseguir atualizar o computador porque ele reinicia antes de você conseguir fazer logon, você deverá usar a restauração do sistema para restaurar o sistema até um ponto anterior ao momento em que instalou um dos aplicativos afetados. Para obter informações sobre como usar a restauração do sistema, consulte [Opções de recuperação no Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

Se a atualização não corrigir o problema ou se você não souber como recuperar o computador, entre em contato com o [Suporte da Microsoft](https://support.microsoft.com/contactus/).

Se for um desenvolvedor, você desejará impedir a instalação do aplicativo empacotado em versões do Windows que não incluam essa atualização. Observe que, ao fazer isso em seu aplicativo não estará disponível para os usuários que ainda não tem instalado a atualização. Para limitar a disponibilidade do seu aplicativo para usuários que instalaram essa atualização, modifique o arquivo Appxmanifest XML da seguinte maneira:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Os detalhes a respeito do Windows Update podem ser encontrados em:
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>Erros comuns que podem aparecer quando você assina seu aplicativo

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>Incompatibilidade de publicador e cert faz com que o Signtool erro "Erro: Falha de SignerSign()"(-2147024885/0x8007000b)

A entrada de Fornecedor no manifesto do pacote de aplicativo do Windows deve corresponder ao Assunto do certificado que você está assinando.  Você pode usar qualquer um dos seguintes métodos para ver o assunto do certificado.

**Opção 1: Powershell**

Execute o seguinte comando do PowerShell: Ambos .cer ou .pfx pode ser usado como o arquivo de certificado, pois eles têm as mesmas informações de fornecedor.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Opção 2: Explorador de arquivos**

Clique duas vezes no certificado no Explorador de Arquivos, selecione a guia *Detalhes* e depois o campo *Assunto* na lista. Em seguida, você pode copiar o conteúdo.

**Opção 3: CertUtil**

Execute **certutil** da linha de comando no arquivo PFX e copiar a *assunto* campo da saída.

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert" />

### <a name="bad-pe-certificate-0x800700c1"></a>Certificado incorreto de PE (0x800700C1)

Isso pode acontecer quando o pacote contenha um binário que tem um certificado corrompido. Eis algumas das razões por que isso pode acontecer:

* O início do certificado não está no final de uma imagem.  

* O tamanho do certificado não é positivo.

* Não é o início do certificado após o `IMAGE_NT_HEADERS32` estrutura para um executável de 32 bits ou após o `IMAGE_NT_HEADERS64` estrutura para um executável de 64 bits.

* O ponteiro de certificado não está alinhado corretamente para uma estrutura WIN_CERTIFICATE.

Para localizar arquivos que contêm um certificado incorreto do PE, abra uma **Prompt de comando**e defina a variável de ambiente denominada `APPXSIP_LOG` como um valor de 1.

```
set APPXSIP_LOG=1
```

Em seguida, do **Prompt de comando**, assinar o aplicativo novamente. Por exemplo:

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

Informações sobre os arquivos que contêm um certificado incorreto do PE aparecerão na **janela do Console**. Por exemplo:

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```
## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
