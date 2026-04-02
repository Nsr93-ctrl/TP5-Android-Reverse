# TP 5 - Reverse Engineering Android IoT
### InjuredAndroid - Analyse de securite mobile

**Auteur : Tasnim LAIOUAR**

---

## Description

Ce depot contient l'analyse complete de l'application Android vulnerable **InjuredAndroid v1.0.12**.
L'objectif est d'extraire des donnees sensibles, identifier des vulnerabilites et bypasser l'authentification via des techniques de reverse engineering statique et dynamique.

---

## Structure du depot

```
TP-Android-Reverse/
├── rapport_TP5.md              # Rapport complet avec captures
├── frida_scripts/              # Scripts Frida pour l'analyse dynamique
│   ├── hook_flag1.js           # Hook Flag 1 - credential hardcode
│   ├── hook_flag4.js           # Hook Flag 4 - decodage Base64
│   ├── hook_flag5_broadcast.js # Hook Flag 5 - BroadcastReceiver DES
│   ├── hook_flag6_des.js       # Hook Flag 6 - dechiffrement DES
│   └── hook_bypass_auth.js     # Bypass global authentification
├── captures/                   # Captures des etapes d'analyse statique
│   ├── 01_jadx_version.png
│   ├── 02_decompilation.png
│   ├── 03_activites_exportees.png
│   ├── 04_flag1_hardcode.png
│   ├── 05_flag3_strings.png
│   ├── 06_flag4_base64.png
│   ├── 07_cles_des.png
│   ├── 08_flag5_des.png
│   ├── 09_flag6_des.png
│   ├── 10_flag7_firebase.png
│   ├── 11_flag8_aws.png
│   └── 12_activites_obfusquees.png
└── objection_captures/         # Captures Phase 3 - Objection
    ├── commandes_objection.txt
    ├── 13_objection_launch.png
    ├── 14_objection_root_disable.png
    ├── 15_objection_ssl_pinning.png
    ├── 16_objection_list_activities.png
    ├── 17_objection_launch_activity.png
    ├── 18_objection_shared_prefs.png
    ├── 19_objection_filesystem.png
    └── 20_objection_hook_method.png
```

---

## Outils utilises

| Outil | Version | Usage |
|-------|---------|-------|
| jadx | 1.5.0 | Decompilation APK -> Java |
| Frida | 16.x | Analyse dynamique, hooking |
| Objection | 1.11.0 | Bypass root/SSL, exploration runtime |
| ADB | - | Communication avec l'emulateur |

---

## Phases du TP

### Phase 1 - Analyse statique
Decompilation de l'APK avec jadx et analyse du code source pour extraire :
- Credentials hardcodes
- Cles de chiffrement DES
- Activites exportees sans permission
- Endpoints Firebase publics

### Phase 2 - Analyse dynamique avec Frida
Hooking des methodes critiques pour intercepter les comparaisons de flags et bypasser l'authentification en temps reel.

### Phase 3 - Bypass avec Objection
Desactivation du root detection et du SSL pinning, exploration du systeme de fichiers et des SharedPreferences, lancement direct des activites exportees.

---

## Flags decouverts

| Flag | Valeur | Technique |
|------|--------|-----------|
| Flag 1 | `F1ag_0n3` | Hardcode dans le code source |
| Flag 3 | `F1ag_thr33` | Ressource strings.xml |
| Flag 4 | `4_overdone_omelets` | Decodage Base64 |
| Flag 5 | `{F1v3!}` | Dechiffrement DES |
| Flag 6 | `{This_Isn't_Where_I_Parked_My_Car}` | Dechiffrement DES |
| Flag 7 | `S3V3N_11` | Firebase endpoint public |
| Flag 8 | `C10ud_S3cur1ty_lol` | Firebase /aws public |

---

## Vulnerabilites identifiees

| Severite | Vulnerabilite | CWE |
|----------|--------------|-----|
| CRITIQUE | Credentials hardcodes dans le code source | CWE-798 |
| CRITIQUE | Cles DES hardcodees en Base64 | CWE-321 |
| HAUTE | Chiffrement DES obsolete (56-bit) | CWE-327 |
| HAUTE | Activites exportees sans permission | CWE-926 |
| HAUTE | Firebase endpoint public sans auth | CWE-284 |
| HAUTE | Donnees sensibles en clair dans SQLite | CWE-312 |
| MOYENNE | Obfuscation faible (Base64, ROT47) | CWE-261 |
| MOYENNE | BroadcastReceiver expose | CWE-926 |
