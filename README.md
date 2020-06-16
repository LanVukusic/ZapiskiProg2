# GOOD STUFF
### To je pač stuff ka sm ga rabu pr delanju nalog in sm ga mogu pogooglat... ja getchar nism vedu...sue me.

 - tolower(char) ti vrne stvari z lowercase  
 - toupper(char) ti vrne stvari z uppercasom  
 - getchar() bere naslednji thing iz stdio in ga returna  
 - scanf("%d", *float) boljši getchar; Bere iz vhoda formatan input, ter zapiše na addres drugega argumenta.  
 - putchar(char) napiše ta bogi osamljen char  
 - printf("formatan string %d") putchar na steroidih.  

 - __ZMERI RES ZMERI USE FAKING INITI Z 0 AL PA NULL...KR CENE BOS DO 01:18 DEBUGAU FAKING 2 VRSTICI__


# operator &  

__Fora tega "end"-a je da ti da memory naslov stvari. recimo:__  

```c
#include <stdio.h>

int main(){
    int i = 3;
    printf("%18p",&i);  // sprinti naslov od i -ja
}
```

> 0x55810e567018

# POINTERJI *
__Tole ( * ) ti da pointer do neke vrednosti. To nam omogoča da delamo z njihovimi naslovi in ne z vrednostjo direkt. To je kul kr lahko potem to vrednosts preminjaš al pa jo pointaš drugam.  

Ideja za tem je taka. tole zvezdico __(*)__ mej u glavi kokr _"vrednost na naslovu"_.  

Nardimo primer za lazje razumet ... Najprej nardimo neko int vrednost in pointer ki ne kaze nikamor in ma random vrednost.  

```c
int stevilo = 420;
int *ptr;
```

Potem moremo povedat kam nej ta pointer kaže. Sepravi na kater __NASLOV__ ne na katero vrednost. (nas zanima samo addres od "stevilo")

```c
ptr = &stevilo;  // &nekaj vrne naslov od nekaj
```

Pointer zdej kaže na naslov na katerem se nahaja število.  
Če želimo iz pointerja dobit vrednost, uporabljamo tist "vrednost na naslovu" od zgori:  

```c
int lol = *ptr;  // lol je "vrednost na naslovu" ptr
*ptr = 17;  // "vrednost na naslovu" ptr je 17
```

Lep primer uporabe:

```c
#include <stdio.h>

int main () {

   int  var = 20;   /* actual variable declaration */
   int  *ip;        /* pointer variable declaration */

   ip = &var;  /* store address of var in pointer variable*/

   /* NASLOV o vrednosti kot stevilka */
   printf("Address of var variable: %x\n", &var  );

   /* NASLOV od vrednosti */
   printf("Address stored in ip variable: %x\n", ip );

   /* VREDNOST na NASLOVU  */
   printf("Value of *ip variable: %d\n", *ip );

   return 0;
}
```

 vrne :  
> Address of var variable: bffd8b3c  
  Address stored in ip variable: bffd8b3c  
  Value of *ip variable: 20


__Pointer lahko tudi povečaš z recimo _ptr++_; Zabavno je, kr ti "visokonivjoski C" nardi u ozadju vs garbage iz ARS-a in ti naslov dejansko inkrementira za 4 (ni zmer 4... ja ok whatever), tako da kaže na nov ukaz. Hvala C.  
Uporabna vrednost tega je recimo to da lahko tko na izi loopaš čez tabele:  
Primer:__  
```c
#include <stdio.h>

const int MAX = 2;

int main () {

   int  var[] = {10, 100};
   int  i, *ptr;

   ptr = var;  // damo v pointer lokacijo tabele
   i = 0;
	
   while ( ptr <= &var[MAX - 1] ) {  // ponterje lahko primerjamo z klasičnimi operatorji

      printf("Address of var[%d] = %x\n", i, ptr );
      printf("Value of var[%d] = %d\n", i, *ptr );

      /* point to the next location */
      ptr++;  // dejansko inkrementira na naslednji poravnan naslov. AKA +4
      i++;
   }
	
   return 0;
}

```

