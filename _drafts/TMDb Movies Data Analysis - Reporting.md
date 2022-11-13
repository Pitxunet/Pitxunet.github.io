# Project: TMDb Movies Data Analysis

## Table of Contents
<ul>
<li><a href="#intro">Introduction</a></li>
<li><a href="#wrangling">Data Wrangling</a></li>
<li><a href="#eda">Exploratory Data Analysis</a></li>
<li><a href="#conclusions">Conclusions</a></li>
</ul>

<a id='intro'></a>
## Introduction

### Dataset Description 


In this Notebook I'll be analyzing data regarding 10,000 movies. This data has been collected from The Movie Database.

The columns names of the dataset and their significance are the following:

id   : general id of the movie                  
imdb_id  : id assigned at imdb               
popularity : different from rating, it depends on votes and views of the day, release date, previous days score. It fluctuates frequently             
budget   : for producing the movie               
revenue: revenue of the movie                 
original_title: title of the movie          
cast :  main roles                  
homepage                
director                
tagline                 
keywords: several keywords related to the movie                
overview: sum-up of the movie's plot               
runtime : duration in minutes                 
genres                  
production_companies    
release_date: year, month and day of release            
vote_count: total vote count in TMDb            
vote_average: the mean of the votes received by the movie            
release_year: year of release          
budget_adj: updated budget in terms of 2010 dollars, accounting for inflation    
revenue_adj: updated in terms of 2010 dollars, accounting for inflation

### Questions for Analysis

I asked myself:

- Is there a correlation between good-ratings and other variables?
- Are some production companies better than others producing well rated movies?
- Does the size of the budget matter to get good ratings?
- Are sequels' ratings high as compared to the original movie?




```python
# importing packages
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
%matplotlib inline


```


```python
# Upgrade pandas to use dataframe.explode() function. 
!pip install --upgrade pandas==0.25.0
```

    Collecting pandas==0.25.0
      Using cached pandas-0.25.0.tar.gz (12.6 MB)
    Requirement already satisfied: python-dateutil>=2.6.1 in c:\programmes\anaconda\anaconda3\lib\site-packages (from pandas==0.25.0) (2.8.2)
    Requirement already satisfied: pytz>=2017.2 in c:\programmes\anaconda\anaconda3\lib\site-packages (from pandas==0.25.0) (2021.3)
    Requirement already satisfied: numpy>=1.13.3 in c:\programmes\anaconda\anaconda3\lib\site-packages (from pandas==0.25.0) (1.20.3)
    Requirement already satisfied: six>=1.5 in c:\programmes\anaconda\anaconda3\lib\site-packages (from python-dateutil>=2.6.1->pandas==0.25.0) (1.16.0)
    Building wheels for collected packages: pandas
      Building wheel for pandas (setup.py): started
      Building wheel for pandas (setup.py): finished with status 'error'
      Running setup.py clean for pandas
    Failed to build pandas
    Installing collected packages: pandas
      Attempting uninstall: pandas
        Found existing installation: pandas 1.3.5
        Uninstalling pandas-1.3.5:
          Successfully uninstalled pandas-1.3.5
        Running setup.py install for pandas: started
        Running setup.py install for pandas: still running...
        Running setup.py install for pandas: finished with status 'error'

    Le chemin d'accŠs sp‚cifi‚ est introuvable.
      ERROR: Command errored out with exit status 1:
       command: 'C:\Programmes\Anaconda\anaconda3\python.exe' -u -c 'import io, os, sys, setuptools, tokenize; sys.argv[0] = '"'"'C:\\Users\\Propriétaire\\AppData\\Local\\Temp\\pip-install-p46j6ptv\\pandas_c3ff3a5d39284da1abfd686d0ec46106\\setup.py'"'"'; __file__='"'"'C:\\Users\\Propriétaire\\AppData\\Local\\Temp\\pip-install-p46j6ptv\\pandas_c3ff3a5d39284da1abfd686d0ec46106\\setup.py'"'"';f = getattr(tokenize, '"'"'open'"'"', open)(__file__) if os.path.exists(__file__) else io.StringIO('"'"'from setuptools import setup; setup()'"'"');code = f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' bdist_wheel -d 'C:\Users\Propriétaire\AppData\Local\Temp\pip-wheel-mqtstcrq'
           cwd: C:\Users\Propriétaire\AppData\Local\Temp\pip-install-p46j6ptv\pandas_c3ff3a5d39284da1abfd686d0ec46106\
      Complete output (907 lines):
      running bdist_wheel
      running build
      running build_py
      creating build
      creating build\lib.win-amd64-3.8
      creating build\lib.win-amd64-3.8\pandas
      copying pandas\conftest.py -> build\lib.win-amd64-3.8\pandas
      copying pandas\testing.py -> build\lib.win-amd64-3.8\pandas
      copying pandas\_typing.py -> build\lib.win-amd64-3.8\pandas
      copying pandas\_version.py -> build\lib.win-amd64-3.8\pandas
      copying pandas\__init__.py -> build\lib.win-amd64-3.8\pandas
      creating build\lib.win-amd64-3.8\pandas\api
      copying pandas\api\__init__.py -> build\lib.win-amd64-3.8\pandas\api
      creating build\lib.win-amd64-3.8\pandas\arrays
      copying pandas\arrays\__init__.py -> build\lib.win-amd64-3.8\pandas\arrays
      creating build\lib.win-amd64-3.8\pandas\compat
      copying pandas\compat\chainmap.py -> build\lib.win-amd64-3.8\pandas\compat
      copying pandas\compat\pickle_compat.py -> build\lib.win-amd64-3.8\pandas\compat
      copying pandas\compat\_optional.py -> build\lib.win-amd64-3.8\pandas\compat
      copying pandas\compat\__init__.py -> build\lib.win-amd64-3.8\pandas\compat
      creating build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\accessor.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\algorithms.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\api.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\apply.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\base.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\common.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\config_init.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\frame.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\generic.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\index.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\indexers.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\indexing.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\missing.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\nanops.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\resample.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\series.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\sorting.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\strings.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\window.py -> build\lib.win-amd64-3.8\pandas\core
      copying pandas\core\__init__.py -> build\lib.win-amd64-3.8\pandas\core
      creating build\lib.win-amd64-3.8\pandas\errors
      copying pandas\errors\__init__.py -> build\lib.win-amd64-3.8\pandas\errors
      creating build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\api.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\clipboards.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\common.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\date_converters.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\feather_format.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\gbq.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\gcs.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\html.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\packers.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\parquet.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\parsers.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\pickle.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\pytables.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\s3.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\spss.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\sql.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\stata.py -> build\lib.win-amd64-3.8\pandas\io
      copying pandas\io\__init__.py -> build\lib.win-amd64-3.8\pandas\io
      creating build\lib.win-amd64-3.8\pandas\plotting
      copying pandas\plotting\_core.py -> build\lib.win-amd64-3.8\pandas\plotting
      copying pandas\plotting\_misc.py -> build\lib.win-amd64-3.8\pandas\plotting
      copying pandas\plotting\__init__.py -> build\lib.win-amd64-3.8\pandas\plotting
      creating build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_algos.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_base.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_common.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_compat.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_downstream.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_errors.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_expressions.py -> build\lib.win-amd64-3.8\pandas\tests
    

    
      Rolling back uninstall of pandas
      Moving to c:\programmes\anaconda\anaconda3\lib\site-packages\pandas-1.3.5.dist-info\
       from C:\Programmes\Anaconda\anaconda3\Lib\site-packages\~andas-1.3.5.dist-info
      Moving to c:\programmes\anaconda\anaconda3\lib\site-packages\pandas\
       from C:\Programmes\Anaconda\anaconda3\Lib\site-packages\~andas
    

      copying pandas\tests\test_join.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_lib.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_multilevel.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_nanops.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_optional_dependency.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_register_accessor.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_strings.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\test_take.py -> build\lib.win-amd64-3.8\pandas\tests
      copying pandas\tests\__init__.py -> build\lib.win-amd64-3.8\pandas\tests
      creating build\lib.win-amd64-3.8\pandas\tseries
      copying pandas\tseries\api.py -> build\lib.win-amd64-3.8\pandas\tseries
      copying pandas\tseries\converter.py -> build\lib.win-amd64-3.8\pandas\tseries
      copying pandas\tseries\frequencies.py -> build\lib.win-amd64-3.8\pandas\tseries
      copying pandas\tseries\holiday.py -> build\lib.win-amd64-3.8\pandas\tseries
      copying pandas\tseries\offsets.py -> build\lib.win-amd64-3.8\pandas\tseries
      copying pandas\tseries\plotting.py -> build\lib.win-amd64-3.8\pandas\tseries
      copying pandas\tseries\__init__.py -> build\lib.win-amd64-3.8\pandas\tseries
      creating build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\testing.py -> build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\_decorators.py -> build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\_depr_module.py -> build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\_doctools.py -> build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\_exceptions.py -> build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\_print_versions.py -> build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\_tester.py -> build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\_test_decorators.py -> build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\_validators.py -> build\lib.win-amd64-3.8\pandas\util
      copying pandas\util\__init__.py -> build\lib.win-amd64-3.8\pandas\util
      creating build\lib.win-amd64-3.8\pandas\_config
      copying pandas\_config\config.py -> build\lib.win-amd64-3.8\pandas\_config
      copying pandas\_config\dates.py -> build\lib.win-amd64-3.8\pandas\_config
      copying pandas\_config\display.py -> build\lib.win-amd64-3.8\pandas\_config
      copying pandas\_config\localization.py -> build\lib.win-amd64-3.8\pandas\_config
      copying pandas\_config\__init__.py -> build\lib.win-amd64-3.8\pandas\_config
      creating build\lib.win-amd64-3.8\pandas\_libs
      copying pandas\_libs\__init__.py -> build\lib.win-amd64-3.8\pandas\_libs
      creating build\lib.win-amd64-3.8\pandas\api\extensions
      copying pandas\api\extensions\__init__.py -> build\lib.win-amd64-3.8\pandas\api\extensions
      creating build\lib.win-amd64-3.8\pandas\api\types
      copying pandas\api\types\__init__.py -> build\lib.win-amd64-3.8\pandas\api\types
      creating build\lib.win-amd64-3.8\pandas\compat\numpy
      copying pandas\compat\numpy\function.py -> build\lib.win-amd64-3.8\pandas\compat\numpy
      copying pandas\compat\numpy\__init__.py -> build\lib.win-amd64-3.8\pandas\compat\numpy
      creating build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\array_.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\base.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\categorical.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\datetimelike.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\datetimes.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\integer.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\interval.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\numpy_.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\period.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\sparse.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\timedeltas.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\_ranges.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      copying pandas\core\arrays\__init__.py -> build\lib.win-amd64-3.8\pandas\core\arrays
      creating build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\align.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\api.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\check.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\common.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\engines.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\eval.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\expr.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\expressions.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\ops.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\pytables.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\scope.py -> build\lib.win-amd64-3.8\pandas\core\computation
      copying pandas\core\computation\__init__.py -> build\lib.win-amd64-3.8\pandas\core\computation
      creating build\lib.win-amd64-3.8\pandas\core\dtypes
      copying pandas\core\dtypes\api.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
      copying pandas\core\dtypes\base.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
      copying pandas\core\dtypes\cast.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
      copying pandas\core\dtypes\common.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
      copying pandas\core\dtypes\concat.py -> build\lib.win-amd64-3.8\pandas\core\dtypes

<a id='wrangling'></a>
## Data Wrangling


### General Properties

In this section I loaded the dataset. I then explored the first few rows (df.head), the format of the variables (df.info), the shape of the dataframe (df.shape) and the descriptive statistics of the numerical variables (df.describe)


