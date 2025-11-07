#  Proyecto Docker  
**Odoo, Página Web, Proxy Inverso**

---

##  Página Web

Se va a la página: [https://html5up.net/](https://html5up.net/)  
Descarga la plantilla que más te guste.  
Una vez descargado el `.zip`, descomprímelo y busca el archivo **index.html**.

Luego, abrimos **Visual Studio Code** para modificar el código.  
Es HTML básico y fácil de entender.  
Podemos cambiar nombres, textos y colores según lo que necesitemos.  
Finalmente, se puede montar en el puerto elegido y acceder con:

```
http://localhost:PUERTO
```

---

##  Página Odoo

**Odoo** es una plataforma que permite administrar páginas y gestionar sistemas de forma sencilla.  
Más información en la documentación oficial:  
[Guía Odoo Docker](https://www.odoo.com/es/forum/ayuda-1/how-to-set-up-odoo-community-on-docker-250199)

---

##  Proxy Inverso

El **proxy inverso** refuerza la seguridad del sistema y simplifica el acceso a las aplicaciones.  
Sirve como capa intermedia entre los usuarios y los servicios internos.

---

##  docker-compose.yml

Este archivo contiene el código que lanza **todos los servicios desde cero**:  
- Página web  
- Odoo  
- Proxy inverso  

Permite montar todo el sistema y ejecutarlo automáticamente.

---

##  Esquema de Carpetas

```
nginx-proxy-odoo/
│
├── docker-compose.yml          # Definición de todos los servicios (NPM, Odoo, HTML, DB)
│
├── npm/                        # Datos persistentes del Nginx Proxy Manager
│   ├── data/                   # Configuración de NPM
│   └── letsencrypt/            # Certificados SSL
│
├── html-app/                   # Carpeta de tu aplicación web HTML (Sitio A)
│   └── index.html              # Archivo HTML de prueba
│
└── odoo/                       # Configuración y datos de Odoo
    ├── config/                 # Archivos de configuración
    ├── data/                   # Datos de Odoo (filestore)
    └── db/                     # Datos de la base de datos PostgreSQL
```

---

##  Puertos Utilizados

| Aplicación | Hostname / IP | Puerto Interno |
|-------------|----------------|----------------|
| HTML        | app-html       | 80             |
| Odoo        | odoo           | 8069           |

---

##  Esquema Explicación

Se crea una carpeta donde se guarda **todo el sistema**.  
Desde **Visual Studio Code**, se carga el esquema completo.  
Una vez configurado, todo queda centralizado y listo para ejecutarse fácilmente.

---

##  Acceso a la Consola de Nginx Proxy Manager (NPM)

| Detalle | Valor |
|----------|--------|
| **URL de Acceso** | `http://TU_IP_SERVIDOR:81` |
| **Propósito** | Configurar y gestionar hosts y certificados SSL |
| **Usuario Inicial** | `admin@example.com` |
| **Contraseña Inicial** | `changeme` |
| **Acción Requerida** | Cambiar correo y contraseña al iniciar sesión |

---

##  Checar IP en Windows

Abre el **CMD** y ejecuta:

```
ipconfig
```

Ejemplo de salida:

```
Sufijo DNS específico para la conexión. . :
Vínculo: dirección IPv6 local. . . : fe80::fdc4:c9ba:fdf4:c5a0%19
Dirección IPv4. . . . . . . . . . . . : 172.18.224.1
Máscara de subred . . . . . . . . . . : 255.255.240.0
Puerta de enlace predeterminada . . . :
```

>  La IP puede variar según la red a la que estés conectado.

---

##  Configuración de los Proxy Hosts en NPM

| Aplicación | Domain Name | Hostname / IP | Puerto |
|-------------|-------------|---------------|--------|
| HTML App | app-html.lan | app-html | 80 |
| Odoo | odoo.lan | odoo | 8069 |

---

##  Comandos Útiles de Docker

**Iniciar Docker Compose:**
```bash
docker-compose up -d
```

**Detener y eliminar contenedores actuales:**
```bash
docker-compose down
```

**Verificar estado de contenedores:**
```bash
docker-compose ps
```

**Ver logs de Odoo y DB:**
```bash
docker-compose logs odoo-app
docker-compose logs odoo-db
```

**Detener contenedor app-html:**
```bash
docker-compose stop app-html
```

**Dar permisos a la carpeta HTML:**
```bash
chmod -R 755 html-app
```

**Reiniciar contenedor:**
```bash
docker-compose start app-html
```

**Eliminar todo (contenedores, redes, volúmenes):**
```bash
docker-compose down -v
```

**Revisar que no queden contenedores:**
```bash
docker ps -a
```

**Reconstruir e iniciar servicios:**
```bash
docker-compose up -d --build
```

---

##  Configuración de Proxy Hosts (Redirecciones)

### Para la Aplicación HTML (Puerto 80)
| Campo | Valor |
|--------|--------|
| Domain Names | app-html.lan |
| Scheme | http |
| Forward Hostname / IP | app-html |
| Forward Port | 80 |

### Para Odoo (Puerto 8069)
| Campo | Valor |
|--------|--------|
| Domain Names | odoo.lan |
| Scheme | http |
| Forward Hostname / IP | odoo |
| Forward Port | 8069 |

---

##  Acceso Final a las Aplicaciones

Editar el archivo **hosts** según el sistema operativo:

- **Windows:** `C:\Windows\System32\drivers\etc\hosts`  
- **Linux/Mac:** `/etc/hosts`

Agregar las siguientes líneas:

```
127.0.0.1    app-html.lan
127.0.0.1    odoo.lan
```

Para otros equipos en la red:
```
IP_DEL_SERVIDOR    app-html.lan
IP_DEL_SERVIDOR    odoo.lan
```

---

##  Error 502 Bad Gateway

El error **502 Bad Gateway** indica un problema de conexión entre **Nginx Proxy Manager (NPM)** y el contenedor objetivo.

### Diagnóstico del Estado de Odoo

**Verificar estado:**
```bash
docker-compose ps
```

**Ver logs de Odoo:**
```bash
docker-compose logs odoo
```

---

 **Consejo:**  
Si todo está configurado correctamente, podrás acceder a tus servicios de forma segura, aislada y totalmente automatizada usando **Docker + Nginx Proxy Manager + Odoo**.

---

**Autor:** Carlos Eduardo Gutierrez Aguilar

 Proyecto Docker - Sistema Integrado de Aplicaciones Web
