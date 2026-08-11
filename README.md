# Analyse d'e-mails d'hameçonnage

## Objectif

L’objectif de ce TP est de mener une investigation complète sur un courriel suspect afin de déterminer s’il s’agit d’une tentative d’hameçonnage et d’identifier les menaces qui lui sont associées.
L’analyse est réalisée à partir du laboratoire PhishStrike de la plateforme BlueDefenders. Elle consiste notamment à examiner les en-têtes et les mécanismes d’authentification du courriel (SPF, DKIM et DMARC), à analyser les URL et fichiers suspects, puis à utiliser différentes sources de renseignement sur les menaces et des outils d’analyse de logiciels malveillants.
L’investigation vise également à identifier les familles de malwares impliquées, leurs mécanismes de persistance et leurs communications de commande et de contrôle (C2), ainsi qu’à extraire les principaux indicateurs de compromission (IOC) pouvant être utilisés dans le cadre d’une investigation ou d’une réponse à incident.


## Compétences acquises

La réalisation de ce TP m’a permis de développer et de mettre en pratique plusieurs compétences liées à l’analyse des courriels d’hameçonnage, au renseignement sur les menaces et à l’investigation de logiciels malveillants.

### Analyse des courriels suspects

* Examiner la structure et les en-têtes techniques d’un courriel.
* Identifier l’expéditeur, le destinataire, le chemin de retour et les serveurs ayant participé à l’acheminement du message.
* Interpréter les mécanismes d’authentification SPF, DKIM et DMARC.
* Repérer des anomalies pouvant indiquer une usurpation d’identité ou une tentative d’hameçonnage.

### Investigation OSINT et Threat Intelligence

* Enrichir des indicateurs suspects à l’aide de plusieurs sources de renseignement sur les menaces.
* Analyser la réputation d’adresses IP, de domaines, d’URL et de fichiers.
* Recouper les résultats provenant de plusieurs sources avant de tirer une conclusion.
* Identifier et documenter des indicateurs de compromission (IOC).

### Analyse de logiciels malveillants

* Identifier différentes familles de malwares associées à une même chaîne d’infection, notamment Coinminer, BitRAT et AsyncRAT.
* Analyser les comportements observés lors de l’exécution d’un malware.
* Identifier des mécanismes de persistance reposant sur les clés d’exécution automatique du registre Windows.
* Examiner les communications HTTP utilisées pour télécharger des charges malveillantes.
* Identifier des infrastructures de commande et de contrôle (C2).

### Investigation et réponse à incident

* Extraire des IOC tels que des adresses IP, URL, domaines, noms de fichiers et hachages SHA-256.
* Distinguer un indicateur réellement malveillant d’une infrastructure légitime potentiellement utilisée abusivement.
* Évaluer les risques liés au blocage d’une adresse IP appartenant à un fournisseur de services cloud.
* Documenter méthodiquement les différentes étapes d’une investigation afin de produire des résultats exploitables par une équipe de sécurité.

* 
VirusTotal →  ; Malpedia →  ; Joe Sandbox →  ; CyberChef →  ; MalwareBazaar →  ; Email Header Analyzer → .

### Outils utilisés

- VirusTotal : réputation des URL/IP/fichiers
- Malpedia: renseignements sur les familles de malwares
- Urlhauss.abuse.ch
- Notepad++
- AlienVault
- JoeSandbox: analyse comportementale
- Cyberchef :décodage/transformation de données
- DomainTools
- MalwareBazaar: recherche d’échantillons et de hash
- Email Header Analyzer: analyse des en-têtes

## Étapes du TP
### **1- Prise de connaissance du cas** ###

Pour débuter on prend connaissance du scenario proposé. L'image ci dessous résume 

<img width="1833" height="539" alt="scenario" src="https://github.com/user-attachments/assets/79be6122-00fa-400d-ac12-125bc1c9630a" />

*Image 1: Scénario*

Les questions auxquelles on doit répondre après investigation sont les suivantes:

**Q1** : Identifier l'adresse IP de l'expéditeur grâce à des valeurs SPF et DKIM spécifiques permet de remonter à la source d'un courriel d'hameçonnage. Quelle est l'adresse IP de l'expéditeur dont la valeur SPF est « softfail » et la valeur DKIM « fail » ?

