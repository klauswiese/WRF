# Compilar WRF

Es una guía para compilar WRF ([The Weather Research and Forcasting](https://www.mmm.ucar.edu/weather-research-and-forecasting-model)) usando bash en Ubuntu 20.04. 

## 1. Compilador gfortran (gcc)

Como compilador usaremos gfortran ([gcc](https://gcc.gnu.org/wiki/GFortran#:~:text=Gfortran%20is%20the%20name%20of,GCC%2C%20the%20GNU%20Compiler%20Collection.)).

```console
#Confirmar si gfrotran esta instalado
gfortran -v
```
En caso de no tener instalado gfortran:

```console
sudo apt-get install gfortran
```

Adicionalmente se debe contar con [csh](https://www.mkssoftware.com/docs/man1/csh.1.asp), [perl](https://www.perl.org/) y [sh](https://en.wikipedia.org/wiki/Bourne_shell#:~:text=The%20Bourne%20shell%20(%20sh%20)%20is,interpreter%20for%20computer%20operating%20systems.&text=Unix%2Dlike%20systems%20continue%20to,are%20used%20by%20most%20users.)

```console
which perl
which csh
which sh
```

En caso de no tenerlos instalados:

```console
sudo apt-get install perl
sudo apt-get install csh
sudo apt-get install sh
```

## 2. Preparar espacio de trabajo

Vamos a hacer la compilación en la raíz de [Ubuntu 20.04](https://ubuntu.com/), aquí crearemos la carpeta WRF, dentro de esta carpeta crearemos dos carpetas más: 1) libs y 2) descargas. dentro de la carpeta libs crearemos tres carpetas más 1) grib2, 2) netcdf y 3) mpich.

Para ello abrimos la consola usando el comando Control + Alt + T y ejecutaremos los siguientes comandos: 

```console
#Cambiar directorio a la raiz (por si estabas en otro sitio), aunque la instalación puede 
#ser en cualquier directorio

cd

#Crear carpeta WRF

mkdir WRF

#Crear carpetas libs y descargas

mkdir WRF/libs
mkdir WRF/descargas

#Crear carpetas grib2, netcdf y mpich

mkdir WRF/libs/grib2
mkdir WRF/libs/netcdf
mkdir WRF/libs/mpich

```


## 3. Descarga de librerias, WRF y WPS

Las librerías se deben descargar y guardar en la carpeta WRF/descargas.

### 3.1 Dependencias 

Dependiendo del uso que se de a WRF se requiere la instalación de varias librerías, estas pueden ser descargadas de los siguientes hipervínculos:

- [zlib](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/zlib-1.2.7.tar.gz)
- [libpng](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/libpng-1.2.50.tar.gz)
- [jasper](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/jasper-1.900.1.tar.gz)
- [netcdf](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/netcdf-4.1.3.tar.gz)
- [mpich](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/mpich-3.0.4.tar.gz)

### 3.2 WRF y WPS

- [WRF](https://github.com/wrf-model/WRF/archive/refs/tags/v4.1.5.tar.gz)
- [WPS](https://github.com/wrf-model/WPS/archive/refs/tags/v4.1.tar.gz)

## 4. Compilar dependencias 

Tenemos 5 librerías de las cuales depende WRF. Lo primero que debemos hacer es descomprimir las librerías para luego compilarlas.

```console
#Cambiar al directorio descargas
cd ~/WRF/descargas

#Descomprimir todas las librerías
for i in *.gz ; do tar xzf $i; done
```

Antes de comenzar a compilar las librerías crearemos una variable que almacene la dirección donde ubicaremos las librerías:

```console
#Crear variable en la sesión
export LIBDIR=/home/usuario/WRF/libs

#Verificar creación de variable
echo $LIBDIR
```

### 4.1 Compilar zlib

La primera librería que necesitamos instalar es zlib, ya que libpng depende de esta. La compilación es sencilla, debemos movernos al directorio del instalador en la carpeta descargas, luego ejecutamos el archivo configure, ejecutar make y finalmente make install.

```console
#Cambiar al directorio de la librería
cd ~/WRF/descargas/zlib-1.2.7

#Ejecutar archivo configure y definir el directorio donde se instalará (grib2)
./configure --prefix=$LIBDIR/grib2
make
make install
```

### 4.2 Compilar libpng

La segunda librería que instalaremos es libpng, siguiendo los mismos pasos que ejecutamos para zlib.

```console
#Cambiar al directorio de la librería
cd ~/WRF/descargas/libpng-1.2.50

#Ejecutar archivo configure y definir el directorio donde se instalará (grib2)
./configure --prefix=$LIBDIR/grib2 LDFLAGS="-L$LIBDIR/grib2/lib" CRRFLAGS="-I$LIBDIR/grib2/include"
make
make install
```

### 4.3 Compilar jasper

La tercera librería que instalaremos es jasper, siguiendo los mismos pasos que ejecutamos para zlib y libpng.

```console
#Cambiar al directorio de la librería
cd ~/WRF/descargas/jasper-1.900.1

#Ejecutar archivo configure y definir el directorio donde se instalará (grib2)
./configure --prefix=$LIBDIR/grib2
make
make install
```

### 4.4 Compilar netcdf

La cuarta libreria que instalaremos es netcdf, siguiendo los mismos pasos que ejecutamos para zlib, libpng y jasper.

```console
#Cambiar al directorio de la librería
cd ~/WRF/descargas/netcdf-4.1.3

#Ejecutar archivo configure y definir el directorio donde se instalará (netcdf)
#también deshabilitamos dap y netcdf-4
./configure --prefix=$LIBDIR/netcdf --disable-dap --disable-netcdf-4
make
make install
make check
```

### 4.5 Compilar mpich

La quinta y última libreria que instalaremos es mpich, siguiendo los mismos pasos que ejecutamos para zlib, libpng, jasper y netcdf.

```console
#Cambiar al directorio de la librería
cd ~/WRF/descargas/mpich-3.0.4

#Ejecutar archivo configure y definir el directorio donde se instalará (mpich)
./configure --prefix=$LIBDIR/mpich
make
make install
```

## 5. Compilar WRF y WPS

Ubicaremos los instaladores de WRF-4.1.5 y WPS-4.1 en la carpeta WRF.

```console
#Cambiar al directorio de la librería
cd ~/WRF

#mover WRF
mv ~/WRF/descargas/WRF-4.1.5 ~/WRF/

#mover WPS
mv ~/WRF/descargas/WPS-4.1 ~/WRF/

```

### 5.1 Definir direcciones de cada librería instalada previamente

En este momento definiremos de manera provisional las direcciones, si funcionan, las definiremos de manera permanente en **.bashrc**.

```console
#netcdf
export NETCDF=$LIBDIR/netcdf
export LD_LIBRARY_PATH=$LIBDIR/netcdf/lib:$LD_LIBRARY_PATH

#mpich, poner en el path mpicc
export PATH=$LIBDIR/mpich/bin:$PATH

#jasper
export JASPERLIB=$LIBDIR/grib2/lib
export JASPERINC=4LIBDIR/grib2/include

#libpng
export LD_LIBRARY_PATH=$LIBDIR/grib2/lib:$LD_LIBRARY_PATH
```

### 5.2 Compilar WRF

Aquí tenemos tres archivos shell importantes:

- clean
- configure
- compile 

```console
#Cambiar al directorio de la librería
cd ~/WRF/WRF-4.1.5

#Ejecutar configure
./configure

```
Tras ejecutar este comando se dsplegará la lista de compiladores disponibles y lo alcances de cada uno, en nuestro caso usaremos GNU (gfortran/gcc), que en mi caso esta resumido en 4 opciones 32) serial 33) smpar 34) dmpar y 35) dm + sm.  Seleccionaremos el número **34** - **dmpar** que nos permitirá usar varios procesadores de nuestro ordenador y luego seleccionaremos la opción básica del **Nesting** digitando **1**.

```console
#Ejecutar compile
./compile em_real

```

### 5.2 Compilar WPS

```console
#Cambiar al directorio de la librería
cd ~/WRF/WPS-4.1

#Definir la ubicación de WRF
export WRF_DIR=/home/usuario/WRF/WRF-4.1.5

#Ejecutar configure
./configure

```

Tras este paso seleccionares l aopción **dmpar** en mi caso el número **3**.

```console
#Ejecutar compile
./compile
```

## 6. Probar instalación de WRF y WPS

### 6.1 WRF
Usaremos si logramos instalar WRF probando el archivo **wrf.exe**.
```console
#Cambiar al diectorio de wrt.exe
cd ~/WRF/WRF-4.1.5/main

#Ejecutar archivo
./wrf.exe
```
Como resultado tendremos: * starting wrf task            0  of            1*, esto porque aún no configuramos datos ni tareas que ejecutar

### 6.2 WPS

```console
#Cambiar al diectorio de wrt.exe
cd ~/WRF/WPS-4.1

#Ejecutar archivos geogrid.exe, metgrid.exe y ungrib.exe
./geogrid.exe
./metgrid.exe 
./ungrib.exe
```

