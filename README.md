# T+40.347
This problem was asked by Airbnb...

You are given a huge list of airline ticket prices between different cities around the world on a given day. These are all direct flights. Each element in the list has the format (source_city, destination, price)..

Consider a user who is willing to take up to k connections from their origin city A to their destination B. Find the cheapest fare possible for this journey and print the itinerary for that journey..

For example, our traveler wants to go from JFK to LAX with up to 3 connections, and our input flights are as follows:. .

[
    ('JFK', 'ATL', 150),
    ('ATL', 'SFO', 400),
    ('ORD', 'LAX', 200),
    ('LAX', 'DFW', 80),
    ('JFK', 'HKG', 800),
    ('ATL', 'ORD', 90),
    ('JFK', 'LAX', 500),
]
Due to some improbably low flight prices, the cheapest itinerary would be JFK -> ATL -> ORD -> LAX, costing $440...

Solution
Let's first think about how we would approach this problem without the constraint of limiting our traveler to k connections. This reduces to finding the shortest path between two points in a graph, which can be efficiently solved using Dijkstra's algorithm...

We will maintain a heap that is keyed on the total cost of the journey so far, and which additionally holds the current node and the accumulated path. Initially, this heap will store a single item representing the fact that it costs nothing to begin at our source airport.

At each step of the process, we pop the lowest cost item off the heap. Then, we take all unvisited connecting airports and place them on the heap, with their accumulated flight cost and path. Once we reach our destination, we return these values.

To handle the extra constraint, we can add another variable to each heap item representing how many remaining connections are allowed. Initially this will be k + 1, and for each flight taken we will decrement by one. If we reach 0, we know that we cannot continue the current path, so we will skip to the next item...

The time complexity of Dijkstra's algorithm is O(E + V log V). Here, E represents the number of flights in our input and V represents the number of airports. As for space complexity, our heap may store up to V items, each of which can have a path of length k, so we can describe this as O(V * k).

Alternatively, we can solve this problem using a dynamic programming approach.

Initially, we will set the cost to get to all airports as infinite, except for the source, which will be zero. At each step of our journey, we will loop through our list of flights to find cheaper ways of reaching a given destination. If one is found, we reset that destination's cost and previous stop.

For an example, let's look at the input above. Early on we find a flight from JFK to LAX for $500, and soon after we find a two-step route to ORD that totals $240. Once we see that the cost to get to ORD plus the cost to get from ORD to LAX is less than $500, we can reduce the total for LAX.

We will need to perform this loop k times, since there may be up to k connecting flights. Finally, we reconstruct our path from the previous stops stored along the way.
