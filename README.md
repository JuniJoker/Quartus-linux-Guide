#Quartus 

##Instalação Quartus e modelsim

Esse tutorial tem intuito de ajudar usuarios linux a instalar o programa quartus e modelsim.
Para usuarios windows, a instalação é feita normalmente e bem intuitiva.

###Obtenha o quartus

É possivel fazer o download direto no site da [Altera](http://dl.altera.com/?edition=lite).
O download pode ser feito em partes ou em um arquivo inteiro.
Esse tutorial foi criado e testado na versão 16.0, outras versoes podem geral erros de incompatibilidade com as lib's.

###Instalar dependencias

O software principal Quartus Prime é de 64 bits, um monte de ferramentas Altera enviados com Quartus Prime ainda são software de 32 bits. É por isso que nós precisamos instalar lotes de bibliotecas `lib32-` e outros programas. 

Se voce usa derivados de arch-linux, é necessario habilitar `multilib` editando o arquivo `/etc/pacman.conf`.

Os pacotes nativos necessarios sao  `expat fontconfig freetype2 xorg-fonts-type1 glibc gtk2 libcanberra libpng libpng12 libice libsm util-linux ncurses tcl tcllib zlib libx11 libxau libxdmcp libxext libxft libxrender libxt libxtst`.

Multiliib versão `lib32-expat lib32-fontconfig lib32-freetype2 lib32-glibc lib32-gtk2 lib32-libcanberra lib32-libpng lib32-libpng12 lib32-libice lib32-libsm lib32-util-linux lib32-ncurses lib32-zlib lib32-libx11 lib32-libxau lib32-libxdmcp lib32-libxext lib32-libxft lib32-libxrender lib32-libxt lib32-libxtst`

No arch-linux é possivel encontrar facilmente as lib's usando o comando `pacman` ou `yaourt`.

###Instalação

Depois de ter baixado o arquivo completo `Quartus-lite-16.0.0.211-linux.tar`, extraia o arquivo.
```tar -zxvf Quartus-lite-16.0.0.211-linux.tar```
Obtendo algo assim.
```
	setup.sh
	/componets
		arria_lite-16.0.0.211.qdz
		cyclonev-16.0.0.211.qdz  
		max-16.0.0.211.qdz
		QuartusHelpSetup-16.0.0.211-linux.run
		cyclone-16.0.0.211.qdz
		max10-16.0.0.211.qdz
		ModelSimSetup-16.0.0.211-linux.run
		QuartusLiteSetup-16.0.0.211-linux.run
```

O `setup.sh` precisa ter permisao de execução.

```
$ sudo chmod +x setup.sh
```
execute a instalação e siga os passos do assistente.

```
# ./setup.sh
```
Use o comando acima para iniciar a instalação.

a instalação padrao tentara instalar em `/root/altera/xx.x/`, recomenda-se que seja instalado em `/opt/altera/xx.x/`.

###Executando Quartus

Assumindo que o quartus foi instalado em `/opt/altera/xx.x`, seu binario estara localizado em `/opt/altera/15.1/quartus/bin`. Rode quartus (64bits versão).
```
$ /opt/altera/xx.x/quartus/bin/quartus --64bit
```
ou 32 bits versão:
```
$ /opt/altera/15.1/quartus/bin/quartus
```

###Integrando Quartus com o Linux

Tentando digitar `$ quartus` nota-se que o comando nao existe, pode se criar o comando criando um arquivo `quartus.sh` dentro da pasta `/etc/profile.d/`.

```
/etc/profile.d/quartus.sh
...............................................
export PATH=$PATH:/opt/altera/xx.x/quartus/bin
```
O arquivo precisa ser executavel
```
# chmod +x /etc/profile.d/quartus.sh
```
Os script's inseridos em `/etc/profile.d` na mesma sessão nao funcionam sem reeniciar o sistema, paranao precisar reeniciar pode-se executar o comando.
```
$ source /etc/profile.d/quartus.sh
```

###Atalho application

Tambem pode se criar um atalho na pasta application. Crie um arquivo `quartus.desktop` dentro da pasta `~/.local/share/applications`.
```
~/.local/share/applications/quartus.desktop
...............................................
[Desktop Entry]
Version=1.0
Name=Quartus Prime Standard Edition v15.1
Comment=Quartus Prime design software for Altera FPGA's
Exec=/opt/altera/15.1/quartus/bin/quartus --64bit
Icon=/opt/altera/15.1/quartus/adm/quartusii.png
Terminal=false
Type=Application
Categories=Development
```

##Modelsim-Altera Edition

###intall

A instalação é feita junto com o quartus se voce baixou a versão completa. Caso tenha baixado em partes, é preciso dar permissão de execução e inciar a instalação.
```
# ./modelsim.xx.x.run
```
Se o diretorio de instalação foi o `/opt`, entao o modelsim foi instalado em `/opt/altera/15.1/modelsim_ase`.

###Compatibilidade

Em algumas distros, ao iniciar modelsim com o comando `$ vsim`, gera o erro descrito abaixo.
```
$ vsim
Error: cannot find "/opt/altera/xx.x/modelsim_ase/bin/../linux_rh60/vsim"
```
A soluçao é bem simples, edite o arquivo `vco` no diretorio `/opt/altera/xx.x/modelsim_ase/` como é sugerido.
```
/opt/altera/xx.x/modelsim_ae/vco line 206
 *)                vco="linux_rh60" ;;
```
para
```
/opt/altera/xx.x/modelsim_ae/vco line 206
 *)                vco="linux" ;;
```
Pode ser necessario permisao de escrita.

### freetype2 2.5.0.1-1

Ao atualizar o `freetype2` pode gerar erro na inicialização do modelsim.
```
$ vsim
Error in startup script:
Initialization problem, exiting.
Initialization problem, exiting.
Initialization problem, exiting.
   while executing
"EnvHistory::Reset"
   (procedure "PropertiesInit" line 3)
   invoked from within
"PropertiesInit"
   invoked from within
"ncFyP12 -+"
   (file "/opt/questasim/linux_x86_64/../tcl/vsim/vsim" line 1)
** Fatal: Read failure in vlm process (0,0)
```

