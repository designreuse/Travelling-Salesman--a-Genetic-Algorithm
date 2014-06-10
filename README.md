#Genetic Algorithm for the Travelling Salesman Problem (TSP)

Data Structures & Algorithms; National University of Ireland, Maynooth; April 2014

__Problem:__ We were given the task of finding the shortest route visiting all the given towns only once and returning to the origin (specifically the [symmetric TSP](http://en.wikipedia.org/wiki/Travelling_salesman_problem#Asymmetric_and_symmetric)). 

##Overview
Any genetic algorithm (GA) is made up of the following components and my approach to the TSP using a GA is no different:
1. Selection
2. Reproduction (genetic crossover)
3. Mutation

In my code each gene is represented as a `Town` object and each `Path` object is a collection of 80 `Town`s. The fitness of each `Path` is evaluated according to it's `distance` : the shorter the route around the 80 `Town`s the better (or "fitter"). Each `Population` is made up of a number of `Path`s (the higher the number the less generations the GA will take but the slower the running time will be - this is a trade-off). At each generation, the `Population` is repopulated with the fittest `Path`s having a higher probability of being selected for reproduction. To keep the mating pool from becoming stagnant, a mutation (which may or may not improve the `Path` fitness) is applied at random to the offspring ("crossed-over") `Path` of the chosen parents. This child `Path` is then a member of the `Population` for the next generation.

An all-time fittest `Path` is stored and when this best `distance` hasn't changed in 100 generations, it is returned. My algorithm runs this process a number of times to try to iron out some of the inherent randomness. 

## Steps of this Genetic Algorithm

### 1. Initialisation
`TSP.java` contains the `main` method where the data is read from a `.csv` file. This data is in the form: _town number_ , _town name_ , _latitude_ , _longitude_ : eg `5,Athlone,53.433,-7.95` etc. An adjacency matrix to store the distance between any 2 `Town`s is calculated using the [Haversine formula](http://en.wikipedia.org/wiki/Haversine_formula) and stored in a 2D array. Variables are initialised to control the `Population` size, mutation rate, number of `Generation`s to tolerate without improvement, and number of runs of the complete algorithm. The first generation  `Population` is make up of entirely random (though still valid) `Path`s.

### 2. Selection
The fitness function uses the inverse of the `Path` `distance` (because lower is better) for each member of the `Population`. These values are sorted and each successive `Path`'s value is raised to the power of 4 to create more incentive for better `distance`s. This number will vary according to your `Population` size (4 was the highest a size of 2000 could take without overflowing). __At this stage all `Path`s are evaluated and if a new best has been found, a message is printed and it is stored.__ The mating pool of `Path`s is generated by adding each `Path` a number of times directly proportional to its `fitness` relative to the overall `fitness` of the generation.

### 3. Reproduction
Two parents are chosen from the mating pool and their genetic makeup (ie `Town` objects) are combined with the goal of carrying their best characteristics (or "subpaths") to the next generation. The crossover function used in this GA is the [Edge Recombination operator](http://en.wikipedia.org/wiki/Edge_recombination_operator) which is particularly suited to the TSP because it produces a vaild `Path` (it does not need to be repaired after crossover). This technique also maintains a level of mutation that is critical to keep variety in the next generation.

### 4. Mutation
Added variety is sometimes necessary however and it is produced by the mutation function. Not every child `Path` undergoes mutation - it depends on whether a random number falls below your chosen mutation rate (I chose `0.05` or 5%). The mutation function uses reverses a random subtour and puts it back in a random postion. A valid `Path` is maintained and this technique works well even at low mutation rates.

__keep repeating steps 2-4 until no improvement has been found for the given number of generations.__

## Notes

* I did not write `FileIO.java`. This class was given to all students for repeated use in the module.
* Routes produced by this program can be verified online using [my](https://github.com/stubbedtoe/tsp_checker) Google Maps-based [TSP route checker](http://www.andrewhealy.com/tsp_checker/verifier.html)
* This algorithm managed to find the 3rd best route for the given collection of towns. The winning route used a Branch and Bound algorithm. Even though this algorithm 



