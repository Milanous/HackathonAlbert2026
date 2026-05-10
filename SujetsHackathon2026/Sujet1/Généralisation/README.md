**Avec les fichiers :**

+ Extraction MITRE ATT&CK du 7 mai, domaine “Entreprise”
  + Fichier fournit
  + Source : https://raw.githubusercontent.com/mitre-attack/attack-stix-data/master/enterprise-attack/enterprise-attack.json

+ CVE : à récupérer
  + Format : sur github, CVEPproject/cve-schema
  + Source : https://www.cve.org/Downloads, cvelistv5-main.zip


1. Généralisez la démarche avec une extraction automatique quotidienne du fichier (utilisez le cas échéant l’API TAXII de MITRE ou la bibliothèque "attackcti", pyattack ou stix2) et une association TTP/CVE
2. Testez la démarche avec une base de logs plus réalistes synthétiques ou anonymisés 
3. Explorez l’analyse textuelle (NLP), l’utilisation de bases de données tierces,…
4. Donner un score à la relation TTP/CVE : score de confiance, score d’impact sur l’environnement SI d’après les logs; …
5. Comment identifier les CVE critiques pour nous ?


## Pour les équipes avec des étudiants de l'école 42 et des compétences en yaml : génération des politiques de logs 

Une fois l’étape précédente maîtrisée/en supposant maitrisée l’étape précédente, avec les données fournies
Générer des politiques de logs (au format YAML/JSON) qui définissent quels logs collecter pour détecter les TTPs (Techniques, Tactiques, Procédures) et CVE identifiées dans les étapes précédentes et automatiser la détection des menaces.

Etapes :
1. Découvrir quels logs sont nécessaires pour détecter chaque TTP/CVE
    + Log de processus, registres windows, logs web, logs de connexion, log de tâches, log réseau, etc.
    + Utilisez pour cela la chaine logique : vulnérabilité -> est détectée via l’indicateur  indicateur relevé dans un log pour chaque système : windows, linux, réseau, etc. 
    + Créer une base de connaissances des logs par TTP/CVE.
2. Générer les politiques automatiquement
    + Utilisez le template Jinja2 ci-dessous pour générer les politiques YAML à partir des TTPs, ou un autre template de votre choix
3. Scripter pour traiter toutes les TTPs critiques : au dessus d’un certain score, pour traiter les TTPs concernés par le SI de l’organisation (cf. les log d’événements), les tactiques d’attaques les plus utilisées, etc..


```yaml
from jinja2 import Template
import yaml

# Template Jinja2 pour les politiques
policy_template = """
policy:
  name: "Detect {{ ttp_id }} - {{ ttp_name }}"
  id: "{{ ttp_id }}"
  description: "{{ ttp_description }}"
  severity: "{{ severity }}"
  mitre_attack_tactic: "{{ tactic }}"
  systems: {{ systems }}
  logs: {{ logs }}
"""

# Exemple de données pour T1059
ttp_data = {
    "ttp_id": "T1059",
    "ttp_name": "Command and Scripting Interpreter",
    "ttp_description": "Collect process logs to detect command execution (PowerShell, Cmd, Bash).",
    "severity": "high",
    "tactic": "execution",
    "systems": ["Windows", "Linux"],
    "logs": [
        {
            "type": "process",
            "source": "sysmon",
            "fields": ["CommandLine", "ProcessName", "ParentProcessName"],
            "retention": "30 days"
        },
        {
            "type": "process",
            "source": "auditd",
            "fields": ["exe", "cmdline"],
            "retention": "30 days"
        }
    ]
}

# Remplir le template
template = Template(policy_template)
policy_yaml = template.render(**ttp_data)

# Sauvegarder dans un fichier
with open(f"policies/{ttp_data['ttp_id']}.yaml", "w") as f:
    f.write(policy_yaml)

print(f"Politique générée pour {ttp_data['ttp_id']}:")
print(policy_yaml)


