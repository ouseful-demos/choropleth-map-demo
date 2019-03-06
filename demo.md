## Demo

<section-start onLoad>

```python
import geopandas

#From the downloads area of the page, grab the link for the shapefile download
url='https://opendata.arcgis.com/datasets/7ff28788e1e640de8150fb8f35703f6e_2.zip?outSR=%7B%22wkid%22%3A27700%2C%22latestWkid%22%3A27700%7D'
gdf = geopandas.read_file(url)

data_url = 'https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/464464/File_10_ID2015_Local_Authority_District_Summaries.xlsx'

import pandas as pd

df = pd.read_excel(data_url, sheet_name=None)

datacols = {c.split('-')[1].strip():c for c in df['Education'].columns if c.startswith('Education')}
datacols

from ipywidgets import interact

#Merge in data
gdf = pd.merge(gdf, df['Education'], 
               how='inner',  #The type of join (what happens if data is in one dataset and not the other)
               left_on='lad16cd', #Column we're merging on in left dataframe
               right_on='Local Authority District code (2013)'#Column we're merging on in right dataframe
              )
```
</section-start>


<section-live>

<variable-dropdown items="[c for c in df['Education'].columns if c.startswith('Education')]" >Indicator</variable-dropdown>

```python
ax = gdf.plot(column=Indicator,  cmap='OrRd')
ax.axis('off');
```
    </section-live>
