## Plotting similarity matrix using Matplot lib
From my previous post of [How similar are neighborhoods of San Francisco] (https://kapilddatascience.wordpress.com/2016/03/04/how-similar-are-neighborhoods-of-san-francisco) , in this post I will briefly mention how to plot the similarity scores in the form of a matrix.

- **Data**: For this post, the plot is the similarity score of one neighborhood with another. In my data, there are 32 neighborhoods in the city of San Francisco. Each cell(i,j) in the matrix represent the similarity score of neighborhood i with neighborhood j. sim(i,j)

- **ColorSchemes**: I have plotted the similarity matrix in 4 different color schemes. Each color scheme on its right has it color map which represent the value of the cell and its color value.

**Note**: Similarity is one way to plot the matrix, you can this of this plotting as a general purpose solution to plotting distance between 2 cities, correlation between n variables etc

Here are the the 4 plots of San Francisco neighborhood similarity matrix.

### San Franscisco neighborhood similarity matrix.
 - `ColorSchemes`: `default`
 
 ![SF] (./pics/san_francisco_sim_matrix.png)
 

 - ColorSchemes: `Greens`
 	- All Greens 
 
 ![SF] (./pics/san_francisco_sim_matrix_greens.png)

 - ColorSchemes: `YlGnBu`
	 - Yelllow Green Blue
 
 ![SF] (./pics/san_francisco_sim_matrix_greens_yellow_green_blue.png)

 - `ColorSchemes`: `RdYlGn`
	 - Red Yellow Green
 
 ![SF] (./pics/san_francisco_sim_matrix_greens_red_yellow_green.png)




## How to plot the simialrity matrix. 
- Here is the snipet of code you will need to plot this matrix.	

	````python
	import matplotlib.pyplot as plt

    labels = []
    for hood in hood_menu_data:
        labels.append(hood["properties"]['NAME'])

    fig, ax = plt.subplots(figsize=(20,20))
    cax = ax.matshow(hood_cosine_matrix, interpolation='nearest')
    ax.grid(True)
    plt.title('San Francisco Similarity matrix')
    plt.xticks(range(33), labels,  rotation=90);
    plt.yticks(range(33), labels);
    fig.colorbar(cax, ticks=[0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, .75,.8,.85,.90,.95,1])
    plt.show()

	````
	
- The main components to note 
	- `matplotlib`: Ploting is done via `matplotlib`.
	- `matshow` : This function takes the input similarity matrix. Note this can also be a 		correlation matrix between `n` variables. 
	- `Grid`: enable the grid using `ax.grix(True)`
	- `labels.append `: You can add lables to the matrix by passingg the labels array to the 		xticks functions. 
	- `plt.xticks`: The lables arrays is passed to the xticks function along with the number of 		elements.
	- `rotation=90`: Note I have to rotate the x ticks to `90 degree` so that they are plotted 	vertically
	- `colorbar`: this is the color bar on the right of the matrix. This is used to plot teh gradeint of different colors. Red belongs to 1.0 and dark blue belongs to 0.
	- `ticks`: You can pass an array of values which will represents the values in the legends of the colorbar.


## Observation: 
 - The matrix is a diagonal matrix.
 - Seaclif has an almost blue line, which signies that its similarity is very less with all neighborhood. Astute readers will note that the similarity values are not normalized across neighborhoods. 
 - `Chinatown`: One can easliy interpret that the similarity of Chinatown is very similar to the boxes which are redish and orange. Conretely, `Chinatown` is most similart to `Inner richmond`, `Outer richmond`, `Outer sunset` etc.
 - There is a very high correlation (red, orange area) around `Outer mission`, `Outer Richmond`, `Outer sunset`

 
### ColorMaps: 
- Styles: `Sequential`, `Qualitative` etc
- You can also play around with color maps schemes do give different color schemes to the matrix. 
- Chose from a list of color map from [here] (http://matplotlib.org/examples/color/colormaps_reference.html)
- The change in code is very minimal
  
	````python

    from matplotlib import cm as cm

    cmap = cm.get_cmap('Greens')
    //cmap = cm.get_cmap('YlGnBu')
    //cmap = cm.get_cmap('RdYlGn')
    
    cax = ax.matshow(hood_cosine_matrix, interpolation='nearest', cmap=cmap)

	````
- Code:
	- `cm`: Import the color map library
	- `get_map`: Pick from a pre-defined list of color map schemes. 
	- `cmap`: Pass the color map variable as an argument to the matshow function 
 
	
## Related	
- There are other ways to plot similarity matrix. Here are few more examples:
	- Diagonal correlation matrix [Link] (http://stanford.edu/~mwaskom/software/seaborn/examples/many_pairwise_correlations.html)	
	
	
	
	
	
	