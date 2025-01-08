
# Bitácora de Auditoría CIS Level 2 - Ubuntu Pro
**¡LOS REGIS SE ENCARGAN DE AUDITAR TU SISTEMA!**
![Descripción de la imagen](https://github.com/cifpfbmoll/proyecto-seguridad-en-redes-corporativas-grupo-bts-borja-tomeu-y-santiago/blob/tcifre-capturas/6e202659389977a8370e4ec617d562159bc04ab5ae7219139ffdd1561663fa55_1.gif?raw=true)

## **Introducción**
El objetivo de esta auditoría es asegurar que un sistema Ubuntu cumpla con las pautas de seguridad del nivel **CIS Level 2 Server**. Para ello, seguiremos un proceso de auditoría que incluye generar un fichero de configuración `tailor.xml`, modificar ciertas configuraciones del sistema (como deshabilitar el firewall UFW y el GDM), y realizar auditorías antes y después de los cambios para medir la evolución de la seguridad.

## **Proceso de Auditoría**
### 0. Previsión de Tareas

| Tarea | Descripción | Completada |
|-------|-------------|------------|
| **1. Instalación del Sistema** | Instalación de Ubuntu y activación de Ubuntu Pro y USG. | ✅ |
| **2. Desactivación del Cortafuegos** | Deshabilitar UFW antes de la auditoría. | ✅ |
| **3. Primera Auditoría** | Realizar la auditoría inicial usando el perfil CIS Level 2. | ✅ |
| **4. Modificación del Tailor File** | Deshabilitar UFW y GDM en el archivo tailor.xml. | ✅ |
| **5. Aplicación del Fix** | Aplicar el fix tailor.xml y reiniciar el sistema. | ✅ |
| **6. Reauditoría del Sistema** | Segunda auditoría después de aplicar las modificaciones. | ✅ |
| **7. Configuración de Seguridad Adicional** | Configuración de GRUB, creación de contraseñas y permisos. | ✅ |
| **8. Creación de Usuarios** | Crear un nuevo usuario y hacerlo sudoer. | ✅ |
| **9. Actualización de Software** | Ajustar el archivo `sources.list` para cumplir con las CIS y el INCIBE. | ✅ |
### 1. **Instalación del Sistema**
- Instalamos **Ubuntu** en una máquina virtual.
- Activamos **Ubuntu Pro** desde la consola con el comando `pro attach`.
- Activamos el **USG** (Ubuntu Security Guide). Si Ubuntu Pro está activado, esta operación se realiza automáticamente.
- Comprobamos la correcta instalación con comandos como `screenshot` para verificar los pasos.

### 2. **Desactivación del Cortafuegos (UFW)**
Antes de realizar la primera auditoría, deshabilitamos el cortafuegos:
```bash
sudo ufw disable
```

### 3. **Primera Auditoría**
Realizamos la primera auditoría usando el perfil de **CIS Level 2 Server**:
```bash
sudo usg audit cis_level2_server
```
Movemos el informe HTML a una ubicación apropiada, ¡home y desktop NO!, para poder visualizar el archivo en nuestro navegador de confianza.
```bash
sudo cp /var/lib/usg/usg-report-20241020.0115.html ~/Escritorio/
```

### 4. **Modificación del Tailor File**
Creamos un archivo `tailor.xml` para realizar las configuraciones: deshabilitar las reglas relacionadas con el **firewall (UFW)** y el **GDM** (entorno gráfico de GNOME):
```bash
sudo usg generate-tailoring cis_level2_server tailor.xml
```
Desactivamos todas las configuraciones relacionadas con **UFW** y **GDM**.
```bash
sudo usg fix --tailoring-file tailor.xml
```

### 5. **Reinicio del Sistema**
¡Después de modificar el archivo `tailor.xml`, es necesario reiniciar el sistema!
```bash
sudo reboot
```

### 6. **Reauditoría del Sistema**
Volvemos a auditar el sistema:
```bash
sudo usg audit cis_level2_server
```
Copiamos el nuevo informe de auditoría y comparamos los resultados.

### 7. **Configuración de Seguridad Adicional**
Accedemos al terminal con `CTRL + ALT + F3` configuramos el arranque de GRUB:
- Creamos una contraseña de arranque para **GRUB**:
```bash
grub-mkpasswd-pbkdf2
```
- Establecemos permisos solo para root en caso de querer ver el archivo de configuración de GRUB:
```bash
chmod 400 /boot/grub/grub.cfg
```

### 8. **Creación de Usuarios**
Creamos un nuevo usuario llamado y lo agregamos a los **sudoers**:
```bash
adduser admrgion
usermod -aG sudo admrgion
```

### 9. **Actualización de Software**
Modificar `sources.list` para que cumplan con las directrices de **INCIBE**. 
```bash
nano /etc/apt/sources.list
```
_________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
# Bitácora de Copias de Seguridad - Duplicati  
**¡REGIGIGAS ASEGURA TUS ARCHIVOS CON DUPLICATI!**  
![Descripción de la imagen](https://i.redd.it/te5olfvbszgb1.gif?raw=true)

## **Introducción**
En esta segunda fase, nos enfocaremos en implementar un sistema de copias de seguridad que garantice la integridad de los datos. Utilizaremos **Duplicati**, una herramienta de código abierto, para crear y gestionar las copias de seguridad en Google Drive.

---

## Proceso de Auditoría de Copias de Seguridad
### 0. Previsión de Tareas

| Tarea                             | Descripción                                                                                                 | Completada |
|-----------------------------------|-------------------------------------------------------------------------------------------------------------|------------|
| **1. Instalación de Duplicati**   | Configurar el entorno y descargar Duplicati                                   | ✅         |
| **2. Configuración de Duplicati** | Ajustar el software para implementar copias cifradas y no cifradas según las especificaciones | ✅         |
| **3. Prueba de Backup en Documentos**  | Crear una copia cifrada de la carpeta Documentos y probar la recuperación | ✅         |
| **4. Prueba de Backup en Imágenes** | Configurar una copia no cifrada de la carpeta Imágenes y probar la restauración  | ✅         |
| **5. Evaluación de Resultados**     | Revisar la correcta ejecución y recuperación de las copias de seguridad creadas en Google Drive.             | ✅         |

---

### 1. Instalación de Duplicati
1. Habilitamos el acceso a repositorios de terceros:
   ```bash
   cd /etc/apt
   sudo nano sources.list
   ```
2. Descargamos e instalamos Duplicati desde su página oficial:
   ```bash
   wget https://updates.duplicati.com/beta/duplicati_X.X.X.X-1_all.deb
   sudo apt update
   sudo apt install ./duplicati_X.X.X.X-1_all.deb
   sudo systemctl status duplicati
   ```
   > **Nota:** Verificar que Duplicati esté funcionando correctamente.

---

### 2. Configuración de Duplicati
Para proteger y gestionar las copias de seguridad, configuramos Duplicati siguiendo los pasos:

1. **Añadir contraseña a la interfaz de Duplicati** para acceso seguro:
   ```bash
   sudo duplicati-server
   ```

2. Configuramos las copias de seguridad en **Duplicati** para las carpetas especificadas:
   - **Copia en Documentos**: Cifrado AES-256, con un **RPO** de 1 hora.
   - **Copia en Imágenes**: Sin cifrado, con un **RPO** de 1 día.

---

### 3. Copia de Seguridad I: Documentos
Creamos una copia de seguridad en Google Drive para la carpeta Documentos:
1. **Configurar parámetros generales**:
   - Nombre, descripción y cifrado AES-256.
   - Establecer el origen en `/Documentos`.
2. **Configurar el destino** en Google Drive, generando un **token** de autenticación si es necesario.
3. **Establecer frecuencia de copia** para cumplir el **RPO** de 1 hora y asegurarnos de su correcto funcionamiento realizando pruebas de recuperación.

---

### 4. Copia de Seguridad II: Imágenes
Para la carpeta Imágenes configuramos un backup sin cifrado en Google Drive:
1. **Parámetros generales**: Establecemos la carpeta de origen en `/Imágenes` y omitimos el cifrado.
2. **Definimos el destino** en Google Drive y configuramos la frecuencia para cumplir con el **RPO** de 1 día.
3. **Ejecutamos una prueba de recuperación** para validar la correcta restauración de los archivos.

---

### 5. Evaluación y Resultados
Verificamos que las copias de seguridad y las restauraciones de las carpetas Documentos e Imágenes funcionen correctamente:
- Las copias se realizaron en Google Drive, cumpliendo con los criterios de cifrado y frecuencia de cada carpeta.
- Llevamos a cabo las pruebas de restauración, resultaron exitosas, lo que garantiza la disponibilidad y seguridad de los datos respaldados.

_________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
# Bitácora de Hardening de Apache - Ubuntu Pro y Kali Linux
**¡LOS REGIS SE ENCARGAN DE ENDURECER TU APACHE!**
![Descripción de la imagen](https://cdn74.picsart.com/182821608000202.gif?to=min&r=640)

## **Introducción**  
El hardening de Apache busca aumentar la seguridad del servidor web, reduciendo su exposición a ciberataques y cumpliendo con las mejores prácticas del sector. Este proceso se basa en la guía básica de hardening de Apache ofrecida por el INCIBE. En este apartado, configuraremos aspectos críticos de Apache, desde la instalación inicial hasta la protección contra ataques DoS y SQL Injection.

---

## **Proceso de Hardening**  
### 0. Previsión de Tareas  

| Tarea                                       | Descripción                                                                                     | Completada |
|---------------------------------------------|-------------------------------------------------------------------------------------------------|------------|
| **1. Instalación de Apache**                | Instalar y verificar el servidor web Apache.                                                   | ✅         |
| **2. Configuraciones globales**             | Configurar parámetros de seguridad esenciales como usuarios, grupos y ocultación de versiones. | ✅         |
| **3. Creación de Virtual Host**             | Configurar un VirtualHost personalizado con opciones seguras.                                  | ✅         |
| **4. Ficheros `.htaccess` y Hotlinking**    | Configurar restricciones en directorios y proteger recursos frente a uso indebido.             | ✅         |
| **5. Configuración HTTPS**                  | Implementar certificados SSL/TLS usando OpenSSL.                                               | ✅         |
| **6. Instalación y Configuración de ModSecurity** | Proteger Apache con un WAF e implementar reglas OWASP y detección de SQL Injection.            | ✅         |
| **7. Pruebas de Ataques y Validación**      | Simular ataques DoS y verificar la resistencia del servidor.                                   | ✅         |

---

## 1. Instalación de Apache
Instalamos y verificamos el servidor web Apache para asegurarnos de que esté funcionando correctamente.
```bash
sudo apt update
sudo apt install apache2
sudo systemctl status apache2
```
> **Nota:** Verificar que Apache esté activo y corriendo sin errores.

---

## 2. Configuraciones globales
Configuramos parámetros de seguridad esenciales como la ocultación de la versión de Apache, los usuarios y grupos, entre otros ajustes para proteger el servidor.
```bash
sudo nano /etc/apache2/conf-available/security.conf
sudo systemctl restart apache2
```
> **Nota:** Es importante ocultar información sensible, como la versión de Apache, para reducir el riesgo de ataques dirigidos.

---

## 3. Creación de Virtual Host
Creamos y configuramos un VirtualHost para alojar nuestros sitios web de manera segura y personalizada, especificando las rutas y configuraciones necesarias.
```bash
sudo nano /etc/apache2/sites-available/mi_sitio.conf
sudo a2ensite mi_sitio.conf
sudo systemctl reload apache2
```
> **Nota:** Verificar que el sitio esté correctamente accesible a través de su dominio o IP.

---

## 4. Ficheros `.htaccess` y Hotlinking
Configurar archivos `.htaccess` para restringir el acceso a ciertos directorios y proteger recursos como imágenes frente a hotlinking.
```bash
sudo nano /var/www/html/.htaccess
sudo systemctl restart apache2
```
> **Nota:** Asegurarse de que la opción `AllowOverride` esté habilitada en la configuración de Apache para usar `.htaccess`.

---

## 5. Configuración HTTPS
Implementamos un certificado SSL/TLS para habilitar HTTPS en el servidor web, utilizando OpenSSL o Certbot para la configuración.
```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache
```
> **Nota:** Verificar que el certificado SSL esté correctamente instalado usando `https://` en la URL.

---

## 6. Instalación y Configuración de ModSecurity
Instalamos y configuramos ModSecurity como un firewall de aplicaciones web (WAF) para proteger Apache contra ataques como SQL Injection y XSS.
```bash
sudo apt install libapache2-mod-security2
sudo systemctl restart apache2
```
> **Nota:** Asegúrese de activar las reglas de seguridad de OWASP para una mayor protección.

---

## 7. Pruebas de Ataques y Validación
Simulamos ataques de Denegación de Servicio (DoS) para validar que el servidor pueda resistir intentos de sobrecarga y otros vectores de ataque comunes.
```bash
sudo msfconsole
```
> **Nota:** Verificar los registros de Apache para asegurar que no se haya producido una caída del servidor.

_________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
# Bitácora de Hardening SSH - Ubuntu Pro

**¡LOS REGIS SE ENCARGAN DE ENDURECER TU SERVIDOR SSH!**
<img src="https://media.tenor.com/kgtbAHhOuYcAAAAM/regidrago-regieleki.gif?to=min&r=640" alt="Regis" width="800">

## **Introducción**  
El hardening de SSH busca reforzar la seguridad en las conexiones remotas a través del protocolo Secure Shell, mitigando riesgos de accesos no autorizados y cumpliendo con las mejores prácticas del sector. Esta guía describe un conjunto de configuraciones esenciales para proteger OpenSSH Server, desde su instalación hasta la implementación de autenticación multifactor.

---

## **Proceso de Hardening**  
### 0. Previsión de Tareas  

| Tarea                                            | Descripción                                                                                      | Completada |
|--------------------------------------------------|--------------------------------------------------------------------------------------------------|------------|
| **1. Instalación de SSH Server**                | Instalar y verificar OpenSSH Server.                                                             | ✅         |
| **2. Autenticación basada en clave pública**   | Configurar la autenticación segura mediante claves SSH.                                         | ✅         |
| **3. Reasignación de puertos por defecto**      | Cambiar el puerto por defecto del servicio SSH.                                                  | ✅         |
| **4. Límite de vinculación IP**                | Configurar el acceso solo a IPs específicas.                                                   | ✅         |
| **5. Deshabilitar el usuario root**              | Prevenir accesos directos a la cuenta root.                                                      | ✅         |
| **6. Limitar acceso a usuarios determinados**    | Restringir el acceso SSH a ciertos usuarios.                                                     | ✅         |
| **7. Deshabilitar inicio de sesión por contraseña** | Obligar el uso de claves SSH para el inicio de sesión.                                          | ✅         |
| **8. Deshabilitar contraseñas vacías**          | Rechazar inicios de sesión con contraseñas vacías.                                           | ✅         |
| **9. Limitar intentos de autenticación fallida** | Prevenir ataques de fuerza bruta configurando límites de intentos fallidos.                      | ✅         |
| **10. Limitar conexiones simultáneas**           | Reducir el número de conexiones SSH no autenticadas.                                            | ✅         |
| **11. Habilitar un banner de advertencia**       | Configurar un mensaje previo al inicio de sesión.                                               | ✅         |
| **12. Configurar intervalos de cierre de sesión**| Establecer tiempos de inactividad para cerrar sesiones automáticamente.                        | ✅         |
| **13. Deshabilitar X11Forwarding**               | Prevenir la ejecución de aplicaciones gráficas remotas mediante SSH.                             | ✅         |
| **14. Configurar CHROOT para usuarios**          | Restringir a los usuarios a sus directorios de inicio.                                           | ✅         |
| **15. Configurar autenticación multifactor**     | Implementar doble factor de autenticación para máxima seguridad.                                | ✅         |

---

## 1. Instalación de SSH Server
Instalamos OpenSSH Server y verificamos su correcto funcionamiento:
```bash
sudo apt update
sudo apt install openssh-server
sudo systemctl status sshd
```
> **Nota:** Es fundamental limitar los permisos del archivo `sshd_config` para evitar modificaciones no autorizadas.

---

## 2. Autenticación basada en clave pública
La autenticación mediante claves SSH aumenta la seguridad al eliminar contraseñas.
```bash
ssh-keygen -t ed25519
ssh-copy-id usuario@ip_servidor
```
> **Nota:** Las claves deben protegerse con una passphrase para mayor seguridad.

---

## 3. Reasignación de puertos por defecto
Reconfiguramos el puerto 22 para mitigar ataques automatizados:
```bash
sudo nano /etc/ssh/sshd_config
# Cambiar o añadir:
Port +300
sudo systemctl restart sshd
```
> **Nota:** No olvides abrir el nuevo puerto en el firewall.

---

## 4. Límite de vinculación IP
Restricción de acceso solo a IPs específicas mediante la directiva ListenAddress:
```bash
ListenAddress 192.168.1.100
```
> **Nota:** Asegúrate de que la IP configurada es accesible.

---

## 5. Deshabilitar el usuario root
Desactivamos el inicio de sesión directo para la cuenta root:
```bash
sudo nano /etc/ssh/sshd_config
# Cambiar o añadir:
PermitRootLogin no
sudo systemctl restart sshd
```
> **Nota:** Usa cuentas normales con permisos de sudo para tareas administrativas.

---

## 6. Limitar acceso a usuarios determinados
Definimos qué usuarios pueden conectarse vía SSH:
```bash
sudo nano /etc/ssh/sshd_config
# Añadir:
AllowUsers x.x.x., y.y.y.y
```
> **Nota:** Esta medida reduce la superficie de ataque.

---

## 7. Deshabilitar inicio de sesión por contraseña
Una vez configuradas las claves SSH, desactivamos las contraseñas:
```bash
sudo nano /etc/ssh/sshd_config
# Cambiar o añadir:
PasswordAuthentication no
```
> **Nota:** Verifica que las claves funcionan antes de aplicar esta configuración.

---

## 8. Deshabilitar contraseñas vacías
Bloqueamos cuentas con contraseñas vacías:
```bash
sudo nano /etc/ssh/sshd_config
# Cambiar o añadir:
PermitEmptyPasswords no
```
> **Nota:** Asegúrate de que todas las cuentas tienen contraseñas seguras.

---

## 9. Limitar intentos de autenticación fallida
Configuramos límites para prevenir ataques de fuerza bruta:
```bash
sudo nano /etc/ssh/sshd_config
# Añadir:
MaxAuthTries 4
```
> **Nota:** Ajusta el límite según tus necesidades de seguridad.

---

## 10. Limitar conexiones simultáneas
Reducimos el número de conexiones no autenticadas:
```bash
sudo nano /etc/ssh/sshd_config
# Añadir:
MaxStartups 3
```
> **Nota:** Esto disminuye la exposición a ataques masivos.

---

## 11. Habilitar un banner de advertencia
Mostramos un mensaje de advertencia antes de la autenticación:
```bash
sudo nano /etc/issue
# Edita el contenido del mensaje
```
> **Nota:** Configura el banner en `sshd_config` con la directiva `Banner`.

---

## 12. Configurar intervalos de cierre de sesión
Establecemos tiempos de inactividad para cerrar sesiones:
```bash
sudo nano /etc/ssh/sshd_config
# Añadir:
ClientAliveInterval 300
ClientAliveCountMax 0
```
> **Nota:** Esto previene accesos desatendidos.

---

## 13. Deshabilitar X11Forwarding
Prevenimos la ejecución de aplicaciones gráficas remotas:
```bash
sudo nano /etc/ssh/sshd_config
# Cambiar o añadir:
X11Forwarding no
```
> **Nota:** Desactívalo si no usas aplicaciones gráficas remotas.

---

## 14. Configurar CHROOT para usuarios
Restringimos a los usuarios a sus directorios personales:
```bash
sudo nano /etc/ssh/sshd_config
# Añadir:
Match User usuario
ChrootDirectory /home/usuario
ForceCommand internal-sftp
AllowTcpForwarding no
```
> **Nota:** Configura adecuadamente los permisos del directorio.

---

## 15. Configurar autenticación multifactor
Aumentamos la seguridad implementando doble factor de autenticación:
```bash
sudo apt install libpam-google-authenticator
sudo systemctl restart sshd
```
> **Nota:** Usa aplicaciones como Google Authenticator para generar códigos.

_________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
# Bitácora de Escaneo de Vulnerabilidades - Ubuntu Pro