> Address of var[0] = bf882b30  
 Value of var[0] = 10  
 Address of var[1] = bf882b34  
 Value of var[1] = 100

 Tuki je pomembno da maš u glavi tisto __* = vrednost na naslovu__  
 Kr potem lahko delaš neki u tem smislu:  

 ```c
 // nadaljevanje code - blocka od zgori
 int lol = *(ptr + 4);
 // vrednost na naslovu: ptr (kamor kaze pointer) + 4
 // AKA vrednost na ptr + 4 zaporednem naslovu
 // AKA ptr[4]
 ```

__Seveda nam nihče ne preprečuje da se nebi malo pozabavali in nardili pointer fuckfest.  
Primer:__  

```c
#include <stdio.h>
 
int main () {

   int  var;  // nič hudega sluteči int
   int  *ptr;  // pointer → int
   int  **pptr;  // pointer → pointer → int

   var = 3000;

   /* take the address of var */
   ptr = &var;

   /* take the address of ptr using address of operator & */
   pptr = &ptr;

   /* take the value using pptr */
   printf("Value of var = %d\n", var );
   printf("Value available at *ptr = %d\n", *ptr );
   printf("Value available at **pptr = %d\n", **pptr);

   return 0;
}
```

>Value of var = 3000  
 Value available at *ptr = 3000  
 Value available at **pptr = 3000


__IMO je najbolj pomemben deu pointerjeu to da jih lahko metaš u funkcije in pol mutiraš zadeve iz funkcije vn.  
Primer:__ 


```c
 int A, B;
 B = 5;
 A = twoValues(&B);

 int twoValues(int *x){  // funkcija išče reference
  int y = *x * 2;
  *x = x + 10;  //lahko mutiramo tud podan element
  return y; 
 }

 printf("A:%d, B:%d",A,B)
```
>  A:10  B:15  

Če je to rahlo confusing je tle še en primer.  
```c
// nek test cja
#include <stdio.h>

//int racunaj(int* ,int*);

int racunaj(int* i, int* j){
    int res = *i + *j;
    *i = 0;
    *j = res;
    return res;
}

int main(){
    int value = 3000;
    int value2= 1000;

    int *ptri = &value;

    //printf("value: %d, ptr: %d, ptr*: %d", value, *ptr, **ptrr);
    int a = racunaj (ptri, &value2);

    printf("value i: %d, value j: %d\n", value, value2);
    return (0);
}
```
> value i: 0, value j: 4000

A drži vrednost ki jo zračuna funkcija  
B pa dobi novo vrednost, ki jo je funkcija spremenila

# TABELE  

```c
int data[100]; 
int mark[5] = {19, 10, 8, 17, 9};
int mark[] = {19, 10, 8, 17, 9};
int *ptr[5];  // yes pointers mmm.... to je prazn array pointrjeu

// branje in pisanje elementov
mark[2] = -1;

// to je usefull trick
// input shrani na i-to mesto arraya
scanf("%d", &mark[i-1]);
```
To je ubistvu to.  
Mamo sicer še 2D ka zgledajo tko:  

```c
int c[2][3] = {{1, 3, 0}, {-1, 5, 9}};
int c[][3] = {{1, 3, 0}, {-1, 5, 9}};
int c[2][3] = {1, 3, 0, -1, 5, 9};

float x[3][4];
```

Dost bl usefull je, da namesto da nardiš tabelo, nardiš samo pointer in alociraš N placa. Ta primer mi je dost bl prou pršu  
Primer:  
```c
# include <stdio.h>
# include <stdlib.h>

int main(){
    // preberemo kolk nizov bomo vpisal
    int len = 0;
    scanf("%i ",&len);

    // alociramo tolk placa v spominu da lahk shranmo use to notr
    char *str1 = calloc(len,  sizeof(char));

    // zaloopamo tolkrat in beremo naše črke
    for (int i = 0; i < len; i++){
        // pointer zamikamo za en naslov naprej, ter na to mesto pišemo vrednost
        *(str1 + i) = getchar();
    }

    printf("\nVALS: ");

    // če VEŠ doužino
    for (int i = 0; i < len; i++){
        putchar(*(str1 + i));
    }

    // če NEVEŠ doužine
    int count = 0;
    // Basically beremo in potem pogledamo če smo že pršli do ničelnega elementa
    // problem je samo da mamo težko "0" v tej tabeli
    do{
       putchar(*(str1 + count));
       count++;
    } while (*(str1 + count) != 0);
    

    // čeprou nismo definiral "klasične" tabele lahko tko dostopaš do elementou
    char lol = str1[2];
} 

```


