# SecureErase

- This Bash script, named "SecureErase," provides a secure method for permanently erasing data from specified external storage devices. The script prompts the user to enter the target devices, checks their existence, skips erasing the root device (/dev/sda) for safety, and securely wipes the selected devices using random data with a block size of 4 megabytes. Users with superuser privileges should execute the script, exercising caution as it irreversibly deletes data from the specified devices.



## code:
   ```bash
#!/bin/bash

# Function to check device existence
check_device() {
  if [ ! -e "$1" ]; then
    echo "Device $1 does not exist."
    return 1
  fi
}

# Function to securely erase a device
secure_erase() {
  echo "Erasing $1..."
  dd if=/dev/urandom of="$1" bs=4M status=progress
  echo "Finished erasing $1."
}

# Check if the script is run with superuser privileges
if [ "$EUID" -ne 0 ]; then
  echo "Please run this script as root (sudo)."
  exit 1
fi

# Prompt the user for the target devices
read -p "Enter the target devices (e.g., /dev/sdX /dev/sdY): " target_devices

# Iterate through the space-separated list of target devices
for device in $target_devices; do
  # Check if the device exists
  check_device "$device" || continue

  # Avoid erasing important devices
  if [ "$device" == "/dev/sda" ]; then
    echo "Skipping erasure of the root device (/dev/sda)."
    continue
  fi

  # Overwrite with random data
  secure_erase "$device"
done
   ```

## Installation:

```
https://github.com/hesam-zahiri/SecureErease.git
```

## How to use:

   ```bash
cd SecureErease
   ```
   
   ```bash
bash SecureErease.sh
   ```

## Attention!!
- The deleted data is not recoverable.
