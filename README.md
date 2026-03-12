# INSTALACION EN UBUNTU SERVER

## Usuarios
1. usuario: hugob --> contraseña: hugob
2. usuario: usuarioftp -->contraseña: usuario
3. usuario: usuario_SSH --> contraseña: usuario

## PARTICIONES
1. Partición "/" --> Root, aloja el sistema operativo
2. Partición "/home" --> Protege los archivos del usuario/usuarios
3. Partición "/swap" --> Es una memoria RAM de emergencia

![Particiones](fotos/Pasted%20image%2020260309091841.png)

## Instalación de SSH
1. Para que sea mas fácil usamos el comando **"sudo bash"**. Esto lo que hace es ponernos como administrador.
2. Actualizamos el sistema con el comando **"apt update"**.
3. Usamos el comando **"apt install openssh-server"**, debería salir algo así:
![Instalación SSH](fotos/Pasted%20image%2020260311093809.png)

4. Ahora usamos el comando **"systemctl status ssh"** para comprobar que funciona.
![Estado SSH](fotos/Pasted%20image%2020260311094041.png)

- Para levantar el servicio SSH:
  - **"systemctl enable ssh"** ![Enable SSH](fotos/Pasted%20image%2020260311114818.png)

5. Por último: **"ufw allow ssh"** para el firewall.
![Firewall SSH](fotos/Pasted%20image%2020260311094245.png)

## INSTALACION SERVIDOR APACHE
1. Comando **"apt install apache2"**.
![Apache](fotos/Pasted%20image%2020260311095927.png)

2. Comprobamos el estado:
   - **"systemctl status apache2"**
![Status Apache](fotos/Pasted%20image%2020260311100214.png)

3. Abrimos el puerto en el Firewall:
   - **"ufw allow in 'Apache'"**
![Firewall Apache](fotos/Pasted%20image%2020260311115448.png)

## Instalación servidor FTP
1. Instalamos **"vsftpd"**.
2. Comando **"apt install vsftpd"**.
![Install FTP](fotos/Pasted%20image%2020260311121208.png)

3. Comprobamos estado: **"systemctl status vsftpd"**.
![Status FTP](fotos/Pasted%20image%2020260311121345.png)

4. Habilitar puertos 20 y 21:
![Puertos FTP](fotos/Pasted%20image%2020260311121345.png) 

## Usuario en el servidor FTP
1. Creamos usuario: **"adduser (nombre)"**.
![Adduser](fotos/Pasted%20image%2020260312082305.png)

2. Configuramos: **"nano /etc/vsftpd.conf"**.
3. Activar **"chroot_local_user=YES"**.
![Chroot](fotos/Pasted%20image%2020260312083515.png)

4. Activar **"write_enable=YES"**.
5. Añadir al final: **"allow_writeable_chroot=YES"**.
![Write Permissions](fotos/Pasted%20image%2020260312084701.png)

6. Reiniciar: **"systemctl restart vsftpd"**.

## Usuario SSH
1. Crear Usuario: **"adduser (nombre)"**.
![User SSH](fotos/Pasted%20image%2020260312090910.png)

2. Añadir a sudo: **"usermod -aG sudo (nombre)"**.

## Cambiar a Adaptador Puente
1. Apagar máquina y entrar en Configuración -> Red.
![Config Red](fotos/Pasted%20image%2020260312093555.png)
2. Seleccionar "Adaptador puente".
![Adaptador Puente](fotos/Pasted%20image%2020260312093833.png)

## COMO TRANSFERIR ARCHIVOS
1. Usar FileZilla y conectar con la IP de la VM.
![Filezilla](fotos/Pasted%20image%2020260312101459.png)

2. Si hay error de permisos: **"sudo chown -R usuarioftp:usuarioftp /home/usuarioftp"**.

## PASAR DE FTP A APACHE
1. Copiar archivos: **"sudo cp -R /home/usuarioftp/nombre_proyecto/* /var/www/html/"**.
