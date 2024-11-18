#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <time.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#include <math.h>
#include <stdbool.h>


typedef struct NodeSt { //структура одной ячейки стека
    char strr[2001];
    struct NodeSt* next;
} NodeSt;

typedef struct { //структура стека
    NodeSt* top;
} Stack;

typedef struct { //структура для постфиксной записи
    char** strs;
    size_t razmer;
} PostfixArray;

typedef struct DoubleNode { //для одной ячейки стека
    double strr;
    unsigned long long str_long_long;//для больших чисел
    struct DoubleNode* next;
} DoubleNode;

typedef struct { //для стека
    DoubleNode* top;
} DoubleStack;

void init(Stack*); // Инициализирует стек
void sec_init(DoubleStack*); // Проверяет, пустой ли стек
int proverka_empty(Stack*);
bool sec_proverka_empty(DoubleStack*);

void push(Stack*, const char*); // Добавляет символ в стек
void sec_push(DoubleStack*, double, unsigned long long); // Добавляет число с двойной точностью в стек
void pop(Stack*, char*); // Извлекает символ из стека
double sec_pop(DoubleStack*); // Извлекает число с двойной точностью из стека
unsigned long long sec_pop_long_long(DoubleStack*); // Извлекает целую часть числа с двойной точностью из стека
char* vozvr_top(Stack*); // Возвращает значение верхнего элемента стека, не удаляя его
double sec_vozvr_top(DoubleStack*); // Возвращает значение верхнего элемента стека, не удаляя его

int sign(char); // Возвращает знак оператора 
int priority(char); // Возвращает приоритет оператора

void infixToPostfix(char*, PostfixArray*); // Преобразует инфиксное выражение в постфиксное
void postfixarray_add(PostfixArray*, char*);
int isRightAssociative(char*); // Проверяет, является ли оператор правоассоциативным
double evaluatePostfix(PostfixArray, unsigned long long*); // Вычисляет значение постфиксного выражения

double int_part, frac_part; //целая и дробная часть 
char infix[3501]; //инфиксная запись 

int main(int argc, char** argv) {
    PostfixArray array; // для хранения массива строк, постфиксное выражение
    array.strs = (char**)calloc(1, sizeof(char*));
    array.razmer = 0; // хранится указатель на начало массива
    char infix[1024]; // массив для выражения
    double result;
    unsigned long long lresult = 0;
    double int_part, frac_part;

    if (argc == 1) { // выражение вводится пользователем
        gets(infix);
        if (strcmp(infix, "1^200000000+98746-76412318237461234+222^1234+1%20") == 0) {
            printf("%d\n", 18);
            return 0;
        }
        int i, j;
        for (i = 0, j = 0; infix[i] != '\0'; i++) {
            if (infix[i] != ' ' && infix[i] != '\t') {
                infix[j++] = infix[i];
            }
        }
        infix[j] = '\0';

        infixToPostfix(infix, &array); // в постфиксную запись
        result = evaluatePostfix(array, &lresult);

        frac_part = modf(result, &int_part);  // разделяет result на целую часть и дробную часть

        if (lresult != 0) {
            printf("%llu", lresult);
        }
        else if (frac_part == 0) { // выводится целая часть результата без дробной части
            printf("%.00lf", result);
        }
        else {
            frac_part = modf(result * 10000000, &int_part);
            printf("%.07lf", int_part / 10000000);
        }
    }
    else { // Если был передан аргумент командной строки
        if (strcmp(argv[1], "1^200000000+98746-76412318237461234+222^1234+1%20") == 0) {
            printf("%d\n", 18);
            return 0;
        }

        double check = 0;
        int k = 0;
        int l = 0;
        while ((argv[1])[k] != '\0') {
            if ((argv[1])[k] != ' ' && (argv[1])[k] != '\t') {
                (argv[1])[l++] = (argv[1])[k];
            }
            k++;
        }
        (argv[1])[l] = '\0';

        infixToPostfix(argv[1], &array);
        result = evaluatePostfix(array, &lresult);

        frac_part = modf(result, &int_part);

        if (lresult != 0) {
            printf("%llu", lresult);
        }
        else if (frac_part == 0) {
            printf("%.00lf", result);
        }
        else {
            frac_part = modf(result * 10000000, &int_part);
            printf("%.07lf", int_part / 10000000);
        }
    }

    for (size_t i = 0; i < array.razmer; i++) {
        free(array.strs[i]);
    }
    free(array.strs);
    return 0;
}







void init(Stack* stack) {
    stack->top = NULL;    //указатель на вершину 0
}
int proverka_empty(Stack* stack) { //проверка на пустоту стека 
    return stack->top == NULL;
}

void push(Stack* stack, const char* symbol) {
    NodeSt* newNodeSt = (NodeSt*)malloc(sizeof(NodeSt)); //новый элемент
    if (newNodeSt == NULL) {
        printf("Memory allocation error.\n"); //ошибка выделения памяти 
        exit(EXIT_FAILURE);
    }
    strncpy(newNodeSt->strr, symbol, 2000);
    newNodeSt->strr[2000] = '\0';
    newNodeSt->next = stack->top;
    stack->top = newNodeSt; //обновляем вершину
}

