# ProjectI2022
Cryptography Program in C

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>


#define ENCRYPT_MSG 1
#define DECRYPT_MSG 2
const char URETRY = 'M';
const char LRETRY = 'm';
const char UNOTRETRY = 'E';
const char LNOTRETRY = 'e';

void menu(){
    printf("1. Encrypt Message \n2. Decrypt Message\n");
    printf("Select the Operation that you want to do..\n");
}

int getChipper(int plain, int key) //this Function to get the chiper text
{
    return (int) plain / key;
}

int getChipKey(int plain, int key) //this function to get the key
{
    return (int) plain % key; 
}


int** encrypt(char *str, int key, int len)
{
    int** chipper = (int**)malloc(sizeof(int) * 2); // first is chipper, second is chipkey
    chipper[0] = (int*)malloc(sizeof(int) * len); // chipper[0][4] --> row for chipper
    chipper[1] = (int*)malloc(sizeof(int) * len+1); // chipper[1][4] --> row for key

    printf("\nChipper Text: %s\n", str);
    printf("There is a ASCII value:\n");

    for (int i = 0; i < len; i++)
    {
        if(isalnum(str[i])){             // catch if and only if alphanumeric
            int ascii = (int)str[i];
            printf("%d ", ascii); // to get the ascii
            chipper[0][i] = getChipper(ascii, key); // chipper
            chipper[1][i] = getChipKey(ascii, key); // chipkey
        }
    }
    printf("\n");

    return chipper;
}

int *decrypt(int *chipper, int *chipkey, int key, int size)
{
    int *arr = (int*)malloc(sizeof(int) * size);
    memset(arr, 0, sizeof(int) * size);  // set default to null
    for (int i = 0; i < size; i++)
    {
        arr[i] = (chipper[i] * key + chipkey[i]); // to get the plaintext
    }

    printf("\n");
    return arr;
}

void showEncrypt(char *str,int key, int len){
    int** chipper = encrypt(str, key, len);

    printf("Chipper: \n");
    for (int i = 0; i < len; i++)
    {
        printf("%d ", chipper[0][i]);
    }
    printf("\nChipkey: \n");
    for (int i = 0; i < len; i++)
    {
        printf("%d ", chipper[1][i]);
    }
    printf("\n");
}
void showDecrypt(int **chipper, int key, int len){
    int* decrypted = decrypt(chipper[0], chipper[1], key, len);
    printf("Decrypted: ");
    for (int i = 0; i < len; i++)
    {
        printf("%c", (char)decrypted[i]);
    }
    printf("\n");
}

int **getChipperChipkey(char *chipper, char* chipkey, int *len){
    int **arr = (int**)malloc(sizeof(int) * 2);
    int i = 0;

    arr[0] = (int*)malloc(sizeof(int) * 100);
    arr[1] = (int*)malloc(sizeof(int) * 100);

    // split chipper by space
    char *token = strtok(chipper, " ");
    while (token != NULL)
    {
        arr[0][i] = atoi(token);
        token = strtok(NULL, " ");
        i++;
    }

    // split chipkey by space
    token = strtok(chipkey, " ");
    i = 0;
    while (token != NULL)
    {
        arr[1][i] = atoi(token);
        token = strtok(NULL, " ");
        i++;
    }

    *len = i;

    return arr;
}

void retry(){
    char tempRetry;
    printf("Kembali ke menu? (enter M/m)\nKeluar? (enter E/e) : ");
    fflush(stdout);   // clear stdout, because probably include to scanf
    scanf(" %c",&tempRetry); // also to ignore newlines in scanf use this format
    printf("Current isRetry Prosedur is %c\n",tempRetry);
    if(tempRetry != URETRY && tempRetry != LRETRY && tempRetry != UNOTRETRY && tempRetry != LNOTRETRY){
        retry();
    }
    if(tempRetry == UNOTRETRY || tempRetry == LNOTRETRY){
        exit(0);
    }
}


int main(){

    while(1){
        char str[100];
        int select,len,key;
        char strChipper[100], strChipkey[100];
        int **chipper = (int**)malloc(sizeof(int) * 2);
        system("cls");
        menu();
        scanf("%d", &select);

        switch(select){
            case ENCRYPT_MSG:
                printf("Let's Encrypt the message!\n");
                printf("Enter the plain string: ");
                scanf("%99s%n", &str, &len); // %99s to get the range for char str and %n to get the length of string
                printf("Enter the key: ");
                scanf("%d", &key);
                showEncrypt(str,key,len-1);
                break;
            case DECRYPT_MSG:
                printf("Let's Decrypt the message!\n");
                printf("Enter the Chipper: ");
                scanf(" %[^\n]", strChipper);
                printf("Enter the ChipKey: ");
                scanf(" %[^\n]",&strChipkey);
                printf("Enter the key: ");
                scanf("%d", &key);
                chipper = getChipperChipkey(strChipper, strChipkey, &len);
                // printf("\nLen while decrypt : %d\n", len);
                showDecrypt(chipper, key, len);
                break;
            default:
                printf("There is only Two choice thats u need to choose [1] for Encrypt | [2] for Decrypt\n");
        }
        retry();
    }
    

 }
