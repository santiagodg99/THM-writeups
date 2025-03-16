# PICKLE RICK

### First Ingredient
En primer lugar realizamos un escaneo con **nmap** y encontramos **2** puertos abiertos: **22 (SSH)** y **80 (servidor Web)**.

![](./Imagenes/1.png)

Si accedemos al **servidor web** y miramos el **código fuente** encontraremos un **username**.

![](./Imagenes/2.png)

![](./Imagenes/3.png)

Si ahora hacemos **fuzzing** con **gobuster** encontraremos **3** archivos interesantes: **login.php**, **portal.php** y **robots.txt**.

![](./Imagenes/4.png)

Si accedemos en primer lugar al **robots.txt** encontraremos lo que parece ser una **contraseña** para el **username** que encontramos antes.

![](./Imagenes/5.png)

Si vamos ahora al **login.php** e introducimos ese par **username/contraseña** obtendremos acceso al **portal.php**.

![](./Imagenes/6.png)

![](./Imagenes/7.png)

Si escribimos ahora **ls -a** en el **Command Panel** veremos una lista de archivos, entre los que destaca **Sup3rS3cretPickl3Ingred.txt**.

![](./Imagenes/8.png)

Lamentablemente no podemos acceder a su contenido usando **cat**, así que tendremos que buscar otra alternativa.

![](./Imagenes/9.png)

Investigando un poco descubrimos que también podríamos utilizar el comando **less**. Probamos y listo, ya tenemos nuestro **first ingredient**.

![](./Imagenes/10.png)

![](./Imagenes/11.png)

**ANSWER:** mr.meeseek hair

### Second Ingredient
En la lista de archivos anterior había otro archivo llamado **clue.txt**, que como su nombre indica podría darnos alguna pista que nos ayude a continuar. Si accedemos a su contenido utilizando **less** nos aparecerá lo siguiente.

![](./Imagenes/12.png)

Luego tendremos que movernos por los diferentes directorios para encontrar un nuevo ingrediente.

Probaremos en primer lugar con el directorio **/home**. Si hacemos **ls** veremos que contiene **dos directorios de usuario**: **rick** y **ubuntu**.

![](./Imagenes/13.png)

Si miramos en el directorio **rick** encontraremos el archivo **second ingredients**.

![](./Imagenes/14.png)

Si visualizamos su contenido con **less home/rick/"second ingredients"** encontraremos la segunda respuesta.

![](./Imagenes/15.png)

**ANSWER:** 1 jerry tear

### Third Ingredient
Si introducimos el comando **whoami** veremos que somos el user **www-data**, que parece no ser **root**.

![](./Imagenes/16.png)

Sin embargo, si introducimos el comando **sudo -l** veremos que nuestro user **puede ejecutar cualquier comando como root sin necesidad de contraseña**. 

![](./Imagenes/17.png)

Entonces, simplemente utilizando el prefijo **sudo** podremos ver todo el contenido del directorio **root**, donde encontraremos el archivo **3rd.txt**.

![](./Imagenes/18.png)

Si visualizamos el contenido de este archivo con **sudo less /root/3rd.txt** encontraremos el **tercer y último ingrediente**.

![](./Imagenes/19.png)

**ANSWER:** fleeb juice














