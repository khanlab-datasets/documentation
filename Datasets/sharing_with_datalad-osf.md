# Datalad-osf (Last Updated: March 11, 2020)

To more easily convert a dataset from OSF to Datalad, `datalad-osf`, the utility scripts created by [TemplateFlow](https://github.com/templateflow) have been modified and made available at the following Github repository (link to be added).

A command-line version of the tool has been created and can be called using the command `datalad-osf` following installation. This tool may be included in existing Khanlab containers in the future. Setup of this utility requires Python 3 and a number of Python dependencies (see `requirements.txt` contained in the repository).

## Usage
```
datalad-osf <project key> [-s <subset>]
```


`<project key>` - The set of characters following the project's URL. For example, the project key for `BigBrainHippoUnfold` is `x542s` (https://osf.io/x542s)

`<subset>` - An optional argument; converts only a subset of data hosted within osf. This can be a path to any valid directory within the OSF project relative to the top level of the project

# Dataset Conversion
_Note: Conversion of the dataset will store a (temporary) copy of the data locally on the machine being used. Please ensure there is sufficient space._


1. Create the Datalad dataset locally and navigate to the directory.

    ```
    datalad create -d <local directory>
    ```

2. Download the OSF (subset of) data to the datalad dataset.

    ```
    datalad-osf <key>
    ```

3. Using `datalad`, create the repository on Github and host on the khanlab-datasets Github organization, use the following command:

    ```
    datalad create-sibling-github --github-organization khanlab-datasets -d <local directory> <project_name>
    ```

4. Save and publish the changes.

    ```
    datalad save
    datalad publish --to github
    ```

## Host on CONP
5. Add `descriptor.json` file to the repository. Contents of the descriptor can be found below:

    ```
    {
    "title": "A descriptive title",
    "description": "A description of the dataset - can include available modalities, goal of project, or link to paper"
    "authors": "Authors of Dataset or Owning Organization",
    "licenses": "License for distribution (e.g. CC-By Attribution 4.0 International)",
    "contact": "contact@email.com"
    }
    ```

    Add the descriptor to the datalad dataset and publish to the repository.

    ```
    datalad add --to-git ./descriptor.json
    ```

6. Create and add a README.md file directly in the repository if one does not already exist.
    ```
    datalad add --to-git ./README.md
    ```

7. Save and publish modifications
    ```
    datalad save
    datalad publish --to github
    ```


**_The remaining steps should be done by the maintainer(s) of the forked `conp-dataset` repository._**


8. Install the [conp-dataset repository](https://github.com/khanlab-datasets/conp-dataset) if there is no previous installation of `conp-dataset`. Add a remote link to the original `conp-dataset` hosted by the CONP in order to keep the the `khanlab-dataset` version up-to-date.
    ```
    git clone https://github.com/khanlab-datasets/conp-dataset
    cd conp-dataset
    git remote add conp https://github.com/conp-pcno/conp-dataset
    ```

9. Prior to making any changes ensure that both 1) your local copy of the repository and 2) the khanlab-datasets version of the `conp-dataset` repository are up to date.

    ```
    git pull origin
    git pull conp
    ```

10. Navigate to the installed conp-dataset. Check that you are currently working on the master branch `git checkout master`. Add dataset to be shared as a submodule to the conp-dataset fork.
```
git submodule add https://github.com/khanlab-datasets/<project_name>.git projects/Khanlab/<project_name>
```

11. Save and publish the modifications to fork.
```
git commit -m "Added projects/Khanlab/<project_name>"
git push
```

12. Create pull request to merge dataset into main conp-dataset repository with additional dataset.
