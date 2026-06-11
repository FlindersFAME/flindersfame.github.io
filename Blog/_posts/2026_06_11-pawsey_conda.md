---
layout: single
show_date: true
tags : pawsey conda mamba
title: Using Conda and Mamba on Pawsey
author: Rob Edwards
---


# Using Conda (or Mamba) on Pawsey

> [!NOTE]
> This is part of our series on Pawsey that are written by users - not by Pawsey staff. There are certainly other, and probably better, ways to do this, but this is what we are currently doing!
> You should also read the [Pawsey Help Documentation](https://pawsey.atlassian.net/wiki/spaces/US/overview?homepageId=51904514)

## Pawsey storage locations (disks)

There are three main storage locations that you can access:

1. `/home` (where your log into) has a limit of 10,000 files and 1Gb of storage, so you will quickly fill that up.
2. `/software` has a limit of 16,384G abd 250k files, so you can put _more_ things there, but not everything
3. `/scratch` has 9.8P of storage, **but** everything is deleted after 21 days, so this is not a brilliant location either.
4. `acacia` is for longer term storage but you can't access that directly, so you can't install software there.


## A basic conda set up.

I use `/software` for some _basic_ conda environments that I am going to use regularly. For example, I have an `rclone` environment  that only has `rclone` and I use to move data on and off of setonix or acacia. My other environments are a `bioinformatics` environment which has a few common tools I use day-to-day like `samtools` and `minimap` and a `git-lfs` environment I also use regularly, that only has `git-lfs` installed. (If you don't know what `git-lfs` is for, you probably don't need it!)

Everything else, I put in a temporary directory in `/scratch` and then I recreate them as I need it.

There are two different solutions to this problem, and I use both depending on how I feel.

## Disposable `/scratch` conda environments.

I make a temporary environment on `/scratch` with a directory name that is a meaningless random set of characters. I install what I need, use it as I need it, and then later, when I remember, I delete the environment.

The advantage of this approach, is you leave it if something is broken and start again, and you make a new directory for each thing you are doing.


## Rememberable, but disposable, `/scratch` conda environments.

The alternative is to use a name that you will remember, but then you also need to remember that things are probably broken after 21 days and you need to reinstall everything.

Let's walk through setting up your conda, installing some software, remembering how to do it, and deleting the environment.

For this example, I'm going to use [autocycler](https://github.com/rrwick/Autocycler) as my software to install, and I'll also install [minimap2](https://github.com/lh3/minimap2) and [samtools](https://github.com/samtools/samtools).

## Install conda/mamba

Start with installing conda/mamba from [miniforge](https://github.com/conda-forge/miniforge).

Go to the instructions for [installing miniforge on a Unix-like platform](https://github.com/conda-forge/miniforge#unix-like-platforms-macos-linux--wsl)  and use either `wget` or `curl` to download the installer. It doesn't matter which one, so start with `curl` (because that is first on the list), and if that doesn't work use `wget`.

## Set up your `.condarc` file. 

Use `nano ~/.condarc` and copy the block below and paste into the file.

```
channels:
   - conda-forge
   - bioconda
envs_dirs:
   - /software/projects/$PAWSEY_PROJECT/$USER/miniconda3/envs_dirs
pkgs_dirs:
   - /scratch/$PAWSEY_PROJECT/$USER/software/miniconda3/pkg_dirs
env_prompt: "({name}) "
channel_priority: strict
```

This block adds `conda-forge` and `bioconda` so you can easily install software, sets the default environment directory to `/software` and the location where the files are downloaded to `/scratch`.

## Create a environment file to install the software

If you use environment files, you can install the software directly from the file, and then if you need to reinstall things (e.g. because the file has been deleted, you just need one command!).

Use `nano` to create a file called `environment.yml` and paste this information:


```
name: autocycler
channels:
   - conda-forge
   - bioconda
dependencies:
   - samtools>=1.20
   - minimap2>=2.29
   - autocycler
```

> _Notes:_ 
> 1. We specify `conda-forge` and `bioconda` here as well, which is not really necessary as we have them in our `~/.condarc` but it allows other people to use this environment too.
> 2. You can specify _exact_ versions of software (e.g. `samtools==1.20`) or minimum versions (e.g. `samtools>=1.20). Exact versions help with reproducibility, but mean you don't get the newest additions!

If you install this with:

```
mamba env create -f environment.yml
```

it will download the packages and install them into a `mamba` environment called `autocycler` located on `/software`.

Once the install is complete, you can list the environments with:

```
mamba info --envs
```

This is now consuming a part of your quota on `/software` and if you install too many packages here, it will get full!

## Create a disposable environment

Now that we have an environment file, we don't need to install it on `/software` every time.

Here, we make a random 12 character long string, and make the environment with that name.

```
TMP=$(for i in {1..12}; do printf "%x" $((RANDOM % 16)); done)
mamba env create --yes --prefix /scratch/$PAWSEY_PROJECT/$USER/software/miniconda3/$TMP --file environment.yml
mamba activate /scratch/$PAWSEY_PROJECT/$USER//software/miniconda3/$TMP
```

Note that when the installation is complete it tells you how to activate the environment. When I did this, mine was called `e95467637aed`.

You can also see that environment listed using `mamba info --envs`


## Create a memorable, disposable, environment

You can do the same thing, but give the environment a name you remmeber. For example:

```
mamba env create --yes --prefix /scratch/$PAWSEY_PROJECT/$USER/software/miniconda3/autocycler --file environment.yml
mamba activate /scratch/$PAWSEY_PROJECT/$USER//software/miniconda3/autocycler
```

> [NOTE!]
> The environment created without using a `--prefix` command is called `autocycler` and is on `/software`. The environment created _with_ the `--prefix` command is on `/scratch` and is a _different environment_. Since this is exceptionally confusing, do one or the other, but NOT both!

## Deleting environments

Pawsey will automatically delete any files that are older than 21 days, so you don't need to worry about old environments, however it gets very confusing, so you should delete them.

Start by doing `mamba info --envs` to get a list of your environments, and then choose the _path_ of the one you want to remove.

Delete the environment, and any files left in it, using 

```
mamba env remove --prefix  /software/projects/$PAWSEY_PROJECT/$USER/miniconda3/envs_dirs/autocycler
```


# Clean up your downloaded packages

Sometimes when you are installing software you will get random errors about packages being incomplete or not able to be installed. Usually, the problem is that the packages on `/scratch` have been deleted, so clean them out and try again, which will force them to be re-downloaded.

```
mamba clean -af
```