```python
# Loading data and printing first few lines

df=pd.read_csv('tmdb-movies.csv')

df.head()

```

    
      copying pandas\core\dtypes\dtypes.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
      copying pandas\core\dtypes\generic.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
      copying pandas\core\dtypes\inference.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
      copying pandas\core\dtypes\missing.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
      copying pandas\core\dtypes\__init__.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
      creating build\lib.win-amd64-3.8\pandas\core\groupby
      copying pandas\core\groupby\base.py -> build\lib.win-amd64-3.8\pandas\core\groupby
      copying pandas\core\groupby\categorical.py -> build\lib.win-amd64-3.8\pandas\core\groupby
      copying pandas\core\groupby\generic.py -> build\lib.win-amd64-3.8\pandas\core\groupby
      copying pandas\core\groupby\groupby.py -> build\lib.win-amd64-3.8\pandas\core\groupby
      copying pandas\core\groupby\grouper.py -> build\lib.win-amd64-3.8\pandas\core\groupby
      copying pandas\core\groupby\ops.py -> build\lib.win-amd64-3.8\pandas\core\groupby
      copying pandas\core\groupby\__init__.py -> build\lib.win-amd64-3.8\pandas\core\groupby
      creating build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\accessors.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\api.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\base.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\category.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\datetimelike.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\datetimes.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\frozen.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\interval.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\multi.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\numeric.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\period.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\range.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\timedeltas.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      copying pandas\core\indexes\__init__.py -> build\lib.win-amd64-3.8\pandas\core\indexes
      creating build\lib.win-amd64-3.8\pandas\core\internals
      copying pandas\core\internals\arrays.py -> build\lib.win-amd64-3.8\pandas\core\internals
      copying pandas\core\internals\blocks.py -> build\lib.win-amd64-3.8\pandas\core\internals
      copying pandas\core\internals\concat.py -> build\lib.win-amd64-3.8\pandas\core\internals
      copying pandas\core\internals\construction.py -> build\lib.win-amd64-3.8\pandas\core\internals
      copying pandas\core\internals\managers.py -> build\lib.win-amd64-3.8\pandas\core\internals
      copying pandas\core\internals\__init__.py -> build\lib.win-amd64-3.8\pandas\core\internals
      creating build\lib.win-amd64-3.8\pandas\core\ops
      copying pandas\core\ops\docstrings.py -> build\lib.win-amd64-3.8\pandas\core\ops
      copying pandas\core\ops\missing.py -> build\lib.win-amd64-3.8\pandas\core\ops
      copying pandas\core\ops\roperator.py -> build\lib.win-amd64-3.8\pandas\core\ops
      copying pandas\core\ops\__init__.py -> build\lib.win-amd64-3.8\pandas\core\ops
      creating build\lib.win-amd64-3.8\pandas\core\reshape
      copying pandas\core\reshape\api.py -> build\lib.win-amd64-3.8\pandas\core\reshape
      copying pandas\core\reshape\concat.py -> build\lib.win-amd64-3.8\pandas\core\reshape
      copying pandas\core\reshape\melt.py -> build\lib.win-amd64-3.8\pandas\core\reshape
      copying pandas\core\reshape\merge.py -> build\lib.win-amd64-3.8\pandas\core\reshape
      copying pandas\core\reshape\pivot.py -> build\lib.win-amd64-3.8\pandas\core\reshape
      copying pandas\core\reshape\reshape.py -> build\lib.win-amd64-3.8\pandas\core\reshape
      copying pandas\core\reshape\tile.py -> build\lib.win-amd64-3.8\pandas\core\reshape
      copying pandas\core\reshape\util.py -> build\lib.win-amd64-3.8\pandas\core\reshape
      copying pandas\core\reshape\__init__.py -> build\lib.win-amd64-3.8\pandas\core\reshape
      creating build\lib.win-amd64-3.8\pandas\core\sparse
      copying pandas\core\sparse\api.py -> build\lib.win-amd64-3.8\pandas\core\sparse
      copying pandas\core\sparse\frame.py -> build\lib.win-amd64-3.8\pandas\core\sparse
      copying pandas\core\sparse\scipy_sparse.py -> build\lib.win-amd64-3.8\pandas\core\sparse
      copying pandas\core\sparse\series.py -> build\lib.win-amd64-3.8\pandas\core\sparse
      copying pandas\core\sparse\__init__.py -> build\lib.win-amd64-3.8\pandas\core\sparse
      creating build\lib.win-amd64-3.8\pandas\core\tools
      copying pandas\core\tools\datetimes.py -> build\lib.win-amd64-3.8\pandas\core\tools
      copying pandas\core\tools\numeric.py -> build\lib.win-amd64-3.8\pandas\core\tools
      copying pandas\core\tools\timedeltas.py -> build\lib.win-amd64-3.8\pandas\core\tools
      copying pandas\core\tools\__init__.py -> build\lib.win-amd64-3.8\pandas\core\tools
      creating build\lib.win-amd64-3.8\pandas\core\util
      copying pandas\core\util\hashing.py -> build\lib.win-amd64-3.8\pandas\core\util
      copying pandas\core\util\__init__.py -> build\lib.win-amd64-3.8\pandas\core\util
      creating build\lib.win-amd64-3.8\pandas\io\clipboard
      copying pandas\io\clipboard\clipboards.py -> build\lib.win-amd64-3.8\pandas\io\clipboard
      copying pandas\io\clipboard\exceptions.py -> build\lib.win-amd64-3.8\pandas\io\clipboard
      copying pandas\io\clipboard\windows.py -> build\lib.win-amd64-3.8\pandas\io\clipboard
      copying pandas\io\clipboard\__init__.py -> build\lib.win-amd64-3.8\pandas\io\clipboard
      creating build\lib.win-amd64-3.8\pandas\io\excel
      copying pandas\io\excel\_base.py -> build\lib.win-amd64-3.8\pandas\io\excel
      copying pandas\io\excel\_odfreader.py -> build\lib.win-amd64-3.8\pandas\io\excel
      copying pandas\io\excel\_openpyxl.py -> build\lib.win-amd64-3.8\pandas\io\excel
      copying pandas\io\excel\_util.py -> build\lib.win-amd64-3.8\pandas\io\excel
      copying pandas\io\excel\_xlrd.py -> build\lib.win-amd64-3.8\pandas\io\excel
      copying pandas\io\excel\_xlsxwriter.py -> build\lib.win-amd64-3.8\pandas\io\excel
      copying pandas\io\excel\_xlwt.py -> build\lib.win-amd64-3.8\pandas\io\excel
      copying pandas\io\excel\__init__.py -> build\lib.win-amd64-3.8\pandas\io\excel
      creating build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\console.py -> build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\css.py -> build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\csvs.py -> build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\excel.py -> build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\format.py -> build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\html.py -> build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\latex.py -> build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\printing.py -> build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\style.py -> build\lib.win-amd64-3.8\pandas\io\formats
      copying pandas\io\formats\__init__.py -> build\lib.win-amd64-3.8\pandas\io\formats
      creating build\lib.win-amd64-3.8\pandas\io\json
      copying pandas\io\json\_json.py -> build\lib.win-amd64-3.8\pandas\io\json
      copying pandas\io\json\_normalize.py -> build\lib.win-amd64-3.8\pandas\io\json
      copying pandas\io\json\_table_schema.py -> build\lib.win-amd64-3.8\pandas\io\json
      copying pandas\io\json\__init__.py -> build\lib.win-amd64-3.8\pandas\io\json
      creating build\lib.win-amd64-3.8\pandas\io\msgpack
      copying pandas\io\msgpack\exceptions.py -> build\lib.win-amd64-3.8\pandas\io\msgpack
      copying pandas\io\msgpack\_version.py -> build\lib.win-amd64-3.8\pandas\io\msgpack
      copying pandas\io\msgpack\__init__.py -> build\lib.win-amd64-3.8\pandas\io\msgpack
      creating build\lib.win-amd64-3.8\pandas\io\sas
      copying pandas\io\sas\sas7bdat.py -> build\lib.win-amd64-3.8\pandas\io\sas
      copying pandas\io\sas\sasreader.py -> build\lib.win-amd64-3.8\pandas\io\sas
      copying pandas\io\sas\sas_constants.py -> build\lib.win-amd64-3.8\pandas\io\sas
      copying pandas\io\sas\sas_xport.py -> build\lib.win-amd64-3.8\pandas\io\sas
      copying pandas\io\sas\__init__.py -> build\lib.win-amd64-3.8\pandas\io\sas
      creating build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\boxplot.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\compat.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\converter.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\core.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\hist.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\misc.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\style.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\timeseries.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\tools.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      copying pandas\plotting\_matplotlib\__init__.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
      creating build\lib.win-amd64-3.8\pandas\tests\api
      copying pandas\tests\api\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\api
      copying pandas\tests\api\test_types.py -> build\lib.win-amd64-3.8\pandas\tests\api
      copying pandas\tests\api\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\api
      creating build\lib.win-amd64-3.8\pandas\tests\arithmetic
      copying pandas\tests\arithmetic\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
      copying pandas\tests\arithmetic\test_datetime64.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
      copying pandas\tests\arithmetic\test_numeric.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
      copying pandas\tests\arithmetic\test_object.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
      copying pandas\tests\arithmetic\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
      copying pandas\tests\arithmetic\test_timedelta64.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
      copying pandas\tests\arithmetic\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
      creating build\lib.win-amd64-3.8\pandas\tests\arrays
      copying pandas\tests\arrays\test_array.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
      copying pandas\tests\arrays\test_datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
      copying pandas\tests\arrays\test_datetimes.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
      copying pandas\tests\arrays\test_integer.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
      copying pandas\tests\arrays\test_numpy.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
      copying pandas\tests\arrays\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
      copying pandas\tests\arrays\test_timedeltas.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
      copying pandas\tests\arrays\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
      creating build\lib.win-amd64-3.8\pandas\tests\computation
      copying pandas\tests\computation\test_compat.py -> build\lib.win-amd64-3.8\pandas\tests\computation
      copying pandas\tests\computation\test_eval.py -> build\lib.win-amd64-3.8\pandas\tests\computation
      copying pandas\tests\computation\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\computation
      creating build\lib.win-amd64-3.8\pandas\tests\config
      copying pandas\tests\config\test_config.py -> build\lib.win-amd64-3.8\pandas\tests\config
      copying pandas\tests\config\test_localization.py -> build\lib.win-amd64-3.8\pandas\tests\config
      copying pandas\tests\config\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\config
      creating build\lib.win-amd64-3.8\pandas\tests\dtypes
      copying pandas\tests\dtypes\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
      copying pandas\tests\dtypes\test_concat.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
      copying pandas\tests\dtypes\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
      copying pandas\tests\dtypes\test_generic.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
      copying pandas\tests\dtypes\test_inference.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
      copying pandas\tests\dtypes\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
      copying pandas\tests\dtypes\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
      creating build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\test_categorical.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\test_external_block.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\test_integer.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\test_numpy.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\test_sparse.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      copying pandas\tests\extension\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension
      creating build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\common.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_alter_axes.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_apply.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_asof.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_axis_select_reindex.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_block_internals.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_combine_concat.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_constructors.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_convert_to.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_duplicates.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_explode.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_join.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_mutate_columns.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_nonunique_indexes.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_operators.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_quantile.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_query_eval.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_rank.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_replace.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_repr_info.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_reshape.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_sort_values_level_as_str.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_subclass.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_timeseries.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_to_csv.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\test_validate.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      copying pandas\tests\frame\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\frame
      creating build\lib.win-amd64-3.8\pandas\tests\generic
      copying pandas\tests\generic\test_frame.py -> build\lib.win-amd64-3.8\pandas\tests\generic
      copying pandas\tests\generic\test_generic.py -> build\lib.win-amd64-3.8\pandas\tests\generic
      copying pandas\tests\generic\test_label_or_level_utils.py -> build\lib.win-amd64-3.8\pandas\tests\generic
      copying pandas\tests\generic\test_series.py -> build\lib.win-amd64-3.8\pandas\tests\generic
      copying pandas\tests\generic\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\generic
      creating build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_apply.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_bin_groupby.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_categorical.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_counting.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_filters.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_function.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_groupby.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_grouping.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_index_as_string.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_nth.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_rank.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_timegrouper.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_transform.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_value_counts.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\test_whitelist.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      copying pandas\tests\groupby\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
      creating build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\common.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\test_base.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\test_category.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\test_frozen.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\test_numeric.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\test_numpy_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\test_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      copying pandas\tests\indexes\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
      creating build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\common.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_callable.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_categorical.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_chaining_and_caching.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_coercion.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_floats.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_iloc.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_indexing_engines.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_indexing_slow.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_ix.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_loc.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_partial.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_scalar.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\test_timedelta.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      copying pandas\tests\indexing\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
      creating build\lib.win-amd64-3.8\pandas\tests\internals
      copying pandas\tests\internals\test_internals.py -> build\lib.win-amd64-3.8\pandas\tests\internals
      copying pandas\tests\internals\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\internals
      creating build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\generate_legacy_storage_files.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_clipboard.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_compression.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_date_converters.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_feather.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_gbq.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_gcs.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_html.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_packers.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_parquet.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_pickle.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_s3.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_spss.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_sql.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\test_stata.py -> build\lib.win-amd64-3.8\pandas\tests\io
      copying pandas\tests\io\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io
      creating build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\common.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\test_backend.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\test_boxplot_method.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\test_converter.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\test_datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\test_frame.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\test_groupby.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\test_hist_method.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\test_misc.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\test_series.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      copying pandas\tests\plotting\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
      creating build\lib.win-amd64-3.8\pandas\tests\reductions
      copying pandas\tests\reductions\test_reductions.py -> build\lib.win-amd64-3.8\pandas\tests\reductions
      copying pandas\tests\reductions\test_stat_reductions.py -> build\lib.win-amd64-3.8\pandas\tests\reductions
      copying pandas\tests\reductions\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\reductions
      creating build\lib.win-amd64-3.8\pandas\tests\resample
      copying pandas\tests\resample\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\resample
      copying pandas\tests\resample\test_base.py -> build\lib.win-amd64-3.8\pandas\tests\resample
      copying pandas\tests\resample\test_datetime_index.py -> build\lib.win-amd64-3.8\pandas\tests\resample
      copying pandas\tests\resample\test_period_index.py -> build\lib.win-amd64-3.8\pandas\tests\resample
      copying pandas\tests\resample\test_resampler_grouper.py -> build\lib.win-amd64-3.8\pandas\tests\resample
      copying pandas\tests\resample\test_resample_api.py -> build\lib.win-amd64-3.8\pandas\tests\resample
      copying pandas\tests\resample\test_timedelta.py -> build\lib.win-amd64-3.8\pandas\tests\resample
      copying pandas\tests\resample\test_time_grouper.py -> build\lib.win-amd64-3.8\pandas\tests\resample
      copying pandas\tests\resample\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\resample
      creating build\lib.win-amd64-3.8\pandas\tests\reshape
      copying pandas\tests\reshape\test_concat.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
      copying pandas\tests\reshape\test_cut.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
      copying pandas\tests\reshape\test_melt.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
      copying pandas\tests\reshape\test_pivot.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
      copying pandas\tests\reshape\test_qcut.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
      copying pandas\tests\reshape\test_reshape.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
      copying pandas\tests\reshape\test_union_categoricals.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
      copying pandas\tests\reshape\test_util.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
      copying pandas\tests\reshape\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
      creating build\lib.win-amd64-3.8\pandas\tests\scalar
      copying pandas\tests\scalar\test_nat.py -> build\lib.win-amd64-3.8\pandas\tests\scalar
      copying pandas\tests\scalar\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar
      creating build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\common.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_alter_axes.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_apply.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_asof.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_block_internals.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_combine_concat.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_constructors.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_datetime_values.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_duplicates.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_explode.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_internals.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_io.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_operators.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_quantile.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_rank.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_replace.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_repr.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_subclass.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_timeseries.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_ufunc.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\test_validate.py -> build\lib.win-amd64-3.8\pandas\tests\series
      copying pandas\tests\series\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\series
      creating build\lib.win-amd64-3.8\pandas\tests\sparse
      copying pandas\tests\sparse\common.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
      copying pandas\tests\sparse\test_combine_concat.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
      copying pandas\tests\sparse\test_format.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
      copying pandas\tests\sparse\test_groupby.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
      copying pandas\tests\sparse\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
      copying pandas\tests\sparse\test_pivot.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
      copying pandas\tests\sparse\test_reshape.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
      copying pandas\tests\sparse\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
      creating build\lib.win-amd64-3.8\pandas\tests\tools
      copying pandas\tests\tools\test_numeric.py -> build\lib.win-amd64-3.8\pandas\tests\tools
      copying pandas\tests\tools\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tools
      creating build\lib.win-amd64-3.8\pandas\tests\tseries
      copying pandas\tests\tseries\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tseries
      creating build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_array_to_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_ccalendar.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_conversion.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_libfrequencies.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_liboffsets.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_normalize_date.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_parse_iso8601.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_parsing.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_period_asfreq.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_timedeltas.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      copying pandas\tests\tslibs\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
      creating build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_assert_almost_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_assert_categorical_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_assert_extension_array_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_assert_frame_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_assert_index_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_assert_interval_array_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_assert_numpy_array_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_assert_produces_warning.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_assert_series_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_deprecate.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_deprecate_kwarg.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_hashing.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_move.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_safe_import.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_util.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_validate_args.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_validate_args_and_kwargs.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\test_validate_kwargs.py -> build\lib.win-amd64-3.8\pandas\tests\util
      copying pandas\tests\util\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\util
      creating build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\common.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_ewm.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_expanding.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_grouper.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_moments.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_pairwise.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_rolling.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_timeseries_window.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\test_window.py -> build\lib.win-amd64-3.8\pandas\tests\window
      copying pandas\tests\window\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\window
      creating build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\common.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_algos.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_constructors.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_operators.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_repr.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_subclass.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\test_warnings.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      copying pandas\tests\arrays\categorical\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
      creating build\lib.win-amd64-3.8\pandas\tests\arrays\interval
      copying pandas\tests\arrays\interval\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\interval
      copying pandas\tests\arrays\interval\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\interval
      copying pandas\tests\arrays\interval\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\interval
      creating build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
      copying pandas\tests\arrays\sparse\test_accessor.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
      copying pandas\tests\arrays\sparse\test_arithmetics.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
      copying pandas\tests\arrays\sparse\test_array.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
      copying pandas\tests\arrays\sparse\test_dtype.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
      copying pandas\tests\arrays\sparse\test_libsparse.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
      copying pandas\tests\arrays\sparse\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
      creating build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_construct_from_scalar.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_construct_ndarray.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_construct_object_arr.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_convert_objects.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_downcast.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_find_common_type.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_infer_datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_infer_dtype.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_promote.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\test_upcast.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      copying pandas\tests\dtypes\cast\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
      creating build\lib.win-amd64-3.8\pandas\tests\extension\arrow
      copying pandas\tests\extension\arrow\bool.py -> build\lib.win-amd64-3.8\pandas\tests\extension\arrow
      copying pandas\tests\extension\arrow\test_bool.py -> build\lib.win-amd64-3.8\pandas\tests\extension\arrow
      copying pandas\tests\extension\arrow\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension\arrow
      creating build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\base.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\casting.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\constructors.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\dtype.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\getitem.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\groupby.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\interface.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\io.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\methods.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\missing.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\ops.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\printing.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\reduce.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\reshaping.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\setitem.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      copying pandas\tests\extension\base\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
      creating build\lib.win-amd64-3.8\pandas\tests\extension\decimal
      copying pandas\tests\extension\decimal\array.py -> build\lib.win-amd64-3.8\pandas\tests\extension\decimal
      copying pandas\tests\extension\decimal\test_decimal.py -> build\lib.win-amd64-3.8\pandas\tests\extension\decimal
      copying pandas\tests\extension\decimal\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension\decimal
      creating build\lib.win-amd64-3.8\pandas\tests\extension\json
      copying pandas\tests\extension\json\array.py -> build\lib.win-amd64-3.8\pandas\tests\extension\json
      copying pandas\tests\extension\json\test_json.py -> build\lib.win-amd64-3.8\pandas\tests\extension\json
      copying pandas\tests\extension\json\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension\json
      creating build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
      copying pandas\tests\groupby\aggregate\test_aggregate.py -> build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
      copying pandas\tests\groupby\aggregate\test_cython.py -> build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
      copying pandas\tests\groupby\aggregate\test_other.py -> build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
      copying pandas\tests\groupby\aggregate\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
      creating build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_date_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_formats.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_misc.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_partial_slicing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_scalar_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\test_tools.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      copying pandas\tests\indexes\datetimes\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
      creating build\lib.win-amd64-3.8\pandas\tests\indexes\interval
      copying pandas\tests\indexes\interval\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
      copying pandas\tests\indexes\interval\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
      copying pandas\tests\indexes\interval\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
      copying pandas\tests\indexes\interval\test_interval_new.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
      copying pandas\tests\indexes\interval\test_interval_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
      copying pandas\tests\indexes\interval\test_interval_tree.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
      copying pandas\tests\indexes\interval\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
      copying pandas\tests\indexes\interval\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
      creating build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_constructor.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_contains.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_conversion.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_copy.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_drop.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_duplicates.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_equivalence.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_format.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_get_set.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_integrity.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_join.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_monotonic.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_names.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_partial_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_reindex.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_reshape.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_set_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      copying pandas\tests\indexes\multi\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
      creating build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_asfreq.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_formats.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_partial_slicing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_period_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_scalar_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\test_tools.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      copying pandas\tests\indexes\period\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
      creating build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_formats.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_partial_slicing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_scalar_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_timedelta.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_timedelta_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\test_tools.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      copying pandas\tests\indexes\timedeltas\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
      creating build\lib.win-amd64-3.8\pandas\tests\indexing\interval
      copying pandas\tests\indexing\interval\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\interval
      copying pandas\tests\indexing\interval\test_interval_new.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\interval
      copying pandas\tests\indexing\interval\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\interval
      creating build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_chaining_and_caching.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_getitem.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_iloc.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_indexing_slow.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_ix.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_loc.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_multiindex.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_partial.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_setitem.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_set_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_slice.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_sorted.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\test_xs.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      copying pandas\tests\indexing\multiindex\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
      creating build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\test_odf.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\test_openpyxl.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\test_readers.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\test_style.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\test_writers.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\test_xlrd.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\test_xlsxwriter.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\test_xlwt.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      copying pandas\tests\io\excel\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
      creating build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_console.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_css.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_eng_formatting.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_format.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_printing.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_style.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_to_csv.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_to_excel.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_to_html.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\test_to_latex.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      copying pandas\tests\io\formats\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
      creating build\lib.win-amd64-3.8\pandas\tests\io\json
      copying pandas\tests\io\json\test_compression.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
      copying pandas\tests\io\json\test_json_table_schema.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
      copying pandas\tests\io\json\test_normalize.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
      copying pandas\tests\io\json\test_pandas.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
      copying pandas\tests\io\json\test_readlines.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
      copying pandas\tests\io\json\test_ujson.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
      copying pandas\tests\io\json\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
      creating build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\common.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_buffer.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_case.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_except.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_extension.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_format.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_limits.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_newspec.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_obj.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_pack.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_read_size.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_seq.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_sequnpack.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_subtype.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_unpack.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\test_unpack_raw.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      copying pandas\tests\io\msgpack\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
      creating build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_comment.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_compression.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_converters.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_c_parser_only.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_dialect.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_header.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_index_col.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_mangle_dupes.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_multi_thread.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_na_values.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_network.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_parse_dates.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_python_parser_only.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_quoting.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_read_fwf.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_skiprows.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_textreader.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_unsupported.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\test_usecols.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      copying pandas\tests\io\parser\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
      creating build\lib.win-amd64-3.8\pandas\tests\io\pytables
      copying pandas\tests\io\pytables\test_compat.py -> build\lib.win-amd64-3.8\pandas\tests\io\pytables
      copying pandas\tests\io\pytables\test_pytables.py -> build\lib.win-amd64-3.8\pandas\tests\io\pytables
      copying pandas\tests\io\pytables\test_pytables_missing.py -> build\lib.win-amd64-3.8\pandas\tests\io\pytables
      copying pandas\tests\io\pytables\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\pytables
      creating build\lib.win-amd64-3.8\pandas\tests\io\sas
      copying pandas\tests\io\sas\test_sas.py -> build\lib.win-amd64-3.8\pandas\tests\io\sas
      copying pandas\tests\io\sas\test_sas7bdat.py -> build\lib.win-amd64-3.8\pandas\tests\io\sas
      copying pandas\tests\io\sas\test_xport.py -> build\lib.win-amd64-3.8\pandas\tests\io\sas
      copying pandas\tests\io\sas\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\sas
      creating build\lib.win-amd64-3.8\pandas\tests\reshape\merge
      copying pandas\tests\reshape\merge\test_join.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
      copying pandas\tests\reshape\merge\test_merge.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
      copying pandas\tests\reshape\merge\test_merge_asof.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
      copying pandas\tests\reshape\merge\test_merge_index_as_string.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
      copying pandas\tests\reshape\merge\test_merge_ordered.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
      copying pandas\tests\reshape\merge\test_multi.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
      copying pandas\tests\reshape\merge\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
      creating build\lib.win-amd64-3.8\pandas\tests\scalar\interval
      copying pandas\tests\scalar\interval\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\interval
      copying pandas\tests\scalar\interval\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\interval
      copying pandas\tests\scalar\interval\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\interval
      creating build\lib.win-amd64-3.8\pandas\tests\scalar\period
      copying pandas\tests\scalar\period\test_asfreq.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\period
      copying pandas\tests\scalar\period\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\period
      copying pandas\tests\scalar\period\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\period
      creating build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
      copying pandas\tests\scalar\timedelta\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
      copying pandas\tests\scalar\timedelta\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
      copying pandas\tests\scalar\timedelta\test_formats.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
      copying pandas\tests\scalar\timedelta\test_timedelta.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
      copying pandas\tests\scalar\timedelta\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
      creating build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
      copying pandas\tests\scalar\timestamp\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
      copying pandas\tests\scalar\timestamp\test_comparisons.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
      copying pandas\tests\scalar\timestamp\test_rendering.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
      copying pandas\tests\scalar\timestamp\test_timestamp.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
      copying pandas\tests\scalar\timestamp\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
      copying pandas\tests\scalar\timestamp\test_unary_ops.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
      copying pandas\tests\scalar\timestamp\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
      creating build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\test_alter_index.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\test_boolean.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\test_callable.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\test_iloc.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\test_loc.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\test_numeric.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      copying pandas\tests\series\indexing\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
      creating build\lib.win-amd64-3.8\pandas\tests\sparse\frame
      copying pandas\tests\sparse\frame\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
      copying pandas\tests\sparse\frame\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
      copying pandas\tests\sparse\frame\test_apply.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
      copying pandas\tests\sparse\frame\test_frame.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
      copying pandas\tests\sparse\frame\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
      copying pandas\tests\sparse\frame\test_to_csv.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
      copying pandas\tests\sparse\frame\test_to_from_scipy.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
      copying pandas\tests\sparse\frame\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
      creating build\lib.win-amd64-3.8\pandas\tests\sparse\series
      copying pandas\tests\sparse\series\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\series
      copying pandas\tests\sparse\series\test_series.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\series
      copying pandas\tests\sparse\series\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\series
      creating build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
      copying pandas\tests\tseries\frequencies\test_freq_code.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
      copying pandas\tests\tseries\frequencies\test_inference.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
      copying pandas\tests\tseries\frequencies\test_to_offset.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
      copying pandas\tests\tseries\frequencies\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
      creating build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
      copying pandas\tests\tseries\holiday\test_calendar.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
      copying pandas\tests\tseries\holiday\test_federal.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
      copying pandas\tests\tseries\holiday\test_holiday.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
      copying pandas\tests\tseries\holiday\test_observance.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
      copying pandas\tests\tseries\holiday\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
      creating build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
      copying pandas\tests\tseries\offsets\common.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
      copying pandas\tests\tseries\offsets\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
      copying pandas\tests\tseries\offsets\test_fiscal.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
      copying pandas\tests\tseries\offsets\test_offsets.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
      copying pandas\tests\tseries\offsets\test_offsets_properties.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
      copying pandas\tests\tseries\offsets\test_ticks.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
      copying pandas\tests\tseries\offsets\test_yqm_offsets.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
      copying pandas\tests\tseries\offsets\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
      creating build\lib.win-amd64-3.8\pandas\_libs\tslibs
      copying pandas\_libs\tslibs\__init__.py -> build\lib.win-amd64-3.8\pandas\_libs\tslibs
      creating build\lib.win-amd64-3.8\pandas\io\formats\templates
      copying pandas\io\formats\templates\html.tpl -> build\lib.win-amd64-3.8\pandas\io\formats\templates
      UPDATING build\lib.win-amd64-3.8\pandas/_version.py
      set build\lib.win-amd64-3.8\pandas/_version.py to '0.25.0'
      running build_ext
      building 'pandas._libs.algos' extension
      error: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-build-tools/
      ----------------------------------------
      ERROR: Failed building wheel for pandas
        ERROR: Command errored out with exit status 1:
         command: 'C:\Programmes\Anaconda\anaconda3\python.exe' -u -c 'import io, os, sys, setuptools, tokenize; sys.argv[0] = '"'"'C:\\Users\\Propriétaire\\AppData\\Local\\Temp\\pip-install-p46j6ptv\\pandas_c3ff3a5d39284da1abfd686d0ec46106\\setup.py'"'"'; __file__='"'"'C:\\Users\\Propriétaire\\AppData\\Local\\Temp\\pip-install-p46j6ptv\\pandas_c3ff3a5d39284da1abfd686d0ec46106\\setup.py'"'"';f = getattr(tokenize, '"'"'open'"'"', open)(__file__) if os.path.exists(__file__) else io.StringIO('"'"'from setuptools import setup; setup()'"'"');code = f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record 'C:\Users\Propriétaire\AppData\Local\Temp\pip-record-t4ceruvq\install-record.txt' --single-version-externally-managed --compile --install-headers 'C:\Programmes\Anaconda\anaconda3\Include\pandas'
             cwd: C:\Users\Propriétaire\AppData\Local\Temp\pip-install-p46j6ptv\pandas_c3ff3a5d39284da1abfd686d0ec46106\
        Complete output (984 lines):
        Compiling pandas\_libs/algos.pyx because it changed.
        Compiling pandas\_libs/groupby.pyx because it changed.
        Compiling pandas\_libs/hashing.pyx because it changed.
        Compiling pandas\_libs/hashtable.pyx because it changed.
        Compiling pandas\_libs/index.pyx because it changed.
        Compiling pandas\_libs/indexing.pyx because it changed.
        Compiling pandas\_libs/internals.pyx because it changed.
        Compiling pandas\_libs/interval.pyx because it changed.
        Compiling pandas\_libs/join.pyx because it changed.
        Compiling pandas\_libs/lib.pyx because it changed.
        Compiling pandas\_libs/missing.pyx because it changed.
        Compiling pandas\_libs/parsers.pyx because it changed.
        Compiling pandas\_libs/reduction.pyx because it changed.
        Compiling pandas\_libs/ops.pyx because it changed.
        Compiling pandas\_libs/properties.pyx because it changed.
        Compiling pandas\_libs/reshape.pyx because it changed.
        Compiling pandas\_libs/skiplist.pyx because it changed.
        Compiling pandas\_libs/sparse.pyx because it changed.
        Compiling pandas\_libs/tslib.pyx because it changed.
        Compiling pandas\_libs/tslibs/c_timestamp.pyx because it changed.
        Compiling pandas\_libs/tslibs/ccalendar.pyx because it changed.
        Compiling pandas\_libs/tslibs/conversion.pyx because it changed.
        Compiling pandas\_libs/tslibs/fields.pyx because it changed.
        Compiling pandas\_libs/tslibs/frequencies.pyx because it changed.
        Compiling pandas\_libs/tslibs/nattype.pyx because it changed.
        Compiling pandas\_libs/tslibs/np_datetime.pyx because it changed.
        Compiling pandas\_libs/tslibs/offsets.pyx because it changed.
        Compiling pandas\_libs/tslibs/parsing.pyx because it changed.
        Compiling pandas\_libs/tslibs/period.pyx because it changed.
        Compiling pandas\_libs/tslibs/resolution.pyx because it changed.
        Compiling pandas\_libs/tslibs/strptime.pyx because it changed.
        Compiling pandas\_libs/tslibs/timedeltas.pyx because it changed.
        Compiling pandas\_libs/tslibs/timestamps.pyx because it changed.
        Compiling pandas\_libs/tslibs/timezones.pyx because it changed.
        Compiling pandas\_libs/tslibs/tzconversion.pyx because it changed.
        Compiling pandas\_libs/testing.pyx because it changed.
        Compiling pandas\_libs/writers.pyx because it changed.
        Compiling pandas\io/sas/sas.pyx because it changed.
        [ 1/38] Cythonizing pandas\_libs/algos.pyx
        [ 2/38] Cythonizing pandas\_libs/groupby.pyx
        [ 3/38] Cythonizing pandas\_libs/hashing.pyx
        [ 4/38] Cythonizing pandas\_libs/hashtable.pyx
        [ 5/38] Cythonizing pandas\_libs/index.pyx
        [ 6/38] Cythonizing pandas\_libs/indexing.pyx
        [ 7/38] Cythonizing pandas\_libs/internals.pyx
        [ 8/38] Cythonizing pandas\_libs/interval.pyx
        [ 9/38] Cythonizing pandas\_libs/join.pyx
        [10/38] Cythonizing pandas\_libs/lib.pyx
        [11/38] Cythonizing pandas\_libs/missing.pyx
        [12/38] Cythonizing pandas\_libs/ops.pyx
        [13/38] Cythonizing pandas\_libs/parsers.pyx
        warning: pandas\_libs\parsers.pyx:1724:34: Casting a GIL-requiring function into a nogil function circumvents GIL validation
        [14/38] Cythonizing pandas\_libs/properties.pyx
        [15/38] Cythonizing pandas\_libs/reduction.pyx
        [16/38] Cythonizing pandas\_libs/reshape.pyx
        [17/38] Cythonizing pandas\_libs/skiplist.pyx
        [18/38] Cythonizing pandas\_libs/sparse.pyx
        [19/38] Cythonizing pandas\_libs/testing.pyx
        [20/38] Cythonizing pandas\_libs/tslib.pyx
        [21/38] Cythonizing pandas\_libs/tslibs/c_timestamp.pyx
        [22/38] Cythonizing pandas\_libs/tslibs/ccalendar.pyx
        [23/38] Cythonizing pandas\_libs/tslibs/conversion.pyx
        [24/38] Cythonizing pandas\_libs/tslibs/fields.pyx
        [25/38] Cythonizing pandas\_libs/tslibs/frequencies.pyx
        [26/38] Cythonizing pandas\_libs/tslibs/nattype.pyx
        [27/38] Cythonizing pandas\_libs/tslibs/np_datetime.pyx
        [28/38] Cythonizing pandas\_libs/tslibs/offsets.pyx
        [29/38] Cythonizing pandas\_libs/tslibs/parsing.pyx
        [30/38] Cythonizing pandas\_libs/tslibs/period.pyx
        [31/38] Cythonizing pandas\_libs/tslibs/resolution.pyx
        [32/38] Cythonizing pandas\_libs/tslibs/strptime.pyx
        [33/38] Cythonizing pandas\_libs/tslibs/timedeltas.pyx
        [34/38] Cythonizing pandas\_libs/tslibs/timestamps.pyx
        [35/38] Cythonizing pandas\_libs/tslibs/timezones.pyx
        [36/38] Cythonizing pandas\_libs/tslibs/tzconversion.pyx
        [37/38] Cythonizing pandas\_libs/writers.pyx
        [38/38] Cythonizing pandas\io/sas/sas.pyx
        running install
        running build
        running build_py
        creating build
        creating build\lib.win-amd64-3.8
        creating build\lib.win-amd64-3.8\pandas
        copying pandas\conftest.py -> build\lib.win-amd64-3.8\pandas
        copying pandas\testing.py -> build\lib.win-amd64-3.8\pandas
        copying pandas\_typing.py -> build\lib.win-amd64-3.8\pandas
        copying pandas\_version.py -> build\lib.win-amd64-3.8\pandas
        copying pandas\__init__.py -> build\lib.win-amd64-3.8\pandas
        creating build\lib.win-amd64-3.8\pandas\api
        copying pandas\api\__init__.py -> build\lib.win-amd64-3.8\pandas\api
        creating build\lib.win-amd64-3.8\pandas\arrays
        copying pandas\arrays\__init__.py -> build\lib.win-amd64-3.8\pandas\arrays
        creating build\lib.win-amd64-3.8\pandas\compat
        copying pandas\compat\chainmap.py -> build\lib.win-amd64-3.8\pandas\compat
        copying pandas\compat\pickle_compat.py -> build\lib.win-amd64-3.8\pandas\compat
        copying pandas\compat\_optional.py -> build\lib.win-amd64-3.8\pandas\compat
        copying pandas\compat\__init__.py -> build\lib.win-amd64-3.8\pandas\compat
        creating build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\accessor.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\algorithms.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\api.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\apply.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\base.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\common.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\config_init.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\frame.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\generic.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\index.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\indexers.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\indexing.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\missing.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\nanops.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\resample.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\series.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\sorting.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\strings.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\window.py -> build\lib.win-amd64-3.8\pandas\core
        copying pandas\core\__init__.py -> build\lib.win-amd64-3.8\pandas\core
        creating build\lib.win-amd64-3.8\pandas\errors
        copying pandas\errors\__init__.py -> build\lib.win-amd64-3.8\pandas\errors
        creating build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\api.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\clipboards.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\common.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\date_converters.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\feather_format.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\gbq.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\gcs.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\html.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\packers.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\parquet.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\parsers.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\pickle.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\pytables.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\s3.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\spss.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\sql.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\stata.py -> build\lib.win-amd64-3.8\pandas\io
        copying pandas\io\__init__.py -> build\lib.win-amd64-3.8\pandas\io
        creating build\lib.win-amd64-3.8\pandas\plotting
        copying pandas\plotting\_core.py -> build\lib.win-amd64-3.8\pandas\plotting
        copying pandas\plotting\_misc.py -> build\lib.win-amd64-3.8\pandas\plotting
        copying pandas\plotting\__init__.py -> build\lib.win-amd64-3.8\pandas\plotting
        creating build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_algos.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_base.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_common.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_compat.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_downstream.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_errors.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_expressions.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_join.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_lib.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_multilevel.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_nanops.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_optional_dependency.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_register_accessor.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_strings.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\test_take.py -> build\lib.win-amd64-3.8\pandas\tests
        copying pandas\tests\__init__.py -> build\lib.win-amd64-3.8\pandas\tests
        creating build\lib.win-amd64-3.8\pandas\tseries
        copying pandas\tseries\api.py -> build\lib.win-amd64-3.8\pandas\tseries
        copying pandas\tseries\converter.py -> build\lib.win-amd64-3.8\pandas\tseries
        copying pandas\tseries\frequencies.py -> build\lib.win-amd64-3.8\pandas\tseries
        copying pandas\tseries\holiday.py -> build\lib.win-amd64-3.8\pandas\tseries
        copying pandas\tseries\offsets.py -> build\lib.win-amd64-3.8\pandas\tseries
        copying pandas\tseries\plotting.py -> build\lib.win-amd64-3.8\pandas\tseries
        copying pandas\tseries\__init__.py -> build\lib.win-amd64-3.8\pandas\tseries
        creating build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\testing.py -> build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\_decorators.py -> build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\_depr_module.py -> build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\_doctools.py -> build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\_exceptions.py -> build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\_print_versions.py -> build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\_tester.py -> build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\_test_decorators.py -> build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\_validators.py -> build\lib.win-amd64-3.8\pandas\util
        copying pandas\util\__init__.py -> build\lib.win-amd64-3.8\pandas\util
        creating build\lib.win-amd64-3.8\pandas\_config
        copying pandas\_config\config.py -> build\lib.win-amd64-3.8\pandas\_config
        copying pandas\_config\dates.py -> build\lib.win-amd64-3.8\pandas\_config
        copying pandas\_config\display.py -> build\lib.win-amd64-3.8\pandas\_config
        copying pandas\_config\localization.py -> build\lib.win-amd64-3.8\pandas\_config
        copying pandas\_config\__init__.py -> build\lib.win-amd64-3.8\pandas\_config
        creating build\lib.win-amd64-3.8\pandas\_libs
        copying pandas\_libs\__init__.py -> build\lib.win-amd64-3.8\pandas\_libs
        creating build\lib.win-amd64-3.8\pandas\api\extensions
        copying pandas\api\extensions\__init__.py -> build\lib.win-amd64-3.8\pandas\api\extensions
        creating build\lib.win-amd64-3.8\pandas\api\types
        copying pandas\api\types\__init__.py -> build\lib.win-amd64-3.8\pandas\api\types
        creating build\lib.win-amd64-3.8\pandas\compat\numpy
        copying pandas\compat\numpy\function.py -> build\lib.win-amd64-3.8\pandas\compat\numpy
        copying pandas\compat\numpy\__init__.py -> build\lib.win-amd64-3.8\pandas\compat\numpy
        creating build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\array_.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\base.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\categorical.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\datetimelike.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\datetimes.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\integer.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\interval.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\numpy_.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\period.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\sparse.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\timedeltas.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\_ranges.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        copying pandas\core\arrays\__init__.py -> build\lib.win-amd64-3.8\pandas\core\arrays
        creating build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\align.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\api.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\check.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\common.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\engines.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\eval.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\expr.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\expressions.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\ops.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\pytables.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\scope.py -> build\lib.win-amd64-3.8\pandas\core\computation
        copying pandas\core\computation\__init__.py -> build\lib.win-amd64-3.8\pandas\core\computation
        creating build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\api.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\base.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\cast.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\common.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\concat.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\dtypes.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\generic.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\inference.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\missing.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        copying pandas\core\dtypes\__init__.py -> build\lib.win-amd64-3.8\pandas\core\dtypes
        creating build\lib.win-amd64-3.8\pandas\core\groupby
        copying pandas\core\groupby\base.py -> build\lib.win-amd64-3.8\pandas\core\groupby
        copying pandas\core\groupby\categorical.py -> build\lib.win-amd64-3.8\pandas\core\groupby
        copying pandas\core\groupby\generic.py -> build\lib.win-amd64-3.8\pandas\core\groupby
        copying pandas\core\groupby\groupby.py -> build\lib.win-amd64-3.8\pandas\core\groupby
        copying pandas\core\groupby\grouper.py -> build\lib.win-amd64-3.8\pandas\core\groupby
        copying pandas\core\groupby\ops.py -> build\lib.win-amd64-3.8\pandas\core\groupby
        copying pandas\core\groupby\__init__.py -> build\lib.win-amd64-3.8\pandas\core\groupby
        creating build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\accessors.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\api.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\base.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\category.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\datetimelike.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\datetimes.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\frozen.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\interval.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\multi.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\numeric.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\period.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\range.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\timedeltas.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        copying pandas\core\indexes\__init__.py -> build\lib.win-amd64-3.8\pandas\core\indexes
        creating build\lib.win-amd64-3.8\pandas\core\internals
        copying pandas\core\internals\arrays.py -> build\lib.win-amd64-3.8\pandas\core\internals
        copying pandas\core\internals\blocks.py -> build\lib.win-amd64-3.8\pandas\core\internals
        copying pandas\core\internals\concat.py -> build\lib.win-amd64-3.8\pandas\core\internals
        copying pandas\core\internals\construction.py -> build\lib.win-amd64-3.8\pandas\core\internals
        copying pandas\core\internals\managers.py -> build\lib.win-amd64-3.8\pandas\core\internals
        copying pandas\core\internals\__init__.py -> build\lib.win-amd64-3.8\pandas\core\internals
        creating build\lib.win-amd64-3.8\pandas\core\ops
        copying pandas\core\ops\docstrings.py -> build\lib.win-amd64-3.8\pandas\core\ops
        copying pandas\core\ops\missing.py -> build\lib.win-amd64-3.8\pandas\core\ops
        copying pandas\core\ops\roperator.py -> build\lib.win-amd64-3.8\pandas\core\ops
        copying pandas\core\ops\__init__.py -> build\lib.win-amd64-3.8\pandas\core\ops
        creating build\lib.win-amd64-3.8\pandas\core\reshape
        copying pandas\core\reshape\api.py -> build\lib.win-amd64-3.8\pandas\core\reshape
        copying pandas\core\reshape\concat.py -> build\lib.win-amd64-3.8\pandas\core\reshape
        copying pandas\core\reshape\melt.py -> build\lib.win-amd64-3.8\pandas\core\reshape
        copying pandas\core\reshape\merge.py -> build\lib.win-amd64-3.8\pandas\core\reshape
        copying pandas\core\reshape\pivot.py -> build\lib.win-amd64-3.8\pandas\core\reshape
        copying pandas\core\reshape\reshape.py -> build\lib.win-amd64-3.8\pandas\core\reshape
        copying pandas\core\reshape\tile.py -> build\lib.win-amd64-3.8\pandas\core\reshape
        copying pandas\core\reshape\util.py -> build\lib.win-amd64-3.8\pandas\core\reshape
        copying pandas\core\reshape\__init__.py -> build\lib.win-amd64-3.8\pandas\core\reshape
        creating build\lib.win-amd64-3.8\pandas\core\sparse
        copying pandas\core\sparse\api.py -> build\lib.win-amd64-3.8\pandas\core\sparse
        copying pandas\core\sparse\frame.py -> build\lib.win-amd64-3.8\pandas\core\sparse
        copying pandas\core\sparse\scipy_sparse.py -> build\lib.win-amd64-3.8\pandas\core\sparse
        copying pandas\core\sparse\series.py -> build\lib.win-amd64-3.8\pandas\core\sparse
        copying pandas\core\sparse\__init__.py -> build\lib.win-amd64-3.8\pandas\core\sparse
        creating build\lib.win-amd64-3.8\pandas\core\tools
        copying pandas\core\tools\datetimes.py -> build\lib.win-amd64-3.8\pandas\core\tools
        copying pandas\core\tools\numeric.py -> build\lib.win-amd64-3.8\pandas\core\tools
        copying pandas\core\tools\timedeltas.py -> build\lib.win-amd64-3.8\pandas\core\tools
        copying pandas\core\tools\__init__.py -> build\lib.win-amd64-3.8\pandas\core\tools
        creating build\lib.win-amd64-3.8\pandas\core\util
        copying pandas\core\util\hashing.py -> build\lib.win-amd64-3.8\pandas\core\util
        copying pandas\core\util\__init__.py -> build\lib.win-amd64-3.8\pandas\core\util
        creating build\lib.win-amd64-3.8\pandas\io\clipboard
        copying pandas\io\clipboard\clipboards.py -> build\lib.win-amd64-3.8\pandas\io\clipboard
        copying pandas\io\clipboard\exceptions.py -> build\lib.win-amd64-3.8\pandas\io\clipboard
        copying pandas\io\clipboard\windows.py -> build\lib.win-amd64-3.8\pandas\io\clipboard
        copying pandas\io\clipboard\__init__.py -> build\lib.win-amd64-3.8\pandas\io\clipboard
        creating build\lib.win-amd64-3.8\pandas\io\excel
        copying pandas\io\excel\_base.py -> build\lib.win-amd64-3.8\pandas\io\excel
        copying pandas\io\excel\_odfreader.py -> build\lib.win-amd64-3.8\pandas\io\excel
        copying pandas\io\excel\_openpyxl.py -> build\lib.win-amd64-3.8\pandas\io\excel
        copying pandas\io\excel\_util.py -> build\lib.win-amd64-3.8\pandas\io\excel
        copying pandas\io\excel\_xlrd.py -> build\lib.win-amd64-3.8\pandas\io\excel
        copying pandas\io\excel\_xlsxwriter.py -> build\lib.win-amd64-3.8\pandas\io\excel
        copying pandas\io\excel\_xlwt.py -> build\lib.win-amd64-3.8\pandas\io\excel
        copying pandas\io\excel\__init__.py -> build\lib.win-amd64-3.8\pandas\io\excel
        creating build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\console.py -> build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\css.py -> build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\csvs.py -> build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\excel.py -> build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\format.py -> build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\html.py -> build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\latex.py -> build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\printing.py -> build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\style.py -> build\lib.win-amd64-3.8\pandas\io\formats
        copying pandas\io\formats\__init__.py -> build\lib.win-amd64-3.8\pandas\io\formats
        creating build\lib.win-amd64-3.8\pandas\io\json
        copying pandas\io\json\_json.py -> build\lib.win-amd64-3.8\pandas\io\json
        copying pandas\io\json\_normalize.py -> build\lib.win-amd64-3.8\pandas\io\json
        copying pandas\io\json\_table_schema.py -> build\lib.win-amd64-3.8\pandas\io\json
        copying pandas\io\json\__init__.py -> build\lib.win-amd64-3.8\pandas\io\json
        creating build\lib.win-amd64-3.8\pandas\io\msgpack
        copying pandas\io\msgpack\exceptions.py -> build\lib.win-amd64-3.8\pandas\io\msgpack
        copying pandas\io\msgpack\_version.py -> build\lib.win-amd64-3.8\pandas\io\msgpack
        copying pandas\io\msgpack\__init__.py -> build\lib.win-amd64-3.8\pandas\io\msgpack
        creating build\lib.win-amd64-3.8\pandas\io\sas
        copying pandas\io\sas\sas7bdat.py -> build\lib.win-amd64-3.8\pandas\io\sas
        copying pandas\io\sas\sasreader.py -> build\lib.win-amd64-3.8\pandas\io\sas
        copying pandas\io\sas\sas_constants.py -> build\lib.win-amd64-3.8\pandas\io\sas
        copying pandas\io\sas\sas_xport.py -> build\lib.win-amd64-3.8\pandas\io\sas
        copying pandas\io\sas\__init__.py -> build\lib.win-amd64-3.8\pandas\io\sas
        creating build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\boxplot.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\compat.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\converter.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\core.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\hist.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\misc.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\style.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\timeseries.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\tools.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        copying pandas\plotting\_matplotlib\__init__.py -> build\lib.win-amd64-3.8\pandas\plotting\_matplotlib
        creating build\lib.win-amd64-3.8\pandas\tests\api
        copying pandas\tests\api\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\api
        copying pandas\tests\api\test_types.py -> build\lib.win-amd64-3.8\pandas\tests\api
        copying pandas\tests\api\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\api
        creating build\lib.win-amd64-3.8\pandas\tests\arithmetic
        copying pandas\tests\arithmetic\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
        copying pandas\tests\arithmetic\test_datetime64.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
        copying pandas\tests\arithmetic\test_numeric.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
        copying pandas\tests\arithmetic\test_object.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
        copying pandas\tests\arithmetic\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
        copying pandas\tests\arithmetic\test_timedelta64.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
        copying pandas\tests\arithmetic\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arithmetic
        creating build\lib.win-amd64-3.8\pandas\tests\arrays
        copying pandas\tests\arrays\test_array.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
        copying pandas\tests\arrays\test_datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
        copying pandas\tests\arrays\test_datetimes.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
        copying pandas\tests\arrays\test_integer.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
        copying pandas\tests\arrays\test_numpy.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
        copying pandas\tests\arrays\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
        copying pandas\tests\arrays\test_timedeltas.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
        copying pandas\tests\arrays\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arrays
        creating build\lib.win-amd64-3.8\pandas\tests\computation
        copying pandas\tests\computation\test_compat.py -> build\lib.win-amd64-3.8\pandas\tests\computation
        copying pandas\tests\computation\test_eval.py -> build\lib.win-amd64-3.8\pandas\tests\computation
        copying pandas\tests\computation\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\computation
        creating build\lib.win-amd64-3.8\pandas\tests\config
        copying pandas\tests\config\test_config.py -> build\lib.win-amd64-3.8\pandas\tests\config
        copying pandas\tests\config\test_localization.py -> build\lib.win-amd64-3.8\pandas\tests\config
        copying pandas\tests\config\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\config
        creating build\lib.win-amd64-3.8\pandas\tests\dtypes
        copying pandas\tests\dtypes\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
        copying pandas\tests\dtypes\test_concat.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
        copying pandas\tests\dtypes\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
        copying pandas\tests\dtypes\test_generic.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
        copying pandas\tests\dtypes\test_inference.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
        copying pandas\tests\dtypes\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
        copying pandas\tests\dtypes\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes
        creating build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\test_categorical.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\test_external_block.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\test_integer.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\test_numpy.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\test_sparse.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        copying pandas\tests\extension\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension
        creating build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\common.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_alter_axes.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_apply.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_asof.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_axis_select_reindex.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_block_internals.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_combine_concat.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_constructors.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_convert_to.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_duplicates.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_explode.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_join.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_mutate_columns.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_nonunique_indexes.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_operators.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_quantile.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_query_eval.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_rank.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_replace.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_repr_info.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_reshape.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_sort_values_level_as_str.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_subclass.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_timeseries.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_to_csv.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\test_validate.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        copying pandas\tests\frame\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\frame
        creating build\lib.win-amd64-3.8\pandas\tests\generic
        copying pandas\tests\generic\test_frame.py -> build\lib.win-amd64-3.8\pandas\tests\generic
        copying pandas\tests\generic\test_generic.py -> build\lib.win-amd64-3.8\pandas\tests\generic
        copying pandas\tests\generic\test_label_or_level_utils.py -> build\lib.win-amd64-3.8\pandas\tests\generic
        copying pandas\tests\generic\test_series.py -> build\lib.win-amd64-3.8\pandas\tests\generic
        copying pandas\tests\generic\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\generic
        creating build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_apply.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_bin_groupby.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_categorical.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_counting.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_filters.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_function.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_groupby.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_grouping.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_index_as_string.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_nth.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_rank.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_timegrouper.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_transform.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_value_counts.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\test_whitelist.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        copying pandas\tests\groupby\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\groupby
        creating build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\common.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\test_base.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\test_category.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\test_frozen.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\test_numeric.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\test_numpy_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\test_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        copying pandas\tests\indexes\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes
        creating build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\common.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_callable.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_categorical.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_chaining_and_caching.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_coercion.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_floats.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_iloc.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_indexing_engines.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_indexing_slow.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_ix.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_loc.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_partial.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_scalar.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\test_timedelta.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        copying pandas\tests\indexing\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexing
        creating build\lib.win-amd64-3.8\pandas\tests\internals
        copying pandas\tests\internals\test_internals.py -> build\lib.win-amd64-3.8\pandas\tests\internals
        copying pandas\tests\internals\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\internals
        creating build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\generate_legacy_storage_files.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_clipboard.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_compression.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_date_converters.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_feather.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_gbq.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_gcs.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_html.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_packers.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_parquet.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_pickle.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_s3.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_spss.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_sql.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\test_stata.py -> build\lib.win-amd64-3.8\pandas\tests\io
        copying pandas\tests\io\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io
        creating build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\common.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\test_backend.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\test_boxplot_method.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\test_converter.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\test_datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\test_frame.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\test_groupby.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\test_hist_method.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\test_misc.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\test_series.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        copying pandas\tests\plotting\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\plotting
        creating build\lib.win-amd64-3.8\pandas\tests\reductions
        copying pandas\tests\reductions\test_reductions.py -> build\lib.win-amd64-3.8\pandas\tests\reductions
        copying pandas\tests\reductions\test_stat_reductions.py -> build\lib.win-amd64-3.8\pandas\tests\reductions
        copying pandas\tests\reductions\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\reductions
        creating build\lib.win-amd64-3.8\pandas\tests\resample
        copying pandas\tests\resample\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\resample
        copying pandas\tests\resample\test_base.py -> build\lib.win-amd64-3.8\pandas\tests\resample
        copying pandas\tests\resample\test_datetime_index.py -> build\lib.win-amd64-3.8\pandas\tests\resample
        copying pandas\tests\resample\test_period_index.py -> build\lib.win-amd64-3.8\pandas\tests\resample
        copying pandas\tests\resample\test_resampler_grouper.py -> build\lib.win-amd64-3.8\pandas\tests\resample
        copying pandas\tests\resample\test_resample_api.py -> build\lib.win-amd64-3.8\pandas\tests\resample
        copying pandas\tests\resample\test_timedelta.py -> build\lib.win-amd64-3.8\pandas\tests\resample
        copying pandas\tests\resample\test_time_grouper.py -> build\lib.win-amd64-3.8\pandas\tests\resample
        copying pandas\tests\resample\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\resample
        creating build\lib.win-amd64-3.8\pandas\tests\reshape
        copying pandas\tests\reshape\test_concat.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
        copying pandas\tests\reshape\test_cut.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
        copying pandas\tests\reshape\test_melt.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
        copying pandas\tests\reshape\test_pivot.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
        copying pandas\tests\reshape\test_qcut.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
        copying pandas\tests\reshape\test_reshape.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
        copying pandas\tests\reshape\test_union_categoricals.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
        copying pandas\tests\reshape\test_util.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
        copying pandas\tests\reshape\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\reshape
        creating build\lib.win-amd64-3.8\pandas\tests\scalar
        copying pandas\tests\scalar\test_nat.py -> build\lib.win-amd64-3.8\pandas\tests\scalar
        copying pandas\tests\scalar\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar
        creating build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\common.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_alter_axes.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_apply.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_asof.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_block_internals.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_combine_concat.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_constructors.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_datetime_values.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_duplicates.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_explode.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_internals.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_io.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_operators.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_quantile.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_rank.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_replace.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_repr.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_subclass.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_timeseries.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_ufunc.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\test_validate.py -> build\lib.win-amd64-3.8\pandas\tests\series
        copying pandas\tests\series\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\series
        creating build\lib.win-amd64-3.8\pandas\tests\sparse
        copying pandas\tests\sparse\common.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
        copying pandas\tests\sparse\test_combine_concat.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
        copying pandas\tests\sparse\test_format.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
        copying pandas\tests\sparse\test_groupby.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
        copying pandas\tests\sparse\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
        copying pandas\tests\sparse\test_pivot.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
        copying pandas\tests\sparse\test_reshape.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
        copying pandas\tests\sparse\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\sparse
        creating build\lib.win-amd64-3.8\pandas\tests\tools
        copying pandas\tests\tools\test_numeric.py -> build\lib.win-amd64-3.8\pandas\tests\tools
        copying pandas\tests\tools\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tools
        creating build\lib.win-amd64-3.8\pandas\tests\tseries
        copying pandas\tests\tseries\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tseries
        creating build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_array_to_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_ccalendar.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_conversion.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_libfrequencies.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_liboffsets.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_normalize_date.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_parse_iso8601.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_parsing.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_period_asfreq.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_timedeltas.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        copying pandas\tests\tslibs\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tslibs
        creating build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_assert_almost_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_assert_categorical_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_assert_extension_array_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_assert_frame_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_assert_index_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_assert_interval_array_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_assert_numpy_array_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_assert_produces_warning.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_assert_series_equal.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_deprecate.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_deprecate_kwarg.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_hashing.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_move.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_safe_import.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_util.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_validate_args.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_validate_args_and_kwargs.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\test_validate_kwargs.py -> build\lib.win-amd64-3.8\pandas\tests\util
        copying pandas\tests\util\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\util
        creating build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\common.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_ewm.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_expanding.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_grouper.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_moments.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_pairwise.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_rolling.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_timeseries_window.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\test_window.py -> build\lib.win-amd64-3.8\pandas\tests\window
        copying pandas\tests\window\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\window
        creating build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\common.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_algos.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_api.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_constructors.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_operators.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_repr.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_subclass.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\test_warnings.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        copying pandas\tests\arrays\categorical\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\categorical
        creating build\lib.win-amd64-3.8\pandas\tests\arrays\interval
        copying pandas\tests\arrays\interval\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\interval
        copying pandas\tests\arrays\interval\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\interval
        copying pandas\tests\arrays\interval\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\interval
        creating build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
        copying pandas\tests\arrays\sparse\test_accessor.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
        copying pandas\tests\arrays\sparse\test_arithmetics.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
        copying pandas\tests\arrays\sparse\test_array.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
        copying pandas\tests\arrays\sparse\test_dtype.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
        copying pandas\tests\arrays\sparse\test_libsparse.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
        copying pandas\tests\arrays\sparse\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\arrays\sparse
        creating build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_construct_from_scalar.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_construct_ndarray.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_construct_object_arr.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_convert_objects.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_downcast.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_find_common_type.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_infer_datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_infer_dtype.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_promote.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\test_upcast.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        copying pandas\tests\dtypes\cast\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\dtypes\cast
        creating build\lib.win-amd64-3.8\pandas\tests\extension\arrow
        copying pandas\tests\extension\arrow\bool.py -> build\lib.win-amd64-3.8\pandas\tests\extension\arrow
        copying pandas\tests\extension\arrow\test_bool.py -> build\lib.win-amd64-3.8\pandas\tests\extension\arrow
        copying pandas\tests\extension\arrow\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension\arrow
        creating build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\base.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\casting.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\constructors.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\dtype.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\getitem.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\groupby.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\interface.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\io.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\methods.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\missing.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\ops.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\printing.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\reduce.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\reshaping.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\setitem.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        copying pandas\tests\extension\base\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension\base
        creating build\lib.win-amd64-3.8\pandas\tests\extension\decimal
        copying pandas\tests\extension\decimal\array.py -> build\lib.win-amd64-3.8\pandas\tests\extension\decimal
        copying pandas\tests\extension\decimal\test_decimal.py -> build\lib.win-amd64-3.8\pandas\tests\extension\decimal
        copying pandas\tests\extension\decimal\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension\decimal
        creating build\lib.win-amd64-3.8\pandas\tests\extension\json
        copying pandas\tests\extension\json\array.py -> build\lib.win-amd64-3.8\pandas\tests\extension\json
        copying pandas\tests\extension\json\test_json.py -> build\lib.win-amd64-3.8\pandas\tests\extension\json
        copying pandas\tests\extension\json\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\extension\json
        creating build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
        copying pandas\tests\groupby\aggregate\test_aggregate.py -> build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
        copying pandas\tests\groupby\aggregate\test_cython.py -> build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
        copying pandas\tests\groupby\aggregate\test_other.py -> build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
        copying pandas\tests\groupby\aggregate\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\groupby\aggregate
        creating build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_datetimelike.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_date_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_formats.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_misc.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_partial_slicing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_scalar_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\test_tools.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        copying pandas\tests\indexes\datetimes\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\datetimes
        creating build\lib.win-amd64-3.8\pandas\tests\indexes\interval
        copying pandas\tests\indexes\interval\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
        copying pandas\tests\indexes\interval\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
        copying pandas\tests\indexes\interval\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
        copying pandas\tests\indexes\interval\test_interval_new.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
        copying pandas\tests\indexes\interval\test_interval_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
        copying pandas\tests\indexes\interval\test_interval_tree.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
        copying pandas\tests\indexes\interval\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
        copying pandas\tests\indexes\interval\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\interval
        creating build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_constructor.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_contains.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_conversion.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_copy.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_drop.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_duplicates.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_equivalence.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_format.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_get_set.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_integrity.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_join.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_missing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_monotonic.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_names.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_partial_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_reindex.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_reshape.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_set_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\test_sorting.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        copying pandas\tests\indexes\multi\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\multi
        creating build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_asfreq.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_formats.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_partial_slicing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_period_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_scalar_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\test_tools.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        copying pandas\tests\indexes\period\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\period
        creating build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_astype.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_formats.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_partial_slicing.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_scalar_compat.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_setops.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_timedelta.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_timedelta_range.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\test_tools.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        copying pandas\tests\indexes\timedeltas\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexes\timedeltas
        creating build\lib.win-amd64-3.8\pandas\tests\indexing\interval
        copying pandas\tests\indexing\interval\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\interval
        copying pandas\tests\indexing\interval\test_interval_new.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\interval
        copying pandas\tests\indexing\interval\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\interval
        creating build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_chaining_and_caching.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_getitem.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_iloc.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_indexing_slow.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_ix.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_loc.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_multiindex.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_partial.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_setitem.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_set_ops.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_slice.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_sorted.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\test_xs.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        copying pandas\tests\indexing\multiindex\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\indexing\multiindex
        creating build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\test_odf.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\test_openpyxl.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\test_readers.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\test_style.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\test_writers.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\test_xlrd.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\test_xlsxwriter.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\test_xlwt.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        copying pandas\tests\io\excel\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\excel
        creating build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_console.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_css.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_eng_formatting.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_format.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_printing.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_style.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_to_csv.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_to_excel.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_to_html.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\test_to_latex.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        copying pandas\tests\io\formats\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\formats
        creating build\lib.win-amd64-3.8\pandas\tests\io\json
        copying pandas\tests\io\json\test_compression.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
        copying pandas\tests\io\json\test_json_table_schema.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
        copying pandas\tests\io\json\test_normalize.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
        copying pandas\tests\io\json\test_pandas.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
        copying pandas\tests\io\json\test_readlines.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
        copying pandas\tests\io\json\test_ujson.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
        copying pandas\tests\io\json\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\json
        creating build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\common.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_buffer.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_case.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_except.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_extension.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_format.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_limits.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_newspec.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_obj.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_pack.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_read_size.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_seq.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_sequnpack.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_subtype.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_unpack.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\test_unpack_raw.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        copying pandas\tests\io\msgpack\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\msgpack
        creating build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_comment.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_common.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_compression.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_converters.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_c_parser_only.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_dialect.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_dtypes.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_header.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_index_col.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_mangle_dupes.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_multi_thread.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_na_values.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_network.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_parse_dates.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_python_parser_only.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_quoting.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_read_fwf.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_skiprows.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_textreader.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_unsupported.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\test_usecols.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        copying pandas\tests\io\parser\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\parser
        creating build\lib.win-amd64-3.8\pandas\tests\io\pytables
        copying pandas\tests\io\pytables\test_compat.py -> build\lib.win-amd64-3.8\pandas\tests\io\pytables
        copying pandas\tests\io\pytables\test_pytables.py -> build\lib.win-amd64-3.8\pandas\tests\io\pytables
        copying pandas\tests\io\pytables\test_pytables_missing.py -> build\lib.win-amd64-3.8\pandas\tests\io\pytables
        copying pandas\tests\io\pytables\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\pytables
        creating build\lib.win-amd64-3.8\pandas\tests\io\sas
        copying pandas\tests\io\sas\test_sas.py -> build\lib.win-amd64-3.8\pandas\tests\io\sas
        copying pandas\tests\io\sas\test_sas7bdat.py -> build\lib.win-amd64-3.8\pandas\tests\io\sas
        copying pandas\tests\io\sas\test_xport.py -> build\lib.win-amd64-3.8\pandas\tests\io\sas
        copying pandas\tests\io\sas\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\io\sas
        creating build\lib.win-amd64-3.8\pandas\tests\reshape\merge
        copying pandas\tests\reshape\merge\test_join.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
        copying pandas\tests\reshape\merge\test_merge.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
        copying pandas\tests\reshape\merge\test_merge_asof.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
        copying pandas\tests\reshape\merge\test_merge_index_as_string.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
        copying pandas\tests\reshape\merge\test_merge_ordered.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
        copying pandas\tests\reshape\merge\test_multi.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
        copying pandas\tests\reshape\merge\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\reshape\merge
        creating build\lib.win-amd64-3.8\pandas\tests\scalar\interval
        copying pandas\tests\scalar\interval\test_interval.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\interval
        copying pandas\tests\scalar\interval\test_ops.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\interval
        copying pandas\tests\scalar\interval\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\interval
        creating build\lib.win-amd64-3.8\pandas\tests\scalar\period
        copying pandas\tests\scalar\period\test_asfreq.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\period
        copying pandas\tests\scalar\period\test_period.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\period
        copying pandas\tests\scalar\period\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\period
        creating build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
        copying pandas\tests\scalar\timedelta\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
        copying pandas\tests\scalar\timedelta\test_construction.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
        copying pandas\tests\scalar\timedelta\test_formats.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
        copying pandas\tests\scalar\timedelta\test_timedelta.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
        copying pandas\tests\scalar\timedelta\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timedelta
        creating build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
        copying pandas\tests\scalar\timestamp\test_arithmetic.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
        copying pandas\tests\scalar\timestamp\test_comparisons.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
        copying pandas\tests\scalar\timestamp\test_rendering.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
        copying pandas\tests\scalar\timestamp\test_timestamp.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
        copying pandas\tests\scalar\timestamp\test_timezones.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
        copying pandas\tests\scalar\timestamp\test_unary_ops.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
        copying pandas\tests\scalar\timestamp\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\scalar\timestamp
        creating build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\test_alter_index.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\test_boolean.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\test_callable.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\test_datetime.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\test_iloc.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\test_loc.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\test_numeric.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        copying pandas\tests\series\indexing\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\series\indexing
        creating build\lib.win-amd64-3.8\pandas\tests\sparse\frame
        copying pandas\tests\sparse\frame\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
        copying pandas\tests\sparse\frame\test_analytics.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
        copying pandas\tests\sparse\frame\test_apply.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
        copying pandas\tests\sparse\frame\test_frame.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
        copying pandas\tests\sparse\frame\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
        copying pandas\tests\sparse\frame\test_to_csv.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
        copying pandas\tests\sparse\frame\test_to_from_scipy.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
        copying pandas\tests\sparse\frame\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\frame
        creating build\lib.win-amd64-3.8\pandas\tests\sparse\series
        copying pandas\tests\sparse\series\test_indexing.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\series
        copying pandas\tests\sparse\series\test_series.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\series
        copying pandas\tests\sparse\series\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\sparse\series
        creating build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
        copying pandas\tests\tseries\frequencies\test_freq_code.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
        copying pandas\tests\tseries\frequencies\test_inference.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
        copying pandas\tests\tseries\frequencies\test_to_offset.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
        copying pandas\tests\tseries\frequencies\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\frequencies
        creating build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
        copying pandas\tests\tseries\holiday\test_calendar.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
        copying pandas\tests\tseries\holiday\test_federal.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
        copying pandas\tests\tseries\holiday\test_holiday.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
        copying pandas\tests\tseries\holiday\test_observance.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
        copying pandas\tests\tseries\holiday\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\holiday
        creating build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
        copying pandas\tests\tseries\offsets\common.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
        copying pandas\tests\tseries\offsets\conftest.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
        copying pandas\tests\tseries\offsets\test_fiscal.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
        copying pandas\tests\tseries\offsets\test_offsets.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
        copying pandas\tests\tseries\offsets\test_offsets_properties.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
        copying pandas\tests\tseries\offsets\test_ticks.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
        copying pandas\tests\tseries\offsets\test_yqm_offsets.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
        copying pandas\tests\tseries\offsets\__init__.py -> build\lib.win-amd64-3.8\pandas\tests\tseries\offsets
        creating build\lib.win-amd64-3.8\pandas\_libs\tslibs
        copying pandas\_libs\tslibs\__init__.py -> build\lib.win-amd64-3.8\pandas\_libs\tslibs
        creating build\lib.win-amd64-3.8\pandas\io\formats\templates
        copying pandas\io\formats\templates\html.tpl -> build\lib.win-amd64-3.8\pandas\io\formats\templates
        UPDATING build\lib.win-amd64-3.8\pandas/_version.py
        set build\lib.win-amd64-3.8\pandas/_version.py to '0.25.0'
        running build_ext
        building 'pandas._libs.algos' extension
        error: Microsoft Visual C++ 14.0 or greater is required. Get it with "Microsoft C++ Build Tools": https://visualstudio.microsoft.com/visual-cpp-build-tools/
        ----------------------------------------
    ERROR: Command errored out with exit status 1: 'C:\Programmes\Anaconda\anaconda3\python.exe' -u -c 'import io, os, sys, setuptools, tokenize; sys.argv[0] = '"'"'C:\\Users\\Propriétaire\\AppData\\Local\\Temp\\pip-install-p46j6ptv\\pandas_c3ff3a5d39284da1abfd686d0ec46106\\setup.py'"'"'; __file__='"'"'C:\\Users\\Propriétaire\\AppData\\Local\\Temp\\pip-install-p46j6ptv\\pandas_c3ff3a5d39284da1abfd686d0ec46106\\setup.py'"'"';f = getattr(tokenize, '"'"'open'"'"', open)(__file__) if os.path.exists(__file__) else io.StringIO('"'"'from setuptools import setup; setup()'"'"');code = f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compile(code, __file__, '"'"'exec'"'"'))' install --record 'C:\Users\Propriétaire\AppData\Local\Temp\pip-record-t4ceruvq\install-record.txt' --single-version-externally-managed --compile --install-headers 'C:\Programmes\Anaconda\anaconda3\Include\pandas' Check the logs for full command output.
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>imdb_id</th>
      <th>popularity</th>
      <th>budget</th>
      <th>revenue</th>
      <th>original_title</th>
      <th>cast</th>
      <th>homepage</th>
      <th>director</th>
      <th>tagline</th>
      <th>...</th>
      <th>overview</th>
      <th>runtime</th>
      <th>genres</th>
      <th>production_companies</th>
      <th>release_date</th>
      <th>vote_count</th>
      <th>vote_average</th>
      <th>release_year</th>
      <th>budget_adj</th>
      <th>revenue_adj</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>135397</td>
      <td>tt0369610</td>
      <td>32.985763</td>
      <td>150000000</td>
      <td>1513528810</td>
      <td>Jurassic World</td>
      <td>Chris Pratt|Bryce Dallas Howard|Irrfan Khan|Vi...</td>
      <td>http://www.jurassicworld.com/</td>
      <td>Colin Trevorrow</td>
      <td>The park is open.</td>
      <td>...</td>
      <td>Twenty-two years after the events of Jurassic ...</td>
      <td>124</td>
      <td>Action|Adventure|Science Fiction|Thriller</td>
      <td>Universal Studios|Amblin Entertainment|Legenda...</td>
      <td>6/9/15</td>
      <td>5562</td>
      <td>6.5</td>
      <td>2015</td>
      <td>1.379999e+08</td>
      <td>1.392446e+09</td>
    </tr>
    <tr>
      <th>1</th>
      <td>76341</td>
      <td>tt1392190</td>
      <td>28.419936</td>
      <td>150000000</td>
      <td>378436354</td>
      <td>Mad Max: Fury Road</td>
      <td>Tom Hardy|Charlize Theron|Hugh Keays-Byrne|Nic...</td>
      <td>http://www.madmaxmovie.com/</td>
      <td>George Miller</td>
      <td>What a Lovely Day.</td>
      <td>...</td>
      <td>An apocalyptic story set in the furthest reach...</td>
      <td>120</td>
      <td>Action|Adventure|Science Fiction|Thriller</td>
      <td>Village Roadshow Pictures|Kennedy Miller Produ...</td>
      <td>5/13/15</td>
      <td>6185</td>
      <td>7.1</td>
      <td>2015</td>
      <td>1.379999e+08</td>
      <td>3.481613e+08</td>
    </tr>
    <tr>
      <th>2</th>
      <td>262500</td>
      <td>tt2908446</td>
      <td>13.112507</td>
      <td>110000000</td>
      <td>295238201</td>
      <td>Insurgent</td>
      <td>Shailene Woodley|Theo James|Kate Winslet|Ansel...</td>
      <td>http://www.thedivergentseries.movie/#insurgent</td>
      <td>Robert Schwentke</td>
      <td>One Choice Can Destroy You</td>
      <td>...</td>
      <td>Beatrice Prior must confront her inner demons ...</td>
      <td>119</td>
      <td>Adventure|Science Fiction|Thriller</td>
      <td>Summit Entertainment|Mandeville Films|Red Wago...</td>
      <td>3/18/15</td>
      <td>2480</td>
      <td>6.3</td>
      <td>2015</td>
      <td>1.012000e+08</td>
      <td>2.716190e+08</td>
    </tr>
    <tr>
      <th>3</th>
      <td>140607</td>
      <td>tt2488496</td>
      <td>11.173104</td>
      <td>200000000</td>
      <td>2068178225</td>
      <td>Star Wars: The Force Awakens</td>
      <td>Harrison Ford|Mark Hamill|Carrie Fisher|Adam D...</td>
      <td>http://www.starwars.com/films/star-wars-episod...</td>
      <td>J.J. Abrams</td>
      <td>Every generation has a story.</td>
      <td>...</td>
      <td>Thirty years after defeating the Galactic Empi...</td>
      <td>136</td>
      <td>Action|Adventure|Science Fiction|Fantasy</td>
      <td>Lucasfilm|Truenorth Productions|Bad Robot</td>
      <td>12/15/15</td>
      <td>5292</td>
      <td>7.5</td>
      <td>2015</td>
      <td>1.839999e+08</td>
      <td>1.902723e+09</td>
    </tr>
    <tr>
      <th>4</th>
      <td>168259</td>
      <td>tt2820852</td>
      <td>9.335014</td>
      <td>190000000</td>
      <td>1506249360</td>
      <td>Furious 7</td>
      <td>Vin Diesel|Paul Walker|Jason Statham|Michelle ...</td>
      <td>http://www.furious7.com/</td>
      <td>James Wan</td>
      <td>Vengeance Hits Home</td>
      <td>...</td>
      <td>Deckard Shaw seeks revenge against Dominic Tor...</td>
      <td>137</td>
      <td>Action|Crime|Thriller</td>
      <td>Universal Pictures|Original Film|Media Rights ...</td>
      <td>4/1/15</td>
      <td>2947</td>
      <td>7.3</td>
      <td>2015</td>
      <td>1.747999e+08</td>
      <td>1.385749e+09</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 21 columns</p>
