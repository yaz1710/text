#include <iostream> 
#include <cctype> 
#include <string> 
using namespace std; 
#define LETTER 1 
#define DIGIT 10 
#define UNKNOWN 100 
#define INT_LIT 1 
#define IDENT 11 
#define ASSIGN_OP 2 
#define ADD_OP 22 
#define SUB_OP 3 
#define MULT_OP 33 
#define DIV_OP 4 
#define LEFT_PAREN 44 
#define RIGHT_PAREN 5 
int charClass; 
string lexeme; 
char nextChar; 
int token; 
int nextToken; 
string input = "A = ( M + Y )/100";  
int inputIndex = 0; 
void addChar(); 
void getChar(); 
void getNonBlank(); 
int lex(); 
int lookup(char ch); 
int main() { 
getChar(); 
do { 
lex(); 
} while (nextToken != EOF); 
return 0; 
} 
int lookup(char ch) { 
switch (ch) { 
case '(': 
addChar(); 
nextToken = LEFT_PAREN; 
break; 
case ')': 
addChar(); 
nextToken = RIGHT_PAREN; 
break; 
case '+': 
addChar(); 
nextToken = ADD_OP; 
 
            break; 
        case '-': 
            addChar(); 
            nextToken = SUB_OP; 
            break; 
        case '*': 
            addChar(); 
            nextToken = MULT_OP; 
            break; 
        case '/': 
            addChar(); 
            nextToken = DIV_OP; 
            break; 
        case '=': 
            addChar(); 
            nextToken = ASSIGN_OP; 
            break; 
        default: 
            addChar(); 
            nextToken = EOF; 
            break; 
    } 
    return nextToken; 
} 
 
void addChar() { 
    lexeme += nextChar; 
} 
 
void getChar() { 
if (inputIndex < input.length()) { 
nextChar = input[inputIndex++]; 
if (isalpha(nextChar)) 
charClass = LETTER; 
else if (isdigit(nextChar)) 
charClass = DIGIT; 
else 
charClass = UNKNOWN; 
} else { 
charClass = EOF; 
} 
} 
void getNonBlank() { 
while (isspace(nextChar)) 
getChar(); 
} 
int lex() { 
lexeme = ""; 
getNonBlank(); 
switch (charClass) { 
case LETTER: 
addChar(); 
getChar(); 
while (charClass == LETTER || charClass == DIGIT) { 
addChar(); 

                getChar(); 
            } 
            nextToken = IDENT; 
            break; 
        case DIGIT: 
            addChar(); 
            getChar(); 
            while (charClass == DIGIT) { 
                addChar(); 
                getChar(); 
            } 
            nextToken = INT_LIT; 
            break; 
        case UNKNOWN: 
            lookup(nextChar); 
            getChar(); 
            break; 
        case EOF: 
            nextToken = EOF; 
            lexeme = "EOF"; 
            break; 
    } 
 
    cout << "Next token is: " << nextToken << ", Next lexeme is " << lexeme << endl; 
    return nextToken; 
}