Ker je C zelo gay, predstavimo _string_ e z tabelo 🙉🙊
```c
char str1[12] = "Hello";
char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'};

// char arraye lahko normalno printamo in nam izpiše "Hello"
printf("Greeting message: %s\n", greeting );
```
>Greeting message: Hello

Če so stringi Null terminated ('\0' nakonc), lahko na jih zmetamo en kup funkcij kot so:  
|Funkcija|Opis|
|---|---|
|strcpy(s1, s2);|Kopira S2 v S1|
|strcat(s1, s2);|Concata (prilepi) S2 na konc S1|
|strlen(s1);| Vrne dolžino stringa|
|strcmp(s1, s2);| Vrne: __0__ ko sta S1 in S2 enaka; __<0__ ko S1<S2; __>0__ ko S1>S2|
|strchr(s1, ch);|Vrne pointer na mesto kjer se prvič nahaja ch v S1|
|strstr(s1, s2);|Vrne pointer na mesto kjer prvič najde S2 v S1


# STRUCTI
To je najbližji JSON-a ka boš v Cj pršu  
```c
struct Person {
    char name[50];
    int citNo;
    float salary;
};

// dostopamo z . kot pri normalnih jezikih
float letePare = person2.salary; 

// tko passaš te structe v funkcijo
float dvojniDenarci (struct Person osebica){
    return osebica.salary * 2;
}

int main(){
    // tko lahko definiraš nove osebe
    struct Person person1, person2, p[20];
    return 0;
}
```
Ker hočemo pisat bl hitro in smooth obstaja še ta notacija:  
```c
//agh bad no
struct Distance{
    int feet;
    float inch;
};

int main() {
    struct Distance d1, d2;
}


//mmm gooood yes
typedef struct Distance{
    int feet;
    float inch;
} distances;

int main() {
    distances d1, d2;
}
```
Lahko jih nestaš obviously...

__PAZI!!!__ Če naprimer definiraš drevo, ka ma neke node, moreš referencirat po  "struct ime". Primer:  
__PAZI SPET__ Če je node list drevesa, potem njegove left in right pointerje setaj na NULL;  
Čene dobiš tak garbage:  
> 10  
 1  
 29590344  
 Segmentation fault (core dumped)
```c
typedef struct binNode {
    int value;
    struct binNode *ptrLeft;  // tole lepo dela
    node1 *ptrRight;  // tole ne dela tud ce smo rekli da je isto
} node1;
```

Tuki pridemo do naslednjega bolečega spoznanja...in sicer __malloc__ in __sizeof__.  
__PAZI__ includat moreš _"<stdlib.h>"_  
Primer uporabne na structih:  

```c
struct person {
   int age;
   float weight;
   char name[30];
};

int main(){
  struct person *ptr;
   int i, n;

   printf("Enter the number of persons: ");
   scanf("%d", &n);

   // allocating memory for n numbers of struct person
   ptr = (struct person*) malloc(n * sizeof(struct person));

   for(i = 0; i < n; ++i)
   {
       printf("Enter first name and age respectively: ");

       // To access members of 1st struct person,
       // ptr->name and ptr->age is used

       // To access members of 2nd struct person,
       // (ptr+1)->name and (ptr+1)->age is used

       // tuki prebere in zapiše direkt na memory naslove.
       scanf("%s %d", (ptr+i)->name, &(ptr+i)->age);
   }

   printf("Displaying Information:\n");
   for(i = 0; i < n; ++i)
       printf("Name: %s\tAge: %d\n", (ptr+i)->name, (ptr+i)->age);

   return 0;
}
```
__malloc()__ ti memory alocata (bruh) neko velikost na disku  
__calloc()__ ti memory alocata plus seta ta memory block na 0. calloc(a, b); kjer je __a__ število alociranih blokov, __b__ pa velikost bloka.  


Pri obeh  lah dostopaš do pointerjev kokr array.  
```c
arr = calloc (10, sizeof(float));
printf(arr[3]);
```  
__sizeof()__ ti vrne kok je ena stvar velika. Dostkrat rabiš v uporabi z malloc da veš kolko prostora rabiš.
Ta vrstica je usefull:  

```ptr = (struct person*) malloc(n * sizeof(struct person));```

