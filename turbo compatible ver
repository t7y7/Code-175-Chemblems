#include <stdio.h>
#include <iostream.h>
#include <string.h>
#include <ctype.h>
#include <stdlib.h>
#include <conio.h>

#define MAX_TEACHERS 5
#define MAX_STUDENTS 10
#define MAX_ELEMENTS 500
#define TEACHER_CODE "teach2025"

struct User {
    char username[20];
    char password[20];
    char role[10];
};

User teachers[MAX_TEACHERS];
User students[MAX_STUDENTS];
int teacherCount = 0;
int studentCount = 0;

struct Element {
    int atomicNumber;        
    char symbol[16];          
    char name[32];            
    float atomicMass;         
    char classification[20];  
    int group;               
    int period;              
    int isCompound;          
} elements[MAX_ELEMENTS];

char currentUser[20] = "";
char currentRole[10] = "";

void saveTeachers() {
    FILE *fp = fopen("teachers.dat", "wb");
    if (fp == NULL) return;
    fwrite(&teacherCount, sizeof(int), 1, fp);
    fwrite(teachers, sizeof(User), teacherCount, fp);
    fclose(fp);
}

void saveStudents() {
    FILE *fp = fopen("students.dat", "wb");
    if (fp == NULL) return;
    fwrite(&studentCount, sizeof(int), 1, fp);
    fwrite(students, sizeof(User), studentCount, fp);
    fclose(fp);
}

void loadTeachers() {
    FILE *fp = fopen("teachers.dat", "rb");
    if (fp != NULL) {
        fread(&teacherCount, sizeof(int), 1, fp);
        fread(teachers, sizeof(User), teacherCount, fp);
        fclose(fp);
    } else {
        teacherCount = 5;
        for (int i = 0; i < 5; i++) {
            sprintf(teachers[i].username, "teacher%d", i+1);
            strcpy(teachers[i].password, "teach123");
            strcpy(teachers[i].role, "teacher");
        }
    }
}

void loadStudents() {
    FILE *fp = fopen("students.dat", "rb");
    if (fp != NULL) {
        fread(&studentCount, sizeof(int), 1, fp);
        fread(students, sizeof(User), studentCount, fp);
        fclose(fp);
    } else {
        studentCount = 10;
        for (int i = 0; i < 10; i++) {
            sprintf(students[i].username, "student%d", i+1);
            strcpy(students[i].password, "stud123");
            strcpy(students[i].role, "student");
        }
    }
}

void saveElements() {
    FILE *fp = fopen("elements.dat", "wb");
    if (fp == NULL) { printf("Cannot open file.\n"); return; }
    fwrite(elements, sizeof(elements), 1, fp);
    fclose(fp);
    printf("Elements saved.\n");
}

void loadElements() {
    FILE *fp = fopen("elements.dat", "rb");
    if (fp != NULL) {
        if (fread(elements, sizeof(elements), 1, fp) == 1) {
            printf("");
        }
        fclose(fp);
    }
}

int login() {
    char u[20], p[20], code[32];
    printf("Username: "); scanf("%s", u);
    printf("Password: "); scanf("%s", p);
    printf("Code (Write NA if none): "); scanf("%s", code);

    int teacherFlag = (!strcmp(code, TEACHER_CODE)) ? 1 : 0;

    if (teacherFlag) {
        for (int i = 0; i < teacherCount; i++) {
            if (!strcmp(teachers[i].username, u) && !strcmp(teachers[i].password, p)) {
                strcpy(currentUser, teachers[i].username);
                strcpy(currentRole, "teacher");
                return 1;
            }
        }
    } else {
        for (int i = 0; i < studentCount; i++) {
            if (!strcmp(students[i].username, u) && !strcmp(students[i].password, p)) {
                strcpy(currentUser, students[i].username);
                strcpy(currentRole, "student");
                return 1;
            }
        }
    }
    return 0;
}

