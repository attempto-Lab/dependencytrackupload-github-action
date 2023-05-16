# deptrackupload-github-action

GitHub Action to upload generated BOMs to [OWASP Dependency-Track](https://owasp.org/www-project-dependency-track/).

**Please note:** node_modules has to be checked in, because the action is using the node_modules of the action itself.

## Inputs

### `serverUrl`

**Required** full qualified url of your dependency track server.

### `apiKey`

**Required** your dependency track api-key.

### `bomFile`

**Required** file including path to bomFile, e.g. /build/reports/bom.xml.

### `projectUUID`

**Optional** project uuid

### `projectName`

**Optional** name of the project

### `projectVersion`

**Optional** project version.

### `autoCreate`

**Optional** should the project be automatically created if it does not exist, default is `false`.


**Please note:** either `projectUUID` OR (`projectName` AND `projectVersion`) are **Required**.


## Outputs

## `statusCode`

The status code of the upload.


## Example usage in a github-workflow:

You should have generated your [CycloneDX BOM](https://cyclonedx.org/)
with [the tool of your choice](https://cyclonedx.org/tool-center/)
to `build/reports/bom.xml`

To upload the bom, you can use one of the following variants:

You specifiy `projectName` and `projectVersion` and `autoCreate: true`
and let Dependency-Track create a project for you automatically:

```yaml
    - name: Upload bom to dependency-track instance
      uses: attempto-Lab/dependencytrackupload-github-action@v1
      id: deptrack
      with:
        serverUrl: 'https://deptrack.yourhost.org/api/v1/bom'
        apiKey: ${{ secrets.DEPENDENCYTRACK_APIKEY }}
        bomFile: 'build/reports/bom.xml'
        projectName: 'your-fancy-project'
        projectVersion: '1.0.0'
        autoCreate: true
    - name: StatusCode
      run: echo "Upload returned ${{ steps.deptrack.outputs.statusCode }}"
```

You create the project in Dependency-Track beforehand and specifiy the UUID
using the key `projectUUID`:

```yaml
    - name: Upload bom to dependency-track instance
      uses: attempto-Lab/dependencytrackupload-github-action@v1
      id: deptrack
      with:
        serverUrl: 'https://deptrack.yourhost.org/api/v1/bom'
        apiKey: ${{ secrets.DEPENDENCYTRACK_APIKEY }}
        bomFile: 'build/reports/bom.xml'
        projectUUID: '...PUT YOUR PROJECT-UUID HERE...'
    - name: StatusCode
      run: echo "Upload returned ${{ steps.deptrack.outputs.statusCode }}"
```