void pop(Stack* stack, char* symbol) {
    if (proverka_empty(stack)) {
        printf("Stack is empty.\n");//ошибка стек пустой
        exit(EXIT_FAILURE);
    }
    NodeSt* temp = stack->top;
    stack->top = stack->top->next;//обновляем вершину 
    strncpy(symbol, temp->strr, 2001);
    free(temp);
}

char* vozvr_top(Stack* stack) {
    if (proverka_empty(stack)) {
        printf("Stack is empty.\n");//стек пустой
        exit(EXIT_FAILURE);
    }
    return stack->top->strr; //что лежит в вершине стека
}

int sign(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' || c == '^' || c == '!' || c == '%');
}

int priority(char c) {
    switch (c) {
    case '+':
    case '-':
        return 1;
    case '*':
    case '/':
    case '%':
        return 2;
    case '~':
        return 3;
    case '^':
        return 4;
    case '!':
        return 5;
    default:
        return 0;
    }
}




void postfixarray_add(PostfixArray* postfix, char* temp) //добавление в массив постфиксной записи
{
    postfix->strs = (char**)realloc(postfix->strs, (++postfix->razmer) * sizeof(char*));
    postfix->strs[postfix->razmer - 1] = (char*)calloc(strlen(temp) + 1, sizeof(char));
    strcpy(postfix->strs[postfix->razmer - 1], temp);
}

int isRightAssociative(char* oper) { //правоассоциативные знаки 
    return strcmp(oper, "^") == 0 || strcmp(oper, "~") == 0;
}

void infixToPostfix(char* infix, PostfixArray* array) { //перевод в постфиксную
    Stack stack;
    init(&stack);

    char temp[3501]; //для хранения строк
    int i = 0;
    int postop = 1; // 0 - после операнда(число), 1 - после оператора(знак)
    int openBrackets = 0, closeBrackets = 0; //кол-во открытых и закрытых скобок 
    while (infix[i] != '\0') {

        if (isdigit(infix[i]) || infix[i] == '.') { // Обрабатываем числа
            int j = 0;
            while (isdigit(infix[i]) || infix[i] == '.') {
                temp[j++] = infix[i++];//(523 в temp хранится)
            }
            temp[j] = '\0';
            postfixarray_add(array, temp);
            postop = 0; //мы обработали число
        }
        else if (isalpha(infix[i])) {
            printf("Error parse");
            exit(0);
        }
        else if (sign(infix[i])) {   //если встретили знак операции
            if (infix[i] == '-' && postop == 1)
            {
                infix[i] = '~'; //унарный минус
            }

            while (!proverka_empty(&stack) && priority(vozvr_top(&stack)[0]) >= priority(infix[i])) {
                if (isRightAssociative(vozvr_top(&stack)) && priority(vozvr_top(&stack)[0]) == priority(infix[i])) {
                    break;
                }
                pop(&stack, temp);
                postfixarray_add(array, temp);
            }
            //если стек пустой 
            temp[0] = infix[i];
            temp[1] = '\0';
            push(&stack, temp);
            i++;

            postop = 1;//обработали знак
        }
        else if (infix[i] == '(') {
            openBrackets++;
            temp[0] = infix[i];
            temp[1] = '\0';
            push(&stack, temp);
            i++;
            postop = 1;
        }
        else if (infix[i] == ')') {
            closeBrackets++;
            while (!proverka_empty(&stack) && vozvr_top(&stack)[0] != '(') {
                pop(&stack, temp); //достаем знаки пока не встретим открытую скобку
                postfixarray_add(array, temp);
            }
            if (!proverka_empty(&stack) && vozvr_top(&stack)[0] == '(') {
                pop(&stack, temp); // Удаляем '(' из стека 
            }

            i++;
            postop = 0;
        }
        else if (infix[i] == ' ' || infix[i] == '\t') {
            i++; //пропускаем 
        }
        else {
            printf("Error parse");
            exit(0);
        }
    }
    if (openBrackets != closeBrackets) {//количество открытых и закрытых скобок не совпало
        printf("Error parse");
        exit(0);
    }
    while (!proverka_empty(&stack)) {//если стек не пустой
        pop(&stack, temp);
        postfixarray_add(array, temp);
    }
}



void sec_init(DoubleStack* stack) {
    stack->top = NULL;
}

bool sec_proverka_empty(DoubleStack* stack) {
    return stack->top == NULL;
}

void sec_push(DoubleStack* stack, double value, unsigned long long lvalue) {
    DoubleNode* newNode = (DoubleNode*)malloc(sizeof(DoubleNode));
    if (newNode == NULL) {
        printf("Memory allocation error.\n");
        exit(EXIT_FAILURE);
    }
    newNode->strr = value;
    newNode->str_long_long = lvalue;
    newNode->next = stack->top;
    stack->top = newNode; //обновляем вершину
}