void editAccount() {
    char newUser[20], newPass[20];
    printf("Edit account for: %s (%s)\n", currentUser, currentRole);
    printf("New username: "); scanf("%s", newUser);
    printf("New password: "); scanf("%s", newPass);

    if (!strcmp(currentRole, "teacher")) {
        for (int i = 0; i < teacherCount; i++) {
            if (!strcmp(teachers[i].username, currentUser)) {
                strcpy(teachers[i].username, newUser);
                strcpy(teachers[i].password, newPass);
                strcpy(currentUser, newUser);
                printf("Account updated.\n");
                saveTeachers();
                return;
            }
        }
    } else {
        for (int i = 0; i < studentCount; i++) {
            if (!strcmp(students[i].username, currentUser)) {
                strcpy(students[i].username, newUser);
                strcpy(students[i].password, newPass);
                strcpy(currentUser, newUser);
                printf("Account updated.\n");
                saveStudents();
                return;
            }
        }
    }
    printf("Account not found.\n");
}

int teacherMenu() {
    int c;
    printf("\nTEACHER MENU (%s)\n", currentUser);
    printf("[1] Enter element/compound\n");
    printf("[2] Search (symbol or name)\n");
    printf("[3] Display all\n");
    printf("[4] Save elements\n");
    printf("[5] Edit my username/password\n");
    printf("[6] Edit element\n");
    printf("[7] Logout\n");
    printf("[8] Exit\n");
    printf("Choose: ");
    scanf("%d", &c);
    return c;
}

int studentMenu() {
    int c;
    printf("\nSTUDENT MENU (%s)\n", currentUser);
    printf("[1] Search (symbol or name)\n");
    printf("[2] Display all\n");
    printf("[3] Edit my username/password\n");
    printf("[4] Logout\n");
    printf("[5] Exit\n");
    printf("Choose: ");
    scanf("%d", &c);
    return c;
}

void logout() {
    strcpy(currentUser, "");
    strcpy(currentRole, "");
    printf("\nYou have been logged out.\n\n");
}

int firstFreeSlot() {
    for (int i = 0; i < MAX_ELEMENTS; i++) {
        if (elements[i].symbol[0] == '\0') return i;
    }
    return -1;
}

int findBySymbol(const char *sym) {
    for (int i = 0; i < MAX_ELEMENTS; i++) {
        if (!strcmp(elements[i].symbol, sym)) return i;
    }
    return -1;
}

int findByName(const char *nm) {
    for (int i = 0; i < MAX_ELEMENTS; i++) {
        if (!strcmp(elements[i].name, nm)) return i;
    }
    return -1;
}

float atomicMass(const char *sym) {
    int idx = findBySymbol(sym);
    if (idx == -1) return -1.0f;
    if (elements[idx].isCompound) return -1.0f;
    return elements[idx].atomicMass;
}

float computeMolarMass(const char *formula) {
    float total = 0.0f;
    int len = strlen(formula), i = 0;
    while (i < len) {
        if (!isalpha(formula[i])) return -1.0f;
        char sym[4]; sym[0] = formula[i]; sym[1] = '\0'; i++;
        if (i < len && islower(formula[i])) { 
            sym[1] = formula[i]; 
            sym[2] = '\0'; 
            i++; 
        }
        int count = 0;
        while (i < len && isdigit(formula[i])) { 
            count = count*10 + (formula[i]-'0'); 
            i++; 
        }
        if (count == 0) count = 1;
        float mass = atomicMass(sym);
        if (mass < 0.0f) return -2.0f;
        total += mass * count;
    }
    return total;
}

