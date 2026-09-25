# \# 🏥 Redis pour la Santé Numérique

# \## Plateforme de Surveillance Épidémiologique

# 

# !\[Redis](https://img.shields.io/badge/Redis-7.x-DC382D?style=for-the-badge\&logo=redis\&logoColor=white)

# !\[Docker](https://img.shields.io/badge/Docker-29.8-2496ED?style=for-the-badge\&logo=docker\&logoColor=white)

# !\[Santé](https://img.shields.io/badge/Domaine-Santé\_Numerique-4CAF50?style=for-the-badge)

# !\[Statut](https://img.shields.io/badge/Statut-Terminé-success?style=for-the-badge)

# 

# \---

# 

# \## 📋 Contexte

# 

# Dans le cadre de ma montée en compétences en \*\*Big Data et NoSQL\*\*, j'ai conçu une \*\*plateforme de surveillance épidémiologique\*\* utilisant \*\*Redis\*\* comme base de données clé-valeur.

# 

# Ce projet simule le backend d'une application utilisée par le \*\*Ministère de la Santé du Sénégal\*\* pour :

# \- Suivre en temps réel les cas suspects

# \- Classer les pathologies par fréquence

# \- Gérer les sessions des agents de santé

# \- Diffuser des alertes épidémiques persistantes

# \- Stocker les dossiers patients en JSON natif

# 

# \*\*Pourquoi Redis ?\*\* Sa rapidité (100 000+ opérations/seconde), ses structures de données riches, et son support du TTL en font l'outil idéal pour les systèmes de santé temps réel.

# 

# \---

# 

# \## 🎓 Objectifs pédagogiques

# 

# | Objectif | Statut |

# |---|---|

# | Maîtriser les 5 structures de données Redis | ✅ |

# | Comprendre le positionnement CAP | ✅ |

# | Implémenter des sessions avec TTL | ✅ |

# | Utiliser Pub/Sub pour les alertes | ✅ |

# | Explorer Redis Streams (persistance) | ✅ |

# | Manipuler RedisJSON (JSONPath) | ✅ |

# | Documenter et versionner sur GitHub | ✅ |

# 

# \---

# 

# \## 🏗️ Architecture

# ┌─────────────────────────────────────────────────────────┐

# │ PLATEFORME DE SANTÉ NUMÉRIQUE │

# ├─────────────────────────────────────────────────────────┤

# │ │

# │ ┌──────────────┐ ┌──────────────┐ ┌──────────┐ │

# │ │ Agents de │───▶│ Redis │───▶│Dashboard │ │

# │ │ santé │ │ (Cache + │ │ temps │ │

# │ └──────────────┘ │ Temps réel)│ │ réel │ │

# │ └──────────────┘ └──────────┘ │

# │ │ │

# │ ▼ │

# │ ┌──────────────┐ │

# │ │ MongoDB │ │

# │ │ (Persistance)│ │

# │ └──────────────┘ │

# │ │

# └─────────────────────────────────────────────────────────┘

# 

# text

# 

# \---

# 

# \## 🗂️ Structures de données utilisées

# 

# | Structure | Clé Redis | Cas d'usage santé |

# |---|---|---|

# | \*\*String\*\* | `session:{id}` | Session agent (JSON + TTL) |

# | \*\*String\*\* | `consultations\_jour` | Compteur quotidien (INCR) |

# | \*\*Hash\*\* | `patient:{id}` | Dossier patient structuré |

# | \*\*List\*\* | `file\_attente` | File d'attente FIFO |

# | \*\*List\*\* | `file\_urgences` | File prioritaire |

# | \*\*Sorted Set\*\* | `classement\_pathologies` | Classement des pathologies |

# | \*\*Stream\*\* | `alertes\_epidemiques` | Alertes persistantes |

# | \*\*JSON\*\* | `patient:{id}` | Dossier JSON natif |

# 

# \---

# 

# \## ⚙️ Installation

# 

# \### Prérequis

# 

