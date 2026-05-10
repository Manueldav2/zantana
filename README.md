# Zantana

A Quiplash-style party game you can play in any browser. Host on a big screen, friends join from their phones with a 4-letter room code. No accounts, no installs, no backend to deploy.

**Built with:** vanilla HTML/CSS/JS · MQTT over WebSocket (free public broker) · ~1k lines, one file.

## How to play

1. One person opens the site and clicks **Host on Big Screen** — a 4-letter room code appears, plus a QR code.
2. Other players open the site on their phones (or scan the QR), enter the code, and pick a name.
3. Each round, everyone gets two silly prompts and types the funniest answer they can.
4. Two answers per prompt go head-to-head and everyone else votes on the funnier one.
5. After 3 rounds the highest score wins.

3–8 players · about 10 minutes per game · age 16+ for the prompts.

### Solo testing

Click **+ Add test bot** in the host lobby to fill the room with auto-playing bots. Useful for trying it out without rounding up friends.

## Run it locally

```sh
git clone https://github.com/Manueldav2/zantana.git
cd zantana
python3 -m http.server 8765
```

Open `http://localhost:8765` on the host machine. For other devices on the same wifi, share `http://<your-lan-ip>:8765`.

## Deploy

It's a single static file — drop `index.html` on any static host:

- **Cloudflare Pages / Netlify / Vercel** — drag-and-drop or connect this repo
- **Firebase Hosting** — `firebase init hosting && firebase deploy`
- **Public tunnel from localhost** — `cloudflared tunnel --url http://localhost:8765`

## How it works

There is no backend. The host's browser tab holds authoritative game state. All players join the same MQTT topic on a free public broker (`broker.emqx.io`) and exchange messages:

- Host publishes game state to `zantana2026/<CODE>/state` (retained, so late joiners catch up immediately).
- Players publish commands (`hello`, `answer`, `vote`) to `zantana2026/<CODE>/cmd`.

That's it. Anyone with the code is in the room — fine for a party game, don't use this to share secrets.

## License

MIT — see [LICENSE](LICENSE).