**Q2** : Comprendre le chemin de retour d'un courriel est essentiel pour en retracer l'origine. Quel est le chemin de retour spécifié dans ce courriel ?

**Q3** : Identifier la source d'un logiciel malveillant est crucial pour une atténuation et une réponse efficaces aux menaces. Quelle est l'adresse IP du serveur hébergeant le fichier malveillant lié à la distribution du logiciel malveillant ?

**Q4** : Identifier les logiciels malveillants qui exploitent les ressources système pour le minage de cryptomonnaies est essentiel pour prioriser les efforts d'atténuation des menaces. L'URL malveillante peut diffuser plusieurs types de logiciels malveillants. Quelle famille de logiciels malveillants est responsable du minage de cryptomonnaies ?

**Q5** : Identifier les URL spécifiques demandées par le logiciel malveillant est essentiel pour perturber ses canaux de communication et réduire son impact. D'après l'analyse précédente de l'échantillon de logiciel malveillant de cryptomonnaie, quelle URL ce logiciel malveillant demande-t-il ?

**Q6** : Comprendre les entrées de registre ajoutées à la clé d’exécution automatique par un logiciel malveillant est crucial pour identifier ses mécanismes de persistance. D’après l’analyse de l’échantillon de logiciel malveillant BitRAT, quel est le nom de l’exécutable dans la première valeur ajoutée à la clé d’exécution automatique du registre ?

**Q7** : Identifier le hachage SHA-256 des fichiers téléchargés depuis une URL malveillante est essentiel pour suivre et analyser l’activité des logiciels malveillants. D’après l’analyse de BitRAT, quel est le hachage SHA-256 du fichier précédemment téléchargé et ajouté aux clés d’exécution automatique ?

**Q8** : Analyser les requêtes HTTP effectuées par un logiciel malveillant permet d’identifier ses modes de communication. Quelle est l’URL de la requête HTTP utilisée par le chargeur pour récupérer le logiciel malveillant BitRAT ?

**Q9** : Introduire un délai dans l’exécution d’un logiciel malveillant peut permettre de contourner les mécanismes de détection. Quel est le délai (en secondes) causé par la commande PowerShell d’après l’analyse de BitRAT ?

**Q10** : Suivre les domaines de commande et de contrôle (C2) utilisés par les logiciels malveillants est essentiel pour détecter et bloquer les activités malveillantes. Quel est le domaine C2 utilisé par le malware BitRAT ?

**Q11** : Comprendre comment les malwares exfiltrent des données est essentiel pour détecter et prévenir les violations de données. D’après l’analyse d’AsyncRAT, quel est l’identifiant du bot Telegram utilisé par ce malware ?


### **2- Mise en place de Notepad++** ###

Pour analyser le courriel suspicieux, j'utilise Notepad++ avec une configuration précise. Il s' agit de sélectionner le langage **YAML** sous la section language. Ce paramètres permet de distinguer facilement les différentes parties du courriel dont les entêtes.

<img width="3072" height="1753" alt="notepad++" src="https://github.com/user-attachments/assets/14863a90-a12e-406e-82c2-4bc6f15c6f57" />

*Image 2: Aperçu de la configuration Notepad++*

Je commence l' investigation par les entêtes **To** et **Subject** qui permettent d' avoir des informations respectivement sur le destinataire du courriel et l' objet du courriel. Ces deux entêtes se situent légèrement au dessus de l entête **FROM** qui nous informe sur l' origine du courriel. Dans ce cas il s' agit de **ERIKA JOHANA LOPEZ VALIENTE < erikajohana.lopez@uptc.edu.co >** . L'entête To affiche **undisclosed-recipients:;** donc ne nous donne pas réellement une information claire sur le destinataire du courriel. L' entête **Message-ID: < CABWu4iua5_uex6=G8pi_OJz1tBLJiNakMK-1=7128orpzxbKxw@mail.gmail.com >** est un identificateur unique. On peut lire gmail.com à la fin, ce qui veut dire que gmail a été utilisé dans le processus de livraison de ce courriel. On observe egalement l' entête **Received: by mail-wr1-f65.google.com with SMTP id ffacd0b85a97d-332e7630a9dso2382526f8f.1  for < servicios.informaticos@fsfb.org.co>;Thu, 9 Dec 2022 06:58:39 -0800 (PST)** qui correspond  au premier serveur mail qui a recu le courriel

