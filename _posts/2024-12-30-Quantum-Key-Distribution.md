---
title: Quantum Mechanics and Security - Part 1 - QKD
description: What is QKD? How can we rely on quantum principals to make distribution of encryption keys better?
author: Guykorol
date: 2024-12-30 08:00:00 +0300
categories: [Quantum]
tags: [Quantum, Cryptography, Keys, Physics]
pin: true
math: true
mermaid: true
---

# Quantum Mechanics and Security - Part 1 - QKD

The world of encryption is complicated, but, it exists everywhere, from **HTTPS** protocol, to storing sensitive data, and many more.
In this blog post I'm going to describe a way to exchange encryption keys between two parties, in a quantum channel.

The QKD technique is based on quantum principles, which allows us to know if There is a **MITM attack** on the key excahnge channel, Therefore it is superior to the other techniques in the sense of detection and security of the key exchange, However there are still problems to solve with it.

But in order to do so, we first need to talk about the ways of exchanging keys in today's world of networking.

## Key Exchange 
