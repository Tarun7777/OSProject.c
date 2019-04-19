#include<stdio.h>
#include<stdlib.h>
int timer4=0;
struct process
{
    int pid;         //process id
    float AT;        //arrival time
    float BT;        //burst time
    float WT;        // waiting time
    float priority;  //priority
    float TAT;       //turnaround time
    float CC;        //completion time
    int CC1;         //completion tiime for main process
    int WTIME;       //waiting time for main process
    int status;
/*Status=0 indicates all processes are ready to enter into ready queue.
  Status=1 indicates all processes are in ready queue.
  Status-2,3 indicates all processes are executed.*/
};

int main()
{
printf("\n ************************* SHORTEST JOB SCHEDULING ALLGORITHM PROJECT ***********************************");
printf("\n\n");
printf("\n                           Submitted to:Kamini Joshi\n");
printf("\n                           Submitted by :Tarun\n");
printf("\n                           REG NO:11702435\n");
printf("\n                           ROLL NO:A04\n");
    printf("\n\n\n Enter the number of processes you want to schedule :  ");//taking input from user for number of processes to schedule.
    int number;
    scanf("%d",&number);
    struct process proc[number];//Intializing Structure.
    printf("\n Enter the burst time of the processes \n");

    for(int i=1;i<=number;i++)//taking burst time for different processes.
    {
        printf("\n Enter the burst time for the %d process  :  ",i);
        scanf("%f",&proc[i].BT);
        proc[i].pid=i;
        proc[i].status=0;//Intially setting all processes status to zero.
    }

    printf("\n Enter the arival time of the processes \n");//taking arrival time for different processes.
    for(int i=1;i<=number;i++)
    {
        printf("\n Enter the arival time for the %d process  :  ",i);
        scanf("%f",&proc[i].AT);
     }
    printf("\n Process:     |   Burst Time:     |   Arival Time:     | ");//printing all processes along with process id, burst time and arrival time in console.
    for(int i=1;i<=number;i++)
    {
        printf("\n   %d               %d                   %d      ",proc[i].pid,(int )proc[i].BT,(int )proc[i].AT);
    }
    printf("\n");
    for(int i=1;i<=number;i++)//Intially set waiting time to zero for all processes.
    {
        proc[i].WT=0;
    }

    int max_at=0;
    for(int i=1;i<=number;i++)//find maximum arrival time in all processes.
    {
        if((int)proc[i].AT>max_at)
        {
            max_at=(int)proc[i].AT;
        }
    }

   int min_at=proc[1].AT;
    for(int i=1;i<=number;i++)//finding minimum arrival time in all processes.
    {
        if((int)proc[i].AT<min_at)
        {
            min_at=(int)proc[i].AT;
        }
    }
   printf("--------------------------------------------------------------------------:\n");

   int timer=0;
   int j=1;
   int seq[number];//declare an array for storing sequence of processes.
    seq[j]=0;//Intilize with zero.
    for (timer=0;timer<=max_at;timer++)
    {
        for(int i=1;i<=number;i++)
        {
            if(timer>=(int)proc[i].AT)
            {
                if(proc[i].status==0)
                {
                    seq[j]=proc[i].pid;//store all processes in a sequences.
		    proc[i].status=1;//setting all processes status to 1 when they enter into array(ready queue).
		    printf("%d \n",seq[j]);
		    j++;
                }
           }
        }
    }

    printf("\n\n\n");

    int final_sequence1[number];
    final_sequence1[0]=0;
    int n;
    n=number;
    int order[n][n];

    for(int i=1;i<=n;i++)
    {
        order[0][i]=seq[i];//store all sequences in two dimensional array for better understanding and also avoiding confusion.
    }
    printf("-------------------------------------------------------------------------\n");
int timer1;//It is used for calculating completion time for all processes. 
timer1=min_at;//It is intialized with minimum arrival time because we don't know arrival time always starts from 0 or 1 or ....
    for(int j=1;j<=n;j++)
     {
       final_sequence1[j]=order[0][j]; //based on sequence processes are executed.
       printf("Final sequence1 is of:P%d \n",final_sequence1[j]);
       timer1=timer1+proc[order[0][j]].BT;
       proc[order[0][j]].CC=(float)timer1;//It stores completion time for all processes.
       printf("Complete time is: %f \n",proc[order[0][j]].CC);
	 }

printf("\n\n\n");
int i=1;
proc[order[0][i]].status=2;//Intialize first executing process status to 2 .It indiacates process is executed.
           for(int j=1;j<=number;j++)
            {
                if(proc[order[0][j]].status==1)
                {
                     if(proc[order[0][j]].WT==0)
                    {
                         proc[order[0][j]].WT=proc[order[0][i]].CC - proc[order[0][j]].AT;//It is used to calculte waiting time for all processes.
			 i=j;
                    }
                    else
                    {
                    proc[order[0][j]].WT=proc[order[0][j]].WT+proc[order[0][i]].BT;
                    }
                }
             }
int k;
           for(k=1;k<=number;k++)
           {
                proc[order[0][k]].priority=1+((proc[order[0][k]].WT)/(proc[order[0][k]].BT)) ;//calculating priority for all processes based on sequence.
		printf("The priority for each process  P%d is:  %f \n",order[0][k],proc[order[0][k]].priority);
	    }
   float max_pt=proc[1].priority;
    for(int i=1;i<=number;i++)//calculating maximum priority in all processes.
    {
        if((float)proc[i].priority>max_pt)
        {
            max_pt=(float)proc[i].priority;
        }
    }
int timer2=0;
int m=1;
int seq2[number];
seq2[m]=0;
    for (timer2=0;timer2<=max_pt;timer2++)//priorities are arranged in ascending order.
    {
        for(int i=1;i<=number;i++)
        {
            if(timer2>=(float)proc[i].priority)
            {
                if(proc[i].status==1||proc[i].status==2)
                {
                    seq2[m]=proc[i].pid;
                    proc[i].status=3;
                    printf("%d \n",seq2[m]);
                    m++;
                }
           }
        }
    }
    int order3[n][n];
    order[0][0];
    int n1;
     n1=number;
    int s=1;
    for(int j=n1;j>=1;j--)//priorities are arranged in descending order.
    {
        order3[0][s]=seq[j];
        printf("Final priority order of sequence is:%d \n",order3[0][s]);
	s++;
    }
    printf("----------------------------------------------------------------------------\n");
    printf("\n\n\n");
int final_sequence[number];
final_sequence[0]=0;
    for(int j=1;j<=1;j++)//processing all processes based on priority .
     {
       final_sequence[j]=order3[0][j];
       timer4=proc[order3[0][j]].AT+proc[order3[0][j]].BT;
       proc[order3[0][j]].CC1=(float)timer4;
       printf("Complete time is: %d \n",proc[order3[0][j]].CC1);
         }
     for(int j=2;j<=n;j++)
     {
       final_sequence[j]=order3[0][j];
       timer4=timer4+proc[order3[0][j]].BT;
       proc[order3[0][j]].CC1=(float)timer4;
       printf("Complete time is: %d \n",proc[order3[0][j]].CC1);
      }
  for(int j=1;j<=number;j++)//calculaitn Turn around time for all processes.
  {
   proc[order3[0][j]].TAT=proc[order3[0][j]].CC1-proc[order3[0][j]].AT;
   }
for(int j=1;j<=number;j++)//calculaitng total waiting time.
  {
   proc[order3[0][j]].WTIME=proc[order3[0][j]].TAT-proc[order3[0][j]].BT;
   }

printf("\n\n\n");
    int total_at=0;
    for(int i=1;i<=number;i++)//calculaitng total arrival time.
    {
        total_at=total_at+proc[i].AT;
    }
    printf("Total arrival time is %d \n",total_at);

    printf("\n The final scheduled order in shortest job first are as follow \n\n");
    printf("\n\n");

    printf(" Process      :  ");
    for(int i=1;i<=number;i++)
    {
        printf("    P%d   ",final_sequence[i]);
    }
    printf("\n\n");

    printf(" Arrival time :   ");
    for(int i=1;i<=number;i++)
    {
        printf("    %d    ",(int)proc[final_sequence[i]].AT);
    }
    printf("\n\n");
    printf(" Complete time :   ");
    for(int i=1;i<=number;i++)
    {
        printf("   %d    ",(int)proc[final_sequence[i]].CC1);
    }
    printf("\n\n");

    printf(" Waiting time:    ");
    for(int i=1;i<=number;i++)
    {
        printf("    %d    ",(int)proc[final_sequence[i]].WTIME);
    }
    printf("\n\n");
    printf(" Turnaround time: ");
    for(int i=1;i<=n;i++)
    {
        printf("    %d    ",(int)proc[final_sequence[i]].TAT);
    }
    printf("\n\n");
float total_wt;
total_wt=0;
    for(int i=1;i<=n;i++)//calculaitng total waiting time.
    {
     total_wt=total_wt+proc[final_sequence[i]].WTIME;
     }
     printf("Total waiting time is:%f\n",total_wt);

    printf("\n\n");
    float avg_wt;
    avg_wt=0;
    avg_wt=(float)total_wt/n;//calculating Average waiting time.
    printf("AVERAGE WAITING TIME  : %f \n\n",avg_wt);
    printf("\n");
}
