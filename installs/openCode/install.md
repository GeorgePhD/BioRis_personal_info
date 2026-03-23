 OpenCode Slash Commands Study Sheet
> Important: Slash commands can vary by OpenCode build/version.  
> Always run `/help` first to see the **exact commands available on your machine**.
 1) Discovery commands (run these first)
- `/help` -> list available commands
- `/help <command>` -> detailed usage for one command
- `/commands` -> (if available) compact command list
- `/version` -> tool/runtime version
- `/status` -> current session state (model, mode, context)
---
 2) Common core commands (usually available)
- `/init` -> initialize workspace/session context
- `/plan` -> switch to plan-first workflow
- `/clear` -> clear chat/session context
- `/model <name>` -> change active model
- `/exit` -> close the CLI session
---
 3) Common work commands (often present)
- `/read <path>` -> inspect a file
- `/grep <pattern>` -> search text in repo
- `/glob <pattern>` -> find files by name/pattern
- `/diff` -> show pending changes
- `/apply` -> apply approved edits
- `/test` -> run test workflow
- `/commit` -> create git commit (after review)
- `/pr` -> prepare/open pull request
---
 4) Agent/skills-related commands (if your install includes them)
- `/agents` -> list available subagents
- `/agent <name>` -> run with a specific agent
- `/skills` -> list installed skills
- `/skill <name>` -> load/use one skill
- `/task <command>` -> run a slash-style task flow
---
 5) Safe workflow you can memorize
1. `/help`
2. `/init`
3. `/plan`
4. Investigate (`/read`, `/grep`, `/glob`)
5. Propose fix
6. `/apply`
7. `/diff`
8. `/test`
9. `/commit`
10. `/pr`
---
 6) If a command says "unknown"
- Run `/help` again (different builds expose different command sets).
- Check CLI docs for your installed version.
- Use natural-language equivalent:
  - "Plan only"
  - "Read-only check"
  - "Apply minimal fix"
---
 7) BioRis-focused prompt shortcuts (works even without slash commands)
- `Plan only: why login shows background but not form`
- `Read-only: check docker-compose and apache vhost`
- `Apply minimal fix only`
- `Give me copy-paste commands in markdown`


OpenCode Usage Guide (BioRis)
 What I am in this project
I am your CLI coding/devops assistant for BioRis.  
I can help you with:
- Docker/Compose startup and troubleshooting
- Apache/PHP + frontend asset debugging
- Git workflows (status, branch, commit guidance)
- Read-only code/config analysis
- Minimal safe fixes on request
---
 How to work with me (recommended workflow)
 1) Plan mode first
Ask me to plan before changes:
- `Plan the fix for login page not loading`
- `Plan only: investigate docker route issue`
I will return:
1. Plan
2. Risks/assumptions
3. Exact commands/edits
4. Ask your approval before execution
 2) Execution mode (only after approval)
When you approve:
- `go`
- `apply minimal fix`
- `run step 1 only`
I will execute exactly what you approved.
---
 Command styles that work best
 Clear intent
- `Check why cuidate-2 returns 404`
- `Only inspect files, no edits`
- `Fix with minimal changes only`
 Scope control
- `Only touch docker-compose.yaml`
- `Do not change apache config`
- `Only run cuidate2, not proxy`
 Output format
- `Answer in markdown`
- `Give me copy-paste commands only`
- `Short checklist format`
---
 BioRis daily commands
 Start only main container
cd "/mnt/storage/My_Documents/Projects/BioRis/cuidate_files"
docker compose up -d cuidate2
Verify
docker compose ps cuidate2
curl -I http://localhost:7000
Expected: 302 redirect to login.php.
Logs
docker logs --tail=120 cuidate-2
Stop
cd "/mnt/storage/My_Documents/Projects/BioRis/cuidate_files"
docker compose stop cuidate2
Recreate if broken
cd "/mnt/storage/My_Documents/Projects/BioRis/cuidate_files"
docker compose up -d --force-recreate cuidate2
---
When to run proxy_latam4
Run it only if study/image-related features fail.
cd "/mnt/storage/My_Documents/Projects/BioRis/cuidate_files"
docker compose up -d proxy_latam4
If login and normal UI work, you usually do not need it.
---
Fast troubleshooting checklist
1. Container state:
docker compose ps cuidate2
2. Apache/PHP errors:
docker exec -it cuidate-2 bash -lc "tail -n 200 /var/log/apache2/error.log"
3. Browser DevTools:
- Network tab -> check red files (404/500)
- Console tab -> JS errors
4. If CSS/JS 404:
- Verify file exists in project
- Verify path in login.php/templates
---
Git routine (safe and simple)
cd "/mnt/storage/My_Documents/Projects/BioRis"
git status
git branch --show-current
git pull origin <your-branch>
# make changes
git add <files>
git commit -m "fix: minimal change description"
git push origin <your-branch>
---
Prompt templates you can reuse
- Plan only: why does login show background but no form?
- Inspect only: check docker-compose and cuidate.sh
- Apply minimal fix only, no refactor
- Give me commands to validate after fix
- Write final steps in markdown for docs
---
Notes
- Your environment is CachyOS (Arch-based): use pacman, not apt.
- For this legacy project, prefer minimal edits and avoid broad upgrades.
- You can always tell me: plan mode only, and I will stay read-only.
If you want, next I can give you a second file in the same style: `BIO RIS_INCIDENT_RESPONSE.md` (step-by-step for common failures).