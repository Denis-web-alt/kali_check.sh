# kali_check.sh

#!/bin/bash

echo "=== Kali Linux System Check ==="
echo ""

# Основная информация о системе
echo "[+] Системная информация:"
uname -a
lsb_release -a 2>/dev/null
echo ""

# Обновления системы
echo "[+] Проверка обновлений:"
sudo apt update -y >/dev/null && echo " - Пакеты обновлены (apt update)" || echo " - Не удалось выполнить apt update"
echo ""

# Проверка дискового пространства
echo "[+] Использование диска:"
df -h /
echo ""

# Проверка оперативной памяти
echo "[+] Использование оперативной памяти:"
free -h
echo ""

# Проверка загрузки процессора
echo "[+] Загрузка CPU:"
uptime
echo ""

# Проверка сервисов
echo "[+] Проверка основных сервисов:"
for svc in ssh tor apache2 postgresql; do
    systemctl is-active --quiet $svc && echo " - $svc: активен" || echo " - $svc: не активен"
done
echo ""

# Проверка нужных утилит
echo "[+] Проверка инструментов Kali:"
for tool in nmap metasploit msfconsole sqlmap wireshark burpsuite; do
    command -v $tool &>/dev/null && echo " - $tool: установлен" || echo " - $tool: не найден"
done
echo ""

echo "=== Проверка завершена ==="
