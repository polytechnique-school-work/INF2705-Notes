### Cours 02 : Objets Graphiques et Pipelines Graphiques

#### Objectifs du Cours

1. Différencier sommet et connectivité.
2. Situer les transferts entre CPU et GPU.
3. Définir des objets graphiques pour les données de vertex et de connectivité (VBO).
4. Définir des objets graphiques pour un contexte de vertex (VAO).
5. Décrire un exemple d'affichage 3D par objet graphique VAO et VBO.

---

### Concepts Clés et Commandes

#### Vertex Buffer Object (VBO)

Les VBOs permettent d'optimiser le transfert de données de sommets entre le CPU et le GPU en les stockant directement sur la carte graphique. Voici un processus détaillé pour créer et utiliser un VBO.

1. **Génération d'un Identifiant VBO** :

   ```cpp
   GLuint vboId;
   glGenBuffers(1, &vboId);
   ```

   - Crée un identifiant unique pour le VBO que vous allez utiliser.

2. **Liaison du VBO** :

   ```cpp
   glBindBuffer(GL_ARRAY_BUFFER, vboId);
   ```

   - Lie le VBO au contexte graphique courant, ici spécifié comme un `GL_ARRAY_BUFFER`, ce qui signifie qu'il contiendra des sommets.

3. **Copie des Données vers le VBO** :
   ```cpp
   GLfloat sommets[] = { /* données des sommets */ };
   glBufferData(GL_ARRAY_BUFFER, sizeof(sommets), sommets, GL_STATIC_DRAW);
   ```
   - `GL_STATIC_DRAW` indique que les données ne seront pas modifiées souvent et seront utilisées principalement pour le dessin.

#### Exemple Complet d'Initialisation d'un VBO

```cpp
GLfloat sommets[] = {
    -0.5f, -0.5f, 0.0f,
     0.5f, -0.5f, 0.0f,
     0.0f,  0.5f, 0.0f
};
GLuint vboId;
glGenBuffers(1, &vboId);
glBindBuffer(GL_ARRAY_BUFFER, vboId);
glBufferData(GL_ARRAY_BUFFER, sizeof(sommets), sommets, GL_STATIC_DRAW);
```

#### Vertex Array Object (VAO)

Les VAOs facilitent la gestion des VBOs en capturant tous les appels de liaison et d'association des attributs de vertex.

1. **Génération d'un Identifiant VAO** :

   ```cpp
   GLuint vaoId;
   glGenVertexArrays(1, &vaoId);
   ```

2. **Liaison du VAO** :

   ```cpp
   glBindVertexArray(vaoId);
   ```

   - Toute configuration des VBOs faite après cette liaison sera "mémorisée" par le VAO.

3. **Configuration des Attributs de Vertex** :
   ```cpp
   // Assume que le VBO est déjà lié
   GLuint locVertex = glGetAttribLocation(prog, "Vertex");
   glVertexAttribPointer(locVertex, 3, GL_FLOAT, GL_FALSE, 0, 0);
   glEnableVertexAttribArray(locVertex);
   ```

#### Exemple Complet d'Initialisation d'un VAO et VBOs

```cpp
GLuint vao, vbo[2];
GLfloat xyzh[] = { /* sommets */ };
GLfloat rgba[] = { /* couleurs */ };

glGenVertexArrays(1, &vao);
glGenBuffers(2, vbo);

glBindVertexArray(vao);

// VBO pour les sommets
glBindBuffer(GL_ARRAY_BUFFER, vbo[0]);
glBufferData(GL_ARRAY_BUFFER, sizeof(xyzh), xyzh, GL_STATIC_DRAW);
glVertexAttribPointer(locVertex, 4, GL_FLOAT, GL_FALSE, 0, 0);

// VBO pour les couleurs
glBindBuffer(GL_ARRAY_BUFFER, vbo[1]);
glBufferData(GL_ARRAY_BUFFER, sizeof(rgba), rgba, GL_STATIC_DRAW);
glVertexAttribPointer(locColor, 4, GL_FLOAT, GL_FALSE, 0, 0);

glEnableVertexAttribArray(locVertex);
glEnableVertexAttribArray(locColor);

glBindVertexArray(0); // Unbind VAO
```

---

### Affichage de Primitives Utilisant VBOs et VAOs

#### Affichage avec VBOs

```cpp
glBindBuffer(GL_ARRAY_BUFFER, vboId1); // lier VBO des sommets
glDrawArrays(GL_TRIANGLES, 0, 3); // dessiner des triangles
```

#### Affichage avec VAOs

```cpp
glBindVertexArray(vao); // lier VAO
glDrawArrays(GL_TRIANGLES, 0, 36); // 36 sommets
glBindVertexArray(0); // unbind VAO
```

---

### Pipeline Graphique et Nuanceurs (Shaders)

Le pipeline graphique moderne se compose de plusieurs étapes programmables permettant d'assembler, transformer et rendre des primitives graphiques sur un écran 2D.

#### Pipeline Graphique Programmable

1. **Nuanceur de Sommets (Vertex Shader)** :

   - Transforme les coordonnées des sommets en coordonnées d'écran.
   - Exemple :

   ```glsl
   #version 450
   uniform mat4 P,V,M;
   in vec4 Vertex;
   void main(void) {
       gl_Position = P * V * M * Vertex;
   }
   ```

2. **Tramage (Rasterization)** :

   - Convertit les primitives en fragments.

3. **Nuanceur de Fragments (Fragment Shader)** :
   - Détermine la couleur des fragments.
   - Exemple :
   ```glsl
   #version 450
   in vec4 laCouleur;
   out vec4 fragColor;
   void main(void) {
       fragColor = laCouleur;
   }
   ```

#### Chargement et Utilisation des Nuanceurs

1. **Création des Objets Nuanceurs** :

   ```cpp
   GLuint nuanceurSommets = glCreateShader(GL_VERTEX_SHADER);
   GLuint nuanceurFragments = glCreateShader(GL_FRAGMENT_SHADER);
   ```

2. **Chargement du Code Source** :

   ```cpp
   const GLchar *ns = lireNuanceur("nuanceurSommets.glsl");
   glShaderSource(nuanceurSommets, 1, &ns, NULL);
   ```

3. **Compilation des Nuanceurs** :

   ```cpp
   glCompileShader(nuanceurSommets);
   ```

4. **Création du Programme et Lien des Nuanceurs** :

   ```cpp
   GLuint prog = glCreateProgram();
   glAttachShader(prog, nuanceurSommets);
   glAttachShader(prog, nuanceurFragments);
   glLinkProgram(prog);
   glUseProgram(prog);
   ```

5. **Définition des Variables Uniformes** :
   ```cpp
   GLint locMVP = glGetUniformLocation(prog, "MVP");
   glUniformMatrix4fv(locMVP, 1, GL_FALSE, &mvp[0][0]);
   ```

---

Ce résumé approfondi vous permet de comprendre les concepts centraux des objets graphiques, des transferts de données entre CPU et GPU, et les nuances des pipelines graphiques modernes utilisant OpenGL. En suivant les exemples et les explications fournies, vous serez en mesure de manipuler efficacement les VBOs, VAOs et nuanceurs pour créer des applications graphiques avancées.
