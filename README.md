## bibcheck.py

bibcheck.py is a simple python class / command line utility for doing automated 
literature searches using Google Scholar. It takes as input a BibTeX bibliography 
(.bib) file and outputs a list of papers that might be relevant to the papers 
contained in the bibliography.

When used from the command line, the program returns a list of suggested papers, and
the number of references each suggested paper has in common with the supplied bibliography.

The recommendations are generated by [triangle closing](https://en.wikipedia.org/wiki/Triadic_closure). 
Briefly, we consider each paper in the bibliography as a node in a network, 
along with the bibliography itself. Edges connect the bibliography to each 
paper it cites. Each cited paper also has edges connecting it to other papers 
which include it in their bibliographies. To make recommendations, the code 
searches for other nodes, which when connected to the node of the supplied 
bibliography, would create multiple triangles in the network. 

A more intuitive example: if you know Alice, Bob, Carol, and Dan, and Elise 
knows Bob, Carol, and Dan, then you are likely to know Elise. A hypothetical 
link between you and Elise would create three new triangles: (you, Bob, Elise), 
(you, Carol, Elise), and (you, Dan, Elise). 

bibcheck.py is built on top of a couple great python libraries; thanks to 
@ckreibich for building 
[scholar.py](https://github.com/ckreibich/scholar.py) and to 
@sciunto for building 
[python-bibtexparser](https://github.com/sciunto/python-bibtexparser).

### Usage
    usage: bibcheck.py [-h] [-o outfile] [-c cookie-file] [-r N] bibfile

    Search Google Scholar for papers which share references with a supplied BibTeX
    bibliography file.

    positional arguments:
      bibfile         the BibTeX (.bib) file to be parsed

    optional arguments:
      -h, --help      show this help message and exit
      -o outfile      save output to file
      -c cookie-file  use cookies stored in file
      -r N, --rmax N  specify max number of references per article

All the options should be self-explanatory, except for `rmax` which is used to 
filter out review articles with hundreds / thousands of citations. Any article 
in the supplied bibliography with more than `rmax` citations will be excluded 
from the search.

### Troubleshooting
#####If you run into

    Trying to get cluster ID for article N/N
    Error: failed to pull Google Scholar cluster IDs. Probably running into a captcha.
  
try supplying a cookie file. To generate a valid cookie:

- open Google Scholar in a browser
- do a search, and solve the captcha
- view your browser's cookies via your favorite method
- copy 'value' and 'expiration' from the 'GSP' cookie into the supplied notarobot.cookie file


#####If you run into

    (any error that that originates in scholar.py)
make sure you're using the [bibcheck](https://github.com/hinnefe2/scholar.py/tree/bibcheck) branch of hinnefe2/scholar.py.
