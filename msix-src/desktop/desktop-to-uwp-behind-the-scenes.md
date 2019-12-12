---
Description: Este artigo se aprofunda no funcionamento da Ponte de Desktop nos bastidores.
title: Nos bastidores da Ponte de Desktop
ms.date: 04/25/2019
ms.topic: article
keywords: Windows 10, UWP, MSIX
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: c3f2d5adb3bdd790b3fef7731faebd90089f8a59
ms.sourcegitcommit: d749fa662214bddaa6854f1ee95761c547db8dae
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2019
ms.locfileid: "75008096"
---
# <a name="behind-the-scenes-of-your-packaged-desktop-application"></a>Nos bastidores do aplicativo de área de trabalho empacotado

Este artigo fornece mais detalhes sobre o que acontece com arquivos e entradas de registro quando você cria um pacote de aplicativo do Windows para seu aplicativo de desktop.

Uma meta importante de um pacote moderno é separar o estado do aplicativo do estado do sistema tanto quanto possível, mantendo a compatibilidade com outros aplicativos. O Windows 10 realiza isso colocando o aplicativo dentro de um pacote MSIX e, em seguida, detectando e redirecionando algumas alterações feitas no sistema de arquivos e no registro em tempo de execução.

Os pacotes que você cria para seu aplicativo de área de trabalho são aplicativos somente de área de trabalho, confiança total e não são virtualizados nem protegidos. Isso permite que eles interajam com outros aplicativos da mesma maneira que aplicativos da área de trabalho clássicos.

## <a name="installation"></a>Instalação

Os pacotes de aplicativos são instalados em *C:\Program Files\WindowsApps\package_name*, com o executável intitulado *app_name.exe*. Cada pasta de pacote contém um manifesto (chamado AppxManifest.xml) que contém um namespace XML especial para apps empacotados. Dentro desse arquivo de manifesto está um elemento ```<EntryPoint>```, que faz referência ao aplicativo de confiança total. Quando esse aplicativo é iniciado, ele não é executado dentro de um contêiner de aplicativo, mas, em vez disso, ele é executado como o usuário normalmente.

Depois da implantação, os arquivos do pacote serão marcados como somente leitura e totalmente bloqueados pelo sistema operacional. O Windows evitará a inicialização dos aplicativos se esses arquivos forem adulterados.

## <a name="file-system"></a>Sistema de arquivos

O sistema operacional dá suporte a diferentes níveis de operações de sistema de arquivos para aplicativos de área de trabalho empacotados, dependendo do local da pasta.

### <a name="appdata-operations-on-windows-10-version-1903-and-later"></a>Operações de AppData no Windows 10, versão 1903 e posterior

Todos os arquivos e pastas recém-criados na pasta AppData do usuário (por exemplo, *c:\users\ user_name \AppData*) são gravados em um local privado por usuário, por aplicativo, mas mesclados em tempo de execução para serem exibidos no local de AppData real. Isso permite algum grau de separação de estado para artefatos que são usados apenas pelo próprio aplicativo, e isso permite que o sistema Limpe esses arquivos quando o aplicativo é desinstalado. As modificações nos arquivos existentes na pasta AppData do usuário têm permissão para evitar um nível mais alto de compatibilidade e interatividade entre aplicativos e o sistema operacional. Isso reduz o sistema de arquivos "corrompidos" porque o sistema operacional está ciente de cada alteração de arquivo ou diretório feita por um aplicativo. A separação de estado também permite que aplicativos de área de trabalho empacotados selecionem onde uma versão não empacotada do mesmo aplicativo parou. Observe que o sistema operacional não dá suporte a uma pasta VFS (sistema de arquivos virtual) para a pasta AppData do usuário.

### <a name="appdata-operations-on-windows-10-version-1809-and-earlier"></a>Operações de AppData no Windows 10, versão 1809 e anterior

Todas as gravações na pasta AppData do usuário (por exemplo, *c:\users\ user_name \AppData*), incluindo criar, excluir e atualizar, são copiadas na gravação para uma localização privada por usuário, por aplicativo. Isso cria a ilusão de que o aplicativo empacotado está editando os AppData reais quando está realmente modificando uma cópia privada. Redirecionando gravações dessa maneira, o sistema pode acompanhar todas as modificações de arquivo feitas pelo aplicativo. Isso permite que o sistema Limpe esses arquivos quando o aplicativo é desinstalado, reduzindo assim o sistema "corrompidos" e fornecendo uma melhor experiência de remoção do aplicativo para o usuário.

### <a name="other-folders"></a>Outras pastas

Além de redirecionar AppData, as pastas conhecidas do Windows (system32, arquivos de programas (x86), etc.) são mescladas dinamicamente com os diretórios correspondentes no pacote do aplicativo. Cada pacote contém uma pasta chamada "VFS" na raiz. Todas as leituras de diretório ou arquivo no diretório VFS são mescladas em tempo de execução às respectivas contrapartes nativas. Por exemplo, um aplicativo pode conter *C:\Program files\windowsapps\ package_name \vfs\systemx86\vc10.dll* como parte de seu pacote do aplicativo, mas o arquivo pareceria estar instalado em *C:\Windows\System32\vc10.dll*.  Isso mantém a compatibilidade com aplicativos da área de trabalho, que podem esperar que arquivos estejam em locais sem pacote.

As gravações em arquivos/pastas no pacote de apps não são permitidas. As gravações em arquivos e pastas que não fazem parte do pacote são ignoradas pelo sistema operacional e são permitidas desde que o usuário tenha permissão.

