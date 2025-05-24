## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
## Submitted By: Ashwin Akash M
## Reference Number: 212223230024

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
1. Prepare the key: Remove duplicate characters and form a 5x5 matrix with the
letters of the key.
2. Prepare the plaintext: Remove any non-alphabet characters and split the
message into digraphs (pairs of letters). If there are any double letters (e.g.,
"LL"), insert an extra letter (commonly "X") between them.
3. Encryption: For each digraph:<br>
● If the letters are in the same row, replace them with the letters to their
immediate right.<br>
● If the letters are in the same column, replace them with the letters
immediately below.<br>
● If the letters form a rectangle, replace them with the letters on the same
row but at the other corners of the rectangle.<br>
Decryption: Inverse the above rules.




## Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>
#define SIZE 5
void createMatrix(char key[], char matrix[SIZE][SIZE]) {
int i, j, k = 0;
int used[26] = {0};
for (i = 0; key[i] != '\0'; i++) {
if (isalpha(key[i])) {
char c = toupper(key[i]);
if (!used[c - 'A']) {
matrix[k / SIZE][k % SIZE] = c;
used[c - 'A'] = 1;
k++;
}
}
}
for (i = 0; i < 26; i++) {
if (!used[i]) {
matrix[k / SIZE][k % SIZE] = 'A' + i;
k++;
}
}
}
void findPosition(char c, char matrix[SIZE][SIZE], int *row, int *col) {
for (int i = 0; i < SIZE; i++) {
for (int j = 0; j < SIZE; j++) {
if (matrix[i][j] == c) {
*row = i;
*col = j;
return;
}
}
}
}
void prepareText(char *text, char *preparedText) {
int j = 0;
for (int i = 0; text[i] != '\0'; i++) {
if (isalpha(text[i])) {
preparedText[j++] = toupper(text[i]);
}
}
preparedText[j] = '\0;
if (strlen(preparedText) % 2 != 0) {
preparedText[strlen(preparedText)] = 'X';
preparedText[strlen(preparedText) + 1] = '\0';
}
}
void encryptPlayfair(char *plaintext, char matrix[SIZE][SIZE], char *ciphertext) {
int len = strlen(plaintext);
int row1, col1, row2, col2;
int j = 0;
for (int i = 0; i < len; i += 2) {
findPosition(plaintext[i], matrix, &row1, &col1);
findPosition(plaintext[i + 1], matrix, &row2, &col2);
if (row1 == row2) {
ciphertext[j++] = matrix[row1][(col1 + 1) % SIZE];
ciphertext[j++] = matrix[row2][(col2 + 1) % SIZE];
else if (col1 == col2) {
ciphertext[j++] = matrix[(row1 + 1) % SIZE][col1];
ciphertext[j++] = matrix[(row2 + 1) % SIZE][col2];
}
else {
ciphertext[j++] = matrix[row1][col2];
ciphertext[j++] = matrix[row2][col1];
}
}
ciphertext[j] = '\0';
}
void decryptPlayfair(char *ciphertext, char matrix[SIZE][SIZE], char *plaintext) {
int len = strlen(ciphertext);
int row1, col1, row2, col2;
int j = 0;
for (int i = 0; i < len; i += 2) {
findPosition(ciphertext[i], matrix, &row1, &col1);
findPosition(ciphertext[i + 1], matrix, &row2, &col2);
if (row1 == row2) {
plaintext[j++] = matrix[row1][(col1 - 1 + SIZE) % SIZE];
plaintext[j++] = matrix[row2][(col2 - 1 + SIZE) % SIZE];
}
else if (col1 == col2) {
plaintext[j++] = matrix[(row1 - 1 + SIZE) % SIZE][col1];
plaintext[j++] = matrix[(row2 - 1 + SIZE) % SIZE][col2];
}
else {
plaintext[j++] = matrix[row1][col2];
plaintext[j++] = matrix[row2][col1];
}
}
plaintext[j] = '\0';
}
int main() {
char key[100], text[100], encryptedText[100], decryptedText[100];
char matrix[SIZE][SIZE];
printf("Enter the key: ");
fgets(key, sizeof(key), stdin);
key[strcspn(key, "\n")] = '\0';
printf("Enter the text: ");
fgets(text, sizeof(text), stdin);
text[strcspn(text, "\n")] = '\0';
char preparedText[100];
prepareText(text, preparedText);
createMatrix(key, matrix);
encryptPlayfair(preparedText, matrix, encryptedText);
printf("Ciphertext (Encrypted): %s\n", encryptedText);
decryptPlayfair(encryptedText, matrix, decryptedText);
printf("Decrypted text: %s\n", decryptedText);
retu
```


## Output:
![Screenshot 2025-03-15 144201](https://github.com/user-attachments/assets/f733eae6-109b-449b-8f5e-e86948086328)
![Screenshot 2025-03-17 161652](https://github.com/user-attachments/assets/243e7fcb-5ca9-408b-a31a-e9889e6e7b17)

## Result:
The program is executed successfully
