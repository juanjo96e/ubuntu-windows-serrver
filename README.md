# 🔗 Unión de Ubuntu a Dominio Windows Server

Este repositorio documenta paso a paso cómo unir un sistema operativo **Ubuntu** a un dominio de **Active Directory** en un servidor **Windows Server**.  
El objetivo es permitir la autenticación de usuarios de dominio en Ubuntu, integrando sistemas Linux y Windows en una misma red corporativa.

---

## 🧩 Requisitos Previos

### En Windows Server
- Tener instalado y configurado el rol **Active Directory Domain Services (AD DS)**.
- Crear un usuario en el dominio (por ejemplo: `pepeUbuntu`).
- Compartir una carpeta accesible solo para dicho usuario (opcional para pruebas de autenticación).

### En Ubuntu
- Conectividad de red con el servidor Windows.
- Acceso con permisos de superusuario (`sudo`).
- Sistema actualizado.

---

## ⚙️ Pasos de Configuración

### 1. Configuración en Windows Server
- Verifica que AD DS esté activo.
- Crea el usuario en el AD.
- Comparte recursos para validación (opcional).

### 2. Configuración en Ubuntu

#### a. Editar `/etc/hosts`
Relaciona la IP del servidor de dominio con su nombre:

```bash
sudo nano /etc/hosts
# Ejemplo:
192.168.1.100 EQUIPOJUANJO.juanjo.server
```

#### b. Verificar conexión

```bash
ping EQUIPOJUANJO.juanjo.server
```

#### c. Actualizar sistema

```bash
sudo apt update && sudo apt upgrade -y
```

#### d. Instalar paquetes necesarios

```bash
sudo apt install realmd sssd sssd-tools adcli samba-common krb5-user packagekit -y
```

Durante la instalación se te solicitará:
- Nombre del dominio (ej: `juanjo.server`)
- Nombre del servidor (ej: `EQUIPOJUANJO`)
- Usuario administrativo del dominio

#### e. Unir Ubuntu al dominio

```bash
sudo realm join --user=Administrador juanjo.server
```

Reemplaza `Administrador` y `juanjo.server` por los valores correspondientes a tu entorno.

#### f. Verificar unión al dominio

```bash
id pepeUbuntu@juanjo.server
```

---

## ✅ Resultado Esperado

- Ubuntu unido exitosamente al dominio.
- Autenticación de usuarios de dominio posible desde Ubuntu.
- Acceso controlado a recursos compartidos desde políticas del AD.

---

## 🛠️ Solución de Problemas

- Verifica la sincronización horaria entre Ubuntu y Windows Server.
- Asegúrate de que el firewall no esté bloqueando puertos clave (ej. LDAP, Kerberos).
- Consulta logs útiles:
  - `/var/log/auth.log`
  - `/var/log/sssd/`
