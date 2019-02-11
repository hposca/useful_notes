# Wiping a file

```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'

if [[ "$#" -ne 1 ]]; then
  echo "Please use this command with the filename you want to wipe. Like:"
  echo "$0 file.txt"
  exit 1
fi

_file="${1}"

_times=8
filesize=$(stat -c %s "${_file}")

# Zeroes
dd if=/dev/zero of="${_file}" bs="${filesize}" count=1 conv=notrunc
# Ones
dd if=<(yes $'\xFF' | tr -d '\n') of="${_file}" bs="${filesize}" count=1 conv=notrunc

# Random stuff
for (( i = 0; i < _times; i++ )); do
  dd if=/dev/urandom of="${_file}" bs="${filesize}" count=1 conv=notrunc
done
```

Made possible with the help of [Securely Wipe a File with DD](https://www.marksanborn.net/security/securely-wipe-a-file-with-dd/) and [How do I get an equivalent of /dev/one in Linux](https://stackoverflow.com/questions/10905062/how-do-i-get-an-equivalent-of-dev-one-in-linux)
