---
layout: post
title: Genetic Algorithms
featured-img: evolution
mathjax: true
categories: Guides
---

# Overview to Genetic Algorithms:
Genetic Algorithms (GA), in the context of Computer Science and Operations Research, is an optimization heuristic algorithm inspired from evolution as seen in nature. Genetic algorithms belong to a broader class of Evolutionary Algorithms (EA). 


# Inpiration and Drawing Parallels
GAs are inspired from the natural process of evolution as seen in nature: species, over generations, are more "evolved"    (better) than their predecessors. Getting better, after all in essense is optimization. 

We will draw parallels between what is seen in nature and how it is translated in the GAs as we go on. 

#### Generations:
"Generations" in nature can be viewed as "iterations" in a GA. That is, with each passing iteration of the GA, our best solution is expected to improve. That is, *Generations are viewed as iterations*.

#### Genotypes and Phenotypes:
In natural selection, two individuals of a species mate to give birth to progeny which have a combination of the genetic encoding of their parents. The genetic infomation of an individual is called the "genotype". A given "genotype" results in a "phenotype", which is simply a physical charateristic of the individual. Perhaps having a specific genotype results in an individual being taller (a physical characteristic, hence the "phenotype"), or have dark hair. When this is translated to the computational world of GAs, the genotype is a "potential solution" and the phenotype is the "measure of interest", or fitness, which we are optimizing for. Therefore,  *a Genotype is a possible solution and Phenotypes (or fitness), is what we want to optimize.*.

To keep things in perspective, we will use the example of the classical Knapsack Problem, and potentially work towards using GAs to solve the Knapsack problem. 

---
### Quick Detour: Knapsack Problem Definition

<img style="float: right;" src="/assets/img/posts/knapsack.png">

**The Knapsack problem:** Say you are Indiana Jones (a famous, fictional archaeologist) and you are find a treasure chest. It has five things; a heavy gold locket, an old diary, a ring, a golden cup and a tiara, and no they are not horcruxes. Each of these items have a weight (in kgs) $ W_1, W_2, .. , W_5 $ and a value (in dollars) $ V_1, V_2, .. , V_5 $. 

However Indiana Jones has just one bag, which can only take a total weight of say, $W$ kgs. He has to decide which of these items he must choose so that the combined weight of the selected items is lesser than $W$ kgs but has the most value. This is called the Knapsack problem: Given a bunch of items to choose from, you want to choose the best subset which results in the most value.  

---

For the Knapsack Problem, one possible genotype representation could be a $[1X5]$ vector. A genotype (recall is a "possible solution") with value $ [1,0,0,0,1] $ would mean Indiana Jones decided to place the Gold Locket and the Tiara into his bag and left eveything else. The phenotype (or the fitness, which we want to optimize) in this case would be $ V_1 + V_5 $. 

#### Cross-over and Mutation:

In the preceding paragraph, we spoke about how the child has a *combination* of the genotype of it's parents. This is called "Cross-Over". Also, in nature, some genotypic sites undergo "mutation", a random change of genetic material at a given genetic site. For example, if two genotypes $ [1,1,0,0,1] $ and $ [1,0,0,0,1] $ were to cross-over at the third index, the child would have genotype, $ [1,1,0,0,1] $. That is, the first three indices are obtained from the first parent, and the last two indices are inherited from the second parent. Also, once cross-over is done, there is a small probability (generally the order of 10<sup>-3</sup>) that mutation at a random site (say 4), the genotype of the child would them be $ [1,1,1,0,1] $. However, one must note that the knapsack problem is a constrained problem and the constraints must be checked before the cross-over or mutation is carried out. If the constraint is not satisfied it must not be accepted as a possible solution. Once a valid-child is produced, it must be added to the population. 

# Componenets in a GA: 
Most GAs consist of the following componenets: 