void addRecord() {
    int slot = firstFreeSlot();
    if (slot == -1) { printf("Storage full.\n"); return; }

    int choice;
    printf("\nADD RECORD\n");
    printf("[1] Element\n");
    printf("[2] Compund\n");
    printf("Choose: ");
    scanf("%d", &choice);

    if (choice == 1) {
        Element e; e.isCompound = 0;
        printf("Atomic number: "); scanf("%d", &e.atomicNumber);
        printf("Symbol: "); scanf("%s", e.symbol);
        printf("Name: "); scanf("%s", e.name);
        printf("Atomic mass: "); scanf("%f", &e.atomicMass);
        printf("Classification: "); scanf("%s", e.classification);
        printf("Group: "); scanf("%d", &e.group);
        printf("Period: "); scanf("%d", &e.period);

        if (findBySymbol(e.symbol) != -1) { printf("Symbol exists.\n"); return; }
        elements[slot] = e;
        printf("Element added.\n");
    }
    else if (choice == 2) {
        Element c; 
        c.isCompound = 1; 
        c.atomicNumber = 0; 
        c.group = 0; 
        c.period = 0;
        strcpy(c.classification, "Compound");

        char formula[32];
        printf("Compound formula: ");
        scanf("%s", formula);
        strcpy(c.symbol, formula);

        printf("Compound name: ");
        scanf("%s", c.name);

        if (findBySymbol(formula) != -1) {
            printf("A record with this formula already exists.\n");
            return;
        }

        float mm = computeMolarMass(formula);
        if (mm < 0.0f) {
            if (mm == -2.0f) 
                printf("Unknown element symbol in formula. Add required elements first.\n");
            else 
                printf("Invalid formula.\n");
            return;
        }
        c.atomicMass = mm;

        elements[slot] = c;
        printf("Compound added. Molar mass = %.3f g/mol\n", mm);
    }
    else {
        printf("Invalid choice.\n");
    }
}

void editElement() {
    char sym[16];
    printf("Enter symbol/formula of element/compound to edit: ");
    scanf("%s", sym);

    int idx = findBySymbol(sym);
    if (idx == -1) {
        printf("Record not found.\n");
        return;
    }

    Element *e = &elements[idx];
    int choice;
    do {
        printf("\nEDIT ELEMENT (%s)\n", e->symbol);
        printf("[1] Atomic number\n");
        printf("[2] Symbol\n");
        printf("[3] Name\n");
        printf("[4] Atomic mass\n");
        printf("[5] Classification\n");
        printf("[6] Group\n");
        printf("[7] Period\n");
        printf("[8] Done\n");
        printf("Choose: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: 
                if (e->isCompound) { 
                    printf("Compounds do not have atomic numbers.\n"); 
                } else {
                    printf("New atomic number: "); scanf("%d", &e->atomicNumber);
                }
                break;
            case 2: {
                char newSym[16];
                printf("New symbol/formula: "); scanf("%s", newSym);
                if (findBySymbol(newSym) != -1 && strcmp(newSym, e->symbol)) {
                    printf("Another record already uses this symbol/formula.\n");
                } else {
                    strcpy(e->symbol, newSym);
                }
                break;
            }
            case 3:
                printf("New name: "); scanf("%s", e->name);
                break;
            case 4:
                printf("New atomic/molar mass: "); scanf("%f", &e->atomicMass);
                break;
            case 5:
                printf("New classification: "); scanf("%s", e->classification);
                break;
            case 6:
                if (e->isCompound) { 
                    printf("Compounds do not have group.\n"); 
                } else {
                    printf("New group: "); scanf("%d", &e->group);
                }
                break;
            case 7:
                if (e->isCompound) { 
                    printf("Compounds do not have period.\n"); 
                } else {
                    printf("New period: "); scanf("%d", &e->period);
                }
                break;
            case 8:
                printf("Finished editing.\n");
                break;
            default:
                printf("Invalid choice.\n");
        }
    } while (choice != 8);
}

void displayAll() {
    printf("\n%-8s %-8s %-15s %-20s %-12s %-15s %-6s %-6s\n",
           "Type", "Atomic#", "Symbol/Formula", "Name", "Mass", "Class", "Group", "Period");
    printf("-------------------------------------------------------------------------------------------------\n");

    for (int i = 0; i < MAX_ELEMENTS; i++) {
        if (elements[i].symbol[0] != '\0') {
            printf("%-8s %-8d %-15s %-20s %-12.3f %-15s %-6d %-6d\n",
                   elements[i].isCompound ? "Compound" : "Element",
                   elements[i].atomicNumber,
                   elements[i].symbol,
                   elements[i].name,
                   elements[i].atomicMass,
                   elements[i].classification,
                   elements[i].group,
                   elements[i].period);
        }
    }
}

