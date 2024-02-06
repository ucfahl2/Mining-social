GEOG0051 Formative Assessment

Haoxuan Liu

ucfahl2@ucl.ac.uk

**Introduction**


This short analysis aims to answer the question of whether closeness centrality and betweenness centrality has an impact on the locations of cafes near Edgeware Road. A cafe is usually an informal establishment that provides light meals and drinks, with an emphasis on coffee and tea. The assumption of this analysis is that the performance of businesses like cafe might be more dependent upon the geolocation and hence connectivity or road networks comparing to the other businesses.


**Dataset and the related neighbourhood**

 Edgeware Road and its nearby neighbourhoods are generally considered to be one of the busiest areas in central London, given the presence of both Marylebone and Paddington within 2000m radius. From my experience of living nearby, most of the cafes and coffee shops are located near Marylebone Road and Baker Street, where most tourists and travellers would visit and pass by. In contrast, the cafes are rarely seen near the residential buildings or other public facilities like Church or Children’s centre, which may not have a comparable connectivity.

**Results and Visualisations**
(see in the attached png files)


**Discussion**

 As the closeness centrality plot reveals, there are visibly more cafes in highlighted area, especially around the Marylebone Station. However, closeness centrality does not explain the cluster near Mayfair. This might be due to the presence of commercial centres and shopping malls like the Bond Street and the Selfridges which are likely to be the destinations for the customers. In other words, the café customers in this area are not necessarily drawn to the cafes because the location of the cafes are more accessible but simply because they are close to their destination. Another finding is that the betweenness centrality does align with the locations of the cafes. There are rarely any cafes located around these highlighted roads/edges, which might be due to the higher rent or that people tend to choose other public transports instead of walking.


