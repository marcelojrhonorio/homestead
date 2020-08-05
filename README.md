# Configuração do ambiente local

Para configuração do ambiente local usaremos o Laravel Homestead, que nos provê toda configuração pronta de um servidor web. Você encontrará mais informações na [documentação oficial](https://laravel.com/docs/5.6/homestead).

### 1. Instale o Vagrant e o Virtual Box
Baixe a última versão do [Vagrant](https://www.vagrantup.com/downloads.html) e do [Virtual Box](https://www.virtualbox.org/wiki/Downloads). 

### 2. Gere uma chave SSH
Para acessar sua máquina virtual, você deverá configurar uma chave SSH. Para isso, digite do seu terminal o comando `ssh-keygen rsa`.

### 3. Clone o Laravel Homestead
Crie uma pasta para o Homestead e clone através do comando:

`git clone https://github.com/laravel/homestead.git`

Você pode também criar a pasta no momento que baixar o repositório, acrescentando o nome ao final do comando, exemplo:

`git clone https://github.com/laravel/homestead.git Homestead`.

### 4.  Mude para a última release estável

Entre no diretório do seu Homestead e digite o comando `git checkout v7.6.0`. Acesse a página [Homestead no Github](https://github.com/laravel/homestead/releases) para ver a última release disponível. Neste exemplo foi usada a v7.6.0.

Por fim, inicialize o Homestead digitando `init.sh` para Linux ou Mac ou `init.bat` para Windows.

### 5. Instale a box do Homestead

Ainda dentro do diretório do homestead, digite o comando `vagrant box add laravel/homestead`. Aparecerão as seguintes opções:

    1) hyperv
    2) parallels
    3) virtualbox
    4) vmware_desktop
Escolha virtualbox e prossiga. Demorará alguns minutos para concluir. Se tudo ocorreu bem, aparecerá uma mensagem na última linha similar a esta: 

    ==> Box: Successfully added box 'laravel/homestead' (v2.0.0) for 'virtualbox'!

Para confirmar se a box foi adicionada com sucesso, você pode também digitar o comando `vagrant box list`. Se tudo estiver ok, retornará uma mensagem similar a esta: 

    laravel homestead (virtualbox, 2.0.0)

### 6. Clone os projetos

Clone para sua pasta padrão em seu computador. Esta pasta será mapeada para o diretório padrão de projetos na máquina virtual do Homestead. 

⚠ Leia o arquivo README.md de cada projeto para fazer o setup inicial correto. 

### 7. Configure  o arquivo Homestead.yaml

Abra o arquivo *Homestead.yaml* dentro do diretório do Homestead. Este arquivo possui a seguinte estrutura: 

    ip: "192.168.10.10"
    memory: 2048
    cpus: 1
    provider: virtualbox

*Nessa parte, você configurará o IP que será vinculado ao endereço local que configuraremos posteriormente. Configure também a quantidade de memória e processadores que sua máquina virtual terá e escolha o software para virtualização. Nesse caso, estamos utilizando o Virtualbox*.


    authorize: C:/Users/marce/.ssh/id_rsa.pub

    keys:
       - C:/Users/marce/.ssh/id_rsa

*Caminho para as chaves SSH criadas no início*.


    folders:
      - map: C:/www/api
        to: /home/vagrant/Sites/api.sweetmedia.test
      - map: C:/www/dashboard
        to: /home/vagrant/Sites/sweetmedia.test

*Mapeamento das pastas do seu projeto para a pasta da máquina virtual*.



    sites:
      - map: api.sweetmedia.test
        to: /home/vagrant/Sites/api.sweetmedia.test/public
        schedule: true
      - map: sweetmedia.test
        to: /home/vagrant/Sites/sweetmedia.test/public
      - map: sweetbonus.test

*Mapeamento dos endereços locais para a pasta pública*.


    databases:
      - sweet
*Banco de dados do projeto*.


O arquivo final ficará da similar ao abaixo: 

    ip: "192.168.10.10"
    memory: 2048
    cpus: 1
    provider: virtualbox

    authorize: C:/Users/marce/.ssh/id_rsa.pub

    keys:
       - C:/Users/marce/.ssh/id_rsa
    
    folders:
      - map: C:/www/api
        to: /home/vagrant/Sites/api.sweetmedia.test
      - map: C:/www/dashboard
        to: /home/vagrant/Sites/sweetmedia.test
      - map: C:/www/sweetbonus
        to: /home/vagrant/Sites/sweetbonus.test
      - map: C:/www/store
        to: /home/vagrant/Sites/store.sweetbonus.test
      - map: C:/www/cristalclub
        to: /home/vagrant/Sites/cristalclub.test

    sites:
      - map: api.sweetmedia.test
        to: /home/vagrant/Sites/api.sweetmedia.test/public
        schedule: true
      - map: sweetmedia.test
        to: /home/vagrant/Sites/sweetmedia.test/public
      - map: sweetbonus.test
        to: /home/vagrant/Sites/sweetbonus.test/public
      - map: store.sweetbonus.test
        to: /home/vagrant/Sites/store.sweetbonus.test/public
      - map: cristalclub.test
        to: /home/vagrant/Sites/cristalclub.test/public

    databases:
      - sweet


### 8. Configure  o arquivo de hosts do seu sistema operacional

A configuração final ficará similar a esta: 

    192.168.10.10 api.sweetmedia.test
    192.168.10.10 sweetmedia.test
    192.168.10.10 sweetbonus.test
    192.168.10.10 store.sweetbonus.test

### 9. Iniciando a máquina virtual

Para isso, digite o comando `vagrant up`.

### 10. Restaure o banco de dados do projeto

Com o arquivo de backup do banco de dados, acesse sua máquina virtual via ssh. Para isso, entre no diretório do Homestead e digite o comando `vagrant ssh`. Acesse a API do projeto e digite o comando `php artisan migrate`. Em seguida, acesse o MySQL, selecione o banco de dados do projeto e rode o comando `source /home/vagrant/Sites/database/sweetbonus.sql`, onde você deverá ser especificado o path do arquivo de backup.

### 11. Digite o endereço no navegador

Finalmente, digite os endereços configurados no arquivo de hosts em seu navegador. 

### 12. Desligando a máquina virtual

Digite o comando `vagrant halt`.

### 13. Atualizando o arquivo Homestead.yaml

Você pode à qualquer momento atualizar as informações contidas no arquivo *Homestead.yaml*. Após a alteração, digite o comando `vagrant reload --provision` caso a máquina já esteja iniciada ou `vagrant up --provision` para inicializar a máquina virtual com as alterações.
