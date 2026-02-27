Ein YAML-skrá fyri hvønn hátt.

Dømi:

```yaml

```

# Av github Spraakbanken back-end 2025-11-19:

## Mode Configuration

At least one mode file is required, and that file must be named `default.yaml`. This is the mode that will be loaded when no mode is explicitly requested.

**Required**:

- **label**: The name of the mode, which will be shown in the interface.

**Optional**:

- **description**: A description of the mode, shown when first entering it. May include HTML.
- **order**: A number used for sorting the modes in the interface. Modes without an order will end up last.
- **folders**: A folder structure for the corpus selector. These folders can then be referenced by individual corpora. The folder structure can be of any depth, and folders can have any number of sub-folders (using the key `subfolders`). You may use HTML in the descriptions. Example:

    ```yaml
    folders:
    novels:
        title:
        eng: Novels
        swe: Skönlitteratur
        description:
        eng: Corpora consisting of novels.
        swe: Korpusar bestående av skönlitteratur.
        subfolders:
        classics:
            title:
            eng: Classics
            swe: Klassiker
        scifi:
            title: Science-Fiction
    ```

- **preselected_corpora**: A list of corpus IDs which will be pre-selected when the user enters the mode. You may also refer to folders by using the prefix `__`, and dot-notation for refering to subfolders. Example:

    ```yaml
    preselected_corpora:
    - my-corpus
    - __novels.scifi
    ```

- Other than the above, you can also override almost all the global settings set in the frontend's `config.yaml`. See [the documentation for the frontend](https://github.com/spraakbanken/korp-frontend/blob/master/doc/frontend_devel.md#settings-in-configyml) for a list of available settings.