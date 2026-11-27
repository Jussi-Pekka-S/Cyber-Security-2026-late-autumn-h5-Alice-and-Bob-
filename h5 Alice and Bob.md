You're using public key encryption every day. If you know what you're doing, it can help you even more.

Can you guess what's my favourite book on crypto basics?

x) Read and summarize (with some bullet points; check tips for free access)

Source:
€ Schneier 2015: Applied Cryptography:
Chapter 1 - Foundations
1.1 Terminology
Sender and Receiver, Messages and Encryption, Algorithms and Keys, Symmetric Algorithms, Public-Key Algorithms (From start until the end of Algorithms; not Cryptoanalysis and onwards)

Sender and Receiver: 
- Cryptography starts with a simple communication model, where the sender wants to transmit information to a receiver.
- To prevent unauthorized third party personnel such as eavesdroppers from understanding the message there is the security requirement.
- Basic sender -> Protected communication -> Receiver model. This underlies cryptographic communication. 

Messaged and Encryption:
- Ciphertext is a disguised or unreadable form of plaintext that was made by Encryption. The reverse operation of this is performed by Decryption.
- Plaintext does not need to be written text, it can be any form of binary information like digitized voice, files, images or video.
- So the original form of readable information is called either cleartext or plaintext.
- Information that is being stored or transmitted can be applied with Cryptography.
- Plaintext -> Encryption -> Ciphertext -> Decryption -> Plaintext.

Algorithms and Keys:
- Key is a value that is supplied to an algorithm, determining the particular transformation.
- For decryption and encryption the mathematical procedure that is being used is defined by a Cipher, or a cryptographic algorithm.
- Rather to keep the algorithm itself secret, security should generally depend on protection of the key.
- Ciphertext = Encrypt(Key, Plaintext) and Plaintext = Decrypt(Key, Ciphertext).

Symmetric Algorithms:
- The access to the appropriate secret key must be open to both the sender and receiver.
- This must be protected from unauthorized users.
- Confidentiality of the encrypted communication can be compromised, if the secret key is obtained by the attacker.
- Therefore secure key management and distribution is a major practical issue.

Public-Key Algorithms:
- Instead of relying on one shared secret. There are two related keys being used in Public-key cryptography. These are:
- Private key: This must remain secret with its owner.
- Public key: Can be distributed to other people.

Chapter 2 - Protocol Building Blocks, sections
2.5 Communications Using Public-Key Cryptography (From the start of "Communications using Public Key" until the start of "Digital signatures with encryption")

- Public key cryptography uses two different, but mathematically related keys. These are the previously mentioned public and private key.
- It should not be possible or practical to determine the private key, while knowing the public key.
- Basically: Person A makes their public key available and person B obtains it. Person B uses the public key to encrypt their message and sends the results in ciphertext to person A. Person A decrypts it with their private key.
- And thus private keys should remain secret and public keys can be distributed.

Source:
Karvinen 2023: PGP - Send Encrypted and Signed Message - gpg

- PGP provides at least these three security goals that are closely related:
1. Digital signatures that verify the person that signed the message and help with modification detection.
2. Public key cryptography that establishes secure communication for the parties without exchanging a shared secret over a secure channel.
3. Encryption that protects the message from the outsiders, while protecting the confidentiality.
- The same aspect that was mentioned before, where both parties exchange public keys.
- Combining both the signing and encryption, this provides evidence of authenticity/integrity and confidentiality.

a) Pubkey today. Explain how you have used public key cryptography today or yesterday, outside of this homework. In addition to naming the system, identify how different parties use keys in different steps of the system. 
(Answering this question likely requeries finding sources on your own. This subtask does not require tests with a computer.) 
Used Source: (Microsoft 2026)

- Today I have used public-key cryptography with Microsoft Edge as the system, as I have browsed HTTPS -websites while working to look for information.
- While I have accessed an HTTPS website, the cryptography has helped to establish the fact, that I am communicating with the genuine server to help in the setting up of a secure connection:
 
1. Actual data transfer that happens after the handshake. Using session keys derived during the handshake, connection switches to fast symmetric encryption.
2. Certificate Authority (CA), A trusted CA signs digitally the server's certificate using the CA's private key.
3. Key establishment, in the modern TLS most commonly public-key mechanisms is ephemeral ECDHE. This is used between server and browser to establish shared secret material.
4. Server authentication, where during the TLS handshake it demonstrates possession of the private key. Resulting in the helping of the browser in the verification of the communication with the server that is associated with the certificate.
5. The browser I am using, in this case Microsoft Edge uses appropriate CA public key in the verification of the server certificate.
6. Website has the private key that it keeps a secret. While presenting a certificate with the corresponding public key.

b) Encrypt and decrypt a message. Explain how different parties use different keys at different stages of operation. Evaluate the security of the tool you've chosen.

Used Sources: (Both links of GnuPG 2026 & OpenPGP)

For this part, I have used OpenPGP, where i chose the GnuPG(gpg). I think this in a way demonstrates/uses the same public-key concepts from the exercise that Karvinen had in the article. 
I will call myself person A(Encryptor) , and the other end person B(Decryptor).

