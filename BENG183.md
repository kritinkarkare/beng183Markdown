#  Dendrograms
This BENG 183 chapter introduces dendrograms, the use of them, several examples, and things to think about when creating them. It was created by Kritin Karkare. 
**Table of Contents**
1. [Introduction](#intro)
2. [Hierarchical Clustering](#hcluster)
	2.1. [Brief Introduction to Hierarchical Clustering](#introHC)
	2.2. [Dendrograms](#dendro)
3. [Data](#data)
	3.1. [Phylogenetic](#phylo)
	3.2. [mRNA Expression](#mRNA)
	3.3. [Unknown Data](#unknown)
4. [Considerations](#consider)
	4.1. [Questions to ask yourself](#quest) 
	4.2. [Dendrogram-Specific](#dendrogr)
5. [Resources](#resources)
6. [Sources](#sources)


##  Part 1. Motivations <a name="intro"></a>
**Data visualization** is a key point of entry for anyone looking to understand your results and as you might realize, you've probably seen plenty of poorly done visualizations from other scientists. Understanding how to properly relay your figures to others without them brushing your paper or presentation aside because they were confused is an **important** skill. 

>It matters, and keeping in with the adage "Pictures are worth an 1000 words", data visualizations are worth, what, 1000 **numbers**? Probably more. 

In particular, we're going to take a look at using **dendrograms** to visualize hierarchical relationships between data, and at two bioinformatics related ones. We can infer relationships between classes of cancers and how dissimilar families of points are from each other. 

>**Takeaway**: Dendrograms can be produced to look at insights into probably (**not certain**) classes for your data, as well as divergence of your data points. 

> [Intro](#intro) | [HClustering](#hcluster) | [Data](#data) | [Considerations](#consider) | [Resources](#resources) 
##  Part 2. Hierarchical Clustering and the Dendrogram <a name="hcluster"></a>


### Introduction to Hierarchical Clustering <a name ="introHC"></a>
What is _Hierarchical Clustering_? It's creating a hierarchy of data point groups based on the similarity between points and between groups. 

1. First it calculates distance between points using similarity methods and then merges points into clusters. 
2. Following that it calculates linkage numbers for the
cluster based on the members and merges clusters together from then on and out until one big cluster is created. 

For more detailed information on the Hierarchical Clustering algorithm, look down at the [resources](#resources).

Calculating similarity is entirely dependent on **your** method of choice, and that can change the output of the graph. There's really not enough space for going into much detail about both linkage and similarity methods, so I've put a small table here.  

 **Three Similarity Calculating Methods**
*Euclidean Distance*
$\sqrt{\sum _{j=1}^k (a_{j}-b_{j})^2}$

*Correlation*

$\frac{cov(a, b)}{std(a)*std(b)}$

$cov(a, b)  = \frac{1}{k}\sum_{j=1}^	k (a_{j} -\bar{a})\cdot(b_{j}-\bar{b})$

$std(a)  = \sqrt{\frac{1}{k}\sum_{j=1}^k (a_{j}-\bar{a})^2}$

$\bar{a} = \frac{1}{k}\sum_{j=1}^k	a_{j}$

*Cosine Correlation*

$\frac{\frac{1}{k}\sum_{j=1}^	k a_{j}\cdot b_{j}}{norm(a)\cdot norm(b)}$

$norm(a)  = \sqrt{\frac{1}{k}\sum_{j=1}^k a_{j}^2}$



**Linkage Methods**
<table>
 <tbody>
    <tr>
        <th>Method</td>
        <th>What It Does</td>
        <th>Notes</td>
    </tr>
    <tr>
        <td>Single Linkage </td>
        <td>Dissimilarity between two groups is equal to minimum dissimilarity between one member of a cluster and a member of another cluster</td>
        <td>Produces clusters in large chains and easily identify outliers<br></td>
    </tr>
    <tr>
    <td>Complete Linkage </td>
    <td>Dissimilarity between two groups is equal to greatest dissimilarity between one member of a cluster and a member of another cluster.</td>
    <td>Produces very tight clusters</td>
    </tr>
    <tr>
    <td>UPGMA </td>
    <td>Average Distance calculated from each point in a cluster to all other points in another cluster.</td>
    <td></td>
    </tr>
 </tbody>
</table>

> Note: These notes are taken from the BENG 183 Lecture 5: Machine Learning slides. They also involved me using latex a little more than I wanted to...

### Dendrograms <a name="dendro"></a>

After using hierarchical clustering, we get a nice figure known as the dendrogram!
A simple one taken from online: 


**![](https://lh4.googleusercontent.com/faEPNHKgNWvyCY-lvZOwhNIYzN2i9xlw55vDopk_UqQURTo8cUxJ3x30IuJdUx2fjGtM02_fo9Zhf4mhHd9fNshsH7nD378EdJLP_9CzMfUoaL8kwadZnnHZaOmDjQUwBTHuF6M)**
Source: https://www.mathworks.com/help/stats/dendrogram.html 

This one looks kinda boring, but it's a simple dendrogram produced by MatLab. Many programming languages that deal in data analysis and visualization like Matlab, Python, and R all have packages and libraries that let you create dendrograms. See the [resources](#resources) section if you're interested. 

To get a little more specific, we use dendrograms to understand the **dissimilarity** between clusters, and it's pretty easy to interpret! The x-axis of the dendrogram shows the data points, and the y-axis the measured distance between clusters. In the top picture, we can see that the cluster of (2, 10) is distant to the cluster of (5, 8, 9) by about 0.5 (pretty far!) but compared to the cluster (3, 6, 7) the dissimilarity is much larger!  

But what about insights can we learn from visualizing biological data in a dendrogram...?  
>**Takeaway**: Depending on what linkage and similarity calculating method you choose, your dendrogram looks different! You may need to do several iterations of hierarchical clustering. 
> [Intro](#intro) | [HClustering](#hcluster) | [Data](#data) | [Considerations](#consider) | [Resources](#resources) 
##  Part 3. Data <a name="data"></a>
Because dendrograms are used for looking at hierarchical data, we can examine the relationship that certain biological members have with each other using hierarchical clustering. 
 
### Phylogenetic Data <a name="data"></a>
You've heard of phylogeny trees right? It's something you've probably seen in your biology class from time to time. 

**![](https://lh3.googleusercontent.com/wE8EYru-o6ntFhlbiCsN5HqWgxwv4vzmIJ0m3NsPexp_FdghXFUDdto3r8-sBF2W_QPfxcfvD2A471Bl5Hz-ULAYhgsjoZrmekrx0PwA-VlApP6vuh9ifT82fP6jatKJBYb4GLo)**

This is one example of one I pulled from a paper categorizing bacterial species. Methods like UPGMA to calculate genetic distance from multiple sequence alignments will produce an tree with inferred relationships between species. Because the dendrogram shows us how **dissimilar** species can be, perhaps you can infer where there might be divergent evolution? 

A simple example is understanding how the dog (or domesticated canine) is within the same family as the wolf, but not the same family as felines.  

### mRNA Expression Profiling <a name="data"></a>
Our second example is taken from a research study that used hierarchical clustering to find classes of human lung carcinoma. Using mRNA expression profiling, they were able to look at what genes were expressed highly for different cancers and group them into clusters based on how high they were. In particular, we can see that at the top of the picture, five classes, all highlighted in different colors, have potentially been found. 
**![](https://lh5.googleusercontent.com/9vGQI6iTn92Pz0gE2V6_9zFA2hugnOkS7ebNBtInU0crCY6u32dCPZ44ZsCQ_OTw0R52Gl0fV2xQ2tVmQifXyLqGLkYIUOx6_VoQuzdfYs9SOnJBRYwNyYDfp2YiCsdl8YZUUPMyHQ)**
In addition, to show visually how high (or low) genes were expressed, the researchers included a heatmap with an expression index detailing that the red squares are highly expressed genes and green ones are not. How easy is it to tell which groups of genes to look for? 

An insight from the researchers:
>"Hierarchical Clustering methods offer a powerful approach to class discovery, but provide no means of determining confidence for the classes discovered. 
Combining bootstrap probabilistic clustering with HC allowed us to measure the strength of sample-sample association."
[1]

### My Data Doesn't Fit! <a name="unknown"></a>
I unfortunately can't solve your problem, but I suggest to look up data figures related to your research field - it's possible dendrograms aren't the thing for you! But that's why there are so many kinds of visualizations out there....
> __Takeaway__: Dendrograms are very useful for visualizing data that has relationships with each other and in groups with each other, such as subclasses of cancer and species that are closely related. Where does your data fall into?
> [Intro](#intro) | [HClustering](#hcluster) | [Data](#data) | [Considerations](#consider) | [Resources](#resources) 
## Part 4. Important Considerations  <a name="consider"></a>

When creating your visualization ask yourself these basic questions. 
**Questions** <a name = "quest"> </a>
```
 - Who are you targeting? 
	1.1. Academia, in your field 
	1.2. Academia, not in your field 
	1.3. Public 
 -  Where is this appearing? 
	2.1. Academic Journal
	2.2. Presentation  	
 -  What's the immediate takeaway people should have?
 -  How does the text help the visualization? 
 - Have others looked at your data visualization for you? 
```
### A few more points for dendrograms: <a name="dendrogr"></a>

 - Do **not** assume that you automatically learn the amount of clusters or what clusters there are from the dendrogram.  
 - When looking at them, look at differences in similarity between clusters. By setting a threshold line that goes straight through the tops of the clusters, you can detail  
 - Have possible clusters to highlight? **Color** them for your figure!    
 - The mRNA gene expression dendrogram study did **two** important things: 
	 - "used a model based probabilistic clustering method to reduce classification bias due to choice of clustering method" using bootstrap datasets.  
	 - Added the heatmap (with corresponding genes at the right) to show using colors how genes can be clustered together. Keep this in mind if your data can take advantage of a heatmap.


>**Takeaway**: Remember to figure out what your audience probably knows. In a few sentences, can you convey what your dendrogram is showing?

> [Intro](#intro) | [HClustering](#hcluster) | [Data](#data) | [Considerations](#consider) | [Resources](#resources) 
## Part 5. Resources <a name="resources"></a>
*Machine Learning (General)* 
1. General Machine Learning Information
https://www.geeksforgeeks.org/machine-learning/
2. Coursera - Machine Learning Course
     https://www.coursera.org/learn/machine-learning

*Hierarchical Clustering/Dendrograms*
1.  Hiearchical Clustering PDF
https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/Hierarchical_Clustering-Dendrograms.pdf
2. Bioinformatics Visualizing Techniques https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2957900/

*Coding Dendrograms*
1. Python - SciPy &https://docs.scipy.org/doc/scipy/reference/generated/scipy.cluster.hierarchy.dendrogram.html
2.  R - Dendrograms
 https://www.rdocumentation.org/packages/stats/versions/3.5.1/topics/dendrogram
> [Intro](#intro) | [HClustering](#hcluster) | [Data](#data) | [Considerations](#consider) | [Resources](#resources) 
## Part 6. Sources <a name="sources"></a>

Studies used:
1. Classification of Human Lung Carcinomas by mRNA expression Profiling
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC61120/
2. Rational Design of a Plasmid Origin 
 https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0013244

Miscellaneous:

1. https://www.mathworks.com/help/stats/dendrogram.html
2. https://ncss-wpengine.netdna-ssl.com/wp-content/themes/ncss/pdf/Procedures/NCSS/Hierarchical_Clustering-Dendrograms.pdf
3. BENG 183 Lecture 5 (Machine Learning) Slides  
4.  Information Visualization Techniques in Bioinformatics in the Post Genomic Era
https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2957900/

Thanks for reading this chapter! If you have any suggestions or questions, please let me know. 

> Written with [StackEdit](https://stackedit.io/).
> This chapter has been edited and created by:
Kritin Karkare | kkarkare@eng.ucsd.edu
> Last updated on December 13th, 2018.
