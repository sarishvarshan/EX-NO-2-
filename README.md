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




## Program:

```
 #include <stdio.h>
 #include <string.h>
 #include <ctype.h>
 #define SIZE 5
 void generateKeyTable(char key[], int keyLength, char keyTable[SIZE][SIZE]) {
 int used[26] = {0}; // To track used characters
 int i, j, k = 0;
 for (i = 0; i < keyLength; i++) {
 if (key[i] != 'j') {
 if (used[key[i]- 'a'] == 0) {
 used[key[i]- 'a'] = 1;
 keyTable[k / SIZE][k % SIZE] = key[i];
 k++;
 }
 }
 }
 for (i = 0; i < 26; i++) {
 if (i + 'a' != 'j' && used[i] == 0) {
 keyTable[k / SIZE][k % SIZE] = i + 'a';
 k++;
 }
 }
}
 void search(char keyTable[SIZE][SIZE], char a, char b, int *row1, int *col1, int *row2, int
 *col2) {
 int i, j;
 for (i = 0; i < SIZE; i++) {
 for (j = 0; j < SIZE; j++) {
 if (keyTable[i][j] == a) {
 *row1= i;
 *col1 = j;
 } else if (keyTable[i][j] == b) {
 *row2= i;
 *col2 = j;
 }
 }
 }
 }
 void encryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
 int row1, col1, row2, col2;
 search(keyTable, a, b, &row1, &col1, &row2, &col2);
 if (row1 == row2) {
 *x = keyTable[row1][(col1 + 1) % SIZE];
 *y = keyTable[row2][(col2 + 1) % SIZE];
 } else if (col1 == col2) {
 *x = keyTable[(row1 + 1) % SIZE][col1];
*y = keyTable[(row2 + 1) % SIZE][col2];
 } else {
 *x = keyTable[row1][col2];
 *y = keyTable[row2][col1];
 }
 }
 void decryptPair(char keyTable[SIZE][SIZE], char a, char b, char *x, char *y) {
 int row1, col1, row2, col2;
 search(keyTable, a, b, &row1, &col1, &row2, &col2);
 if (row1 == row2) {
 *x = keyTable[row1][(col1 + SIZE- 1) % SIZE];
 *y = keyTable[row2][(col2 + SIZE- 1) % SIZE];
 } else if (col1 == col2) {
 *x = keyTable[(row1 + SIZE- 1) % SIZE][col1];
 *y = keyTable[(row2 + SIZE- 1) % SIZE][col2];
 } else {
 *x = keyTable[row1][col2];
 *y = keyTable[row2][col1];
 }
 }
 void encrypt(char keyTable[SIZE][SIZE], char plainText[], char cipherText[]) {
 int i, j = 0;
 char a, b;
 for (i = 0; plainText[i] != '\0'; i += 2) {
a =plainText[i];
 if (plainText[i + 1] != '\0') {
 b =plainText[i + 1];
 } else {
 b ='x';
 }
 if (a == b) {
 b ='x';
 i--;
 }
 encryptPair(keyTable, a, b, &cipherText[j], &cipherText[j + 1]);
 j += 2;
 }
 cipherText[j] = '\0';
 }
 void decrypt(char keyTable[SIZE][SIZE], char cipherText[], char plainText[]) {
 int i, j = 0;
 char a, b;
 for (i = 0; cipherText[i] != '\0'; i += 2) {
 a =cipherText[i];
 b =cipherText[i + 1];
 decryptPair(keyTable, a, b, &plainText[j], &plainText[j + 1]);
 j += 2;
 }
 plainText[j] = '\0';
}
 int main() {
 char key[100], plainText[100], cipherText[100], decryptedText[100];
 char keyTable[SIZE][SIZE];
 int i, keyLength;
 printf("Enter the key: ");
 scanf("%s", key);
 for (i = 0; key[i] != '\0'; i++) {
 key[i] = tolower(key[i]);
 }
 keyLength = strlen(key);
 generateKeyTable(key, keyLength, keyTable);
 printf("Enter the plaintext: ");
 scanf("%s", plainText);
 for (i = 0; plainText[i] != '\0'; i++) {
 plainText[i] = tolower(plainText[i]);
 }
 encrypt(keyTable, plainText, cipherText);
 printf("Encrypted Text: %s\n", cipherText);
 decrypt(keyTable, cipherText, decryptedText);
 printf("Decrypted Text: %s\n", decryptedText);
 return 0;
}
```




## Output:
![image](https://github.com/user-attachments/assets/ef00f90a-803d-41ec-a4d3-ba1e8abfb3c6)
Result:
The program is executed successfully

