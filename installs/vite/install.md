 Vite + React Daily Runbook
 First-time setup
cd "/path/to/your-vite-react-app"
npm install
Start app (development)
npm run dev
Default URL:
- http://localhost:5173
Start app with Docker (if your project uses compose)
cd "/path/to/your-vite-react-app"
docker compose up -d frontend
Verify app is running
# local dev
curl -I http://localhost:5173
# docker (if mapped differently, use your port)
docker compose ps
docker logs --tail=120 frontend
Stop app
# local dev: press Ctrl+C in terminal
# docker:
docker compose stop frontend
Restart clean (when something is broken)
# local dev
rm -rf node_modules package-lock.json
npm install
npm run dev
# docker
docker compose up -d --force-recreate frontend
Production build check
npm run build
npm run preview
Preview URL:
- http://localhost:4173