[[/Command/Automatic import conversion]] -- Automatic conversion of input files 

# Synopsis

~~~
bash$ gridlabd <filename>.<ext>
~~~

# Description

The GridLAB-D GLM loader will automatically convert to GLM format all input files that have "known" extensions.  A known extension is one for which a Python converter exists in GLPATH. Python converters use the naming convention `<ext>2glm.py` and are normally stored in the GridLAB-D `shared` folder.

Some converters require a hint regarding the type of conversion to use.  For example, the syntax to load a CYME model is

~~~
bash$ gridlabd -D mdb_load_options="-t cyme" <model-name>.mdb
~~~

## Support File Formats

### `ZIP`

When a ZIP file is specified as an input file, the contents of the archive are extracted into the same folder as the ZIP file, and the corresponding GLM file is run. If the GLM file is already present, it will be overwritten.

### `csv-config`

Input converters can be used to load CSV configuration files.  The standard method for configuring a simulation is as follows.

1. Create the CSV file with the global variable definitions, i.e., `config.csv`

~~~
VARIABLE1,value1
VARIABLE2,value2
...
VARIABLEN,valueN
~~~ 

2. Load the configuration file in the GLM file

~~~
#input "config.csv" -f config -t config
~~~

After the load `#input` macro is done, the global variables listed file will be defined as specified in `config.csv`.

#### Options

The import converter supports the following options.

##### `prefix`

Using the `-p prefix=<str>` option allows all the variables generated by the converter to have a prefix to avoid conflicts with existing variable names.

##### `relax`

Using the `-p relax=True` option allows the variables to be redefined (i.e., using `#set` instead of `#define`) without causing an error or warning.

### `csv-modify`

Input converters can be used to load CSV modifications to objects.  The standard for modifying object properties after they are created, but before the simulation starts is as follows.

1. Create the CSV file with the object property modifications, i.e., `modify.csv`

~~~
object-name1.object-property1,value1
object-name2.object-property2,value2
...
object-nameN.object-propertyN,valueN
~~~

2. Load the modification file in the GLM file

~~~
#input "modify.csv" -f modify -t modify
~~~

## Converter Development

Converters use the following calling syntax:

~~~
bash$ python3 <ext>2glm.py -i filename.<ext> -o filename.glm [options ...]
~~~

where `<ext>` is the extension that is supported.

# Examples

Extract files in `my-model.zip` and run `my-model.glm`:

~~~
bash$ gridlabd my-model.zip
~~~

# Caveats

When a converter is run, the input file is converted in place to a GLM file by the same name, but with the `.glm` extension instead.  If the GLM file already exists, **the existing GLM file may be overwritten**. 

# See also

* [[/Command/JSON input]]
* [[/Command/JSON output]]
