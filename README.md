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

def generate_html_link_list(list_of_elements, link_base="https://example.com/"):
    return "".join(["<li><a href='%s%s.html'>%s</a></li>" % (link_base, x.lower(), x) for x in list_of_elements])



```


```python
print(generate_html_link_list(lang_variation_dict.keys(), "https://stenos.io/"))
```

    <li><a href='https://stenos.io/afrikaans.html'>Afrikaans</a></li><li><a href='https://stenos.io/amharic.html'>Amharic</a></li><li><a href='https://stenos.io/arabic.html'>Arabic</a></li><li><a href='https://stenos.io/bulgarian.html'>Bulgarian</a></li><li><a href='https://stenos.io/burmese.html'>Burmese</a></li><li><a href='https://stenos.io/catalan.html'>Catalan</a></li><li><a href='https://stenos.io/chinese.html'>Chinese</a></li><li><a href='https://stenos.io/croatian.html'>Croatian</a></li><li><a href='https://stenos.io/czech.html'>Czech</a></li><li><a href='https://stenos.io/danish.html'>Danish</a></li><li><a href='https://stenos.io/dutch.html'>Dutch</a></li><li><a href='https://stenos.io/english.html'>English</a></li><li><a href='https://stenos.io/estonian.html'>Estonian</a></li><li><a href='https://stenos.io/filipino.html'>Filipino</a></li><li><a href='https://stenos.io/finnish.html'>Finnish</a></li><li><a href='https://stenos.io/french.html'>French</a></li><li><a href='https://stenos.io/german.html'>German</a></li><li><a href='https://stenos.io/greek.html'>Greek</a></li><li><a href='https://stenos.io/gujarati.html'>Gujarati</a></li><li><a href='https://stenos.io/hebrew.html'>Hebrew</a></li><li><a href='https://stenos.io/hindi.html'>Hindi</a></li><li><a href='https://stenos.io/hungarian.html'>Hungarian</a></li><li><a href='https://stenos.io/icelandic.html'>Icelandic</a></li><li><a href='https://stenos.io/indonesian.html'>Indonesian</a></li><li><a href='https://stenos.io/irish.html'>Irish</a></li><li><a href='https://stenos.io/italian.html'>Italian</a></li><li><a href='https://stenos.io/japanese.html'>Japanese</a></li><li><a href='https://stenos.io/javanese.html'>Javanese</a></li><li><a href='https://stenos.io/kannada.html'>Kannada</a></li><li><a href='https://stenos.io/khmer.html'>Khmer</a></li><li><a href='https://stenos.io/korean.html'>Korean</a></li><li><a href='https://stenos.io/lao.html'>Lao</a></li><li><a href='https://stenos.io/latvian.html'>Latvian</a></li><li><a href='https://stenos.io/lithuanian.html'>Lithuanian</a></li><li><a href='https://stenos.io/macedonian.html'>Macedonian</a></li><li><a href='https://stenos.io/malay.html'>Malay</a></li><li><a href='https://stenos.io/maltese.html'>Maltese</a></li><li><a href='https://stenos.io/marathi.html'>Marathi</a></li><li><a href='https://stenos.io/norwegian.html'>Norwegian</a></li><li><a href='https://stenos.io/persian.html'>Persian</a></li><li><a href='https://stenos.io/polish.html'>Polish</a></li><li><a href='https://stenos.io/portuguese.html'>Portuguese</a></li><li><a href='https://stenos.io/romanian.html'>Romanian</a></li><li><a href='https://stenos.io/russian.html'>Russian</a></li><li><a href='https://stenos.io/serbian.html'>Serbian</a></li><li><a href='https://stenos.io/sinhala.html'>Sinhala</a></li><li><a href='https://stenos.io/slovak.html'>Slovak</a></li><li><a href='https://stenos.io/slovenian.html'>Slovenian</a></li><li><a href='https://stenos.io/spanish.html'>Spanish</a></li><li><a href='https://stenos.io/swahili.html'>Swahili</a></li><li><a href='https://stenos.io/swedish.html'>Swedish</a></li><li><a href='https://stenos.io/tamil.html'>Tamil</a></li><li><a href='https://stenos.io/telugu.html'>Telugu</a></li><li><a href='https://stenos.io/thai.html'>Thai</a></li><li><a href='https://stenos.io/turkish.html'>Turkish</a></li><li><a href='https://stenos.io/ukrainian.html'>Ukrainian</a></li><li><a href='https://stenos.io/uzbek.html'>Uzbek</a></li><li><a href='https://stenos.io/vietnamese.html'>Vietnamese</a></li><li><a href='https://stenos.io/zulu.html'>Zulu</a></li>


## Generating the overview
First we generate the overview page that links to the specific subpages


```python
# We load the template for the page that links to the programmatic pages
overview_template = open_template("templates/multilingual.html")

