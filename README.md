# :package: Github Action `full release data`

> A Github action to retrieve all release data from github releases. 

## :question: Why another github-action to retrieve release data?

During the development of build jobs, I had to use different Github actions to download release asset files, read the last tag name or retrieve the release notes.
The goal is to provide a simplified, well-documented Github action that provides all the information of a github release in just a single github action.

## About

Use latest release tag, body content or, download release assets of a github release. The data can be retrieved from `public` and `private` repositories. 

## Usage

>:white_flag: See the [inputs](#inputs) section for detailed descriptions.

```yml
  - name: Get Release data of private repository
    id: release_data
    uses: KevinRohn/github-full-release-data@v2.0.1
    with:
      repository: "organisation-or-username/repository" # (Optional) Repository name to fetch release data.
      token: ${{ secrets.GITHUB_TOKEN }} # (Optional) `token` of the private repository from which the release information should be retrieved.
      version: latest  # (Optional) The version from which the release information is to be retrieved.
      body-markdown-file-path: output/release-body-content.md # (Optional) Specify an output path where the body output should be saved.
      asset-file: '*.dat,picture.jpg' # (Optional) Name of the asset files to download.
      asset-output: 'download-output/' # (Optional) The output path in which the downloaded asset files should be placed.
``` 


## Usage Examples

>:triangular_flag_on_post: See the [`example-github-full-release-data` repository](https://github.com/KevinRohn/example-github-full-release-data) for more examples.

### Get the latest `tag_name` and use it in further steps

```yml
    # Fetch release data of the current repository from where the workflow is used.
  - name: Get Release data
    id: release_data
    uses: KevinRohn/github-full-release-data@v2.0.1

  - name: Show tag name with echo
    run: echo ${{ steps.release_data.outputs.tag_name }}
```

### Get `body` content from github release of a private repository and use it in further steps

```yml
  - name: Get Release data of private repository
    id: release_data
    uses: KevinRohn/github-full-release-data@v2.0.1
    with:
      repository: "organisation-or-username/repository"
      token: ${{ secrets.GITHUB_TOKEN }}
      version: latest
  
  - name: Write release `body` content line by line to file
    run: |
      {
        echo '${{ fromJSON(steps.release_data.outputs.body) }}'
      } >> test-file.txt

  - name: Show release `body` content
    run: |
      echo '${{ fromJSON(steps.release_data.outputs.body) }}'
```

### Download release `asset-file` from github release

```yml
  - name: Download release asset file
    id: release_data
    uses: KevinRohn/github-full-release-data@v2.0.1
    with: 
      asset-file: '*.dat'
      asset-output: './'

  - name: Show file output
    run: |
      ls -lah
```


## Inputs
The action supports the following inputs:

- `repository`
  
  Repository name to fetch release data.
  
  >If no repository is passed as an input. The repository from the build workflow is used. 
  It's possible to fetch release data from any given repository. Just use `<org>/<repo>` as an input parameter.
  It's also possible to fetch release data from private repositories. In this case, just pass the `token` input parameter.
  
  **Required:**
  *false*

- `token`
  
  `token` of the private repository from which the release information should be retrieved.

  >Pass the input parameter `token` to fetch release information from private repositories.      
  e.g.: `secrets.GITHUB_TOKEN`
  
  **Required:**
  *false*

- `version`
  
  The version from which the release information is to be retrieved.
 
  >If the input value is not set, the default value `latest` is used.
  It's possible to fetch release data from a given tag. To fetch a specific tag just use `<tag-name>`.
  
  **Required:**
  *false*

- `body-markdown-file-path`
  
  Specify an output path where the body output should be saved.
  
  >If the path is valid and the file is accessible then, the body output content will be saved into the file.
  Save the file in the current working directory: `./release-content.md`.
  
  **Required:**
  *false* 

- `asset-file`
  
  Name of the asset files to download.
  
  >To download release asset files, just use the input parameter `asset-file`.
  To download multiple release asset files use `,` comma as a separator.
  <br/><br/>Example: <br/>
    Download all .PNG and .JPEG files: `*.PNG,*.JPEG` <br/>
    Download release asset with a static name: `myfile-to-download.extension`
  
  **Required:**
  *false* 

- `asset-output`
  
  The output path in which the downloaded asset files should be placed.

  >The input value is required if the input value `asset-file` was specified.
  
  **Required:**
  *false* 

## Outputs

| Output              | Description                                                                                                                                                                                                 |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `url`               | Release URL to use in API calls                                                                                                                                                                             |
| `html_url`          | Release URL                                                                                                                                                                                                 |
| `assets_url`        | Asset URL to use in API calls                                                                                                                                                                               |
| `upload_url`        | Upload URL for release assets                                                                                                                                                                               |
| `tarball_url`       | Url to tarball content                                                                                                                                                                                      |
| `zipball_url`       | Url to zipball content                                                                                                                                                                                      |
| `discussion_url`    | Discussion URL - can be `null` if discussion is deactivated                                                                                                                                                 |
| `id`                | Release id                                                                                                                                                                                                  |
| `node_id`           | Release node id                                                                                                                                                                                             |
| `tag_name`          | Release tag name                                                                                                                                                                                            |
| `target_commitish`  | Target from which the release is created                                                                                                                                                                    |
| `body`              | The `body` output from release. It's a multiline output, which is packed in a JSON object. <br/> The output can be used with the `fromJSON` function `(steps.<id>.outputs.body)`.                           |
| `draft`             | Shows if the release is a draft release or not (true/false)                                                                                                                                                 |
| `prerelease`        | Shows if the release is a pre-release (true/false)                                                                                                                                                          |
| `created_at`        | Shows the release creation date                                                                                                                                                                             |
| `published_at`      | Shows the published date of the release                                                                                                                                                                     |
| `assets_array_json` | The `assets_array_json` output from release. It's a multiline output, which is packed in a JSON object. <br/> The output can be used with the `fromJSON` function `(steps.<id>.outputs.assets_array_json)`. |
| `author_json`       | The `author_json` output from release. It's a multiline output, which is packed in a JSON object. <br/> The output can be used with the `fromJSON` function `(steps.<id>.outputs.assets_array_json)`.       |

## Support
This action only supports Linux runners. If you encounter Error: action is only supported on Linux then you are using non-linux runner.
