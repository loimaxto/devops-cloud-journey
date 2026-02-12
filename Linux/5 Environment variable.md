# Managing Environment Variables in Linux

## 1. Understanding Environment Variables

```bash
$PATH      # Directories to search for executable programs
$HOME      # Current user's home directory
$USER      # Current username
$SHELL     # Current shell being used
$PWD       # Present working directory
$LANG      # Language and locale settings
$PS1       # Primary prompt string
```

## 2. Viewing Environment Variables

### List All Environment Variables
```bash
# Print all environment variables
printenv
env

# View a specific variable
echo $PATH
echo $HOME

# Alternative syntax
echo ${PATH}
```

## 3. Setting Environment Variables

### Temporary Variables (Current Session Only)
```bash
# Set a variable
MY_VAR="value"

# Export to make it available to child processes
export MY_VAR="value"

# Multiple variables
export VAR1="value1" VAR2="value2"
```

### Permanent Variables

#### For Current User Only
Add to `~/.bashrc`, `~/.bash_profile`, or `~/.profile`:
```bash
# Edit the file
nano ~/.bashrc

# Add your variables
export JAVA_HOME="/usr/lib/jvm/java-11-openjdk"
export PATH="$PATH:$HOME/bin"

# Apply changes immediately
source ~/.bashrc
```

#### System-wide
Add to `/etc/environment` or `/etc/profile.d/custom.sh`:
```bash
# System-wide environment file
sudo nano /etc/environment

# Or create a custom script
sudo nano /etc/profile.d/my_vars.sh
```

## 4. Common Operations

### Appending to PATH
```bash
# Correct way - prevents duplication
export PATH="$PATH:/new/path"

# Check if already in PATH before adding
if [[ ":$PATH:" != *":/new/path:"* ]]; then
    export PATH="$PATH:/new/path"
fi
```

### Using Variables in Commands
```bash
# Reference variables
echo "My home is $HOME"

# Use in commands
cp file.txt $HOME/Documents/

# With other text (use curly braces)
echo "Java home: ${JAVA_HOME}/bin"
```

### Unsetting Variables
```bash
# Remove a variable
unset MY_VAR

# Remove exported variable
export -n MY_VAR
```

## 5. Variable Scope and Inheritance

### Local vs Global Variables
```bash
# Local variable (not exported)
local_var="hello"

# Global variable (exported)
export global_var="world"

# Test in a subshell
bash -c 'echo $local_var'      # Empty
bash -c 'echo $global_var'     # Prints "world"
```

### Preserving Variables with sudo
```bash
# Normally, sudo doesn't preserve user environment
sudo printenv HOME

# Preserve specific variables
sudo MY_VAR="$MY_VAR" command

# Preserve entire environment (use with caution)
sudo -E command
```

## 6. Practical Examples

### Example 1: Custom Development Environment
```bash
# ~/.bashrc additions
export PROJECT_HOME="$HOME/projects"
export EDITOR="nano"
export VISUAL="code"
export PYTHONPATH="$PYTHONPATH:$PROJECT_HOME/python"

alias proj="cd $PROJECT_HOME"
```

### Example 2: Application Configuration
```bash
# Set database connection variables
export DB_HOST="localhost"
export DB_PORT="5432"
export DB_USER="myuser"
export DB_PASS="mypassword"
export DB_NAME="mydatabase"

# Application-specific
export APP_ENV="development"
export DEBUG="true"
```

## 7. Best Practices

### Security Considerations
```bash
# Never store passwords in plain text environment variables
# Use dedicated credential stores or restricted files

# Sensitive variables should be set per session
export API_KEY="$(cat ~/.secrets/api_key)"
```

### Organization Tips
1. Group related variables together in your profile
2. Add comments explaining each variable's purpose
3. Use uppercase for global variables
4. Use lowercase for local script variables
5. Keep sensitive data out of version control

### Troubleshooting Common Issues

#### Variable Not Available in Scripts
```bash
# Problem: Script can't see your exported variables
# Solution: Export before running or source the script
. ./myscript.sh   # Sources the script
./myscript.sh     # Runs in subshell
```

#### PATH Problems
```bash
# Check PATH order
echo $PATH | tr ':' '\n'

# Find which executable will be used
which python
type python
```

## 8. Advanced Techniques

### Conditional Variable Setting
```bash
# Only set if not already set
export ${MY_VAR:="default_value"}

# Different values for different conditions
if [ "$HOSTNAME" = "prod-server" ]; then
    export APP_ENV="production"
else
    export APP_ENV="development"
fi
```

### Using env with Commands
```bash
# Run command with temporary environment
env MY_VAR="value" command

# Clear environment except for specified variables
env -i PATH="$PATH" HOME="$HOME" command
```

### Arrays in Environment Variables
```bash
# Bash arrays (note: not portable to all shells)
export MY_ARRAY=(item1 item2 item3)

# Simpler approach for portability
export MY_ITEMS="item1:item2:item3"
IFS=':' read -ra ITEMS <<< "$MY_ITEMS"
```

## 9. Quick Reference

| Command            | Description                                 |
| ------------------ | ------------------------------------------- |
| `printenv`         | Display all environment variables           |
| `echo $VAR`        | Display specific variable                   |
| `export VAR=value` | Set and export variable                     |
| `unset VAR`        | Remove variable                             |
| `env`              | Run command with modified environment       |
| `source file`      | Execute commands from file in current shell |