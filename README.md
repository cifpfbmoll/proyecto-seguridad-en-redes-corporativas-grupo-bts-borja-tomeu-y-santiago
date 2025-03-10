
# Bitácora de Auditoría CIS Level 2 - Ubuntu Pro
**¡LOS REGIS SE ENCARGAN DE AUDITAR TU SISTEMA!**
![Descripción de la imagen](https://i.kym-cdn.com/photos/images/original/001/737/447/581.gif?raw=true)

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
# Bitácora de Escaneo de Vulnerabilidades - Metasploitable2  
**¡LOS REGIS ESCANEAN LAS REDES CON NMAP!**  
![Descripción de la imagen](https://i.gifer.com/DMt2.gif?to=min&r=640)

## **Introducción**  
Nmap es una herramienta de código abierto usada para analizar redes. Su principal objetivo es identificar equipos conectados, servicios activos y características de la red analizada. Posee el reconocimiento de muchos administradores de sistemas y profesionales de seguridad para verificar configuraciones de endurecimiento. Sin embargo, debe usarse de manera ética y con autorización, ya que escanear redes sin permiso puede incurrir en consecuencias legales.

---

## **Proceso de Actividades con Nmap**  
### 0. Previsión de Tareas  

| Tarea                                                  | Descripción                                                                                              | Completada |
|--------------------------------------------------------|----------------------------------------------------------------------------------------------------------|------------|
| **1. Descargar y configurar Metasploitable2**          | Descargar el servicio virtualizado Metasploitable2 e importarlo en VirtualBox.                          | ✅         |
| **2. Realizar un escaneo SYN Scan**                    | Descubrir puertos abiertos en Metasploitable2 y el servidor de Ubuntu mediante escaneo SYN.              | ✅         |
| **3. Realizar un escaneo UDP**                         | Detectar los principales puertos UDP abiertos en Metasploitable2.                                       | ✅         |
| **4. Realizar un escaneo por script `vuln`**           | Automatizar la detección de vulnerabilidades conocidas en Metasploitable2 y el servidor de Ubuntu.       | ✅         |
| **5. Realizar un escaneo agresivo**                    | Obtener información detallada sobre Metasploitable2 y el servidor de Ubuntu.                            | ✅         |
| **6. Realizar un escaneo de descubrimiento en la red** | Identificar dispositivos conectados en la red doméstica.                                                | ✅         |

---

## **1. Descargar y configurar Metasploitable2**  
Para realizar los escaneos de seguridad y vulnerabilidades, precisamos de una máquina virtual Kali Linux. Los parámetros de red en VirtualBox deben asignarse como **Adaptador Solo Anfitrión**. El servicio virtualizado Metasploitable2 debe montarse como partición a partir de una nueva máquina virtual y configurarse también en **Adaptador Solo Anfitrión**.

---

## **2. Realizar un escaneo SYN Scan**  
Escaneo destinado a descubrir los puertos abiertos sin completar la comunicación por TCP (sin hacer el handshake completo).

```bash
nmap -sS 192.XXX.YYY.ZZZ 192.XXX.YYY.ZZZ
```

> **Nota:** Asegúrate de utilizar la IP correspondiente a Metasploitable2 y al servidor de Ubuntu.

---

## **3. Realizar un escaneo UDP**  
Escaneo que, mediante el protocolo UDP (User Datagram Protocol), detecta puertos abiertos en el objetivo.

```bash
nmap -sU [ -top-ports [VALOR] ] 192.XXX.YYY.ZZZ
```

> **Nota:** Este escaneo puede ser más lento, pero es esencial para detectar servicios que utilizan UDP.

---

## **4. Realizar un escaneo por script `vuln`**  
Escaneo mediante el script predefinido `vuln`. Utiliza el **Nmap Scripting Engine (NSE)** para automatizar la detección de vulnerabilidades conocidas.

```bash
nmap --script [nombre_del_script] 192.XXX.YYY.ZZZ
```

> **Nota:** Utiliza el script adecuado (`vuln`) para identificar vulnerabilidades en los servicios expuestos.

---

## **5. Realizar un escaneo agresivo**  
Escaneo destinado a obtener información detallada sobre el objetivo: sistema operativo, versiones de servicios, ejecución de scripts NSE y traceroute.

```bash
nmap -A 192.XXX.YYY.ZZZ 192.XXX.YYY.ZZZ
```

> **Nota:** Este tipo de escaneo es más intrusivo y debe realizarse con autorización previa.

---

## **6. Realizar un escaneo de descubrimiento en la red**  
Escaneo que identifica dispositivos conectados a la red sin realizar un análisis de puertos. Obtiene información sobre direcciones IP, MAC y nombres de host.

```bash
nmap -sn 192.XXX.YYY.0/ZZZ
```

> **Anotaciones:**  
> - Si tu máquina virtual usa conexión NAT, puede ser necesario configurar redirección de puertos.  
> - También puedes instalar Nmap en el equipo anfitrión para realizar pruebas sobre tu sistema personal.

_________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
# Bitácora de Seguridad Perimetral con pfSense  
**¡LOS REGIS CONFIGURAN TU FIREWALL!**  
![Descripción de la imagen](https://i.gifer.com/BmhC.gif)

## **Introducción**  
pfSense es un firewall de código abierto utilizado para la seguridad perimetral en redes. Su funcionalidad permite el control del tráfico, filtrado de paquetes y configuración de reglas avanzadas para proteger la infraestructura de TI. En este sprint, implementaremos reglas esenciales para garantizar la seguridad de la red.

---

## **Proceso de Configuración en pfSense**  
### 0. Previsión de Tareas  

| Tarea | Descripción | Completada |
|---------------------------------|----------------------------------------------------------------------------|------------|
| **1. Configurar reglas para que los empleados tengan acceso a internet** | Permitir el tráfico de salida desde la LAN de empleados a internet. | ✅ |
| **2. Configurar reglas para el acceso al servidor web** | Permitir acceso solo por el puerto 443 y redirigir el tráfico del puerto 80 al 443. | ✅ |
| **3. Configurar reglas para el acceso al servidor SSH** | Habilitar acceso solo desde el puerto configurado en el Sprint de Hardening SSH. | ✅ |
| **4. Bloquear el tráfico directo entre servidores y empleados** | Crear reglas que impidan comunicación directa entre ambos segmentos de red. | ✅ |
| **5. Implementar regla para evitar exfiltración de información** | Configurar regla de contención en caso de ciberataque. | ✅ |
| **6. Implementar regla contra ataques DoS** | Bloquear direcciones IP sospechosas que generen tráfico malicioso. | ✅ |
| **7. Implementar regla extra** | Personalizar una regla adicional que mejore la seguridad del entorno. | ✅ |

---

## **1. Configurar reglas para que los empleados tengan acceso a internet**  
Se crea una regla en pfSense que permite el tráfico saliente desde la LAN de empleados (10.0.2.0/24) hacia cualquier destino en internet.

```bash
Action: Pass - Source: LAN_EMP - Destination: Any - Protocol: Any
```
> **Nota:** Es recomendable restringir el acceso a los puertos y servicios indispensables.

---

## **2. Configurar reglas para el acceso al servidor web**  
Para restringir el acceso al servidor web solo por HTTPS y redirigir el tráfico HTTP:

```bash
Action: Pass - WAN - Destination: 10.0.3.XXX - Port: 443 - Protocol: TCP
NAT - WAN - Redirect 80 -> 443
```
> **Nota:** Se garantiza el cifrado de las comunicaciones forzando el uso de HTTPS.

---

## **3. Configurar reglas para el acceso al servidor SSH**  
Solo se permite el acceso SSH desde el puerto definido en el Sprint de Hardening SSH.

```bash
Pass - WAN - Destination: 10.0.3.XXX - Port: [PUERTO_CONFIGURADO_SSH] - Protocol: TCP
```
> **Nota:** Se puede configurar o consultar el puerto configurado en el Sprint 4 en /etc/ssh/sshd_config

---

## **4. Bloquear el tráfico directo entre servidores y empleados**  

```bash
Action: Block - Source: LAN_EMP - Destination: DMZ
Action: Block - Source: DMZ - Destination: LAN_EMP
```

---

## **5. Implementar regla para evitar exfiltración de información**  
Esta regla bloquea todo el tráfico en caso de un ciberataque.

```bash
Action: Block - Source: Any - Destination: Any - Protocol: Any
```
> **Nota:** Se debe probar la regla y dejarla deshabilitada hasta su uso en caso de emergencia. Hay que aprovechar el tiempo de desconexión para identificar las incidencias y reucirlas.

---

## **6. Implementar regla contra ataques DoS o Denegación de Servicios**  
Bloqueo de IP sospechosa en caso de detectar un ataque DoS.

```bash
Block - Source: [IP_DEL_ATACANTE] - Destination: 10.0.3.XX - Protocol: Any
```
> **Nota:** Podemos consultar los logs desde el panel de adminitración de pfSense desde nuestro navegador para rastrear anomalías.

---

## **7. Implementar regla extra**  
Regla personalizada para fortalecer la seguridad. Ejemplo:
· Reglas de VLAN para permitir tráfico entre VLANs
· Reglas de lista negra
· Redirección de DNS a tu firewall
· Regla para limitar el número de conexiones de una misma IP
· Habilitar pings externos desde ciertos sitios
· Permitir tráfico según un horario

_________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________________
# Bitácora de Seguridad en Entornos Empresariales y Alta Disponibilidad  
**¡LOS REGIS ORGANIZAN TU TRÁFICO DE RED!**  
![Descripción de la imagen](https://i.kym-cdn.com/photos/images/original/001/114/875/7a8.gif)

## Introducción
Este Sprint tiene como objetivo configurar un servidor **proxy inverso** con Apache, redirigiendo las solicitudes hacia un servidor backend, y configurar un **balanceador de carga** para distribuir las solicitudes entre tres servidores backend. Además, se habilitaron módulos específicos de Apache para hacer funcionar tanto el proxy como el balanceador de carga.

---

## Proceso de Actividades

| Tarea                                                   | Descripción                                                                                             | Completada |
|---------------------------------------------------------|---------------------------------------------------------------------------------------------------------|------------|
| **1. Creación de directorios para servidores backend**  | Crear los directorios correspondientes a los servidores backend.                                        | ✅         |
| **2. Activación de módulos de Apache para el proxy**    | Habilitar los módulos necesarios para que el proxy inverso funcione correctamente.                      | ✅         |
| **3. Configuración del proxy inverso**                  | Configurar Apache como proxy inverso que redirija las solicitudes hacia un servidor backend.            | ✅         |
| **4. Verificación del funcionamiento del proxy**        | Comprobar que el proxy inverso está funcionando correctamente.                                          | ✅         |
| **5. Activación de módulos de Apache para balanceador** | Habilitar los módulos necesarios para que el balanceador de carga funcione correctamente.               | ✅         |
| **6. Configuración del balanceador de carga**           | Configurar Apache como balanceador de carga, distribuyendo las solicitudes entre tres servidores backend. | ✅         |
| **7. Activación del Balancer Manager**                  | Habilitar y configurar el **Balancer Manager** para gestionar los servidores backend.                   | ✅         |
| **8. Verificación del balanceador de carga**             | Comprobar que el balanceador de carga distribuye las solicitudes correctamente entre los tres servidores backend. | ✅         |

---

## 1. Creación de directorios para servidores backend

Para crear los servidores backend, es necesario definir sus directorios de trabajo. Estos serán utilizados para almacenar los archivos de los servidores backend y luego se ejecutará un servidor Python en cada uno de ellos.

1. **Crear los directorios para los servidores backend**:
   ```bash
   sudo mkdir -p /var/www/servidor_final
   sudo mkdir -p /var/www/balancer/nodo_uno
   sudo mkdir -p /var/www/balancer/nodo_dos
   sudo mkdir -p /var/www/balancer/nodo_tres
   ```

2. **Iniciar los servidores Python en cada directorio**:
   ```bash
   # Servidores Backend
   cd /var/www/balancer/nodo_(...)
   nohup python3 -m http.server (puerto_deseado)
   ```

---

## 2. Activación de módulos de Apache para el proxy

Para que el proxy inverso funcione correctamente, debemos habilitar los módulos de proxy en Apache.

1. **Activar los módulos de proxy**:
   ```bash
   sudo a2enmod proxy
   sudo a2enmod proxy_http
   ```

2. **Reiniciar Apache para aplicar los cambios**:
   ```bash
   sudo systemctl restart apache2
   ```

---

## 3. Configuración del proxy inverso

Con los módulos habilitados, configuramos Apache para que actúe como un proxy inverso, redirigiendo las solicitudes hacia los servidores backend. A continuación, el archivo de configuración para **Apache VirtualHost**:

1. **Configurar el archivo de VirtualHost**:
   Edita el archivo de configuración de Apache (`/etc/apache2/sites-available/(...).conf`) y configura el proxy inverso.

   ```apache
   <VirtualHost *:80>
       ServerName proxy.tudominio.com

       <Proxy "balancer://ApellidoBalancer">
           BalancerMember http://127.0.0.1:(puerto_deseado)
           BalancerMember http://127.0.0.1:(puerto_deseado)
           BalancerMember http://127.0.0.1:(puerto_deseado) status=+H
       </Proxy>

       ProxyPreserveHost On
       ProxyPass "/" "balancer://ApellidoBalancer/"
       ProxyPassReverse "/" "balancer://ApellidoBalancer/"
   </VirtualHost>
   ```

2. **Reiniciar Apache**:
   ```bash
   sudo systemctl restart apache2
   ```

---

## 4. Verificación del funcionamiento del proxy

Para verificar que el proxy inverso está funcionando correctamente, se debe acceder desde un navegador a `proxy.sad.tomeucifre.com`. Si está configurado correctamente, las solicitudes se redirigirán al servidor backend adecuado.

---

## 5. Activación de módulos de Apache para balanceador

Para configurar el balanceador de carga, es necesario activar algunos módulos adicionales en Apache.

1. **Activar los módulos necesarios para el balanceador de carga**:
   ```bash
   sudo a2enmod proxy_balancer
   sudo a2enmod lbmethod_byrequests
   sudo a2enmod slotmem_shm
   sudo a2enmod proxy_connect
   ```

2. **Reiniciar Apache para aplicar los cambios**:
   ```bash
   sudo systemctl restart apache2
   ```

---

## 6. Configuración del balanceador de carga

Configuramos Apache como un balanceador de carga, para que distribuya las solicitudes entre los tres servidores backend definidos. Para ello, editamos nuevamente el archivo de configuración.

1. **Configurar el archivo de VirtualHost para el balanceo de carga**:
   ```apache
   <VirtualHost *:80>
       ServerName balancer.tudominio.com

       <Proxy "balancer://ApellidoBalancer">
           BalancerMember http://127.0.0.1:(puerto) loadfactor=3
           BalancerMember http://127.0.0.1:(puerto) loadfactor=1
           BalancerMember http://127.0.0.1:(puerto) status=+H
       </Proxy>

       ProxyPreserveHost On
       ProxyPass "/" "balancer://ApellidoBalancer/"
       ProxyPassReverse "/" "balancer://ApellidoBalancer/"

       <Location "/balancer-manager">
           SetHandler balancer-manager
           Require all granted
       </Location>
   </VirtualHost>
   ```

2. **Reiniciar Apache para aplicar los cambios**:
   ```bash
   sudo systemctl restart apache2
   ```

---

## 7. Activación del Balancer Manager

El **Balancer Manager** nos permite gestionar el estado de los servidores backend en el balanceador de carga.

1. **Habilitar el acceso al Balancer Manager**:
   Asegúrate de que el siguiente bloque esté configurado en el archivo de configuración de Apache:

   ```apache
   <Location "/balancer-manager">
       SetHandler balancer-manager
       Require all granted
   </Location>
   ```

2. **Reiniciar Apache para aplicar la configuración**:
   ```bash
   sudo systemctl restart apache2
   ```

---

## 8. Verificación del balanceador de carga

Para comprobar que el balanceador de carga funciona correctamente, accede a `balancer.sad.tomeucifre.com` desde un navegador. Las solicitudes deben distribuirse entre los tres servidores backend según el factor de carga.

---

**Pasos Finales**:  
1. Realizar pruebas de tolerancia a fallos para asegurar que el nodo "hot-standby" esté operando correctamente.
2. Continuar monitoreando y ajustando el balanceo de carga según convenga

---

**Fin de la Bitácora.**

---


