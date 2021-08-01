Há dois tipos de drivers para CPUs INTEL: `intel_pstate` e `acpi-cpufreq`.

O `acpi-cpufreq` permite uma melhor escala do uso da CPU a partir do que está sendo usado. Com isso, eu desabilitei o `intel_pstate` e ativei o `acpi-cpufreq`. 

Para isso, eu utilizei o seguinte [guia](https://silvae86.github.io/2020/06/13/switching-to-acpi-power/). O problema é que eu utilizo o Pop_OS, que tem como administrador do `boot`o `systemd`, em vez do `grub`, que é mostrado no guia.

Com isso, eu segui os mesmos passos que ele, mas substitui o final. Vamos aos passos:

1)  Vamos analisar como está a frequência da nossa GPU com o seguinte código:

```
watch grep \"cpu MHz\" /proc/cpuinfo
```

2) Vamos mudar para o acpi_cpufreq:

```
sudo apt update
sudo apt install acpi-support acpid acpi
```

3) Agora vem a parte diferente do guia. Eu utilizei a seguinte [postagem](https://www.reddit.com/r/pop_os/comments/m71hai/systemdboot_commands/) no reddit. Nela, houve um comentário com as instruções no caso de um sistema com o `systemd`:

```
sudo kernelstub --add-options "intel_idle.max_cstate=0 processor.max_cstate=0 idle=poll intel_pstate=disable"
```

Pronto, acabei de substituir o driver. Basta reiniciar que já estará funcionando!

## Bônus

Há um programa que faz a mudança das frequências de forma automática em relação à demanda:  o [auto-cpufreq](https://github.com/AdnanHodzic/auto-cpufreq).

Basta instalar que ele já funcionará automaticamente, além de funcionar junto com o TLP!

PS: eu havia instalado com o `intel_pstate` mas não estava funcionando corretamente. Com o `acpi-cpufreq` eu não tive mais problemas.

## Bônus II

Uma ferramenta muito boa para checar as inforações da CPU é o `cpufreq-info`. Basta instalar via terminal:

```
sudo apt install cpufreq-info
```

A documentação do pacote pode ser encontrada [aqui](https://linux.die.net/man/1/cpufreq-info)
