
# Usando IPFS para sistemas de armazenamento de arquivos distribuídos

*IPFS (InterPlanetary File System)* é uma solução de armazenamento descentralizada para conteúdo baseado em blockchain. O IPFS usa um modelo de rede P2P (Peer-to-Peer) para compartilhamento de arquivos que é descentralizado e distribuído em muitos computadores ou nós. Os arquivos são divididos em diferentes partes e armazenados em uma rede de nós, que rastreia o arquivo por hashes. Quando as peças são montadas juntas, com base em seu *valor de hash*, ele recria o arquivo original.
[](figuras/arquivos.png)

## Armazenamento de arquivos descentralizado

O uso de *tabelas de hash distribuídas ( Distributed Hash Tables - DHT)* para armazenamento e recuperação de sistema de arquivos é a inovação central do IPFS. É semelhante ao protocolo BitTorrent, mas diferente na forma como apontam para o arquivo para compartilhamento. Isso armazena arquivos em um blockchain como pares de valores de chave. Os dados são divididos em blocos de 256 KB e espalhados por uma rede de nós ou computadores. É coordenado de forma eficiente para permitir acesso e pesquisa eficientes entre os nós. O BitTorrent não usa blockchain, mas depende de torrents para apontar para arquivos. Você pode ter torrents diferentes apontando para o mesmo arquivo, mas no IPFS você só precisa de um ID de hash que aponta para um arquivo.
Os arquivos não são postados no IPFS da mesma forma que postar um arquivo na nuvem. Todos os dados no IPFS são endereçados por seu ID de hash. Quando alguém solicita esses dados, eles estão solicitando esses dados diretamente por seu ID de hash e não pelo próprio arquivo. O IPFS, portanto, fornece uma abstração da localização real do arquivo, de forma que a localização física real não importa para o aplicativo. Essa abstração remove a complexidade para desenvolvedores de aplicativos.
[](figuras/armazenamento_ipfs.png)
Os nós hospedam o arquivo na rede. Eles são incentivados a fazer isso por um ativo digital como o Filecoin em um blockchain IPFS. Os nós são incentivados a fornecer espaço de armazenamento em seu computador ou servidor para hospedar arquivos. Esses arquivos recebem um hash ID que pode ser distribuído pela rede. Outros nós também podem hospedar o mesmo arquivo, permitindo que muitas cópias dele sejam feitas. Os usuários que desejam o arquivo irão acessá-lo com base no hash do nó mais próximo de sua localização.

Todos os nós que hospedam o arquivo farão referência a um hash raiz que é o ID de hash do arquivo. Sempre que uma solicitação de arquivo é feita, o hash do nó mais próximo que armazena o arquivo com base em seu hash raiz é usado pelo usuário para fazer o download do arquivo. Não há duplicatas no IPFS, porque o hash sempre se referirá ao arquivo ou a um pedaço do arquivo quando foi carregado.
Depois que um arquivo é colocado em uma blockchain IPFS, ele permanece disponível até que seja removido, desafixando um arquivo e executando uma rotina de coleta de lixo. O próprio arquivo pode ter diferentes nós apontando para ele por seu hash. Nós diferentes também podem hospedar o arquivo, desde que haja o hash apontando para ele. O IPFS pode ser atualizado para apontar para diferentes hashes, mas qualquer pessoa com o hash original ainda pode acessar esses dados, desde que pelo menos um nó ainda hospede esses dados agora ou a qualquer momento no futuro.

## Esquemas de endereçamento de armazenamento
O que diferencia o IPFS dos sistemas típicos de armazenamento baseados na Internet na nuvem é que ele é baseado em conteúdo (conteúdo endereçado) e não baseado em localização (local endereçado). Um exemplo de sistema de armazenamento com endereço de localização é o protocolo HTTP. Quando um sistema de armazenamento é baseado em localização, trata-se de identificar um servidor por seu nome de host usando um servidor DNS. Isso rastreia um host por um esquema de endereçamento lógico (por exemplo, endereço IP) mapeado para um nome amigável. Se o host mudar seu nome ou endereço, ele também deve ser modificado na tabela de serviço de nomes.

O armazenamento de endereçamento baseado em conteúdo pertence ao conteúdo para obter dados da rede. Isso requer um identificador de conteúdo que determina a localização física de um arquivo. Nesse caso, os dados são acessados ​​com base em seu hash criptográfico em vez de no endereço lógico, muito parecido com a impressão digital de um arquivo. A rede sempre retornará o mesmo conteúdo com base nesse hash, independentemente de quem carregou o arquivo, onde e quando foi carregado.

