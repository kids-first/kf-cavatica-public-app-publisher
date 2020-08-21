<p align="center">
  <img src="docs/kids_first_logo.svg" alt="Kids First repository logo" width="660px" />
</p>
<p align="center">
  <a href="https://github.com/kids-first/kf-template-repo/blob/master/LICENSE"><img src="https://img.shields.io/github/license/kids-first/kf-template-repo.svg?style=for-the-badge"></a>
</p>

# Kids First Cavatica Public App Pubplisher

This repo contains a script to patch CWL tool and/or workflow scripts for public app use on Cavatica.
It satisfies the requirements as laid out [here](https://www.notion.so/d3b/Cavatica-public-app-publishing-e8b8f28f0a2544ecbfe641855a8c1efb)

## cavatica_app_pub.py

### Requirements
 - python3
 - ruamel yaml: `pip install ruamel.yaml`

### Inputs
```python
parser.add_argument('-i', '--input-cwl', action='store', dest='cwl', help='Input cwl file',required=True)
parser.add_argument('-r', '--readme', action='store', dest='readme', help='Readme file to insert into workflow/tool doc, if applicable', required=False)
parser.add_argument('-n', '--id-name', action='store', dest='id_name', help='Short app ID link name to use, i.e. kfdrc-align-wf', required=False)
parser.add_argument('-l', '--label', action='store', dest='label', help='User-friendly label to add to tool/workflow cwl, if needed', required=False)
parser.add_argument('-t', '--tags', action='store', dest='tags', help='Seven bridges tags file, as csv string, ex RNASEQ,FUSION', required=False)
parser.add_argument('-f', '--files', action='store', dest='files', help='Cavatica-style tsv manifest with file ID, file name, and associated cwl input key', required=False)
parser.add_argument('-p', '--publisher', action='store', dest='pub', help='Publisher name', required=False, default="KFDRC")
```

### Input tips
1. `-r, --readme`: There is no need to copy in or write up a README and comprehensive doc section. The README can be fed to the script and it will automatically place it. Also, in the README, put the general workflow description first before the main header and logo - that way the public app preview will display properly
1. `-n, --id-name`: Short app ID, should be `kfdrc-what-it-do-tool/workflow`
1. `-l, --label`: User-friendly display name, like `Kids First DRC Alignment Workflow`
1. `-t, --tags`: Keywords in csv string format that users will be able to search, like `DNA,ALIGNMENT`
1. `-f, --files`: tsv file manifest with file IDs, file names, and cwl input references. Use `template_files/align_inputs_manifest_APP_PUB.tsv` as an example. If input is an array, just put one per line, usi ng same input key. If order matters, then order them in the correct way
1. `-p, --publisher`: simple identifier of center, likely `KFDRC`, unless it;s another center

### General tips
1. All but the input cwl is optional, so if you want to fix something later, only pick the field you need to update
1. The file will be output to stdout, it's probably a good idea if you want to keep he file name, to rename the old one until the PR is approved
1. After updating the cwl, push the app to make sure it loads and copies/pre-populates files
1. Check the app preview after uploading

## cp_files_to_project.py
Simple script to copy final references from a source project to apps publishing project.
It will search the destination project and, if the file name already exists, use the existing file ID of the destination, or copy the file if not, and provide the new file ID. Output is to stdout

### Inputs
```python
parser.add_argument('-f', '--files', action='store', dest='files', help='Cavatica-style tsv manifest with file ID, file name, and associated cwl input key', required=False)
parser.add_argument('-p', '--project', action='store', dest='project', help='Project to copy to', required=True, default="cavatica/apps-publisher")
parser.add_argument('-n', '--profile', action='store', dest='profile', help='Cavatica profile name', required=True, default="cavatica")
```

### Input tips
1. `-f, --files`: Use the same manifest as for the pub script
1. `-p, --project`: Cavatica project name to copy to, likely `cavatica/apps-publisher`
1. `-n, --profile`: This script runs on the assumption that you have a local `.sevenbridges/credentials` file.

You'll likely end up having to run just the `-f` portion of the `cavatica_app_pub.py` again for the final file locations
