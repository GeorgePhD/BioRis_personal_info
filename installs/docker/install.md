BioRis Docker Daily Runbook
 Start (only `cuidate-2`)
cd "/mnt/storage/My_Documents/Projects/BioRis/cuidate_files"
docker compose up -d cuidate2
Verify it's running
docker compose ps cuidate2
curl -I http://localhost:7000
Expected response:
- HTTP/1.1 302 Found
- Redirect to login.php (this is normal for this app)
Open the app
- http://localhost:7000/login.php
View logs (when needed)
docker logs --tail=120 cuidate-2
Stop at end of day
cd "/mnt/storage/My_Documents/Projects/BioRis/cuidate_files"
docker compose stop cuidate2
Restart if something looks broken
cd "/mnt/storage/My_Documents/Projects/BioRis/cuidate_files"
docker compose up -d --force-recreate cuidate2
Notes
- The warning about version in docker-compose.yaml is non-blocking and can be ignored.
- This setup runs only the cuidate-2 service (not proxy_latam4).