# \- Docker Desktop (Windows/Mac) ou Docker Engine (Linux)

# \- Un terminal (PowerShell, Bash)

# 

# \### Lancer Redis Stack

# 

# ```bash

# docker run -d --name redis-stack \\

# &#x20; -p 6379:6379 \\

# &#x20; -p 8001:8001 \\

# &#x20; redis/redis-stack:latest

# Se connecter au CLI

# bash

# docker exec -it redis-stack redis-cli

# 🧪 Démonstration pratique

# 1️⃣ Sessions agents (TTL)

# bash

# SET session:AGENT-DAKAR-01 "{\\"agent\_id\\":1,\\"nom\\":\\"Dr Aminata Fall\\"}" EX 3600

# TTL session:AGENT-DAKAR-01

# 2️⃣ Compteur de consultations

# bash

# INCR consultations\_jour

# EXPIRE consultations\_jour 86400

# 3️⃣ Dossier patient (Hash)

# bash

# HSET patient:100 nom "Aliou Diop" age 45 ville "Dakar" pathologie "Paludisme"

# HGETALL patient:100

# 4️⃣ Classement des pathologies

# bash

# ZADD classement\_pathologies 45 "Paludisme"

# ZADD classement\_pathologies 32 "Diarrhee"

# ZREVRANGE classement\_pathologies 0 -1 WITHSCORES

# 🌊 Redis Streams : Messagerie persistante

# bash

# XADD alertes\_epidemiques \* region "Saint-Louis" maladie "Paludisme" cas 45

# XGROUP CREATE alertes\_epidemiques groupe\_ministere 0

# XREADGROUP GROUP groupe\_ministere agent\_1 COUNT 1 STREAMS alertes\_epidemiques >

# XACK alertes\_epidemiques groupe\_ministere <ID>

# XPENDING alertes\_epidemiques groupe\_ministere

# Avantage santé : aucune alerte n'est perdue, même en cas de déconnexion.

# 

# 📦 RedisJSON : Manipulation JSON native

# bash

# JSON.SET patient:200 $ '{"nom":"Fatou Ndiaye","age":42,"pathologies":\["Paludisme"]}'

# JSON.ARRAPPEND patient:200 $.pathologies '"Anemie"'

# JSON.GET patient:200

# 🏆 Compétences acquises

# Techniques

# ✅ Déploiement de Redis via Docker

# 

# ✅ Maîtrise des 5 structures Redis

# 

# ✅ Gestion du TTL et de l'expiration

# 

# ✅ Pub/Sub et Streams pour la messagerie

# 

# ✅ RedisJSON pour le stockage JSON natif

# 

# ✅ JSONPath pour l'interrogation

# 

# Métier (Santé)

# ✅ Modélisation d'un système de surveillance épidémiologique

# 

# ✅ Gestion de sessions agents sécurisées

# 

# ✅ Compteurs temps réel de consultations

# 

# ✅ Files d'attente prioritaires (urgences)

# 

# ✅ Alertes persistantes pour le Ministère

# 

# 📚 Ressources

# Ressource	Lien

# Documentation Redis	redis.io/docs

# Try Redis	try.redis.io

# Redis Streams	redis.io/topics/streams-intro

# RedisJSON	redis.io/docs/stack/json

# 👩‍💻 Auteur

# Khadidiatou NDIAYE

# 

# 🎓 Master 2 Sciences des Données et Applications (Spécialité Statistique)

# 

# 🏛️ Université Gaston Berger de Saint-Louis

# 

# 🔬 Expérience : Institut Pasteur de Dakar (Biostatistique)

# 

# 🎯 Objectif : Data Scientist Santé | Big Data \& NoSQL

# 

# Contact :

# 

# 💼 LinkedIn

# 

# 🐙 GitHub

# 

# 📄 Licence

# Ce projet est sous licence MIT — libre d'utilisation pour l'enseignement et la recherche.

# 

# ⭐ Si ce projet vous a été utile, n'hésitez pas à lui donner une étoile !