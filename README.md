<!-- LOGO -->
<p align="center">
  <img width="400" alt="fur-box" src="https://github.com/user-attachments/assets/0f716f7b-8df3-4e18-84f8-d503996026db" />
</p>

# The FUR Safe

An experimental repository exploring the **locking and encryption capabilities** of recent versions of **FUR**.

The goal of this repository is to test how FUR conversations behave when they are **password-protected, encrypted, and version-controlled inside a public Git repository**.

In other words:

> Can conversations remain private even when the repository storing them is public?

This repository attempts to answer that question.

---

## What is FUR?

FUR is a command-line tool for **conversation control**.

Rather than leaving conversations trapped inside proprietary platforms, FUR allows users to manage conversations as **structured artifacts on disk**.

Once conversations become files, they can be:

- organized
- archived
- forked
- version controlled
- encrypted

FUR treats conversations as **durable objects** that belong to the user.

Crate:

https://crates.io/crates/fur-cli

---

## Unlocking the Safe

The encrypted conversations in this repository are protected using **FUR's password-locking system**.

For demonstration purposes, the password is intentionally public so that anyone can reproduce the decryption process. This repository exists only to demonstrate the encryption capabilities of FUR.

The demo password is stored in: [demo-password.txt](demo-password.txt)

Unlock the repository using FUR:

```
fur unlock
```

This will decrypt the encrypted conversation artifacts and restore the original FUR conversation structures locally.

---

## Inspecting the conversation

After unlocking the repository, the reconstructed conversation can be printed with:

```
fur print
```

This command rebuilds the conversation from the unlocked artifacts and prints the full dialogue.

This repository therefore functions as a **reproducible demonstration** of how FUR conversations can be encrypted, stored publicly, and later restored.

---

### Security note

The password above is intentionally public and used only for demonstration.

Real conversations should always use a **freshly generated high-entropy password**.

One way to generate secure passwords is with another tool in this ecosystem.

Install:

```
cargo install aesus
```

Generate a password:

```
aesus generate
```

By default AESus generates **6 Diceware words**, providing approximately **77.5 bits of entropy**, which is suitable for strong password-based encryption.

---

## Purpose of this repository

**The FUR Safe** is a sandbox used to test the **security and locking features** introduced in newer versions of FUR.

The experiments here focus on:

- password-protected conversations
- encryption of conversation artifacts
- compatibility with Git version control
- integrity of encrypted conversation structures

The repository intentionally stores **encrypted FUR artifacts** to evaluate how they behave when tracked with Git.

The plaintext conversations are not visible until the artifacts are unlocked.

---

## What FUR encryption protects

FUR encryption is designed to protect **all structural components of a conversation**, not only message content.

Encrypted components include:

- conversation metadata
- conversation identifiers
- message ordering and sequencing
- short-form messages (*jots*) stored in metadata
- long-form messages stored as markdown conversation files

After encryption, the conversation becomes a **ciphertext artifact**.

These artifacts can safely exist inside a repository without exposing the underlying conversation.

---

## Why the ciphertext changes every time

If you experiment with the repository you may notice something interesting.

Running a cycle such as:

```
lock → unlock → lock
```

will produce **different ciphertext each time**, even when:

- the password is identical
- the plaintext conversation has not changed

This behavior is intentional.

FUR encryption uses authenticated encryption based on **AES-GCM**. Each encryption operation includes a **random nonce (initialization vector)**.

Conceptually:

```
ciphertext = AES_GCM(key, nonce, plaintext)
```

Because the nonce is randomly generated each time, encrypting the same plaintext repeatedly produces different ciphertext outputs.

```
encrypt(plaintext, password) → ciphertext A
encrypt(plaintext, password) → ciphertext B
encrypt(plaintext, password) → ciphertext C
```

All ciphertext artifacts will differ.

The nonce is stored alongside the ciphertext so the system can still decrypt the conversation correctly.

This property ensures the encryption is **non-deterministic**, which prevents attackers from detecting when the same plaintext appears multiple times.

If ciphertext did **not** change between encryptions, it would indicate deterministic encryption — a serious security weakness.

Changing ciphertext for identical plaintext is therefore **expected and desirable**.

---

## Version controlling encrypted conversations

One objective of this repository is to explore whether encrypted conversations behave well under Git.

Encrypted artifacts should behave like normal repository files:

- they can be committed
- they can be versioned
- they can change over time
- they remain unreadable without the key

This repository therefore functions as a **test environment for encrypted conversational artifacts under version control**.

---

## Status

Experimental.

This repository exists solely as a **testbed for the locking and encryption features of FUR conversations** and their behavior under version control.

The artifacts contained here are intentionally opaque until unlocked.
