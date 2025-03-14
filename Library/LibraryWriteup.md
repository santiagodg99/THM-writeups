# LIBRARY

## User.txt
En primer lugar obtenemos **la IP de THM** y hacemos un **ping** y un **nmap**. Gracias al **nmap** vemos que los puertos **SSH** y **HTTP** están abiertos.

![](./user/ping_nmap.png)

Ahora realizaremos un **feroxbuster** para encontrar **directorios ocultos**. Lo único interesante que podemos encontrar son los directorios **images** y **robots.txt**.
![](./user/feroxbuster_1.png)
![](./user/feroxbuster_2.png)

Si visitamos la web (ponemos la IP en brave) encontramos un post del usuario **meliodas** y tres comentarios de **root**, **www-data** y **Anonymous**.
![](./user/web_1_meliodas.png)
![](./user/web_2_root_data_anonymous.png)

Abrimos ahora el directorio **robots.txt** en el navegador y encontramos la siguiente información. 
![](./user/rockyou.png)

Esta es una pista que nos indica que debemos usar el diccionario **rockyou.txt** para encontrar una contraseña. Probemos a usar **hydra** empleando el usuario **meliodas**. Efectivamente obtenemos la contraseña.
![](./user/password_meliodas.png)

Ahora que tenemos la contraseña hacemos **ssh** a la máquina y obtenemos la **user flag**.
![](./user/user_flag.png)

## Root.txt
En primer lugar miraremos los permisos **sudo** que posee nuestro usuario **meliodas**. 
![](./root/bak_script.png)

Vemos que podemos usar **python** para ejecutar un script llamado **bak.py**. Veamos qué contiene `nano bak.py`
![](./root/contenido_bak.png)

Lamentablemente no podemos introducir ahí nuestra **shell** ya que no tenemos **permiso de escritura**. 

Lo que haremos será **eliminar este script**, **crear uno nuevo** e **introducir ahí nuestro exploit**.
![](./root/nuevo_bak_1.png)
![](./root/nuevo_bak_2.png)
**(ctrl+O, Enter, ctrl+X)**

Ahora ejecutaremos el código para hacernos **root**.
![](./root/rooot.png)

Por último nos movemos al directorio **root** y obtenemos la **flag**.
![](./root/root_txt.png)