La raison pour laquelle je commence l' investigation par ces entêtes est simplement parce qu' on peut généralement obtenir des informations supplémentaires sur l' origine de l' attaqueur. Par exemple on peut voir le domaine fsfb.org.co qui semble suspicieux. On va utiliser des outils OSINT comme VirusTotal, DomainTools pour creuser en profondeur, obtenir plus de contexte et vérifier la légitimité de informations.

### **3- Entêtes d' authentification** ###

 La prochaine étape dans l' investigation est la vérification des résultats d' authentification ou entêtes d' authentification. Cette obeservation nous permettra de donner des informations sur **SPF, DKIM et DMARC**. SPF  **DKIM DomainKeys Identified Mail** permet d' authentifier l’expéditeur d’un email et **DMARC — Domain-based Message Authentication Reporting and Conformance** protège contre le spoofing email. Ces trois paramètres représentent en quelque soit la sécurité du courriel.
 
 <img width="1509" height="434" alt="authentification results" src="https://github.com/user-attachments/assets/d7006ed6-f456-4ce2-a736-404c19946779" />

 *Image 3: Aperçu des entêtes d' authentification*

On observe sur l' image 3, un "*softfail**" du SPF et aussi l' état "Fail" du DKIM ce qui signifie que l' expéditeur n' a pas été authentifié. L' adresses IP de l'expéditeur est 18.208.22.104. On peut ensuite passer au corps du courriel.

### **3- Corps du courriel** ###
L' image 4 présente ce qui est considéré comme le corps du courriel. Il contient le contenu du courriel.


<img width="3012" height="820" alt="corps du courriel" src="https://github.com/user-attachments/assets/924de197-a6d3-47a5-b0b5-983f7afc973d" />

 *Image 4: Corps du courriel*

On observe le message du courriel mais on peut également apercevoir un lien qui attire particulièrement l' attention. On utilise virustotal pour investiguer le lien. On pourrait commencer par vérifier l' adresse IP de l envoyeur qu' on a découvert plus haut (18.208.22.104).

<img width="2910" height="1629" alt="sender IP" src="https://github.com/user-attachments/assets/2d08266e-a4f8-4177-9ae1-f5bcd96da64d" />


 *Image 5: Adresse IP de l envoyeur*

 On observe que l' adresse IP appartient a **AMAZON** ce qui n' est pas une mauvaise chose. Il est recommandé de ne pas bloquer ces adresses IP lorsqu'on tombe sur des adresses IP de grandes compagnies comme Google, Amazon, Microsoft etc car ca peut créer une rupture de services  et donc affecter la disponibilité de services à certains niveaux. Au lieu de bloquer l' adresse, il est recommandé de poursuivre l" investigation en profondeur pour trouver d' autres informations.

On va a présent verifier dans VirusTotal le lien contenu dans le corps du message : **http://107.175.247.199/loader/install.exe**


<img width="2906" height="1658" alt="installer" src="https://github.com/user-attachments/assets/f212bc24-ddd4-4054-a6cf-feb3f7c08611" />

 *Image 6: URL malveillante*



On observe que  l' URL est potentiellement dangereuse 12/92 fournisseurs de sécurité ont signalé cette URL comme malveillante. En suivant les avis de la cummunauté et des fournisseurs , on observe que le lien contient un programme malicieux que est classé dans plusieurs catégories: **AsyncRat**, **BitRat** et **Coinminer**. 

Apres quelques recherches notamment **Malpedia**, un **AsyncRat** est un outil d'accès à distance (RAT) conçu pour surveiller et contrôler à distance d'autres ordinateurs via une connexion chiffrée et sécurisée. Bien qu'il s'agisse d'un outil d'administration à distance open source, il peut également être utilisé à des fins malveillantes car il intègre des fonctionnalités telles que l'enregistrement de frappe au clavier, le contrôle de bureau à distance et bien d'autres fonctions susceptibles d'endommager l'ordinateur de la victime. De plus, AsyncRAT peut être diffusé par diverses méthodes, notamment le spear-phishing, la publicité malveillante, les kits d'exploitation et d'autres techniques.

