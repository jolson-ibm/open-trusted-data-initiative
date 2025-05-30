# Hub Python Library

The [`huggingface_hub`](https://github.com/huggingface/huggingface_hub) library is for interacting with the Hugging Face Hub, which is a collection of git-based repositories (models, datasets, or spaces). There are two main ways to access the Hub using `huggingface_hub`:
* Git-based approach, implemented by the `Repository` class - a wrapper around 
the git command with additional functions specifically designed to interact with the Hub. 
* HTTP-based approach, involves making HTTP requests using the `HfApi` client (compare to [PyGithub](https://github.com/PyGithub/PyGithub)).

The main advantage of using a `Repository` instance is that it allows you to maintain a local copy of the entire repository on your machine. This can also be a disadvantage as it requires you to constantly update and maintain this local copy.

The `HfApi` class provides an alternative to local git repositories, which can be cumbersome to maintain, especially when dealing with large models or datasets. The `HfApi` class offers the same functionality as git-based approaches, such as downloading and pushing files and creating branches and tags, but without the need for a local repo clone that must be kept in sync. In addition to the functionalities already provided by git, the `HfApi` class offers additional features, such as the ability to manage repos, download files using caching for efficient reuse, search the Hub for repos and metadata, access community features such as discussions, PRs, and comments, and configure spaces hardware 
and secrets.

In our evaluation below, we looked only at `HfApi` and only at its usage for working with datasets.

## Installation

The library can be installed using the following command:

```shell
pip install huggingface_hub
```

## Create and Manage Datasets

The library provides a wealth of APIs using [HfApi Client](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#hfapi-client)
to create, delete and manage repository. This includes support for the following:

* creating a [repository](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.create_repo) (dataset)
* creating a [pull request](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.create_pull_request)
* merging a [pull request](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.merge_pull_request)
* creating a [branch](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.create_branch)
* creating a [discussion](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.create_discussion)
* creating a [tag](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.create_tag)
* creating a [webhook](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_api#huggingface_hub.HfApi.create_webhook)

And many others. Refer to `HfApi` documentation for more details.

## Search Datasets

The library provides simple and straightforward APIs for exploring assets in the Hugging Face hub. 

See this simple code example, [`ds_explorer.py`](ds_explorer.py). See this 
[reference](https://huggingface.co/docs/huggingface_hub/package_reference/hf_api#huggingface_hub.HfApi.list_datasets) for more options on filtering, ordering and paging of the search parameters.

## Working with the Dataset Cards

A simple python example for reading a dataset card is [`dataset_card.py`](dataset_card.py). The class used, `DatasetCard`, can also be used for creating of a new card. 

Considering the importance of dataset cards for managing datasets, HF hub provides an ability to set up [automatic metadata quality review for datasets](https://huggingface.co/docs/hub/webhooks-guide-metadata-review). This is done via implementing and setting up a [Webhook](https://docs.github.com/en/webhooks/about-webhooks) that will be invoked on every repo update. These webhooks can be deployed either in an individual user account or using Hugging Face [spaces](https://huggingface.co/docs/hub/spaces-overview). In general, webhooks can be extremely useful for implementing any specific logic for all new dataset commits.

# Accessing Dataset Data

The library provides APIs to [download](https://huggingface.co/docs/huggingface_hub/main/en/guides/download) and [upload](https://huggingface.co/docs/huggingface_hub/main/en/guides/upload) data files. While the APIs provide support
for downloading and uploading individual files and directories, their capabilities are sort of limited.

A more flexible approach is provided by the `Filesystem` API. A simple example showing its capabilities is [`file_system.py`](file_system.py). For all methods on this class refer to this [documentation](https://huggingface.co/docs/huggingface_hub/main/en/package_reference/hf_file_system#huggingface_hub.HfFileSystem).

# Parquet Support

Input data files can be in every format, for example, JSON, CSV, etc. However, when uploading to the dataset, these files are automatically converted to Parquet, using the `parquet-converter` bot, and the Parquet files are stored in the `refs/convert/parquet` branch. As explained [here](https://huggingface.co/datasets/haibaraconan/video/discussions/1), when the dataset is already in Parquet format, the files are not converted and the files in `refs/convert/parquet` are links to the original files. This rule has an exception to ensure the dataset viewer API performance remains fast; if the row group size of the original Parquet files is too big, then new Parquet files are generated.
