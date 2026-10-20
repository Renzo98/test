# Simulador AI-901

Simulador de examen con 727 preguntas validadas. Es una sola página estática: `index.html` trae las preguntas incluidas y no necesita servidor ni base de datos.

## Probarlo en tu computadora
Haz doble clic en `index.html`, o sírvelo localmente:

```bash
python -m http.server 8000
# abre http://localhost:8000
```

## Publicarlo en internet (elige una opción)

### Opción A: Azure Storage, sitio web estático
```bash
az storage account create -n <cuenta> -g <grupo> -l eastus --sku Standard_LRS --kind StorageV2
az storage blob service-properties update --account-name <cuenta> --static-website --index-document index.html
az storage blob upload -c '$web' --account-name <cuenta> -f index.html -n index.html --content-type "text/html; charset=utf-8" --overwrite
az storage account show -n <cuenta> -g <grupo> --query "primaryEndpoints.web" -o tsv
```

### Opción B: Azure Static Web Apps (plan gratuito)
Sube esta carpeta a un repositorio de GitHub y crea un Static Web App apuntando a ella (app location `/`, sin build). O, con la CLI:
```bash
npm i -g @azure/static-web-apps-cli
swa deploy . --env production
```

### Opción C: GitHub Pages
Sube `index.html` a un repositorio y activa Settings → Pages → Deploy from branch (`main`, carpeta raíz).

### Opción D: Netlify Drop
Arrastra la carpeta a https://app.netlify.com/drop

## Cómo se guarda el progreso
- Siempre se guarda en el navegador (`localStorage`).
- **Sincronizar entre dispositivos:** en la pantalla de inicio, sección *Guardar tu progreso*, conecta un token de GitHub con permiso `gist`. El progreso se guarda en un gist privado tuyo (`progreso-ai901.json`) y se combina automáticamente en cada dispositivo donde conectes el mismo token.
- Sin GitHub, usa **Exportar progreso** e **Importar progreso**.

## Traducción
El botón **Ver traducción** traduce el enunciado y las alternativas en la misma página. Usa el servicio web de Google Translate y, si falla, MyMemory. Cada traducción queda guardada en el navegador. Necesita internet.

## Actualizar las preguntas
Las preguntas están dentro de `index.html`, en `<script id="bank" type="application/json">`. Su formato es:
`{"cats":[...], "topics":[...], "q":[[id, índiceCategoría, índiceTema, enunciado, [opción A..E], [índices correctos], explicación], ...]}`
