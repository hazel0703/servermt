/**
 * server-mt.c: Multi-threaded web server implementation.
 *
 * Repeatedly handles HTTP requests sent to this port number.
 * Most of the work is done within routines written in request.c
 *
 * Course: 1DT032
 *
 * Student: Adriana Devera 820307T223
 * 
 * To run:
 *  server <portnum (above 2000)> <threads> <schedalg>
 */

#include <assert.h>
#include "server_types.h"
#include "request.h"
#include "util.h"


// 
// server.c: A very, very simple web server
//
// To run:
//  server <portnum (above 2000)>
//
// Repeatedly handles HTTP requests sent to this port number.
// Most of the work is done within routines written in request.c
//

pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t fill = PTHREAD_COND_INITIALIZER;
pthread_cond_t empty = PTHREAD_COND_INITIALIZER;

pthread_t **cid;

typedef struct {
	int fd;
	long size, arrival, dispatch;
} request;

typedef struct {
	int requests;
	int fillptr;
	request **buffer;
} epoch;

epoch **epochs;
request **buffer;
int fillptr, useptr, max, numfull, algorithm, threadid;


/* Statistics for the average */
volatile int clients_treated = 0;
volatile int latencies_acc = 0;
/**
 * Very simple input option parser.
 *
 * @argc: 
 * @argv:
 * @port: port number that the server will listen.
 */
void getargs(int argc, char *argv[], int *port, int *threads, int *buffers, sched_alg *alg)
{
	assert(port != NULL);
	assert(threads != NULL);
	assert(buffers != NULL);
	assert(alg != NULL);
	assert(argc >=0);
	assert(argv != NULL);

	if (argc != 4) {
		fprintf(stderr, "Usage: %s <port> <threads> <schedalg>\n", argv[0]);
		exit(1);
	}
	
	*port = atoi(argv[1]);
	*threads = atoi(argv[2]);
	/* We set the number of buffers==threads */
	*buffers = atoi(argv[2]);

	if(strcasecmp(argv[3], "FIFO") == 0) {
		*alg = FIFO;
	} else if(strcasecmp(argv[3], "SFF") == 0) {
		*alg = SFF;
	} else {
		fprintf(stderr, "Scheduling algorithm must be one of the following options: FIFO, STACK, SFF, BFF.\n");
		exit(1);
	}
}

int requestcmp(const void *first, const void *second) {
	assert(first != NULL);
	assert(second != NULL);
	return ((*(request **)first)->size - (*(request **)second)->size);
}

/**
 * Calculates the time in microsecond resultion.
 * 
 * ARGUMENTS:
 * @t: timeval structure
 */
long calculate_time(struct timeval t) {
	return ((t.tv_sec) * 1000 + t.tv_usec/1000.0) + 0.5;
}


/**
 * Consumer. The function that will be executed by each of the threads
 * created in main. (Entry point)
 * 
 */
