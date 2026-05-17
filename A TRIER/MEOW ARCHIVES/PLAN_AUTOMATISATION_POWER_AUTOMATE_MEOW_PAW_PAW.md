# Plan d'automatisation Power Automate / Meow Paw Paw

## 1. Resume

Objectif : mettre en place une chaine semi-automatique fiable entre Microsoft Lists, Outlook, SharePoint et les fichiers JSON de verite Meow Paw Paw.

Principe retenu :

- Power Automate prepare une demande structuree.
- Tu declenches ensuite le traitement manuellement.
- Je traite la demande en priorite.
- J'analyse les impacts systeme.
- Je genere les patchs RFC 6902 par fichier.
- J'applique les modifications sur les JSON si demande.

Pourquoi ce choix :

- tu gardes la main ;
- tu apprends Power Automate sans risque ;
- on fiabilise le format des demandes ;
- on stabilise les regles avant toute automatisation complete ;
- on protege la coherence entre plusieurs fichiers de verite.

## 2. Ce qu'on a decide

### 2.1 Pour les fichiers JSON

Power Automate ne patchera pas directement les fichiers JSON.

Son role sera de :

- detecter un changement ou un evenement ;
- rassembler les informations utiles ;
- deposer une demande structuree dans un dossier dedie ;
- eventuellement notifier qu'une demande est prete.

Mon role sera de :

- lire la demande ;
- l'analyser contre les index, regles, schemas et fichiers applicables ;
- determiner les fichiers reellement impactes ;
- generer les patchs RFC 6902 par fichier ;
- appliquer les modifications si tu me le demandes ;
- signaler les conflits, ambiguities et risques si necessaire.

### 2.2 Pour Microsoft Lists / SharePoint / Outlook

Pour tout ce qui est configuration native Microsoft 365, il vaut mieux utiliser Copilot dans l'interface Microsoft :

- creation de flows Power Automate ;
- mise a jour d'elements de Lists ;
- mise a jour de donnees SharePoint ;
- creation de notifications ;
- synchronisation avec Outlook ;
- approbations, rappels et actions standards.

Regle pratique :

- si c'est de la configuration d'un flow Microsoft : plutot Copilot ;
- si c'est de la logique systeme, des impacts multi-fichiers et des patchs JSON : plutot moi.

## 3. Architecture cible retenue

### 3.1 Chaine de traitement

1. Un evenement se produit dans Microsoft Lists, SharePoint, Outlook ou une autre source Microsoft.
2. Power Automate construit une demande structuree.
3. Power Automate depose cette demande dans un dossier d'entree dedie.
4. Tu me declenches pour traiter les demandes en priorite.
5. J'analyse la demande comme une demande systeme potentiellement multi-fichiers.
6. Je genere les patchs RFC 6902 necessaires.
7. Je modifie les fichiers cibles si tu me le demandes.
8. Le resultat est ensuite classe ou journalise.

### 3.2 Pourquoi on ne fait pas du full auto maintenant

Parce que :

- plusieurs fichiers peuvent devoir etre mis a jour ensemble ;
- les fichiers JSON sont des fichiers de verite ;
- les schemas et les regles doivent rester au centre ;
- tu decouvres encore Power Automate ;
- un mauvais design trop tot serait difficile a reprendre.

## 4. Format de la demande deposee par Power Automate

### 4.1 Principe

La demande ne doit pas etre un simple texte libre.

Elle doit etre un fichier structure, lisible et exploitable. Le format retenu pour commencer est un fichier Markdown structure.

### 4.2 Nomenclature recommandee

Convention proposee :

- `PA_REQ_YYYY-MM-DD_HHMMSS_TITRE.md`

Exemple :

- `PA_REQ_2026-03-31_154500_MAJ_COUT_FOURNISSEUR.md`

### 4.3 Contenu minimal du fichier

Le fichier doit contenir :

- un identifiant de demande ;
- la source ;
- la priorite ;
- le type de changement ;
- le contexte metier ;
- les objets concernes ;
- les fichiers potentiellement impactes ;
- les regles attendues ;
- les contraintes ou interdictions.

### 4.4 Exemple de structure

```md
---
request_id: REQ-2026-03-31-001
source: power_automate
priority: high
change_type: multi_file_sync
mode: generate_and_apply_patches
---

# Contexte
Mise a jour recue depuis Microsoft Lists.

# Objet concerne
- SKU : MPP-BAT-STD-MAT-V1

# Changement demande
- Nouveau cout fournisseur USD : 0.65
- Date effet : 2026-03-31

# Fichiers potentiellement impactes
- SKU.json
- SONGWAYPET.json
- PRICING.json

# Regles attendues
- Aligner le cout fournisseur
- Recalculer les champs derives si necessaire
- Produire des patchs RFC 6902 par fichier
- Refuser si incoherent avec les schemas

# Contraintes
- Ne pas modifier les fichiers hors perimetre justifie
- Signaler toute ambiguite avant application
```

