O CoreCTRL é uma aplicação open source que permite o controle de GPUs e CPUs AMD de forma bem similar ao Adrenaline para Windows.

Na sua [página no GitLab](https://gitlab.com/corectrl/corectrl), temos informações acerca da instalação e das configurações que devem ser feitas.

## Instalação

Para instalar em distribuições baseadas em Ubuntu, como o Pop!OS, primeiro temos que instalar uma PPA via terminal:

```
sudo add-apt-repository ppa:ernstp/mesarc && sudo apt update
```

Em seguida, podemos instalar:

```
sudo apt install corectrl
```

Com o aplicativo instalado, podemos realizar algumas configurações para i) iniciar com o boot do sistema; ii) não pedir senha de acesso e; iii) adicionar funções avanç.

Infelizmente, a [página de instruções para a configuração não é muito detalhada](https://gitlab.com/corectrl/corectrl/-/wikis/Setup), principalmente para sistemas que não utilizam o `grub` como meio de inicialização do sistema.

Entretanto, com a ajuda de alguns usuários do Reddit e com algumas pesquisas, consegui fazer funcionar.

## Iniciar com o sistema

Para iniciar o CoreCTRL com o sistema, precisamos adicionar a aplicação na pasta `~/.config/autostart/`:

```
cp /usr/share/applications/org.corectrl.corectrl.desktop ~/.config/autostart/org.corectrl.corectrl.desktop
```

Caso apareça algum erro de que este diretório não existe, basta digitar esta linha de código:

```
mkdir ~/.config/autostart/
```

E repetir o processo anterior.

## Não pedir senha

Instruções vistas [neste comentário do Reddit](https://www.reddit.com/r/pop_os/comments/kwpt94/how_to_set_up_corectrl_on_pop_os/) e [nessa postagem do blog Diolinux](https://diolinux.com.br/tutoriais/gpu-amd-corectrl-mais-poderoso.html).

### Checar versão do polkit

Primeiro, é preciso checar a versão do polkit do sistema:

```
pkaction --version
```

### Polkit < que 0.106

Caso a versão seja menor do que 0.106, é preciso seguir os seguintes passos:

Criar o arquivo `/etc/polkit-1/localauthority/50-local.d/90-corectrl.pkla` com o comando `sudo nano`:

```
sudo nano /etc/polkit-1/localauthority/50-local.d/90-corectrl.pkla
```

Depois, adicionar o seguinte conteúdo no arquivo:

```
[User permissions]
Identity=unix-group:your-user-group
Action=org.corectrl.*
ResultActive=yes
```

Assim que for adicionado, você precisa mudar onde está `your-user-group` para o nome do seu usuário, ou para `sudo`. Para fechar o arquivo, aperte Ctrl+X e aperte Y+Enter para salvar.

### Polkit >= que 0.106

Caso a versão seja menor do que 0.106, é preciso seguir os seguintes passos:

Criar o arquivo `/etc/polkit-1/rules.d/90-corectrl.rules` com o comando `sudo nano`:

```
sudo nano /etc/polkit-1/rules.d/90-corectrl.rules
```

Depois, adicionar o seguinte conteúdo no arquivo:

```
polkit.addRule(function(action, subject) {
    if ((action.id == "org.corectrl.helper.init" ||
         action.id == "org.corectrl.helperkiller.init") &&
        subject.local == true &&
        subject.active == true &&
        subject.isInGroup("your-user-group")) {
            return polkit.Result.YES;
    }
});
```

Assim que for adicionado, você precisa mudar onde está `your-user-group` para o nome do seu usuário, ou para `sudo`. Para fechar o arquivo, aperte Ctrl+X e aperte Y+Enter para salvar.

## Adicionar funções avançadas

Caso você utilize o `grub`, basta seguir as instruções na [página de configuração](https://gitlab.com/corectrl/corectrl/-/wikis/Setup) do CoreCTRL. Caso você tenha `systemd` como bootloader, é preciso mudar o seguinte arquivo:

```
boot/EFI/efi/loader/entries/Pop_OS-current.conf
```

Para acessar este arquivo, é preciso estar com acesso de root. Com isso, eu utilizei a seguinte opção conforme descrita [aqui](https://askubuntu.com/questions/650291/how-to-edit-boot-efi):

- Digite `sudo gnome-system-monitor` no terminal e abra a pasta a partir do sistema de arquivos.

Conforme [este comentário](https://www.reddit.com/r/pop_os/comments/irb1e9/how_to_setup_corectrl_on_pop_os/) do Reddit, é preciso adicionar o seguinte parâmetro no final da linha options: `amdgpu.ppfeaturemask=0xffffffff` e reiniciar o computador.
