# covid19
#include<stdio.h>
#include<stdlib.h>
#include<time.h>

// defining struct for priority queue node
struct priority_q_node
{
	// lower time indicates higher priority
	int time;
	// event type = 1 represents transmisiion
	// event type = 0 represents recovery
	int event_type;
	struct vertex* node;
	struct priority_q_node* next;

};

// defining struct for priority queue
struct priority_queue
{
	// head of the priority queue
	struct priority_q_node* head;
};

// defining struct for all the linked edges of the particular vertex in graph
struct linked_vertices
{
	// all the edges of a particular vertex are stored in linked list
	// person number and the the next number address are stored
	int id_number;
	struct linked_vertices* link;
};

// defining struct for each vertex (person) in graph
struct vertex
{
	// person number and his status ( susceptible or infected or recovered ) are stored
	int vertex_id;
	char status;
	// head address of the vertex edges is stored
	struct linked_vertices* head_link;
};

// defining struct node for linked list
struct node
{
	int id_number;
	int time;
	struct node* next;
};

// defining struct final_node for final list
struct final_node
{
	int days;
	int sus_no;
	int inf_no;
	int rec_no;
	struct final_node* next;
};

// defining struct for linked list
struct list
{
	struct node* head;
};

// defining struct for final list
struct final_list
{
	struct final_node* head;
};

// a function to create linked list
struct list* create_list()
{
	struct list* L = (struct list*)malloc(sizeof(struct list));
	L->head = NULL;

	return L;
}

// a function to create final list
struct final_list* create_final_list()
{
	struct final_list* L = (struct final_list*)malloc(sizeof(struct final_list));
	L->head = NULL;

	return L;
}

// a function to create a node in linked list
struct node* create_list_node(int id_number, int time)
{
	// creating new node
	struct node* temp = (struct node*)malloc(sizeof(struct node));
	// assigning the respective values of new node
	temp->id_number = id_number;
	temp->time = time;
	temp->next = NULL;

	// returning new node address
	return temp;
}

// a function to create a node in final list
struct final_node* create_final_list_node(int days, int sus_no, int inf_no, int rec_no)
{
	// creating new node
	struct final_node* temp = (struct final_node*)malloc(sizeof(struct final_node));
	// assigning the respective values of new node
	temp->days = days;
	temp->sus_no = sus_no;
	temp->inf_no = inf_no;
	temp->rec_no = rec_no;
	temp->next = NULL;

	// returning new node address
	return temp;
}

// a function to insert new numbers into list
void list_insert(struct list* L, int id_number, int time)
{
	// declaring all variables
	struct node* K;
	struct node* N;

	K = L->head;
	// creating new node with its data
	N = create_list_node(id_number,time);

	// if list is empty inserting as head
	if(K == NULL)
	{
		L->head = N;
	}
	// else inserts at the end
	else
	{
		while(K->next != NULL)
		{
			K = K->next;
		}
		K->next = N;
	}
}

// a function to insert new numbers into final list
void final_list_insert(struct final_list* L, int days, int sus_no, int inf_no, int rec_no)
{
	// declaring all variables
	struct final_node* K;
	struct final_node* N;

	K = L->head;
	// creating new node with its data
	N = create_final_list_node(days,sus_no,inf_no,rec_no);

	// if list is empty inserting as head
	if(K == NULL)
	{
		L->head = N;
	}
	// else inserts at the end
	else
	{
		while(K->next != NULL)
		{
			K = K->next;
		}
		K->next = N;
	}
}

// a function delete a node in list
void list_delete(struct list* L, int id_number)
{
	// declaring all variables
	struct node* K;
	struct node* N;

	// if list is not empty
	if(L->head != NULL )
	{
		K = L->head;

		// deleting the head case
		if(K->id_number == id_number)
		{
			N = K->next;
			K->next = NULL;
			L->head = N;
		}
		// deleting a node which is not the head
		else
		{
			// traversing through list to find the node that has to be deleted
			while(K!=NULL)
			{
				// find the node which is just before the node that has to be deleted
				if(K->next!=NULL && K->next->id_number == id_number)
				{
					N = K->next;
					K->next = N->next;
					break;
				}
				K = K->next;
			}
		}
	}
}

