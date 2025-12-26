# install
https://nodejs.org/en/download

Get Node.js® v24.12.0 (LTS)

for Linux

using nvm

with npm

```bash
# Download and install nvm:
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

## fix
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
# in lieu of restarting the shell
\. "$HOME/.nvm/nvm.sh"
# Download and install Node.js:
nvm install 24
# Verify the Node.js version:
node -v # Should print "v24.12.0".
# Verify npm version:
npm -v # Should print "11.6.2".
```