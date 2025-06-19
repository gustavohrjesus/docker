#INSTALANDO DOCKER NO LINUX CORP
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

#GERENCIAR DOCKER COM UM USUÁRIO NON-ROOT
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

#COMANDOS DO DOCKER
O comando abaixo lista somente os containers rodando
``docker ps``

Para ver todos os containers (em execução e executados), use o comando acima com a flag **-all** (or **-a**)
``docker ps -a``

Os comandos acima evidenciam o id do container, a imagem, entre outras informações.

#SUBINDO UM DOCKER UBUNTU
``docker run ubuntu``
Após rodar o coma
ndo, verificamos que é baixada a imagem do docker ubuntu mas, após isso, é fechada a conexão. Verificação esta é feita com o comando **docker ps**. Isto acontece pois não existe nenhum processo em execução dentro deste ubuntu. Então, para deixar o docker ubuntu UP, usamos o comando **-it** para iteragir com o docker e também passamos o termo **bash** para que o processo bash fique em execução dentro deste ubuntu.
``docker run -it ubuntu bash``

Verifique que após rodar o comando acima, estaremos logados no terminal do próprio docker (Ubuntu) que subimos (Verifique que o nome do computador, que fica após o root@ não é o mesmo nome do seu computador).
De forma a comprovarmos que quando pararmos nosso docker ubuntu e subí-lo novamente, criaremos um diretório **new_dic** dentro da **home** deste nosso Ubuntu.
``mkdir new_dic``

Deixamos esta imagem rodando em um terminal e em outro, rodamos o comando ``docker ps`` para evidenciar que essa nossa imagem Ubuntu encontra-se em execução.
Verificamos que quando rodamos o comando ``docker ps``, em **status** temos a informação **Up <qtdTempExecut>**.

#PARANDO UM DOCKER
De forma a exemplificar, ainda continuaremos utilizando a imagem do Ubuntu.
Assim, quando rodamos o comando ``docker ps``, este comando exibe dentre as informações, o id deste docker (no nosso exemplo, o id é "ebcbfe70cc0c"). Então, para pararmos este docker (para tudo, inclusive os processos que estão rodando neste container), rodamos o comando abaixo:
``docker stop ebcbfe70cc0c``
Feito isto, no terminal no qual o docker estava rodando, verificamos que saímos do terminal do docker e voltamos para o terminal da nossa máquina.

#SUBINDO O NOSSO DOCKER UBUNTU NOVAMENTE
Para isto, rodamos o comando abaixo, apontando o id do nosso container:
``docker start ebcbfe70cc0c``
Assim, rodando o comando ``docker ps``, verificamos que o nosso container está em execução. Porem, não conseguimos interagir com ele.
Para que consigamos interagir com ele, rodamos o comando abaixo, com a flag **exec**, para executando um comando dentro daquele container, informando que queremos interagir com ele através da flag **-iti**, informando o id do container e com o que gostaríamos de iteragir que, nosso caso, seria o **bash**.
``docker exec -iti ebcbfe70cc0c bash``
Fazendo isto, podemos verificar que voltamos para o **bash** do nosso container. Podemos verificar que é realmente o nosso docker, acessando o diretório **home** do nosso container e listando (comando **ls**) os diretórios dentro dele.

####

#VERIFICANDO A VERSÃO DO DOCKER
``docker -v``

####

#PROBLEMAS COM O DOCKER
##ERRO 01 -
Ao tentar subir um arquivo **docker-compose.yml**, através do comando ``docker compose up -d``, obtivermos o erro abaixo:
> unable to get image 'bitnami/postgresql:latest': permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.49/images/bitnami/postgresql:latest/json": dial unix /var/run/docker.sock: connect: permission denied

##SOLUÇÃO PARA O ERRO 01:
[How to fix the permission denied error in Docker](https://www.hostinger.com/tutorials/how-to-fix-docker-permission-denied-error?utm_campaign=Generic-Tutorials-DSA|NT:Se|LO:BR-EN&utm_medium=ppc&gad_source=1&gad_campaignid=20990084344&gbraid=0AAAAADMy-hYbQhUOgYMJDy5G12m3tENrT&gclid=EAIaIQobChMIm4ra4uf7jQMV-19IAB3WKiJIEAAYASAAEgIWZvD_BwE)
O problema acima descrito ocorreu na utilização do terminal dentro do VSCode. Mesmo saindo do terminal, dando **exit**, encerrando a seção do usuário no Linux, abrindo o VSCode novamente, o erro persistia. Como solução foram redigitados novamente os comandos abaixo:
```
newgrp docker
groups
docker ps
```

O comando ``docker ps`` foi utilizado somente para verificar que o usuário non-root conseguia executar. Desta forma, foi obtido êxito.
