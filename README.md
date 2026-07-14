# 🚗 Analyse en Composantes Principales (ACP) : Performances des Voitures
> **Projet d'Analyse de Données Multidimensionnelles**  
> *Implémentation d'une ACP normée par Décomposition en Valeurs Singulières (SVD) généralisée sous Python.*

---

## 📌 1. Présentation du Problème & Justification

Dans ce projet, nous étudions un jeu de données contenant **6 modèles de voitures** représentant les ``individus`` évalués selon **4 caractéristiques techniques** représenant les ``variables`` :
*   **Puissance** (en chevaux, ch)
*   **Consommation** (en litres aux 100 km, L/100)
*   **Poids** (en kilogrammes, kg)
*   **Prix** (en milliers d'euros, k€)

### Pourquoi une ACP Normée est-elle indispensable ici ?

À travers ce jeu de données, nous constatons que les variables ne sont pas exprimées dans les mêmes unités, ni dans les mêmes ordres de grandeur. Par exemple, le **Poids** (exprimé en kilogrammes, avec des valeurs dépassant 1500 kg) possède une variance mathématique bien plus élevée que la **Consommation** (exprimée en L/100, variant entre 4.8 et 13). 

Si nous réalisions une ACP non-normée (sur les données brutes), les variables à forte variance comme le Poids ou le Prix écraseraient totalement les autres dans le calcul des distances et des axes. Afin que chaque caractéristique technique contribue équitablement à l'analyse, il est indispensable de réaliser une **ACP normée** en centrant et réduisant nos données.

## 📝 2. La Modélisation Mathématique.

Notre jeu de données étant sans valeurs manquantes, nous allons procéder à la mathématisation.
Nous désignons $X$ comme la matrice centrée et réduite issue de notre jeu de données de taille $$n = 6$$ (représentant les lignes/individus) et $$p = 4$$ (représentant les colonnes/variables).

On démarre par la réalisation de la SVD généralisée. Pour ce faire, nous allons en premier poser : $$Y = D^{1/2}XM^{1/2}$$ et ensuite réaliser la SVD réduite de Y : $$Y = U_Y S V_Y^T$$

Où :

$S$ est la matrice diagonale contenant les valeurs singulières de $Y$ classées par ordre décroissant.
$U_Y$ est la matrice des vecteurs singuliers à gauche de $Y$ (taille  $n \times p$).
$V_Y$ est la matrice des vecteurs singuliers à droite de $Y$ (taille  $p \times p$).

Les Métriques $D$ et $M$Pour notre ACP normée (standardisée), nous définissons :
La matrice des poids des individus $D = \frac{1}{n} I_n$.
La métrique des variables $M = I_p$ (identité).
Retour aux facteurs d'origine ($U$ et $V$)Pour retrouver les facteurs propres du triplet $(X, D, M)$, on applique les transformations inverses :

$U = D^{-1/2} U_Y$

$V = M^{-1/2} V_Y$

Ces matrices vérifient les conditions d'orthogonalité généralisées : $U^T D U = I_p$  et  $V^T M V = I_p$.

## Calcul des coordonnées factorielles 

Enfin, nous projetons nos données pour obtenir les coordonnées de nos points sur les nouveaux axes factoriels :

1. Coordonnées des individus ($F$) : $F = X M V$
2. Coordonnées des variables ($G$) : $G = X^T D U = V S$

## 📉 3. Analyse de l'Inertie et Choix du nombre d'axes

![Scree Plot](chemin/vers/ton/image_scree_plot.png) <!-- Remplace ce chemin par le nom de ton image (ex: scree_plot.png) une fois enregistrée dans ton dossier -->

Au regard du graphique de Scree plot, on constate que nous avons un coude au niveau de l'axe 2, et l'information apportée par l'axe 3 n'est pas très significative. De plus, en appliquant le critère de Kaiser, seuls les axes 1 et 2 ont des valeurs propres supérieures à 1, ce qui confirme ce que le graphique nous a montré.

## 4. ⭕ Analyse des variables grâce au Cercle des Corrélations

![Cercle des Corrélations](chemin/vers/ton/image_cercle_correlation.png) <!-- Remplace par ton image (ex: cercle_correlations.png) -->

En analysant le graphique, nous constatons tout d'abord que toutes les variables sont très bien représentées puisque l'extrémité de leurs flèches est très proche de la bordure du cercle. 

Les variables **Consommation** et **Puissance** sont fortement corrélées positivement, ce qui traduit physiquement que plus une voiture est puissante, plus elle consomme. Ces deux variables sont également corrélées avec le **Prix**, montrant que la puissance et la technologie de réduction de consommation ont un coût important. 

En revanche, les variables **Poids** et **Prix** apparaissent comme presque orthogonales sur le graphique, indiquant qu'elles sont globalement indépendantes. Ainsi, dans notre échantillon, le poids d'un véhicule n'a pas d'effet significatif direct sur son prix (une voiture de sport très chère pouvant être particulièrement légère).

## 5. 👥 Analyse de la projection des individus (les voitures)

![Projection des individus](chemin/vers/ton/image_individus.png) <!-- Remplace par ton image (ex: individus.png) -->

Après observation du graphique, nous pouvons tirer les conclusions suivantes sur la structure de notre parc automobile :

* **L'Axe 1 (Axe de la performance et du prestige) :** Cet axe horizontal oppose nettement les sportives de luxe, **Ferrari F40** et **Porsche 911** (situées très à droite), aux citadines économiques, **Clio** et **Twingo** (situées très à gauche). La Ferrari et la Porsche se caractérisent par une puissance et une consommation très élevées, ce qui justifie leur prix de vente particulièrement important. À l'inverse, la Clio et la Twingo consomment peu, ont une puissance modeste et restent très abordables financièrement.

* **L'Axe 2 (Axe du gabarit et du poids) :** Cet axe vertical met en évidence l'opposition sur le critère du poids. On y retrouve en haut la **Passat** et la **Golf**, des voitures familiales et routières allemandes imposantes et lourdes. Elles s'opposent en bas à la **Twingo** (citadine ultra-légère) et à la **Ferrari F40** (supercar de course conçue avec des matériaux légers comme le carbone).


## 6. 💡Conlusion

Cette analyse en composantes principales (ACP) a permis de structurer efficacement notre parc automobile en réduisant nos 4 variables de départ à seulement 2 axes d'analyse majeurs, tout en conservant plus de $98\%$ de l'information d'origine. L'étude montre que le marché automobile se segmente naturellement selon deux logiques clés : la puissance associée au prestige d'une part (Axe 1), et le gabarit physique du véhicule d'autre part (Axe 2). Grâce au cercle des corrélations et à la projection des individus, nous avons pu opposer clairement les sportives d'exception, les routières familiales lourdes et les citadines économiques. En définitive, cette méthode démontre sa puissance pour transformer un tableau de données brutes en une cartographie visuelle claire et directement interprétable.

