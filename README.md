# INSTALACION EN UBUNTU SERVER
---

## Usuarios
1.  usuario: hugob --> contraseña: hugob.
2. usuario: usuarioftp -->contraseña: usuario.
3. usuario: usuario_SSH --> contraseña: usuario.

## PARTICIONES

1. Partición *"/"* --> Root, aloja el sistema operativo
2. Partición *"/home"* --> Protege los archivos del usuario/usuarios
3. Partición *"/swap"* --> Es una memoria RAM de emergencia

![Particiones](fotos/Pasted%20image%2020260309091841.png)

## Instalación de SSH

1. Para que sea mas fácil usamos el comando __*"sudo bash"*__. Esto lo que hace es ponernos como administrador y no tener que estar usando *"sudo"* todo el rato.
2. actualizamos el sistema con el comando __*"apt update"*__.
3. Usamos el comando __*"apt install openssh-server"*__, debería salir algo así:
![Instalacion SSH](fotos/Pasted%20image%2020260311093809.png)

4. Ahora usamos el comando __*"systemctl status ssh"*__, con este comando comprobamos que este funcionando bien.
   ![Estado SSH](fotos/Pasted%20image%2020260311094041.png)
   - Para levantar el servicio SSH se usa el comando:
     - __*"systemctl enable ssh"*__ ![Enable SSH](fotos/Pasted%20image%2020260311114818.png)

5. Por ultimo usamos el comando __*"ufw allow ssh"*__, este comando es por si tienes activado el firewall de Ubuntu, para que no tenga problemas.
   ![Firewall SSH](fotos/Pasted%20image%2020260311094245.png)

## INSTALACION SERVIDOR APACHE

1. Con el comando __*"apt install apache2"*__ , instalamos el servidor apache.
   ![Apache](fotos/Pasted%20image%2020260311095927.png)

2. comprobamos si esta funcionando y que funciona bien con el mismo comando que en la instalacion de SSH:
   - __*"systemctl status apache2"*__
   - ![Status Apache](fotos/Pasted%20image%2020260311100214.png)

3. Hacemos lo mismo que con SSH, abrimos el puerto para que te el Firewall no de problemas:
   - __*"ufw allow in "Apache""*__
     ![Firewall Apache](fotos/Pasted%20image%2020260311115448.png)


## Instalación servidor FTP

1. Buscando en Google he leído que actualmente el servicio mas seguro de FTP es __"vsftpd"__ , así que he elegido instalar este.
2. Con el comando __*"apt install vsftpd"*__ instalamos el servicio 
   ![Install FTP](fotos/Pasted%20image%2020260311121208.png)

3. Usando el mismo comando que para los otros servicio, __*"systemctl status vsftpd"*__ vemos si esta habilitado o no.
   ![Status FTP](fotos/Pasted%20image%2020260311121345.png)

4. Aquí también debemos de habilitar sus puertos para no tener problemas con el firewall, mismo comando:
   - __*"ufw allow 20/tcp"*__ y __*"ufw allow 21/tcp"*__
   - En este servicio se abren 2 puertos:
     - Puerto 20: sirve para la transferencia de datos 
     - Puerto 21: Gestiona la conexión y controla los datos 



## Usuario en el servidor FTP

1. Creamos el usuario usando el comando __*"adduser (nombre)"*__ , en el mismo comando nos va a pedir una contraseña, le ponemos contraseña y lo demás le podemos dar todo __"Enter"__
   ![Adduser](fotos/Pasted%20image%2020260312082305.png)

2. Abrimos el archivo de configuración del servidor FTP con el comando __*"nano /etc/vsftpd.conf"*__
   
   
3.  Dentro del archivo tenemos que activar el __*"Chroot"*__, viene comentado, así que para activarlo tienes que quitar la __"#"__.
   ![Chroot](fotos/Pasted%20image%2020260312083515.png)

4. Exactamente igual que en el paso 3, pero con __*"wirte_enable=YES"*__ 
   
5.  Tenemos que añadir los permisos de escritura, nos vamos al final del archivo y añadimos este comando en una linea nueva:
	- __*"Allow_writeable_chroot=YES"*__ , ctrl O y enter![Write Permissions](fotos/Pasted%20image%2020260312084701.png)

6. Reiniciamos el servicio ftp para aplicar los cambios
	- comando: __*"systemctl restart vsftpd"*__.

## Usuario SSH
1.  Crear Usuario, Mismo comando que antes;
	- __*"adduser (nombre)"*__
	- ![User SSH](fotos/Pasted%20image%2020260312090910.png)

2.  Añadirlo al grupo de administradores (mejor añadirlo al grupo de admins que darle todos lo permisos, por temas de seguridad)
	- Comando: __*"usermod -aG sudo (nombre)"*__


## Cambiar a Adaptador Puente

1. Para poder transferir archivos con la maquina anfitriona tendríamos que cambiar el tipo de red de la maquina, seria así:
	- Apagamos la maquina --> poweroff
	- Nos vamos a la configuración de la maquina virtual:![Config Red](fotos/Pasted%20image%2020260312093555.png)
	
	- En configuración buscamos el apartado "RED"
	  - En "Conectar a" desplegamos el menú y buscamos "Adaptador puente"	  ![Adaptador Puente](fotos/Pasted%20image%2020260312093833.png)
	  - Aceptamos y arrancamos la maquina.


## COMO TRANSFERIR ARCHIVOS
1. Instalamos FileZilla en la maquina anfitriona (en el portátil)
2. Después de haber puesto el modo "Adaptador puente", con el comando __*"ip addr"*__ sacamos la IP de la maquina virtual.
3. Rellenamos lo que nos pide en la parte de arriba (IP, nombre, contraseña y puerto)  y una vez que este todo le damos a conexión rápida.
	- ![Filezilla](fotos/Pasted%20image%2020260312101459.png)

4. Si llegas a este punto y te da un Error al subir los archivos se soluciona fácil.
	- En la VM usa el comando __*"sudo chown -R usuarioftp:usuarioftp /home/usuarioftp"*__
	- Este comando cambia el dueño de la carpeta, este comando significa que quiero que el dueño de esta carpeta sea el "usuarioftp"
	- ahora ya es dueño total de esa carpeta


## PASAR DE FTP A APACHE

1. Ahora lo ultimo que hay que hacer es pasar la carpeta del proyecto desde el servidor "FTP" a el servidor "Apache"
2. Una vez que tengamos la carpeta del proyecto en el "usuarioftp" usamos el comando __"sudo cp -R /home/usuarioftp/nombre_proyecto/* /var/www/html/"__
	- este comando lo que hace es copiar todo lo que tenga la carpeta del proyecto y pasarlo a el servidor apache para que se pueda ver la pagina web en otra maquina poniendo la IP
