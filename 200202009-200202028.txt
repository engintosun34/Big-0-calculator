#include <stdio.h>
#include <stdlib.h>
#include <string.h> //0 n5 1 n4 2 n3 3 n2 4 n 5 logn 6 1
#include <time.h>
struct node
{
    char deger[250];
    struct node *next;

};
struct node *start = NULL;
struct node *tail = NULL;
int kac_tane(char *string,char *aranan)
{
    int count = 0;
    const char *tmp;
    tmp = string;
    while(tmp = strstr(tmp, aranan))
    {
        count++;
        tmp++;
    }
    return count;
}
int main()
{
    int array[7]= {0};
    int parantez=0;
    int virgul=0,kontrol=-1;
    char *p;
    int rec_bulundu_mu=0;
    FILE *dosya;
    if ((dosya = fopen("dosya.txt", "r")) == NULL)
        printf("Dosya okunamadi.");
    char satir[250];
    float sure=0;
    printf("Kod Parcasi:\n\n");
    clock_t basla=clock();
    system("gcc kodcalistir.c -o kodcalistir");
    clock_t bitis=clock();
    sure=(float)(bitis-basla)/CLOCKS_PER_SEC;
    printf("Calisma Suresi : %f\n\n",sure);

    while(fgets(satir,sizeof(satir),dosya))
    {
        if(start==NULL)
        {
            struct node *new_node=(struct node*)malloc(sizeof(struct node));
            strcpy(new_node->deger, satir);
            new_node->next = NULL;
            start = tail = new_node;
        }
        else
        {
            struct node *new_node=(struct node*)malloc(sizeof(struct node));
            strcpy(new_node->deger, satir);
            new_node->next = NULL;
            tail->next=new_node;
            tail=new_node;
        }
    }
    struct node *oku=start;
    struct node *test;
    while(oku!=NULL)
    {
        virgul=0;
        kontrol=-1;
        parantez=0;
        if(strstr(oku->deger,";")==NULL && strstr(oku->deger,"do{")==NULL &&strstr(oku->deger,"while")==NULL)
        {
            if(strstr(oku->deger,"main")==NULL && strstr(oku->deger,"(")!=NULL)
            {

                if(rec_bulundu_mu == 0)
                {
                    p=strstr(oku->deger," ");
                    p++;
                    strtok(p," ");
                    strtok(p,"(");
                    rec_bulundu_mu++;
                }
            }
            virgul=kac_tane(oku->deger,",");
            array[6]=array[6]+virgul+1;
        }
        else
        {
            if(strstr(oku->deger,"for")!=NULL)
            {
                kontrol=0;
            }
            else if(strstr(oku->deger,"while")!=NULL && strstr(oku->deger,"}")==NULL)
            {
                kontrol=1;
            }
            else if(strstr(oku->deger,"printf")!=NULL)
            {
                kontrol=2;
            }
            else if(strstr(oku->deger,"do{")!=NULL)
            {
                kontrol=3;
            }
            else if(strstr(oku->deger,p)!=NULL)
            {
                kontrol=4;
            }
        }
        if(kontrol==-1)
        {
            virgul=kac_tane(oku->deger,",");
            array[6]= array[6]+virgul+1;
        }
        else if(kontrol==0)
        {
            if(kac_tane(oku->deger,"+")>0 || kac_tane(oku->deger,"-")>0)
            {
                array[4]++;
            }
            else if(kac_tane(oku->deger,"*")>0 || kac_tane(oku->deger,"/")>0)
            {
                array[5]++;
            }
            else
            {
                array[6]++;
            }
        }
        else if(kontrol==1)
        {
            test=oku;
            while(test!=NULL)
            {
                if(strstr(test->deger,"{")!=NULL)
                {
                    parantez++;
                }
                else if(strstr(test->deger,"}")!=NULL)
                {
                    parantez--;
                }
                if(parantez==1 && strstr(test->next->deger,"}")!=NULL)
                {
                    if(kac_tane(test->deger,"+")>0 || kac_tane(test->deger,"-")>0)
                    {
                        array[4]++;
                    }
                    else if(kac_tane(test->deger,"*")>0 || kac_tane(test->deger,"/")>0)
                    {
                        array[5]++;
                    }
                    /*else
                    {
                        printf("elseden geldik");
                        array[4]++;
                    }*/
                }
                test=test->next;
            }

        }
        else if(kontrol==2)
        {
            array[6]++;
        }
        else if(kontrol==3)
        {
            test=oku;
            while(test!=NULL)
            {
                if(strstr(test->deger,"{")!=NULL)
                {
                    parantez++;
                }
                else if(strstr(test->deger,"}")!=NULL)
                {
                    parantez--;
                }
                if(parantez==1 && strstr(test->next->deger,"}")!=NULL && strstr(test->next->deger,"while")!=NULL)
                {
                    if(kac_tane(test->deger,"+")>0 || kac_tane(test->deger,"-")>0)
                    {
                        array[4]++;
                    }
                    else if(kac_tane(test->deger,"*")>0 || kac_tane(test->deger,"/")>0)
                    {
                        array[5]++;
                    }
                    /*else
                    {
                        array[4]++;
                    }*/
                }
                test=test->next;
            }
        }
        else if(kontrol==4)
        {
            if(strstr(oku->deger,"return"))
            {
                if(kac_tane(oku->deger,"+")>0 || kac_tane(oku->deger,"-")>0)
                {
                    if(strstr(oku->deger,"+")<strstr(oku->deger,")") && strstr(oku->deger,"+")>strstr(oku->deger,"("))
                    {
                        array[4]++;
                    }
                    if(strstr(oku->deger,"-")<strstr(oku->deger,")") && strstr(oku->deger,"-")>strstr(oku->deger,"("))
                    {
                        array[4]++;
                    }

                }
                if(kac_tane(oku->deger,"*")>0 || kac_tane(oku->deger,"/")>0)
                {
                    if(strstr(oku->deger,"*")<strstr(oku->deger,")") && strstr(oku->deger,"*")>strstr(oku->deger,"("))
                    {
                        array[5]++;
                    }
                    else if(strstr(oku->deger,"/")<strstr(oku->deger,")") && strstr(oku->deger,"/")>strstr(oku->deger,"("))
                    {
                        array[5]++;
                    }
                }
            }
        }
        printf("%s",oku->deger);
        oku=oku->next;
    }
    if(array[4]>0)
        printf("\nn sayisi:%d",array[4]);
    if(array[5]>0)
        printf("\nlogn sayisi:%d",array[5]);
    else if(array[4]==0 && array[5]==0)
        printf("\nBig(O)=1 --> Sabit");

    printf("\n\n");
    int yer_array[7]= {0};
    oku=start;
    int koseli_p=0;
    while(oku!=NULL)
    {
        virgul=0;
        koseli_p=0;
        if(strstr(oku->deger,";")!=NULL && strstr(oku->deger,"[")==NULL && strstr(oku->deger,"return")==NULL)
        {
            if(strstr(oku->deger,"printf")==NULL)
            {
                if(strstr(oku->deger,"int")!=NULL || strstr(oku->deger,"float")!=NULL || kac_tane(oku->deger,"long")==1 )
                {
                    virgul=kac_tane(oku->deger,",");
                    yer_array[6]=yer_array[6]+ 4*(virgul+1);
                }
                else if(strstr(oku->deger,"long long")!=NULL || strstr(oku->deger,"double")!=NULL)
                {
                    virgul=kac_tane(oku->deger,",");
                    yer_array[6]=yer_array[6]+ 8*(virgul+1);
                }
                else if(strstr(oku->deger,"char")!=NULL)
                {
                    virgul=kac_tane(oku->deger,",");
                    yer_array[6]=yer_array[6]+ (virgul+1);
                }
                else if(strstr(oku->deger,"short")!=NULL)
                {
                    virgul=kac_tane(oku->deger,",");
                    yer_array[6]=yer_array[6]+ 2*(virgul+1);
                }
            }

        }
        else if(strstr(oku->deger,"[")!=NULL)
        {
            if(strstr(oku->deger,"int")!=NULL || strstr(oku->deger,"float")!=NULL || kac_tane(oku->deger,"long")==1)
            {
                koseli_p=kac_tane(oku->deger,"[");
                if(koseli_p==1)
                    yer_array[4]+=4;

                else if(koseli_p==2)
                    yer_array[3]+=4;

                else if(koseli_p==3)
                    yer_array[2]+=4;

                else if(koseli_p==4)
                    yer_array[1]+=4;

                else if(koseli_p==5)
                    yer_array[0]+=4;
            }
            else if(strstr(oku->deger,"long long")!=NULL || strstr(oku->deger,"double")!=NULL)
            {
                koseli_p=kac_tane(oku->deger,"[");
                if(koseli_p==1)
                    yer_array[4]+=8;

                else if(koseli_p==2)
                    yer_array[3]+=8;

                else if(koseli_p==3)
                    yer_array[2]+=8;

                else if(koseli_p==4)
                    yer_array[1]+=8;

                else if(koseli_p==5)
                    yer_array[0]+=8;
            }
            else if(strstr(oku->deger,"char")!=NULL)
            {
                koseli_p=kac_tane(oku->deger,"[");
                if(koseli_p==1)
                    yer_array[4]+=1;

                else if(koseli_p==2)
                    yer_array[3]+=1;

                else if(koseli_p==3)
                    yer_array[2]+=1;

                else if(koseli_p==4)
                    yer_array[1]+=1;

                else if(koseli_p==5)
                    yer_array[0]+=1;
            }
            else if(strstr(oku->deger,"short")!=NULL)
            {
                koseli_p=kac_tane(oku->deger,"[");
                if(koseli_p==1)
                    yer_array[4]+=2;

                else if(koseli_p==2)
                    yer_array[3]+=2;

                else if(koseli_p==3)
                    yer_array[2]+=2;

                else if(koseli_p==4)
                    yer_array[1]+=2;

                else if(koseli_p==5)
                    yer_array[0]+=2;
            }
        }
        else if(strstr(oku->deger,"return")!=NULL)
        {
            if(strstr(oku->deger,p)!=NULL)
            {

                if(kac_tane(oku->deger,"+")>0 || kac_tane(oku->deger,"-")>0)
                {
                    if(strstr(oku->deger,"+")<strstr(oku->deger,")") && strstr(oku->deger,"+")>strstr(oku->deger,"("))
                    {
                        yer_array[4]++;
                    }
                    else if(strstr(oku->deger,"-")<strstr(oku->deger,")") && strstr(oku->deger,"-")>strstr(oku->deger,"("))
                    {
                        yer_array[4]++;
                    }

                }
                if(kac_tane(oku->deger,"*")>0 || kac_tane(oku->deger,"/")>0)
                {
                    if(strstr(oku->deger,"*")<strstr(oku->deger,")") && strstr(oku->deger,"*")>strstr(oku->deger,"("))
                    {
                        yer_array[5]++;
                    }
                    else if(strstr(oku->deger,"/")<strstr(oku->deger,")") && strstr(oku->deger,"/")>strstr(oku->deger,"("))
                    {
                        yer_array[5]++;
                    }
                }
            }
        }

        oku=oku->next;
    }
    if(yer_array[0]>0)
        printf("\n\n%dn5 + ",yer_array[0]);
    if(yer_array[1]>0)
        printf("%dn4 + ",yer_array[1]);
    if(yer_array[2]>0)
        printf("%dn3 + ",array[2]);
    if(yer_array[3]>0)
        printf("%dn2 + ",yer_array[3]);
    if(yer_array[4]>0)
        printf("%dn + ",yer_array[4]);
    if(yer_array[5]>0)
        printf("%dlogn + ",yer_array[5]);
    if(yer_array[6]>0)
        printf("%d",yer_array[6]);

    fclose(dosya);
    return 0;
}