Quando se trata de velocidade e confiabilidade, o IPFS pode ter um desempenho melhor do que o HTTP. Em vez de depender de um local de servidor para obter um arquivo, um sistema de armazenamento de conteúdo endereçado pode fornecer o arquivo de vários servidores (por exemplo, um par ou nó na rede IPFS) que estão mais próximos do usuário. Em outras palavras, um usuário pode simplesmente pesquisar um arquivo sem que um mecanismo de pesquisa precise fazer referência ao local, ou seja, o nome ou endereço do servidor. Em vez disso, eles farão referência a ele pelo hash do arquivo, e ele estará disponível nos nós disponíveis mais próximos na rede.

## Instalando IPFS
Existem 2 opções de nós para uma instalação comum de IPFS.

 - IPFS Desktop - Hospede e compartilhe arquivos diretamente de um computador (laptop ou PC de mesa). Um aplicativo complementar IPFS pode ser instalado para permitir o acesso a um nó local usando um navegador da web. Este é o tipo de instalação para compartilhamento de arquivos do tipo peer.
 
 - Cluster IPFS - Para hospedar e compartilhar arquivos em escala, o cluster permite orquestrar e coordenar conjuntos de pinos em um enxame de nós IPFS. Isso permite que um sistema de armazenamento de arquivos em grande escala seja construído por meio de nós distribuídos.
 
 Depois de instalar a área de trabalho IPFS básica, a configuração do nó começa com a inicialização do repositório. A seguir estão os comandos que você digita em um terminal shell do Windows Powershell ou Mac / Linux.
 
```bash
ipfs init
> initializing ipfs node at /Users/<username>/.go-ipfs
> generating 2048-bit RSA keypair...done
> peer identity: Qmcpo2iLBikrdf1d6QU6vXuNb6P7hwrbNPW9kLAH8eG67z
> to get started, enter:
>
```

Esta inicialização é executada ao usar o IPFS pela primeira vez. A próxima etapa é executar o processo daemon IPFS para unir o nó à rede.

```bash
ipfs daemon
> Initializing daemon...
> API server listening on /ip4/127.0.0.1/tcp/5001
> Gateway server listening on /ip4/127.0.0.1/tcp/8080
```

Isso inicializa e executa o processo daemon na máquina local 127.0.0.1. Ele lança um API Server que escuta na porta TCP 5001 e um Gateway Server na porta TCP 8080. Agora você deve ser capaz de ver outros nós IPFS na rede emitindo o comando swarm. Deve ser parecido com o seguinte:

```bash
ipfs swarm peers
> /ip4/104.131.131.82/tcp/4001/p2p/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
> /ip4/104.236.151.122/tcp/4001/p2p/QmSoLju6m7xTh3DuokvT3886QRYqxAzb1kShaanJgW36yx
> /ip4/134.121.64.93/tcp/1035/p2p/QmWHyrPWQnsz1wxHR219ooJDYTvxJPyZuDUPSDpdsAovN5
> /ip4/178.62.8.190/tcp/4002/p2p/QmdXzZ25cyzSF99csCQmmPZ1NTbWTe8qtKFaZKpZQPdTFB
```
Conforme explicado na documentação do IPFS, os pares assumem o formato:

*<transport address>/p2p/<hash-of-public-key>*

Aqui está um exemplo de comando para obter um arquivo na rede:

```bash
ipfs cat /ipfs/QmW2WQi7j6c7UgJTarActp7tDNikE4B2qXtFCfLPdsgaTQ/cat.jpg 
> cat.jpg
open cat.jpg
```

Isso obtém um objeto do par especificado chamado ‘cat.jpg’ e o abre localmente.

## Javascript com IPFS
A seguir está o código de teste para gravar dados na rede IPFS usando Runkit NPM e o gateway Infura (gratuito para o público).

```javascript
const IPFS = require('ipfs-mini' 1.1.5 );
const ipfs = new IPFS({host: 'ipfs.infura.io', port: 5001, protocol: 'https'});
const data = "Writing a test message on the network";
ipfs.add(data, (err, hash) => {
    if(err){
        return console.log(err);
    }
    console.log('https://ipfs.infura.io/ipfs/'+hash);
})
```
Neste código, solicito o pacote ‘ipfs-mini’ no Node.JS usando a função require. Em seguida, configuro o acesso ao gateway Infura IPFS ‘ipfs.infura.io’. Em seguida, especifico os dados como a string “Escrevendo uma mensagem de teste na rede”. Em seguida, crio uma condição para retornar um erro se houver um problema, caso contrário, quero o valor do hash e, em seguida, registro do console a URL do gateway mais o valor do hash.
O resultado retornará o hash exclusivo:
*QmQhadgstSRUv7aYnN25kwRBWtxP1gB9Kowdeim32uf8Td*

