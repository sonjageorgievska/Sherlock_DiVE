
# Preamble

This is a version of [DiVE](https://github.com/NLeSC/DiVE) customized for the Sherlock project Clustering Image Noise patterns.

To upload different image datasets, go to https://sherlock-clustering.github.io/Sherlock_DiVE/.

## Datasets 

Embedded datasets from the Dresden Image Database accompanied with ground truth (source camera) and with cluster labels can be found in https://figshare.com/articles/data_zip/5016965 . Download, extract and upload each json file via the 'Upload local json data file' section. To color them based on label go to the last section of the UI.
 
Please cite the software if you are using it in your scientific publication:

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.581175.svg)](https://doi.org/10.5281/zenodo.581175)



[![Codacy Badge](https://api.codacy.com/project/badge/Grade/9ba82068db534a19b0d70dd80c8238bd)](https://www.codacy.com/app/sonjageorgievska/DiVE?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=NLeSC/DiVE&amp;utm_campaign=Badge_Grade)

# (Sherlock)DiVE   -  Interactive Visualization of Embedded Data

 
(Sherlock)DiVE is an interactive 3D web viewer of up to million points on one screen that represent data. It is meant to provide interaction for viewing high-dimensional data that has been previously [embedded](https://en.wikipedia.org/wiki/Nonlinear_dimensionality_reduction) in 3D. For embedding (non-linear dimensionality reduction, or manifold learning) we recommend [LargeVis](http://github.com/sonjageorgievska/LargeVis/) (a new algorithm by Microsoft Research, ) or [tSNE](https://github.com/lvdmaaten/bhtsne).       

## Installation - for users ##

The simplest way is to download the code and open *index.html* with your browser. Try it by uploading datasets from the *data* folder. The application can work completely offline.    

To use it with a local http server:

1. Open your command line interpreter (CLI)
2. Clone this repository
3. Go to the main folder of *DiVE* in your CLI (where *index.htm*l is)
4. Install *node.js* server together with the node package manager *npm* from (https://www.npmjs.com/get-npm).
5. Type `npm install connect serve-static`
6. Type `node server.js` 
7. Open your browser and type `http://localhost:8082/index.html` 

## Build - for developers ##

1. Clone this repository
1. Install [node](http://nodejs.org/), [npm](https://www.npmjs.org/), and [grunt-cli](https://www.npmjs.org/package/grunt-cli)
1. Run `npm install` to install all the build requirements
1. Run `grunt` to build. The resulting compiled JavaScript will be in `dist/` and the docs will be in `doc/`



## Data description and functionality ##

* Every point has 3 coordinates and a unique ID. (For a best view, the absolute values of the coordinates should be smaller than 1. When using LargeVis with similarities (weights) as input, this can be achieved by re-scaling the similarities to be smaller than 1.) 
 
* A point also has `Properties`:
   
  - `Properties` is a list of strings which can be empty. Each string  represents the value of a respective property. These values are used in the Coloring section of the UI of the web-page. When the user selects a property, each point is colored in a color representing the value of the property.
* A node can also have an image associated to it, see the Data format section for more info.

## User interaction ##
### Search ###
* A user can search for all points that contain a certain substring in their ids, names or properties, by using the *Search* section. Then all points that are a match become red, and the rest become grey. One can search also for boolean expressions of regular expressions. An example of a boolean expression is `xx AND yy OR NOT zz`, where xx, yy, and zz are regular expressions and NOT binds more than AND, which binds more than OR. In this case all points that contain in their metadata the regular espressions xx and yy, or that do not contain zz, will be coloured in red. 

* *Show only found nodes* will show only the nodes that result from the search.
  
* The *Resume colors* button at the bottom returns the colors of the points to the previous coloring scheme. 

### Visualization options ###

* *See all data* : will zoom-out such that all data is visible
* *Scase point size*: very useful when the user has zoomed-in enough. When this option is not selected, the points do not get bigger as the camera moves closer to them, so that they can be separated and inspected individually. 


### Coloring by value of property###

As explained in section **Data description and functionality** .

## Data format ##

- The data is in a JSON (JavaScript Object Notation)  format. (See folder *data* for examples.)
To obtain *data.js*, first a data structure

		Dictionary<string, Point>

is created in any programming language, where the keys are the id’s of the points and `Point` is an object of the class 
  
		public class Point
		    {
		        public List<double> Coordinates;
		        public List<double> Properties;
		    }

`Coordinates` and `Properties` are as discussed in the previous section.

Next, the dictionary is serialized using JavaScriptSerializer and written in *data.json* (name is flexible). 

Optionally, if data has properties, the dictionary should also contain an entry 
```json
		"NamesOfProperties":["name1", "name2", ..., "name_n"]
```

Optionally, if images are associated to the nodes, the node image can be displayed in a pop-up when hovering over the node. 
If the datafile starts with `namedataset_` then the folder with images should be `images_namedataset` in folder `data`. 
(see examples in folder `data`, sorry for their size). The name of an image should be `nodeId.jpg`.

If your images have  a `.png` extension then the `fingerprints_namedataset` folder is an option, although it is currently made for the Sherlock purposes.

## From output of LargeVis to input of DiVE ##

The output of [LargeVis](http://github.com/sonjageorgievska/LargeVis/) is a text file - every line has the id of the point, and 3 coordinates (real numbers). Only the first line is an exception: it contains the number of points and the dimension. Here is an example:

		4271 3
		0 -33.729916 17.692684 17.466749
		1 -32.923210 17.249269 18.111458
		
It can be processed into an input of the viewer by using the python script "MakeVizDataWithProperMetaData.py" in the folder "scripts_prepareData". It is called with 
		
		python MakeVizDataWithProperMetaData.py -coord coordinatesFile -metadata metaDataFile -dir baseDir -np -namesOfPropertiesFile 
		
		
		
* `coordinatesFile`: the output file of LargeVis
* `metaData`: file containing meta information about data. Format: `[id] [metadata]`.  Format of metadata:  `"first_line" "second_line" "third_line"` (number of lines is not limited). Example line of `metadata`: `35 "A dog" "Age:2" "Color brown"`.
	
* `baseDir`: base directory to store output file

* `namesOfPropertiesFile`: A json file containing list of properties names. Ex: `["Height", "Weight", "Place of birth"]`. If file is omitted, its name should be `"No"`


## Licence ##
The software is released under the GPL2 licence.
[Contact](mailto:s.georgievska@esciencecenter.nl) the author if you would like a version with an Apache licence 



