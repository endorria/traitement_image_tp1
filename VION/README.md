= compte rendu

Rémi Vion


![alt text][img1]


![alt text][img2]


[img1]: /tp1_filtrage/VION/images/morpho/carre64_GN_01_S_T_064_D_3.pgm "Logo Title Text 2"
[img2]: https://github.com/rvion/traitement_image_tp1/blob/master/tp1_filtrage/VION/images/morpho/carre64_GN_01_S_T_064_D_3.pgm "Logo Title Text 2"

Introduction

Le but de ce TP est de d’implanter en C différentes opérateurs de TI et de les tester sur des images réelles et de synthèse.

=== consignes:

    Ce compte rendu, doit contenir des explications sur la démarche suivie ansi que votre analyse sur la façon dont vous avez géré le bruit. Vos explications seront illustrés par des exemples de résultats avec à partir d’image faiblement bruitées et d’autres fortement bruitées. La prise d’initiative dans la combinaison d’opérateur sera évaluée.
    Votre compte-rendu sera une archive zip (exclusivement) à votre nom. Cette archive devra contenir un répertoire à votre nom, contenant les différences codes C ainsi que le comptre rendu au format pdf. L’archive sera à envoyer par mail à l’adresse andres.romero@lri.fr. Le sujet de ce mail sera APP5_TI. Le respect de ces consignes ainsi que la qualité des commentaires (dans le compte-rendu) et dans les codes sources sera prise en compte dans l’évaluation. Pour visualiser les images au format PGM (binaire) : différents viewers sont disponibles comme Irfanview ou XnView.
    Remplir fonctions vides dans les fichiers:
    
    * filterNRC
    * lutNRC
    * morphoNR

Travail à faire

Le travail à faire reprend les différentes étapes de la figure 1.

    ajouter les codes d’opérateurs manquant dans les fichiers filterNRC, lutNRC et morphoNR.
    ajouter des noms d’images à traiter ou à retraiter (image avec bruit gaussien ou avec bruit impul- sionnel) dans les fonctions test_.
    ajouter des nouveaux appels à des fonctions de traitement (nouvelle valeur de paramètre) dans les fonctions routine_.

=== Bruitage

    Les fonctions de bruitage (bruit gaussien, bruit impulsionnel) sont fournies, tout comme la fonction pour générer des images contenant un carré dont le niveau de gris est séparé de celui du fond d’un certain écart.

    Bruiter les images de synthèse avec du bruit gaussien et du bruit impulsionnel. Un exemple est fourni, en faire d’autres.
    qu’observe-t-on ?

On observe xxx

=== Débruitage

    Coder la fonction effectuant un opérateur de lissage de type moyenneur (average en anglais) en s’inspirant du lissage gaussien fourni.
    Coder la fonction effectuant un opérateur de lissage de type médian (basé sur un tri classique).
    tester les filtres de débruitage sur du bruit additionnel ou du bruit impulsionnel

```cpp

/* ------------------------------------------------- */
void sort_selection_ui8vector(uint8 *X, int i0, int i1)
/* ------------------------------------------------- */
{
    int c,d;
    uint8 swap;
    for (c = i0; c < i1; c++) {
        for (d = i0 ; d < i1 - c; d++) {
            if (X[d] > X[d+1]){
            /* For decreasing order use < */
                swap = X[d];
                X[d] = X[d+1];
                X[d+1] = swap;
            }
        }
    }
}
/* --------------------------------------------------------------------------------- */
void median_ui8matrix(uint8 **X, int i0, int i1, int j0, int j1, int radius, uint8 **Y)
/* --------------------------------------------------------------------------------- */
{
    int i, j;
    int ii, jj;
    int k;
    uint8 **T;
    float32 y;
    uint8 *vec;
    vec = ui8vector(0,(2*radius+1)*(2*radius+1));
    T = ui8matrix(i0-radius, i1+radius, j0-radius, j1+radius);
    zero_ui8matrix(T, i0-radius, i1+radius, j0-radius, j1+radius);
    dup_ui8matrix(X, i0, i1, j0, j1, T);
    extend_ui8matrix(T, i0, i1, j0, j1, radius);

    for(i=i0; i<=i1; i++) {
        for(j=j0; j<=j1; j++) {
            k = 0;
            for(ii=-radius; ii<=radius; ii++) {
                for(jj=-radius; jj<=radius; jj++) {
                    //y += T[i+ii][j+jj] * K[ii][jj];
                    vec[k]= T[i+ii][j+jj];//X[ii][jj];
                    k ++;
                }
            }
            if(y<0.0f) y = 0.0f;
            if(y>255.0f) y = 255.0f;

            sort_selection_ui8vector(vec,0,k);
            Y[i][j] = (uint8) vec[radius*radius];
        }
    }
    free_ui8matrix(T, i0-radius, i1+radius, j0-radius, j1+radius);
    //free_ui8vector(vec);
}
```

=== Détection de contours

    Coder la fonction effectuant le calcul de la norme L1 du gradient de Sobel.
    Coder la fonction effectuant le calcul de la norme L1 du gradient de FGL.
    Tester les deux opérateurs sur des images sans bruit, puis avec bruit.
    Seuiller les images (soit avec un seuil manuel, soit avec l’opérateur d’Otsu).

=== Morphologie Mathématique

    Implanter les opérateurs suivants :
        érosion et dilatation, pour des images binaires et en niveaux de gris,
        ouverture et fermeture (qui réaliseront des appels aux fonctions précédentes),
        filtres alternés séquentiels, basés sur un ensembles d’ouverture et sur un ensemble de fermetures
    tester les différents opérateurs sur des images avec ou sans bruit impulsionnel.


on ajout un morpho dilatation 6 pour essayer de bien fermer le contour/carre64_GN_01_S_T_064.pgm