Če imamo nek struct lahko dostopamo do njegovih komponent z ".", če smo fancy af in mamo pointer "*ptr" na struct pa uporabljamo tole :  
__(ptr) -> name__; ekvivalentno __oseba.name__ če bi meli direkt osebo

# UNIONS
Okay zdej sm zvedu da je tole nek slabši struct.  
Fora je da __UNION__ rezervira tok memorya da lahko notr spraviš največčjega memberja, __STRUCT__ pa rezervira plac za use memberje... kokr bi člouk prčakavu....  

In other words, če maš u unionu 5 različnih stvari, lahko uporabljas od teg sam enga (pač delijo si memory, sepravi če zapišeš v prvi element pokvariš drugega itd.). Realno ne vidim nekga usecasa za to uporabljat.  

Kak Čopi bi lahko opazu da so lahko sick za kšne reees nišne optimizacije... sam pač struct go brrr  

# ENUM
Yey končno neki uporabnega nazaj.  
To je uno ka nardiš int in si z številkami od 1 do 5 označuješ neka stanja in jih pol pozabiš.  
Enum je proper way za to nardit.  
```c
enum State {Working = 1, Failed = 0}; 
```
Tle je primer
```c
#include<stdio.h> 
  
enum week{Mon, Tue, Wed, Thur, Fri, Sat, Sun}; 
  
int main() { 
    enum week day; 
    day = Wed; 
    printf("%d",day); 
    return 0; 
}
```
izpiše:
> 2

To je pa tud to.



# PODATKOVNE STRUKTURE REKURZIJA ..JADA JADA
Tle pač ni nč takga sepcial. razna drevesa in take itak veš kako delajo in pol razni heapi, linked listi queueueu-ji itd... pač znaš že iz drugih jezikou. implementiras z structi

# DATOTEKE read/write

Ideja je dost podobna kokr u pythonu. Nardiš nek file object, in mu poveš kaj gleda pa nek MODE mu da. alora read/write/readbytes/append ...  
tle so usi MODI:  https://www.programiz.com/c-programming/c-file-input-output  

To copypasti in ti bo zapisalo v fajl 👌👌

``` c
FILE *fptr;
fptr = fopen("E:\\cprogram\\newprogram.txt","r");
fprintf(fptr,"%d",12);
fclose(fptr)
```

Writing file :
```c
#include <stdio.h>
#include <stdlib.h>

int main (int argc, char *args[])
{
    if (argc != 2) exit (1);
    char *name = args[1];

    FILE *file;
    file = fopen (name, "a");
    if (file == NULL) {
        fprintf (stderr, "Cannot open file '%s' for writing.\n", name);
        exit (1);
    }

    // tole je un deu ka piše u fajl
    printf ("fprintf=%d\n", fprintf (file, "Hello, world."));

    if (fclose (file) != 0) {
        fprintf (stderr, "Cannot close file '%s'.\n", name);
        exit (1);
    }

    return 0;
}
```
 Branje vrstic:  
 _EOF (end of file) je in built char ka lahk z njim primerjas kdaj si konc_

 To copypasti in ti bo bralo 👌👌

``` c
FILE *fptr;
fptr = fopen("E:\\cprogram\\newprogram.txt","r");
char c;
while (fscanf (fptr, "%c", &c) != EOF) {
    printf ("prebran znak '%c'\n", c);
}
fclose(fptr)
```

Tle je se doug primer  

 ```c
 #include <stdio.h>
#include <stdlib.h>

int main (int argc, char *args[])
{
    if (argc != 3) exit (1);
    char *iname = "/neki/disk/fajl.txt";

    FILE *ifile;
    if ((ifile = fopen (iname, "r")) == NULL){
        exit (2);
    }

    // tle sta dve opciji

    /*
    int data;
    while ((data = fgetc (ifile)) != EOF) {
        printf ("prebran znak '%c' [%d]\n", data, data);
    }
    */

   // fscanf naceloma returna stevilo ustrezno prebranih bytov.
   // razn ce prides to konc fajla, takrat returna EOF
    char c;
    while (fscanf (ifile, "%c", &c) != EOF) {
        printf ("prebran znak '%c'\n", c);
    }

    // napišeš sam "fclose(ifile)", pa roka...nerabiš usega okrog
    if (fclose (ifile) != 0) exit (4);

    return 0;
}
