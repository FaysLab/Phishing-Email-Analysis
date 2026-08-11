# Analyse d'e-mails d'hameçonnage

## Objectif

L'objectif de ce TP est d'analyser un e-mail suspect à l'aide de plusieurs outils afin de déterminer s'il est malveillant ou non. Pour ce faire, nous utiliserons les ressources de la plateforme BlueDefenders, et plus particulièrement le laboratoire PhishStrike. l' exercice consiste à analyser, enquêter avec différents outils pour  répondre aux questions concernant le cas proposé Nous analyserons les en-têtes de l'e-mail et les renseignements sur les menaces afin d'identifier les indicateurs d'hameçonnage, la persistance du logiciel malveillant et les canaux de commande et de contrôle (C2), d'extraire des indicateurs de compromission (IOC) exploitables et de répondre aux questions connexes.

### Compétences acquises

- Investigating real threat

### Outils utilisés

- VirusTotal
- Malpedia
- Urlhauss.abuse.ch
- Notepad++
- AlienVault
- DomainTools
- MalwareBazaar
- Email Header Analyzer

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

On observe sur l' image 3, un "*softfail**" du SPF et l' adresses IP de l' envoyeur est 18.208.22.104. On peut ensuite passer au corps du courriel.

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

Enfin **Coinminer**  est un logiciel malveillant indésirable qui utilise la puissance de calcul de la victime (principalement le processeur et la mémoire vive) pour miner des cryptomonnaies (par exemple, Monero ou Zcash). Ce logiciel malveillant assure sa persistance en installant un mineur open source au démarrage, sans l'accord de la victime. Les mineurs de cryptomonnaies les plus sophistiqués utilisent des paramètres de minuterie ou limitent l'utilisation du processeur pour rester discrets.


<img width="2594" height="1625" alt="details du malware" src="https://github.com/user-attachments/assets/24c379a3-9a4e-4e0d-8a21-4b5f4e0c6924" />

 *Image 7: Details du malware*















### **1- Prise de connaissance du cas** ###































### **1- Prise de connaissance du cas** ###




