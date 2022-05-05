# Helper function for programmatic pages
This resource helps to generate a couple of landing page around a data source. This resource can help with programmatic SEO

This notebook:
 - Loads a csv datasource from **data/**
 - collects templates from **templates/**
 - Writes an overview page linking to the subpages in **output/**
 - Writes several subpages in **output/**
 - Writes the additions to a sitemap in **output/**


```python
# Here we read the data source for our programmatic pages
import pandas as pd
df = pd.read_csv("data/languages.csv")
lang_variation_dict = df.groupby(['Language']).apply(lambda x: x['Variation'].tolist()).to_dict()
```

## Helper functions


```python
# takes a html template file marked up with elements to replace
# returns the content as string
def open_template(path):
    text_file = open(path, "r")
    template = text_file.read()
    text_file.close()
    return template

# takes a string with html contents
# writes it to file
def write_page(path, str):
    f = open(path, "a")
    f.write(str)
    f.close()   
    return

def insert_values_into_html_template(html_string, replacement_dict):
    for replacement in replacement_dict.keys():
        html_string = html_string.replace(replacement, replacement_dict[replacement])
    return html_string
```

## Generating the overview
First we generate the overview page that links to the specific subpages


```python
# We load the template for the page that links to the programmatic pages
overview_template = open_template("templates/multilingual.html")

# We construct a HTML fragment with a series of links in list elements
html_fragment = "".join(["<li><a href='https://example.com/%s.html'>%s</a></li>" % (x.lower(), x) for x in lang_variation_dict.keys()])

# We construct a dict of replacements
# And we generate the overview page by replacing the bits in the template
replacement_dict = {"{list_of_languages}":html_fragment}   
overview_page = insert_values_into_html_template(overview_template, replacement_dict)

#And we write it to file
write_page("output/multilingual.html", overview_page)
```

## Generating the specific subpages


```python
subpage_template = open_template("templates/language.html")

for x in lang_variation_dict.keys():
    
    # We construct the fragments we want to sub in
    language = x
    num_variants = "%d variant" % (len(lang_variation_dict[x]))
    num_variants += "s" if len(lang_variation_dict[x])>1 else ""
    list_of_variants = ""
    for y in lang_variation_dict[x]:
        list_of_variants += "%s, " % (y)
    
    list_of_variants = list_of_variants[:len(list_of_variants)-2]                    
    
    # We construct a dict of replacements
    # And we generate the overview page by replacing the bits in the template
    replacement_dict = {
        "{list_of_variants}":list_of_variants,
        "{language}":language,
        "{num_variants}":num_variants
    }   
    subpage = insert_values_into_html_template(subpage_template, replacement_dict)
    write_page("output/%s.html"%(x.lower()), subpage)
```

## Finally update the sitemap


```python
sitemap_template = open_template("templates/sitemap.xml")

# We construct the fragment we want to sub in
pages_fragment = ""
for x in lang_variation_dict.keys():
    pages_fragment += """<url>
        <loc>https://example.com/{0}.html</loc>
        <lastmod>2022-05-05</lastmod>
        <xhtml:link rel='alternate' hreflang='en' href='https://example.com/{0}.html'/>
    </url>""".format(x.lower())

# We construct a dict of replacements
# And we generate the overview page by replacing the bits in the template
replacement_dict = {"{pages}":pages_fragment}   
sitemap = insert_values_into_html_template(sitemap_template, replacement_dict)
write_page("output/sitemap.xml", sitemap)
```


```python

```


```python

```
