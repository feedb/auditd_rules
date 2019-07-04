правила auditd
многое взято из мануала pt siem,
можно добавлять по своему вкусу...

мониторинг изменений в веб-дирректории:
-w /var/www/html -p wa -k fileaud__mod_web

детект "злых" утилит в системе и добавление логирования их использования
touch /tmp/tfadl.tmp && find /bin/ /sbin/ /*/bin/ /*/sbin/ -type f -or -type l | grep -E
'nmap|hping|tcpdump|gcc|masscan | awk '{print "-w "$0" -p x -k fileaud_hackutil_"$0}'
> /tmp/tfadl.tmp && echo "## Suspicious utilities usage audit" >> /etc/audit/rules.d/
audit.rules && cat /tmp/tfadl.tmp >> /etc/audit/rules.d/audit.rules && echo "" >> /etc/
audit/rules.d/audit.rules && rm -f /tmp/tfadl.tmp && auditctl -R /etc/audit/rules.d/
audit.rules

