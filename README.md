#include <stdio.h>
#include <string.h>

#define max_chaine 56
#define nmax 50

#define RESET "\033[0m"
#define RED "\033[31m"
#define GREEN "\033[32m"
#define YELLOW "\033[33m"
#define BLUE "\033[34m"
#define MAGENTA "\033[35m"
#define CYAN "\033[36m"

typedef enum {C, M, D} etat_civil;

typedef struct {
    char matricule[max_chaine];
    char nom[max_chaine];
    etat_civil Etat;
    float salaire;
} employee;

typedef struct {
    employee t[nmax];
    int n;
} service;

void saisie_employee(employee *e) {
    printf("matricule: ");
    scanf("%s", e->matricule); 
    
    printf("nom: ");
    scanf("%s", e->nom);
    
    char c;
    do {
        printf("etat_civil C/M/D: ");
        scanf(" %c", &c);  
    } while (c != 'C' && c != 'M' && c != 'D');
    
    switch(c) {
        case 'C': e->Etat = C; break;
        case 'M': e->Etat = M; break;
        case 'D': e->Etat = D; break;
    }
    
    do {
        printf("salaire : ");
        scanf("%f", &e->salaire);
    } while (e->salaire <= 0);
}

void ajout(service *s) {
    if (s->n < nmax) {
        saisie_employee(&s->t[s->n]);
        (s->n)++;
    } else {
        printf(RED "vous avez depasse la taille max" RESET "\n");
    }
}

void remplir(service *s) {
    for (int i = 0; i < s->n; i++) {
        saisie_employee(&s->t[i]);
    }
}

void afficher(service s) {
    printf("Fonction afficher non implémentée\n");
}

int rechercheemployee(service s, char mat[]) {
    printf("Fonction rechercheemployee non implémentée\n");
    return -1;
}

void menu(service *s) {
    int reponse;
    
    do {
        printf(MAGENTA "______MENU______\n" RESET);
        printf(GREEN "1: Ajouter un employee \n" RESET);
        printf(GREEN "2: Afficher tout les employes \n" RESET);
        printf(GREEN "3: Chercher un employee par matricule \n" RESET);
        printf(GREEN "4: quitter \n" RESET);
        
        do {
            printf(YELLOW "reponse? " RESET);
            scanf("%d", &reponse);
        } while (reponse <= 0 || reponse > 4);
        
        switch(reponse) {
            case 1: 
                ajout(s);
                break;
            case 2: 
                afficher(*s);
                break;
            case 3: {
                char mat[max_chaine];
                int rep;
                printf("matricule: ");
                scanf("%s", mat);
                rep = rechercheemployee(*s, mat);
                break;
            }
            case 4: 
                printf(RED "quitter !!" RESET "\n");
                break;
        }
    } while (reponse != 4);
}

int main() {
    service s = { .n = 0 };
    menu(&s);
    return 0;
}
