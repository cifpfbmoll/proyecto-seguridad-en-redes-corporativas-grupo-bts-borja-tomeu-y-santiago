
# Bitácora de Auditoría CIS Level 2 - Ubuntu Pro

## **Introducción**
El objetivo de esta auditoría es asegurar que un sistema Ubuntu cumpla con las pautas de seguridad del nivel **CIS Level 2 Server**. Para ello, seguiremos un proceso de auditoría que incluye generar un fichero de configuración `tailor.xml`, modificar ciertas configuraciones del sistema (como deshabilitar el firewall UFW y el GDM), y realizar auditorías antes y después de los cambios para medir la evolución de la seguridad.

## **Proceso de Auditoría**

### 1. **Instalación del Sistema**
- Instalamos **Ubuntu** en una máquina virtual.
- Activamos **Ubuntu Pro** desde la consola con el comando `pro attach`.
- Activamos el **USG** (Ubuntu Security Guide). Si Ubuntu Pro está activado, esta operación se realiza automáticamente.
- Comprobamos la correcta instalación con comandos como `screenshot` para verificar los pasos.

### 2. **Desactivación del Cortafuegos (UFW)**
Antes de realizar la primera auditoría, deshabilitamos el cortafuegos para facilitar la auditoría:
```bash
sudo ufw disable
```

### 3. **Primera Auditoría**
Realizamos la primera auditoría usando el perfil de **CIS Level 2 Server**:
```bash
sudo usg audit cis_level2_server
```
Los resultados de la auditoría inicial se pueden ver tanto en un archivo HTML como en formato XML. Movemos el informe HTML al escritorio para una fácil visualización:
```bash
sudo cp /var/lib/usg/usg-report-20241020.0115.html ~/Escritorio/
```

### 4. **Modificación del Tailor File**
Creamos un archivo `tailor.xml` para realizar cambios personalizados, como deshabilitar las reglas relacionadas con el **firewall (UFW)** y el **GDM** (gestor gráfico GNOME):
```bash
sudo usg generate-tailoring cis_level2_server tailor.xml
```
Desactivamos todas las configuraciones relacionadas con **UFW** y **GDM** en el archivo `tailor.xml` y hacemos un fix de las configuraciones:
```bash
sudo usg fix --tailoring-file tailor.xml
```

### 5. **Reinicio del Sistema**
Después de modificar y aplicar el `tailor.xml`, es necesario reiniciar el sistema para aplicar los cambios:
```bash
sudo reboot
```

### 6. **Reauditoría del Sistema**
Una vez que hemos aplicado los cambios y reiniciado, volvemos a auditar el sistema:
```bash
sudo usg audit cis_level2_server
```
Copiamos el nuevo informe de auditoría y comparamos los resultados con la primera auditoría para ver la evolución.

### 7. **Configuración de Seguridad Adicional**
Accedemos al terminal con `CTRL + ALT + F3` y realizamos ajustes de seguridad adicionales como configurar el arranque de GRUB:
- Creamos una contraseña de inicio para **GRUB**:
```bash
grub-mkpasswd-pbkdf2
```
- Establecemos permisos estrictos para el archivo de configuración de GRUB:
```bash
chmod 400 /boot/grub/grub.cfg
```

### 8. **Creación de Usuarios**
Creamos un nuevo usuario llamado `admtcifre` y lo agregamos al grupo de **sudoers**:
```bash
adduser admtcifre
usermod -aG sudo admtcifre
```

### 9. **Actualización de Software**
Revisamos las actualizaciones de software y ajustamos las fuentes de paquetes en `sources.list` para que cumplan con las directrices de **INCIBE**. Comentamos y descomentamos las líneas correspondientes:
```bash
nano /etc/apt/sources.list
```

## **Previsión de Tareas**

| Tarea | Descripción | Completada |
|-------|-------------|------------|
| **1. Instalación del Sistema** | Instalación de Ubuntu y activación de Ubuntu Pro. | ✅ |
| **2. Desactivación del Cortafuegos** | Deshabilitar UFW antes de la auditoría. | ✅ |
| **3. Primera Auditoría** | Realizar la auditoría inicial usando el perfil CIS Level 2. | ✅ |
| **4. Modificación del Tailor File** | Deshabilitar UFW y GDM en el archivo tailor.xml. | ✅ |
| **5. Reinicio del Sistema** | Reiniciar el sistema después de los cambios. | ✅ |
| **6. Reauditoría del Sistema** | Realizar una auditoría después de aplicar las modificaciones. | ✅ |
| **7. Configuración de Seguridad Adicional** | Configuración de GRUB, creación de contraseña y permisos de arranque. | ✅ |
| **8. Creación de Usuarios** | Crear un nuevo usuario y asignarle privilegios sudo. | ✅ |
| **9. Actualización de Software** | Ajuste de `sources.list` para cumplir con las pautas de INCIBE. | ✅ |

## **Conclusiones**
A lo largo del proceso de auditoría y configuración, hemos podido observar una evolución significativa en el nivel de seguridad del sistema. A partir de la auditoría inicial y las modificaciones aplicadas mediante el archivo `tailor.xml`, el sistema pasó de un cumplimiento del **44.30%** en la primera auditoría a un cumplimiento significativamente mayor en las auditoría finales, del **95%**. Las configuraciones adicionales de GRUB y la gestión de usuarios han fortalecido aún más la seguridad del sistema.
