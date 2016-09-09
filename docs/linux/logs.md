# Onde logs estão localizados no Linux

## Logs de ssh

- /var/log/auth.log
- /var/log/secure
- `journalctl -u sshd | tail -100`
