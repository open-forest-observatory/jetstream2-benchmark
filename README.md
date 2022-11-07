# jetstream2-benchmark
Instructions and code to benchmark metashape processing performance on Jetstream2 (and other computing platforms).

This is a simplified and more specific version of the instructions in the [automate-metashape](https://github.com/open-forest-observatory/automate-metashape) repo.

The instructions assume a bash-like shell.

## Steps

1. Download the [benchmarking imagery dataset](https://ucdavis.box.com/s/vezqf93xig710t0aunv5te6qbzuyh0si). It is a set of 1000 photos from a P4RTK captured in 2022 from a portion of the 2021 Caldor Fire at 120 m with 90/90 overlap. These instructions will assume you downloaded the images to `~/Documents/jetstream2-benchmark-dataset/`. TODO: Move the imagery dataset to CyVerse public storage.

1. Clone this repo. These instructions will assume you cloned it to `~/repos/jetstream2-benchmark`. This is really just to get the one YAML config file that is stored in `configs/`. 

1. Cone the [automate-metashape](https://github.com/open-forest-observatory/automate-metashape) repo. These instructions will assume you cloned it to `~/repos/automate-metashape`. This is the code to run Metashape (via the Python API) following the parameters specified in a YAML config file accessed via the previous step. 

1. Create a Conda environment with the right Python version and current PyYaml library: `conda create --name meta1.8.4 python=3.8 PyYaml`

1. Activate the Conda environment: `conda activate meta1.8.4`

1. Download the [Metashape Python API module (.whl) file](https://www.agisoft.com/downloads/installer/)

1. Install it: `pip install Metashape-1.8.4-cp35.cp36.cp37.cp38-abi3-linux_x86_64.whl`

1. Set up the license. Once you have a license file (whether a node-locked or floating license), you need to set the `agisoft_LICENSE` environment variable (search onilne for instructions for your OS; look for how to permanently set it) to the path to the folder containing the license file (metashape.lic). You can test that the license is working by starting an interactive Python session (type `python` at the command line) and, within the session, type `import Metashape`. It should give you a new line to accept your next command immediately. If the license is not set up right, it wll hang for a few seconds and then give an error.

1. Edit the config file from your cloned copy of this repo (assumed to be located at `~/repos/jetstream2-benchmark/configs/config-benchmark_01.yml`) so that the file paths for the entries `photo_path`, `output_path`, and `project_path` match the locations of the input (imagery) and desired locations of the outputs. Refer to the comments in the config file for descriptions of what input and output is associated with what directory. Also optionally edit the `run_name` entry to something that's meaningful to you. The rest of the config file should be left as is to create a run that is identical to the benchmarking run used across platforms.

1. Launch the run! `python ~/repos/automate-metashape/python/metashape_workflow.py ~/repos/jetstream2-benchmark/configs/config-benchmark_01.yml` Edit the paths to the workflow script and config file if you did not clone the repos to the locations assumed above. 

Data outputs, a processing log (including timings for each step), and a barebones copy of the config settings will be saved to the directory specified by  `output_path` in the YAML config file, with the filenames of each output prefixed with the `run_name` specified in the config file, plus a timestamp of when the run started.
