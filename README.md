# PETRA
## transform
Transform crea los cortes (axial,coronal y sagital) de una imagen biomédica neuronal de diferentes formatos (.nii,.hdr/.img ó .dcm). Tiene dos argumentos de entrada, la ruta donde se ubica la imagen y la modalidad de la imagen, puesto que las imágenes tipo 101 generan los cortes de forma diferente al resto. Se generarán los cortes en una carpeta dentro de esa ruta (bajo el mismo nombre que la imagen) que contiene otras tres carpetas con cada corte. Está formada por dos funciones: convert (el principal) y saveThumbnail. 

### Argumentos
1. @arg1: str que contiene la ruta donde se ubica la imagen. Se obtiene como sys.argv[1].
2. @arg2: entero con la modalidad de la imagen (ej: 101,301,...). Se obtiene como sys.argv[2].

### Funciones
* convert (argv,modalida): Se encarga de separar el nombre y la extensión de la imagen. Carga la imagen según la extensión (aunque este script no soportará ficheros DICOM) y se llama al método saveThumnail. 

* saveThumbnail (arv,Filename,img,modalidad): se crean los directorios correspondientes a los cortes en el caso de que no existan ya. Se cargan los datos de la imagen (img). Según la modalidad y dimensión de img (puede ser 3 ó 4) se generan archivos (con la librería scipy) en .jpg dentro de su carpeta correspondiente. El formato en el que se nombran las imagenes es: str(i) + '-' + Filename.  

## montage
Montage crea un montaje en .jpg, a partir de las imágenes de los cortes. Tiene la opción de introducir 2 o 4 parámetros de entrada: La ruta de la carpeta donde se encuentran las imágenes de los cortes, cuyo formato de nombre será el creado en transform.py (str(i) + '-' + Filename) y el nombre del montaje. Adicionalmente se podrá introducir el número de imágenes que aparezcan en las filas y en las columnas del montaje. Está formado por 3 funciones: generate_montage (el principal), get_number y ordenaBurbuja. 

### Argumentos
1. @arg1: str con la ruta de la carpeta de cortes. En el main, se llama a la función glob.glob(ruta). Se obtiene como sys.argv[1].
2. @arg2: ruta donde se guardará el montaje con el siguiente formato: ruta_imagen/nombre_imagen/montage/nombre_montaje.jpg. El nombre del montaje suele ser el corte. Se obtiene como sys.argv[2].
3. @arg3: (opcional) entero con el número de filas del montaje. Se obtiene como sys.argv[3].
4. @arg4: (opcional) entero con el número de columnas del montaje. Se obtiene como sys.argv[4].

### Funciones
* generate_montage(filenames, output_fn, row_size = None, col_size = None): Primero, se ordenan los ficheros filenames según el número de corte, llamando a la función get_number y después a ordenaBurbuja. Se cargan las imagenes con una lista. En caso de que no se definan row_size y col_size, se calcula row_size como el entero más próximo a la raíz cuadrada del numero de filenames, para crear un montaje cuadrado. A partir de este valor, se obtiene el de col_size. En caso de que col_size sea superior a max_colsixe (definido como 10), se recalcularán los valores. Todo ello es para que no se obtenga un montaje que se salga. Se calculan los parámetro de ancho y alto del montaje y con un bucle for se va rellenando el montaje con las imágenes del corte, con la ayuda de la variable offset_x y offset_y. Se tiene la opción de definir un espacio entre cada imagen, con la variable margin.

## montage_inicio_fin
Montage crea un montaje en .jpg, a partir de las imágenes de los cortes. Tiene la opción de introducir 4 o 6 parámetros de entrada: La ruta de la carpeta donde se encuentran las imágenes de los cortes, cuyo formato de nombre será el creado en transform.py (str(i) + '-' + Filename) y el nombre del montaje, así como el inicio y el final del montaje (deben de ser enteros que estén en el rango de numero de imágenes del corte). Adicionalmente se podrá introducir el número de imágenes que aparezcan en las filas y en las columnas del montaje. Está formado por 3 funciones: generate_montage (el principal), get_number y ordenaBurbuja. 

### Argumentos
1. @arg1: str con la ruta de la carpeta de cortes. En el main, se llama a la función glob.glob(ruta). Se obtiene como sys.argv[1].
2. @arg2: ruta donde se guardará el montaje con el siguiente formato: ruta_imagen/nombre_imagen/montage/nombre_montaje.jpg. El nombre del montaje suele ser el corte. Se obtiene como sys.argv[2].
3. @arg3: (opcional) entero con el número de filas del montaje. Se obtiene como sys.argv[3].
4. @arg4: (opcional) entero con el número de columnas del montaje. Se obtiene como sys.argv[4].