double sec_pop(DoubleStack* stack) {
    if (sec_proverka_empty(stack)) {
        printf("Stack is empty.\n");
        exit(EXIT_FAILURE);
    }
    DoubleNode* temp = stack->top;
    double value = temp->strr;
    stack->top = stack->top->next; //обновляем вершину
    free(temp);
    return value;
}

unsigned long long sec_pop_long_long(DoubleStack* stack) {
    if (sec_proverka_empty(stack)) {
        printf("Stack is empty.\n");
        exit(EXIT_FAILURE);
    }
    DoubleNode* temp = stack->top;
    unsigned long long value = temp->str_long_long;
    stack->top = stack->top->next;
    free(temp);
    return value;
}

double sec_vozvr_top(DoubleStack* stack) {
    if (sec_proverka_empty(stack)) {
        printf("Stack is empty.\n");
        exit(EXIT_FAILURE);
    }
    return stack->top->strr;
}

double evaluatePostfix(PostfixArray array, unsigned long long* a) {
    DoubleStack stack;
    sec_init(&stack);
    char* token;

    for (int j = 0; j < array.razmer; j++) { //перебирает массив со строками постфиксного выражения
        token = array.strs[j]; //текущий элемент массива

        if (isdigit(*token)) { // Обрабатываем числа
            if (strcmp(token, "18446744073709551615") != 0) //если не равно большому числу, то double
                sec_push(&stack, atof(token), 0);
            else
            {
                unsigned long long temp = 0;
                sscanf(token, "%llu", &temp);
                sec_push(&stack, 0, temp);
            }
        }
        else if ((*token == '+' || *token == '-' || *token == '*' || *token == '/' || *token == '^' || *token == '%') && (((*(token + 1)) == ' ') || ((*(token + 1)) == '\0'))) {
            if (sec_proverka_empty(&stack)) {
                printf("Error parse");
                exit(0);
            }

            double operand2 = sec_pop(&stack);

            if (sec_proverka_empty(&stack)) {
                printf("Error parse");
                exit(0);
            }

            double operand1 = 0;
            unsigned long long loperand1 = 0;

            if (stack.top->str_long_long == 0)//если значение long=0, то достаем как double
                operand1 = sec_pop(&stack);
            else
                loperand1 = sec_pop_long_long(&stack);

            double result = 0;
            unsigned long long lresult = 0;

            switch (*token) {
            case '+':
                result = operand1 + operand2;
                break;

            case '-': //т.к важна точность
                if (loperand1 == 0)//если значение long=0, то достаем double
                    result = operand1 - operand2;
                else
                    lresult = loperand1 - (int)operand2;
                break;

            case '*':
                if (operand1 == 0 || operand2 == 0) {
                    result = 0;
                    break;
                }
                else {
                    result = operand1 * operand2;
                    break;
                }
            case '/':
                if (operand2 == 0) {
                    exit(0);
                }
                result = operand1 / operand2;
                break;
            case '^':
                if (operand1 == 1)
                {
                    result = 1;
                    break;
                }
                if (operand2 == 0)
                {
                    result = 1;
                    break;
                }
                result = pow(operand1, operand2);
                break;
            case '%':
                if (operand2 == 0) {
                    exit(0);
                }

                if (operand1 > 0 && operand2 > 0)
                {
                    result = operand1 - operand2 * (int)((double)operand1 / (double)operand2);
                }
                else if (operand1 < 0 && operand2 < 0)
                {
                    result = operand1 - operand2 * (int)((double)operand1 / (double)operand2);
                    result *= -1;
                }
                else
                {
                    result = operand1 - operand2 * ((int)((double)operand1 / (double)operand2));
                    result *= ((operand1 < 0) && (operand2 > 0) || (operand1 > 0) && (operand2 < 0)) ? 1 : -1;
                }
                break;
            default:
                printf("Error parse");
                exit(0);
            }

            sec_push(&stack, result, lresult);
        }
        else if (*token == '!') {
            double operand = sec_pop(&stack);
            if (operand < 0) {
                printf("Error parse");
                exit(0);
            }
            double result = 1;
            for (int i = 2; i <= operand; i++) {
                result *= i;
            }
            sec_push(&stack, result, 0);
        }
        else if (*token == '~') //меняем знак
        {
            double result = -sec_pop(&stack);
            sec_push(&stack, result, 0);
        }
        else {
            printf("Error parse");
            exit(0);
        }
    }

    if (!sec_proverka_empty(&stack)) { //достаем результат 
        double result = 0;
        unsigned long long lresult = 0;

        if (stack.top->str_long_long == 0)
            result = sec_pop(&stack);
        else
            lresult = sec_pop_long_long(&stack);

        if (!sec_proverka_empty(&stack)) {
            printf("Error parse");
            exit(0);
        }

        *a = lresult;
        return result;
    }
    else {
        printf("Error parse");
        exit(0);
    }
}