// a function delete a node in final list
void final_list_delete(struct final_list* L, int days)
{
	// declaring all variables
	struct final_node* K;
	struct final_node* N;

	// if list is not empty
	if(L->head != NULL )
	{
		K = L->head;

		// deleting the head case
		if(K->days == days)
		{
			N = K->next;
			K->next = NULL;
			L->head = N;
		}
		// deleting a node which is not the head
		else
		{
			// traversing through list to find the node that has to be deleted
			while(K!=NULL)
			{
				// find the node which is just before the node that has to be deleted
				if(K->next!=NULL && K->next->days == days)
				{
					N = K->next;
					K->next = N->next;
					break;
				}
				K = K->next;
			}
		}
	}
}

// a function to print the final list
void print_final_list(struct final_list* L)
{
	// declaring variables
	struct final_node* K;
	K = L->head;

	// if list is empty prints list is empty
	if(K == NULL)
	{
		printf("list is empty\n");
	}
	// else traverses through list and prints every data
	else
	{
		while(K != NULL)
		{
			printf("%3d :%6d,%6d,%6d\n", K->days, K->sus_no, K->inf_no, K->rec_no);
			K = K->next;
		}
		printf("\n");
	}
}

// a function to print the list
void print_list(struct list* L)
{
	// declaring variables
	struct node* K;
	K = L->head;

	// if list is empty prints list is empty
	if(K == NULL)
	{
		printf("list is empty\n");
	}
	// else traverses through list and prints every data
	else
	{
		while(K != NULL)
		{
			printf("->%d", K->id_number);
			K = K->next;
		}
		printf("\n");
	}
}

// a function to print no of nodes in list
int print_no_of_nodes(struct list* L)
{
	// declaring all variables
	int counter =0;
	struct node* K;
	K = L->head;

	// traverses through list an counts number of nodes in list
	while(K != NULL)
	{
		counter = counter + 1;
		K = K->next;
	}
	return counter;
}

// a function to delete duplicate day numbers in final list
void delete_duplicate(struct final_list* L)
{
	// declaring all variables
	struct final_node* K;
	struct final_node* M;

	K = L->head;

	while(K != NULL)
	{
		M = K->next;
		while(M != NULL)
		{
			// if there are duplicate days in final list
			if(K->days == M->days)
			{
				K = K->next;
				// deletes duplicate days in final list
				final_list_delete(L,K->days);
			}
			M = M->next;
		}
		K = K->next;
	}
}

// a function to create an empty queue
struct priority_queue* create_priority_queue()
{
	struct priority_queue* temp = (struct priority_queue*)malloc(sizeof(struct priority_queue));
	temp->head = NULL;

	return temp;	
}

// a function to create a new node for priority queue
struct priority_q_node* create_q_node(int time, int event,struct vertex* node)
{
	// creating new node
	struct priority_q_node* new_node= (struct priority_q_node*)malloc(sizeof(struct priority_q_node));
	// assigning the respective values of new node 
	new_node->time = time;
	new_node->event_type = event;
	new_node->node = node;
	new_node->next = NULL;

	// returns the address of the new node
	return new_node;
}

// a function to remove the node with highest priority in the queue
void dequeue(struct priority_queue* P)
{
	// Declaring all variables
	struct priority_q_node* K;
	struct priority_q_node* M;
	K = P->head;

	// if queue is not empty removes top event
	if(K != NULL)
	{
		M = K;
		K = K->next;
		M->next = NULL;
		// assigning new head for priority queue
		P->head = K;
	}
}