```python
#for colab
#!pip install osmnx
#!pip install contextily
```

    Collecting contextily
      Downloading contextily-1.5.0-py3-none-any.whl (17 kB)
    Requirement already satisfied: geopy in /usr/local/lib/python3.10/dist-packages (from contextily) (2.3.0)
    Requirement already satisfied: matplotlib in /usr/local/lib/python3.10/dist-packages (from contextily) (3.7.1)
    Collecting mercantile (from contextily)
      Downloading mercantile-1.2.1-py3-none-any.whl (14 kB)
    Requirement already satisfied: pillow in /usr/local/lib/python3.10/dist-packages (from contextily) (9.4.0)
    Collecting rasterio (from contextily)
      Downloading rasterio-1.3.9-cp310-cp310-manylinux2014_x86_64.whl (20.6 MB)
    [2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m20.6/20.6 MB[0m [31m50.2 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: requests in /usr/local/lib/python3.10/dist-packages (from contextily) (2.31.0)
    Requirement already satisfied: joblib in /usr/local/lib/python3.10/dist-packages (from contextily) (1.3.2)
    Requirement already satisfied: xyzservices in /usr/local/lib/python3.10/dist-packages (from contextily) (2023.10.1)
    Requirement already satisfied: geographiclib<3,>=1.52 in /usr/local/lib/python3.10/dist-packages (from geopy->contextily) (2.0)
    Requirement already satisfied: contourpy>=1.0.1 in /usr/local/lib/python3.10/dist-packages (from matplotlib->contextily) (1.2.0)
    Requirement already satisfied: cycler>=0.10 in /usr/local/lib/python3.10/dist-packages (from matplotlib->contextily) (0.12.1)
    Requirement already satisfied: fonttools>=4.22.0 in /usr/local/lib/python3.10/dist-packages (from matplotlib->contextily) (4.47.2)
    Requirement already satisfied: kiwisolver>=1.0.1 in /usr/local/lib/python3.10/dist-packages (from matplotlib->contextily) (1.4.5)
    Requirement already satisfied: numpy>=1.20 in /usr/local/lib/python3.10/dist-packages (from matplotlib->contextily) (1.23.5)
    Requirement already satisfied: packaging>=20.0 in /usr/local/lib/python3.10/dist-packages (from matplotlib->contextily) (23.2)
    Requirement already satisfied: pyparsing>=2.3.1 in /usr/local/lib/python3.10/dist-packages (from matplotlib->contextily) (3.1.1)
    Requirement already satisfied: python-dateutil>=2.7 in /usr/local/lib/python3.10/dist-packages (from matplotlib->contextily) (2.8.2)
    Requirement already satisfied: click>=3.0 in /usr/local/lib/python3.10/dist-packages (from mercantile->contextily) (8.1.7)
    Collecting affine (from rasterio->contextily)
      Downloading affine-2.4.0-py3-none-any.whl (15 kB)
    Requirement already satisfied: attrs in /usr/local/lib/python3.10/dist-packages (from rasterio->contextily) (23.2.0)
    Requirement already satisfied: certifi in /usr/local/lib/python3.10/dist-packages (from rasterio->contextily) (2023.11.17)
    Requirement already satisfied: cligj>=0.5 in /usr/local/lib/python3.10/dist-packages (from rasterio->contextily) (0.7.2)
    Collecting snuggs>=1.4.1 (from rasterio->contextily)
      Downloading snuggs-1.4.7-py3-none-any.whl (5.4 kB)
    Requirement already satisfied: click-plugins in /usr/local/lib/python3.10/dist-packages (from rasterio->contextily) (1.1.1)
    Requirement already satisfied: setuptools in /usr/local/lib/python3.10/dist-packages (from rasterio->contextily) (67.7.2)
    Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests->contextily) (3.3.2)
    Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.10/dist-packages (from requests->contextily) (3.6)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests->contextily) (2.0.7)
    Requirement already satisfied: six>=1.5 in /usr/local/lib/python3.10/dist-packages (from python-dateutil>=2.7->matplotlib->contextily) (1.16.0)
    Installing collected packages: snuggs, mercantile, affine, rasterio, contextily
    Successfully installed affine-2.4.0 contextily-1.5.0 mercantile-1.2.1 rasterio-1.3.9 snuggs-1.4.7





```python
# imports the various library for the lab

import numpy as np
import pandas as pd
import matplotlib
import matplotlib.pyplot as plt
import osmnx as ox
import networkx as nx
import matplotlib.cm as cm
import matplotlib.colors as colors

```

    Collecting osmnx
      Downloading osmnx-1.9.1-py3-none-any.whl (104 kB)
    [?25l     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m0.0/104.3 kB[0m [31m?[0m eta [36m-:--:--[0m[2K     [91m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m[90m╺[0m [32m102.4/104.3 kB[0m [31m3.1 MB/s[0m eta [36m0:00:01[0m[2K     [90m━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━[0m [32m104.3/104.3 kB[0m [31m2.5 MB/s[0m eta [36m0:00:00[0m
    [?25hRequirement already satisfied: geopandas>=0.12 in /usr/local/lib/python3.10/dist-packages (from osmnx) (0.13.2)
    Requirement already satisfied: networkx>=2.5 in /usr/local/lib/python3.10/dist-packages (from osmnx) (3.2.1)
    Requirement already satisfied: numpy>=1.20 in /usr/local/lib/python3.10/dist-packages (from osmnx) (1.23.5)
    Requirement already satisfied: pandas>=1.1 in /usr/local/lib/python3.10/dist-packages (from osmnx) (1.5.3)
    Requirement already satisfied: requests>=2.27 in /usr/local/lib/python3.10/dist-packages (from osmnx) (2.31.0)
    Requirement already satisfied: shapely>=2.0 in /usr/local/lib/python3.10/dist-packages (from osmnx) (2.0.2)
    Requirement already satisfied: fiona>=1.8.19 in /usr/local/lib/python3.10/dist-packages (from geopandas>=0.12->osmnx) (1.9.5)
    Requirement already satisfied: packaging in /usr/local/lib/python3.10/dist-packages (from geopandas>=0.12->osmnx) (23.2)
    Requirement already satisfied: pyproj>=3.0.1 in /usr/local/lib/python3.10/dist-packages (from geopandas>=0.12->osmnx) (3.6.1)
    Requirement already satisfied: python-dateutil>=2.8.1 in /usr/local/lib/python3.10/dist-packages (from pandas>=1.1->osmnx) (2.8.2)
    Requirement already satisfied: pytz>=2020.1 in /usr/local/lib/python3.10/dist-packages (from pandas>=1.1->osmnx) (2023.4)
    Requirement already satisfied: charset-normalizer<4,>=2 in /usr/local/lib/python3.10/dist-packages (from requests>=2.27->osmnx) (3.3.2)
    Requirement already satisfied: idna<4,>=2.5 in /usr/local/lib/python3.10/dist-packages (from requests>=2.27->osmnx) (3.6)
    Requirement already satisfied: urllib3<3,>=1.21.1 in /usr/local/lib/python3.10/dist-packages (from requests>=2.27->osmnx) (2.0.7)
    Requirement already satisfied: certifi>=2017.4.17 in /usr/local/lib/python3.10/dist-packages (from requests>=2.27->osmnx) (2023.11.17)
    Requirement already satisfied: attrs>=19.2.0 in /usr/local/lib/python3.10/dist-packages (from fiona>=1.8.19->geopandas>=0.12->osmnx) (23.2.0)
    Requirement already satisfied: click~=8.0 in /usr/local/lib/python3.10/dist-packages (from fiona>=1.8.19->geopandas>=0.12->osmnx) (8.1.7)
    Requirement already satisfied: click-plugins>=1.0 in /usr/local/lib/python3.10/dist-packages (from fiona>=1.8.19->geopandas>=0.12->osmnx) (1.1.1)
    Requirement already satisfied: cligj>=0.5 in /usr/local/lib/python3.10/dist-packages (from fiona>=1.8.19->geopandas>=0.12->osmnx) (0.7.2)
    Requirement already satisfied: six in /usr/local/lib/python3.10/dist-packages (from fiona>=1.8.19->geopandas>=0.12->osmnx) (1.16.0)
    Requirement already satisfied: setuptools in /usr/local/lib/python3.10/dist-packages (from fiona>=1.8.19->geopandas>=0.12->osmnx) (67.7.2)
    Installing collected packages: osmnx
    Successfully installed osmnx-1.9.1



```python
G=ox.graph_from_address('Edgeware Road, London', dist=2000, network_type='walk')
ox.plot_graph(G)
```


    
![png](Formative_GEOG0051_files/Formative_GEOG0051_6_0.png)
    





    (<Figure size 800x800 with 1 Axes>, <Axes: >)




```python
DG = ox.get_digraph(G)

edge_cc = nx.closeness_centrality(nx.line_graph(DG))

nx.set_edge_attributes(DG,edge_cc,'cc')
G1 = nx.MultiGraph(DG)

nc = ox.plot.get_edge_colors_by_attr(G1, 'cc', cmap='plasma')
#fig, ax = ox.plot_graph(G1, node_size=0, node_color='w', node_edgecolor='gray', node_zorder=2,
                        edge_color=nc, edge_linewidth=1.5, edge_alpha=1)
```


    
![png](Formative_GEOG0051_files/Formative_GEOG0051_7_0.png)
    



```python
# convert graph to geopandas dataframe
gdf_edges = ox.graph_to_gdfs(G1,nodes=False,fill_edge_geometry=True)

# set crs to 3857 (needed for contextily)
gdf_edges = gdf_edges.to_crs(epsg=3857) # setting crs to 3857

# plot edges according to degree centrality
ax=gdf_edges.plot('cc',cmap='plasma',figsize=(10,10))

# add a basemap using contextilly
import contextily as ctx
ctx.add_basemap(ax,source=ctx.providers.CartoDB.Positron)
plt.axis('off')
#plt.show()
```


    
![png](Formative_GEOG0051_files/Formative_GEOG0051_8_0.png)
    



```python
edge_bc = nx.betweenness_centrality(nx.line_graph(DG))
nx.set_edge_attributes(DG,edge_bc,'bc')
G1 = nx.MultiGraph(DG)
# convert graph to geopandas dataframe
gdf_edges = ox.graph_to_gdfs(G1,nodes=False,fill_edge_geometry=True)

# set crs to 3857 (needed for contextily)
gdf_edges = gdf_edges.to_crs(epsg=3857) # setting crs to 3857

# plot edges according to degree centrality
ax=gdf_edges.plot('bc',cmap='plasma',figsize=(10,10))

# add a basemap using contextilly
import contextily as ctx
ctx.add_basemap(ax,source=ctx.providers.CartoDB.Positron)
plt.axis('off')
plt.show()
```


    
![png](Formative_GEOG0051_files/Formative_GEOG0051_9_0.png)
    



```python
tag = {'amenity': 'cafe' }
cafe_geom = ox.geometries.geometries_from_address('Edgeware Road, London', tag, dist=2000)
cafe_geom = cafe_geom.to_crs(epsg=3857)

fig, ax = plt.subplots(figsize=(10, 10))
cafe_geom[cafe_geom['amenity'] == 'cafe'].plot(ax=ax, color='blue', alpha=0.7)
ctx.add_basemap(ax, source=ctx.providers.CartoDB.Positron)  # add a basemap
plt.axis('off')
plt.savefig('cafes_map_points.png', bbox_inches='tight', pad_inches=0, dpi=300)
plt.show()
```

    <ipython-input-30-0e135c8c634c>:2: FutureWarning: The `geometries` module and `geometries_from_X` functions have been renamed the `features` module and `features_from_X` functions. Use these instead. The `geometries` module and function names are deprecated and will be removed in the v2.0.0 release.
      cafe_geom = ox.geometries.geometries_from_address('Edgeware Road, London', tag, dist=2000)



    
![png](Formative_GEOG0051_files/Formative_GEOG0051_10_1.png)
    



```python
from google.colab import files
#files.download('cafes_map_points.png')
```

    /usr/local/lib/python3.10/dist-packages/ipykernel/ipkernel.py:283: DeprecationWarning: `should_run_async` will not call `transform_cell` automatically in the future. Please pass the result to `transformed_cell` argument and any exception that happen during thetransform in `preprocessing_exc_tuple` in IPython 7.17 and above.
      and should_run_async(code)



    <IPython.core.display.Javascript object>



    <IPython.core.display.Javascript object>





```python
fig, ax = plt.subplots(figsize=(10, 10))
gdf_edges.plot(ax=ax, column='bc', cmap='plasma', markersize=10, legend=False)

# overlay cafe locations
cafe_geom.plot(ax=ax, color='green', alpha=0.7, markersize=40)

# add basemap
ctx.add_basemap(ax, source=ctx.providers.CartoDB.Positron)
plt.axis('off')
plt.savefig('cafes_map_bc.png', bbox_inches='tight', pad_inches=0, dpi=300)
plt.show()




```


    
![png](Formative_GEOG0051_files/Formative_GEOG0051_13_0.png)
    



```python
from google.colab import files
#files.download('cafes_map_bc.png')
```

    /usr/local/lib/python3.10/dist-packages/ipykernel/ipkernel.py:283: DeprecationWarning: `should_run_async` will not call `transform_cell` automatically in the future. Please pass the result to `transformed_cell` argument and any exception that happen during thetransform in `preprocessing_exc_tuple` in IPython 7.17 and above.
      and should_run_async(code)



    <IPython.core.display.Javascript object>



    <IPython.core.display.Javascript object>



```python
fig, ax = plt.subplots(figsize=(10, 10))
gdf_edges.plot(ax=ax, column='cc', cmap='plasma', markersize=10, legend=False)

# Overlay cafe locations
cafe_geom.plot(ax=ax, color='green', alpha=0.7, markersize=40)  # Adjust markersize as needed

# Add basemap
ctx.add_basemap(ax, source=ctx.providers.CartoDB.Positron)
plt.axis('off')
plt.savefig('cafes_map.png', bbox_inches='tight', pad_inches=0, dpi=300)
plt.show()
```


    
![png](Formative_GEOG0051_files/Formative_GEOG0051_15_0.png)
    



```python
from google.colab import files
#files.download('cafes_map.png')

```


    <IPython.core.display.Javascript object>



    <IPython.core.display.Javascript object>



```python
!pip show nbconvert || pip install nbconvert
!jupyter nbconvert --to markdown "Formative_GEOG0051.ipynb"

```

    Name: nbconvert
    Version: 6.5.4
    Summary: Converting Jupyter Notebooks
    Home-page: https://jupyter.org
    Author: Jupyter Development Team
    Author-email: jupyter@googlegroups.com
    License: BSD
    Location: /usr/local/lib/python3.10/dist-packages
    Requires: beautifulsoup4, bleach, defusedxml, entrypoints, jinja2, jupyter-core, jupyterlab-pygments, lxml, MarkupSafe, mistune, nbclient, nbformat, packaging, pandocfilters, pygments, tinycss2, traitlets
    Required-by: jupyter-server, nbclassic, notebook
    [NbConvertApp] WARNING | pattern 'Formative_GEOG0051.ipynb' matched no files
    This application is used to convert notebook files (*.ipynb)
            to various other formats.
    
            WARNING: THE COMMANDLINE INTERFACE MAY CHANGE IN FUTURE RELEASES.
    
    Options
    =======
    The options below are convenience aliases to configurable class-options,
    as listed in the "Equivalent to" description-line of the aliases.
    To see all configurable class-options for some <cmd>, use:
        <cmd> --help-all
    
    --debug
        set log level to logging.DEBUG (maximize logging output)
        Equivalent to: [--Application.log_level=10]
    --show-config
        Show the application's configuration (human-readable format)
        Equivalent to: [--Application.show_config=True]
    --show-config-json
        Show the application's configuration (json format)
        Equivalent to: [--Application.show_config_json=True]
    --generate-config
        generate default config file
        Equivalent to: [--JupyterApp.generate_config=True]
    -y
        Answer yes to any questions instead of prompting.
        Equivalent to: [--JupyterApp.answer_yes=True]
    --execute
        Execute the notebook prior to export.
        Equivalent to: [--ExecutePreprocessor.enabled=True]
    --allow-errors
        Continue notebook execution even if one of the cells throws an error and include the error message in the cell output (the default behaviour is to abort conversion). This flag is only relevant if '--execute' was specified, too.
        Equivalent to: [--ExecutePreprocessor.allow_errors=True]
    --stdin
        read a single notebook file from stdin. Write the resulting notebook with default basename 'notebook.*'
        Equivalent to: [--NbConvertApp.from_stdin=True]
    --stdout
        Write notebook output to stdout instead of files.
        Equivalent to: [--NbConvertApp.writer_class=StdoutWriter]
    --inplace
        Run nbconvert in place, overwriting the existing notebook (only
                relevant when converting to notebook format)
        Equivalent to: [--NbConvertApp.use_output_suffix=False --NbConvertApp.export_format=notebook --FilesWriter.build_directory=]
    --clear-output
        Clear output of current file and save in place,
                overwriting the existing notebook.
        Equivalent to: [--NbConvertApp.use_output_suffix=False --NbConvertApp.export_format=notebook --FilesWriter.build_directory= --ClearOutputPreprocessor.enabled=True]
    --no-prompt
        Exclude input and output prompts from converted document.
        Equivalent to: [--TemplateExporter.exclude_input_prompt=True --TemplateExporter.exclude_output_prompt=True]
    --no-input
        Exclude input cells and output prompts from converted document.
                This mode is ideal for generating code-free reports.
        Equivalent to: [--TemplateExporter.exclude_output_prompt=True --TemplateExporter.exclude_input=True --TemplateExporter.exclude_input_prompt=True]
    --allow-chromium-download
        Whether to allow downloading chromium if no suitable version is found on the system.
        Equivalent to: [--WebPDFExporter.allow_chromium_download=True]
    --disable-chromium-sandbox
        Disable chromium security sandbox when converting to PDF..
        Equivalent to: [--WebPDFExporter.disable_sandbox=True]
    --show-input
        Shows code input. This flag is only useful for dejavu users.
        Equivalent to: [--TemplateExporter.exclude_input=False]
    --embed-images
        Embed the images as base64 dataurls in the output. This flag is only useful for the HTML/WebPDF/Slides exports.
        Equivalent to: [--HTMLExporter.embed_images=True]
    --sanitize-html
        Whether the HTML in Markdown cells and cell outputs should be sanitized..
        Equivalent to: [--HTMLExporter.sanitize_html=True]
    --log-level=<Enum>
        Set the log level by value or name.
        Choices: any of [0, 10, 20, 30, 40, 50, 'DEBUG', 'INFO', 'WARN', 'ERROR', 'CRITICAL']
        Default: 30
        Equivalent to: [--Application.log_level]
    --config=<Unicode>
        Full path of a config file.
        Default: ''
        Equivalent to: [--JupyterApp.config_file]
    --to=<Unicode>
        The export format to be used, either one of the built-in formats
                ['asciidoc', 'custom', 'html', 'latex', 'markdown', 'notebook', 'pdf', 'python', 'rst', 'script', 'slides', 'webpdf']
                or a dotted object name that represents the import path for an
                ``Exporter`` class
        Default: ''
        Equivalent to: [--NbConvertApp.export_format]
    --template=<Unicode>
        Name of the template to use
        Default: ''
        Equivalent to: [--TemplateExporter.template_name]
    --template-file=<Unicode>
        Name of the template file to use
        Default: None
        Equivalent to: [--TemplateExporter.template_file]
    --theme=<Unicode>
        Template specific theme(e.g. the name of a JupyterLab CSS theme distributed
        as prebuilt extension for the lab template)
        Default: 'light'
        Equivalent to: [--HTMLExporter.theme]
    --sanitize_html=<Bool>
        Whether the HTML in Markdown cells and cell outputs should be sanitized.This
        should be set to True by nbviewer or similar tools.
        Default: False
        Equivalent to: [--HTMLExporter.sanitize_html]
    --writer=<DottedObjectName>
        Writer class used to write the
                                            results of the conversion
        Default: 'FilesWriter'
        Equivalent to: [--NbConvertApp.writer_class]
    --post=<DottedOrNone>
        PostProcessor class used to write the
                                            results of the conversion
        Default: ''
        Equivalent to: [--NbConvertApp.postprocessor_class]
    --output=<Unicode>
        overwrite base name use for output files.
                    can only be used when converting one notebook at a time.
        Default: ''
        Equivalent to: [--NbConvertApp.output_base]
    --output-dir=<Unicode>
        Directory to write output(s) to. Defaults
                                      to output to the directory of each notebook. To recover
                                      previous default behaviour (outputting to the current
                                      working directory) use . as the flag value.
        Default: ''
        Equivalent to: [--FilesWriter.build_directory]
    --reveal-prefix=<Unicode>
        The URL prefix for reveal.js (version 3.x).
                This defaults to the reveal CDN, but can be any url pointing to a copy
                of reveal.js.
                For speaker notes to work, this must be a relative path to a local
                copy of reveal.js: e.g., "reveal.js".
                If a relative path is given, it must be a subdirectory of the
                current directory (from which the server is run).
                See the usage documentation
                (https://nbconvert.readthedocs.io/en/latest/usage.html#reveal-js-html-slideshow)
                for more details.
        Default: ''
        Equivalent to: [--SlidesExporter.reveal_url_prefix]
    --nbformat=<Enum>
        The nbformat version to write.
                Use this to downgrade notebooks.
        Choices: any of [1, 2, 3, 4]
        Default: 4
        Equivalent to: [--NotebookExporter.nbformat_version]
    
    Examples
    --------
    
        The simplest way to use nbconvert is
    
                > jupyter nbconvert mynotebook.ipynb --to html
    
                Options include ['asciidoc', 'custom', 'html', 'latex', 'markdown', 'notebook', 'pdf', 'python', 'rst', 'script', 'slides', 'webpdf'].
    
                > jupyter nbconvert --to latex mynotebook.ipynb
    
                Both HTML and LaTeX support multiple output templates. LaTeX includes
                'base', 'article' and 'report'.  HTML includes 'basic', 'lab' and
                'classic'. You can specify the flavor of the format used.
    
                > jupyter nbconvert --to html --template lab mynotebook.ipynb
    
                You can also pipe the output to stdout, rather than a file
    
                > jupyter nbconvert mynotebook.ipynb --stdout
    
                PDF is generated via latex
    
                > jupyter nbconvert mynotebook.ipynb --to pdf
    
                You can get (and serve) a Reveal.js-powered slideshow
    
                > jupyter nbconvert myslides.ipynb --to slides --post serve
    
                Multiple notebooks can be given at the command line in a couple of
                different ways:
    
                > jupyter nbconvert notebook*.ipynb
                > jupyter nbconvert notebook1.ipynb notebook2.ipynb
    
                or you can specify the notebooks list in a config file, containing::
    
                    c.NbConvertApp.notebooks = ["my_notebook.ipynb"]
    
                > jupyter nbconvert --config mycfg.py
    
    To see all available configurables, use `--help-all`.
    



```python
from google.colab import drive
drive.mount('/content/drive')

# Adjust the path below to the location of your notebook in Google Drive
!jupyter nbconvert --to markdown "/content/drive/My Drive/Path/To/YourNotebook.ipynb"

```
