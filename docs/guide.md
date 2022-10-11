# Installation Guide

## Deploy an instance of Lightflus cluster on AWS

## CLI Installation

### Mac

- homebrew

```bash
# Install CLI
brew install lightflus-cli

# Check if CLI installed
lightflus version

# Set up authentication
lightflus config --path=${HOME}/.lightflus/config set-credential --username=your_username --password=your_pwd

# Test connect remote cluster
lightflus get tasks
```

### Notes
* If you lost or don't know username and password, you can contact with the customer service on slack
* Please check the server IP if it is public first. Internal IP will never be connected successfully.

## API Installation

- Fetch API package from npm

```bash
npm install lightflus-api
```