</div>




```python
## checking the data type of each column, number of rows, columns, and missing data.
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10866 entries, 0 to 10865
    Data columns (total 21 columns):
     #   Column                Non-Null Count  Dtype  
    ---  ------                --------------  -----  
     0   id                    10866 non-null  int64  
     1   imdb_id               10856 non-null  object 
     2   popularity            10866 non-null  float64
     3   budget                10866 non-null  int64  
     4   revenue               10866 non-null  int64  
     5   original_title        10866 non-null  object 
     6   cast                  10790 non-null  object 
     7   homepage              2936 non-null   object 
     8   director              10822 non-null  object 
     9   tagline               8042 non-null   object 
     10  keywords              9373 non-null   object 
     11  overview              10862 non-null  object 
     12  runtime               10866 non-null  int64  
     13  genres                10843 non-null  object 
     14  production_companies  9836 non-null   object 
     15  release_date          10866 non-null  object 
     16  vote_count            10866 non-null  int64  
     17  vote_average          10866 non-null  float64
     18  release_year          10866 non-null  int64  
     19  budget_adj            10866 non-null  float64
     20  revenue_adj           10866 non-null  float64
    dtypes: float64(4), int64(6), object(11)
    memory usage: 1.7+ MB
    


```python
## Shape of the dataset
df.shape
```




    (10866, 21)




```python
#Descriptive statistics of the dataset's numerical values

