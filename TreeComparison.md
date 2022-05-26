# Comparing and updating phylogenetic trees
### Work in progress - please contact ejmctavish@ucmerced.edu with any issues or questions.


There any many ways to generate phylogenetic trees!  
All kinds of data, lots of analysis methods.
We've talked a lot about estimating trees.

**How can we contextualize our phylogenetic inferences in existing literature and taxonomy?**


In this tutorial we will walk through:
  * Standardizing taxon names
  * Getting existing trees for arbitrary sets of taxa
  * Visualizing conflict between estimates
  * Getting date estimates for nodes
  * Updating an existing phylogeny with new data

# The Open Tree of Life
The Open Tree of Life (https://opentreeoflife.github.io/) is a project that unties phylogenetic inferences and taxonomy to provide a synthetic estimate of species relationships across the entire tree of life.  
![](img/otol_logo.png)  


This tree currently includes 2.7 million tips.
65,000 of these taxa have relationships inferred from phylogeny.

The synthetic tree uses a combined taxonomy across a large number of taxonomy resources with evolutionary estimates from published phylogenetic studies.
https://opentreeoflife.github.io/browse/


## The synthetic tree

## Tree Browser

[https://tree.opentreeoflife.org](https://tree.opentreeoflife.org)
is our interactive tree viewer.
You can browse by  the synthetic tree and leave feedback.

### Navigation

Click on nodes to move through the tree.
If you click the "Legend" button at the top, you will get an explanation
    of what information the visual elements of the tree convey.

### Seeing more info about a node

You can reveal the "Properties panel" by clicking on "**ⓘ** Show Properties"
    button or the "**ⓘ**" link that appears when your mouse is over a node or
    branch in the tree.

The properties panel contains:

  * links to the taxon in our reference taxonomy (OTT) and other taxonomies
  * the ID of the node
  * the count of how many tips in the tree descend from the node
  * information about how to download a Newick representation of the subtree
      rooted at that node, and
  * information about taxonomies and phylogenies that support or disagree with
    that node. (e.g. https://tree.opentreeoflife.org/opentree/opentree10.4@mrcaott3089ott32977/Corytophaninae--Leiocephalus)

Clicking on "**ⓘ** Hide Properties" will hide the panel so that you can see more
    of the tree.

### Feedback

If you have feedback about the relationships that you see, use the "Add Comment" button.
Comments that are entered here are stored as issues in our
[feedback repository](https://github.com/OpenTreeOfLife/feedback/issues).


## Taxonomy Browser

[https://tree.opentreeoflife.org/taxonomy/browse](https://tree.opentreeoflife.org/taxonomy/browse?id=93302) is our browser for the Open Tree Taxonomy.
That taxonomy is an input into our full synthetic tree and
is used to help us align tips in different trees that refer to the same taxon.

The taxonomy includes links to unique identifiers in other digitally available taxonomies, such as GBIF or NCBI.


## Accessing data using the website
Check out https://tree.opentreeoflife.org

Search for your favorite organism!
Don't agree with the relationships? Never fear! We'll see how to fix them by uploading new inferences.


You can use the download a subtree of interest directly from the website.


## Getting a tree for your taxa

It is often more useful to access the pruned subtree for just the taxa you are interested in.
In order to do so, you need to map taxon names to unique identifiers.

Get the tutorial folder using
```
    git clone https://github.com/snacktavish/Mole2022.git
    cd  Mole2022/tutorial
```

The names of the taxa you included used in your tree estimation in Minh's lab are in the file
'species_names.txt'


One of the key challenges of comparing trees across studies is minor differences in names and naming.
We will map them to unique identifiers using the Open Tree TNRS bulk upload tool https://tree.opentreeoflife.org/curator/tnrs/


*Try this*
  * Click on "add names", and upload the names file. (tutorial/species_names.txt)  
  * In the mapping options section,
    - select 'animals' to narrow down the possibilities and speed up mapping
    - set it to replace '\_' with ' '
  * Click "Map selected names"

Exact matches will show up in green, and can be accepted by clicking "accept exact matches".

A few taxa still show several suggested names. Click through to the taxonomy, and select the one that you think is correct based on the phylogenetic context. (The tree is in the tutorial file as well if you want to double check).

Once you have accepted names for each of the taxa, click "save nameset". 

*Make sure your mappings were saved! If you don't 'accept' matches, they don't download.*

Download it to your laptop.
Extract the files.
Take a look at the human readable version (output/main.csv).


main.json contains the the same data in a more computer readable format.
Transfer the main.json file to the tutorial folder on the cluster.

### Using API's
You can use the OpenTree API's to get the tree for a subset of taxa directly from the command line

For example:
```
curl -X POST https://api.opentreeoflife.org/v3/tree_of_life/induced_subtree -H "content-type:application/json" -d '{"ott_ids":[292466, 267845, 316878]}'
```
For more on the OpenTree APIs see https://github.com/OpenTreeOfLife/germinator/wiki/Open-Tree-of-Life-Web-APIs


It is often more convenient to manipulate both trees and names within a scripting language.


### Using Python
We will use wrappers available in the python package [OpenTree](https://academic.oup.com/sysbio/article/70/6/1295/6273200) to make it easier to work with the Open Tree Api's.

This package is already installed on your virtual machine, but you can install it on your own machine either using 

```pip install opentree```

or by cloning and installing from https://github.com/OpenTreeOfLife/python-opentree


### Getting a subtree
Take a look at the script in the tutorials folder 'get_subtree.py'.

```
    $ python get_synth_subtree.py --input-file output/main.csv 

```

It will write two files out to your current working directory - the tree in newick format, 'synth_subtree.tre' and 'citations.txt' the citations of published trees that went into generating that tree, and support the relationships in it.

Move both those files to your computer.
Open the synthetic subtree in figtree to look at the placement of turtles.



## Comparing trees
Imagine that we want to get some more context for our inferences that we made when estimating tree.
How does the tree we estimated compare to taxonomy and other published literature including these taxa?

In order to make comparisons about statements that two different trees are making about the same set of taxa, we need to make sure the labels on the tree match.

I have generated a tree file for you 'turtle_iqtree_OTT.tre' from an IQTtree excercise from a previous year of this course (http://www.iqtree.org/workshop/molevol2019), and labelled it with standardized taxon name labels.

You can get a comparison tree from OpenTree using the mapped names file, tutorial/data/turtle_tree_names.csv
Instead of including the name and the ott id on each, tip, we will just download it with the names.

```
    $ python get_synth_subtree.py --input-file tutorial/data/turtle_tree_names.csv --label-format name --output turtle_synth
```

### Compare two trees visually
Open the synthetic tree (turtle_synth.tre) and the example inferred tree (turtle_iqtree_OTT.tre) in figtree.


**Q** Are the relationships in 'turtle_iqtree_OTT.tre' different than the relationships from OpenTree?

**Q** How so?


### Lets take a closer look!
You can also pass ott ids into `get_synth_subtree.py` directly. The ids for each taocn are listed in 'data/turtle_tree_names.csv'. 

For example, to see the relationships between python, 
<img src="img/python.jpg" alt="drawing" width="200"/>  

podarcis, 

<img src="img/podarcis.jpg" alt="drawing" width="200"/>  
and Anolis carolinenesis

<img src="img/anolis.jpg" alt="drawing" width="200"/>  


```
    $ python get_synth_subtree.py --ott-ids 970153 675102 937560  --output lizards
```

## DIY section:


- get a list of trees that have all 3 contentious taxa.




## Automated updating of an existing tree
There is a lot of sequence data that has been generated, but has never been incorporated into any phylogenetic estimates.



One it is done running, take a look at the output:

**Q)** What is the MRCA of the sampled taxa?

**Q)** How many new sequences were found?

**Q)** How many new taxa?



### Uploading your own tree to OpenTree for interactive comparison with the OpenTree synthetic tree and Taxonomy


I will do a demonstration of how to upload your inferred tree to OpenTree using the curator sites,
but if you want to try it out yourself later, there are detailed instructions at:
https://github.com/OpenTreeOfLife/opentree/wiki/Submitting-phylogenies-to-Open-Tree-of-Life

To upload your published trees, go to
https://tree.opentreeoflife.org/curator


##  Exercise
<img src="img/mastigias.jpg" alt="drawing" width="400"/>  

A student is studying jellyfish that live in Jellyfish Lake in Palau.   
Check out https://www.youtube.com/watch?v=DhpaqFya2pg for a cool video of them swimming around!
They are in genus 'Mastigias'. She needs to assemble a transcriptome, and wants to use an assembled reference genus.
There are genomic resources available in the genera Cassiopea, Aurelia and Rhopilema.  
*Which genome should they use to assemble their transcriptome?*


**Q)** What are the relationships among these taxa? Which taxon is most closely related to mastigias?

**Q)** What studies support this inference?

One of the genera got renamed! Why?
Look in the synthetic tree, to assess what is happened.  

**Q)**  Which genus?

**Q)**  What phylogenetically supported three-taxon relationship breaks up this genus?

