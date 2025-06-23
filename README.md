# INSTALANDO DOCKER NO LINUX CORP
Referências:
[Install Docker Desktop on Linux](https://docs.docker.com/desktop/setup/install/linux/#kvm-virtualization-support)
[Install Docker Engine on Debian](https://docs.docker.com/engine/install/debian/#install-using-the-repository)

Antes é necessário que seja verificado se o computador possui suporte de virtualização KVM. O módulo **kvm** deveria ser carregado automaticamente caso o equipamento host possua suporte para virtualização. Para isto, utilize o comando abaixo:
``$ lsmod | grep kvm``
A resposta deverá ser algo como:
```
> $ lsmod | grep kvm
> kvm_amd               167936  0
> ccp                   126976  1 kvm_amd
> kvm                  1089536  1 kvm_amd
> irqbypass              16384  1 kvm
```

## INSTALAÇÃO USANDO O REPOSITÓRIO **apt**
### Setando o repositório **apt** do Docker
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

### Instalando os pacotes do Docker
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
apt install docker.io docker-compose

systemctl enable --now docker docker.socket containerd

```

### Verificando se a instalação ocorreu com sucesso rodando a imagem **hello-world**
```
sudo docker run hello-world
```

# GERENCIAR DOCKER COM UM USUÁRIO NON-ROOT
Adicione com o usuário não-root, passando o comando **sudo** no início. Desta forma, funcionou.
```
sudo usermod -aG docker $USER
newgrp docker
```
Caso o Docker Engine tenha sido instalado corretamente, um exemplo de resposta seria algo como mostrado abaixo:
```
$ docker run hell-world

Hello from Docker! ...
```

# COMANDOS DO DOCKER
O comando abaixo lista somente os containers rodando
``docker ps``

Para ver todos os containers (em execução e executados), use o comando acima com a flag **-all** (or **-a**)
``docker ps -a``

Os comandos acima evidenciam o id do container, a imagem, entre outras informações.

# SUBINDO UM DOCKER UBUNTU
``docker run ubuntu``
Após rodar o comando, verificamos que é baixada a imagem do docker ubuntu mas, após isso, é fechada a conexão. Verificação esta é feita com o comando **docker ps**. Isto acontece pois não existe nenhum processo em execução dentro deste ubuntu. Então, para deixar o docker ubuntu UP, usamos o comando **-it** para iteragir com o docker e também passamos o termo **bash** para que o processo bash fique em execução dentro deste ubuntu.
``docker run -it ubuntu bash``

Verifique que após rodar o comando acima, estaremos logados no terminal do próprio docker (Ubuntu) que subimos (Verifique que o nome do computador, que fica após o root@ não é o mesmo nome do seu computador).
De forma a comprovarmos que quando pararmos nosso docker ubuntu e subí-lo novamente, criaremos um diretório **new_dic** dentro da **home** deste nosso Ubuntu.
``mkdir new_dic``

Deixamos esta imagem rodando em um terminal e em outro, rodamos o comando ``docker ps`` para evidenciar que essa nossa imagem Ubuntu encontra-se em execução.
Verificamos que quando rodamos o comando ``docker ps``, em **status** temos a informação **Up <qtdTempExecut>**.

# PARANDO UM DOCKER
De forma a exemplificar, ainda continuaremos utilizando a imagem do Ubuntu.
Assim, quando rodamos o comando ``docker ps``, este comando exibe dentre as informações, o id deste docker (no nosso exemplo, o id é "ebcbfe70cc0c"). Então, para pararmos este docker (para tudo, inclusive os processos que estão rodando neste container), rodamos o comando abaixo:
``docker stop ebcbfe70cc0c``
Feito isto, no terminal no qual o docker estava rodando, verificamos que saímos do terminal do docker e voltamos para o terminal da nossa máquina.

# SUBINDO O NOSSO DOCKER UBUNTU NOVAMENTE
Para isto, rodamos o comando abaixo, apontando o id do nosso container:
``docker start ebcbfe70cc0c``
Assim, rodando o comando ``docker ps``, verificamos que o nosso container está em execução. Porem, não conseguimos interagir com ele.
Para que consigamos interagir com ele, rodamos o comando abaixo, com a flag **exec**, para executando um comando dentro daquele container, informando que queremos interagir com ele através da flag **-iti**, informando o id do container e com o que gostaríamos de iteragir que, nosso caso, seria o **bash**.
``docker exec -it ebcbfe70cc0c bash``
Fazendo isto, podemos verificar que voltamos para o **bash** do nosso container. Podemos verificar que é realmente o nosso docker, acessando o diretório **home** do nosso container e listando (comando **ls**) os diretórios dentro dele, confirmando que temos o diretório que criamos (**new_dic**).

####

# VERIFICANDO A VERSÃO DO DOCKER
``docker -v``

# BAIXANDO UMA IMAGEM DIRETAMENTE DO DOCKER HUB
Como exemplo será baixada uma imagem do wordpress. Como não informamos qual a versão que queremos, este comando irá baixar a última versão contida no DockerHub: 
``docker pull wordpress``

# VERIFICANDO AS IMAGENS EXISTENTES JÁ BAIXADAS
``docker images``

# SUBINDO UM DOCKER POR LINHA DE COMANDO
No comando abaixo temos o comando para rodar o container (**docker run**), especificamos o nome para este container (**--name meu_wordpress**), uma porta para este serviço rodar (**-p 8080:80**) e, por fim, qual a imagem que iremos rodar através desta porta (**-d wordpress**)
``docker run --name mew_wordpress -p 8080:80 -d wordpress``

# APAGANDO UM CONTAINER QUE TEMOS E APAGANDO UMA IMAGEM BAIXADA DO DOCKERHUB
Para conseguir apagar um container que criamos, primeiramente é necessário pará-lo: ``docker stop ID_DO_CONTAINER``. Feito isto, agora é possível excluir o container passando o id do mesmo:
``docker rm ID_DO_CONTAINER``
1. OBS. 01: Com o comando acima apagamos apenas o container que passamos o id. As imagens que baixamos ainda existem na máquina.
Caso queiramos também excluir a imagem, utilizamos o comando a seguir:
``docker rmi ID_DA_IMAGEM``
2. OBS. 02: Caso tenhamos mais containers utilizando uma imagem que queiramos remover, é indicado primeiramente remover os containers que a utilizam e, após isto, remover a imagem utilizando o comando já citado acima. Existe a possibilidade de remover a imagem de modo forçado, mas não é indicado por poder a vir a gerar alguns problemas.

# DOCKER PRUNE - REMOVE TUDO DA MÁQUINA (CONTAINERS E IMAGENS)
``docker system prune -a``

# CRIANDO UMA REDE INTRANET PARA QUE OS CONTAINERS CONSIGAM ACESSAR UM AO OUTRO
O comando a seguir cria uma rede chamada **my-network**, do tipo **bridge**. Assim os containers conseguirão conversar entre si.
``docker network create --driver bridge my-network``

Para verificar a rede criada, use o comando a seguir. Ele exibirá dentre outras, a rede **my-network** que criamos:
``docker network ls``

# CRIANDO UM CONTAINER POSTGRES COM ACESSO À REDE BRIDGE CRIADA
``docker run --name my-postgres --network=my-network -p 5433:5432 -e POSTGRES_PASSWORD=postgres -d postgres``

No comando anterior damos o nome ao container de **my-postgres**, tendo este container a porta 5432, a qual é redirecionada para a porta 5433 do host. O atributo **-e** refere-se à **environment** e, através dele, conseguimos definir parâmetros tais como **POSTGRES_PASSWORD** e passar a ela a senha de acesso ao banco.

# ACESSANDO O POSTGRES DENTRO DO CONTAINER
[Como acessar o postgres de uma imagem docker?](https://pt.stackoverflow.com/questions/413924/como-acessar-o-postgres-de-uma-imagem-docker)
``docker exec -it my-postgres psql -U postgres``
Lembrando que o **my-postgres** é o nome que demos ao nosso container e **-U** é para informar qual o nome do usuário que definimos para acesso ao nosso Postgres.

# INSPECIONAR A REDE INTERNA QUE CRIAMOS NO DOCKER
Mostra o docker que temos inserido nesta rede: 
``docker inspect my-network``

# CRIANDO UM CONTAINER MY-PGADMIN COM ACESSO À REDE CRIADA
O e-mail pode ser qualquer um. A senha **postgres** não foi informada se precisa ser a mesma do postgres.
``docker run --name my-pgadmin --network=my-network -p 15432:80 -e PGADMIN_DEFAULT_EMAIL=UM_EMAIL_QQUER -e PGADMIN_DEFAULT_PASSWORD=postgres -d dpage/pgadmin4``

No nosso estudo, adotamos como e-mail no **PGADMIN_DEFAULT_EMAIl=gustavo@gmail.com**.

Imagem criada, é hora de acessar o container através do navegador do host. Para isto, basta colocar na URL: **localhost:15432**, sendo que definimos a porta 15432 enquanto estávamos configurando o container.
Assim, caindo na tela de login, devemos colocar o e-mail (*UM_EMAIL_QQUER*) e a senha (*postgres*) que definimos enquanto estávamos configurando o container.
Então, na tela de **Registro de Servidor** (em *Servers*, clique com o botão direito e selecione **Register->Server**), dentro do pgadmin, vamos passar as informações do container do postgres que criamos. Na aba **General**, no campo **Name**, damos o nome de **docker**. Então, na aba **Connection**, em **Host name/address**, inserimos o nome do container que criamos o postgres que, no caso, é: **my-postgres**. Após isto, a porta do postgres no nosso container **my-postgres** é a que definimos: **5432**. O *Maintenance database* é o **postgres**. O username é o username que foi criado automaticamente na criação do container: **postgres**. E a senha que definimos: **postgres**. Feito isto, podemos salvar.
A partir disto temos acesso ao nosso container **my-postgres** através do nosso **my-pgadmin**, no nosso navegador do host.

####
# VOLUMES TEMPORÁRIOS - ONDE ESTÃO
/var/lib/docker/volumes/7e42e13cda9b0998090be71349856ccc96b58e10648929bfbf26db626ac0eb08/_data# 

####

# PROBLEMAS COM O DOCKER
## ERRO 01 -
Ao tentar subir um arquivo **docker-compose.yml**, através do comando ``docker compose up -d``, obtivermos o erro abaixo:
> unable to get image 'bitnami/postgresql:latest': permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.49/images/bitnami/postgresql:latest/json": dial unix /var/run/docker.sock: connect: permission denied

## SOLUÇÃO PARA O ERRO 01:
[How to fix the permission denied error in Docker](https://www.hostinger.com/tutorials/how-to-fix-docker-permission-denied-error?utm_campaign=Generic-Tutorials-DSA|NT:Se|LO:BR-EN&utm_medium=ppc&gad_source=1&gad_campaignid=20990084344&gbraid=0AAAAADMy-hYbQhUOgYMJDy5G12m3tENrT&gclid=EAIaIQobChMIm4ra4uf7jQMV-19IAB3WKiJIEAAYASAAEgIWZvD_BwE)
O problema acima descrito ocorreu na utilização do terminal dentro do VSCode. Mesmo saindo do terminal, dando **exit**, encerrando a seção do usuário no Linux, abrindo o VSCode novamente, o erro persistia. Como solução foram redigitados novamente os comandos abaixo:
```
newgrp docker
groups
docker ps
```

O comando ``docker ps`` foi utilizado somente para verificar que o usuário non-root conseguia executar. Desta forma, foi obtido êxito.

####

# REFERÊNCIAS
> [Diolinux - O mínimo que você precisa saber sobre Docker!](https://www.youtube.com/watch?v=ntbpIfS44Gw)
> [Fernanda Kipper - APRENDA DOCKER DO ZERO | TUTORIAL COMPLETO COM DEPLOY](https://www.youtube.com/watch?v=DdoncfOdru8)
> [RockerSeat - A combinação que todo dev back-end precisa saber (Postgres + Docker)](https://www.youtube.com/watch?v=KlbL-8CEjN0&t=162s)
> [Prof. Diego Pinho - Postgres e PGAdmin diretamente pelo Docker](https://www.youtube.com/watch?v=CdoBvd_bPdk)
