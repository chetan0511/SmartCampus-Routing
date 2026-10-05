# SmartCampus-Routing#include <stdio.h>
#include <string.h>

#define MAX 20
#define INF 99999

// Campus Graph
int graph[MAX][MAX];
char location[MAX][50];

int n = 0;
int source = -1;

int distance[MAX];
int visited[MAX];
int parent[MAX];


// Find Minimum Distance Vertex
int findMinDistance()
{
    int min = INF;
    int index = -1;

    for (int i = 0; i < n; i++)
    {
        if (visited[i] == 0 && distance[i] < min)
        {
            min = distance[i];
            index = i;
        }
    }

    return index;
}


// Enter Campus Graph
void enterGraph()
{
    int distanceValue;

    printf("\nEnter number of locations: ");
    scanf("%d", &n);

    if (n <= 0 || n > MAX)
    {
        printf("Invalid number of locations!\n");
        n = 0;
        return;
    }

    printf("\nEnter Location Names:\n");

    for (int i = 0; i < n; i++)
    {
        printf("Location %d: ", i + 1);
        scanf(" %[^\n]", location[i]);
    }

    // Initialize graph
    for (int i = 0; i < n; i++)
    {
        graph[i][i] = 0;

        for (int j = i + 1; j < n; j++)
        {
            printf("\nDistance between %s and %s: ",
                   location[i], location[j]);

            scanf("%d", &distanceValue);

            if (distanceValue < 0)
            {
                printf("Distance cannot be negative!\n");
                j--;
            }
            else if (distanceValue == 0)
            {
                graph[i][j] = INF;
                graph[j][i] = INF;
            }
            else
            {
                graph[i][j] = distanceValue;
                graph[j][i] = distanceValue;
            }
        }
    }

    source = -1;

    printf("\nCampus Graph Created Successfully!\n");
}


// Display Adjacency Matrix
void displayGraph()
{
    if (n == 0)
    {
        printf("\nPlease enter the campus graph first!\n");
        return;
    }

    printf("\n----------- ADJACENCY MATRIX -----------\n\n");

    printf("%-22s", "");

    for (int i = 0; i < n; i++)
    {
        printf("%-18s", location[i]);
    }

    printf("\n");

    for (int i = 0; i < n; i++)
    {
        printf("%-22s", location[i]);

        for (int j = 0; j < n; j++)
        {
            if (graph[i][j] == INF)
                printf("%-18s", "INF");
            else
                printf("%-18d", graph[i][j]);
        }

        printf("\n");
    }
}


// Select Source Location
void selectSource()
{
    int choice;

    if (n == 0)
    {
        printf("\nPlease enter the campus graph first!\n");
        return;
    }

    printf("\n----------- CAMPUS LOCATIONS -----------\n");

    for (int i = 0; i < n; i++)
    {
        printf("%d. %s\n", i + 1, location[i]);
    }

    printf("\nEnter source location: ");
    scanf("%d", &choice);

    if (choice < 1 || choice > n)
    {
        printf("Invalid location!\n");
        source = -1;
        return;
    }

    source = choice - 1;

    printf("\nSource Selected: %s\n", location[source]);
}


// Dijkstra's Algorithm
void dijkstra()
{
    if (n == 0)
    {
        printf("\nPlease enter the campus graph first!\n");
        return;
    }

    if (source == -1)
    {
        printf("\nPlease select source location first!\n");
        return;
    }

    // Initialize arrays
    for (int i = 0; i < n; i++)
    {
        distance[i] = INF;
        visited[i] = 0;
        parent[i] = -1;
    }

    distance[source] = 0;

    // Find shortest distances
    for (int i = 0; i < n - 1; i++)
    {
        int u = findMinDistance();

        if (u == -1)
            break;

        visited[u] = 1;

        for (int j = 0; j < n; j++)
        {
            if (visited[j] == 0 &&
                graph[u][j] != INF &&
                distance[u] != INF &&
                distance[u] + graph[u][j] < distance[j])
            {
                distance[j] = distance[u] + graph[u][j];
                parent[j] = u;
            }
        }
    }

    printf("\nShortest Distances Calculated Successfully!\n");
}


// Display Shortest Path
void printPath(int vertex)
{
    if (parent[vertex] == -1)
    {
        printf("%s", location[vertex]);
        return;
    }

    printPath(parent[vertex]);

    printf(" -> %s", location[vertex]);
}


// Display Shortest Paths
void displayPaths()
{
    if (n == 0)
    {
        printf("\nPlease enter the campus graph first!\n");
        return;
    }

    if (source == -1)
    {
        printf("\nPlease select source and find shortest distance first!\n");
        return;
    }

    printf("\n----------- SHORTEST PATHS -----------\n");
    printf("Source: %s\n\n", location[source]);

    for (int i = 0; i < n; i++)
    {
        printf("Destination: %s\n", location[i]);

        if (distance[i] == INF)
        {
            printf("Path: Not Reachable\n");
        }
        else
        {
            printf("Path: ");
            printPath(i);
            printf("\nDistance: %d\n", distance[i]);
        }

        printf("\n");
    }
}


// Display Distance from Source
void displayDistances()
{
    if (n == 0)
    {
        printf("\nPlease enter the campus graph first!\n");
        return;
    }

    if (source == -1)
    {
        printf("\nPlease select source and find shortest distance first!\n");
        return;
    }

    printf("\n------- DISTANCES FROM SOURCE -------\n");
    printf("Source: %s\n\n", location[source]);

    for (int i = 0; i < n; i++)
    {
        printf("%-25s : ", location[i]);

        if (distance[i] == INF)
            printf("Not Reachable\n");
        else
            printf("%d\n", distance[i]);
    }
}


// Main Function
int main()
{
    int choice;

    while (1)
    {
        printf("\n=====================================\n");
        printf("       SMART CAMPUS ROUTING SYSTEM\n");
        printf("          DIJKSTRA'S ALGORITHM\n");
        printf("=====================================\n");

        printf("1. Enter Campus Graph\n");
        printf("2. Display Adjacency Matrix\n");
        printf("3. Select Source Location\n");
        printf("4. Find Shortest Distance\n");
        printf("5. Display Shortest Paths\n");
        printf("6. Display Distance from Source\n");
        printf("7. Exit\n");

        printf("\nEnter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
            enterGraph();
            break;

        case 2:
            displayGraph();
            break;

        case 3:
            selectSource();
            break;

        case 4:
            dijkstra();
            break;

        case 5:
            displayPaths();
            break;

        case 6:
            displayDistances();
            break;

        case 7:
            printf("\nThank you for using Smart Campus Routing System!\n");
            return 0;

        default:
            printf("\nInvalid choice! Please try again.\n");
        }
    }

    return 0;
}A menu-driven C program that finds the shortest routes between campus locations using Dijkstra’s Algorithm and an adjacency matrix.