### Funciones
* generate_montage(filenames, output_fn, row_size = None, col_size = None): Primero, se ordenan los ficheros filenames según el número de corte, llamando a la función get_number y después a ordenaBurbuja. Se cargan las imagenes con una lista, según el inicio y fin. En caso de que no se definan row_size y col_size, se calcula row_size como el entero más próximo a la raíz cuadrada del numero de filenames, para crear un montaje cuadrado. A partir de este valor, se obtiene el de col_size. En caso de que col_size sea superior a max_colsixe (definido como 10), se recalcularán los valores. Todo ello es para que no se obtenga un montaje que se salga. Se calculan los parámetro de ancho y alto del montaje y con un bucle for se va rellenando el montaje con las imágenes del corte, con la ayuda de la variable offset_x y offset_y. Se tiene la opción de definir un espacio entre cada imagen, con la variable margin.

* get_number(filenames): se obtiene el número de cada corte a partir del array filenames, pues cada elemento de filenames es una ruta cuyo nombre del corte tiene el formato: str(i) + '-' + Filename. El objetivo es conseguir un array de enteros que obtenga i, que se usará para ordenar filenames según este número de menor a mayor. 

* ordenaBurbuja(lista,aux): ordena por el método de burbuja, los filenames de menor a mayor. Para ello, con la función get_number se obtiene el número del corte de todas las imágenes en un vector auxiliar (aux) que se usará para ordenar filenames (lista).

## montage_inicio_fin_salto
Montage crea un montaje en .jpg, a partir de las imágenes de los cortes. Tiene la opción de introducir 5 o 7 parámetros de entrada: La ruta de la carpeta donde se encuentran las imágenes de los cortes, cuyo formato de nombre será el creado en transform.py (str(i) + '-' + Filename) y el nombre del montaje, así como el inicio, fin y salto de imágenes del corte en el montaje. Adicionalmente se podrá introducir el número de imágenes que aparezcan en las filas y en las columnas del montaje. Está formado por 3 funciones: generate_montage (el principal), get_number y ordenaBurbuja. 

### Argumentos
1. @arg1: str con la ruta de la carpeta de cortes. En el main, se llama a la función glob.glob(ruta). Se obtiene como sys.argv[1].
2. @arg2: ruta donde se guardará el montaje con el siguiente formato: ruta_imagen/nombre_imagen/montage/nombre_montaje.jpg. El nombre del montaje suele ser el corte. Se obtiene como sys.argv[2].
3. @arg3: entero que indica el inicio del montaje.
4. @arg4: entero que indica el final del montaje.
5. @arg5: entero para indicar el número de saltos de imágenes del corte al hacer el montaje.
6. @arg6: (opcional) entero con el número de filas del montaje. Se obtiene como sys.argv[3].
7. @arg7: (opcional) entero con el número de columnas del montaje. Se obtiene como sys.argv[4].

### Funciones
* generate_montage(filenames, output_fn, row_size = None, col_size = None): Primero, se ordenan los ficheros filenames según el número de corte, llamando a la función get_number y después a ordenaBurbuja. Se cargan las imagenes con una lista, según el número de inicio, fin y salto. En caso de que no se definan row_size y col_size, se calcula row_size como el entero más próximo a la raíz cuadrada del numero de filenames, para crear un montaje cuadrado. A partir de este valor, se obtiene el de col_size. En caso de que col_size sea superior a max_colsixe (definido como 10), se recalcularán los valores. Todo ello es para que no se obtenga un montaje que se salga. Se calculan los parámetro de ancho y alto del montaje y con un bucle for se va rellenando el montaje con las imágenes del corte, con la ayuda de la variable offset_x y offset_y. Se tiene la opción de definir un espacio entre cada imagen, con la variable margin.

* get_number(filenames): se obtiene el número de cada corte a partir del array filenames, pues cada elemento de filenames es una ruta cuyo nombre del corte tiene el formato: str(i) + '-' + Filename. El objetivo es conseguir un array de enteros que obtenga i, que se usará para ordenar filenames según este número de menor a mayor. 

* ordenaBurbuja(lista,aux): ordena por el método de burbuja, los filenames de menor a mayor. Para ello, con la función get_number se obtiene el número del corte de todas las imágenes en un vector auxiliar (aux) que se usará para ordenar filenames (lista).

## montage_salto
Montage crea un montaje en .jpg, a partir de las imágenes de los cortes. Tiene la opción de introducir 3 o 5 parámetros de entrada: La ruta de la carpeta donde se encuentran las imágenes de los cortes, cuyo formato de nombre será el creado en transform.py (str(i) + '-' + Filename) y el nombre del montaje, además del número de saltos de imágenes del corte. Adicionalmente se podrá introducir el número de imágenes que aparezcan en las filas y en las columnas del montaje. Está formado por 3 funciones: generate_montage (el principal), get_number y ordenaBurbuja. 