## 5. Instruction de priorite a retenir

La logique a formaliser dans mes instructions est la suivante :

- toute demande issue de Power Automate ;
- ou tout fichier respectant la nomenclature `PA_REQ_*.md` ;
- doit etre traitee comme une demande systeme prioritaire.

Cela signifie :

- analyser la demande contre les index, regles normatives, schemas et fichiers de verite applicables ;
- determiner tous les fichiers reellement impactes ;
- produire des patchs RFC 6902 lorsque pertinent ;
- ne pas modifier de fichiers hors perimetre justifie ;
- signaler les ambiguities et conflits avant application si necessaire.

Important :

"Demande systeme prioritaire" ne veut pas dire "modifier tout le systeme sans filtre".

Cela veut dire :

- considerer que l'impact peut etre systeme ;
- verifier largement ;
- n'appliquer que ce qui est justifie par l'analyse.

## 6. Dossier et cycle de traitement

### 6.1 Structure recommandee

Dossiers recommandes :

- `inbox`
- `processing`
- `done`
- `rejected`
- `logs`

### 6.2 Cycle recommande

1. Power Automate depose une demande dans `inbox`.
2. Tu me demandes de traiter les demandes Power Automate.
3. Je lis les demandes prioritaires.
4. J'analyse les impacts systeme.
5. Je genere les patchs RFC 6902 par fichier.
6. J'applique les modifications si demande.
7. Le dossier ou la trace de resultat peut ensuite etre mis a jour.

## 7. Repartition des roles

### 7.1 Ce qui releve de Copilot

Copilot est le bon outil pour :

- aider a construire les flows dans Power Automate ;
- configurer Microsoft Lists ;
- configurer SharePoint ;
- parametrer Outlook, notifications, approbations et actions Microsoft 365 ;
- suggerer des expressions et des actions dans l'interface Microsoft.

### 7.2 Ce qui releve de moi

Je suis le bon outil pour :

- definir l'architecture du systeme ;
- definir le format des demandes ;
- concevoir la logique multi-fichiers ;
- analyser les impacts entre fichiers de verite ;
- produire les patchs RFC 6902 ;
- verifier les schemas et la coherence systeme ;
- te proposer une methode robuste avant automatisation complete.

## 8. Plan d'execution recommande

### Priorite A - Formalisation immediate

1. Choisir le dossier de depot des demandes Power Automate.
2. Valider la nomenclature `PA_REQ_*.md`.
3. Valider le modele de fichier de demande.
4. Ajouter l'instruction de priorite pour les demandes Power Automate.

### Priorite B - Premier cas d'usage reel

Choisir un seul scenario simple pour demarrer, par exemple :

- un changement dans Microsoft Lists qui doit produire une mise a jour sur un ou plusieurs JSON ;
- ou une mise a jour de statut avec impact Outlook et JSON.

But :

- tester le format ;
- verifier les informations necessaires ;
- identifier les zones floues.

### Priorite C - Stabilisation

1. Ajuster le format des demandes.
2. Ajuster les regles de traitement.
3. Definir les verifications minimales obligatoires.
4. Standardiser les cas d'erreur et de rejet.

### Priorite D - Automatisation plus poussee plus tard

Quand le systeme sera stable :

- soit on reste sur le mode semi-manuel ;
- soit on ajoute un moteur local Python ou PowerShell ;
- soit on met en place un service d'application de patchs.

## 9. Garde-fous obligatoires

Pour proteger le systeme, il faut conserver ces regles :

- pas de patch direct par Power Automate sur les JSON ;
- pas de modifications multi-fichiers sans analyse d'impact ;
- pas de demande libre trop vague ;
- pas de modification hors perimetre justifie ;
- refus ou alerte en cas d'ambiguite ;
- validation contre schemas quand necessaire ;
- patchs RFC 6902 separes par fichier.

## 10. Decision finale retenue

La strategie retenue est :

- **Phase 1 : semi-automatique**
  - Power Automate prepare ;
  - tu me declenches ;
  - je traite la demande.

- **Pour le Microsoft natif**
  - utiliser Copilot pour la configuration des flows, listes, SharePoint et Outlook.

- **Pour le systeme JSON Meow Paw Paw**
  - passer par moi pour la logique, les impacts, les regles, les patchs RFC 6902 et la coherence multi-fichiers.

## 11. Suite logique

Les prochaines etapes utiles sont :

1. definir le dossier cible exact pour les demandes Power Automate ;
2. rediger le modele final de `PA_REQ_*.md` ;
3. rediger l'instruction locale de priorite ;
4. choisir le premier scenario concret a tester ;
5. preparer ensuite le premier flow Power Automate avec Copilot.