Selon Bitdefender, **BitRat** est un cheval de Troie d'accès à distance (RAT) notoire, commercialisé sur les marchés et forums clandestins de cybercriminels. Son prix de 20 $ pour un accès à vie le rend irrésistible pour les cybercriminels et favorise la propagation de son code malveillant. De plus, la diversité des modes opératoires des acheteurs rend BitRAT d'autant plus difficile à neutraliser, car il peut être utilisé dans diverses opérations, telles que l'introduction de logiciels trojanisés, le phishing et les attaques par point d'eau. La popularité de BitRAT tient à sa polyvalence. Cet outil malveillant peut effectuer un large éventail d'opérations, notamment l'exfiltration de données, le contournement du contrôle de compte d'utilisateur (UAC), les attaques DDoS, la surveillance du presse-papiers, l'accès non autorisé à la webcam, le vol d'identifiants, l'enregistrement audio, le minage de cryptomonnaie XMRig et l'enregistrement de frappe au clavier.

Enfin **Coinminer**  est un logiciel malveillant indésirable qui utilise la puissance de calcul de la victime (principalement le processeur et la mémoire vive) pour miner des cryptomonnaies (par exemple, Monero ou Zcash). Ce logiciel malveillant assure sa persistance en installant un mineur open source au démarrage, sans l'accord de la victime. Les mineurs de cryptomonnaies les plus sophistiqués utilisent des paramètres de minuterie ou limitent l'utilisation du processeur pour rester discrets. L' image 7 présente un détail complet du programme malicieux contenu dans le lien url.


<img width="2594" height="1625" alt="details du malware" src="https://github.com/user-attachments/assets/24c379a3-9a4e-4e0d-8a21-4b5f4e0c6924" />

 *Image 7: Details du malware*



 
### **4- Réponses aux questions** ###

Apres avoir enqueter et compri le risque que comportait le lien dans le corps du courriel, il est temps de commencer à répondre aux questions.


**Q1** : Identifier l'adresse IP de l'expéditeur grâce à des valeurs SPF et DKIM spécifiques permet de remonter à la source d'un courriel d'hameçonnage. Quelle est l'adresse IP de l'expéditeur dont la valeur SPF est « softfail » et la valeur DKIM « fail » ?

**Réponse:** l'adresse IP de l'expéditeur  **18.208.22.104**

**Q2** : Comprendre le chemin de retour d'un courriel est essentiel pour en retracer l'origine. Quel est le chemin de retour spécifié dans ce courriel ?

**Réponse:** le chemin de retour spécifié dans ce courriel est  **erikajohana.lopez@uptc.edu.co**

**Q3** : Identifier la source d'un logiciel malveillant est crucial pour une atténuation et une réponse efficaces aux menaces. Quelle est l'adresse IP du serveur hébergeant le fichier malveillant lié à la distribution du logiciel malveillant ?

**Réponse:** l'adresse IP du serveur hébergeant le fichier malveillant est contenu dans le lien http://107.175.247.199/loader/install.exe donc **107.175.247.199**


**Q4** : Identifier les logiciels malveillants qui exploitent les ressources système pour le minage de cryptomonnaies est essentiel pour prioriser les efforts d'atténuation des menaces. L'URL malveillante peut diffuser plusieurs types de logiciels malveillants. Quelle famille de logiciels malveillants est responsable du minage de cryptomonnaies ?

**Réponse:** La famille de logiciels malveillants responsable du minage de cryptomonnaies est **Coinminer**

**Q5** : Identifier les URL spécifiques demandées par le logiciel malveillant est essentiel pour perturber ses canaux de communication et réduire son impact. D'après l'analyse précédente de l'échantillon de logiciel malveillant de cryptomonnaie, quelle URL ce logiciel malveillant demande-t-il ?

**Réponse:** HTTP/ripley.studio/loader/uploads/Qanjttrbv.jpeg 


**Q6** : Comprendre les entrées de registre ajoutées à la clé d’exécution automatique par un logiciel malveillant est crucial pour identifier ses mécanismes de persistance. D’après l’analyse de l’échantillon de logiciel malveillant BitRAT, quel est le nom de l’exécutable dans la première valeur ajoutée à la clé d’exécution automatique du registre ?