# We construct a HTML fragment with a series of links in list elements
html_fragment = generate_html_link_list(lang_variation_dict.keys(), "https://stenos.io/")

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
    list_of_variants = ", ".join(lang_variation_dict[x])
    
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

    Afrikaans (South Africa)
    Amharic (Ethiopia)
    Arabic (Algeria), Arabic (Bahrain), modern standard, Arabic (Egypt), Arabic (Iraq), Arabic (Israel), Arabic (Jordan), Arabic (Kuwait), Arabic (Lebanon), Arabic (Libya), Arabic (Morocco), Arabic (Oman), Arabic (Palestinian Authority), Arabic (Qatar), Arabic (Saudi Arabia), Arabic (Syria), Arabic (Tunisia), Arabic (United Arab Emirates), Arabic (Yemen)
    Bulgarian (Bulgaria)
    Burmese (Myanmar)
    Catalan (Spain)
    Chinese (Cantonese, Traditional), Chinese (Mandarin, Simplified), Chinese (Taiwanese Mandarin)
    Croatian (Croatia)
    Czech (Czech)
    Danish (Denmark)
    Dutch (Belgium), Dutch (Netherlands)
    English (Australia), English (Canada), English (Ghana), English (Hong Kong), English (India), English (Ireland), English (Kenya), English (New Zealand), English (Nigeria), English (Philippines), English (Singapore), English (South Africa), English (Tanzania), English (United Kingdom), English (United States)
    Estonian (Estonia)
    Filipino (Philippines)
    Finnish (Finland)
    French (Belgium), French (Canada), French (France), French (Switzerland)
    German (Austria), German (Germany), German (Switzerland)
    Greek (Greece)
    Gujarati (Indian)
    Hebrew (Israel)
    Hindi (India)
    Hungarian (Hungary)
    Icelandic (Iceland)
    Indonesian (Indonesia)
    Irish (Ireland)
    Italian (Italy)
    Japanese (Japan)
    Javanese (Indonesia)
    Kannada (India)
    Khmer (Cambodia)
    Korean (Korea)
    Lao (Laos)
    Latvian (Latvia)
    Lithuanian (Lithuania)
    Macedonian (North Macedonia)
    Malay (Malaysia)
    Maltese (Malta)
    Marathi (India)
    Norwegian (Bokmål, Norway)
    Persian (Iran)
    Polish (Poland)
    Portuguese (Brazil), Portuguese (Portugal)
    Romanian (Romania)
    Russian (Russia)
    Serbian (Serbia)
    Sinhala (Sri Lanka)
    Slovak (Slovakia)
    Slovenian (Slovenia)
    Spanish (Argentina), Spanish (Bolivia), Spanish (Chile), Spanish (Colombia), Spanish (Costa Rica), Spanish (Cuba), Spanish (Dominican Republic), Spanish (Ecuador), Spanish (El Salvador), Spanish (Equatorial Guinea), Spanish (Guatemala), Spanish (Honduras), Spanish (Mexico), Spanish (Nicaragua), Spanish (Panama), Spanish (Paraguay), Spanish (Peru), Spanish (Puerto Rico), Spanish (Spain), Spanish (Uruguay), Spanish (USA), Spanish (Venezuela)
    Swahili (Kenya), Swahili (Tanzania)
    Swedish (Sweden)
    Tamil (India)
    Telugu (India)
    Thai (Thailand)
    Turkish (Turkey)
    Ukrainian (Ukraine)
    Uzbek (Uzbekistan)
    Vietnamese (Vietnam)
    Zulu (South Africa)


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
