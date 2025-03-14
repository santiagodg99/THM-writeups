## User.txt
En primer lugar obtenemos **la IP de THM** y hacemos un **ping** y un **nmap**. Gracias al **nmap** vemos que los puertos **SSH** y **HTTP** están abiertos.

![](/Library/user/ping_nmap.png)

Ahora realizaremos un **feroxbuster** para encontrar **directorios ocultos**. Lo único interesante que podemos encontrar son los directorios **images** y **robots.txt**.
![](/Library/user/feroxbuster_1.png)
![](/Library/user/feroxbuster_2.png)

Si visitamos la web (ponemos la IP en brave) encontramos un post del usuario **meliodas** y tres comentarios de **root**, **www-data** y **Anonymous**.
![](/Library/user/web_1_meliodas.png)
![](/Library/user/web_2_root_data_anonymous.png)

Abrimos ahora el directorio **robots.txt** en el navegador y encontramos la siguiente información. 
![](/Library/user/rockyou.png)

Esta es una pista que nos indica que debemos usar el diccionario **rockyou.txt** para encontrar una contraseña. Probemos a usar **hydra** empleando el usuario **meliodas**. Efectivamente obtenemos la contraseña.
![](/Library/user/password_meliodas.png)

Ahora que tenemos la contraseña hacemos **ssh** a la máquina y obtenemos la **user flag**.
![](/Library/user/user_flag.png)

## Root.txt
En primer lugar miraremos los permisos **sudo** que posee nuestro usuario **meliodas**. 
![](/Library/root/bak_script.png)

Vemos que podemos usar **python** para ejecutar un script llamado **bak.py**. Veamos qué contiene `nano bak.py`
![](/Library/root/contenido_bak.png)

Lamentablemente no podemos introducir ahí nuestra **shell** ya que no tenemos **permiso de escritura**. 

Lo que haremos será **eliminar este script**, **crear uno nuevo** e **introducir ahí nuestro exploit**.
![](/Library/root/nuevo_bak_1.png)
![](/Library/root/nuevo_bak_2.png)
**(ctrl+O, Enter, ctrl+X)**

Ahora ejecutaremos el código para hacernos **root**.
![](/Library/root/rooot.png)

Por último nos movemos al directorio **root** y obtenemos la **flag**.
![](/Library/root/root_txt.png)















