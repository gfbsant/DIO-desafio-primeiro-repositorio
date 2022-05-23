# __INTRODUÇÃO AO GIT__

## 1. Entendendo o Git e a Sua importância

O Git é um software de versionamento de cógido distribuído, feito de forma colaborativa, lançado em 2005 e que tem como um de seus fundandores o programador Linus Torvalds.

Já o GitHub, é um repositório remoto, criado pela Microsoft e possui uma série de beneficios, como o controle de versão, armazenamento em nuvem, trabalho em equipe, melhora colaborativa dos códigos e reconhecimento por parte de seus pares. 

## 2. Navegação via Command Line Interface

O Git não possui uma interface gráfica, sendo um CLI (command line interface) por padrão. 

Alguns dos principais comandos para o Linux (e demais maquinas baseadas em Unix) são:

**ls** - Traz a lista de pastas e arquivos do local em que você está naquele momento;

**cd** - Te coloca em um novo diretório (pasta) e significa _change directory_; 

__cd ..__ - Retrocede em um nível a sua navegação através dos diretórios;

__clear__ - Apaga os códigos digitados previamente no terminal;

__Tecla TAB__ - Atua como uma espécie de autocomplete;

__sudo su__ - confere privilégios de administrador (root), e deve ser usado com parcimonia, haja visto que um arquivo ou uma pasta criada enquanto root não suporta modificações fora do privilégio de adm;

__mkdir__ - cria uma nova pasta dentro do nível em que você se encontra;

__echo >__ - cria um novo arquivo dentro do repositório (exemplo: _echo > README.md_)

__rm -rf__ - deleta uma pasta especificada, onde _rm_ siginfica _remove_, _-r_ siginfica _recursivo_ e _f_ significa _force_;

## 3. Instalação no Linux

Primeiro, acesse o site oficial do Git: https://git-scm.com/download/linux

Em seguida, utilize a versão upstream do Git. Caso sua flavor seja baseada em Ubuntu, abra o terminal e digite os seguintes comandos: 

`add-apt-repository ppa:git-core/ppa`

`apt update`

`apt install git`

Finalize usando o comando `git --version` para verificar se tudo está correto com a instalação. 

## 4. Tópicos fundamentais para entender o funcionamento do Git

__sha1__ - Trata-se de um algoritmo de encriptação, gerando um identificado de 40 digitos que se altera conforme ocorrem modificações no conteudo dos repositórios.

No terminal, caso seja digitado `openssl sha1 texto.txt`, por exemplo, gerará um código especifico e caso esse arquivo sofra novas modificações, o código _sha1_ se modificará. 

## 5. Objetos internos do Git

__blobs__ - são "bolhas", gerando metadados que referem-se á estrutura básica do arquivo ao qual ela faz referencia, como o tipo de objeto e o o tamanho do arquivo, por exemplo.

`echo -e "contúdo" | openssl sha1` devolverá um sha1; 

Já `echo "conteudo" | git hash-object --stdin`, apesar de apresentar a mesma função, retornará um sha1 completamente diferente. 

__trees__ - armazenam diferentes blobs , montando toda a estrutura dos arquivos. As trees também tem um sha1 especifico e caso alguma das blobs que fazem parte dela seja modificado, esse sha1 também será alterado.

As trees apontam para blobs e também para outrasa trees. 

__commit__ - aponta para uma arvore, um parente (linha do tempo), um autor, uma mensagem (-m) e um timestamp. 

Os commits, como já é de esperar, também são identificaveis através do sha1, e caso algo dentro de sua estrutura seja modificado, seu sha1 será modificado também. 

## 6. Chave SSH e Token

__SSH__ - É uma forma de estabelecer uma conexão segura entre o repositório local e o remoto. 

Para criar a chave, é necessario abrir o terminal e digitar os seguintes cógidos: 

`ssh-keygen -t ed25519 -C email@exemplo.com` onde o email a ser adicionado deve ser preferencialmente o mesmo utilizado na sua conta do GitHub, e logo em seguida definir a pasta onde esse SSH será armazenado e definir a senha. 

