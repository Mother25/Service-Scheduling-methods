#include <stdio.h>
#include <stdlib.h>

int input();
void output();

int main()
{
    float result;
    int choice, num;
    printf("CPU Scheduler Simulator\n");
    printf("1) scheduling Method (None)\n");
    printf("2) Preemptive mode (Off)\n");
    printf("3)Show Results\n");
    printf("4)End Program\n");
    printf("Option>\n");

    choice = input();  

    switch (choice) {
    case 1: {
    int number_processes,burst_time[20],waiting_time[20],tat[20],average_waiting_time=0,average_piority=0,i,j; 

    FILE*pfile=NULL;
    printf("No of processes:");
    scanf("%d",&number_processes);
    printf("\nEnter Process Burst Time\n");

    for(i=0;i<number_processes;i++)
    {
        printf("P[%d]:",i+1);
        scanf("%d",&burst_time[i]);
    }
    waiting_time[0]=0;
    for(i=1;i<number_processes;i++)
    {
        waiting_time[i]=0;
        for(j=0;j<i;j++)
            waiting_time[i]+=burst_time[j];
    }
    printf("\nProcess\t\tBurst Time\tWaiting Time\tpiority Time");

    for(i=0;i<number_processes;i++)
    {
        tat[i]=burst_time[i]+waiting_time[i];
        average_waiting_time+=waiting_time[i];
        average_piority+=tat[i];
        printf("\nP[%d]\t\t%d\t\t%d\t\t%d",i+1,burst_time[i],waiting_time[i],tat[i]);
    }

    average_waiting_time/=i;
    average_piority/=i; 
    pfile = fopen("output.txt", "w");
    fprintf(pfile,"\n\nAverage Waiting Time:%d",average_waiting_time);
    fprintf(pfile,"\nAverage piority:%d",average_piority);
    return 0;
    }

    case 2: {
      FILE*pfile=NULL;
      int burst_time[20],p[20],waiting_time[20],piority[20],pr[20],i,j,n,total=0,pos,temp,average_waiting_time,average_piority;
      printf("No of processes:");
      scanf("%d",&n);
      printf("\nEnter Burst Time and Priority for preemptive scheduling\n");

    for(i=0;i<n;i++)
    {
        printf("\nP[%d]\n",i+1);
        printf("Burst Time:");
        scanf("%d",&burst_time[i]);
        printf("Priority:");
        scanf("%d",&pr[i]);
        p[i]=i+1;           
    }

    for(i=0;i<n;i++)
    {
        pos=i;
        for(j=i+1;j<n;j++)
        {
            if(pr[j]<pr[pos])
                pos=j;
        }

        temp=pr[i];
        pr[i]=pr[pos];
        pr[pos]=temp;

        temp=burst_time[i];
        burst_time[i]=burst_time[pos];
        burst_time[pos]=temp;
        temp=p[i];
        p[i]=p[pos];
        p[pos]=temp;
    }

    waiting_time[0]=0;
    for(i=1;i<n;i++)
    {
        waiting_time[i]=0;
        for(j=0;j<i;j++)
            waiting_time[i]+=burst_time[j];

        total+=waiting_time[i];
    }


    average_waiting_time=total/n;      //average waiting time
    total=0;

    printf("\nProcess\t    Burst Time    \tWaiting Time\t Piority Time");
    for(i=0;i<n;i++)
    {
        piority[i] = burst_time[i] + waiting_time[i];     //calculate turnaround time
        total += piority[i];
        printf("\nP[%d]\t\t  %d\t\t    %d\t\t\t%d",p[i],burst_time[i],waiting_time[i],piority[i]);
    }
    pfile = fopen("output.txt", "w");
    average_piority=total/n;     //average turnaround time
    fprintf(pfile, "\n\nAverage Waitng Time:%d",average_waiting_time);
    fprintf(pfile,"\nAverage Turnaround Time:%d",average_piority);



   

return 0;
    }
    case 3: {

    char *filename = "output.txt";
    FILE *fp = fopen(filename, "r");

    if (fp == NULL)
    {
        printf("Error: could not open file %s", filename);
        return 1;
    }

    // reading line by line, max 256 bytes
    const unsigned MAX_LENGTH = 256;
    char buffer[MAX_LENGTH];

    while (fgets(buffer, MAX_LENGTH, fp))
        printf("%s", buffer);

    // close the file
    fclose(fp);

    return 0;
    }
      case 4: {
// EXIT_SUCCESS
    exit(0);
    }
    default:
        printf("wrong Input\n");
    }
    return 0;
}
int input()
{
    int number;
    scanf("%d", &number);
    return (number);
}

void output(int number)
{
    printf("%d", number);
}
