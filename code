#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#define MAX 100
struct movie{
    int mid;
    char mname[40];
    int total;
    char times[10];
    int available;
    float price;
};
struct user{
    char username[30];
    char password[20];
};
struct ticket{
    char username[30];
    int mid;
    int seats;
};

void firstmain();
void displayforuser();
int read_use(struct user[]);
void write_use(struct user[],int);
int login(struct user[],int,char[],char[]);
void signin(struct user[],int *);
void book(struct movie[],int,struct ticket[],int *,int );
void cancel(struct movie[],int,struct ticket[],int *,int);
void write_mov(struct movie[],int);
int read_mov(struct movie[],int);

int main() {
    struct user u[MAX];
    int usenum=read_use(u);
    struct movie m[5]={
        {1,"Interstellar",100,"8:00 am",100,500},
        {2,"Inside Out",80,"1:00 pm",80,350},
        {3,"Animal",90,"3:00 pm",90,350},
        {4,"Jatra",60,"10:00 am",60,350},
        {5,"Zootopia",70,"9:00 pm",70,450},
    };
    int movnum=5;
    read_mov(m,movnum);
    struct ticket t[400];
    int ticknum=0;
    int loggedinid=-1;
    while(1){
        if(loggedinid==-1){
            firstmain();
            int n;
            scanf("%d",&n);
            if(n==1){
                signin(u,&usenum);
            } 
            else if(n==2){
                char username[30];
                char password[20];
                printf("Enter your username: ");
                scanf("%s",username);
                printf("Enter your password: ");
                scanf("%s",password);
                loggedinid=login(u,usenum,username,password);
                if(loggedinid==-1){
                    printf("\nUNSUCCESSFUL LOGIN!!!\nYou have to check username and password!!!\n");
                }
                else{
                    printf("Successful login!!! Hello, %s!\n",username);
                }
            } 
            else if(n==3){
                printf("Program exit\n");
                write_mov(m,movnum);
                break;
            } 
            else{
                printf("Only choose from 1-3.\n");
            }
        } 
        else{
            displayforuser();
            int ch;
            scanf("%d",&ch);
            switch(ch){
                case 1:
                    book(m,movnum,t,&ticknum,loggedinid);
                    write_mov(m,movnum);
                    break;
                case 2:
                    cancel(m,movnum,t,&ticknum,loggedinid);
                    write_mov(m,movnum);
                    break;
                case 3:
                    printf("Now we have been log out.\n");
                    loggedinid=-1;
                    write_mov(m,movnum);
                    break;
                default:
                    printf("Only choose from 1-3.\n");
            }
        }
    }
    return 0;
}
void firstmain(){
    printf("\n========================== MOVIE TICKET BOOKING SYSTEM ===============================\n");
    printf("1. Sign in\n");
    printf("2. Login\n");
    printf("3. Exit\n");
    printf("=======================================================================================\n");
    printf("\nEnter your choice: ");
}
void displayforuser(){
    printf("\n\n________________________________________________________________\n");
    printf("\n1. Book Movie Ticket\n");
    printf("2. Cancel Movie Ticket\n");
    printf("3. Logout\n");
    printf("____________________________________________________________________\n");
    printf("\nEnter your choice: ");
}

void write_mov(struct movie m[],int movnum){
    FILE *fg=fopen("movieees.txt","w");
    int p;
    for(p = 0;p<movnum;p++){
        fprintf(fg,"%d %d\n",m[p].mid,m[p].available);
    }
    fclose(fg);
}
int read_mov(struct movie m[],int movnum){
    FILE *fg=fopen("movieees.txt","r");
    int p;
    if(fg!=NULL){
        for(p=0;p<movnum;p++){
            fscanf(fg,"%d %d",&m[p].mid,&m[p].available);
        }
    } 
    else{
        return 0;
    }
    fclose(fg);
    return 1;
}
void write_use(struct user u[],int usenum){
    int i;
    FILE *fp=fopen("users.txt","w");
    for(i=0;i<usenum;i++){
        fprintf(fp,"%s %s\n",u[i].username,u[i].password);
    }
    fclose(fp);
}
void signin(struct user u[],int *usenum){
    char username[30];
    char password[20];
    printf("Enter your username: ");
    scanf("%s",username);
    printf("Enter the password: ");
    scanf("%s",password);
    int i;
    for(i=0;i< *usenum;i++){
        if(strcmp(u[i].username,username)==0){
            printf("Username already in existence, please use another one!!!\n");
        }
    }
    strcpy(u[*usenum].username,username);
    strcpy(u[*usenum].password,password);
    (*usenum)=(*usenum)+1;
    write_use(u,*usenum);
    printf("\n~~SUCCESSFUL REGISTRATION!!!~~ Login can be done!!\n");
}
int read_use(struct user u[]){
    FILE *fp=fopen("users.txt","r");
    if (fp==NULL){
        return 0;
    }
    int i=0;
    while(fscanf(fp,"%s %s",u[i].username,u[i].password)!=EOF){
        i++;
    }
    fclose(fp);
    return i;
}
int login(struct user u[],int usenum,char username[],char password[]){
    int i;
    for(i = 0;i<usenum;i++){
        if(strcmp(u[i].username,username)==0 && strcmp(u[i].password,password)==0){
            return i;
        }
    }
    return -1;
}

