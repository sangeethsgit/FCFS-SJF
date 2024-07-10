#include <stdio.h>

#include <stdlib.h>

#include <limits.h>

typedef struct node

{

	int pid, at, bt, ct, tat, wt;

} process;

process prc[100];

int done(int);

int fcfsselect(int, int);

int sjfselect(int, int);

int fcfs(int);

int sjf(int);

void calcdetails(int, int);

void display(int);

int main()

{

	int i, n, option, time;

	printf("Enter the number of processes- ");

	scanf("%d",&n);

	for(i=0; i<n; ++i)

	{

		printf("Enter the PID, AT and BT- ");

		scanf("%d%d%d",&prc[i].pid,&prc[i].at,&prc[i].bt);

		prc[i].ct=0;

	}

	while(1)

	{

		printf("Menu\n");

		printf("1. FCFS\n");

		printf("2. SJF\n");

		printf("3. Exit\n");

		printf("Enter your option- ");

		scanf("%d",&option);

		switch(option)

		{

			case 1:	time=fcfs(n);

				display(n);

				calcdetails(n, time);

				break;

			case 2: time=sjf(n);

				display(n);

				calcdetails(n, time);

				break;

			case 3: exit(0);

			default: printf("Wrong option\n");

		}

	}

		

}



int done(int n)

{

	int i, flag=0;

	for(i=0; i<n; i++)

	{

		if(prc[i].ct==0)

		{

			flag=1;

			break;

		}

	}

	return flag;

}



int fcfsselect(int n, int time)

{

	int i, min=INT_MAX, cpid=-1;

	for(i=0; i<n; i++)

	{

		if((prc[i].at<min)&&(prc[i].ct==0)&&(prc[i].at<=time))

		{

			min=prc[i].at;

			cpid=prc[i].pid;

		}

	}

	return cpid;

}



int fcfs(int n)

{

	int i, time=0, cpid;

	while(done(n)==1)

	{

		cpid=fcfsselect(n, time);

		if(cpid!=-1)

		{

			for(i=0; i<n; i++)

			{

				if(prc[i].pid==cpid)

				{

					time+=prc[i].bt;

					prc[i].ct=time;

					prc[i].tat=prc[i].ct-prc[i].at;

					prc[i].wt=prc[i].tat-prc[i].bt;

					break;

				}

			}

		}

		else	time++;

	}

	return time;

}



int sjfselect(int n, int time)

{

	int i, min=INT_MAX, cpid=-1;

	for(i=0; i<n; i++)

	{

		if((prc[i].bt<min)&&(prc[i].ct==0)&&(prc[i].at<=time))

		{

			min=prc[i].bt;

			cpid=prc[i].pid;

		}

	}

	return cpid;

}



int sjf(int n)

{

	int i, time=0, cpid;

	while(done(n)==1)

	{

		cpid=sjfselect(n, time);

		if(cpid!=-1)

		{

			for(i=0; i<n; i++)

			{

				if(prc[i].pid==cpid)

				{

					time+=prc[i].bt;

					prc[i].ct=time;

					prc[i].tat=prc[i].ct-prc[i].at;

					prc[i].wt=prc[i].tat-prc[i].bt;

					break;

				}

			}

		}

		else	time++;

	}

	return time;

}



void display(int n)

{

	int i;

	printf("PID\tAT\tBT\tCT\tTAT\tWT\n");

	for(i=0; i<n; i++)

	{

		printf("%d\t%d\t%d\t%d\t%d\t%d\n", prc[i].pid, prc[i].at, prc[i].bt, prc[i].ct, prc[i].tat, prc[i].wt);

	}

}



void calcdetails(int n, int time)

{

	int i;

	float avgtat=0, avgwt=0, tp;

	for(i=0; i<n; i++)

	{

		avgtat+=prc[i].tat;

		avgwt+=prc[i].wt;

	}

	avgtat/=n;

	avgwt/=n;

	tp=(float)time/n;

	printf("Average TAT=%f\n",avgtat);

	printf("Average WT=%f\n",avgwt);

	printf("Throughput=%f\n",tp);

}





		
