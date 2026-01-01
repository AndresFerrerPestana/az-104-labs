# Lab 01 – Gerir Identidades no Microsoft Entra ID ![](https://img.shields.io/badge/Português-PT-green)

🇬🇧 [English version](README.md)

> 📘 **AZ-104 Hands-on Lab (Portuguese)**  
> Part of my practical preparation for the Microsoft Azure Administrator certification.

Este laboratório foca-se na gestão de identidades no **Microsoft Entra ID**, incluindo:
a criação de utilizadores internos, o convite de utilizadores externos (B2B) e
a configuração de **security groups** com associação e ownership corretos.

Todas as evidências (screenshots) encontram-se na diretoria `./screenshots/`.

---

## Microsoft Entra ID – Visão Geral do Tenant

![Microsoft Entra ID Overview](./screenshots/01-entra-id-overview.png)

Este screenshot mostra o **Default Directory** no Microsoft Entra ID,
confirmando o tenant utilizado para a realização deste laboratório.

---

## Gestão de Utilizadores

### Lista de utilizadores (Members e Guests)

![Users list](./screenshots/02-users-list.png)

Este screenshot confirma a presença de:

- Utilizadores internos (**Members**)
- Utilizadores externos (**Guests – B2B**)

---

### Validação de utilizador interno – az104-user1

![az104-user1 properties](./screenshots/03-user-az104-user1.png)

Este screenshot confirma a criação bem-sucedida do utilizador interno
**az104-user1**, com as seguintes características:

- Tipo de utilizador: **Member**
- Estado da conta: **Enabled**
- Associação correta ao tenant

---

### Validação de utilizador convidado (B2B)

![Guest user properties](./screenshots/04-user-guest.png)

Este screenshot confirma o convite bem-sucedido de um utilizador externo
**B2B guest**, validando:

- Tipo de utilizador: **Guest**
- Identidade externa (B2B collaboration)
- Conta ativa

---

## Gestão de Grupos

### Criação de Security Group

![Group overview](./screenshots/05-group-overview.png)

Este screenshot confirma a criação do security group
**IT Lab Administrators**, com as seguintes definições:

- Tipo de grupo: **Security**
- Tipo de membership: **Assigned**
- Grupo criado com sucesso no Microsoft Entra ID

---

### Validação de membros do grupo

![Group members](./screenshots/06-group-members.png)

Este screenshot confirma que os seguintes utilizadores foram adicionados
com sucesso como membros do grupo **IT Lab Administrators**:

- Utilizador interno (**az104-user1**)
- Utilizador externo (**Guest – B2B**)

---

### Validação de owners do grupo

![Group owners](./screenshots/07-group-owners.png)

Este screenshot confirma que o security group **IT Lab Administrators**
tem um **owner** corretamente atribuído, cumprindo os requisitos de
governança do laboratório.

---

## Resultado do Laboratório

Com a conclusão deste laboratório, foram atingidos os seguintes objetivos:

- Criação e gestão de utilizadores internos no Microsoft Entra ID
- Convite e validação de utilizadores externos (B2B)
- Criação de um security group com membership atribuída
- Configuração correta de membros e owners do grupo

Este laboratório demonstra competências fundamentais de
**Identity and Access Management (IAM)** exigidas para a certificação  
**AZ-104 – Microsoft Azure Administrator**.
