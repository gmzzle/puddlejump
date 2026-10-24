# Puddlejump

A kids' learning PWA with Juniper the frog. Static files only: no backend, no accounts, no tracking. Progress is saved in each device's browser.

## Activities
| Activity | Little hops (4–6) | Big hops (7–9) |
|---|---|---|
| Letter Lagoon | Hear a letter, pick the picture that starts with it | Also: see a picture, pick its first letter |
| Spelling Splash | 3-letter words | 4–7 letter words plus decoy letters |
| Number Ripples | Counting to 10, simple adding | +, −, × (times tables to 10) |
| Shape Shore | "Tap the blue triangle" | Tricky look-alike choices |
| Listen & Find | Hear a word, find the picture | Read the word, find the picture |
| Memory Pond | 6 pairs | 8 pairs |

Each round is 6 questions. A star for every first-try answer, and a new sticker every 5 stars (24 stickers). The lock button opens the Grown-ups area behind an addition question: rename players, set levels, see accuracy, voice speed, reset.

## Deploy (homelab)
Service workers need HTTPS, so serve it behind your proxy (e.g. `kids.morgannet.ca` with the wildcard cert).

**Docker / Portainer**
```yaml
services:
  puddlejump:
    image: nginx:alpine
    container_name: puddlejump
    restart: unless-stopped
    ports: ["8088:80"]
    volumes:
      - ./puddlejump:/usr/share/nginx/html:ro
```
Copy the contents of this folder into `./puddlejump`, then point a Nginx Proxy Manager host at port 8088.

**Caddy** (static, no container)
```
kids.morgannet.ca {
  root * /srv/puddlejump
  file_server
  header /sw.js Cache-Control "no-cache"
}
```

## Install on tablets
Open the site, then **Share → Add to Home Screen** (iPad) or **⋮ → Install app** (Android/Chrome). After the first load it works offline.

## Updating
Edit files, then bump `VERSION` in `sw.js` so devices pick up the new copy.

Voice uses the device's built-in text-to-speech, so it sounds best on iPad/Android with an English voice installed.
