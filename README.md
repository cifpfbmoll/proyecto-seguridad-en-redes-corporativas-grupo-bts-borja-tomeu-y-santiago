
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
# Bitácora de Copias de Seguridad con Duplicati - Ubuntu y Duplicati  
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

---
