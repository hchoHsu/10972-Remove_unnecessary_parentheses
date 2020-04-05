# 10972-Remove_unnecessary_parentheses
```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#define MAXEXPR 256
#define NUMSYM 6

char expr[MAXEXPR] = "ABCD&|";
int pos;

typedef enum {ID_A, ID_B, ID_C, ID_D, OP_AND, OP_OR} TokenSet;
char sym[NUMSYM];

typedef struct _Node {
    TokenSet data;
    struct _Node *left, *right;
} BTNode;

BTNode* EXPR();
BTNode* FACTOR();
BTNode* makeNode(char c);
void freeTree(BTNode *root);
void printInfix(BTNode *root);

int main(void){
    scanf("%s", expr);
    pos = strlen(expr) - 1;
    BTNode *root = EXPR();
    printInfix(root);
    freeTree(root);

    return 0;
}

void printInfix(BTNode *root){
    if(root != NULL){
        /*if(root->data >= 0 && root->data <= 3){
            printf("%c", sym[root->data]); return;
        }*/

        printInfix(root->left);
        printf("%c", sym[root->data]);
        if(root->right != NULL){
            if((root->right)->data >= 4 && (root->right)->data <= 5){
                printf("(");
                printInfix(root->right);
                printf(")");
            }
            else printInfix(root->right);
        }
    }
}

/* clean a tree.*/
void freeTree(BTNode *root){
    if (root!=NULL) {
        freeTree(root->left);
        freeTree(root->right);
        free(root);
    }
}

BTNode* EXPR()
{
    char c;
    BTNode *node = NULL, *right = NULL;
    if(pos >= 0){
        right = FACTOR();

        if(pos > 0){
            c = expr[pos];
            //printf("pos = %d\n", pos);
            if(c == '&' || c == '|'){
                node = makeNode(c);
                node->right = right;
                pos--;
                node->left = EXPR();
            }
            else if(c == '('){
                node = right;
                pos--;
            }
            else node = right;
        }
        else node = right;
    }
    return node;
}
BTNode* FACTOR()
{
    char c;
    BTNode *node = NULL;
    //printf("In factor: ");
    if(pos >= 0){
        c = expr[pos--];
        if(c >= 'A' && c <= 'D'){
            node = makeNode(c);
        }
        else if(c == ')'){
            node = EXPR(); 
        }
        else if(c == '(') pos--;
    }
    return node;
}
BTNode* makeNode(char c)
{
    //printf("%c\n", c);
    BTNode *newNode = (BTNode* )malloc(sizeof(BTNode));
    for(int i = 0; i < NUMSYM; i++){
        if(c == sym[i]){
            newNode->data = i; break;
        }
    }
    newNode->left = newNode->right = NULL;
    return newNode;
}
//A|(B&C)|D
```
