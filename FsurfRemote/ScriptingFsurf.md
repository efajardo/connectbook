
[title]: - "Submitting multiple  scans to Fsurf"
[TOC]


## Overview

This document shows how to submit mulitple scans for processing using a
script.  

**Important note on data privacy**:  In order to protect the privacy of your
participants’ scans, we request that you submit only defaced and fully
deidentified scans for processing by fsurf.  Images can be anonymized and
deidentified before they are uploaded to OSG servers as described in [the
article on anonymizing images](https://support.opensciencegrid.org/support/solutions/articles/12000008493-anonymizing-images).


## Preparing to submit scans

There are a few things that will be needed before you can start:

* Terminal access on a computer with the following:
  * Bash installed
  * Wired internet access (preferable) or very fast wireless access 
* A directory with anonymized and defaced MRI brain scans 
* Knowledge of using a text editor within a terminal (e.g. vi, nano, emacs, etc.)

## Running `fsurf` non-interactively

By default, `fsurf` asks if an image has defaced and anonymized when it is
submitted for processing.  However, if fsurf is given `--defaced` and
`--deidentified` as options, these prompts will not appear.

## Scripting `fsurf` submissions

The basic approach to submitting multiple files that we'll take is to generate a
text file with a input file and subject on each line.  Then we'll use a script
that will read this file and use the information in the file to submit each file
for processing.  

### Generating the text file

The first step we'll need to take is to generate a text file with input files
and subjects.  First, we'll generate a list of files.  Assuming, the scans that
we'd like to process are at `~/scans`, run:

     $ ls ~/scans > scan_list

This will generate a file listing each scan on a separate line.  Now edit the
file using a text editor and add the subject for each scan in.  After you're
done, the file should look something like this:

     subject_1_defaced.mgz subject_1
     MRN_1_defaced.mgz MRN_1
     MRN_3.mgz MRN_3

Each line should have the filename for the scan, a space, and then the name of 
the subject.

### Submission script

Now that we have a file that has information on the scans that should be
submitted, let's write a script that will use that to submit the scans
to `fsurf` for processing.

Open a filed called `submit_multiple.sh` in your text editor and cut and paste
the following in it :

     #/bin/bash
     
     for line in `cat $1`;
     do
       input_file=`echo $line | cut -f 1 -d' '`
       subject=`echo $line | cut -f 2 -d' '`
       ./fsurf submit --input-directory $2 --input-file $input_file --subject $subject --defaced --deidentified
     done

Now make the script executable and run it:

     $ chmod a+x submit_multiple.sh
     $ ./submit_multiple.sh scan_list ~/scans

The script may take a while to complete if you have a slow connection or if you
have a lot of scans to submit.  If you're submitting many scans, it's best that
you run this on a computer that's connected to the internet using a wired
connection.

## Getting Help
For assistance or questions, please email the OSG User Support team  at
[user-support@opensciencegrid.org](mailto:user-support@opensciencegrid.org) or
visit the [help desk and community forums](http://support.opensciencegrid.org).


## Validation Information
A list of linux kernels on OSG  on which the Freesurfer workflows have been
validated can be found
[here](https://support.opensciencegrid.org/support/solutions/articles/12000008494-freesurfer-validation-on-the-osg-)