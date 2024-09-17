### Résumé du Cours INF2705 – Cours 01 – Introduction et Bases

#### Objectifs Principaux

- Introduction aux concepts de bases de l'infographie et d'OpenGL.
- Présentation d'un exemple de démarrage en infographie 3D.
- Différenciation entre les primitives graphiques et le contexte graphique.

---

### Concepts et Commandes Importantes

#### Primitives Graphiques

- **Définition** : Formes de base que l'on dessine (ex. points, lignes, triangles).
- **OpenGL Commandes** :
  - **glBegin() et glEnd()** : Entourent le code définissant les points.
  - **glVertex3f(x,y,z)** : Définit un sommet à la position (x, y, z).
  - **glPointSize(taille)** : Définit la taille des points.
  - **glLineWidth(largeur)** : Définit la largeur des lignes.

#### Contexte Graphique

- **Définition** : Ensemble des paramètres qui déterminent comment les primitives graphiques sont dessinées (couleurs, épaisseur, type de trait, etc.).
- **OpenGL Commandes** :
  - **glColor3f(r, g, b)** : Définit la couleur actuelle.
  - **glClearColor(r, g, b, alpha)** : Définit la couleur de nettoyage (couleur de fond).
  - **glClear(GL_COLOR_BUFFER_BIT)** : Efface l'écran avec la couleur définie.
  - **glPolygonMode(face, mode)** : Détermine comment les surfaces des polygones doivent être remplies ou dessinées.

---

### Historique des Librairies Graphiques

- Premières versions d'outils et évènements importants, sans aller dans les détails historiques, montrent la progression vers OpenGL.
- **Évolution** :
  - De **IrisGL** à **OpenGL 1.0** en 1992 : Standard unifié pour toutes les plateformes.

---

### Premiers Pas en Infographie 3D

- **Concepts Clés** : Construction de scènes 3D animées par réaffichage constant.
- **Physiologie Humaine de la Vision** :
  - Fréquence d’échantillonnage visuel humain : ~30 images/sec à maintenir pour la fluidité.
  - Animation continue en actualisant régulièrement l'écran (5-10 mises à jour/sec au minimum).
- **Exemple de Code** :
  ```cpp
  glClear(GL_COLOR_BUFFER_BIT); // Effacer la mémoire de trame
  glBegin(GL_TRIANGLES);        // Commencer à définir des triangles
  glVertex3f(0.0, 1.0, 0.0);    // Premier sommet
  glVertex3f(-1.0, -1.0, 0.0);  // Deuxième sommet
  glVertex3f(1.0, -1.0, 0.0);   // Troisième sommet
  glEnd();                      // Fin de définition des triangles
  glFlush();                    // S'assurer que toutes les commandes OpenGL soient exécutées
  ```

---

### Séparation des Primitives Graphiques et du Contexte Graphique

#### Primitives Géométriques – Modes

- Points, segments de traits divers (indépendants, polylignes), polygones vides ou remplis.
- **Triangles** : Primaires et utilisés pour composer des polygones plus complexes.

#### Contexte Graphique

- **Couleurs et Dimensions** :
  - **glColor3f(r, g, b)** : Définir la couleur courante.
  - **glLineWidth(width)** : Définir l'épaisseur d'une ligne.
  - **glPointSize(size)** : Définir la taille d'un point.
- **Remplissage de Polygones** :
  - **glPolygonMode(face, mode)** : Définir le type de rendu (GL_POINT, GL_LINE, GL_FILL).
- **Filtrage des Faces** :
  - **glFrontFace(mode)** : Détermine l'orientation de la face avant (GL_CCW pour antihoraire, GL_CW pour horaire).
  - **glCullFace(mode)** : Filtrage par face (GL_FRONT, GL_BACK).

#### Variables d'État

- Commandes pour gérer les variables d'état de contexte graphique.
  - **glEnable() / glDisable()** : Activer ou désactiver certaines fonctionnalités (ex. GL_LIGHTING).
  - **glGetFloatv(), glGetIntegerv()** : Accéder aux valeurs des variables d'état.

**Note : Les explications données ici peuvent inclure des exemples et informations supplémentaires non présentes directement sur les diapositives pour une meilleure compréhension.**

---

### Points Importants sur l’OpenGL Actuel (Post OpenGL 4.x)

- **Programmation Moderne** : Utilisation de shaders et des attributs de sommet pour de meilleures performances et flexibilité.
  - **Shaders** : Programmes exécutés sur le GPU pour les opérations de rendu.
  - **glVertexAttribPointer()** : Spécifie les attributs de sommet pour les shaders.
- **Exemple de Code pour Attributs de Sommet** :
  ```cpp
  glEnableVertexAttribArray(locVertex);
  glBindBuffer(GL_ARRAY_BUFFER, vboSommets);
  glBufferData(GL_ARRAY_BUFFER, sizeof(sommets), sommets, GL_STATIC_DRAW);
  glVertexAttribPointer(locVertex, 3, GL_FLOAT, GL_FALSE, 0, 0);
  ```

---

Ce résumé présente une vision complète et enrichie des bases de l'infographie avec OpenGL, couvrant les concepts théoriques clés et les commandes pratiques pour démarrer en infographie 3D.
