# :package: Github Action `Github full release data`

> An Github action to retrieve all release data from github releases. 

## :grey_question: Why another github-action to retrieve release data?

During the development of build jobs I had to use different Github actions to download release asset files, read the last tag name or retrieve the release notes.
The goal is to provide a simplified, well-documented Github action that provides all the information of a github release in just a single github action.

## About

Use latest release tag, body content or download release assets of an github release. The data can be retrieved from `public` and `private` repositories. 

## Usage


## Inputs
The action supports the following inputs:

- `repository`
  Repository name to fetch release data.
  
  > **Description:**
  If no repository is passed as an input. The repository from the build workflow is used. 
  It's possible to fetch release data from any given repository. Just use `<org>/<repo>` as an input parameter.
  It's also possible to fetch release data from private repositories. In this case just pass the `token` input parameter.
  **Required:**
  *false*

- `token`
  `token` of the private repository from which the release information should be retrieved.
  
  > **Description:** 
  Pass the input parameter `token` to fetch release information from private repositories.      
  e.g.: `secrets.GITHUB_TOKEN`
  **Required:**
  *false*

- `version`
  The version from which the release information is to be retrieved.
  > **Description:**
  If the input value is not set, the default value `latest` is used.
  It's possible to fetch release data from a given tag. To fetch a specific tag just use `<tag-name>`.
  **Required:**
  *false*

- `body-markdown-file-path`
  Specify an output path where the body output should be saved.
  
  > **Description:**
  If the path is valid and the file is accessible then, the body output content will be saved into the file.
  Save the file in the current working directory: `./release-content.md`.
  **Required:**
  *false* 

- `asset-file`
  Name of the asset files to download.
  
  > **Description:**
  To download release asset files, just use the input parameter `asset-file`.
  To download multiple release asset files use `,` comma as a separator.
  Example: 
    Download all .PNG and .JPEG files: `*.PNG,*.JPEG`
    Download release asset with static name: `myfile-to-download.extension`
  **Required:**
  *false* 

- `asset-output`
  The output path in which the downloaded asset files should be placed.
  
  > **Description:**
  The input value is required if the input value `asset-file` was specified.
  **Required:**
  *false* 

## Outputs