df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>popularity</th>
      <th>budget</th>
      <th>revenue</th>
      <th>runtime</th>
      <th>vote_count</th>
      <th>vote_average</th>
      <th>release_year</th>
      <th>budget_adj</th>
      <th>revenue_adj</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>10866.000000</td>
      <td>10866.000000</td>
      <td>1.086600e+04</td>
      <td>1.086600e+04</td>
      <td>10866.000000</td>
      <td>10866.000000</td>
      <td>10866.000000</td>
      <td>10866.000000</td>
      <td>1.086600e+04</td>
      <td>1.086600e+04</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>66064.177434</td>
      <td>0.646441</td>
      <td>1.462570e+07</td>
      <td>3.982332e+07</td>
      <td>102.070863</td>
      <td>217.389748</td>
      <td>5.974922</td>
      <td>2001.322658</td>
      <td>1.755104e+07</td>
      <td>5.136436e+07</td>
    </tr>
    <tr>
      <th>std</th>
      <td>92130.136561</td>
      <td>1.000185</td>
      <td>3.091321e+07</td>
      <td>1.170035e+08</td>
      <td>31.381405</td>
      <td>575.619058</td>
      <td>0.935142</td>
      <td>12.812941</td>
      <td>3.430616e+07</td>
      <td>1.446325e+08</td>
    </tr>
    <tr>
      <th>min</th>
      <td>5.000000</td>
      <td>0.000065</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000</td>
      <td>10.000000</td>
      <td>1.500000</td>
      <td>1960.000000</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>10596.250000</td>
      <td>0.207583</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>90.000000</td>
      <td>17.000000</td>
      <td>5.400000</td>
      <td>1995.000000</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>20669.000000</td>
      <td>0.383856</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>99.000000</td>
      <td>38.000000</td>
      <td>6.000000</td>
      <td>2006.000000</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>75610.000000</td>
      <td>0.713817</td>
      <td>1.500000e+07</td>
      <td>2.400000e+07</td>
      <td>111.000000</td>
      <td>145.750000</td>
      <td>6.600000</td>
      <td>2011.000000</td>
      <td>2.085325e+07</td>
      <td>3.369710e+07</td>
    </tr>
    <tr>
      <th>max</th>
      <td>417859.000000</td>
      <td>32.985763</td>
      <td>4.250000e+08</td>
      <td>2.781506e+09</td>
      <td>900.000000</td>
      <td>9767.000000</td>
      <td>9.200000</td>
      <td>2015.000000</td>
      <td>4.250000e+08</td>
      <td>2.827124e+09</td>
    </tr>
  </tbody>
