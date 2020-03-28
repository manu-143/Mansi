#include<pthread.h>
	#include <stdio.h>
	#include<stdlib.h>
	#include<sys/types.h>
	#include<unistd.h>
	

	

	pthread_cond_t cond1=PTHREAD_COND_INITIALIZER;
	pthread_mutex_t mutex;
	int count = 5;
	int num_reader = 0;
	

	void *reader(void *read_n)
	{
	sleep(5);
	
	pthread_mutex_lock(&mutex);                               //O(1)
	
	if(num_reader == -1)                                      //O(1)
 {
	pthread_cond_wait(&cond1,&mutex);                         //O(1)
	}
	num_reader++;
	pthread_mutex_unlock(&mutex);                            //O(1)
	printf("Reader %d: read count is %d\n",(*((int *)read_n)),count); 
	pthread_mutex_lock(&mutex);                               //O(1)
	num_reader--;                                             //O(1)
	if(num_reader == 0)                                       //O(1)
 {
	pthread_cond_signal(&cond1);                               //O(1)
	}
	pthread_mutex_unlock(&mutex);                               //O(1)
	}
	

	void *writer(void *write_n)
	{
	int m=rand()%30;
	sleep(m);
	count = count*2;                                            //O(1)
	printf("Writer changes: %d count to %d\n",(*((int      *)write_n)),count);                                         //O(1)
	pthread_mutex_unlock(&mutex);                               //O(1)
	

	}
	

	int main()
	{
	int num_reader,num_writer;                           //O(1)
	printf("Enter number of reader between 1 and 10\n");//O(1)
	scanf("%d",&num_reader);                            //O(1)
	printf("Enter number of writer between 1 and 10\n");//O(1)
	scanf("%d",&num_writer);                            //O(1)
	

	

	pthread_t read[num_reader],write[num_writer];        //O(1)
	pthread_mutex_init(&mutex, NULL);                    //O(1)
	

	int arr[10] = {1,2,3,4,5,6,7,8,9,10};
	

	for(int i = 0; i < num_writer; i++)           //O(num_writer)
 {
	pthread_create(&write[i], NULL, (void *)writer,(void *)&arr[i]);
	}
	
	for(int i = 0; i < num_reader; i++)           //O(num_reader)
{
	pthread_create(&read[i], NULL, (void *)reader,(void *)&arr[i]);
	}
	
	
	for(int i = 0; i < num_reader; i++)           //O(num_reader)
 {
	pthread_join(read[i], NULL);
	}
	for(int i = 0; i < num_writer; i++)          //O(num_writer)
	{
	pthread_join(write[i], NULL);
	}
	

	pthread_mutex_destroy(&mutex);
	pthread_cond_destroy(&cond1);
	

	return 0;
}
	
	}


