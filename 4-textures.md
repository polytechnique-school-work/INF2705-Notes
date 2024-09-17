### Résumé Complet du Cours INF2705 – Cours 04 – Textures

#### Objectifs du cours

- Utiliser des textures sur des objets 3D.
- Définir des coordonnées de textures.
- Définir des textures multi-niveaux (mip-maps).
- Utiliser des panneaux (billboards) et lutins (sprites).

---

### Concepts et Définitions

#### Texture

- **Définition** : Ajout d’une image sur une surface 3D pour améliorer le réalisme visuel.
- **Avantage** : Permet de donner l'illusion de détails sur des surfaces géométriquement simples.
- Les textures peuvent être des images 1D, 2D ou même 3D chargées dans une unité de texture pour colorier un objet 3D.

#### Texel

- **Définition** : Élément de texture équivalent à un pixel dans une image.
- **Fonction** : Utilisé pour définir la couleur d’un fragment (partie d’un fichier de texture) sur une surface 3D.

---

### Processus d'Utilisation des Textures

1. **Charger une Image en Mémoire**

   - Via un fichier externe ou une création directe en mémoire.
   - Formats communs : bmp, jpg, png.

2. **Activer une Unité de Texture**

   - Choisir une unité de texture pour l’image.

3. **Associer une Image à l’unité de Texture Active**

   - Lier la texture à l’unité active.

4. **Colorer un Objet avec la Texture**
   - Par spécification des coordonnées de texture (u,v).

---

#### Code d'Exemple pour Charger une Texture

```cpp
void ChargerTexture(std::string fichier, GLuint &texID)
{
    GLsizei largeur, hauteur;
    unsigned char *pixels = ChargerImage(fichier, largeur, hauteur);
    glGenTextures(1, &texID);
    glActiveTexture(GL_TEXTURE0);
    glBindTexture(GL_TEXTURE_2D, texID);
    glTexImage2D(GL_TEXTURE_2D, ..., pixels);
    delete [] pixels;
}
```

#### Spécifications

- **glGenTextures** : Génère des noms pour des textures.
- **glActiveTexture** : Active l'unité de texture spécifiée.
- **glBindTexture** : Lie une texture à la cible spécifiée.
- **glTexImage2D** : Spécifie une image de texture 2D.

---

### Coordonnées de Texture

#### Projection

- Utilisation des coordonnées (u,v) pour projeter les texels sur une surface 3D (x,y,z).
- **Projections courantes** :
  - Planaire
  - Cylindrique
  - Sphérique

#### Utilisation des Cordonnées (u, v)

- Assigner des coordonnées variant de 0.0 à 1.0 pour chaque dimension.
- Coordonnées spécifiées typiquement avec `glVertexAttribPointer()`.

#### Espace de Texture et Répétition

- Contrôle du traitement de répétition des coordonnées avec :
  - **GL_REPEAT**
  - **GL_CLAMP_TO_EDGE**
  - **GL_MIRRORED_REPEAT**

---

### Filtres et Niveaux de Détails

#### Magnification et Minification

- **Minification** : Un pixel couvre plusieurs texels.
- **Magnification** : Un texel couvre plusieurs pixels.

#### Interpolation

- **glTexParameterf** : Définir le type d’interpolation (bi-linéaire, plus proche).

#### Mip-maps

- **Définition** : Version multi-résolution d’une texture utilisée pour améliorer le rendu à différentes distances.
- **Avantages** : Diminue le nombre d'interpolations et améliore l'efficacité du rendu.

```cpp
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST_MIPMAP_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
glGenerateMipmap(GL_TEXTURE_2D);
```

---

### Utilisation de Panneaux et Lutins (Billboards et Sprites)

- **Panneau** : Une texture appliquée sur un plan aligné à la caméra. Utile pour des objets complexes comme des arbres ou des nuages.
- **Lutin** : Image animée sur un panneau pour des effets répétitifs comme des particules de feu.

---

### Syntaxe GLSL : Accès aux Textures

- Les textures sont accédées dans GLSL par des types opaque comme `sampler2D`.
- Utilisation typique dans les nuanceurs de sommets et de fragments pour extraire la couleur `vec4` d’une texture.

```glsl
uniform sampler2D laTexture;
vec4 couleur = texture( laTexture, TexCoord.st );
```

- **Fonctions utiles** :
  - **`textureSize`** : Donne la taille de la texture.
  - **`textureOffset`** : Décaler l’échantillon de texture.

---

### Intégration dans le Programme Principal

- Générer une texture et lier la texture avec des coordonnées de texture.
- Assigner les paramètres de traitement et de filtres.

```cpp
glVertexAttribPointer(locTexCoord, ...);
glTexParameteri(..., GL_TEXTURE_WRAP_S, ...);
glTexParameteri(..., GL_TEXTURE_MIN_FILTER, ...);
```

---

Ce résumé présente un panorama détaillé du processus de gestion des textures dans un contexte 3D, couvrant à la fois les aspects théoriques comme pratiques, y compris des échantillons de code pour une compréhension plus approfondie.