Em seguinda, no terminal devemos abrir o local onde foi salvo a chave, que no meu caso está no folder principal: 

`cd /home/user/.ssh/`

`ls`

`cat id_ed25519.pub`

E a partir daí, teremos acesso á chave publica que será adicionada ao SSH do GitHub na aba de configuraçoes. 

Em seguida, devemos inicializar o SSH Agente, onde, dentro da pasta .ssh no terminal, vamos digitar os seguintes cógidos: 

`eval $(ssh-agente -s)`

`ssh-add id_ed25519`

E por fim, a senha que configuramos durante o _keygen_. 

Desse momento em diante, para efetuar um _git clone_ devemos usar a opção SSH. 

Em alguns momentos, podemos usar também o token de acesso pessoal. Para tanto, devemos acessar no site do GitHub a opção _developer options_ e criar um novo token de acesso pessoal colocando-o em um folder protegido dentro da maquina, haja visto que ele só poderá ser acessado uma unica vez de forma remota. 

## 7. Inicializando o Git e criando um commit

Devemos acessar a pasta destino dos commits, que no caso pode ser chamada de "workspace" no folder "/home/user/". 

Denrto da pasta "workspace" podemos criar o diretórios "livro-receitas" para fins de exemplo, utilizando o comando `mkdir livro-receitas`.

Feitos isso, podemos acionar dentro da pasta _livro-receitas_ o comando `git init` e para verificar se tudo correu corretamente, rodar o codigo `ls -a` haja visto que a pasta _.git/_ é criada no formato de hidden folder. 

Em seguidam devemos configurar o email e o user que fará parte dos commits. Para tanto, executaremos os seguintes código dentro do folder _livro-receitas_: 

`git config --global user.email "email@exemplo.com"`

`git config --global user.name "nome exemplo"`

O ideal aqui é que seja utilizado o email e o username identicos aos usados em sua conta no GitHub para que o commit mostre sua identidade de forma correta. 

Em seguida, podemos adicionar o primeiro arquivo em nosso livro de receitas intitulado "bolo-de-chocolate.md", por exemplo. Aqui é importante ressaltar que o nome do arquivo invaria, haja visto que o que realmente importa é o formato do arquivo _.md_. Podemos usar o `echo > bolo-de-chocolate` para criar o arquivo.

Feito isso, vamos digitar `git add .` para passar as modificações para o modo _staged_. Em seguinda, vamos fazer nosso primeiro commit digitando os seguintes códigos no terminal: 

`git commit -m "primeiro commit"`

## 8. Ciclo de vida dos arquivos no Git

O codigo `git init` introduz o conceito ao repositório especificado e o `git add` pega um arquivo _untracked_ e transfere ele para a area staged. Dessa forma, o arquivo passa do status _unmodified_ para _modified_ e se rodarmos o `git add` novamente, ele passa para area staged. 

Ja a etapa seguinte, é fazer o commit, onde um snapshot e o arquivo volta a ser considerado unmodified pelo Git. Vale ressaltar que as alterações que ocorrem na esfera local, ou seja, durante o uso offline, não influencia no respositório do servidor GitHub. O commit apenas torna o arquivo suscetivel a fazer parte do servidor. 

O comando `git status` mostra qual é o status do commit, se é necessarios adicionar algo (`git add .`) ou finalizar o processo (`git commit -m "exemplo" `). 

## 9. Trabalhando com o GitHub

Para verificar as configurações do Git na maquina Local, usaremos o código `git config --list`.

Em seguida, podemos realizar alterações, caso necessário utilizando `git config --global unset user.name` ou `git config --global unset user.email` para resetar os status de nome e email. 

No GitHub, podemos criar um novo repositório e copiar o SSH em _push existing file_ e usar o seguinte comando para criar uma origem ao repositório: `git remote add origin` adicionando ao final o link fornecido. 

Para verificar o fetch e push do nosso repositorio, podemos usar o comando `git remote -v`. 

Por ultimo, usar `git status`para verificar se não há nenhuma pendencia de por fim `git push origin master`para submeter para o repositório remoto nosso código.  

