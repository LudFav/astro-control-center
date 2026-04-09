# Astro Mobile Proxy (Termux/Node)

Petit proxy HTTP pour relayer les appels de l'app web vers OnStepX et contourner les blocages navigateur (CORS/PNA/mixed-content).

## 1) Installation Android (Termux)

```bash
pkg update -y
pkg install -y nodejs
mkdir -p ~/astro-mobile-proxy
cd ~/astro-mobile-proxy
```

Copier `server.js` et `package.json` dans ce dossier.

## 2) Lancement (mode securise recommande)

```bash
cd ~/astro-mobile-proxy
ONSTEP_BASE="http://192.168.0.1" PORT=8787 API_TOKEN="change-moi-token-fort" BIND_HOST="127.0.0.1" node server.js
```

## 3) URL a saisir dans l'app

Dans la configuration de l'app Astro Control Center, renseigner:

`Proxy API URL` = `http://IP_DU_TELEPHONE:8787`
`Proxy API Token` = `change-moi-token-fort`

Exemple: `http://192.168.0.23:8787`

## 4) Test rapide

Depuis un navigateur:

`http://IP_DU_TELEPHONE:8787/health`

Doit renvoyer JSON avec `ok: true`.

## 5) Auto-start (optionnel)

Avec Termux:Boot, creer `~/.termux/boot/start-astro.sh`:

```bash
#!/data/data/com.termux/files/usr/bin/bash
cd ~/astro-mobile-proxy
export ONSTEP_BASE="http://192.168.0.1"
export PORT=8787
export API_TOKEN="change-moi-token-fort"
export BIND_HOST="127.0.0.1"
node server.js
```

Rendre executable:

```bash
chmod +x ~/.termux/boot/start-astro.sh
```

Ensuite le proxy demarre automatiquement au boot (selon restrictions Android constructeur).

## Notes securite

- Par defaut, le serveur ecoute sur `127.0.0.1` (plus sur tout le reseau).
- Pour autoriser un autre appareil du LAN, changer `BIND_HOST="0.0.0.0"` et garder un `API_TOKEN` fort.
- Ne jamais exposer ce proxy directement sur Internet.
