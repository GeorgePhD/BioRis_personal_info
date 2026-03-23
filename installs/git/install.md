Git Setup Guide (CachyOS / Arch)
 1) Install Git
sudo pacman -Syu git
2) Verify installation
git --version
3) First global configuration (identity)
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
4) Default branch name for new repos
git config --global init.defaultBranch main
5) Better defaults we usually set
Use rebase on pull (cleaner history)
git config --global pull.rebase true
Auto-stash when rebasing with local changes
git config --global rebase.autoStash true
Prune deleted remote branches on fetch
git config --global fetch.prune true
Clearer default push behavior
git config --global push.default simple
6) Useful aliases (optional but recommended)
git config --global alias.st "status -sb"
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.cm "commit -m"
git config --global alias.lg "log --oneline --graph --decorate -n 20"
7) Check all applied config
git config --global --list
8) Generate SSH key for GitHub/GitLab (recommended)
ssh-keygen -t ed25519 -C "you@example.com"
Start SSH agent and add key:
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
Copy public key:
cat ~/.ssh/id_ed25519.pub
Then paste it in your Git provider (GitHub/GitLab) SSH keys section.
9) Test SSH access
GitHub
ssh -T git@github.com
GitLab
ssh -T git@gitlab.com
10) Typical first-time workflow
# clone
git clone git@github.com:org/repo.git
cd repo
# check branch and status
git branch --show-current
git status
# create feature branch
git checkout -b feature/my-change
# after edits
git add .
git commit -m "feat: describe why the change is needed"
git push -u origin feature/my-change