EDIT: Changed Person A to (Encryptor), and the person B to (Decryptor)
EDIT: Changed a bit of the structure and added few lines of code to help with the "story".

- I want to sent an message to the person B, where I say "tomorrow's meeting starts at 13:00." 
- Person B will create a key pair:

gpg --full-generate-key

<img width="481" height="369" alt="image" src="https://github.com/user-attachments/assets/b9d4e6ee-49fc-45c2-80eb-5284c4a48a72" />

- After that person B will sent person A the public key, while keeping private key a secret:

gpg --armor --export B@example.com > B_public_key.asc

cat B_public_key.asc

<img width="479" height="167" alt="image" src="https://github.com/user-attachments/assets/5706b2a2-0e7b-4288-9d1a-0f3e581bccea" />

 
- Person A will import the key:

gpg --import B_public_key.asc

<img width="480" height="78" alt="image" src="https://github.com/user-attachments/assets/276258c4-af75-4993-ad9d-b9ebb7fa9d34" />

- Person A will encrypt the file using these commands:

echo "tomorrow's meeting starts at 13:00." > message.txt

gpg --encrypt --armor --recipient B@example.com message.txt

ls -l message.txt

cat message.txt.asc

<img width="479" height="205" alt="image" src="https://github.com/user-attachments/assets/d820c67e-9a27-4135-a469-3c8eb25ff93c" />

- This results in the encrypted file message.txt.asc, that can be sent to person B.
- This will hide the original message from an possible eavesdropper, if they obtain the file.

- Person B can decrypt it with the following command:

gpg --decrypt message.txt.asc

<img width="483" height="83" alt="image" src="https://github.com/user-attachments/assets/651e87fe-9dee-4b04-bc5e-565b026f8b5b" />

- This will result in the GnuPG using the corresponding private/secret key that recovers: "tomorrow's meeting starts at 13:00."

- B's public key is used when encrypting for B
- B's private key is used, when he decrypts.
- With the symmetric session key, encryption/decryption of the actual message happens.

- The strengths of using GnuPG are the following:
1. Hybrid encryption that uses the combination of symmetric encryption with public-key cryptography. 
2. Combinations of encryption and signatures can be used for messages simultaneously.
3. The private keys do not have to be transmitted at all. Private key is under the control of person B, that leads to avoiding the exchange of a shared secret key with the person A.

- The weaknesses of using GnuPG are the following:
1. The private key has the possibility of being stolen by an attacker, leading to wrong usage. This is covered in the GNU Privacy Handbook, where are the instructions on how to protect the private keys and so on.
2. Key verification has a "possibility" of wrong verification, as the cryptography does not mention who the key belongs to. This leads to possibilities of fake public keys being used.

I hope this text opens up the usage of OpenPGP & GnuPG. As I forgot to take the pictures. Look at the original sources to find more information! 

EDIT: Added the pictures, with the help of Tools :)

c) Password management. Demonstrate use of a password manager. What kind of attacks take advantage of people not using password managers? (You can use any password manager, some examples include pass and KeePassXC.)

- I have used for several years in my professional IT-career KeePass, that is an free application that can be downloaded from their website:
<img width="429" height="242" alt="image" src="https://github.com/user-attachments/assets/12702b86-9597-4212-8785-fc2a5f5a968e" />

- Once you have installed the application, you create an database in to the folder of your choice. In this case it is in my workplace OneDrive.
- This MasterFile, is protected by an password. With the password you can open the database that is .kdbx -file.
- With the Add Entry -button, you can add a new credential + password in to the masterfile to be stored:
<img width="490" height="503" alt="image" src="https://github.com/user-attachments/assets/5cec178b-a8b4-4aee-835c-98309fcb7385" />

- The KeePass usually generates an password that is very safe, you can also adjust the options. 
- You can write the title for an example "IT Specialist" and then the user name used for the system and the password. After hitting safe, the credentials are stored in the file for usage.
- To me this has been very practical, as there are tons of different applications, sites and servers that require their own credentials. Remembering these from the top-of-your head is really hard.
- I think the attacks that take advantage of people not using password managers depends on the case. It could be related to phishing, weak-passwords, password re-usage, different pop-ups just to name a few.
- I have never had a problem with attacks, while using KeePass. 

References/Sources:
All accessed on 20.11.2026.

- GnuPG 2026: https://www.gnupg.org/documentation//manuals/gnupg/Operational-GPG-Commands.html
- GnuPG 2026: https://www.gnupg.org/gph/en/manual.pdf
- Karvinen 2023: https://terokarvinen.com/2023/pgp-encrypt-sign-verify/
- Karvinen 2026: https://terokarvinen.com/cyber-security/#agenda
- Microsoft 2026: https://learn.microsoft.com/en-us/deployedge/microsoft-edge-security-cert-verification
- OpenPGP 2026: https://www.openpgp.org/
- Schneier 2015: https://www.oreilly.com/library/view/applied-cryptography-protocols/9781119096726/08_chap01.html
