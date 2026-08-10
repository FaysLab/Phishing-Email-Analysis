# Analyse d'e-mails d'hameçonnage

## Objectif

L'objectif de ce TP est d'analyser un e-mail suspect à l'aide de plusieurs outils afin de déterminer s'il est malveillant ou non. Pour ce faire, nous utiliserons les ressources de la plateforme BlueDefenders, et plus particulièrement le laboratoire PhishStrike. l' exercice consiste à analyser, enquêter avec différents outils pour  répondre aux questions concernant le cas proposé Nous analyserons les en-têtes de l'e-mail et les renseignements sur les menaces afin d'identifier les indicateurs d'hameçonnage, la persistance du logiciel malveillant et les canaux de commande et de contrôle (C2), d'extraire des indicateurs de compromission (IOC) exploitables et de répondre aux questions connexes.

### Compétences acquises

- Investigating real threat

### Outils utilisés

- VirusTotal
- MalwareBazaar
- Email Header Analyzer

## Étapes du TP
### **1- Prise de connaissance du cas** ###

Pour débuter on prend connaissance du scenario proposé. L'image ci dessous résume 

<img width="1833" height="539" alt="scenario" src="https://github.com/user-attachments/assets/79be6122-00fa-400d-ac12-125bc1c9630a" />

*Image 1: Scénario*

Les questions auxquelles on doit répondre après investigation sont les suivantes:

Q1 : Identifying the sender's IP address with specific SPF and DKIM values helps trace the source of the phishing email. What is the sender's IP address that has an SPF value of softfail and a DKIM value of fail?

Q2: Understanding the return path of an email is essential for tracing its origin. What is the return path specified in this email?

Q3: Identifying the source of malware is critical for effective threat mitigation and response. What is the IP address of the server hosting the malicious file related to malware distribution?

Q4: Identifying malware that exploits system resources for cryptocurrency mining is critical for prioritizing threat mitigation efforts. The malicious URL can deliver several malware types. Which malware family is responsible for cryptocurrency mining?

Q5: Identifying the specific URLs malware requests is key to disrupting its communication channels and reducing its impact. Based on the previous analysis of the cryptocurrency malware sample, what does this malware request the URL?

Q6: Understanding the registry entries added to the auto-run key by malware is crucial for identifying its persistence mechanisms. Based on the BitRAT malware sample analysis, what is the executable's name in the first value added to the registry auto-run key?

Q7: Identifying the SHA-256 hash of files downloaded from a malicious URL is essential for tracking and analyzing malware activity. Based on the BitRAT analysis, what is the SHA-256 hash of the file previously downloaded and added to the autorun keys?

Q8: Analyzing the HTTP requests made by malware helps in identifying its communication patterns. What is the URL in the HTTP request used by the loader to retrieve the BitRAT malware?

Q9: Introducing a delay in malware execution can help evade detection mechanisms. What is the delay (in seconds) caused by the PowerShell command according to the BitRAT analysis?

Q10: Tracking the command and control (C2) domains used by malware is essential for detecting and blocking malicious activities. What is the C2 domain used by the BitRAT malware?

Q11: Understanding how malware exfiltrates data is essential for detecting and preventing data breaches. According to the AsyncRAT analysis, what is the Telegram Bot ID used by this malware?

































