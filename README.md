# SearchTool

This README contains instructions for setting up and using the Search Tool.

The tool can be downloaded from : https://drive.google.com/file/d/1A9x0Oafv6iz1OfzndIbFqntWC3ZhzaGw/view?usp=sharing

## Example : Running the project on Libpng

We provide instructions for running the tool on Libpng, an open source project : https://github.com/glennrp/libpng

### Part 1 : Setting up requirements

1. First, download the tool from the drive link - https://drive.google.com/file/d/1A9x0Oafv6iz1OfzndIbFqntWC3ZhzaGw/view?usp=sharing

Extract the tool (let's say the tool is extracted in folder `SearchTool`)

2. For convenience, create two folders - `projects` and `outputs`. Clone the `libpng` repository inside the `projects` folder.

```
mkdir projects
mkdir outputs
cd projects 
git clone https://github.com/glennrp/libpng
cd ..
```

At this point, the directory structure would look like - 
```
 ├── SearchTool
 ├── projects
 │   ├── libpng
 ├── outputs

```

### Part 2 : Setting up the configuration files

There are two files which need to be configured - a config json file, and a runs json file.

1. config json file - This file is used by `main.py`, which is used to run the tool. An example for the contents of the file is - 
```
{
	"python_path":"python",
	"project_path":"/home/dewang/smartKT/SmartKTRepo/projects/libpng",
	"output_path":"/home/dewang/smartKT/SmartKTRepo/outputs_new/",
	"runs_json_path":"/home/dewang/smartKT/SmartKTRepo/runs/runs_libpng.json"
}
```

The details for each key are 
- `python_path` - The path or name with which to use python. As the tool uses `os.system` and runs other python scripts, it needs to know how to call python. An example situation is - If you use `python3` to run python, then you can change python_path to "python3".

- `project_path` : The path where the project has been cloned.
- `output_path` : The path where the output of the project will be stored. A new folder with same name as project will be created there.
- `runs_json_path` : The path to the runs json file. More details about this file are mentioned below in the README.

2. runs json file - This file is used to configure the binary related information about the project. An example for the contents of the file is - 

```
{
    "runs": {
        "/home/dewang/smartKT/SmartKTRepo/projects/libpng/build/pngtest": {
            "/home/dewang/smartKT/SmartKTRepo/projects/libpng/pngtest.png" : 1,
            "/home/dewang/smartKT/SmartKTRepo/projects/libpng/pngbar.png": 1
        }
    },
    "comments": {
        "project_name": "libpng",
        "project_path": "/home/dewang/smartKT/SmartKTRepo/projects/libpng",
        "vocab_loc": "/home/dewang/smartKT/SmartKTRepo/Semantic-Search-Tool/kgg/parsers/comments/program_domain.csv",
        "problem_domain_loc": "/home/dewang/smartKT/SmartKTRepo/Semantic-Search-Tool/kgg/parsers/comments/ProblemDomains/libpng/problem_domain.txt"
    }
}
```

The content of runs json file has two keys : `runs` and `comments`. 

The value of `runs` keys is an object with name of binary (which would be generated after the project is compiled) and the value is an object whose keys are inputs and value is number of runs.

The value of `comments` key is an object with keys - `project_name`, `project_path`, `vocab_loc`, `problem_domain_loc`.

`project_name` and `project_path` are the name and the path to the target project. 

`vocab_loc` and `problem_domain_loc` files are part of the kgg subfolder. Either they can be used directly, or similar files can be created and their paths given in this runs json file.


Configure these two files appropriately based on the absolute paths for the machine.

### Part 3 : Run the tool

The tool can be run by `python main.py <path to config json file>`

So, use 

```
python main.py config.json
```

The logs would be similar to `full_log.txt`. Please check the logs to confirm whether the tool ran successfully or not.



### Part 4 : Running the Query Engine

The script `main.py` prints the required files for Query Engine at the end. An example of the required files printed at the end of `main.py` are - 

```
all_files = '/home/dewang/smartKT/SmartKTRepo/outputs_new/libpng/exe_pngtest/files.p'
all_name_tokens_dict = '/home/dewang/smartKT/SmartKTRepo/outputs_new/libpng/exe_pngtest/name_tokens_dict.p'
all_comment_tokens_dict = '/home/dewang/smartKT/SmartKTRepo/outputs_new/libpng/exe_pngtest/comment_tokens_dict.p'
program_domain_dict = '/home/dewang/smartKT/SmartKTRepo/outputs_new/libpng/exe_pngtest/program_domain_dict.p'
tf_idf_name_tokens = '/home/dewang/smartKT/SmartKTRepo/outputs_new/libpng/exe_pngtest/tf_idf_name_tokens.p'
tf_idf_symbol = '/home/dewang/smartKT/SmartKTRepo/outputs_new/libpng/exe_pngtest/tf_idf_symbol.p'
all_store_file = '/home/dewang/smartKT/SmartKTRepo/outputs_new/libpng/exe_pngtest/all_store.p'
TTLfile = '/home/dewang/smartKT/SmartKTRepo/outputs_new/libpng/exe_pngtest/final.ttl'
```

These paths must be set for `init.py` in `SearchTool/qe/Server_full_archi_code_files` folder. Therefore, replace the lines 15-22 in the `init.py` script with the paths printed by `main.py`.


### Notes : 

1. Using absolute paths is preferred
2. A few files are given in folder `const_files` and they are project-dependent but not generated by the SearchTool. The folder `const_files` has the files for `libpng` project.
The files are - `comment_top10_similar_model_200_W10_CBOW_NEG5.csv`, `crossSimilarity_matrix.csv`, `name_token_similar.csv`.