</table>
</div>




### Data Cleaning

In this part of the analysis, I continued to inspect the data, find issues to be solved to make it more reliable and easier to analyse.

#### a) Dropping irrelevant columns

I decided that some of the columns were unnecessary to answer the questions of the investigation. Also, the release year  was already part of the 'release_date' column.

As for columns 'budget' and 'revenue', I decided to keep the updated variables 'budget_adj' and 'revenue_adj' to compare figures more accurately taking inflation into account.

I dropped all the irrelevant columns to simplify the dataset and the analysis.
 

#### b) Replacing special characters

Certain columns contained values separated by pipe characters. I replaced them by commas.

#### c) Figures in scientific notation
As it's not easy to interpret dollar figures in scientific notation, I converted "budget" and "revenue" columns into float formatting

#### d) Null values

I checked and found over 1000 null values in the "production_companies" column. Before dropping all the rows containing null values in that column, I compared the distribution of the dataset to the distribution of the rows containing null values on the "production_companies" column.

The distribution was very similar. The only difference was that in the null values dataset movies had in average less than 50 votes. I decided to go ahead and drop all rows containing null values.

  

#### e) Duplicated values

I found only one duplicated row in the dataset. I dropped the row. 

#### f) Data format

I changed "release_date" datatype to datetime as it had the wrong data format.


