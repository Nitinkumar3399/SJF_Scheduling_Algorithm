// C++ program to illustrate Banker's Algorithm 
// also known as Deadlock Avoidance Algorithm.
#include<iostream>
using namespace std;

// Number of processes and Number of Resources
int P, R;

// Function to find the need of each process
void calculateNeed(int **need, int **maxm, int **allot)
{
	// Calculating Need of each P
	for (int i = 0 ; i < P ; i++)
		for (int j = 0 ; j < R ; j++)

			// Need of instance = maxm instance -
			//				 allocated instance
			need[i][j] = maxm[i][j] - allot[i][j];
}

// Function to find the system is in safe state or not
bool isSafe( int *avail, int **maxm, int **allot)
{
	int **need;
    need = new int*[P];
    for (int i = 0; i < P; i++) {

        need[i] = new int[R];
    }

	// Function to calculate need matrix
	calculateNeed(need, maxm, allot);

	// Mark all processes as infinish
	bool finish[P] = {0};

	// To store safe sequence
	int safeSeq[P];

	// Make a copy of available resources
	int work[R];
	for (int i = 0; i < R ; i++)
		work[i] = avail[i];

	// While all processes are not finished
	// or system is not in safe state.
	int count = 0;
	while (count < P)
	{
		// Find a process which is not finish and
		// whose needs can be satisfied with current
		// work[] resources.
		bool found = false;
		for (int p = 0; p < P; p++)
		{
			// First check if a process is finished,
			// if no, go for next condition
			if (finish[p] == 0)
			{
				// Check if for all resources of
				// current P need is less
				// than work
				int j;
				for (j = 0; j < R; j++)
					if (need[p][j] > work[j])
						break;

				// If all needs of p were satisfied , then current 
                // process has been executed and releases their all resources.
				if (j == R)
				{
					// Add the allocated resources of
					// current P to the available/work
					// resources i.e.free the resources
					for (int k = 0 ; k < R ; k++)
						work[k] += allot[p][k];

					// Add this process to safe sequence.
					safeSeq[count++] = p;

					// Mark this p as finished
					finish[p] = 1;

					found = true;
				}
			}
		}

		// If we could not find a next process in safe
		// sequence.
		if (found == false)
		{
			cout << "System is not in safe state";
			return false;
		}
	}

	// If system is in safe state then
	// safe sequence will be as below
	cout << "\n\nSystem is in safe state.\nSafe"
		" sequence is: ";
	for (int i = 0; i < P ; i++)
		cout << safeSeq[i] << " ";

	return true;   
}


int main()
{
    
	cout<<"\nEnter the number of processes : ";
    cin>>P;
    cout<<"\nEnter the number of resources : ";
    cin>>R;

	// Available instances of resources
    cout<<"\nEnter the AVAILABLE number of instances of "<<R<<" resources : ";
    
	int *avail;
    avail = new int[R];
    for (int i = 0; i < R ; i++)
    {
        cin>>avail[i];
    }
    cout<<endl;
    
	// Maximum R that can be allocated
	// to processes
    cout<<"MAXIMUM resources that can be allocated to processes"<<endl;
    int **maxm;
    maxm = new int*[P];
    for (int i = 0; i < P; i++) {

        maxm[i] = new int[R];
    }

    for (int i = 0; i < P ; i++)
    {
        cout<<"Enter maximum instances of "<<R<<" resources for process "<<i+1<<" : ";
        for (int j = 0; j < R ; j++){
         
          cin>>maxm[i][j];     
        }
        cout<<endl;   
    }
    cout<<endl;

	// Resources allocated to processes
    cout<<"Resources ALLOCATED to processes"<<endl;
	int **allot;
    allot = new int*[P];
    for (int i = 0; i < P; i++) {

        allot[i] = new int[R];
    }

    for (int i = 0; i < P ; i++)
    {
        cout<<"Enter allocated instances of "<<R<<" resources for process "<<i+1<<" : ";
        for (int j = 0; j < R ; j++){
         
          cin>>allot[i][j];     
        }
        cout<<endl;   
    }

	// Check system is in safe state or not
	isSafe(avail, maxm, allot);

	return 0;
}
