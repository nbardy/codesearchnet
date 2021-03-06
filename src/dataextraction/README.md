# Data Extraction

This document describes the process for extracting raw data from the original source from various languages.  This might be desireable incase you wish to change the data transformations (which consists of parsing and deduplicating code).  These steps are optional and a pre-processed version of the data with all the transformations applied can be obtained by following the **Initial Setup** section of the [README](/README.md) in the root of this project.

## Extracting Data From Source

To extract the data:

* Download and parse the code using the language-specific extractor (in `src/dataextraction/{python, CSharpDataExtraction, JavaDataExtraction}`).  These scripts will generate intermediate `.jsonl.gz` files which must be deduplicated using the instructions below. Documentation for each language can be found in each folder.

* Run `dataextraction/dedup_split.py` to deduplicate and split the data from the intermediate `.jsonl.gz` into train, validation, test and holdout partitions.  Note that the holdout set is not used automatically by our modeling pipeline (however, you can change this default) and we set that aside for your convenience for evaluation purposes.

## Filtering Conventions
For all languages, we do some filtering of data to ensure that the samples we are
considering are meaningful. Concretely, we use the following rules:

* Methods with no documentation are removed.
* Functions that are less than 3 lines long are removed. This removes interface  
  declarations, short methods, getters/setters and unimplemented methods.
* Functions with less than 3 tokens in their documentation are removed. If easily
  recognizable, functions with auto-generated empty documentation should be
  removed as well.
* Test methods are removed. Heuristically, this is any method whose name includes
  the substring "test" (or "Test").
* We only keep the first segment of documentation. By segment we mean a sequence
  of tokens separated from the next segment by an empty line (_i.e._ matching
  `\n\s*\n`).

## Pre-Processed Data Format

This is described in the [main README](/README.md#pre-processed-data-format).