```python
## Looking up the descriptive statistics of the df
df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>runtime</th>
      <th>vote_count</th>
      <th>vote_average</th>
      <th>budget_adj</th>
      <th>revenue_adj</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>3679.000</td>
      <td>3679.000</td>
      <td>3679.000</td>
      <td>3679.000</td>
      <td>3679.000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>109.561</td>
      <td>547.413</td>
      <td>6.182</td>
      <td>45364250.989</td>
      <td>142011525.137</td>
    </tr>
    <tr>
      <th>std</th>
      <td>19.855</td>
      <td>894.797</td>
      <td>0.790</td>
      <td>45191098.534</td>
      <td>219481097.448</td>
    </tr>
    <tr>
      <th>min</th>
      <td>26.000</td>
      <td>10.000</td>
      <td>2.200</td>
      <td>0.969</td>
      <td>2.862</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>96.000</td>
      <td>78.000</td>
      <td>5.700</td>
      <td>13816365.762</td>
      <td>20375176.800</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>106.000</td>
      <td>218.000</td>
      <td>6.200</td>
      <td>31020737.363</td>
      <td>65464324.000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>120.000</td>
      <td>595.000</td>
      <td>6.700</td>
      <td>62130112.856</td>
      <td>170436409.521</td>
    </tr>
    <tr>
      <th>max</th>
      <td>338.000</td>
      <td>9767.000</td>
      <td>8.400</td>
      <td>425000000.000</td>
      <td>2827123750.412</td>
    </tr>
  </tbody>
</table>
</div>



#### g) Incoherent values

After running the descriptive statistics on the dataframe, I realized that minimum value of 'budget_adj' column was very low. I sorted the values of the column to check if there are more outliers or values that seem too low for the production of a film.

The first 30 ascending values in column 'budget_adj' seemed too low for the production of a movie. Nevertheless, and as I didn't know which amount is the minimum reasonable for the production of a movie, I decided to keep in mind this limitation of the dataset when interpreting the results.

<a id='eda'></a>
## Exploratory Data Analysis



### Are there companies more successful than others producing well rated movies?

First, I tried to compare the number of production companies in the dataset to the number of companies of the movies with best ratings (75th - 100th percentile, i.e. over 6.7 points). This didn't shred any light onto the question as the number of unique companies in the best rated movies was roughly 30% of the total number of unique production companies of the dataset (729 vs 2905 companies).

Then I checked the distribution of the production companies within that group.


```python
df_p['production_companies'].value_counts().head(20).plot(kind='bar');
```


    
![png](output_21_0.png)
    


As per our dataset, it seems Paramount Pictures, Universal Pictures, Coumbia Pictures and Warner Bros produced some of the best rated (top 25%) movies. Nevertheless, this could be either because of their experience in the industry or because they produce many more movies than other Production companies.

We can see also that Walt Disney is not in the top three but it has coproduced some of the films and produces them under different names: Walt Disney Pictures, Walt Disney Productions, Walt Disney Feature Animation.

As we can see below, the Production Companies producing more well rated movies by TMDb users are also producing many more movies than other Production Companies (in average each company - or compound of companies- produced 1.27 movies) : 


```python
## Count of produced movies per production company, all ratings included. Only first 20,
##descending order
df['production_companies'].value_counts().head(20)
```




    Paramount Pictures                                     75
    Universal Pictures                                     57
    Columbia Pictures                                      39
    New Line Cinema                                        37
    Warner Bros.                                           32
    Metro-Goldwyn-Mayer (MGM)                              25
    Touchstone Pictures                                    23
    Walt Disney Pictures                                   21
    Twentieth Century Fox Film Corporation                 21
    20th Century Fox                                       20
    Orion Pictures                                         17
    Miramax Films                                          17
    Dimension Films                                        16
    TriStar Pictures                                       15
    DreamWorks Animation                                   15
    United Artists                                         15
    Columbia Pictures Corporation                          15
    Walt Disney Pictures, Pixar Animation Studios          13
    Walt Disney Pictures, Walt Disney Feature Animation    12
    Imagine Entertainment, Universal Pictures              11
    Name: production_companies, dtype: int64



In the pie chart below we can see the ten production companies that produced more movies each. These are all big names of the cinema industry.


```python
#Number of movies, all ratings included, produced by first 10 production
##companies with more movies in the dataset
df['production_companies'].value_counts().head(10).plot(kind='pie');
```


    
![png](output_25_0.png)
    


### Are the most expensive movies also the most popular/best rated movies?

Now I would like to see if there is any correlation between the rating of a movie ('vote_average') and the budget used for producing it:


```python
## Plotting ratings against budget
df.plot(x='vote_average', y='budget_adj', kind='scatter', title= 'Correlation between rating and production budget');
```


    
![png](output_28_0.png)
    


There is some positive correlation, but not very strong. There are a lot of movies with a small budget that got a good 'vote_average' in TMDb.

### Is the rating of sequels better than the average rating? What's the distribution of the popularity of movies considered as sequels ?

I personally find most sequels worse than the original title so I would like to see how many of the movies considered as sequels (53) are rated better than the mean value of 'vote_average' (6.182) and how many worse:


```python
mean_rating= df['vote_average'].mean()


