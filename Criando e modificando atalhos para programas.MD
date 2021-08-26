# Criando ou modificando arquivos de desktop

Para criar/modificar os atalhos nos programas (e suas integrações com o sistema), basta ir na seguinte pasta:

```
/usr/share/applications/
```

Ela contém todos os atalhos do sistema dos programas existentes. Para modificá-los, utilize o seguinte comando:

```
sudo nano /usr/share/applications/<nome_do_programa>
```

Há algumas variações, mas no geral, eles se assemelham ao seguinte formato (estou utilizando como demonstração uma configuração do GOverlay, que foi me passada [nesta](https://github.com/benjamimgois/goverlay/issues/139) "issue" que tive no repositório deles no GitHub):

```
[Desktop Entry]
Name=GOverlay
Comment=Graphical UI to help manage Vulkan / OpenGL overlays
Exec=mangohud --dlsym goverlay --style fusion
Icon=goverlay
Terminal=false
Type=Application
Categories=Game;
Keywords=MangoHud;vkBasalt;ReplaySorcery;afterburner
```

No caso, eu criei um para o GOverlay. O exemplo acima foi levemente modificado.

1) Digitei o mesmo comando com o `nano`:

```
sudo nano /usr/share/applications/io.github.benjamimgois.goverlay.desktop
```

2) Depois modifiquei a linha `Exec`e a linha `Icon` para corresponderem ao diretório onde estão meus arquivos:

```
[Desktop Entry]
Name=GOverlay
Comment=Graphical UI to help manage Vulkan / OpenGL overlays
Exec=mangohud --dlsym /home/ghost/goverlay/goverlay --style fusion
Icon=/home/ghost/goverlay/goverlay3t256.png
Terminal=false
Type=Application
Categories=Game;
Keywords=MangoHud;vkBasalt;ReplaySorcery;afterburner
```

# Excluindo arquivos

Em relação à programas que não "somem" do sistema após a desisntalação, com o ícone ainda persistindo, basta excluir o arquivo correspondente. Isso já aconteceu comigo com um jogo que instalei.