void book(struct movie m[],int movnum,struct ticket t[],int *ticknum,int uid){
    read_mov(m,movnum);
    printf("\n%-10s%-20s%-23s%-15s%-28s%-10s\n","ID","Movie Name","Total seat","Time","Available seats","Price (Rs)");
    printf("-------------------------------------------------------------------------------------------------------------\n");
    int q;
    for(q=0;q<movnum;q++){
        printf("%-10d%-20s%-23d%-15s%-28d%-10g\n",m[q].mid,m[q].mname,m[q].total,m[q].times,m[q].available,m[q].price);
    }
    printf("\nEnter Movie ID: ");
    int mid;
    scanf("%d",&mid);
    int mindex=-1;
    for(int i=0;i<movnum;i++){
        if(m[i].mid==mid){
            mindex=i;
            break;
        }
    }
    if(mindex==-1){
        printf("Movie not found!!! \n");
    }
    else if(m[mindex].available==0){
        printf("Seats are fully booked, SORRY!!\n");
    } 
    else{
        char username[30];
        printf("Enter your username: ");
        scanf("%s",username);
        printf("Enter number of seats to book: ");
        int seats;
        scanf("%d",&seats);
        if(seats>m[mindex].available){
            printf("Not enough available seats! Available seats: %d\n",m[mindex].available);
        } 
        else{
            for(int i=0;i<seats;i++){
                strcpy(t[*ticknum].username,username);
                t[*ticknum].seats=seats;
                t[*ticknum].mid=mid;
                (*ticknum)++;
                m[mindex].available--;
            }
            printf("SUCCESSFUL Ticket Booking!!! You have booked %d seats.\n",seats);
            printf("\n---------|TICKET|----------\n");
            printf("Username     : %s\n",username);
            printf("Movie        : %s\n",m[mid - 1].mname);
            printf("No. of seats : %d\n",seats);
            printf("Price.       : %.2f\n",m[mid - 1].price*seats);
            printf("-----------------------------\n");
        }
    }
}
void cancel(struct movie m[],int movnum,struct ticket t[],int *ticknum,int uid){
    printf("%-10s%-20s%-23s%-15s%-28s%-10s\n","ID","Movie Name","Total seat","Time","Available seats","Price (Rs)");
    printf("-----------------------------------------------------------------------------------------------------------\n");
    int q;
    for(q=0;q<movnum;q++){
        printf("%-10d%-20s%-23d%-15s%-28d%-10g\n",m[q].mid,m[q].mname,m[q].total,m[q].times,m[q].available,m[q].price);
    }
    char username[30];
    int mid;
    printf("\nEnter your username: ");
    scanf("%s",username);
    printf("Enter the movie ID: ");
    scanf("%d",&mid);
    printf("Enter number of tickets to cancel: ");
    int canctick;
    scanf("%d",&canctick);
    int f = 0;
    int i,cancell=0;
    for(i=0;i< *ticknum && canctick>0;i++){
        if(strcmp(t[i].username,username)==0 && t[i].mid==mid){
            if(t[i].seats>=canctick){
            int mindex=-1;
            for(int j= 0;j<movnum;j++){
                if(m[j].mid==mid){
                    mindex=j;
                    break;
                }
            }
            m[mindex].available++;
            for(int j=i;j<(*ticknum)-1;j++){
                t[j]=t[j+1];
            }
            (*ticknum)--;
            canctick--;
            f=1;
            cancell++;
            i--;
        }
        }
    }
    if(f){
        printf("Cancellation success!!! You have cancelled %d tickets.\n",cancell);
    } 
    else{
        printf("%s name is not here for this movie or not enough tickets to cancel.\n",username);
    }
}