above_average=sequels[sequels['vote_average']>mean_rating].count()['original_title']
below_average=sequels[sequels['vote_average']<mean_rating].count()['original_title']
plt.bar(["above_average","below_average"], [above_average,below_average]);
plt.title('Rating of sequels above or below vote_average mean');
plt.ylabel('Vote_average');

```


    
![png](output_32_0.png)
    


### Is there a correlation between budget and profit? 

We've seen that there is no a strong correlation between 'budget' and 'vote_average'. Nevertheless, I'd like to see whether films that were more expensive to produce were also more profitable. 


```python
## plotting on budget and profit to see whether there is a correlation or not.

df.plot(x='budget_adj', y='profit', kind='scatter');
```


    
![png](output_35_0.png)
    


There is a positive correlation which means that as the budget increases the profit sometimes does too. Nevertheless, as per the scatter plot we can see it's not very strong.

### Is there a correlation between profit and good ratings in TMDb?

What about the correlation between 'profit' and 'vote_average'? Let's see if profitable films got a higher 'vote_average' than those that got a smaller profit.


```python
df.plot(x='vote_average', y='profit', kind='scatter');
```


    
![png](output_39_0.png)
    


The correlation is clearly positive between both variables. It means that profitable films got overall better ratings than films that were less profitable.



<a id='conclusions'></a>
## Conclusions



I focused in the ratings of the films of the dataset and their relationship with other variables. 

First, I tried to check if some production companies were better at producing well rated movies. There seem to be some companies that got many more well rated movies than the average of the other production companies. Nevertheless, the analysis has some limitations. Same companies that have produced more well rated movies have also produced many more films, regardless the rating, than the rest of the production companies. This limitation could be overcome by working with proportions. The second limitation is that many companies produce movies together with other production companies. Therefore the number of movies each company has produced is more dificult to determine. 

Second, I wanted to find out if there was some correlation between the most expensive movies and good ratings, i.e.: whether most expensive movies were getting also the highest ratings. Looking to the scatter plot of the two variables, there seems to be some positive correlation but it isn't very strong. There are slightly more films with a big budget that got also better ratings. Nevertheless, there are many films with a budget on the small side that got rated really well. 

Third, I was curious about the rating ('vote_average') of movies classed as sequels. I wanted to know it their rating was over or below the mean of 'vote_average'. I found 53 sequel movies as per the variable 'keyword'. The majority of them (33 out of 53) got a rating below the mean of 'vote_average'. As per this dataset, sequel movies tend to be worse rated overall than movies that aren't a sequel.

Fourth, I calculated the profit of each movie by substracting 'budget_adj' from 'revenue_adj'. I wanted to see whether movies with an important budget are more profitable than those with a small budget. There is no clear correlation between both variables according to the scatter plot I produced: small budget films are profitable more or less as often as big budget films of this dataset. It would be interesting to find out whether there are additional revenues and profit (not only those related to selling cinema tickets) that expensive movies have access to and other movies don't.

Finally, I tried to see if there is any correlation between profitable films and good ratings. As per the scatter plot on variables 'profit' and 'vote_average' it seems to be a positive correlation, i.e. films that are more profitable seem to be also better rated than the average.

An important limitation of the dataset is the number of missing values. There are more than 1000 missing values in the column 'production_companies'. It should be possible to look up the movies concerned by the missing values and find out the name of the production company. Nevertheless, due to the limited time to produce the report I abandoned the idea. I dropped all the null values instead. As a result the dataset was much smaller than the original one.

Another limitation is the one noticed in the column 'budget_adj'. Even after dropping all values equal to zero, there are some very low values in the column. Double checking budget_adj values or dropping rows that don't meet a minimum value could be solutions to improve the quality of the dataset.



```python
from subprocess import call
call(['python', '-m', 'nbconvert', 'Investigate_a_Dataset.ipynb'])
```




    1


