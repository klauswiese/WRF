# Compilar WRF

Es una guia para compilar WRF ([The Weather Research and Forcasting](https://www.mmm.ucar.edu/weather-research-and-forecasting-model)) usando bash en Ubuntu 20.04. 

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

Vamos a hacer la compilación en la raiz de [Ubuntu 20.04](https://ubuntu.com/), aquí crearemos la carpeta WRF, dentro de esta carpeta crearemos dos carpetas más: 1) libs y 2) descargas. dentro de la carpeta libs crearemos tres carpetas más 1) grib2, 2) netcdf y 3) mpich.

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

Dependiendo del uso que se de a WRF se requiere la instalación de varias librerias, estas pueden ser descargadas de los siguientes hipervinculos:

- [zlib](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/zlib-1.2.7.tar.gz)
- [libpng](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/libpng-1.2.50.tar.gz)
- [jasper](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/jasper-1.900.1.tar.gz)
- [netcdf](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/netcdf-4.1.3.tar.gz)
- [mpich](https://www2.mmm.ucar.edu/wrf/OnLineTutorial/compile_tutorial/tar_files/mpich-3.0.4.tar.gz)

### 3.2 WRF y WPS

- [WRF](https://github.com/wrf-model/WRF/archive/refs/tags/v4.1.5.tar.gz)
- [WPS](https://github.com/wrf-model/WPS/archive/refs/tags/v4.1.tar.gz)
