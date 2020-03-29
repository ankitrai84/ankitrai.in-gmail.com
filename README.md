#include<stdio.h> 
#include<conio.h>

void rr(int no,int remaining_time[10],int Current_Time,int arrival_Time[10], int Burst_Time[10]);

main() 
{
	int Proce_no,j,no,Current_Time,RemaProc,indicator,time_quan,wait,turn_Around_Time ,arrival_Time[10],Burst_time[10],remaining_time[10],x=1;
	indicator = 0;
	wait = 0;
	turn_Around_Time = 0;
	printf("Enter number of processes "); 
	scanf("%d",&no);
	RemaProc = no;
	
	printf("\nEnter the burst time and arrival time of the processes\n");
	for(Proce_no = 0;Proce_no < no;Proce_no++) 
	{
		printf("\nProcess P%d\n",Proce_no+1);
		printf("Enter the Arrival time = "); 
		scanf("%d",&arrival_Time[Proce_no]);
		printf("Enter the Burst time = "); 
		scanf("%d",&Burst_time[Proce_no]); 
		remaining_time[Proce_no]=Burst_time[Proce_no]; 
	} 
	printf("The details of time quantum are as follows:\n");
	printf("time quantum for first round is 3.\n"); 
	time_quan=3;
	Current_Time=0;
	for(Proce_no=0;RemaProc!=0;) 
	{
		if(remaining_time[Proce_no]<=time_quan && remaining_time[Proce_no]>0)
		{ 
			Current_Time+=remaining_time[Proce_no]; 
			remaining_time[Proce_no]=0; 
			indicator=1; 
		} 
		else if(remaining_time[Proce_no]>0)
		{ 
			remaining_time[Proce_no]-=time_quan; 
			Current_Time+=time_quan; 
		} 
		if(remaining_time[Proce_no]==0 && indicator==1)			
		{ printf("%d",Proce_no);
			RemaProc--;				
			printf("P %d",Proce_no+1); 
			printf("\t\t\t%d",Current_Time-arrival_Time[Proce_no]);
			printf("\t\t\t%d\n",Current_Time-Burst_time[Proce_no]-arrival_Time[Proce_no]);
			wait+=Current_Time-arrival_Time[Proce_no]-Burst_time[Proce_no]; 
			turn_Around_Time+=Current_Time-arrival_Time[Proce_no]; 
			indicator=0; 
                       
		} 
		if(Proce_no==no-1){
			x++;
			if(x==2){
				Proce_no=0;
				time_quan=6;
				
				printf("time quantum for second round is 6. \n");
			}
			else{
				break;
			}
		}
		else if(Current_Time >= arrival_Time[Proce_no+1]){
			Proce_no++;
		}
		else{
			Proce_no=0;
		}
	}
	
	rr(no,remaining_time,Current_Time,arrival_Time,Burst_time);
	
	return 0;
}


void rr(int no,int remaining_time[10],int Current_Time,int arrival_Time[10], int Burst_time[10]){
	
	float avg_wait,avg_tut;
    int i,j,n=no,temp,bu_time[20],Proce_no[20],wa_time[20],tut_t[20],total=0,loc;
    
    printf("Third round with least burst time.\n");
    
    for(i=0;i<n;i++)
    {
        bu_time[i]=remaining_time[i];
        wa_time[i]=Current_Time-arrival_Time[i]-bu_time[i];
		Proce_no[i]=i+1;
    }
	
    for(i=0;i<n;i++)
    {
        loc=i;
        for(j=i+1;j<n;j++)
        {
            if(bu_time[j]<bu_time[loc]){
            	loc=j;
            }
        }
        temp=bu_time[i];
        bu_time[i]=bu_time[loc];
        bu_time[loc]=temp;
        temp=Proce_no[i];
        Proce_no[i]=Proce_no[loc];
        Proce_no[loc]=temp;
    }
	
    for(i=1;i<n;i++)
    {
        for(j=0;j<i;j++){
        	wa_time[i]+=bu_time[j];
        }
        total+=wa_time[i];
    }
 
    avg_wait=(float)total/n;
    total=0;
    printf("\nProcess\t\tBurst time\t\twaiting time\t\tTurnaround Time");
    for(i=0;i<n;i++)
    {
        tut_t[i]=bu_time[i]+wa_time[i];
        total=total + tut_t[i];
        printf("\nP%d\t\t\t%d\t\t\t%d\t\t\t%d",Proce_no[i],bu_time[i],wa_time[i],tut_t[i]);
    }
    avg_tut=(float)total/n;
    printf("\n Average waiting time = %f",avg_wait);
    printf("\n Average turnaround time = %f\n",avg_tut);
	
}


