# Reporte de Auditoría: Seguridad en Redes Wi-Fi Públicas

## 1. Introducción
Este informe analiza los riesgos de seguridad asociados al tráfico de datos al conectarse a redes Wi-Fi públicas no cifradas. Se documenta la vulnerabilidad de las conexiones HTTP mediante la inspección de tráfico en tiempo real y se analiza cómo la implementación de una VPN mitiga estas amenazas.

---

## 2. Exploración Práctica y Sitio Analizado
Se realizó un análisis de tráfico accediendo al sitio de prueba **neverssl.com** utilizando las Herramientas de Desarrollador (*DevTools - Pestaña Network*) y analizador de paquetes Wireshark.

- **URL Objetiva:** `http://neverssl.com`
- **Método HTTP:** `GET`
- **Host:** `neverssl.com`
- **Protocolo de Red:** `HTTP/1.1` (Puerto 80 - No seguro, sin cifrado TLS/SSL)
- **Estado de Respuesta:** `200 OK`

---

## 3. Cuestionario de Análisis Técnico

### ¿Qué protocolo utiliza el sitio?
El sitio utiliza exclusivamente el protocolo **HTTP** (*Hypertext Transfer Protocol*) sobre el puerto predeterminado 80. No utiliza **HTTPS** (*HTTP Secure*), lo que significa que la comunicación no cuenta con certificados digitales X.509 ni mecanismos de cifrado simétrico/asimétrico (TLS/SSL).

### ¿Qué información puede observarse durante la solicitud?
Dado que los datos viajan en texto plano, cualquier analizador de red (sniffing) dentro de la misma Wi-Fi pública puede capturar:
1. **Host y URL Completa:** `neverssl.com` y la estructura de directorios solicitada.
2. **Método HTTP:** Solicitud `GET /` pidiendo el documento HTML principal.
3. **Headers y User-Agent:** La cadena completa del navegador, sistema operativo, lenguajes aceptados y cookies de sesión de haber estado presentes.

### ¿Qué riesgos existen al navegar mediante HTTP desde una Wi-Fi pública?
- **Sniffing / Captura de Credenciales:** Las contraseñas y datos ingresados en formularios viajan sin cifrar y son visibles para cualquier atacante en la red.
- **Secuestro de Sesión (Session Hijacking):** La intercepción de tokens de autenticación o cookies HTTP permite suplantar la identidad del usuario.
- **Inyección de Contenido (MitM):** Un atacante mediante envenenamiento ARP (ARP Spoofing) puede alterar los archivos transferidos e insertar scripts maliciosos.

### ¿Cómo cambiaría este escenario utilizando una VPN?
Al activar una **VPN** (*Virtual Private Network*), la seguridad se transforma mediante tres pilares:
- **Cifrado:** Toda la información saliente del dispositivo se transforma en código ilegible mediante algoritmos robustos (ej. AES-256).
- **Túnel Seguro:** Se establece un **túnel** blindado entre el dispositivo y el servidor VPN remoto mediante el **encapsulamiento** de los paquetes de datos.
- **Invisibilidad y Privacidad:** Un atacante en la Wi-Fi pública solo observará tráfico cifrado indescifrable dirigido hacia la IP del servidor VPN, protegiendo la confidencialidad total de la navegación.

---

## 4. Las 3 Reglas de Oro para Wi-Fi Públicas

1. **Usar siempre una VPN activa:** Encapsular y cifrar todo el tráfico mediante un **túnel** seguro con **cifrado** de extremo a extremo antes de transmitir cualquier dato por el aire.
2. **Forzar conexiones HTTPS:** Navegar únicamente por sitios con certificado SSL/TLS válido (candado de seguridad) y evitar el ingreso de credenciales en páginas HTTP.
3. **Desactivar conexiones automáticas y uso compartido:** Inhabilitar la reconexión automática a redes abiertas, apagar el intercambio de archivos e impresoras, y desactivar el Bluetooth mientras se esté en redes públicas.
