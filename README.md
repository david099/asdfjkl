#include <iostream>
#include <cstdio>
#include <cstring>
#include <cstdlib>
#include <conio.h>
#include <iomanip>
using namespace std;
int main()
{
    FILE *fp, *ft;
    char another;
    char choice;
    struct student
    {
        char first_name[50], last_name[50];
        char course[100];
        char section[100];
    };
    struct student e;
    char xlast_name[50];
    long int recsize;

    fp=fopen("elevi.txt","rb+");
    if(fp == NULL)
    {
        fp = fopen("elevi.txt","wb+");
        if(fp==NULL)
        {
            puts("Imposibil de deschis fisierul");
            return 0;
        }
    }
    recsize = sizeof(e);
    while(1)
    {
        system("cls");
        cout << "\t\t====== Informatii elevi ======";
        cout <<"\n\n                                          ";
        cout << "\n\n";
        cout << "\n \t\t\t 1. Adauga registru";
        cout << "\n \t\t\t 2. Afiseaza registrele";
        cout << "\n \t\t\t 3. Modifica registrele";
        cout << "\n \t\t\t 4. Sterge registrele";
        cout << "\n \t\t\t 5. Exit";
        cout << "\n\n";
        cout << "\t\t\t Selecteaza optiunea dorita :=> ";
        fflush(stdin);
        choice = getche();
        switch(choice)
        {
        case '1' :
            fseek(fp,0,SEEK_END);
            another ='Y';
            while(another=='Y'||another=='y')
            {
                system("cls");
                cout << "Nume: ";
                cin >> e.first_name;
                cout << "Prenume: ";
                cin >> e.last_name;
                cout << "Clasa: ";
                cin >> e.course;
                cout << "Profil: ";
                cin>>e.section;
                fwrite(&e,recsize,1,fp);
                cout << "\n Vrei sa adaugi un nou registru?(y/n) ";
                fflush(stdin);
                another=getchar();
            }
            break;
        case '2':
            system("cls");
            rewind(fp);
            cout << "=== Vizualizare date elevi ===";
            cout << "\n";
            while (fread(&e,recsize,1,fp) == 1)
            {
                cout << "\n";
                cout <<"Nume: " << e.first_name << setw(10)  <<"Prenume: "<< e.last_name;
                cout << "\n";
                cout <<"Clasa: " <<e.course <<  setw(10)  <<"Profil: "<< e.section;
                cout<< "\n";
            }
            cout << "\n\n";
            system("pause");
            break;
        case '3' :
            system("cls");
            another = 'y';
            while (another == 'y'|| another == 'Y')
            {
                cout << "\n Scrie prenumele elevului : ";
                cin >> xlast_name;
                rewind(fp);
                while (fread(&e,recsize,1,fp) == 1)
                {
                    if (strcmp(e.last_name,xlast_name) == 0)
                    {
                        cout << "Noul nume : ";
                        cin >> e.first_name;
                        cout << "Noul prenume : ";
                        cin >> e.last_name;
                        cout << "Noua clasa    : ";
                        cin >> e.course;
                        cout << "Noul profil   : ";
                        cin >> e.section;
                        fseek(fp, - recsize, SEEK_CUR);
                        fwrite(&e,recsize,1,fp);
                        break;
                    }
                    else
                        cout<<"\n";
                        cout<<"Imposibil de gasit elevul cu acest prenume";
                        cout<<"\n";
                }
                cout << "\n Vrei sa modifici alt registru?(y/n) ";
                fflush(stdin);
                another = getchar();
            }
            break;
        case '4':
            system("cls");
            another = 'y';
            while (another == 'y'|| another == 'Y')
            {
                cout << "\n Scrie prenumele elevului pe care doresti sa-l stergi din registru : ";
                cin >> xlast_name;
                ft = fopen("temp.dat", "wb");
                rewind(fp);
                while (fread (&e, recsize,1,fp) == 1)
                    if (strcmp(e.last_name,xlast_name) != 0)
                    {
                        fwrite(&e,recsize,1,ft);
                    }
                fclose(fp);
                fclose(ft);
                remove("elevi.txt");
                rename("temp.dat","elevi.txt");
                fp=fopen("elevi.txt","rb+");
                cout<<"\n";
                cout << "\n Vrei sa stergi alt elev din registru?(y/n) ";
                fflush(stdin);
                another = getchar();
            }
            break;
        case '5':
            fclose(fp);
            exit(0);
        }
    }
    system("pause");
    return 0;
}