Agora posso inserir o link da URL:
[https://ipfs.infura.io/ipfs/QmQhadgstSRUv7aYnN25kwRBWtxP1gB9Kowdeim32uf8Td](https://ipfs.infura.io/ipfs/QmQhadgstSRUv7aYnN25kwRBWtxP1gB9Kowdeim32uf8Td)

Isso mostrará os dados que acabei de inserir no gateway Infura. Os dados não são persistentes e serão removidos após alguns dias ou semanas de inatividade. Para armazenamento de dados persistente, um servidor dedicado é necessário no local ou em uma nuvem.
[](figuras/javascript.png)

## Os prós do IPFS

 - Descentralização - Os arquivos são armazenados em uma rede de nós, referenciados por hashes. O incentivo aos nós para hospedar arquivos é através do Filecoin.
 - Tolerância a falhas - se um nó falhar, o arquivo ainda estará disponível, desde que haja nós hospedando o arquivo. Não existe um ponto único de falha.
 - Escalabilidade - Quanto mais nós houver para hospedar arquivos, mais rápido e mais disponível ele se tornará para os usuários na rede.
 - Armazenamento persistente - O ponto principal do IPFS é o armazenamento de dados: desde que os objetos correspondentes aos dados originais e quaisquer novas versões estejam acessíveis, todo o histórico do arquivo pode ser recuperado. Dado que os blocos de dados são armazenados localmente na rede e podem ser armazenados em cache indefinidamente, isso significa que os objetos IPFS podem ser armazenados permanentemente sem serem modificados.
 - Resistência à censura - Uma vez que o conteúdo foi carregado para IPFS, nenhuma autoridade central pode removê-lo porque ele é distribuído por uma rede inteira. Removê-lo de apenas um nó não exclui o arquivo inteiramente. Isso significa que ainda há cópias disponíveis em outros nós.
 
## Os contras do IPFS

 - Não amigável - A maneira como os arquivos são indexados em uma rede IPFS não é muito amigável. Por exemplo, para acessar um arquivo por seu ID de hash, é necessário digitar:

 ```bash
 ipfs.io/ipns/QmeQe5FTgMs8PNspzTQ3LRz1iMhdq9K34TQnsCP2jqt8wV
 ```
Os desenvolvedores podem compartilhar arquivos usando links, mas isso pode se tornar tedioso e um processo demorado. O IPFS usa IPNS (Sistema de Nomes Interplanetário) para pesquisar arquivos. O IPNS tentará tornar a resolução de nomes mais amigável, assim como o DNS na Internet. Há uma interface gráfica e um aplicativo complementar IPFS baseado na web que os usuários podem usar para facilitar o acesso. No entanto, ainda não é tão fácil de usar ou simples de usar como um aplicativo de smartphone comum, pois a curva de aprendizado é mais íngreme. Não é tão simples quanto clicar em botões em uma página da web. Os usuários terão que saber como funciona o IPFS para poder usá-lo.

 - Privacidade e conformidade de dados - Não é uma prática recomendada colocar dados do cliente, como informações de identificação pessoal (PII), como KYC, em um sistema de armazenamento público compartilhado usando IPFS. Primeiro, ele viola as regras de conformidade de armazenamento que afirmam que os dados KYC não podem e não devem ser expostos em uma nuvem pública ou espaço de armazenamento compartilhado, e incluiria IPFS. Estar em uma nuvem pública coloca menos controle sobre a organização para gerenciar os dados. Requisitos rígidos para instituições financeiras é ter os dados e backup dos dados em um sistema de armazenamento regulamentado e não público. Outro problema aqui é que, por estar em uma rede pública, qualquer nó pode hospedar os dados KYC. Isso é mais uma violação das leis que impõem estritamente quem e onde os dados podem ser armazenados.
O segundo problema é que todos os nós devem estar em conformidade com as regras e regulamentos dos sistemas financeiros, o que significa que eles devem ter backup, forte segurança, tolerância a falhas, etc. Em uma rede pública, os nós são aleatórios e não podem ser feitos para cumprir com os regras porque eles não precisam confiar em seu sistema. Eles também podem disponibilizar os dados KYC para outros usuários na rede, que os malfeitores podem acessar, mesmo se estiverem criptografados. Eles podem descriptografá-lo por conta própria e isso lhes dá um caminho para fazer isso.

 - Inconsistência de dados - No IPFS, também há pouco incentivo para os nós manterem backups de longo prazo dos dados na rede. Os nós podem escolher limpar os dados em cache para economizar espaço, o que significa que, teoricamente, os arquivos podem "desaparecer" com o tempo se não houver nós restantes hospedando os dados. Nos níveis de adoção atuais, isso não é um problema significativo, mas, a longo prazo, fazer backup de grandes quantidades de dados requer fortes incentivos econômicos.
O problema aqui é que, se uma empresa usa uma rede IPFS pública para armazenamento de arquivos, os nós podem, a qualquer momento, optar por não hospedar o arquivo no futuro. Se todos os nós decidirem fazer isso, não haverá como manter o arquivo na rede, a menos que o IPFS esteja hospedado em uma rede privada. De acordo com o protocolo IPFS, se o arquivo adicionado à rede IPFS não for acessado por muitas pessoas, ele desaparece. Seus dados precisam ser mais populares na rede para que sejam permanentes. Se você nunca deseja que seus dados desapareçam da rede IPFS, você deve fixar seus dados em seu nó. A pinagem garante que pelo menos o seu nó na rede tenha esses dados.
Como o IPFS é descentralizado, todos os nós de hospedagem terão uma cópia do arquivo que você carregou. Normalmente, os arquivos são removidos se não estiverem ativos ou se forem usados ​​com frequência. Isso pode ser um problema muito contencioso, porque às vezes os arquivos são arquivados e não são usados ​​com frequência e, em outras ocasiões, precisam ser excluídos imediatamente. Quando os dados que já estão armazenados no IPFS são alterados, seu hash também deve ser alterado. Se houver uma nova versão, você terá que carregá-la, mas ela não sobrescreve a versão anterior. Isso afeta os links existentes para o arquivo, portanto, o original permanece inalterado, mas agora você precisará criar um novo link para um novo arquivo.
Isso pode ser um desafio ao atualizar os dados KYC, que incluem passaportes e carteiras de motorista. Quando esses documentos expiram, uma nova versão deve ser carregada para substituir a antiga. O IPFS fornece controle de versão, mas se torna complicado quando é colocado em uma rede pública, porque muitas versões podem existir em nós diferentes. A versão antiga não é atualizada automaticamente. O antigo deve ser arquivado ou destruído. O IPFS não pode arquivar o arquivo da mesma forma que no AWS ou Azure.
O IPFS possui um sistema de controle de versão. Este é um recurso da estrutura Merkle DAG do IPFS que permite construir um sistema de controle de versão distribuído (VCS). O exemplo mais popular disso é o Github, que permite aos desenvolvedores colaborar facilmente em projetos simultaneamente. Os arquivos no Github são armazenados e com controle de versão usando um Merkle DAG. Ele permite aos usuários duplicar e editar de forma independente várias versões de um arquivo, armazenar essas versões e, posteriormente, mesclar as edições com o arquivo original. No entanto, isso é praticamente em teoria de acordo com muitos desenvolvedores, e ainda não é uma tecnologia totalmente testada e comprovada que funcione (até o momento desta escrita). Se formos implementá-lo, isso exigiria mais tempo e custos de desenvolvimento, o que pode ser bom a longo prazo.

[](figuras/consistencia.png)

## SINOPSE

O IPFS é mais ideal para armazenamento permanente de dados, como música digital, obras de arte e credenciamentos (por exemplo, certificados, diplomas, prêmios, doações). Esses são tipos de dados que não precisam ser alterados e colocá-los em um sistema de armazenamento baseado em blockchain faz mais sentido. Ele fornece ao criador ou artista uma prova digital que não pode ser alterada por ninguém e que pode ser demonstrada por meio de um sistema baseado em hash com pares de valores-chave exclusivos para um único item.
O armazenamento IPFS também é mais público, portanto, a confidencialidade dos dados é necessária. Isso pode ser uma violação para certos tipos de armazenamento de dados que expõem dados privados (por exemplo, regras GDPR). Também estão disponíveis soluções escalonáveis ​​para armazenamento de dados na nuvem AWS e Azure que atendem à privacidade, segurança e conformidade. Na minha opinião, não sinto necessidade de distribuir informações pessoais da mesma forma que o IPFS armazena dados. Para conteúdo disponibilizado publicamente, o uso do IPFS pode fornecer uma prova de autenticidade ao proprietário desse conteúdo. Isso pode garantir o trabalho de um criador como arte, comprovando sua propriedade, o que lhes permite coletar royalties e evitar que outros recebam crédito por algo que eles não criaram.
Parece que o IPFS oferece armazenamento de arquivos tolerante a falhas rápido e seguro para conteúdo. No entanto, pode não ser adequado para dados financeiros e pessoais que exigem regulamentação estrita de como os dados são armazenados e protegidos. Isso também não é recomendado para armazenar arquivos que mudam com frequência devido a atualizações, como arquivos de log ativos que gravam dados continuamente. O armazenamento de dados no blockchain tem um propósito diferente, e o IPFS fornece essa solução. Conforme o IPFS evolui, ele poderia usar uma camada de privacidade que pode ocultar dados pessoais que também são criptografados em repouso, para que não haja violação de exposição de qualquer coisa confidencial.

## Fonte
[https://medium.com/0xcode/using-ipfs-for-distributed-file-storage-systems-61226e07a6f](https://medium.com/0xcode/using-ipfs-for-distributed-file-storage-systems-61226e07a6f)






















