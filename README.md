# android_workflows

Workflows used at [ZWEIDENKER](https://zweidenker.de) to build and deploy Android Apps

## unit_tests
```yaml
  unitTests:
    uses: zweidenker/android_workflows/.github/workflows/unit_tests.yaml@v1
    with:
      gradleModule: 'app'
      gradleTask: 'testAppDebugUnitTest'
      javaVersion: "11"
      submodules: true
      flutterModulePath: flutter_sub_module
    secrets:
      ssh-key: ${{ secrets.SSH_KEY }}
```

Runs the specific Gradle Task. This is usually used to run unit Tests but can be used to do perform other Gradle Tasks

### Inputs

| Name              | Type     | Description                                                                                                             | Default | required |
|-------------------|----------|-------------------------------------------------------------------------------------------------------------------------|---------|----------|
| javaVersion       | `string` | The Java Version that should be installed                                                                               | 8       |          |
| gradleModule      | `string` | The Gradle Module `gradleTask` should be executed in                                                                    |         | x        |
| gradleTask        | `string` | The Gradle Task that should be executed                                                                                 |         | x        |
| submodules        | `bool`   | Enable Git Submodules                                                                                                   | false   |          |
| flutterModulePath | `bool`   | Optional path for a Flutter Submodule. If this is specified the Pipeline will run `flutter build aar` in this directory | false   |          |

### Secrets
| Name    | Description                                                                       | required |
|---------|-----------------------------------------------------------------------------------|----------|
| ssh-key | SSH Key used for checking out the code. Required if using a private git submodule |          |


## build_and_upload
```yaml
  build:
    uses: zweidenker/android_workflows/.github/workflows/build_and_upload.yaml@v1
    with:
      gradleModule: 'app'
      gradleTask: 'bundleAppRelease'
      packageName: com.example.app
      upload: ${{ (github.ref_name == 'main' }}
      javaVersion: "11"
      submodules: true
      flutterModulePath: flutter_sub_module
    secrets:
      keyalias: ${{ secrets.keyalias }}
      keyfile: ${{ secrets.keyfile }}
      storepassword: ${{ secrets.storepassword }}
      keypassword: ${{ secrets.keypassword }}
      uploadkey: ${{ secrets.play_uploadkey }}
      ssh-key: ${{ secrets.SSH_KEY }}
```

### Inputs

| Name              | Type     | Description                                                                                                                                                      | Default | required |
|-------------------|----------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|----------|
| javaVersion       | `string` | The Java Version that should be installed                                                                                                                        | 8       |          |
| gradleModule      | `string` | The Gradle Module `gradleTask` should be executed in                                                                                                             |         | x        |
| gradleTask        | `string` | The Gradle Task that should be executed                                                                                                                          |         | x        |
| packageName       | `string` | The package name. Required for uploading                                                                                                                         |         |          |
| archiveArtifacts  | `bool`   | If the workflow should archive apks and aabs. This normally should only be needed for a first release build to upload manually to Google Play or for certain PRs | false   |          |
| upload            | boolean  | Enables upload to Google Play to an internal test channel                                                                                                        | false   |          |
| submodules        | `bool`   | Enable Git Submodules                                                                                                                                            | false   |          |
| flutterModulePath | `bool`   | Optional path for a Flutter Submodule. If this is specified the Pipeline will run `flutter build aar` in this directory                                          | false   |          |

### Secrets
| Name          | Description                                                                                                                                                   | required |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|
| keyalias      | Key Alias                                                                                                                                                     | x        |
| keyfile       | Keyfile. Encoded as a base64 String                                                                                                                           | x        |
| storepassword | Password for the Keystore                                                                                                                                     | x        |
| keypassword   | Password for the Key                                                                                                                                          | x        |
| uploadKey     | Google Play Service Account json key used to upload. Information on how to optain this can be found [here](https://docs.fastlane.tools/actions/supply/#setup) |          |
| ssh-key       | SSH Key used for checking out the code. Required if using a private git submodule                                                                             |          |