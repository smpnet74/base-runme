This README.md markdown is designed as a Runme Notebook.  In order to use it as designed you need to install the Runme Notebook VScode extention.  In order to render the Mermaid code, you should also install make sure you have the Markdown Preview Mermaid extention by Matt Bierner installed.

```sh
sudo apt update
sudo apt upgrade -y
```

Install MODS https://github.com/charmbracelet/mods.  Enables the use of LLM's at the command line and shell scripts.

```sh
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://repo.charm.sh/apt/gpg.key | sudo gpg --dearmor -o /etc/apt/keyrings/charm.gpg
echo "deb [signed-by=/etc/apt/keyrings/charm.gpg] https://repo.charm.sh/apt/ * *" | sudo tee /etc/apt/sources.list.d/charm.list
sudo apt update && sudo apt install mods
```

Install GLOW https://github.com/charmbracelet/glow.  Glow is a terminal based markdown reader designed from the ground up to bring out the beauty—and power—of the CLI.

```sh
sudo apt update && sudo apt install glow
```

Script to modify the mods configuration file to default to the perplexity API and llam31-large model and set the API key.  Before you execute this cell you need to run mods --settings so that the base configuration is written to disk.

```sh
#!/bin/bash

# Path to the mods.yml file
CONFIG_FILE="/root/.config/mods/mods.yml"

# Check if the file exists
if [ ! -f "$CONFIG_FILE" ]; then
    echo "Error: Configuration file not found at $CONFIG_FILE"
    exit 1
fi

# Create a backup of the original file
cp "$CONFIG_FILE" "${CONFIG_FILE}.backup"

# Perform the replacements
if sed -i 's/^default-model: gpt-4o$/default-model: llam31-large/' "$CONFIG_FILE" && \
   sed -i '/^  perplexity:/,/^    api-key:/{s/^    api-key:$/    api-key: /}' "$CONFIG_FILE"; then
    echo "Successfully updated default model to perplexity and set API key"
    echo "Backup created at ${CONFIG_FILE}.backup"
else
    echo "Error: Failed to update the configuration file"
    # Restore from backup
    mv "${CONFIG_FILE}.backup" "$CONFIG_FILE"
    exit 1
fi
```

```sh
conda create -n starship python=3.12 -y

```

```sh
conda init zsh
```

```sh
conda activate starship
conda install -c conda-forge starship -y
```

```sh
conda activate starship
starship init zsh >> ~/.zshrc
echo "Install complete"
```

Install nushell into a conda environment.  Change the environment name as appropriate.

```sh
conda activate starship
conda install conda-forge::nushell -y
```

Install AWS CLI v2 in the starship conda environment

```sh
conda activate starship
# Download AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
# Install AWS CLI v2 into the conda environment
./aws/install --bin-dir $CONDA_PREFIX/bin --install-dir $CONDA_PREFIX/aws-cli
rm -rf aws awscliv2.zip
```

Starship configuration of the prompt when AWS has been installed.

```sh
#!/bin/bash

conda activate starship

# Check if AWS CLI is installed
if ! command -v aws &> /dev/null; then
    echo "AWS CLI is not installed"
    exit 1
fi

# Check if Starship is installed
if ! command -v starship &> /dev/null; then
    echo "Starship is not installed"
    exit 1
fi

# Path to starship config
CONFIG_FILE="$HOME/.config/starship.toml"

# Create .config directory if it doesn't exist
mkdir -p "$HOME/.config"

# Create or append to starship.toml
if [ ! -f "$CONFIG_FILE" ]; then
    # If file doesn't exist, create it with the AWS config
    cat > "$CONFIG_FILE" << 'EOF'
[aws]
format = '[$symbol($region)]($style)'
style = 'bold blue'
symbol = '🅰 '
disabled = false
force_display = true
[aws.region_aliases]
ap-southeast-2 = 'au'
us-east-1 = 'va'
EOF
    echo "Created new starship config with AWS settings"
else
    # Check if AWS config already exists
    if grep -q "\[aws\]" "$CONFIG_FILE"; then
        echo "AWS configuration already exists in starship.toml"
    else
        # Append AWS config to existing file
        cat >> "$CONFIG_FILE" << 'EOF'

[aws]
format = '[$symbol($region)]($style)'
style = 'bold blue'
symbol = '🅰 '
disabled = false
force_display = true
[aws.region_aliases]
ap-southeast-2 = 'au'
us-east-1 = 'va'
EOF
        echo "Added AWS configuration to existing starship.toml"
    fi
fi
```

Install Terraform 1.10.4 via conda-forge

```sh
conda install -n starship terraform=1.10.4 -c conda-forge -y
```

Install the 1.321 verison of kubectl

```sh
conda install -n starship kubernetes-client=1.32.1 -c conda-forge -y
```

Install Helm via Apt

```sh
curl https://baltocdn.com/helm/signing.asc | gpg --dearmor | sudo tee /usr/share/keyrings/helm.gpg > /dev/null
sudo apt-get install apt-transport-https --yes
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/helm.gpg] https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

```sh
conda install -n starship exa=0.10.1 -c conda-forge -y
echo 'alias ls="exa"' >> ~/.zshrc
```

Updated version of Cat called batcat which automatically adds a better structure, color, and line numbers to the traditional cat command.

```sh
conda install -n starship bat=0.25.0 -c conda-forge -y
echo 'alias cat="bat"' >> ~/.zshrc
```

A great command, rg, to quickly grep thru all the files in a directory or specifc globs for a word or regex.

```sh
conda install -n starship ripgrep=14.1.1 -c conda-forge -y
```

Installs Fuzzy Finder.  Great tool, so many uses to find files and more.

```sh
conda install -n starship fzf=0.57.0 -c conda-forge -y
echo 'source /opt/conda/envs/starship/share/fzf/shell/key-bindings.zsh' >> ~/.zshrc
echo 'source /opt/conda/envs/starship/share/fzf/shell/completion.zsh' >> ~/.zshrc

```

Zoxide install.  Better more powerful CD command using the Z command that has an FZF interation.

```sh
conda install -n starship zoxide=0.9.6 -c conda-forge -y
echo 'eval "$(/opt/conda/envs/starship/bin/zoxide init zsh)"' >> ~/.zshrc
echo "Added zoxide initialization to .zshrc"

```

Install modal labs python client

```sh
conda run -n starship pip install modal
```

Installs dust.  A tool to help you visualize storage consumption in the term

```sh
conda install -n starship dust=1.1.1 -c conda-forge -y

```

A better Top cli implimentation

```sh
conda install -n starship btop=1.4.0 -c conda-forge -y

```

Install node18

```sh
conda install -n starship nodejs=18 -c conda-forge -y
```
