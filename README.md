# Planche photos d'identité

Générateur de planches de **photos d'identité** prêtes à imprimer, au bon format
officiel selon le pays et le document (France / Canada). Un seul fichier HTML,
**100 % local, hors-ligne, sans installation et sans envoi de la photo sur
Internet**.

> Choisis le document, importe une photo, choisis le nombre de tirages, imprime
> sur A4 à taille réelle. Option fond blanc automatique par IA, exécutée dans le
> navigateur.

## Pourquoi cet outil

- **Gratuit et sans compte.** Aucune limite, aucun filigrane, aucune clé API.
- **Privé.** La photo ne quitte jamais ton ordinateur (tout se fait dans le navigateur).
- **Hors-ligne.** Un seul fichier `index.html`, double-clic, ça marche sans connexion.
- **Tailles officielles vérifiées** (France ANTS / Canada IRCC), pas devinées.
- **Impression à la taille réelle exacte** (dimensionnement en millimètres natif).

## Fonctionnalités

- Presets par **besoin documentaire** : la taille du masque s'adapte automatiquement.
- **Une photo dupliquée** sur toute la planche, masque au format exact.
- **Disposition** au choix : 4, 6, 8, 10, 12, 20, ou grille personnalisée (colonnes × lignes).
- **Formats papier** : A4 (défaut), A5, 10 × 15 cm, Lettre US. Marge et espacement réglables.
- **Cadrage** du visage : zoom et position (curseurs, ou glisser à la souris + molette).
- **Traits de coupe** imprimables ; **repère visage** (zone menton-crâne) affiché à l'écran.
- **Fond blanc automatique (IA)** optionnel, 100 % local.
- Alerte si la grille dépasse la zone imprimable.

## Besoins documentaires (tailles officielles)

| Besoin | Pays | Photo | Visage (menton-crâne) | Fond |
|---|---|---|---|---|
| CNI / Passeport / Permis | France | 35 × 45 mm | 32-36 mm | uni clair (blanc interdit) |
| Passeport | Canada | **50 × 70 mm** | 31-36 mm | blanc uni |
| Carte de résidence permanente | Canada | 35 × 45 mm | 31-36 mm | blanc uni |
| Citoyenneté | Canada | 35 × 45 mm | 31-36 mm | blanc uni |
| Visa / Immigration IRCC | Canada | 35 × 45 mm | 31-36 mm | blanc uni |
| Taille personnalisée | - | au choix (20-100 mm) | ~70-80 % indicatif | - |

Sources : France [service-public.fr](https://www.service-public.fr) / ANTS (ISO/IEC 19794-5) ;
Canada [canada.ca passeport](https://www.canada.ca/en/immigration-refugees-citizenship/services/canadian-passports/photos.html)
et fiche photo IRCC (`photospecs-e.pdf`).

## Prérequis

Un navigateur moderne (Chrome, Edge, Firefox ou Safari récent). Rien d'autre à installer.

## Installation

Trois façons, au choix :

1. **Le plus simple** : télécharger le fichier [`index.html`](index.html)
   (bouton « Raw » puis enregistrer), puis double-cliquer dessus.
2. **Télécharger le ZIP** : bouton vert *Code* → *Download ZIP*, décompresser,
   double-cliquer sur `index.html`.
3. **Cloner** :
   ```bash
   git clone https://github.com/CustomDigitalServices-Kevin/photo-identite.git
   cd photo-identite
   # puis ouvrir index.html dans le navigateur
   ```

Aucune compilation, aucune dépendance à installer.

## Utilisation

1. Ouvre `index.html` (double-clic).
2. **Besoin documentaire** : choisis le document (France / Canada). La taille du masque s'adapte.
3. **Choisis une photo** (clic ou glisser-déposer). La même photo remplit toute la grille.
4. **Disposition** : nombre de photos / grille.
5. **Cadre** le visage (zoom + position).
6. **Imprime la planche** ; dans la boîte de dialogue, règle :
   - **Échelle = 100 % (Taille réelle)** — surtout pas « Ajuster à la page »
   - **Marges = Aucune**
   - le bon **format papier** (A4 par défaut)

> L'échelle 100 % et les marges « Aucune » sont indispensables pour que les
> photos sortent exactement aux bonnes dimensions en millimètres.

## Fond blanc automatique (IA, optionnel)

Le bouton **Fond blanc auto (IA)** détoure le sujet et le pose sur fond blanc
(utile pour le passeport / immigration Canada, où le fond blanc est exigé).

- Modèle **MODNet** (licence **Apache 2.0**) via
  [Transformers.js](https://github.com/huggingface/transformers.js), exécuté
  **dans le navigateur** : la photo ne quitte jamais la machine.
- **Gratuit, sans quota, sans clé API.**
- 1er usage : téléchargement du modèle (**~26 Mo**) puis mise en cache par le
  navigateur (réutilisable hors-ligne). Détourage en **1 à 2 s**.
- Bouton **Revenir à l'original** pour annuler.

**Limite** : un détourage IA peut laisser des artefacts autour des cheveux et ne
corrige ni l'éclairage ni les ombres. C'est un confort, **pas une garantie de
conformité officielle**, en particulier pour le passeport canadien (autorité
stricte). Vérifie toujours le rendu.

> **Note** : la planche et l'impression fonctionnent en double-clic
> (`file://`). En revanche, certains navigateurs (Chrome notamment) bloquent le
> chargement du modèle IA depuis un fichier ouvert en double-clic. Si le bouton
> **Fond blanc auto** reste sans effet, sers le dossier via un petit serveur
> local puis ouvre `http://localhost:8000` :
> ```bash
> cd photo-identite
> python -m http.server 8000
> ```

## Confidentialité

Tout se passe en local dans ton navigateur : la photo n'est ni uploadée, ni
envoyée à un serveur, ni stockée ailleurs. Le seul accès réseau est le
téléchargement unique du modèle IA (si tu utilises le fond blanc auto) depuis le
CDN public jsDelivr / Hugging Face ; ensuite il est mis en cache.

## Détails techniques

- Tout est dimensionné en **`mm` CSS natif** : à 100 % d'échelle, 1 mm CSS = 1 mm
  physique. Une cellule de `35mm` mesure 132,28 px à 96 dpi (= 35 / 25,4 × 96).
- Les photos sont des balises `<img>` : elles s'impriment sans avoir à activer
  « Graphiques d'arrière-plan ».
- La règle `@page { size: <w>mm <h>mm; margin: 0 }` est injectée dynamiquement
  selon le format papier, et la feuille imprimée est réduite de 1 px pour éviter
  qu'une page exactement à la taille du papier ne déborde sur une 2e page.
- Le repère visage est un guide **écran uniquement**, il ne s'imprime jamais.

## Capacité A4

Photos 35 × 45 mm sur A4 (210 × 297) : maximum théorique 6 colonnes × 6 lignes =
36 vignettes (avant marges). Les dispositions 4 / 6 / 12 passent très largement.

## Licence

Code du projet sous licence **MIT** (voir [`LICENSE`](LICENSE)).

Le modèle IA **MODNet** (chargé à l'exécution, non inclus dans ce dépôt) est sous
licence Apache 2.0, et **Transformers.js** sous Apache 2.0.

## Crédits

- [MODNet](https://github.com/ZHKKKe/MODNet) — matting de portrait (Apache 2.0)
- [Transformers.js](https://github.com/huggingface/transformers.js) — exécution du modèle dans le navigateur (Apache 2.0)

Développé par Kevin Tomas / Custom Digital Services.
