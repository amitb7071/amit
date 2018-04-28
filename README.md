#include<iostream>
#include<stdio.h>
#include<queue>
using namespace std;
#define MAX 1000
#define TIME_QUANTAM 2 
int flag[MAX],arrival_time[MAX],burst_time[MAX],priority[MAX],Response_Time[MAX],ft[MAX],fe[MAX],fe_flag[MAX],process_id[MAX],tms,qt[MAX];
queue<int> q;  
void RoundRobin()
{
     if(!q.empty())
      {
        if(Response_Time[q.front()]>0 && qt[q.front()]<4)
        {
                qt[q.front()]++;
                Response_Time[q.front()]--;
                if(Response_Time[q.front()]==0)
                {
                ft[q.front()]=tms+1;
                q.pop();
                }
                if(Response_Time[q.front()]!=0 && qt[q.front()]==4)
                {
                qt[q.front()]=0;
                q.push(q.front());
                q.pop();
                }
            }
      }
}
 
int main()
{
    int i=0,n=0,smallest=0,last_smallest=-1,min,sum=0,large=0;
    printf("Enter the Number of Processes:\n");
    scanf("%d",&n);
    for(i=0;i<n;i++)
    {       printf("Enter the process id here:\n");
            scanf("%d",&process_id[i]);
            printf("Enter the arrival time of process_id %d :\n",process_id[i]);
            scanf("%d",&arrival_time[i]);
            printf("Enter the burst time of process_id %d :\n",process_id[i]);
            scanf("%d",&burst_time[i]);
            printf("Enter the priority time of process_id %d :\n",process_id[i]);
            scanf("%d",&priority[i]);
 
 
           
            if(arrival_time[i]>large)
                large=arrival_time[i];
              sum+=burst_time[i];
              Response_Time[i]=burst_time[i];
             
        printf("\n-----------------------------------------------------\n");
    }
    min=MAX;
    for(tms=0;!q.empty() || tms<=sum+large ;tms++)
    {
      min=MAX;
      smallest=-1;
      for(i=0;i<n;i++)
      {
        if(arrival_time[i]<=tms && priority[i]<min && Response_Time[i]>0 && !flag[i])
        {
            min=priority[i];
                smallest=i;
            }
      }
      if(smallest==-1 && !q.empty())
      {
        if(last_smallest !=-1 && Response_Time[last_smallest]==0)
        {
            ft[last_smallest]=tms;
                flag[last_smallest]=1;
            }
            last_smallest=-1;
            RoundRobin();
            continue;
      }
      else if(smallest!=-1 && !q.empty() && last_smallest==-1)
      {
        if(qt[q.front()]<=4 && qt[q.front()]>0)
        {
            q.push(q.front());
            q.pop();
            }
      }
      if(smallest!=-1 && !fe_flag[smallest])
      {
        fe[smallest]=tms-arrival_time[smallest];
        fe_flag[smallest]=1;
      }
      if( smallest!=last_smallest && last_smallest!=-1 && !flag[last_smallest])
      {
        q.push(last_smallest);
        flag[last_smallest]=1;
      }
      if(smallest !=-1)
        Response_Time[smallest]--;
     
      if((smallest !=-1) && ((Response_Time[smallest]==0) ||(burst_time[smallest]-Response_Time[smallest])==TIME_QUANTAM))
      {
        if((burst_time[smallest]-Response_Time[smallest])==TIME_QUANTAM && Response_Time[smallest]!=0)
        {
            flag[smallest]=1;
            q.push(smallest);
            }
        else if(smallest!=-1)
        {
                ft[smallest]=tms+1;
                flag[smallest]=1;
            }
      }
      last_smallest=smallest;
    }
   for(int i=0;i<n;i++)
   printf("\n The Process Id: %d | Waiting Time : %d | Finish Time : %d | TurnAround Time : %d\n",process_id[i],fe[i],ft[i],ft[i]-burst_time[i]-arrival_time[i]);
     
   
}