// a function to insert new node into queue according to the priority
void enqueue(struct priority_queue* P, int time, int event, struct vertex* node)
{
	// Declaring all variables
	struct priority_q_node* K;
	struct priority_q_node* temp;
	// assigning values to the variables
	K = P->head;
	temp = create_q_node(time,event,node);

	// if queue is empty inserts a new node and makes it head
	if(K == NULL)
	{
		P->head = temp;
	}
	// if queue is not empty
	else
	{
		// if priority of head is less than new node
		if(K->time > time)
		{
			// inserting new node as head
			temp->next = K;
			P->head = temp;
		}
		else
		{
			// moving along the queue to find the position of new node
			while(K->next != NULL && K->next->time < time)
			{
				K = K->next;
			}
			// inserting the new node 
			temp->next = K->next;
			K->next = temp;
		}
	}
}

// a function to predict transmission time to a vertex (person)
int transmission_time()
{
	// declaring and assigning values to the variables
	int r,counter;
	counter = 0;
	
	// infinite loop	
	while(1)
	{
		// randomly generates 0 or 1 with equal probability of 0.5
		r = rand()%2;
		if(r==1)
		{
			// if it is head (1) then adds 1 to counter and returns the value
			counter = counter +1;
			return counter;
		}
		// if it is tail (0) then add 1 to counter and loop repeats
		else
		{
			counter = counter + 1;
		}
	}
}

// a function to predict recovery time of a vertex (person)
int recovery_time()
{
	// declaring and assigning values to the variables
	int r,counter;
	counter = 0;
	
	// infinite loop	
	while(1)
	{
		// randomly generates a number from 0 to 9
		r = rand()%10;

		// heads case
		if(r<2)
		{
			// probability of occuring this event is 0.2
			counter = counter +1;
			return counter;
		}
		// tails case
		else
		{
			counter = counter + 1;
		}
	}
}

// a function for transmission event of a vertex in graph
void process_trans_SIR(struct vertex* array,struct vertex* node,int T,int tmax,struct priority_queue* P, struct list* S, struct list* I, struct list* R, struct final_list* L)
{
	// declaring all variables
	int a,rec_time,trans_time;
	int s,i,r;
	// removing 1 st event in queue as it is processed below
	dequeue(P);
	// changing the node status to infected
	node->status = 'I';

	// deleting that vertex from sucesptible list and inserting to transmission list
	list_delete(S,node->vertex_id);
	list_insert(I,node->vertex_id,T);

	s = print_no_of_nodes(S);
	i = print_no_of_nodes(I);
	r = print_no_of_nodes(R);

	// inserting data in final list
	final_list_insert(L, T, s, i, r);

	// obtaining recovery time of infected node
	rec_time = T + recovery_time();
	if(rec_time < tmax)
	{
		// if recovery time is less than tmax then recovery event is added to the queue
		enqueue(P,rec_time,0,node);
	}
	// creating transmission events to all the edges of the present vertex
	struct linked_vertices*M = node->head_link;
	while(M != NULL)
	{
		a = M->id_number;

		// if node stauts is susceptible
		if(array[a-1].status == 'S')
		{
			trans_time = T + transmission_time();
			// if transmission time is less than tmax
			if(trans_time < tmax)
			{
				// transmission event is added to the queue
				enqueue(P,trans_time,1,&array[a-1]);
			}
		}
		M = M->link;
	}
}

// a function for recovery event of a vertex in graph
void process_rec_SIR(struct vertex* node, int T, struct priority_queue* P,struct list* S, struct list* I, struct list* R, struct final_list* L)
{
	int s,i,r;
	if(node->status == 'I')
	{
		// if node status is infected it is changed to recovered
		node->status = 'R';
		// deleting that vertex from infected list and inserting to recovered list
		list_delete(I,node->vertex_id);
		list_insert(R,node->vertex_id,T);

		s = print_no_of_nodes(S);
		i = print_no_of_nodes(I);
		r = print_no_of_nodes(R);

		// inserting data in final list
		final_list_insert(L, T, s, i, r);		
	}
	// removing the event from the queue
	dequeue(P);
}

