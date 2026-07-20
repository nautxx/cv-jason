# nautsfw

Personal portfolio & CV site for Jason Barnachea — software engineer and multi-agent systems builder based in Santa Monica, California. Built with plain HTML/CSS/JS, no build step, no framework.

Live sections: Experience, Projects, Education & Certifications, Skills & Stack, and Contact, with a canvas-based animated hero and light/dark theme support.

## Stack

- Static HTML (`index.html`, `privacy.html`)
- Vanilla CSS (`static/css/styles.css`)
- Vanilla JS (`static/js/main.js`)
- Self-hosted fonts (`static/fonts/`)

## Running locally

Any static file server works, e.g.:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080`.

### Docker

```bash
docker compose up --build
```

Serves the site via nginx at `http://localhost:8077`.

## Project structure

```
index.html          Main portfolio page
privacy.html         Privacy policy page
static/
  css/styles.css      Styles
  js/main.js          Interactivity (theme toggle, hero animation, nav)
  fonts/              Self-hosted fonts
  img/                Images & icons
```

## License

MIT — see [LICENSE](LICENSE).
