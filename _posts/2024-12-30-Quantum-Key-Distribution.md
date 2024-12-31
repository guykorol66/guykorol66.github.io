---
title: Quantum Mechanics and Security - Part 1 - QKD
description: What is QKD? How can we rely on quantum principals to make distribution of encryption keys better?
author: Guykorol
date: 2024-12-31 08:00:00 +0300
categories: [Quantum]
tags: [Quantum, Cryptography, Keys, Physics]
pin: true
math: true
mermaid: true
---

# Quantum Mechanics and Security - Part 1 - QKD

The world of encryption is complicated, but, it exists everywhere, from **HTTPS** protocol, to storing sensitive data, and many more.
Encryption is the basis of all security, you wouldn't want somebody to get your bank credentials, or look at your medical records. The encryption and secure connections allow us to view and use important information and services.
In this blog post I'm going to describe a way to exchange encryption keys between two parties, in a quantum channel.

The QKD technique is based on quantum principles, which allows us to know if There is a [**MITM attack**](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) on the key exchange channel, Therefore it is superior to the other techniques in the sense of detection of third parties listening to the exchange, However there are still problems to solve with it.

But in order to do so, we first need to talk about the ways of exchanging keys in today's world of networking.

## Key Exchange 
![Man in the middle attack](/assets/img/QKD/MITM.png){: width="972" height="589" .w-50 .left}
In the past, it was common to use a symmetric key to encrypt and decrypt messages. which means **both** sides had the same key and could decrypt or encrypt messages with it. but first they need to exchange the key in order to use it.
The problem of the symmetric key, is that in order to use it, an exchange is needed, so if an attacker used MITM attack and viewd the key, he could encrypt and decrypt messages as well as the users.


In order to solve that, the public and private key system was invented, and using the [Diffie-Hellman key exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange), parties can exchange the public key openly, given that they authenticated against each other prior to it. The authentication issue is solved by the [PKI(Public key infrastructures)](https://en.wikipedia.org/wiki/Public_key_infrastructure), then together we can exchange keys securely and effectively. The focus here however, is on a different approach of key exchange, which is based on quantum mechanics.

## A little physics of light
Light is composed of photons, which are mass-less energetic particles. photons (as is the light) can be polarized in a specific polarization. we can define bits 0 or 1 as different polarizations of light.

![Light polarization](https://cdn1.byjus.com/wp-content/uploads/2023/02/Polarization-of-Light-Updated.png){: width="600" height="300" }
_Light going through a 0&#176; polarizer ([source](https://byjus.com/physics/polarization-of-light/))_

First we would define bases of polarization, which is basically the plane in space where the photons are moving:


|              | **Basis +**  | **Basis X**  |
|--------------|--------------|--------------|
| **Bit 0**    | 0&#176;    | 45&#176;  |
| **Bit 1**    | 90&#176;   | -45&#176;  |



![light polarization measurement](/assets/img/QKD/measurement.png){: width="972" height="589" .w-50 .right}
What would happen if I were to measure a photon in a different, perpendicular base of polarization, to the one i sent originally? Then the output would be random, 50/50 percent chance of receiveing 1 or 0.

This principle is what the QKD system relies on in the detection of an unwanted listener on the key exchange channel.

## The BB84 protocol
![Quantum Channel](/assets/img/QKD/alice_bob_channel.png){: width="972" height="589" .w-50 .right}
In 1984 Charles H. Bennet and Gilles Brassard in their article "Quantum cryptography:Public key distribution and coin tossing" suggested the protocol of exchanging encryption keys based on the principles of light polarization using single photons source.
This protocol relies on the existing authentication between the communicating parties prior to the key exchange.

Let's define Alice as the sender of the encryption key, and bob as the receiver:

1. Alice randomly chooses a base ( ’+’ or ’x’) and a bit ( 0 or 1) in which she will send the photon to Bob.
2. Alice sends the photon in its randomly chosen base and bit to Bob.
3. Bob randomly chooses a base in which to measure the sent photon.
4. Alice and Bob compare their base choices (which can be done publicly).
    * If their base choices were identical, the bit is kept and recorded.
    * If their base choices were nonidentical, the bit is discarded.
5. The remaining bits constitute the secret key between Bob and Alice.
6. With the generated key, Alice encrypts the the message using the shared key.
7. Alice sends the encrypted message to Bob, this is done publicly.

![Alice and bob bases and bits](/assets/img/QKD/results_bob_and_alice.png)
why does it work? Alice and Bob known that the base of the light polarization is identical and therefore the bit measurement is supposed to be 100% the same for both. 

## MITM Attack on Quantum Channel
Now for the interesting part, how can we detect a MITM attack on the connection? first let's say how we perform it!

In order to perform MITM attack on a quantum channel, we as attackers must impersonate both sides, which means we would have to set up both receiver and a sender of single photon source, we would have to randomly choose bases as well as Alice and Bob, and measure the bits sent.
The measurement of an attacker(Eve) yields in a random bit value when measured in the wrong base, this randomness is what introduces errors detectable by Alice and Bob. For instance, an eavesdropper measuring in the wrong basis collapses the quantum state, which alters the original bit value. Another option to for eavesdropping is to split the light using a [**Beamsplitter**](https://en.wikipedia.org/wiki/Beam_splitter), but it is problematic (for interested readers search for "the no-cloning theorem").

In the final moments, every party has a sequence of bases and measured bits. When Alice and Bob compare their base choices they choose only the bits where they agree on the base, which is around 50% of the sent bits.

In the presence of an eavesdropper, the predicted percentage of missmatching bits, even though the bases are the same, can be calculated and it is 25%. This is due to the mistake in measurement for the eavesdropper when measuring in the wrong base!

How can we use that in order to detect it?

After completion of the exchange and the bases validation, Alice and Bob publicly share an agreed upon percentage of their measured bits. If there are missmatches, it is a strong indication of an eavesdropper present!
![Alice and bob bases and bits with eve](/assets/img/QKD/results_bob_and_alice_and_eve.png)

## Notes on the protocol
* Even though the original protocol suggested in 1984 was based on single photon sources, today it is possible to perform it with laser pulses, and the protocols evolved from it are more advanced. 
* It is of course depends on the connection to be with an optical fiber, and therefore, it depends on the length of the optical fiber, which can limit the users of the protocol by distance.
* An attacker is limited due to the need for physical access to the quantum channel
* There are already companies suggesting services of QKD worldwide
* The same principals can be used for _true random number generators_

## Conclusion
The QKD is a very promising way of key exchange, which gives more information to it's users about possible unwanted listening parties.
The main shortage is the dependancy on specific hardware in order to do so, which is not as the normal network components deployed worldwide.
How do you see quantum cryptography shaping the future of cybersecurity?