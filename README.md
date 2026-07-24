☁️ WordPress + Cloud SQL en Google Cloud — Lab

Documentación técnica de un laboratorio práctico de despliegue de WordPress sobre una VM de Compute Engine, conectado a una instancia administrada de Cloud SQL (MySQL 8.4), comparando dos arquitecturas de conexión: Cloud SQL Auth Proxy e IP Privada dentro de la misma VPC.

🎯 Objetivos del Laboratorio
Desplegar una aplicación web (WordPress) alojada en una instancia de Compute Engine (VM).
Conectar la aplicación a una base de datos administrada Cloud SQL (MySQL 8.4) llamada wordpress-db.
Comparar y poner en práctica dos arquitecturas de conexión: Cloud SQL Auth Proxy e IP Privada en la misma VPC.
🛠️ Stack y Herramientas
Categoría	Detalle
Plataforma	Google Cloud Platform (GCP)
Cómputo	Compute Engine (VM)
Base de datos	Cloud SQL — MySQL 8.4 (wordpress-db)
Aplicación	WordPress
Conexión segura	cloud-sql-proxy
Red	VPC — IP Privada
📋 Desarrollo Paso a Paso
1. Configuración Inicial de la Base de Datos
Se verificó el estado de la instancia administrada de Cloud SQL wordpress-db.
Se creó la base de datos wordpress dentro de Cloud SQL.
Se redefinió la contraseña del usuario administrador root.
2. Conexión 1 — Cloud SQL Auth Proxy (Acceso Seguro)
Se intentó conectar WordPress a la base de datos mediante un túnel seguro local, usando cloud-sql-proxy en el puerto 127.0.0.1:3306, desde una VM en Compute Engine.

Troubleshooting:

Se identificó un error 403 Forbidden al intentar usar comandos gcloud desde la terminal SSH de la VM, causado por una cuenta de servicio (service account) con permisos restringidos. Se resolvió administrando los accesos desde la consola web de GCP.
Se detectó que Cloud SQL no tenía IP pública habilitada, por lo que se configuró el proxy agregando la bandera --private-ip para forzar el enrutamiento interno y permitir el túnel seguro hacia la red privada.
3. Conexión 2 — IP Privada (Red VPC)
Se extrajo la IP privada interna (10.x.x.x) de la instancia de Cloud SQL.
Se configuró la aplicación WordPress para conectarse directamente a dicha IP, sin intermediación del proxy.
Se comprobó que esta arquitectura ofrece menor latencia, mayor rendimiento y máxima seguridad, ya que el tráfico de la base de datos permanece 100% dentro de la red privada de Google Cloud, sin exponerse a internet.
4. Instalación y Verificación de WordPress
Se ejecutó el instalador de 5 minutos de WordPress, configurando título del sitio, usuario administrador, contraseña y correo electrónico.
Se validó el funcionamiento del blog accediendo mediante la IP externa de la instancia.
5. Limpieza de Infraestructura (Cleanup)

Se eliminaron los recursos vía línea de comandos para evitar cobros no deseados:

bash
gcloud compute instances delete wordpress-proxy --zone=us-central1-a --quiet
gcloud sql instances delete wordpress-db --quiet

Se confirmó que el proyecto quedó limpio con:

bash
gcloud compute instances list
gcloud sql instances list
🎓 Conceptos Aprendidos
Cloud SQL Auth Proxy: ideal para conectar aplicaciones ubicadas en diferentes redes, regiones o cuentas, sin exponer credenciales ni conexiones sin cifrar.
IP Privada (VPC): la opción ideal para entornos de producción dentro de la misma región y red, ofreciendo la menor latencia posible y maximizando la seguridad del sistema.
Troubleshooting IAM: comprensión de las limitaciones de las cuentas de servicio predeterminadas (service accounts) en VMs de Compute Engine, y cómo gestionar permisos correctamente.
📌 Notas

Este laboratorio forma parte de mi preparación continua para la certificación Google Associate Cloud Engineer (ACE), con enfoque práctico en arquitecturas de conexión segura entre Compute Engine y Cloud SQL.

⬆ Español version above

☁️ WordPress + Cloud SQL on Google Cloud — Lab

Technical documentation of a hands-on lab deploying WordPress on a Compute Engine VM, connected to a managed Cloud SQL (MySQL 8.4) instance, comparing two connection architectures: Cloud SQL Auth Proxy and Private IP within the same VPC.

🎯 Lab Objectives
Deploy a web application (WordPress) hosted on a Compute Engine instance (VM).
Connect the application to a managed Cloud SQL database (MySQL 8.4) named wordpress-db.
Compare and implement two connection architectures: Cloud SQL Auth Proxy and Private IP within the same VPC.
🛠️ Stack and Tools
Category	Detail
Platform	Google Cloud Platform (GCP)
Compute	Compute Engine (VM)
Database	Cloud SQL — MySQL 8.4 (wordpress-db)
Application	WordPress
Secure connection	cloud-sql-proxy
Networking	VPC — Private IP
📋 Step-by-Step Walkthrough
1. Initial Database Configuration
Verified the status of the managed Cloud SQL instance wordpress-db.
Created the wordpress database within Cloud SQL.
Reset the password for the root admin user.
2. Connection 1 — Cloud SQL Auth Proxy (Secure Access)
Attempted to connect WordPress to the database through a secure local tunnel, using cloud-sql-proxy on port 127.0.0.1:3306, from a Compute Engine VM.

Troubleshooting:

Identified a 403 Forbidden error when trying to run gcloud commands from the VM's SSH terminal, caused by a service account with restricted permissions. Resolved by managing access directly through the GCP web console.
Detected that Cloud SQL had no public IP enabled, so the proxy was configured with the --private-ip flag to force internal routing and enable the secure tunnel to the private network.
3. Connection 2 — Private IP (VPC Network)
Retrieved the internal private IP (10.x.x.x) of the Cloud SQL instance.
Configured the WordPress application to connect directly to that IP, without proxy intermediation.
Verified that this architecture provides lower latency, higher performance, and maximum security, since database traffic stays 100% within Google Cloud's private network, without being exposed to the internet.
4. WordPress Installation and Verification
Ran the WordPress 5-minute installer, configuring site title, admin user, password, and email.
Validated the blog's functionality by accessing it via the instance's external IP.
5. Infrastructure Cleanup

Deleted resources via command line to avoid unwanted charges:

bash
gcloud compute instances delete wordpress-proxy --zone=us-central1-a --quiet
gcloud sql instances delete wordpress-db --quiet

Confirmed the project was fully cleaned up with:

bash
gcloud compute instances list
gcloud sql instances list
🎓 Key Concepts Learned
Cloud SQL Auth Proxy: ideal for connecting applications across different networks, regions, or accounts, without exposing credentials or unencrypted connections.
Private IP (VPC): the ideal option for production environments within the same region and network, offering the lowest possible latency and maximizing system security.
IAM Troubleshooting: understanding the limitations of default service accounts on Compute Engine VMs, and how to manage permissions correctly.
📌 Notes

This lab is part of my ongoing preparation for the Google Associate Cloud Engineer (ACE) certification, with a hands-on focus on secure connection architectures between Compute Engine and Cloud SQL.
