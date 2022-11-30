# ProjectI2022
Cryptography Program in C

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int getChipper(int plain, int key){
    return (int) plain / key;
}

int getKey(int plain, int key){
    return (int) plain % key;
}


int** encrypt(char *str, int key)
{
    int** chipper = (int**)malloc(sizeof(int*) * 2); // first is chipper, second is chipkey
    chipper[0] = (int*)malloc(sizeof(int) * strlen(str));
    chipper[1] = (int*)malloc(sizeof(int) * strlen(str));

    for (int i = 0; i < strlen(str); i++)
    {
        int ascii = (int)str[i];
        chipper[0][i] = getChipper(ascii, key); // chipper
        chipper[1][i] = getKey(ascii, key); // chipkey
    }

    return chipper;
}

char *decrypt(int *chipper, int *chipkey, int key, int size)
{
    char *str = (char*)malloc(sizeof(char) * size);
    for (int i = 0; i < size; i++)
    {
        str[i] = (char)(chipper[i] * key) + chipkey[i];;
    }

    return str;
}

int main(){
    char str[100];
    int key;
    printf("Enter the plain string: ");
    scanf(" %[^\n]%*c", &str);
    printf("Enter the key: ");
    scanf("%d", &key);
    int** chipper = encrypt(str, key);

    printf("Chipper: \n");
    for (int i = 0; i < strlen(str); i++)
    {
        printf("%d ", chipper[0][i]);
    }
    printf("\nkey: \n");
     for (int i = 0; i < strlen(str); i++)
    {
        printf("%d ", chipper[1][i]);
    }

    char* decrypted = decrypt(chipper[0], chipper[1], key, strlen(str));
    printf("\nDecrypted: %s", decrypted);

    return 0;
}