**Réponse:** Jzwvix.exe

**Q7** : Identifier le hachage SHA-256 des fichiers téléchargés depuis une URL malveillante est essentiel pour suivre et analyser l’activité des logiciels malveillants. D’après l’analyse de BitRAT, quel est le hachage SHA-256 du fichier précédemment téléchargé et ajouté aux clés d’exécution automatique ?

**Réponse:** le hachage SHA-256 du fichier précédemment téléchargé et ajouté aux clés d’exécution automatique est **bf7628695c2df7a3020034a065397592a1f8850e59f9a448b555bc1c8c639539**

**Q8** : Analyser les requêtes HTTP effectuées par un logiciel malveillant permet d’identifier ses modes de communication. Quelle est l’URL de la requête HTTP utilisée par le chargeur pour récupérer le logiciel malveillant BitRAT ?

**Réponse:** l’URL de la requête HTTP utilisée par le chargeur pour récupérer le logiciel malveillant BitRAT est **http://107.175.247.199/loader/server.exe**

**Q9** : Introduire un délai dans l’exécution d’un logiciel malveillant peut permettre de contourner les mécanismes de détection. Quel est le délai (en secondes) causé par la commande PowerShell d’après l’analyse de BitRAT ?

**Réponse:** le délai (en secondes) causé par la commande PowerShell d’après l’analyse de BitRAT est **50 sec**

**Q10** : Suivre les domaines de commande et de contrôle (C2) utilisés par les logiciels malveillants est essentiel pour détecter et bloquer les activités malveillantes. Quel est le domaine C2 utilisé par le malware BitRAT ?

**Réponse:** le domaine C2 utilisé par le malware BitRAT est **gh9st.mywire.org**

**Q11** : Comprendre comment les malwares exfiltrent des données est essentiel pour détecter et prévenir les violations de données. D’après l’analyse d’AsyncRAT, quel est l’identifiant du bot Telegram utilisé par ce malware ?

**Réponse:** bot5610920260





## Conclusion

Cette investigation a permis d’analyser les différentes étapes d’une attaque d’hameçonnage, depuis l’examen initial du courriel jusqu’à l’identification des logiciels malveillants et de leur infrastructure de communication.
L’analyse des en-têtes du message a notamment mis en évidence des anomalies au niveau des mécanismes d’authentification, avec un résultat SPF « softfail » et un échec de la vérification DKIM. L’examen du corps du courriel a ensuite permis d’identifier une URL suspecte pointant vers un fichier exécutable. L’enrichissement de cet indicateur à l’aide de différentes sources de renseignement sur les menaces a permis de poursuivre l’investigation et d’identifier plusieurs familles de logiciels malveillants associées à la chaîne d’infection, notamment Coinminer, BitRAT et AsyncRAT.
L’analyse a également permis d’identifier plusieurs éléments caractéristiques d’une compromission, notamment des URL de téléchargement, un fichier exécutable utilisé pour la persistance, un hachage SHA-256, un domaine de commande et de contrôle (C2) ainsi qu’un identifiant de bot Telegram. Ces informations constituent des indicateurs de compromission pouvant être utilisés pour approfondir une investigation et contribuer à la détection ou à la réponse à incident.
Ce TP montre également l’importance de ne pas tirer de conclusion à partir d’un seul indicateur. L’utilisation d’une infrastructure appartenant à un fournisseur cloud légitime, par exemple, ne garantit pas que l’activité observée soit légitime et ne justifie pas non plus le blocage systématique de l’ensemble de cette infrastructure. L’investigation doit reposer sur la corrélation de plusieurs éléments techniques et contextuels.
Enfin, ce laboratoire m’a permis de mettre en pratique une méthodologie d’investigation proche de celle utilisée dans un environnement SOC : collecte d’informations, analyse des en-têtes, extraction et enrichissement des IOC, analyse du comportement des malwares, identification des mécanismes de persistance et des communications C2, puis documentation des résultats. Cette approche permet de passer d’un simple courriel suspect à une compréhension plus complète de la chaîne d’attaque et des mesures de détection et de réponse pouvant être mises en œuvre.









































### **1- Prise de connaissance du cas** ###




