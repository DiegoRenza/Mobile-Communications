# End 2 End LTE Network Simulation
Este tutorial muestra de manera simplificada la implementación de una red LTE en un solo computador, utilizando el proyecto SrsRAN (https://www.srslte.com/). Esta solución constituye un primer acercamiento a un esquema de radio definidio por software en donde no es necesario contra con hardware (como por ejemplo una USRP), ni tampoco es necesario instalar su driver (UHD). 

En este sentido, sólo será necesario realizar una compilación desde los archivos fuente de la herramienta. 

Instalación
=============

Operating System
-------------
La red completa se ejecutará sobre una máquina con sistema operativo Linux. Para una versión ligera, se recomienda decargar e instalar LinuxMint: Las siguienets indicaciones de instalación han sido implementadas utilizando la versión LinuxMint 20.2 Xfce de 64 bits disponible en https://linuxmint.com/download.php. 

Esta instalación se realizó mediante una máquina virtual instalada mediante Virtual Box (https://www.virtualbox.org/), asignando un tamaño de disco dinámico y 2 GB de memoria RAM. 

Recursos en VirtualBox
![](https://github.com/DiegoRenza/Mobile-Communications/blob/main/LinuxMint_Resources.png)

Versión del sistema operativo
![](https://github.com/DiegoRenza/Mobile-Communications/blob/main/LinuxMint.png)

Instalación de librerías y paquetes para SrsRAN
-------------

Abrir una venta de terminal en Linux Mint, y ejecutar lo siguiente:

Instalación de librerías
```python 
sudo apt-get install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev
libconfig++-dev libsctp-dev
```

Instalación de Git
```python 
sudo apt install git
```

Descargar y compilar SrsRAN.
```python 
git clone https://github.com/srsRAN/srsRAN.git
```

El comando make de la última línea tardará alrededor de unos 8 minutos
```python 
cd srsRAN
mkdir build
cd build
cmake ../
make
```

Comprobación. El comando make test tardará alrededor de unos 7 minutos
```python 
make test
```

Instalar SrsRAN
```python 
sudo make install
sudo srsran_install_configs.sh service
```

Instalar el driver ZeroMQ
-------------

Instalar libtool (ejecutar desde la carpeta raíz):

```python 
sudo apt-get update
sudo apt-get install libtool 
```

Instalar libzmq
```python 
git clone https://github.com/zeromq/libzmq.git
cd libzmq
./autogen.sh
./configure
make
sudo make install
sudo ldconfig
```

Instalar czmq (instalar desde carpeta raíz)
```python
git clone https://github.com/zeromq/czmq.git
cd czmq
./autogen.sh
./configure
make
sudo make install
sudo ldconfig
```

Compilar SrsRAN (desde la carpeta srsRAN/build)
```python
cmake ../
make
```

Es necesario verificar que la salida de la compilación incluya la detección del driver en las siguientes líneas:
```python
...
-- FINDING ZEROMQ.
-- Checking for module 'ZeroMQ'
-- No package 'ZeroMQ' found
-- Found libZEROMQ: /usr/local/include, /usr/local/lib/libzmq.so
...
```
