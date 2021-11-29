# Timing-Wheel
#include<stdio.h>
#include<stdlib.h>
#define NUM 10
typedef struct PCB
{
	 int name;//进程id
	 int runtime;//剩余需要执行的时间，可随机分配
     int runedtime;//已经执行的时间
	 int state;//执行状态,0表示没完成，1表示完成
struct PCB *next;
}PCB;
int main()
{
	int timeslice=3;
	PCB *runqueue,*p;
	PCB *top,*tail,*temp;//队列指针
	int i;
	int count=10;
	top = tail= (struct PCB*)malloc(sizeof(struct PCB));
	top ->next=NULL;
	runqueue=top;
	printf("********进程初始状态******\n");
	for(i=0;i<NUM;i++)
	{
    	runqueue ->next= (struct PCB*)malloc(sizeof(struct PCB));
    	runqueue = runqueue->next;
    	tail=runqueue;
    	runqueue ->name=i+1;
    	runqueue ->runtime=rand ()%21;
    	runqueue ->runedtime=0;
    	runqueue ->state=0;
    	printf("process name=%d, runtime=%d, runedtime=%d\n"
		,tail->name,tail->runtime,tail->runedtime);
	}
	printf("********进程完后过程******\n");
	tail ->next=runqueue->next=NULL;
	p=top ->next;
	while(1)
    {
        if(top->next==NULL)
        {
            printf("就绪队列进程完成\n");
            break;
        }
        else
        {
            while(p)
            {
                if(p->runtime >0)
                {
                    p->runtime -=timeslice;
                    if(p->runtime<0)
                    {
                        p->runtime=0;
                    }
                    p->runedtime +=timeslice;
                    printf("process name=%d, runtime=%d, runedtime=%d\n"
                        ,p->name,p->runtime,p->runedtime);
                }
                temp =p;
                if(temp ->runtime>0)
                {
                    if (count>1)
                    {
                        top ->next=temp ->next;
                        tail ->next=temp;
                        tail=tail->next;
                        tail->next=NULL;
                    }
                    else if(count=1)
                    {
                        top->next=temp;
                        tail=temp;
                        tail->next=NULL;
                    }
                }
                if(temp->runtime==0)
                {
                    count --;
                    top ->next=temp->next;
                    free(temp);
                }
                p=top ->next;
            }
        }
    }
}














