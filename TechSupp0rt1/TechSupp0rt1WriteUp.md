# TECHSUPP0RT1

## root.txt
Empezamos obteniendo la **IP de THM**, haciéndole **ping** y después **nmap**. Encontramos varios puertos abiertos: **22 (SSH)**, **80 (HTTP)**, **139 y 445 (Samba)**
![](./root/ping_nmap.png)

Si ponemos la IP en el navegador veremos que en el **puerto 80** se está corriendo **Apache2 Ubuntu Default Page**.
![](./root/web_80_port.png)

Si hacemos fuzzing con **feroxbuster** encontraremos 2 directorios que no nos sirven para nada, así que en su lugar examinaremos **SMB** a ver si encontramos algo interesante. **(la contraseña que pide es la de nuestra máquina)**
![](./root/websvr_SMB.png)

Intentemos conectarnos a **websvr**. Una vez hecho escribimos **ls** y con ello encontramos un fichero **enter.txt**.

![](./root/enter_txt.png)

Obtenemos ese fichero, salimos y vemos su contenido. Encontraremos credenciales cifradas y referencias a un directorio llamado **/subrion**
![](./root/get_exit.png)
![](./root/contenido_enter.png)

Usaremos **Cyberchef** para desencriptar esa contraseña. (buscar **from base58**, **from base32**, **from base64**, en ese orden, y listo)
![](./root/cyberchef.png)

Ahora navegamos al directorio **/subrion/panel** en el navegador e introducimos las credenciales que hemos hallado: **admin/Scam2021**
![](./root/credenciales_subrion.png)
![](./root/subrion_interface.png)

Ahora usamos **searchsploit** en la terminal para buscar un exploit de **Authenticated File Upload vulnerability** y lo ejecutamos para obtener una **reverse shell**.
![](./root/searchsploit.png)
![](./root/49876.png)

Ahora vemos el contenido de **wp-config.php** y encontramos una **contraseña**.
![](./root/password_wp_config.png)

Usamos esa contraseña para loggearnos por **SSH** como el usuario **scamsite**.
![](./root/ssh_scamsite.png)

Si ejecutamos el comando `sudo -l` veremos que este usuario puede ejecutar el comando `/usr/bin/iconv` como **root**.
![](./root/comando_sudo.png)

Podemos utilizar el permiso especial de la herramienta **iconv** para hallar la **root flag**.
![](./root/root_flag.png)












