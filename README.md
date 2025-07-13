# Laboratorio 1 Compiladores

## Descripción

Este laboratorio consiste en trabajar con ANTLR para generar un analizador léxico y sintáctico en Python a partir de una gramática definida en `MiniLang.g4`. El entorno está configurado mediante Docker para facilitar la construcción y ejecución del contenedor con todas las dependencias necesarias.

Dentro del contenedor, se monta el directorio `program/` que contiene:

* La gramática ANTLR (`MiniLang.g4`).
* El archivo principal de ejecución (`Driver.py`).
* Un archivo de prueba con código fuente de ejemplo (`program_test.txt`).



## Estructura del proyecto

```
/
├── Dockerfile
├── program/
│   ├── MiniLang.g4
│   ├── Driver.py
│   └── program_test.txt
├── requirements.txt
└── README.md
```


## Requisitos

* Docker instalado en tu máquina.
* Conexión a internet para descargar imágenes y dependencias (solo la primera vez).



## Construir y ejecutar el contenedor

Desde la raíz del proyecto, ejecuta:

```bash
docker build --rm . -t lab1-image
docker run --rm -ti -v "$(pwd)/program":/program lab1-image
```

Esto construye la imagen Docker y abre un contenedor interactivo con el directorio `program` montado en `/program`.


## Generar archivos Lexer y Parser

Dentro del contenedor, ubícate en `/program` y ejecuta:

```bash
cd /program
java -jar /usr/local/lib/antlr-4.13.1-complete.jar -Dlanguage=Python3 MiniLang.g4
```

Esto genera los archivos Python para el lexer y parser basados en la gramática.


## Ejecutar el analizador

Para analizar el archivo de prueba:

```bash
python3 Driver.py program_test.txt
```

* Si el archivo es sintácticamente correcto, **no mostrará ningún mensaje**.
* Si hay errores de sintaxis, ANTLR los reportará en la consola.


## Breve análisis de la gramática y Driver.py

* **Gramática ANTLR (`MiniLang.g4`):** Define los tokens y reglas sintácticas del lenguaje MiniLang.

  * Los **lexer rules** reconocen los elementos básicos (números, operadores, identificadores).
  * Las **parser rules** estructuran cómo se combinan esos tokens para formar expresiones y programas.
  * El símbolo `#` se usa para nombrar alternativas en reglas, facilitando la generación de código en Visitors o Listeners.

* **Driver.py:**

  * Lee el archivo de entrada (`program_test.txt`).
  * Usa el lexer para tokenizar el texto.
  * Usa el parser para validar la estructura sintáctica.
  * Maneja y muestra errores de sintaxis si los hay.
  * En este laboratorio, no imprime nada si el análisis es correcto, solo muestra errores si ocurren.


## Uso recomendado

* Modifica `program_test.txt` para probar distintos fragmentos de código MiniLang.
* Prueba errores sintácticos para verificar la detección de errores.
* Extiende el proyecto implementando Visitors o Listeners para análisis semántico (pasos futuros del curso).


## Video 



