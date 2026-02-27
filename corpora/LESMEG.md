Ein YAML-skrá fyri hvørja málheild.

Dømi:

```yaml

```

# Av github Spraakbanken back-end 2025-11-19:

## Corpus Configuration

Corpus configuration files are placed in the `corpora` folder, and the **filename of each configuration file should correspond to a corpus ID in lowercase, followed by `.yaml`, e.g. `mycorpus.yaml`**.

**Required**:

- `id: `: The corpus' system name, same as the configuration file name (minus `.yaml`).
- `title: `: Title of the corpus.
- `description: `: Description of the corpus. HTML can be used.
- `modes: `: A list of the modes in which the corpus will be included, optionally specifying a folder. Example:

    ```yaml
    mode:
    - name: default
        folder: novels.classics
    ```

**Optional**:

- `within: `: Use this to override `default_within` (set in the global or mode config). `within` is a list of **structural elements** to use as boundaries **when searching**, ordered from smaller to bigger. Example:

    ```yaml
    within:
    - label:
        eng: sentence
        swe: mening
        value: sentence
    - label:
        eng: paragraph
        swe: stycke
        value: paragraph
    ```

- `context: `: Use this to override `default_context` (set in the global or mode config). `context` is a list of **structural elements** that can be used as **context** in the **displaying of the search results**, ordered from smaller to bigger. Example:

    ```yaml
    context:
    - label:
        eng: 1 sentence
        swe: 1 mening
        value: 1 sentence
    - label:
        eng: 1 paragraph
        swe: 1 stycke
        value: 1 paragraph
    ```

- `attribute_filters: `: A list of **structural attributes** on which the user will be able to **filter** the **search results**, using **menus** in both simple and extended search.

