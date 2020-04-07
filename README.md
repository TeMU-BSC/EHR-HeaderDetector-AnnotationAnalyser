# EHR-HeaderDetector-AnnotationAnalyser: Electronic Health Record (EHR) Header Detector and Annotation Analyser #

## Digital Object Identifier (DOI)

## Introduction

This project is about (pre-)annotation of section headers in EHR. It includes different scripts to: 
(i) annotate and normalize section headers in EHR, 
(ii) generate statistical analysis on the annotated files. 

These scripts were used to identify and normalize section headers in discharge reports. 
The generated annotations can be loaded in the BRAT tool.


## Prerequisites
In Linux
<pre>
$ sudo apt-get install python3-dev python3-pip git
</pre>

For header_detector script
<pre>
$  pip3 install unidecode
</pre>

For analysis_annotatedHeaders script
<pre>
$ pip3 install numpy
$ pip3 install pandas
$ pip3 install matplotlib
</pre>


## Directory structure

- [**`scripts/`**](scripts/): 
This folder contains the scripts needed to detect headers, calculate statistical analysis of headers 
and comparing different annotations records (different annotators have done that) for the same file
and finally comparing the manually annotated files with pre-annotated files by [SpaCTeS tool](https://github.com/siabar/SpaCTeS).

  - [**`header_detector.py`**](scripts/header_detector.py): Annotate and normalize section headers in EHR. 
    For detecting the section headers, we need the list of headers that is available of [data](data/) directory. 
    Input is in [TXT](documents/TXT) directory and 
    output is in [XML_SECTION](documents/SELECTED_XML) directory and [ANN_SECTION](documents/ANN_SECTION) Directory.

  - [**`analysis_annotatedHeaders.py`**](scripts/analysis_annotatedHeaders.py): Generate statistical analysis on the annotated files (output of header_detector.py).
    Input is [XML files](documents/SELECTED_XML) and output is CSV and PLOT in analysis_headers directory.
    The script creates analysis_headers folder for output files.
    
- [**`data/`**](data/)
This folder contains relevant information for annotating and normalizing section headers in EHR:

  - **headers.txt**: This file contains a list of allowed headers for your EHRs. 
	The normalizer tries to match detected header candidates to this list.
	This file has three columns that have been separated by TAB ("\t"):
	<pre>
	"ARQUETYPE"[TAB]"HEADER"[TAB]"HEADER VARIATION" 
	</pre>
  - **importat_headers.txt**: This file contains a list of necessary headers that EHRs should have them (doing statistical analysis just on these files).
    <pre>
    Each line one Header
    </pre>
- [**`documents/`**](documents/)
  - [**`txt/`**](documents/txt/): It is text files directory, this folder can have several sub-directories for each annotator
    and each annotator must have a sub-directory for each bunch.

    Example of format of TXT directory (All other directories in documents also follow this format):
    ```
    - TXT/
      - Annotator1/
        - 01/
          - file1.txt
          - file2.txt
          - ...
        - 02/
          - file4.txt
          - file5.txt
          - ...
        - ...

      - Annotator2/
        - 01/
          - file3.txt
          - file2.txt
          - ...
        - 02/
          - file6.txt
          - file7.txt
          - ...
        - ...
      - ...
    ```

  - [**`XML_SECTION/`**](documents/SELECTED_XML/): Output of header_detector.py script.
  - [**`ANN_SECTION/`**](documents/ANN_SECTION/): Output of header_detector.py script.
  - [**`SELECTED_XML/`**](documents/SELECTED_XML/): Output of analysis_annotatedHeaders.py script when --filter option is applied.


- [**`analysis_headers/`**](analysis_headers/)
"analysis_annotatedHeaders" script creates this folder.
And it contains the results (Plot and CVS) of statistical analyses based on the detected headers.
  -  [**`PLOT/`**](analysis_headers/PLOT/):
    Showing how many headers have been detected on the given corpus.
  -  [**`CSV/`**](analysis_headers/CSV/):
      ```
      {Bunch-Number}_analysis_files.csv: Showing all detected headers in each text file. 
      {Bunch-Number}_analysis_headers.csv: Showing all files that contain a specific header. 
      {Bunch-Number}_analysis_header_co-occurrences.csv: It is a matrix which shows how many times two headers co-occurrence.
      {Bunch-Number}_top_10_header_co-occurrences.csv: Showing top 10 co-occurrences headers.
      {Bunch-Number}_analysis_original_headers_in_report.csv: Showing the original section in the EHR that script detects it as a header (By similarity method).
      ```



## Usage

**To annotate and normalize section headers in EHR, use the following command:**

    $ python3 header_detector.py --set NUMBER

<pre>
--set       (Mandatory) Number of bunch [For example: 01] 
</pre>


**To Generate a statistical analysis on the annotated files, use the following command:**

    $ python3 analysis_annotatedHeaders.py --set NUMBER [options] 

Options:
<pre>
--filter, -f    Filter files that have all headers at important_headers.txt (in the data directory)
                and move the filtered files into 'SELECTED_XML' directory 
--strict, -s    Analysis headers at important_headers.txt 
            
--set           Number of the bunch [For example 01] (Mandatory)
</pre>

## Examples

**To generate a statistical analysis:**
<pre>
$ python3 header_detector.py --set 01
</pre>

**To annotate and normalize section headers:**
<pre>
$ python3 analysis_annotatedHeaders.py --set 01
</pre>

- Analysis headers of all files in bunch 01.


**To annotate and normalize section headers with filter option:**
<pre>
$ python3 analysis_annotatedHeaders.py --set 01 --filter
</pre>
- First, filter the files in bunch 01 that have all headers at important_headers.txt.
- Move the filtered files into the "SELECTED_XML" directory 
- Then analysis headers of filtered files.



**To annotate and normalize section headers with filter and strict options:**
<pre>
$ python3 analysis_annotatedHeaders.py --set 01 --filter --strict
</pre>

- First, filter the files in bunch 01 that have all headers at important_headers.txt and move the selected files into the "SELECTED_XML" directory 
- Then analysis headers in important_headers.txt of filtered files.

**To annotate and normalize section headers with  strict option:**
<pre>
$ python3 analysis_annotatedHeaders.py --set 01 --strict
</pre>

- Analysis headers in important_headers.txt of all files in bunch 01.

## Contact

Siamak Barzegar (siamak.barzegar@bsc.es)

## License
MIT License

Copyright (c) 2020 Text Mining Unit at BSC

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
