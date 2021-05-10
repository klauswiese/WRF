# Compilar WRF

Es una guia para compilar WRF ([The Weather Research and Forcasting](https://www.mmm.ucar.edu/weather-research-and-forecasting-model)) usando bash en Ubuntu 20.04. 

## Compilado gfortran (gcc)

Como compilador usaremos gfortran ([gcc](https://gcc.gnu.org/wiki/GFortran#:~:text=Gfortran%20is%20the%20name%20of,GCC%2C%20the%20GNU%20Compiler%20Collection.)).

```console
#Confirmar si gfrotran esta instalado
gfortran -v
```
En caso de no tener instalado gfortran:

```console
sudo apt-get install gfortran
```
