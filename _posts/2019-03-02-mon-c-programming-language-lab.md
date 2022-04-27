---
title:  "Môn: C Programming Language.Lab"
categories: 
    - programming
header:
    image: /images/2019-03-02-mon-c-programming-language-lab/c.png
---
<!--# Môn: C Programming Language.Lab-->
## Mục lục

## Các kiến thức cơ bản
- Syntax, common characters, keywords, variable names, compiler, IDE (1)

Nên dùng Sublime Text làm IDE, nhẹ, giao diện đẹp, compile dễ dàng.

- Variables: integer (decimal/hexa/octal format), long, float, double, char,... (2)

- ASCII table (2)
- Conversion character, skipping character, formatting with printf (2)

- Logic, statement (1)

- Control block: for, if, while, switch case (1)

- Function, pointers (2)

- Array (2)

- String (2)

- Struct (1)

- Input/output File (1)

## Các thuật toán/bài toán quan trọng
- Swap 2 variables
- Sắp xếp tăng dần, giảm dần
- Tìm UCLN, BCNN
- Tích giai thừa
- Tính số fibonacci
- ...


File template: "**insert here**"
## Các thư viện
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
```



## String manipulation

```c
#define MAX_STR 1000
```

### Một số function 
#### `strcmp`
```c
res = strcmp(s1, s2);

if(res == 0) printf("Equal");
else printf("Not equal");
```

#### `strcpy`
```c
strcpy(dest, src);
```

#### `strcat`
```c
strcat(dest, src);
```
return a pointer to the resulting string dest

## Struct
### Define
```c
typedef struct [structure tag]{
    member definition;
    ...
    member definition;
    
} [custom structure tag];
```

### Access members
```c
book a;
a.name;
a.date;

book *aptr = &a;
aptr->name;
aptr->date;
```

## Reference
- [https://www.tutorialspoint.com](https://www.tutorialspoint.com)