### <a name="common-operations"></a>Operações comuns

Essa tabela de referência curta mostra operações comuns do sistema de arquivos e como o sistema operacional as manipula.

| Operação | Resultado | Exemplo |
| --- | --- | --- |
| Ler ou enumerar um arquivo ou uma pasta do Windows conhecida | Uma mescla dinâmica de *C:\Program Files\package_name\VFS\well_known_folder* à contraparte do sistema local. | A leitura de *C:\Windows\System32* retorna o conteúdo de *C:\Windows\System32* mais o conteúdo de *C:\Program Files\WindowsApps\package_name\VFS\SystemX86*. |
| Gravar em AppData | **Windows 10, versão 1903 e posterior:** Novos arquivos e pastas criados nos seguintes diretórios são redirecionados para um local privado por pacote por usuário:<ul><li>Local</li><li>Local\Microsoft</li><li>Roaming</li><li>Roaming\Microsoft</li><li>Roaming\Microsoft\Windows\Start Iniciar\programas</li></ul>Em resposta a um comando File Open, o sistema operacional abrirá o arquivo do local por pacote por usuário primeiro. Se esse local não existir, o sistema operacional tentará abrir o arquivo do local de AppData real. Se o arquivo for aberto a partir do local de AppData real, não ocorrerá nenhuma virtualização para esse arquivo. As exclusões de arquivo em AppData serão permitidas se o usuário tiver permissões.<p/><p/>**Windows 10, versão 1809 e anterior:** Copiar em gravação para uma localização por usuário e por aplicativo. | AppData costuma ser *C:\Users\user_name\AppData*. |
| Gravar no pacote | Não permitido. O pacote é somente leitura. | Gravações em *C:\Program Files\WindowsApps\package_name* não são permitidas. |
| Gravações fora do pacote | Permitido se o usuário tiver permissões. | Uma gravação em *C:\Windows\System32\foo.dll* será permitida se o pacote não contiver *C:\Program Files\WindowsApps\package_name\VFS\SystemX86\foo.dll* e o usuário tiver permissões. |

### <a name="packaged-vfs-locations"></a>Locais dos pacotes VFS

A tabela a seguir mostra onde os arquivos fornecidos como parte do pacote são sobrepostos no sistema para o aplicativo. Seu aplicativo perceberá que esses arquivos estão nos locais de sistema listados, quando, na verdade, estão nos locais redirecionados dentro de *C:\Program files\windowsapps\ package_name \vfs*. Os locais de FOLDERID são das constantes [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx).

Local do sistema | Local redirecionado (em [PackageRoot] \VFS\) | Válido em arquiteturas
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | Windows | x86, amd64
FOLDERID_ProgramData | AppData comum | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>Registro

Os pacotes de apps contêm um arquivo registry.dat, que funciona como o equivalente lógico de *HKLM\Software* no Registro real. Em tempo de execução, esse Registro virtual mescla o conteúdo desse hive ao hive do sistema nativo para oferecer uma visão singular de ambos. Por exemplo, se registry.dat contiver uma única chave "Foo", uma leitura de *HKLM\Software* em tempo de execução também conterá aparentemente "Foo" (além de todas as chaves do sistema nativo).

Somente as chaves em *HKLM\Software* fazem parte do pacote; as chaves em *HKCU* ou outras partes do Registro não fazem parte. Gravações em chaves ou valores no pacote não são permitidas. Gravações em chaves ou valores que não fazem parte do pacote são permitidas desde que o usuário tenha permissão.

Todas as gravações em HKCU são cópias em gravações para um local particular por usuário e por aplicativo. Tradicionalmente, os desinstaladores são conseguem limpar *HKEY_CURRENT_USER* porque os dados do Registro para usuários desconectados estão desmontados e não estão disponíveis.

Todas as gravações são mantidas durante a atualização do pacote e excluídas somente quando o aplicativo é completamente removido.

### <a name="common-operations"></a>Operações comuns

Essa tabela de referência curta mostra operações de registro comuns e como o sistema operacional as manipula.

Operação | Resultado | Exemplo
:--- | :--- | :---
Ler ou enumerar *HKLM\Software* | Uma mesclagem dinâmica do hive do pacote à contraparte do sistema local. | Se registry.dat contiver uma única chave "Foo", em tempo de execução, uma leitura de *HKLM\Software* mostrará o conteúdo de *HKLM\Software* mais *HKLM\Software\Foo*.
Gravações em HKCU | Copiar gravação por usuário, local particular de cada aplicativo. | Igual a AppData para arquivos.
Grava no pacote. | Não permitido. O pacote é somente leitura. | As gravações em *HKLM\Software* não serão permitidas, se um par chave/valor correspondente existir no hive do pacote.
Gravações fora do pacote | Ignorado pelo sistema operacional. Permitido se o usuário tiver permissões. | As gravações em *HKLM\Software* são permitidas desde que um par chave/valor correspondente não exista no hive do pacote e o usuário tenha permissões de acesso corretas.

## <a name="uninstallation"></a>Desinstalação

Quando um pacote é desinstalado pelo usuário, todos os arquivos e pastas localizados em *C:\Program files\windowsapps\ package_name* são removidos, bem como quaisquer gravações redirecionadas para AppData ou o registro que foram capturados durante o processo de empacotamento.

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fornecer comentários ou fazer sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
