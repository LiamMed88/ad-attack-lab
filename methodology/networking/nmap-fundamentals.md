# Nmap fundamentals

Notas de la fase de reconocimiento con Nmap. Cosas que voy entendiendo del porqué, no solo del cómo.

## TTL por defecto según SO

Sirve para intuir el SO de un host solo con un ping, mirando la potencia de 2 más cercana por arriba (baja 1 por cada salto/router):

- Linux / BSD / macOS: 64
- Windows: 128
- Cisco / Solaris / AIX: 255

No es fiable al 100%, se puede falsear, pero da una pista rápida antes de meterle un escaneo agresivo.

## ARP vs ICMP

- ARP: capa 2, solo funciona dentro de la misma LAN, no se puede filtrar (si el host quiere hablar en la red, tiene que responder). Es lo que usa Nmap por defecto cuando escaneas tu propia subred.
- ICMP: capa 3, viaja entre redes, pero muchísimos firewalls lo bloquean. Un host puede estar vivo y no responder a ping.

## Cuándo tocar los parámetros de descubrimiento (-Pn, -n, etc.)

No es un interruptor random, depende de dos preguntas:

1. ¿Ya sé que el host está vivo (me lo han dado, o lo vi antes)? → `-Pn`, me salto el descubrimiento entero.
2. ¿Estoy en LAN? → dejo que Nmap use ARP, no lo toco.
3. ¿Estoy fuera de LAN y no sé si bloquean ICMP? → si me da "down" y tengo dudas, repito con `-Pn` por si acaso.
4. ¿Me importa el sigilo? → cambio el método de descubrimiento (`-PS443` por ejemplo) en vez de simplemente quitarlo.

`-n` desactiva la resolución DNS: más rápido (sin timeouts de hosts sin DNS reverso) y más sigiloso (menos consultas que quedan logueadas en el DNS del objetivo, que en AD suele ser el propio DC).

## Sacar el hostname de un objetivo

- DNS reverso: `nmap -R <ip>` (o por defecto si no usas `-n`)
- NetBIOS: `nmap -sn -PR <rango>`
- SMB (la más fiable en AD, saca hostname + dominio + SO en una):
  ```
  nmap -p 445 --script smb-os-discovery <ip>
  ```

## tcpdump

No intercepta tráfico ajeno, solo captura lo que pasa por tu propia interfaz. Para ver tráfico entre otras dos IPs que no sean la mía, necesito antes algo activo tipo ARP spoofing que me meta en medio — tcpdump por sí solo no hace MITM.

Ejemplo de uso para depurar mis propios escaneos/conexiones:
```
tcpdump -i <interfaz> -n port 80 -X
```

## netcat (nc)

Navaja suiza de conexiones TCP/UDP. Dos modos:

- Cliente: `nc <ip> <puerto>` — conexión en crudo
- Listener: `nc -lvnp <puerto>` — se pone a escuchar

Usos que más voy a repetir: banner grabbing manual, recibir reverse shells, comprobar puertos abiertos rápido (`nc -zv <ip> <puerto>`).

Flags que uso siempre: `-l` listen, `-v` verbose, `-n` sin DNS, `-p` puerto local, `-z` solo comprobar sin mandar datos.

Comprobado con mis propias manos: levantando tcpdump y haciendo una conexión con nc a mí mismo, se ve perfectamente el three-way handshake completo (SYN, SYN-ACK, ACK).
