## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER


## AIM:
To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
~~~
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char m[5][5];

void makeMatrix(char *key) {
    char all[200] = "";
    strcat(all, key);
    strcat(all, "ABCDEFGHIKLMNOPQRSTUVWXYZ");   // alphabet without J
    int used[26] = {0}, k = 0;
    for (int i = 0; all[i]; i++) {
        char c = toupper(all[i]);
        if (c == 'J') c = 'I';
        if (isalpha(c) && !used[c - 'A']) {
            used[c - 'A'] = 1;
            m[k / 5][k % 5] = c;
            k++;
        }
    }
}

void prepare(char *in, char *out) {
    char t[200];
    int n = 0, k = 0;
    for (int i = 0; in[i]; i++)
        if (isalpha(in[i])) t[n++] = (toupper(in[i]) == 'J') ? 'I' : toupper(in[i]);
    for (int i = 0; i < n;) {
        out[k++] = t[i++];
        if (i < n && t[i] != out[k - 1]) out[k++] = t[i++];
        else out[k++] = 'X';                    // pad odd end or double letter
    }
    out[k] = 0;
}

void find(char c, int *r, int *col) {
    for (int i = 0; i < 25; i++)
        if (m[i / 5][i % 5] == c) { *r = i / 5; *col = i % 5; }
}

// s = 1 encrypt, s = 4 decrypt
void cipher(char *t, int s) {
    int r1, c1, r2, c2;
    for (int i = 0; t[i]; i += 2) {
        find(t[i], &r1, &c1);
        find(t[i + 1], &r2, &c2);
        if (r1 == r2)      { c1 = (c1 + s) % 5; c2 = (c2 + s) % 5; }  // same row
        else if (c1 == c2) { r1 = (r1 + s) % 5; r2 = (r2 + s) % 5; }  // same column
        else               { int x = c1; c1 = c2; c2 = x; }           // rectangle
        t[i] = m[r1][c1];
        t[i + 1] = m[r2][c2];
    }
}

int main() {
    char key[100], pt[200], text[200];
    printf("Enter the key: ");
    fgets(key, 100, stdin);
    printf("Enter the plaintext: ");
    fgets(pt, 200, stdin);

    makeMatrix(key);
    prepare(pt, text);

    printf("\nMatrix:\n");
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) printf("%c ", m[i][j]);
        printf("\n");
    }

    cipher(text, 1);
    printf("\nEncrypted: %s\n", text);
    cipher(text, 4);
    printf("Decrypted: %s\n", text);
    return 0;
}
~~~


Output:
<img width="1907" height="983" alt="Screenshot 2026-10-29 115406" src="https://github.com/user-attachments/assets/522e5888-da05-4031-9124-43ca3389cbf0" />
<img width="1907" height="962" alt="Screenshot 2026-10-29 115445" src="https://github.com/user-attachments/assets/29b8344e-72ab-4c37-bf32-59c1a5719e38" />


