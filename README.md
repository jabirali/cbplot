# Crystal Ball plotter
[Oracle Crystal Ball][ocb] is a tool for performing Monte Carlo simulations
on a model formulated as an Excel workbook. This can be useful if you already
have a model in Excel, and want an uncertainty analysis performed using that.

However, the output from those Monte Carlo simulations are in the form of Excel
sheets, and the plots generated by the GUI are mostly not of publication quality.
This repository contains two scripts `cbplot.py` and `cbsens.py`, which try to
remedy that: They take Crystall Ball output files as their inputs, and plot the
statistical distributions and sensitivity analyses using Python and Matplotlib.
This makes the process of generating higher-quality output plots for a large
number of simulations more streamlined, and produces plots that look like this:

![Distribution plot][dist]
![Sensitivity plot][sens]

In addition, the scripts output statistical information and sensitivity data as
plaintext `*.dat` files, which are often easier to work with than Excel.

[dist]: distribution.png
[sens]: sensitivity.png
[ocb]: https://www.oracle.com/applications/crystalball/

## Installation
The scripts require that you have installed Python 3 with the libraries `numpy`,
`scipy`, `pandas`, `matplotlib`, and `seaborn`. If you use a Linux distribution
like Ubuntu, all of these can be installed from the system package manager:

    sudo apt install python3-numpy python3-pandas python3-matplotlib python3-seaborn
    
If you use any platform that uses [Anaconda][conda], you can similarly run:

    conda install numpy pandas matplotlib seaborn
    
Alternatively, all libraries can be installed using `pip` if you prefer that.
    
[conda]: https://www.anaconda.com/distribution/
    
## Usage
The programs can be fed the name of a Crystal Ball forecast, as
well as a number of Excel workbooks, as command-line arguments.
It will then generate histograms from the Monte Carlo simulation
data recorded in each workbook and plot the results together.
Statistical information about the results is also extracted and
written to dat-files with similar names as the Excel workbooks.

In general, the invocation of these scripts will then look like:

    ./cbplot.py FORECAST FILE1 [FILE2 [...]]
    ./cbsens.py FORECAST FILE1 [FILE2 [...]]

For example, if you have `A.xlsx` and `B.xlsx` in the same directory 
as the scripts, and want to plot data for a forecast "Result", run:

    ./cbplot.py "Result" "A.xlsx" "B.xlsx"
    ./cbsens.py "Result" "A.xlsx" "B.xlsx"

The surrounding double quotes are in this case superfluous, but are
required if the provided forecast name or filenames include spaces.
Note that there are some tweakable parameters at the top of the source 
code, including the plot type (`plot`) and range (`dmin` and `dmax`).

## Assumptions
- Each workbook describes a *different* set of simulations. In the
  plot legends, the workbook filename is used as the default label.
- Each worksheet in a workbook describes the *same* set of simulations.
  In other words, if you perform multiple rounds of simulations to
  gather data for a certain setup, you can copy the result sheets
  into a single workbook and this script will recognize it as so.
- All worksheets in all workbooks have been generated using the
  function "Extract Data" in the Crystal Ball ribbon. When that
  is performed, remember to mark that you also want to export the
  "TrialValues" — without those, this script will not function.