void *consumer(void *arg) {
	/* assert(arg != NULL); */

	/* TODO: Create a thread structure */
	thread worker;

	/* TODO: Initialize the statistics of the thread structure */
	worker.id = -1;
	worker.count = 0;
	worker.statics = 0;
	worker.dynamics = 0;
<<<<<<< HEAD
	//worker.client_id = 0;
	
	//request *req;
=======
	worker.client_id = 0;

>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02
	struct timeval dispatch;

	/* Main thread loop */
	while(1) {
		/* TODO: Take the mutex */
		pthread_mutex_lock(&lock);

		/*
		GERMAN
		What happens if pthread_mutex_lock fails? Make sure you check the return value.
		*/
		
		/*
		GERMAN
		What happens if pthread_mutex_lock fails? Make sure you check the return value.
		*/ /* but i need to call the pthread_mutex_lock and the return value will be given by the following while*/
    		
		/* TODO: Wait if there is no client to be served. */
		while(numfull == 0) {
			pthread_cond_wait(&fill, &lock);
		}


		/* TODO: Get the dispatch time */
		gettimeofday(&dispatch, NULL);

		/* TODO: Set the ID of the the thread in charge */
		if(worker.id < 0) {
			worker.id = threadid;
			threadid++; /*Not sure if we need to increment the thread in charge*/
		}
		worker.count++;
		request *req = malloc(sizeof(request));
		

		request *req;

		/* Get the request from the queue according to the sched algorithm */
<<<<<<< HEAD
		if (algorithm == FIFO) {
=======
		if(algorithm == FIFO) {
>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02
			/* TODO: Implement your first scheduling policy here (FIFO or STACK) */
		/*As i am in group 13 i must implement the FIFO*/
			req = (request *)buffer[useptr];
			useptr = (useptr + 1) % max;
<<<<<<< HEAD

		} else if (algorithm == SFF) {
			/* TODO: Implement your second scheduling policy here (SFF or BFF) */
=======
		}
		
		else if(algorithm == SFF) {
		/* TODO: Implement your second scheduling policy here (SFF or BFF) */
>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02
			/*As i am in group 13 i must implement the SFF*/
			req = (request *)buffer[0];
			buffer[0] = buffer[fillptr - 1];
			if(fillptr > 1) {
				qsort(buffer, fillptr, sizeof(*buffer), requestcmp);
			}
			fillptr--;
		}
		
		/* TODO: Set the dispatch time of the request */
		req->dispatch = (calculate_time(dispatch));
<<<<<<< HEAD
	 	
=======
		numfull--;

>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02
		
		printf("Latency for client %d was %ld\n", worker.client_id, (long)(req->dispatch - req->arrival));
		printf("Avg. client latency: %.2f\n", (float)latencies_acc/(float)clients_treated);
	

		/* TODO: Signal that there is one request left */
		numfull--;

		/* Update Server statistics */
		clients_treated++;
		latencies_acc += (long)(req->dispatch - req->arrival);

		/* TODO: Synchronize */
		pthread_cond_signal(&empty);
		pthread_mutex_unlock(&lock);
		
		/* 
		GERMAN
		Same as before, what happens if this function fails?
		*/	

		/* 
		GERMAN
		Same as before, what happens if this function fails?
		*/	

		/* TODO: Dispatch the request to the Request module */

		requestHandle(req->fd, req->arrival, req->dispatch, &worker);
		Close(req->fd);

	}
}

int main(int argc, char *argv[])
{
    /* Variables for the connection */
    int listenfd, connfd, clientlen;
    struct sockaddr_in clientaddr;

    /* Variables for the user arguments */
    int port; 
    int threads, buffers; 
    sched_alg alg;

	
   /* Timestamp variables */
   struct timeval arrival;

  /* Parse the input arguments */
  getargs(argc, argv, &port, &threads, &buffers, &alg);

	/*  TODO:
	 *  Initialize the global variables:
	 *     max,
	 *     buffers,
	 *     numfull,
	 *     fillptr,
	 *     useptr,
	 *     algorithm  */
	max = buffers;
	numfull = 0;
	fillptr = 0;
	useptr = 0;
	algorithm = alg; 

	/* TODO: Allocate the requests queue */
<<<<<<< HEAD
	
	buffer = malloc(buffers * sizeof(*buffer)); /*This is for FIFO and SSF*/
	
	/*revisar*/
=======
	if(alg > 0) {
		epochs = malloc((buffers / alg) * sizeof(*epochs));
	}
	// FIFO or SFF
	else {
		buffer = malloc(buffers * sizeof(*buffer));
	}

>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02
	/* TODO: Allocate the threads buffer */
	cid = malloc(threads*sizeof(*cid));

	/* TODO: Create N consumer threads */
	int i;
	for(i = 0; i< threads; i++) {
		cid[i] = malloc(sizeof(pthread_t));
		pthread_create(cid[i], NULL, consumer, NULL);
	}

    /* Main Server Loop */
    listenfd = Open_listenfd(port);
    while (1) {
		clientlen = sizeof(clientaddr);
		connfd = Accept(listenfd, (SA *)&clientaddr, (socklen_t *) &clientlen);

		/* Save the arrival timestamp */
		gettimeofday(&arrival, NULL);

		/* TODO: Take the mutex to modify the requests queue */
		pthread_mutex_lock(&lock);
<<<<<<< HEAD
=======

		/* GERMAN
		Why are you unlocking here?
		*/
>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02

		/* GERMAN
		Why are you unlocking here?, us lock, not unlock, just made a mistake
		*/
	
		/* TODO: If the request queue is full, wait until somebody frees one slot */
		while(numfull == max) {
			pthread_cond_wait(&empty, &lock);
		}
		
		
		/* Allocate a request structure */
		request *req = malloc(sizeof(request)); 

		/* TODO: Fill the request structure */ 
<<<<<<< HEAD
		req->size = requestFileSize(connfd);
                req->fd = connfd;
                req->arrival = calculate_time(arrival);
=======
>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02
		//pthread_cond_signal(&fill); // <-- GERMAN: I think you want to do this later, maybe after inserting things in the queue.
		
		/* Queue new request depending on scheduling algorithm */
<<<<<<< HEAD
		if (alg == FIFO) { /*Policy 1*/
			/* TODO: Queue request according to POLICY1 (FIFO or STACK) */
			buffer[fillptr] = req;	
			fillptr = (fillptr+1) % max;


		} else if(alg == SFF) {
=======
		if(alg == FIFO) {/*Policy 1*/
			/* TODO: Queue request according to POLICY1 (FIFO or STACK) */
			buffer[fillptr] = req;	
			fillptr = (fillptr+1) % max;	
		}
		
		else if(alg == SFF) {
>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02
			/* TODO: Queue request according to POLICY2 (SFF or BFF) */
			/* HINT: 
			   You can use requestFileSize() to check the size of the file requested.
			   You can use qsort() with requestcmp() to sort the requests by size.
			 */
			req->size = requestFileSize(connfd);
			buffer[fillptr] = req;
			fillptr++;
			if(fillptr > 1) {
<<<<<<< HEAD
			qsort(buffer, fillptr, sizeof(*buffer), requestcmp);
				     }
		}

=======
				qsort(buffer, fillptr, sizeof(*buffer), requestcmp);
			}
		}
		
		
		req->fd = connfd;
		req->arrival = ((arrival.tv_sec) * 1000 + arrival.tv_usec/1000.0) + 0.5;
		
>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02
		/* TODO: Increase the number of clients queued */
		numfull++;

		/* TODO: Synchronize */
<<<<<<< HEAD
		pthread_cond_signal(&fill); 
		pthread_mutex_unlock(&lock); /*Using mutex signal*/
		/* <-- GERMAN: Maybe here you want to unlock and then signal the fill condition */
	}
}

=======
		pthread_cond_signal(&fill);
		pthread_mutex_unlock(&lock); /*Using mutex signal*/
		/* <-- GERMAN: Maybe here you want to unlock and then signal the fill condition */
    }

}
>>>>>>> 4b84aa0dfb3666ad230611b3171c8276b1579d02


    


 