### Argumentos
1. @arg1: str con la ruta de la carpeta de cortes. En el main, se llama a la función glob.glob(ruta). Se obtiene como sys.argv[1].
2. @arg2: ruta donde se guardará el montaje con el siguiente formato: ruta_imagen/nombre_imagen/montage/nombre_montaje.jpg. El nombre del montaje suele ser el corte. Se obtiene como sys.argv[2].
3. @arg3: entero para indicar el número de saltos de imágenes del corte al hacer el montaje.
4. @arg4: (opcional) entero con el número de filas del montaje. Se obtiene como sys.argv[3].
5. @arg5: (opcional) entero con el número de columnas del montaje. Se obtiene como sys.argv[4].

### Funciones
* generate_montage(filenames, output_fn, row_size = None, col_size = None): Primero, se ordenan los ficheros filenames según el número de corte, llamando a la función get_number y después a ordenaBurbuja. Se cargan las imagenes con una lista, según el salto. En caso de que no se definan row_size y col_size, se calcula row_size como el entero más próximo a la raíz cuadrada del numero de filenames, para crear un montaje cuadrado. A partir de este valor, se obtiene el de col_size. En caso de que col_size sea superior a max_colsixe (definido como 10), se recalcularán los valores. Todo ello es para que no se obtenga un montaje que se salga. Se calculan los parámetro de ancho y alto del montaje y con un bucle for se va rellenando el montaje con las imágenes del corte, con la ayuda de la variable offset_x y offset_y. Se tiene la opción de definir un espacio entre cada imagen, con la variable margin.

* get_number(filenames): se obtiene el número de cada corte a partir del array filenames, pues cada elemento de filenames es una ruta cuyo nombre del corte tiene el formato: str(i) + '-' + Filename. El objetivo es conseguir un array de enteros que obtenga i, que se usará para ordenar filenames según este número de menor a mayor. 

* ordenaBurbuja(lista,aux): ordena por el método de burbuja, los filenames de menor a mayor. Para ello, con la función get_number se obtiene el número del corte de todas las imágenes en un vector auxiliar (aux) que se usará para ordenar filenames (lista).

## montage_periodicidad
Montage crea un montaje en .jpg, a partir de las imágenes de los cortes. Tiene la opción de introducir 3 o 5 parámetros de entrada: La ruta de la carpeta donde se encuentran las imágenes de los cortes, cuyo formato de nombre será el creado en transform.py (str(i) + '-' + Filename) y el nombre del montaje, además de dos parámetros de perioricidad, n_si y n_no, con los que se aceptarán n_si imágenes para el montaje y n_no imágenes sin aceptar en el montaje. Adicionalmente se podrá introducir el número de imágenes que aparezcan en las filas y en las columnas del montaje. Está formado por 3 funciones: generate_montage (el principal), get_number y ordenaBurbuja. 

### Argumentos
1. @arg1: str con la ruta de la carpeta de cortes. En el main, se llama a la función glob.glob(ruta). Se obtiene como sys.argv[1].
2. @arg2: ruta donde se guardará el montaje con el siguiente formato: ruta_imagen/nombre_imagen/montage/nombre_montaje.jpg. El nombre del montaje suele ser el corte. Se obtiene como sys.argv[2].
3. @arg3: entero para coger n_si imágenes para el montaje.
4. @arg4: entero para no coger n_no imágenes para el montaje.
5. @arg5: (opcional) entero con el número de filas del montaje. Se obtiene como sys.argv[3].
6. @arg6: (opcional) entero con el número de columnas del montaje. Se obtiene como sys.argv[4].

### Funciones
* generate_montage(filenames, output_fn, n_si, n_no, row_size = None, col_size = None): Primero, se ordenan los ficheros filenames según el número de corte, llamando a la función get_number y después a ordenaBurbuja. Se cargan las imagenes con una lista, según la función vector_zeros_ones. En caso de que no se definan row_size y col_size, se calcula row_size como el entero más próximo a la raíz cuadrada del numero de filenames, para crear un montaje cuadrado. A partir de este valor, se obtiene el de col_size. En caso de que col_size sea superior a max_colsixe (definido como 10), se recalcularán los valores. Todo ello es para que no se obtenga un montaje que se salga. Se calculan los parámetro de ancho y alto del montaje y con un bucle for se va rellenando el montaje con las imágenes del corte, con la ayuda de la variable offset_x y offset_y. Se tiene la opción de definir un espacio entre cada imagen, con la variable margin.

* get_number(filenames): se obtiene el número de cada corte a partir del array filenames, pues cada elemento de filenames es una ruta cuyo nombre del corte tiene el formato: str(i) + '-' + Filename. El objetivo es conseguir un array de enteros que obtenga i, que se usará para ordenar filenames según este número de menor a mayor. 

* ordenaBurbuja(lista,aux): ordena por el método de burbuja, los filenames de menor a mayor. Para ello, con la función get_number se obtiene el número del corte de todas las imágenes en un vector auxiliar (aux) que se usará para ordenar filenames (lista).

* vector_zeros_ones(filenames, n_si, n_no): se crea un array auxiliar aux, que se almacenará tantos unos como n_si, y tantos ceros como n_no. Tras esto, almacena filenames según el array aux. 
