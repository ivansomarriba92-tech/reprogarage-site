# ReproGarage — Deploy-ready assets

Generados:
- `reprogarage_ready_for_deploy_seo_emailjs.html` — HTML listo para deploy con SEO y JSON-LD.
- `sitemap.xml` — Sitemap básico.
- `robots.txt` — Archivo robots que permite indexación y apunta al sitemap.

Fecha: 2025-10-17

---

## EmailJS — Plantillas recomendadas y cómo configurarlas

Recomendado crear dos plantillas en EmailJS: `template_reserva` y `template_reprog`.

### 1) template_reserva
Campos de plantilla (ejemplo):
- `user_name` — Nombre del cliente
- `user_email` — Correo del cliente
- `user_phone` — Teléfono
- `vehicle_make` — Marca del vehículo
- `vehicle_model` — Modelo
- `preferred_date` — Fecha preferida
- `notes` — Notas adicionales

HTML / Texto de ejemplo para Template:

Asunto: Nueva reserva — {{user_name}}

Cuerpo (HTML):

<p>Nueva reserva desde la web ReproGarage</p>
<ul>
  <li><strong>Nombre:</strong> {{user_name}}</li>
  <li><strong>Email:</strong> {{user_email}}</li>
  <li><strong>Teléfono:</strong> {{user_phone}}</li>
  <li><strong>Marca:</strong> {{vehicle_make}}</li>
  <li><strong>Modelo:</strong> {{vehicle_model}}</li>
  <li><strong>Fecha preferida:</strong> {{preferred_date}}</li>
  <li><strong>Notas:</strong> {{notes}}</li>
</ul>

### 2) template_reprog
Campos de plantilla (ejemplo):
- `user_name`, `user_email`, `user_phone`
- `vehicle_make`, `vehicle_model`, `engine_code`
- `current_power`, `desired_power`
- `additional_info`

Asunto: Solicitud de reprogramación — {{user_name}}

Cuerpo (HTML):

<p>Solicitud de reprogramación desde la web</p>
<ul>
  <li><strong>Nombre:</strong> {{user_name}}</li>
  <li><strong>Email:</strong> {{user_email}}</li>
  <li><strong>Teléfono:</strong> {{user_phone}}</li>
  <li><strong>Marca/Modelo:</strong> {{vehicle_make}} / {{vehicle_model}}</li>
  <li><strong>Código motor:</strong> {{engine_code}}</li>
  <li><strong>Potencia actual:</strong> {{current_power}}</li>
  <li><strong>Potencia deseada:</strong> {{desired_power}}</li>
  <li><strong>Información adicional:</strong> {{additional_info}}</li>
</ul>

### Configuración rápida de EmailJS en cliente (ejemplo)
1. Registra una cuenta en https://www.emailjs.com/ y conecta un servicio (Gmail, SMTP, etc).
2. Crea las plantillas con los `template IDs` mencionados (`template_reserva` y `template_reprog`).
3. En tu HTML cliente, inicializa EmailJS con tu `user ID` (no subas la clave a repositorio público).
4. Ejemplo de envío (usar desde el cliente, el archivo ya intenta hacer envíos si EmailJS está presente):

```js
emailjs.init('YOUR_EMAILJS_USER_ID'); // preferiblemente inyectado vía entorno
const params = {
  user_name: 'Juan',
  user_email: 'juan@example.com',
  user_phone: '+34 600 000 000',
  vehicle_make: 'BMW',
  vehicle_model: 'M3',
  preferred_date: '2025-11-01',
  notes: 'Quiero Stage 2'
};
emailjs.send('default_service', 'template_reserva', params)
  .then(resp => console.log('EmailJS enviado', resp))
  .catch(err => console.error('Error EmailJS', err));
```

Seguridad y buenas prácticas
- Evita publicar tu `user ID` o `service key` en repositorios públicos. Usa GitHub Secrets o variables de entorno en el servidor y realiza envíos desde un endpoint seguro si necesitas mantener claves privadas.
- EmailJS user ID no es totalmente secreto (se usa en cliente), pero evita exponer credenciales de SMTP o claves privadas directamente.

---

## SEO y despliegue rápido
- Sube `reprogarage_ready_for_deploy_seo_emailjs.html` como `index.html` a tu hosting (Netlify, Vercel, GitHub Pages, tu servidor).
- Asegúrate de servir el `sitemap.xml` desde la raíz (ej. https://reprogarage.es/sitemap.xml).
- Añade `robots.txt` en la raíz.
- Añade verificación de Google Search Console usando el token en `meta name="google-site-verification"`.

---

Si quieres, puedo:
- Separar ahora estos cambios en archivos listos para commit (index.html, sitemap.xml, robots.txt, README.md) y crear un ZIP/estructura para descargar y subir a GitHub.
- Preparar un `deploy` con GitHub Actions que publique en GitHub Pages o en un bucket S3 (dímelo y lo preparo).
