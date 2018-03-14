# ocr-classify

#### preprocess.sh
**Usage:**  `./preprocess.sh ~/Downloads/document.pdf`  
**Purpose:** Does the following:
+ Takes a given PDF and runs it first through ghostscript to create PNGs of each page
+ Runs each PNG through tesseract to create HTML files
+ Creates a folder within the folder `docs` with the name of the pdf, and a folder for the tesseract and png output.
+ Moves the original document to the new folder and renames it `orig.pdf`
+ Runs the output of tesseract through the script `summarize.py`

#### process.sh
**Purpose:** Runs tesseract on a given page. Not called directed - used by `preprocess.sh`  

#### summarize.py
Takes a given document, generates document-wide statistics with `helpers.summarize_document`, and runs each area in the document
through all the labeling functions in `heuristics.py`. The result is stored in the postgres table `areas`.

#### heuristics.py
Labeling functions for areas. Most return a boolean, and some an integer.

#### classifier.py
Generates a model using SciKit Learn that can b used to classify areas. Contains two methods:
+ `create()` - queries all areas and associated labels created by `tagger` and returns a model
+ `classify(<pages>, <doc_stats>)` - Takes as an input a list of pages and document-level stats generated by `helpers.summarize_document`
and returns those pages with all areas tagged with their classification.

#### setup.md
Contains `brew` installs for dependencies as well as the `CREATE TABLE` statements needed for Postgres

#### tagger
A flask application that runs on port 5555 that can be used for creating training data.