int main(void)
{
	// declaring all variables
	struct priority_queue* P;
	P = create_priority_queue();
	struct priority_q_node* K;

	// Declaring all variables and assigning their values
	int a,t,j,r,k,flag;
	int tmax = 300;
	struct linked_vertices* M;
	struct linked_vertices* N;

	// Declaring all variables and assigning their values
	int n =10000; 																// n = number of vertices
	int e =30; 																	// e = number of edges
	struct vertex array[n];

	// creating lists
	struct list* S = create_list();
	struct list* I = create_list();
	struct list* R = create_list();
	struct final_list* L = create_final_list();

	// Declaring all variables
	int sn,inf,rn;
	int i;

	// initially making all the vertices in the graph susceptible
	for(i =0;i<n;i++)
	{
		array[i].vertex_id = i + 1;
		array[i].status = 'S';
		array[i].head_link = NULL;
		// every vertex will be in susceptible list as they are all susceptible
		list_insert(S,i+1,0);
	}

	srand(time(NULL));
	// for creation of graph
	for(i=0;i<n;i++)
	{	
		// randomly selects a number from 0 to e(maximum number of edges)
		r = rand()%(e+1);

		j=0;
		while(j<r)
		{
			// randomly selects a number from 1 to n(number of vertices (people))
			k = rand()%n +1;

			// in first iteration
			if(j==0)
			{
				// if the vertex number not equal to generated random number
				if(k != array[i].vertex_id)
				{
					// creating the head of the edges list
					array[i].head_link = (struct linked_vertices*)malloc(sizeof(struct linked_vertices));
					M = array[i].head_link;
					// assigning its values
					M->id_number = k;
					M->link = NULL;
					// incrementing by 1
					j=j+1;
				}
			}
			else
			{
				if(k != array[i].vertex_id)
				{
					// traverses to the end of list
					M = array[i].head_link;
					while(M->link != NULL)
					{
						M = M->link;
					}
					// creates a new node
					N = (struct linked_vertices*)malloc(sizeof(struct linked_vertices));
					// assigning the respective values of new node
					N->id_number = k;
					N->link = NULL;
					// joins the new node at the end of the list
					M->link = N;
					// incrementing by 1
					j=j+1;
				}
			}
		}
	}
	// by here graph is created with all the edges 


	// randomly selecting a vertex(person) and putting its transmission event in queue
	r = rand()% n;
	enqueue(P,1,1,&array[r]);

	K = P->head;

	// processes all the events in the queue
	while(K != NULL)
	{
		// if the event is transmission event
		if(K->event_type == 1)
		{
			// if the node status is susceptible
			if(K->node->status == 'S')
			{
				// calling the function process_trans_SIR
				process_trans_SIR(array,K->node,K->time,tmax,P,S,I,R,L);
			}
			else
			{
				// if its a duplicate event it is removed
				dequeue(P);
			}
		}
		// if the event is recovery event
		else
		{
			if(K->node->status == 'I')
			{
				// calling the function process_rec_SIR
				process_rec_SIR(K->node, K->time, P,S,I,R,L);
			}
			else
			{
				// if it is a duplicate event it is removed
				dequeue(P);
			}
		}

		K = P->head;
	}

	printf("After %d days :\n",tmax );
	printf("\n");
	// prints suscetible infected and recovered list
	printf("Suceptible list:\n");
	print_list(S);
	printf("\n");
	printf("Infected list:\n");
	print_list(I);
	printf("\n");
	printf("Recovered list:\n");
	print_list(R);
	printf("\n");

	sn = print_no_of_nodes(S);
	inf = print_no_of_nodes(I);
	rn = print_no_of_nodes(R);

	// prints number of nodes in each list
	printf("Suceptible - %d , Infected - %d , Recovered - %d \n", sn, inf, rn);
	printf("\n");
	printf("below data is in the order of\n");
	printf("Day number : Susceptible people, Infected people, Recovered people\n");
	printf("\n");
	// deletes duplicate elements in final list
	delete_duplicate(L);
	//prints final list
	print_final_list(L);
}