- `pos_attributes: ` and `struct_attributes: `: Lists of **positional** and **structural** attributes. **Every item** in each list should be an **object with one key**. The **key** should be the **ID** of the **attribute**, e.g. **`msd`** for **positional** attributes or `text_title` for **structural**. 
    
    The value should be either 
    
    1) an object with a complete attribute definition, or 
    2) a string referring to an attribute preset containing such a definition, e.g. `msd` to refer to `attributes/positional/msd.yaml`. 

    With option 1, you may also refer to a `preset` by using the key preset and then extend/override that preset.
    
    The **attribute definition** is what **tells the Korp frontend how to handle each attribute**, like how it should be presented in the sidebar and what interface widget to use in extended search.
    
    For more information about what options are available for attribute definitions, see the [Korp frontend documentation](https://github.com/spraakbanken/korp-frontend/blob/master/doc/frontend_devel.md#attribute-settings).
    
    Example:

    ```yaml
    struct_attributes:
    - text_title: title
    - text_type:
        label:
            eng: type
            swe: typ
    - text_source:
        preset: url
        label:
            eng: source
            swe: källa
    ```

- **custom_attributes**: See [Custom attributes](https://github.com/spraakbanken/korp-frontend/blob/master/doc/frontend_devel.md#custom-attributes).
- **reading_mode**: See [Reading mode](https://github.com/spraakbanken/korp-frontend/blob/master/doc/frontend_devel.md#reading-mode).
- **limited_access**: Set to `true` to indicate that this corpus requires the user to be logged in and having the right permissions.

## Dømi frá korp-backend-config-main frá Starkaði

```yaml
id: igc2410ext_books

context:
  - label:
      eng : 1 sentence
      isl : 1 málsgrein
    value : 1 sentence

title: "Útgefnar bækur"
description: "Lýsing"

mode:
  # - name: rmh2022
  - name: default

within:
  - label:
      eng : sentence
      isl : málsgrein
    value : sentence


pos_attributes:
  - word: word
  - lemma: lemma
  - pos: pos
  - tt: tt
  - hattur: hattur
  - mynd: mynd
  - pers: pers
  - tof: tof
  - fnf: fnf
  - kyn: kyn
  - tala: tala
  - fall: fall
  - greinir: greinir
  - sernafn: sernafn
  - tid: tid
  - lob: lob
  - lostig: lostig
  - fsfall: fsfall
  - tob: tob


struct_attributes:
  - date_interval:
      label:
        eng: Time interval
        isl: Tímabil
      hide_sidebar: 'true'
      hide_compare: 'true'
      hide_statistics: 'true'
      opts: false
      extended_component: dateInterval
  - text_title: text_title_hidden
  - text_date: text_date
  - text_release_year: text_release_year
  - text_author: text_author_hidden
  - text_author_book: text_author_book
  - text_author_part: text_author_part
  - text_publisher: text_publisher
  - text_editor: text_editor
  - text_translator: text_translator
  - text_datefrom: text_datefrom
  - text_dateto: text_dateto
  - text_timefrom: text_timefrom
  - text_timeto: text_timeto
  - text_wordcount: text_wordcount
  - text_sentencecount: text_sentencecount
  - text_paragraphcount: text_paragraphcount
  - text_part_title: text_part_title
  - text_url_publisher: text_url_publisher_hidden
  - text_url_leitir: text_url_leitir_hidden
  - sentence_n: sentence_n


custom_attributes:
  - text_url:
      custom_type: struct
      label: ''
      pattern: "<p><span rel='localize[text_title]'></span>: <a target='_blank' href='<%=struct_attrs.text_url_publisher%>'><%=struct_attrs.text_title%></a><a target='_blank' href='<%=struct_attrs.text_url_leitir%>'><img src='app/img/leitir.png' width=16px></a></a></p></p>"
```

# Av github Spraakbanken front-end 2025-11-19:

## Attribute settings

Corpora and their attrbutes are configured in the backend, but most of the available settings are frontend related. These are the available configuration parameters for attributes.

### Possible settings for `pos_attributes` and `struct_attributes`

- `label: `: Label to **display** wherever the attribute is shown.
- `display_type: `: Set to `hidden` to fetch attribute, but never show it in the frontend. See `hide_sidebar`, `hide_statistics`, `hide_extended` and `hide_compare` for more control.
- `extended_component: `: For available components, see [extended components](https://github.com/spraakbanken/korp-frontend/blob/master/doc/frontend_devel.md#extended-components). For writing custom components, see customizing extended search.
- `external_search: `: Link with placeholder for replacing value. Example: 
    `https://spraakbanken.gu.se/karp/#?search=extended%7C%7Cand%7Csense%7Cequals%7C<%= val %>`
- `group_by: `: Set to either `group_by` or `group_by_struct`. Should only be needed for attributes with `is_struct_attr: true`.
  Those attributes are by default sent as `group_by_struct` in the statistics, but can be overridden here.
- `hide_sidebar: `: `boolean`. Default `false`. Hide attribute in sidebar.
- `hide_statistics: `: `boolean`. Default: `false`. Should it be possible to compile statistics based on this attribute?
- `hide_extended: `: `boolean`. Default: `false`. Should it be possible to search using this attribute in extended?
- `hide_compare: `: `boolean`. Default: `false`. Should it be possible to compare searches using this attribute?
- `internal_search: `: `boolean`. Should the value be displayed as a link to a new Korp search? Only works for sets.
  Searches for CQP-expression: `[<attrName> contains "<regescape(attrValue)>"]`
- `is_struct_attr: `: `boolean`. If `true` the attribute will be treated as a structural attribute in every sense except
  it will be included in the `show` query parameter instead of `show_struct` for KWIC requests. Useful for structural
  attributes that extend to smaller portions of the text than the selected context, such as name tagging.
- `opts: `: this represents the auxiliary select box where you can modify the input value.
  See [Operators](#operators) section for format and more information.
- `order: `: Order of attribute in the sidebar. Attributes with a lower `order`-value will be placed above attributes
  with a higher `order`-value.
- `pattern: `: HTML snippet with placeholders for replacing values. Available is `key` (attribute name) and `value`.
  Also works for sets. Example: `'<p style="margin-left: 5px;"><%=val.toLowerCase()%></p>'`
- `sidebar_component: `: See [Customizing sidebar](#customizing-sidebar).
- `sidebar_info_url: `: `string` (URL). If defined and non-empty, add an info symbol ⓘ for the attribute in the
  sidebar, linking to the given URL. This can be used to link to an explanation page for morphosyntactic tags, for example.
- `sidebar_hide_label: `: `boolean`. If `true`, do not show the localized attribute label and the colon following it in the
  sidebar, only the attribute value. This can be used, for example, if the `pattern` for the attribute includes the label but
  the label should be shown in the attribute lists of the extended search or statistics.
- `stats_cqp: `: See [Rendering attribute values in the statistics view](#rendering-attribute-values-in-the-statistics-view).
- `stats_stringify: `: See [Rendering attribute values in the statistics view](#rendering-attribute-values-in-the-statistics-view).
- `translation: `: An object containing translations of possible values of the attribute, in this format:

  ```yaml
  ROOT:
    eng: Root
    swe: Rot
  ++:
    eng: Coordinating conjunction
    swe: Samordnande konjunktion
  +A:
    eng: Conjunctional adverbial
    swe: Konjuktionellt adverb
  ```

  This replaces value-translation in the translation-files, and also the old attribute `translationKey`.
- `type: `: Possible values:
  - `"set"` - The attribute is formatted as "|value1|value2|". Include contains and not contains in `opts`.
    In the sidebar, the value will be split before formatted. When using compile / `groupby` on a "set"
    attribute in a statistics request, it will be added to `split`.
  - `"url"` - The value will be rendered as a link to the URL and possibly truncated if too long.

### Custom attributes

Custom attributes are attributes that do not correspond to an attribute / annotation in the backend. They are mainly used to present information in the sidebar that combines values from other attributes.

- **custom_attributes**: creates fields in the sidebar that have no corresponding attribute in the backend. Useful for combining two different attributes. All settings concerning sidebar format for normal attributes apply in addition to:
  - **custom_type**: `"struct"` / `"pos"` - decides if the attribute should be grouped under word attributes or text attributes.
  - **pattern**: Same as pattern for normal attributes, but `struct_attrs` and `pos_attrs` also available. Example: `'<p style="margin-left: 5px;"><%=struct_attrs.text_title - struct_attrs.text_description%></p>'`
