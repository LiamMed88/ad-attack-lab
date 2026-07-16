# AD Attack Lab

Laboratorio propio de Active Directory para practicar, documentar y demostrar técnicas de ataque ofensivo en entornos empresariales realistas.

## 🎯 Objetivo

Construir un entorno de AD vulnerable (GOAD), atacarlo de forma metódica y documentar cada attack path con el mismo nivel de detalle que se espera en un pentest profesional: contexto, técnica, evidencia, mapeo MITRE ATT&CK y remediación.

## 🖧 Arquitectura del lab

*(diagrama de red — se añade al desplegar GOAD)*

## 🛠️ Stack

- **Entorno**: GOAD (Game of Active Directory) sobre VMware
- **Enumeración**: BloodHound, PowerView, ldapdomaindump
- **Explotación**: Impacket, Rubeus, Mimikatz
- **Documentación**: Markdown + capturas

## 📂 Attack Paths documentados

| # | Técnica | Táctica MITRE | Estado |
|---|---|---|---|
| 01 | Kerberoasting | Credential Access | 🔲 Pendiente |
| 02 | AS-REP Roasting | Credential Access | 🔲 Pendiente |
| 03 | Pass-the-Hash | Lateral Movement | 🔲 Pendiente |
| 04 | Unconstrained Delegation | Privilege Escalation | 🔲 Pendiente |
| 05 | ACL Abuse | Privilege Escalation | 🔲 Pendiente |

*(se va actualizando conforme avanzo por el path)*

## 📋 Metodología

Cada attack path sigue la misma estructura: contexto → enumeración → explotación → evidencia → mapeo ATT&CK → remediación. Ver [`docs/lab-setup.md`](docs/lab-setup.md) para el despliegue del entorno.

## ⚠️ Disclaimer

Lab educativo desplegado en entorno aislado y controlado. Todas las técnicas se ejecutan exclusivamente sobre infraestructura propia.