void printOne(const Element &e) {
    printf("+----------------------------------------+\n");
    printf("| %-15s : %-20s |\n", "Type", e.isCompound ? "Compound" : "Element");
    printf("| %-15s : %-20d |\n", "Atomic Number", e.atomicNumber);
    printf("| %-15s : %-20s |\n", "Symbol/Formula", e.symbol);
    printf("| %-15s : %-20s |\n", "Name", e.name);
    printf("| %-15s : %-20.3f |\n", "Mass", e.atomicMass);
    printf("| %-15s : %-20s |\n", "Classification", e.classification);
    printf("| %-15s : %-20d |\n", "Group", e.group);
    printf("| %-15s : %-20d |\n", "Period", e.period);
    printf("+----------------------------------------+\n");
}

void searchRecord() {
    int choice;
    printf("\nSEARCH RECORD\n");
    printf("[1] Search by Symbol/Formula\n");
    printf("[2] Search by Name\n");
    printf("Choose: ");
    scanf("%d", &choice);

    if (choice == 1) {
        char sym[16];
        printf("Enter symbol/formula: ");
        scanf("%s", sym);
        int idx = findBySymbol(sym);
        if (idx == -1) {
            printf("Not found.\n");
        } else {
            printOne(elements[idx]);
        }
    } 
    else if (choice == 2) {
        char nm[32];
        printf("Enter name: ");
        scanf("%s", nm);
        int idx = findByName(nm);
        if (idx == -1) {
            printf("Not found.\n");
        } else {
            printOne(elements[idx]);
        }
    } 
    else {
        printf("Invalid choice.\n");
    }
}

int main() {
    clrscr();
   
    loadTeachers();
    loadStudents();
    loadElements();

printf("\033[1;31mP R O J E C T  E M B L E M X\033[0m\n");
cout << "\033[1;31mYour chemistry companion. ᥫ᭡\033[0m\n\n";

    cout << "     \\     °∆°     /\n";
    cout << "     ▒▒████▒▒\n";
    cout << "┌── █ ^  —  █───┐\n";
    cout << "│   █ ▄▄▄▄ █    |\n";
    cout << "│   █ ▀▀▀▀ █    │\n";
    cout << "    ▒██████▒\n";
    cout<< "\n";
    
    cout << "\033[1;36mLoading system";
    
    for (int i = 0; i < 3; i++) {
        cout << ".";
        cout.flush();
        usleep(400000); 
        
    }
    
    cout << "\n\033[1;36mPreparing the elements";
    
    for (int i = 0; i < 3; i++) {
        cout << ".";
        cout.flush();
        usleep(400000); 
    }
    
    cout << "\n\033[1;36mAlmost there";
    
    
    for (int i = 0; i < 3; i++) {
        cout << ".";
        cout.flush();
        usleep(400000); 
    }

cout<<"\n\n";

    while (1) {
        while (!login()) {
            printf("\nInvalid username or password. Please try again.\n\n");
        }

        if (!strcmp(currentRole, "teacher")) {
            for (;;) {
                int tc = teacherMenu();
                if (tc == 8) {
                    printf("PROGRAM TERMINATED\n");
                    return 0;
                }
                else if (tc == 7) {
                    logout();
                    break; 
                }
                else if (tc == 1) addRecord();
                else if (tc == 2) searchRecord();
                else if (tc == 3) displayAll();
                else if (tc == 4) saveElements();
                else if (tc == 5) editAccount();
                else if (tc == 6) editElement();
                else printf("Invalid choice.\n");
            }
        } else {
            for (;;) {
                int sc = studentMenu();
                if (sc == 5) { 
                    printf("PROGRAM TERMINATED\n");
                    return 0;
                }
                else if (sc == 4) {
                    logout();
                    break;
                }
                else if (sc == 1) searchRecord();
                else if (sc == 2) displayAll();
                else if (sc == 3) editAccount();
                else printf("Invalid choice.\n");
            }
        }
    }
}
