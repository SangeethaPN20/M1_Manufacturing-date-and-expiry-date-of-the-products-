#include <stdio.h>
#include <time.h>
#define MAX_YR  9999
#define MIN_YR  1900

typedef struct
{
    int yyyy;
    int mm;
    int dd;
} Date;

int  IsLeapYear(int year)
{
    return (((year % 4 == 0) &&
             (year % 100 != 0)) ||
            (year % 400 == 0));
}

int isValidDate(Date *validDate)
{
   
    if (validDate->yyyy > MAX_YR ||
            validDate->yyyy < MIN_YR)
        return 0;
    if (validDate->mm < 1 || validDate->mm > 12)
        return 0;
    if (validDate->dd < 1 || validDate->dd > 31)
        return 0;
    
    if (validDate->mm == 2)
    {
        if (IsLeapYear(validDate->yyyy))
            return (validDate->dd <= 29);
        else
            return (validDate->dd <= 28);
    }
    if (validDate->mm == 4 || validDate->mm == 6 ||
            validDate->mm == 9 || validDate->mm == 11)
        return (validDate->dd <= 30);
    return 1;
}

int enterExpiryDate(Date *getDate)
{
    printf("\n Enter year = ");
    scanf("%d",&getDate->yyyy);
    printf("\n\n Enter month = ");
    scanf("%d",&getDate->mm);
    printf("\n\n Enter day = ");
    scanf("%d",&getDate->dd);
    return isValidDate(getDate);
}
int checkExpiryDate(const Date* expiryDate, const Date * currentDate)
{
    if (NULL==expiryDate||NULL==currentDate)
    {
        return 0;
    }
    else
    {
        if (expiryDate->yyyy > currentDate->yyyy)
        {
            return 0;
        }
        else if (expiryDate->yyyy < currentDate->yyyy)
        {
            return 1;
        }
        else
        {
            if (expiryDate->mm > currentDate->mm)
            {
                return 0;
            }
            else if (expiryDate->mm < currentDate->mm)
            {
                return 1;
            }
            else
            {
                return (expiryDate->dd >= currentDate->dd)? 0 : 1;
            }
        }
    }
}


    

int main(void)
{
    struct date
{
    int day;
    int month;
    int year;
};
struct details
{
    char name[20];
    int price;
    int code;
    int qty;
    struct date mfg;
};
struct details item[50];
    int n, i;
    printf("Enter number of items:");
    scanf("%d", &n);
    fflush(stdin);
    for (i = 0; i < n; i++)
    {
        fflush(stdin);
        printf("Item name: \n");
        scanf("%s", item[i].name);
        fflush(stdin);
        printf("Item code: \n");
        scanf("%d", &item[i].code);
        fflush(stdin);
        printf("Quantity: \n");
        scanf("%d", &item[i].qty);
        fflush(stdin);
        printf("price: \n");
        scanf("%d",  &item[i].price);
        fflush(stdin);
        printf("Manufacturing date(dd-mm-yyyy): \n");
        scanf("%d-%d-%d", &item[i].mfg.day,
        &item[i].mfg.month, &item[i].mfg.year);
    }
    time_t rawtime;
    struct tm * timeinfo;
    
    Date expiryDate = {0};

    Date currentDate = {0};
    int status = 0;
    int button = 0;
    printf("\n\n Please enter product expiry date!\n");
    status = enterExpiryDate(&expiryDate);
    if(status !=1)
    {
        printf("\n\n Please enter a valid date!\n");
        return 0;
    }
    
    time(&rawtime);
    timeinfo = localtime(&rawtime);
    
    currentDate.yyyy = timeinfo->tm_year+1900;
    
    currentDate.mm = timeinfo->tm_mon+1;
    
    currentDate.dd = timeinfo->tm_mday;
    printf("*  INVENTORY * \n");
    printf("-----------------------------------------------------------------\n");
    printf("S.N.|    NAME           |   CODE   |  QUANTITY |  PRIC| MFG.DATE \n");
    printf("------------------------------------------------------------------\n");
    for (i = 0; i < n; i++)
        printf("%d%-15s%-d%-5d%-5d%d/%d/%d \n", i + 1, item[i].name, item[i].code, item[i].qty,item[i].price, item[i].mfg.day, item[i].mfg.month,
        item[i].mfg.year);
    printf("------------------------------------------------------------------\n");
    printf("\n\n Enter 5 to check product expiry date  = ");
    scanf("%d",&button);
    if((button != 5))
    {
        printf("\n\n You have prssed invalid button !!!\n\n");
        return 0;
    }
    
    status = checkExpiryDate(&expiryDate,&currentDate);
    if(status !=0)
    {
        printf("\n\n Product date has been expired !!!\n");
    }
    else
    {
        printf("\n\n You can use !!!\n");
    }
    return 0;
}
