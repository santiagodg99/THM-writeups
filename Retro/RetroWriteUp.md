### (a) ¿Cuál es el directorio oculto en el que se encuentra el web server?
Esta máquina no responde a **ping**, así que haremos **nmap** directamente.
![](./hidden_directory/nmap.png)
Encontramos 2 puertos abiertos: **80** (IIS Webserver) y **3389** (RDP access).

Si ahora realizamos un fuzzing con **feroxbuster** encontraremos el directorio oculto que buscamos: **/retro**.
![](./hidden_directory/retro.png)

Podemos comprobar que efectivamente es el directorio donde se aloja la web buscándolo en **Brave**.
![](./hidden_directory/retro_brave.png)

**ANSWER:** /retro

### (b) user.txt
Si miramos en la página web encontraremos tanto un nombre de usuario: **wade** como una contraseña: **parzival**.
![](./hidden_directory/retro_brave.png)
![](./user/parzival_1.png)
![](./user/parzival_2.png)

Si utilizamos estas credenciales con el protocolo **xfreerdp** de escritorio remoto accederemos al escritorio de Windows en el que encontraremos el fichero **user.txt**. Su contenido es la respuesta de la segunda pregunta de THM.
![](./user/xfreerdp.png)
![](./user/user_txt_1.png)
![](./user/user_txt_2.png)

### (c) root.txt
Si accedemos a la **papelera de reciclaje** encontraremos el archivo **hhupd**, el cual nos permitirá **escalar privilegios**. A continuación se explica cómo:

![](./root/hhupd_restore.png)

En primer lugar, al restaurarlo e intentar abrirlo como **administrador** nos pedirá una contraseña que no tenemos, así que lo que haremos será pinchar en **Show more details**, luego en **Show information about the publisher's certificate** y por último en **VeriSign Commercial Software Publishers CA**.

![](./root/hhupd_as_administrator.png)

Esta acción nos redirigirá al **navegador**, el cual nos indicará que no se puede conectar a la página. Entonces lo que haremos será irnos a **Settings** > **File** > **Save as...** para descargar el archivo.

![](./root/save_webpage_1.png)

A la hora de indicar el **nombre del archivo** escribiremos **C:\Windows\System32\"."** y posteriormente la ruta **C:\Windows\System32\cmd.exe** en la barra superior.

![](./root/save_webpage_2.png)
![](./root/cmd_enter.png)

Esto nos abrirá una **terminal**. Si ejecutamos **whoami** veremos que hemos conseguido **elevar nuestros privilegios** y hacernos **administrador**.

![](./root/whoami.png)

Entonces lo que haremos a continuación será cambiar al usuario **Administrador**, movernos a **Desktop** y hacer **dir**. Encontraremos un archivo **root.txt.txt**. Si visualizamos su contenido con el comando **type** encontraremos la **respuesta a la última pregunta**.

![](./root/root_txt.png)

















