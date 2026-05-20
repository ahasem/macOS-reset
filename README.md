Create file:

```bash
nano mac-setup.sh
```

Paste this:

```bash
#!/bin/zsh

set -e

echo "Installing Xcode Command Line Tools..."
xcode-select -p >/dev/null 2>&1 || xcode-select --install

echo "Installing Homebrew..."
if ! command -v brew >/dev/null 2>&1; then
  /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
fi

echo 'eval "$(/opt/homebrew/bin/brew shellenv zsh)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv zsh)"

echo "Updating Homebrew..."
brew update

echo "Installing CLI tools..."
brew install \
  fnm \
  git \
  pnpm \
  wget \
  gh \
  biome \
  cloudflared \
  cloudflare-wrangler

echo "Installing apps..."
brew install --cask \
  visual-studio-code \
  antigravity \
  google-chrome \
  docker \
  figma \
  zoom \
  adobe-creative-cloud

echo "Setting up Node with fnm..."
grep -q 'fnm env' ~/.zshrc || echo 'eval "$(fnm env --use-on-cd --shell zsh)"' >> ~/.zshrc
source ~/.zshrc

fnm install --lts
fnm use lts-latest

echo "Installing Bun..."
if ! command -v bun >/dev/null 2>&1; then
  curl -fsSL https://bun.sh/install | bash
fi

grep -q '.bun/bin' ~/.zshrc || echo 'export PATH="$HOME/.bun/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

echo "Installing global dev tools..."
npm install -g npm@latest
npm install -g wrangler prisma vercel typescript @openai/codex

echo "Installing Claude Code..."
brew install claude-code || true

echo "Configuring Git..."
git config --global user.name "Abul Hasem"
git config --global user.email "hasem@cdda.io"

echo "Creating projects folder..."
mkdir -p ~/projects

echo "Final versions:"
brew --version
node -v
npm -v
bun -v
git --version
docker --version || true
wrangler --version || true
biome --version || true
cloudflared --version || true
codex --version || true

echo "Setup completed successfully."
```

Save:
`CTRL + O` → Enter → `CTRL + X`

Run:

```bash
chmod +x mac-setup.sh
./mac-setup.sh
```

After reset, this will reinstall most of your environment automatically. For GitHub SSH and Cloudflare tunnel login, you still need manual login steps because they require browser/account authorization.
