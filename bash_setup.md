Here’s an updated guide that covers both .bashrc and .zshrc profiles for configuring your shell environment:

# Shell Configuration Guide for `.bashrc` and `.zshrc`

This guide walks you through setting up common environment variables and tools in both Bash (`.bashrc`) and Zsh (`.zshrc`) profiles, ensuring your shell is configured consistently across both environments.

## Setting Up `~/bin` in Your PATH

Ensure that the `~/bin` directory is prioritized in your `$PATH`, so any binaries or scripts in this directory are used first:

```bash
export PATH=~/bin:/opt/homebrew/bin:$PATH

Homebrew Environment Setup

To initialize Homebrew and ensure it’s available in your shell session:

eval $(/opt/homebrew/bin/brew shellenv)

Node Version Manager (NVM) Setup

If you’re using NVM to manage your Node.js versions, add the following to your profile. This will load NVM and enable Node.js version switching:

export NVM_DIR="$HOME/.nvm"
[ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # Loads NVM
[ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # Loads NVM bash_completion

Alias Python to Python 3

To ensure the python command points to python3:

alias python=python3

Ruby Version Manager (rbenv) Initialization

To manage Ruby versions with rbenv, initialize it with the following command:

eval "$(rbenv init -)"

Python Version Management (pyenv) Initialization

For Python version management using pyenv, add the following to your profile:

export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"

Example: .bashrc Configuration

Here’s how your .bashrc file might look:

# Ensure ~/bin is first in PATH
export PATH=~/bin:/opt/homebrew/bin:$PATH

# Homebrew environment setup
eval $(/opt/homebrew/bin/brew shellenv)

# Node Version Manager (NVM) setup
export NVM_DIR="$HOME/.nvm"
[ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # Loads NVM
[ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # Loads NVM bash_completion

# Alias python to python3
alias python=python3

# rbenv initialization for Ruby version management
eval "$(rbenv init -)"

# pyenv initialization for Python version management
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"

Example: .zshrc Configuration

Here’s how your .zshrc file might look:

# Ensure ~/bin is first in PATH
export PATH=~/bin:/opt/homebrew/bin:$PATH

# Homebrew environment setup
eval $(/opt/homebrew/bin/brew shellenv)

# Node Version Manager (NVM) setup
export NVM_DIR="$HOME/.nvm"
[ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && \. "/opt/homebrew/opt/nvm/nvm.sh"  # Loads NVM
[ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # Loads NVM bash_completion

# Alias python to python3
alias python=python3

# rbenv initialization for Ruby version management
eval "$(rbenv init -)"

# pyenv initialization for Python version management
export PYENV_ROOT="$HOME/.pyenv"
[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"

This guide covers the basic setup for both Bash and Zsh profiles. Simply add the respective configuration to your .bashrc or .zshrc file to ensure your environment is set up consistently, regardless of which shell you use.

This version of the guide provides clear instructions for configuring both Bash and Zsh environments, with specific examples for each shell profile.
```