* **Representation**: How a given solution is characterized. In the Knapsack Problem above, it was represented as a $[1X5]      $ vector. Each vector is a possible solution. 
* **Initial Population**: The set of solutions you start with. This is usually just randomly generated. Imagine 10 of these $[1X5]$ vectors with zeros and ones in random indices. 
* **Selection**: The process of selecting parents from the population used to generate progeny. This is where a gentle pressure (called selection pressure) is applied to make sure you move towards better solutions. There are many types of selection, but the usual premise is, individuals in the population which have higher fitness have higher probability to be selected to be parents. Common selection methods are "Tournament Selection" or "Roulette-Wheel Selection". 
* **Cross-Over**: Once the parents are selected, we need to generate the child. Say Parent 1 has genotype $[ P_{11}, P_{12}, .. , P_{15} ]$ and Parent 2 has genotype $[ P_{21}, P_{22}, .. , P_{25} ]$, one way to generate the child would be to use genotypes from index 0 to 3 from parent 1 and use genotypes from index 4, 5 to create the child. Therefore the child would have genotype $[ P_{11}, P_{12}, P_{13}, P_{24}, P_{25} ]$. This is called the one-fold cross-over. You could also choose to do a two-fold crossover, where the genotype switches from one parent to the other twice. 
* **Mutation**: With a given probability, a randomly chosen index of the genotype is changes. For example, if the child has genotype $[ P_{11}, P_{12}, P_{13}, P_{24}, P_{25} ]$, one position, randomly selected is flipped (if it's a 0, it's flippled to 1 or vice versa). 


#  Using GAs to solve 6 hardest Knapsack instances:

Data for the 6 hardest knapsack instances can be found [here](https://github.com/PadmanabhanAshwin/GeneticAlgo_Knapsack). Book-keeping notes for the data are provided in the project repo. 

Logic Review:
=============

Overall Run Parameters: 
------------------------

-   **Representation**: A binary 1-D array of length equal to number of
    items available to place inside Knapsack. 0/1 representing item
    is/is not added in the Knapsack.

-   **Initial Population**: Start with 40 valid individuals chosen
    randomly. (Ref. section titled "Initialize population")

-   **Progeny progression**: Two parents produce two children.

-   **Number of generations**: Most cases are run for 700 generations.
    This is chosen because this seems to be a good balance between
    having enough generations for convergence and computational cost for
    each generation.


Initialize Population:
----------------------
Given below is a code snippet of how we generate initial population for one of the instances. Note that the probability of success of the Bernoulli trial is set *a priori* based on instance information. 

```python
    def initializepopulation( weight, 
                                knapsackcapacity, inpopulationsize = 40):

            initialpopulation =[]

            # To produce a population of size "inpopulationsize"
            while( len(initialpopulation)< inpopulationsize ):

                #generate random array of 0/1 of size 50 with probability = 0.01. 
                generateindividual = bernoulli.rvs(0.01, size = 50)

                #Check if generated individual is satisfies knapsack capacity constraint. 
                isfeasible = isFeasibleSolution(generateindividual,weight, knapsackcapacity)

                #If feasible, add to initial population pool. 
                if ((isfeasible) and (listcompare(initialpopulation,generateindividual))):
                    initialpopulation.append(generateindividual)

            return(initialpopulation)
```
Within the loop we check if the genotype produced satisfies the capacity constraint of the Knapsack and is appended into the population only if it does. This is the constraint handling mechanism used here. 

Selection:
----------------------
```python
    def selection(population, population1, values, mode= "tournament"):
        if mode == "tournament": 
            parents = []
            for _ in range(2):
                candidateparent = [random.randrange(0,len(population)) 
                                    for _ in range(2)] 
                parent = np.argmax(
                    [population1[candidateparent[0]][1], 
                        population1[candidateparent[1]][1]])
                parent = population[candidateparent[parent]]
                parents.append(parent)
            return(parents)
```
A tournament selection is implemented here. Line 5 above produces two
random indices between *\[0,len(population))* and a tournament takes
place to select the parent with a higher fitness value which then goes
on to be one of the parent. This applies a gently pressure towards the "Survival of the Fittest" Paradigm, whilst not 
engineering the solution too much, which might negatively affect the exploration of the search space. 

For code-snipped on Mutation and Cross-over please visit the project repo. 

Evolution:
----------

Below is a plot showing average fitness and optimal solution evolution
for the instance in file, knapPI\_13\_50\_1000.csv, running for 500
generations. Other evolution plots in submitted folder under \"Evolution
Plots\".

Optimal Evolution|Average Fitness of the Population
:-------------------------:|:-------------------------:
<img src="/assets/img/posts/OptimalEvolution_knap13_50_1000.png" width="250" height="250"> | <img src="/assets/img/posts/AverageEvol_knap13_50_1000.png" width="250" height="250">

Values for Mutation and Cross-Over:
----------

We need to know the values of Mutation and Cross-over *a priori* and is generally tuned once a decent GA is built. A grid-search is one way to find a good balance between Mutation and Cross-Over. Below is a contour plot for different values of Mutation and Cross-Over for the second instance.

Number of fitness calls|Average Fitness
:-------------------------:|:-------------------------:
<img src="/assets/img/posts/MC_balance_knapPI_13_50_1000_functioncalls.png" width="250" height="250"> | <img src="/assets/img/posts/MC_balance_knapPI_13_50_1000_avgfitness2.png" width="250" height="250">

**View the full project [here](https://github.com/PadmanabhanAshwin/GeneticAlgo_Knapsack).**


<!-- 
**Note** that you might have to adjust some CSS depending on the width and height of your logo. You can find Header / Navigation related SCSS in `_sass/layout/nav.scss`.

## Writing content

### Posts

Create a new Markdown file such as `2017-01-13-my-post.md` in `_post` folder. Configure YAML Front Matter (stuff between `---`):

```yaml

---
layout: post # needs to be post
title: Getting Started with Sleek # title of your post
featured-img: evolution #optional - if you want you can include hero image
---

```

#### Images

In case you want to add a hero image to the post, apart from changing `featured-img` in YAML, you also need to add the image file to the project. To do so, just upload an image in `.jpg` format to `_img` folder. The name must before the `.jpg` file extension has to match with `featured-img` in YAML. Next, run `gulp img` from command line to generate optimized version of the image and all the thumbnails. You have to restart  the jekyll server to see the changes. Sleek uses [Lazy Sizes](https://github.com/aFarkas/lazysizes) Lazy Loader for loading images. Check the link for more info. Lazy Sizes doesnt't require any configuration and it's going to be included in your bundled js file.

### Pages

The home page is located under `index.md` file. To change the content or design you have to edit the `default.html` file in `_layouts` folder.

In order to add a new page, create a new html or markdown file under root directory or inside `_pages` folder. To add a link to the page, edit `navigation` setting in `_config.yml`.

### Images TODO

Introduce gulp optimization

Breakpoint | Image Type | Width | Retina
------------ | ------------ | ------------- | -------------
xs |Post Thumb | 535px | 1070px
sm |Post Thumb | 500px| 1000px
md |Post Thumb | 329.375px | 658.75px
lg |Post Thumb | 445.625px | 891.25px
xl |Post Thumb | 353.125px | 706.25px

Breakpoint | Image Type | Width | Retina
------------ | ------------ | ------------- | -------------
xs |Post Hero | 535px | 1070px
sm |Post Hero | 500px| 1000px
md |Post Hero | 329.375px | 658.75px
lg |Post Hero | 445.625px | 891.25px
xl |Post Hero | 353.125px | 706.25px

### MathJax

If you want to use [MathJax](https://www.mathjax.org/) in your posts, add `mathjax: true` in [YAML front matter](https://jekyllrb.com/docs/frontmatter/) of your post:

```yaml
---
layout: post
title: Blog Post with MathJax
featured-img: sleek # optional - if you want you can include name of hero image
mathjax: true # add this line in order to enable MathJax in the post
---
```

#### Example

In N-dimensional simplex noise, the squared kernel summation radius $r^2$ is $\frac 1 2$
for all values of N. This is because the edge length of the N-simplex $s = \sqrt {\frac {N} {N + 1}}$
divides out of the N-simplex height $h = s \sqrt {\frac {N + 1} {2N}}$.
The kerel summation radius $r$ is equal to the N-simplex height $h$.

$$ r = h = \sqrt{\frac {1} {2}} = \sqrt{\frac {N} {N+1}} \sqrt{\frac {N+1} {2N}} $$
Happy hacking-->