**Q)**  Is there conflict between the phylogenetic studies that traverse this part of the tree?


## Choose your own adventure!
If there is time, try one of the ideas below.

### Get a synthetic tree
Make a list of taxa you are interested in and save it in a text file.
(Scientific names only)

Resolve those names to Open Tree identifiers, and modify `get_tree.py` to get a tree for your taxa of interest.


### Contribute to OpenTree

Take a look at the area of the synthetic tree that is interesting to you.

Do you have, or know of a published tree that would do a better job on those relationships, but it isn't included in the synthetic tree?

Upload it to the main website  https://tree.opentreeoflife.org/curator, and those phylogenetic inferences will be incorporated into later drafts of the synthetic tree!


### Update a different tree from OpenTree

There are some alignments in the alignments folder labelled as 'StudyIdTreeId.aln'.
Check out what the studies are on
By going to https://tree.opentreeoflife.org/curator/study/view/{StudyId}
*Replace StudyId with the id of the study you are interested in*

If any of them interest you, to try to scrape data for those taxa, by modifying data_scrape_alt

Upload your extended tree to https://devtree.opentreeoflife.org/curator
(requires a github login)


### Update your own alignment and  tree file!

If you have:
  * an alignment (single gene)
  * a tree

You can automatically update your own tree using physcraper.

Generate a name-mapping file using the Bulk TNRS.

then
follow the example and instructions in `own_data_scrape.py`

This doesn't work well currently for more that 50 taxa.

### Unifying geographic and phylogenetic data using R/Rstudio
There is a great package, [Rotl](https://github.com/ropensci/rotl) that makes it easy to access and work with OpenTree data in R.

Try it out using either:
Tutorial on rotl at, https://ropensci.org/tutorials/rotl_tutorial/
Tutorial on linking data from OpenTree with species locations from GBIF,
https://mctavishlab.github.io/BIO144/labs/rotl-rgbif.html

<img src="img/rotlrgbif.png" alt="drawing" width="400"/>  


### Zoom around

Brain a bit tired? There are some fun visualizations of the OpenTree tree.

Take a look around OneZoom https://www.onezoom.org/ tree of life explorer

or this emoji hyperbolic tree https://glouwa.github.io/


---
