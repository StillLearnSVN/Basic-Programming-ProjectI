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
        printf("%d ", ascii);
        chipper[0][i] = getChipper(ascii, key); // chipper
        chipper[1][i] = getKey(ascii, key); // chipkey
    }
    printf("\n");

    return chipper;
}

int *decrypt(int *chipper, int *chipkey, int key, int size)
{
    int *arr = (int*)malloc(sizeof(int) * size);
    // set default to null
    memset(arr, 0, sizeof(int) * size);
    for (int i = 0; i < size; i++)
    {
        arr[i] = (chipper[i] * key + chipkey[i]);
    }

    printf("\n");
    return arr;
}

int main(){
    char str[100];
    int key, len;
    printf("Enter the plain string: ");
    scanf("%99s%n", &str, &len);
    printf("Enter the key: ");
    scanf("%d", &key);
    int** chipper = encrypt(str, key);

    printf("Chipper: \n");
    for (int i = 0; i < len; i++)
    {
        printf("%d ", chipper[0][i]);
    }
    printf("\nkey: \n");
     for (int i = 0; i < len; i++)
    {
        printf("%d ", chipper[1][i]);
    }

    int* decrypted = decrypt(chipper[0], chipper[1], key, len);
    free(chipper);
    printf("Decrypted: ");
    for (int i = 0; i < len; i++)
    {
        printf("%c", (char)decrypted[i]);
    }
    printf("\n");

    return 0;
}
