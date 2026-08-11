# Analyse d'e-mails d'hameçonnage

## Objectif

L'objectif de ce TP est d'analyser un e-mail suspect à l'aide de plusieurs outils afin de déterminer s'il est malveillant ou non. Pour ce faire, nous utiliserons les ressources de la plateforme BlueDefenders, et plus particulièrement le laboratoire PhishStrike. l' exercice consiste à analyser, enquêter avec différents outils pour  répondre aux questions concernant le cas proposé Nous analyserons les en-têtes de l'e-mail et les renseignements sur les menaces afin d'identifier les indicateurs d'hameçonnage, la persistance du logiciel malveillant et les canaux de commande et de contrôle (C2), d'extraire des indicateurs de compromission (IOC) exploitables et de répondre aux questions connexes.

### Compétences acquises

- Investigating real threat

### Outils utilisés

- VirusTotal
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

Q1 : Identifier l'adresse IP de l'expéditeur grâce à des valeurs SPF et DKIM spécifiques permet de remonter à la source d'un courriel d'hameçonnage. Quelle est l'adresse IP de l'expéditeur dont la valeur SPF est « softfail » et la valeur DKIM « fail » ?

Q2 : Comprendre le chemin de retour d'un courriel est essentiel pour en retracer l'origine. Quel est le chemin de retour spécifié dans ce courriel ?

Q3 : Identifier la source d'un logiciel malveillant est crucial pour une atténuation et une réponse efficaces aux menaces. Quelle est l'adresse IP du serveur hébergeant le fichier malveillant lié à la distribution du logiciel malveillant ?

Q4 : Identifier les logiciels malveillants qui exploitent les ressources système pour le minage de cryptomonnaies est essentiel pour prioriser les efforts d'atténuation des menaces. L'URL malveillante peut diffuser plusieurs types de logiciels malveillants. Quelle famille de logiciels malveillants est responsable du minage de cryptomonnaies ?

Q5 : Identifier les URL spécifiques demandées par le logiciel malveillant est essentiel pour perturber ses canaux de communication et réduire son impact. D'après l'analyse précédente de l'échantillon de logiciel malveillant de cryptomonnaie, quelle URL ce logiciel malveillant demande-t-il ?

Q6 : Comprendre les entrées de registre ajoutées à la clé d’exécution automatique par un logiciel malveillant est crucial pour identifier ses mécanismes de persistance. D’après l’analyse de l’échantillon de logiciel malveillant BitRAT, quel est le nom de l’exécutable dans la première valeur ajoutée à la clé d’exécution automatique du registre ?

Q7 : Identifier le hachage SHA-256 des fichiers téléchargés depuis une URL malveillante est essentiel pour suivre et analyser l’activité des logiciels malveillants. D’après l’analyse de BitRAT, quel est le hachage SHA-256 du fichier précédemment téléchargé et ajouté aux clés d’exécution automatique ?

Q8 : Analyser les requêtes HTTP effectuées par un logiciel malveillant permet d’identifier ses modes de communication. Quelle est l’URL de la requête HTTP utilisée par le chargeur pour récupérer le logiciel malveillant BitRAT ?

Q9 : Introduire un délai dans l’exécution d’un logiciel malveillant peut permettre de contourner les mécanismes de détection. Quel est le délai (en secondes) causé par la commande PowerShell d’après l’analyse de BitRAT ?

Q10 : Suivre les domaines de commande et de contrôle (C2) utilisés par les logiciels malveillants est essentiel pour détecter et bloquer les activités malveillantes. Quel est le domaine C2 utilisé par le malware BitRAT ?

Q11 : Comprendre comment les malwares exfiltrent des données est essentiel pour détecter et prévenir les violations de données. D’après l’analyse d’AsyncRAT, quel est l’identifiant du bot Telegram utilisé par ce malware ?


### **2- Mise en place de Notepad++** ###

Pour analyser le courriel suspicieux, j'utilise Notepad++ avec une configuration précise. Il s' agit de sélectionner le langage **YAML** sous la section language. Ce paramètres permet de distinguer facilement les différentes parties dont entêtes du courriel.

<img width="3072" height="1753" alt="notepad++" src="https://github.com/user-attachments/assets/14863a90-a12e-406e-82c2-4bc6f15c6f57" />

*Image 2: Aperçu de la configuration Notepad++*

Je commence l' investigation par les entêtes **To** et **Subject** qui permet d' avoir des informations respectivement sur le destinataire du courriel et l' objet du courriel. Ces deux entêtes se situent légèrement au dessus de l entête **FROM** qui nous informe sur l' origine du courriel. Dnas ce cas il s' agit de **ERIKA JOHANA LOPEZ VALIENTE <erikajohana.lopez@uptc.edu.co>** . L entete To affiche **undisclosed-recipients:;** donc ne nous donne pas reellement une information claire sur le destinataire du courriel





















### **1- Prise de connaissance du cas** ###































### **1- Prise de connaissance du cas** ###




