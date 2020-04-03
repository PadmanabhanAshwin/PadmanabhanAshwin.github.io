---
layout: post
title: Genetic Algorithms
featured-img: evolution
mathjax: true
---

# Overview to Genetic Algorithms:
Genetic Algorithms (GA), in the context of Computer Science and Operations Research, is an optimization heuristic algorithm inspired from evolution as seen in nature. Genetic algorithms belong to a broader class of Evolutionary Algorithms (EA). 


# Inpiration and Drawing Parallels
GAs are inspired from the natural process of evolution as seen in nature. Species, over generations, are more "evolved"    (better) than their predecessors. Getting better, after all in essense is optimization. 

We will draw parallels between what is seen in nature and how it is translated in the GAs as we go on. "Generations" in nature as viewed as "iterations" in a GAs. That is, with each passing iteration of the GA, our best solution is expected to improve, atleast on average. 

In natural selection, two individuals of a species mate to give birth to progeny which have a combination of the genetic encoding of their parents. The genetic infomation of an individual is called the "genotype". A given "genotype" results in a "phenotype", which is simply a physical charateristic of the individual. Perhaps having a specific genotype results in an individual being taller (a physical characteristic, hence the "phenotype"), or have dark hair. When this is translated to the computational world of GAs, the genotype is a "potential solution" and the phenotype is the "measure of interest", or fitness, which we are optimizing for.  

To keep things in perspective, we will use the example of the classical Knapsack Problem, and potentially work towards using GAs to solve the Knapsack problem. 

---
### Quick Detour: Knapsack Problem Definition

**The Knapsack problem:** Say you are Indiana Jones (a famous, fictional archaeologist) and you are find a treasure chest. It has five things; a heavy gold locket, an old diary, a ring, a golden cup and a tiara, and no they are not horcruxes. Each of these items have a weight (in kgs) $ W_1, W_2, .. , W_5 $ and a value (in dollars) $ V_1, V_2, .. , V_5 $. 

However Indiana Jones has just one bag, which can only take a total weight of say, $W$ kgs. He has to decide which of these items he must choose so that the combined weight of the selected items is lesser than $W$ kgs but has the most value. This is called the Knapsack problem: Given a bunch of items to choose from, you want to choose the best subset which results in the most value.  

---

For the Knapsack Problem, one possible genotype representation could be a $[1X5]$ vector. A genotype (recall is a "possible solution") with value $ [1,0,0,0,1] $ would mean Indiana Jones decided to place the Gold Locket and the Tiara into his bag and left eveything else. The phenotype (or the fitness, which we want to optimize) in this case would be $ V_1 + V_5 $. 

In the preceding paragraph, we spoke about how the child has a *combination* of the genotype of it's parents. This is called "Cross-Over". Also, in nature, some genotypes sites undergo "mutation", a random change of genetic material at a given genetic site. These translate almost directly in GAs. 

# Componenets in a GA: 
Most GAs consist of the following componenets: 

* Representation: How a given solution is represented. In the Knapsack Problem above, it was represented as a $[1X5]$ vector. Each vector is a possible solution. 
* Initial Population: The set of solutions you start with. This is usually just randomly generated. Imagine 10 of these 
 $[1X5]$ vectors with zeros and ones in random indices. 
 * Selection: The process of selecting parents from the population used to generate progeny. This is where a gentle pressure (called selection pressure) is applied to make sure you move towards better solutions. There are many types of selection, but the usual premise is, individuals in the population which have higher fitness have higher probability to be selected to be parents. Commong selection methods are "Tournament Selection" or "Roulette-Wheel Selection". 
 * Cross-Over: Once the parents are selected, we need to generate the child. Say Parent 1 has genotype $[ P_{11}, P_{12}, .. , P_{15} ]$ and Parent 2 has genotype $[ P_{21}, P_{22}, .. , P_{25} ]$, one way to generate the child would be to use genotypes from index 0 to 3 from parent 1 and use genotypes from index 4, 5 to create the child. Therefore the child would have genotype $[ P_{11}, P_{12}, P{13}, P{24}, P_{25} ]$. This is called the one-fold cross-over. 
 * Mutation: With a given probability, a random index of the genotype is changes. For example, if the child has genotype 
$[ P_{11}, P_{12}, P{13}, P{24}, P_{25} ]$, one position, randomly selected is flipped (if its a 0, it's flippled to 1 or vice versa). 


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
