# Instagram Post insights analysis

## This is an insight generator tool for Instagram posts

1. Download your Instagram analytic data by going to your Instagram account setting then go to > privacy and security option > Data Download > Request Download.
2. Create the folder "assets" in the root directory of the project.
3. Extract the "past_instagram_insights" folder from this downloaded dataset in step 1 to this "assets" folder from step 2
4. Copy and paste another folder "media" (which should be found in the downloaded dataset from step 2) to the path "assets/past_instagram_insights/"


TODO:

1. Analyze the comments
2. Analyze Videos
   1. Analyze audio from videos and Reels


## Load the json data and convert it to a CSV



```python
import json
import csv
from datetime import datetime

# [convert_json_to_csv] converts the given json file to csv


def convert_json_to_csv(parent_folder_path, file_name, top_json_key):

    # Opening JSON file and loading the data
    # into the variable data
    with open(parent_folder_path+file_name+'.json') as json_file:
        data = json.load(json_file)

    json_data = data[top_json_key]

    # now we will open a file for writing
    data_file = open(parent_folder_path+file_name+'.csv', 'w')

    # create the csv writer object
    csv_writer = csv.writer(data_file)

    # Counter variable used for writing
    # headers to the CSV file
    count = 0

    for data in json_data:
        if count == 0:
            # Writing headers of CSV file
            #header = list(data.keys())
            csv_writer.writerow(
                ['title', 'post_media', 'creation_timestamp', 'post_likes'])
            count += 1

        # Writing data of CSV fileu
        # cell = [data['media_map_data']['Media Thumbnail']['title'], path_to_image_html(parent_folder_path, data['media_map_data']['Media Thumbnail']['uri']),
        #         data['media_map_data']['Media Thumbnail']['creation_timestamp'], data['string_map_data']['Likes']['value']]
        cell = [data['media_map_data']['Media Thumbnail']['title'], data['media_map_data']['Media Thumbnail']['uri'],
                datetime.fromtimestamp(data['media_map_data']['Media Thumbnail']['creation_timestamp']), data['string_map_data']['Likes']['value']]
        csv_writer.writerow(cell)

    data_file.close()

```

## Call the convert_json_to_csv function to convert json to csv



```python
import pandas as pd

parent_folder_path = 'assets/past_instagram_insights/'
file_name = 'posts'

# Convert the json into CSV
convert_json_to_csv(parent_folder_path, file_name, 'organic_insights_posts')

# Load this saved csv
csv = pd.read_csv(parent_folder_path + file_name+'.csv')

```

## Load this saved csv



```python
import pandas as pd

# Load the saved csv
df = pd.read_csv(parent_folder_path + file_name+'.csv', index_col=False)

print('Shape: ', df.shape)

# Limit the rows for testing
# df = df.head(15)
df

```

    Shape:  (540, 4)





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>post_media</th>
      <th>creation_timestamp</th>
      <th>post_likes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Python â¢\nTag Your Programmer groups â¢\nâ...</td>
      <td>media/posts/201912/69503764_1316105261901993_3...</td>
      <td>2019-12-03 21:15:34</td>
      <td>19</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@multiverseapp Follow us @multiverseapp for mo...</td>
      <td>media/posts/201912/79984281_434751277472386_56...</td>
      <td>2019-12-02 22:33:00</td>
      <td>19</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Beautiful Onboarding animation concept of a 'I...</td>
      <td>media/posts/201912/FQIoAkMzLBdAG3bItDlYEBgSZGF...</td>
      <td>2019-12-02 22:30:26</td>
      <td>30</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@multiverseapp Follow @multiverseapp for daily...</td>
      <td>media/posts/201912/79862581_805719879875319_32...</td>
      <td>2019-12-02 22:28:03</td>
      <td>19</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@welovewebdesign by Miroslav SkoÄdopoly\nFoll...</td>
      <td>media/posts/201912/78766756_198610067846038_40...</td>
      <td>2019-12-02 22:04:20</td>
      <td>18</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>535</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/147398703_249918756774677_6...</td>
      <td>2021-02-06 21:00:30</td>
      <td>24</td>
    </tr>
    <tr>
      <th>536</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/145893246_179494400637566_8...</td>
      <td>2021-02-05 21:01:06</td>
      <td>18</td>
    </tr>
    <tr>
      <th>537</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/145403028_1579481252439746_...</td>
      <td>2021-02-04 21:00:37</td>
      <td>13</td>
    </tr>
    <tr>
      <th>538</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/145495364_846748032724421_7...</td>
      <td>2021-02-03 21:00:35</td>
      <td>10</td>
    </tr>
    <tr>
      <th>539</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/144959755_1815253618650357_...</td>
      <td>2021-02-02 21:00:39</td>
      <td>7</td>
    </tr>
  </tbody>
</table>
<p>540 rows × 4 columns</p>
</div>



## Fetch hashtags from the title



```python
titles = df.title

all_hashtags = []

# Go theough the title, and saperate out all the hashtags
for i in range(len(titles)):
    # Define an empty hashtag list
    # If no hashtag is found, this empty list is returned
    hashtags_column = []
    title = titles[i]

    if (isinstance(title, str) and '#' in title):
        first_hashtag = title.find('#')
        title = title[first_hashtag:]
        hashtags_column = title.split('#')
        hashtags_column.remove('')

        # remove blank spaces
        for j in range(len(hashtags_column)):
            hashtags_column[j] = hashtags_column[j].replace(' ', '')

    all_hashtags.append(hashtags_column)

# Go through this column to get the most occuring hashtags
hashtags_dict = {}
for hashtag_cell in all_hashtags:
    for single_hashtag in hashtag_cell:
        if single_hashtag not in hashtags_dict:
            hashtags_dict[single_hashtag] = 1
        else:
            hashtags_dict[single_hashtag] += 1

# Rearrange the dict to have the highest occurance first
hashtags_dict = {k: v for k, v in sorted(
    hashtags_dict.items(), key=lambda item: item[1], reverse=True)}

# Get the first n hashtags from the most used hasttags
max_hashtag_count = 5
most_occuring_hashtags = list(hashtags_dict.keys())[:max_hashtag_count]
print('Most occuring hashtags', most_occuring_hashtags)

# Go through hashtags from the dataframe and set the
hashtag_distribution = []
hashtag_distribution

# Add these hashtags as new columns
# add appropriate values for the posts in which these hashtags occur
for row_hashtags in all_hashtags:
    hashtag_distribution_row = []
    for most_occuring_hashtag in most_occuring_hashtags:
        if most_occuring_hashtag in row_hashtags:
            hashtag_distribution_row.append(1)
        else:
            hashtag_distribution_row.append(0)
    hashtag_distribution.append(hashtag_distribution_row)

# Add this occurance frequency to the dataframe
for most_occuring_hashtag in most_occuring_hashtags:
    df[most_occuring_hashtags] = hashtag_distribution

df

```

    Most occuring hashtags ['uitrends', 'appdesign', 'mobiledesign', 'dailyui', 'userinterfacedesign']





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>post_media</th>
      <th>creation_timestamp</th>
      <th>post_likes</th>
      <th>uitrends</th>
      <th>appdesign</th>
      <th>mobiledesign</th>
      <th>dailyui</th>
      <th>userinterfacedesign</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Python â¢\nTag Your Programmer groups â¢\nâ...</td>
      <td>media/posts/201912/69503764_1316105261901993_3...</td>
      <td>2019-12-03 21:15:34</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@multiverseapp Follow us @multiverseapp for mo...</td>
      <td>media/posts/201912/79984281_434751277472386_56...</td>
      <td>2019-12-02 22:33:00</td>
      <td>19</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Beautiful Onboarding animation concept of a 'I...</td>
      <td>media/posts/201912/FQIoAkMzLBdAG3bItDlYEBgSZGF...</td>
      <td>2019-12-02 22:30:26</td>
      <td>30</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@multiverseapp Follow @multiverseapp for daily...</td>
      <td>media/posts/201912/79862581_805719879875319_32...</td>
      <td>2019-12-02 22:28:03</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@welovewebdesign by Miroslav SkoÄdopoly\nFoll...</td>
      <td>media/posts/201912/78766756_198610067846038_40...</td>
      <td>2019-12-02 22:04:20</td>
      <td>18</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>535</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/147398703_249918756774677_6...</td>
      <td>2021-02-06 21:00:30</td>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>536</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/145893246_179494400637566_8...</td>
      <td>2021-02-05 21:01:06</td>
      <td>18</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>537</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/145403028_1579481252439746_...</td>
      <td>2021-02-04 21:00:37</td>
      <td>13</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>538</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/145495364_846748032724421_7...</td>
      <td>2021-02-03 21:00:35</td>
      <td>10</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>539</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/144959755_1815253618650357_...</td>
      <td>2021-02-02 21:00:39</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>540 rows × 9 columns</p>
</div>



## Saperate all the date feilds



```python
dates = df.creation_timestamp

weekdays = []
times = []

for i in range(len(dates)):
    date = datetime.fromisoformat(dates[i])
    # Add day of the week as an integer, where Monday is 0 and Sunday is 6.
    weekdays.append(date.weekday())

    # Add time as a float
    times.append(date.time().hour + (date.time().minute / 60))

df['post_weekday'] = weekdays
df['post_time'] = times

```

## Get Dominant color from an image



```python
from colorthief import ColorThief
import webcolors

# To find the closest colour name https://stackoverflow.com/a/9694246/6559381


def closest_colour(requested_colour):
    min_colours = {}
    for key, name in webcolors.css3_hex_to_names.items():
        r_c, g_c, b_c = webcolors.hex_to_rgb(key)
        rd = (r_c - requested_colour[0]) ** 2
        gd = (g_c - requested_colour[1]) ** 2
        bd = (b_c - requested_colour[2]) ** 2
        min_colours[(rd + gd + bd)] = name
    return min_colours[min(min_colours.keys())]


post_media = df.post_media

dominant_colors = []

for i in range(len(post_media)):
    # Get the image from the list of images
    image = post_media[i]

    # Get the dominant color
    print('Finding the dominant color in the image: ', i)
    if '.jpg' in image:
        color_thief = ColorThief(parent_folder_path+image)
        dominant_color = color_thief.get_color(quality=1)
        dominant_colors.append(closest_colour((dominant_color)))
    else:
        dominant_colors.append('video')

df['post_dominant_colors'] = dominant_colors
df

```

    Finding the dominant color in the image:  0
    Finding the dominant color in the image:  1
    Finding the dominant color in the image:  2
    Finding the dominant color in the image:  3
    Finding the dominant color in the image:  4
    Finding the dominant color in the image:  5
    Finding the dominant color in the image:  6
    Finding the dominant color in the image:  7
    Finding the dominant color in the image:  8
    Finding the dominant color in the image:  9
    Finding the dominant color in the image:  10
    Finding the dominant color in the image:  11
    Finding the dominant color in the image:  12
    Finding the dominant color in the image:  13
    Finding the dominant color in the image:  14
    Finding the dominant color in the image:  15
    Finding the dominant color in the image:  16
    Finding the dominant color in the image:  17
    Finding the dominant color in the image:  18
    Finding the dominant color in the image:  19
    Finding the dominant color in the image:  20
    Finding the dominant color in the image:  21
    Finding the dominant color in the image:  22
    Finding the dominant color in the image:  23
    Finding the dominant color in the image:  24
    Finding the dominant color in the image:  25
    Finding the dominant color in the image:  26
    Finding the dominant color in the image:  27
    Finding the dominant color in the image:  28
    Finding the dominant color in the image:  29
    Finding the dominant color in the image:  30
    Finding the dominant color in the image:  31
    Finding the dominant color in the image:  32
    Finding the dominant color in the image:  33
    Finding the dominant color in the image:  34
    Finding the dominant color in the image:  35
    Finding the dominant color in the image:  36
    Finding the dominant color in the image:  37
    Finding the dominant color in the image:  38
    Finding the dominant color in the image:  39
    Finding the dominant color in the image:  40
    Finding the dominant color in the image:  41
    Finding the dominant color in the image:  42
    Finding the dominant color in the image:  43
    Finding the dominant color in the image:  44
    Finding the dominant color in the image:  45
    Finding the dominant color in the image:  46
    Finding the dominant color in the image:  47
    Finding the dominant color in the image:  48
    Finding the dominant color in the image:  49
    Finding the dominant color in the image:  50
    Finding the dominant color in the image:  51
    Finding the dominant color in the image:  52
    Finding the dominant color in the image:  53
    Finding the dominant color in the image:  54
    Finding the dominant color in the image:  55
    Finding the dominant color in the image:  56
    Finding the dominant color in the image:  57
    Finding the dominant color in the image:  58
    Finding the dominant color in the image:  59
    Finding the dominant color in the image:  60
    Finding the dominant color in the image:  61
    Finding the dominant color in the image:  62
    Finding the dominant color in the image:  63
    Finding the dominant color in the image:  64
    Finding the dominant color in the image:  65
    Finding the dominant color in the image:  66
    Finding the dominant color in the image:  67
    Finding the dominant color in the image:  68
    Finding the dominant color in the image:  69
    Finding the dominant color in the image:  70
    Finding the dominant color in the image:  71
    Finding the dominant color in the image:  72
    Finding the dominant color in the image:  73
    Finding the dominant color in the image:  74
    Finding the dominant color in the image:  75
    Finding the dominant color in the image:  76
    Finding the dominant color in the image:  77
    Finding the dominant color in the image:  78
    Finding the dominant color in the image:  79
    Finding the dominant color in the image:  80
    Finding the dominant color in the image:  81
    Finding the dominant color in the image:  82
    Finding the dominant color in the image:  83
    Finding the dominant color in the image:  84
    Finding the dominant color in the image:  85
    Finding the dominant color in the image:  86
    Finding the dominant color in the image:  87
    Finding the dominant color in the image:  88
    Finding the dominant color in the image:  89
    Finding the dominant color in the image:  90
    Finding the dominant color in the image:  91
    Finding the dominant color in the image:  92
    Finding the dominant color in the image:  93
    Finding the dominant color in the image:  94
    Finding the dominant color in the image:  95
    Finding the dominant color in the image:  96
    Finding the dominant color in the image:  97
    Finding the dominant color in the image:  98
    Finding the dominant color in the image:  99
    Finding the dominant color in the image:  100
    Finding the dominant color in the image:  101
    Finding the dominant color in the image:  102
    Finding the dominant color in the image:  103
    Finding the dominant color in the image:  104
    Finding the dominant color in the image:  105
    Finding the dominant color in the image:  106
    Finding the dominant color in the image:  107
    Finding the dominant color in the image:  108
    Finding the dominant color in the image:  109
    Finding the dominant color in the image:  110
    Finding the dominant color in the image:  111
    Finding the dominant color in the image:  112
    Finding the dominant color in the image:  113
    Finding the dominant color in the image:  114
    Finding the dominant color in the image:  115
    Finding the dominant color in the image:  116
    Finding the dominant color in the image:  117
    Finding the dominant color in the image:  118
    Finding the dominant color in the image:  119
    Finding the dominant color in the image:  120
    Finding the dominant color in the image:  121
    Finding the dominant color in the image:  122
    Finding the dominant color in the image:  123
    Finding the dominant color in the image:  124
    Finding the dominant color in the image:  125
    Finding the dominant color in the image:  126
    Finding the dominant color in the image:  127
    Finding the dominant color in the image:  128
    Finding the dominant color in the image:  129
    Finding the dominant color in the image:  130
    Finding the dominant color in the image:  131
    Finding the dominant color in the image:  132
    Finding the dominant color in the image:  133
    Finding the dominant color in the image:  134
    Finding the dominant color in the image:  135
    Finding the dominant color in the image:  136
    Finding the dominant color in the image:  137
    Finding the dominant color in the image:  138
    Finding the dominant color in the image:  139
    Finding the dominant color in the image:  140
    Finding the dominant color in the image:  141
    Finding the dominant color in the image:  142
    Finding the dominant color in the image:  143
    Finding the dominant color in the image:  144
    Finding the dominant color in the image:  145
    Finding the dominant color in the image:  146
    Finding the dominant color in the image:  147
    Finding the dominant color in the image:  148
    Finding the dominant color in the image:  149
    Finding the dominant color in the image:  150
    Finding the dominant color in the image:  151
    Finding the dominant color in the image:  152
    Finding the dominant color in the image:  153
    Finding the dominant color in the image:  154
    Finding the dominant color in the image:  155
    Finding the dominant color in the image:  156
    Finding the dominant color in the image:  157
    Finding the dominant color in the image:  158
    Finding the dominant color in the image:  159
    Finding the dominant color in the image:  160
    Finding the dominant color in the image:  161
    Finding the dominant color in the image:  162
    Finding the dominant color in the image:  163
    Finding the dominant color in the image:  164
    Finding the dominant color in the image:  165
    Finding the dominant color in the image:  166
    Finding the dominant color in the image:  167
    Finding the dominant color in the image:  168
    Finding the dominant color in the image:  169
    Finding the dominant color in the image:  170
    Finding the dominant color in the image:  171
    Finding the dominant color in the image:  172
    Finding the dominant color in the image:  173
    Finding the dominant color in the image:  174
    Finding the dominant color in the image:  175
    Finding the dominant color in the image:  176
    Finding the dominant color in the image:  177
    Finding the dominant color in the image:  178
    Finding the dominant color in the image:  179
    Finding the dominant color in the image:  180
    Finding the dominant color in the image:  181
    Finding the dominant color in the image:  182
    Finding the dominant color in the image:  183
    Finding the dominant color in the image:  184
    Finding the dominant color in the image:  185
    Finding the dominant color in the image:  186
    Finding the dominant color in the image:  187
    Finding the dominant color in the image:  188
    Finding the dominant color in the image:  189
    Finding the dominant color in the image:  190
    Finding the dominant color in the image:  191
    Finding the dominant color in the image:  192
    Finding the dominant color in the image:  193
    Finding the dominant color in the image:  194
    Finding the dominant color in the image:  195
    Finding the dominant color in the image:  196
    Finding the dominant color in the image:  197
    Finding the dominant color in the image:  198
    Finding the dominant color in the image:  199
    Finding the dominant color in the image:  200
    Finding the dominant color in the image:  201
    Finding the dominant color in the image:  202
    Finding the dominant color in the image:  203
    Finding the dominant color in the image:  204
    Finding the dominant color in the image:  205
    Finding the dominant color in the image:  206
    Finding the dominant color in the image:  207
    Finding the dominant color in the image:  208
    Finding the dominant color in the image:  209
    Finding the dominant color in the image:  210
    Finding the dominant color in the image:  211
    Finding the dominant color in the image:  212
    Finding the dominant color in the image:  213
    Finding the dominant color in the image:  214
    Finding the dominant color in the image:  215
    Finding the dominant color in the image:  216
    Finding the dominant color in the image:  217
    Finding the dominant color in the image:  218
    Finding the dominant color in the image:  219
    Finding the dominant color in the image:  220
    Finding the dominant color in the image:  221
    Finding the dominant color in the image:  222
    Finding the dominant color in the image:  223
    Finding the dominant color in the image:  224
    Finding the dominant color in the image:  225
    Finding the dominant color in the image:  226
    Finding the dominant color in the image:  227
    Finding the dominant color in the image:  228
    Finding the dominant color in the image:  229
    Finding the dominant color in the image:  230
    Finding the dominant color in the image:  231
    Finding the dominant color in the image:  232
    Finding the dominant color in the image:  233
    Finding the dominant color in the image:  234
    Finding the dominant color in the image:  235
    Finding the dominant color in the image:  236
    Finding the dominant color in the image:  237
    Finding the dominant color in the image:  238
    Finding the dominant color in the image:  239
    Finding the dominant color in the image:  240
    Finding the dominant color in the image:  241
    Finding the dominant color in the image:  242
    Finding the dominant color in the image:  243
    Finding the dominant color in the image:  244
    Finding the dominant color in the image:  245
    Finding the dominant color in the image:  246
    Finding the dominant color in the image:  247
    Finding the dominant color in the image:  248
    Finding the dominant color in the image:  249
    Finding the dominant color in the image:  250
    Finding the dominant color in the image:  251
    Finding the dominant color in the image:  252
    Finding the dominant color in the image:  253
    Finding the dominant color in the image:  254
    Finding the dominant color in the image:  255
    Finding the dominant color in the image:  256
    Finding the dominant color in the image:  257
    Finding the dominant color in the image:  258
    Finding the dominant color in the image:  259
    Finding the dominant color in the image:  260
    Finding the dominant color in the image:  261
    Finding the dominant color in the image:  262
    Finding the dominant color in the image:  263
    Finding the dominant color in the image:  264
    Finding the dominant color in the image:  265
    Finding the dominant color in the image:  266
    Finding the dominant color in the image:  267
    Finding the dominant color in the image:  268
    Finding the dominant color in the image:  269
    Finding the dominant color in the image:  270
    Finding the dominant color in the image:  271
    Finding the dominant color in the image:  272
    Finding the dominant color in the image:  273
    Finding the dominant color in the image:  274
    Finding the dominant color in the image:  275
    Finding the dominant color in the image:  276
    Finding the dominant color in the image:  277
    Finding the dominant color in the image:  278
    Finding the dominant color in the image:  279
    Finding the dominant color in the image:  280
    Finding the dominant color in the image:  281
    Finding the dominant color in the image:  282
    Finding the dominant color in the image:  283
    Finding the dominant color in the image:  284
    Finding the dominant color in the image:  285
    Finding the dominant color in the image:  286
    Finding the dominant color in the image:  287
    Finding the dominant color in the image:  288
    Finding the dominant color in the image:  289
    Finding the dominant color in the image:  290
    Finding the dominant color in the image:  291
    Finding the dominant color in the image:  292
    Finding the dominant color in the image:  293
    Finding the dominant color in the image:  294
    Finding the dominant color in the image:  295
    Finding the dominant color in the image:  296
    Finding the dominant color in the image:  297
    Finding the dominant color in the image:  298
    Finding the dominant color in the image:  299
    Finding the dominant color in the image:  300
    Finding the dominant color in the image:  301
    Finding the dominant color in the image:  302
    Finding the dominant color in the image:  303
    Finding the dominant color in the image:  304
    Finding the dominant color in the image:  305
    Finding the dominant color in the image:  306
    Finding the dominant color in the image:  307
    Finding the dominant color in the image:  308
    Finding the dominant color in the image:  309
    Finding the dominant color in the image:  310
    Finding the dominant color in the image:  311
    Finding the dominant color in the image:  312
    Finding the dominant color in the image:  313
    Finding the dominant color in the image:  314
    Finding the dominant color in the image:  315
    Finding the dominant color in the image:  316
    Finding the dominant color in the image:  317
    Finding the dominant color in the image:  318
    Finding the dominant color in the image:  319
    Finding the dominant color in the image:  320
    Finding the dominant color in the image:  321
    Finding the dominant color in the image:  322
    Finding the dominant color in the image:  323
    Finding the dominant color in the image:  324
    Finding the dominant color in the image:  325
    Finding the dominant color in the image:  326
    Finding the dominant color in the image:  327
    Finding the dominant color in the image:  328
    Finding the dominant color in the image:  329
    Finding the dominant color in the image:  330
    Finding the dominant color in the image:  331
    Finding the dominant color in the image:  332
    Finding the dominant color in the image:  333
    Finding the dominant color in the image:  334
    Finding the dominant color in the image:  335
    Finding the dominant color in the image:  336
    Finding the dominant color in the image:  337
    Finding the dominant color in the image:  338
    Finding the dominant color in the image:  339
    Finding the dominant color in the image:  340
    Finding the dominant color in the image:  341
    Finding the dominant color in the image:  342
    Finding the dominant color in the image:  343
    Finding the dominant color in the image:  344
    Finding the dominant color in the image:  345
    Finding the dominant color in the image:  346
    Finding the dominant color in the image:  347
    Finding the dominant color in the image:  348
    Finding the dominant color in the image:  349
    Finding the dominant color in the image:  350
    Finding the dominant color in the image:  351
    Finding the dominant color in the image:  352
    Finding the dominant color in the image:  353
    Finding the dominant color in the image:  354
    Finding the dominant color in the image:  355
    Finding the dominant color in the image:  356
    Finding the dominant color in the image:  357
    Finding the dominant color in the image:  358
    Finding the dominant color in the image:  359
    Finding the dominant color in the image:  360
    Finding the dominant color in the image:  361
    Finding the dominant color in the image:  362
    Finding the dominant color in the image:  363
    Finding the dominant color in the image:  364
    Finding the dominant color in the image:  365
    Finding the dominant color in the image:  366
    Finding the dominant color in the image:  367
    Finding the dominant color in the image:  368
    Finding the dominant color in the image:  369
    Finding the dominant color in the image:  370
    Finding the dominant color in the image:  371
    Finding the dominant color in the image:  372
    Finding the dominant color in the image:  373
    Finding the dominant color in the image:  374
    Finding the dominant color in the image:  375
    Finding the dominant color in the image:  376
    Finding the dominant color in the image:  377
    Finding the dominant color in the image:  378
    Finding the dominant color in the image:  379
    Finding the dominant color in the image:  380
    Finding the dominant color in the image:  381
    Finding the dominant color in the image:  382
    Finding the dominant color in the image:  383
    Finding the dominant color in the image:  384
    Finding the dominant color in the image:  385
    Finding the dominant color in the image:  386
    Finding the dominant color in the image:  387
    Finding the dominant color in the image:  388
    Finding the dominant color in the image:  389
    Finding the dominant color in the image:  390
    Finding the dominant color in the image:  391
    Finding the dominant color in the image:  392
    Finding the dominant color in the image:  393
    Finding the dominant color in the image:  394
    Finding the dominant color in the image:  395
    Finding the dominant color in the image:  396
    Finding the dominant color in the image:  397
    Finding the dominant color in the image:  398
    Finding the dominant color in the image:  399
    Finding the dominant color in the image:  400
    Finding the dominant color in the image:  401
    Finding the dominant color in the image:  402
    Finding the dominant color in the image:  403
    Finding the dominant color in the image:  404
    Finding the dominant color in the image:  405
    Finding the dominant color in the image:  406
    Finding the dominant color in the image:  407
    Finding the dominant color in the image:  408
    Finding the dominant color in the image:  409
    Finding the dominant color in the image:  410
    Finding the dominant color in the image:  411
    Finding the dominant color in the image:  412
    Finding the dominant color in the image:  413
    Finding the dominant color in the image:  414
    Finding the dominant color in the image:  415
    Finding the dominant color in the image:  416
    Finding the dominant color in the image:  417
    Finding the dominant color in the image:  418
    Finding the dominant color in the image:  419
    Finding the dominant color in the image:  420
    Finding the dominant color in the image:  421
    Finding the dominant color in the image:  422
    Finding the dominant color in the image:  423
    Finding the dominant color in the image:  424
    Finding the dominant color in the image:  425
    Finding the dominant color in the image:  426
    Finding the dominant color in the image:  427
    Finding the dominant color in the image:  428
    Finding the dominant color in the image:  429
    Finding the dominant color in the image:  430
    Finding the dominant color in the image:  431
    Finding the dominant color in the image:  432
    Finding the dominant color in the image:  433
    Finding the dominant color in the image:  434
    Finding the dominant color in the image:  435
    Finding the dominant color in the image:  436
    Finding the dominant color in the image:  437
    Finding the dominant color in the image:  438
    Finding the dominant color in the image:  439
    Finding the dominant color in the image:  440
    Finding the dominant color in the image:  441
    Finding the dominant color in the image:  442
    Finding the dominant color in the image:  443
    Finding the dominant color in the image:  444
    Finding the dominant color in the image:  445
    Finding the dominant color in the image:  446
    Finding the dominant color in the image:  447
    Finding the dominant color in the image:  448
    Finding the dominant color in the image:  449
    Finding the dominant color in the image:  450
    Finding the dominant color in the image:  451
    Finding the dominant color in the image:  452
    Finding the dominant color in the image:  453
    Finding the dominant color in the image:  454
    Finding the dominant color in the image:  455
    Finding the dominant color in the image:  456
    Finding the dominant color in the image:  457
    Finding the dominant color in the image:  458
    Finding the dominant color in the image:  459
    Finding the dominant color in the image:  460
    Finding the dominant color in the image:  461
    Finding the dominant color in the image:  462
    Finding the dominant color in the image:  463
    Finding the dominant color in the image:  464
    Finding the dominant color in the image:  465
    Finding the dominant color in the image:  466
    Finding the dominant color in the image:  467
    Finding the dominant color in the image:  468
    Finding the dominant color in the image:  469
    Finding the dominant color in the image:  470
    Finding the dominant color in the image:  471
    Finding the dominant color in the image:  472
    Finding the dominant color in the image:  473
    Finding the dominant color in the image:  474
    Finding the dominant color in the image:  475
    Finding the dominant color in the image:  476
    Finding the dominant color in the image:  477
    Finding the dominant color in the image:  478
    Finding the dominant color in the image:  479
    Finding the dominant color in the image:  480
    Finding the dominant color in the image:  481
    Finding the dominant color in the image:  482
    Finding the dominant color in the image:  483
    Finding the dominant color in the image:  484
    Finding the dominant color in the image:  485
    Finding the dominant color in the image:  486
    Finding the dominant color in the image:  487
    Finding the dominant color in the image:  488
    Finding the dominant color in the image:  489
    Finding the dominant color in the image:  490
    Finding the dominant color in the image:  491
    Finding the dominant color in the image:  492
    Finding the dominant color in the image:  493
    Finding the dominant color in the image:  494
    Finding the dominant color in the image:  495
    Finding the dominant color in the image:  496
    Finding the dominant color in the image:  497
    Finding the dominant color in the image:  498
    Finding the dominant color in the image:  499
    Finding the dominant color in the image:  500
    Finding the dominant color in the image:  501
    Finding the dominant color in the image:  502
    Finding the dominant color in the image:  503
    Finding the dominant color in the image:  504
    Finding the dominant color in the image:  505
    Finding the dominant color in the image:  506
    Finding the dominant color in the image:  507
    Finding the dominant color in the image:  508
    Finding the dominant color in the image:  509
    Finding the dominant color in the image:  510
    Finding the dominant color in the image:  511
    Finding the dominant color in the image:  512
    Finding the dominant color in the image:  513
    Finding the dominant color in the image:  514
    Finding the dominant color in the image:  515
    Finding the dominant color in the image:  516
    Finding the dominant color in the image:  517
    Finding the dominant color in the image:  518
    Finding the dominant color in the image:  519
    Finding the dominant color in the image:  520
    Finding the dominant color in the image:  521
    Finding the dominant color in the image:  522
    Finding the dominant color in the image:  523
    Finding the dominant color in the image:  524
    Finding the dominant color in the image:  525
    Finding the dominant color in the image:  526
    Finding the dominant color in the image:  527
    Finding the dominant color in the image:  528
    Finding the dominant color in the image:  529
    Finding the dominant color in the image:  530
    Finding the dominant color in the image:  531
    Finding the dominant color in the image:  532
    Finding the dominant color in the image:  533
    Finding the dominant color in the image:  534
    Finding the dominant color in the image:  535
    Finding the dominant color in the image:  536
    Finding the dominant color in the image:  537
    Finding the dominant color in the image:  538
    Finding the dominant color in the image:  539





<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>post_media</th>
      <th>creation_timestamp</th>
      <th>post_likes</th>
      <th>uitrends</th>
      <th>appdesign</th>
      <th>mobiledesign</th>
      <th>dailyui</th>
      <th>userinterfacedesign</th>
      <th>post_weekday</th>
      <th>post_time</th>
      <th>post_dominant_colors</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Python â¢\nTag Your Programmer groups â¢\nâ...</td>
      <td>media/posts/201912/69503764_1316105261901993_3...</td>
      <td>2019-12-03 21:15:34</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>21.250000</td>
      <td>black</td>
    </tr>
    <tr>
      <th>1</th>
      <td>@multiverseapp Follow us @multiverseapp for mo...</td>
      <td>media/posts/201912/79984281_434751277472386_56...</td>
      <td>2019-12-02 22:33:00</td>
      <td>19</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>22.550000</td>
      <td>video</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Beautiful Onboarding animation concept of a 'I...</td>
      <td>media/posts/201912/FQIoAkMzLBdAG3bItDlYEBgSZGF...</td>
      <td>2019-12-02 22:30:26</td>
      <td>30</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>22.500000</td>
      <td>video</td>
    </tr>
    <tr>
      <th>3</th>
      <td>@multiverseapp Follow @multiverseapp for daily...</td>
      <td>media/posts/201912/79862581_805719879875319_32...</td>
      <td>2019-12-02 22:28:03</td>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>22.466667</td>
      <td>video</td>
    </tr>
    <tr>
      <th>4</th>
      <td>@welovewebdesign by Miroslav SkoÄdopoly\nFoll...</td>
      <td>media/posts/201912/78766756_198610067846038_40...</td>
      <td>2019-12-02 22:04:20</td>
      <td>18</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>22.066667</td>
      <td>darkslategrey</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>535</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/147398703_249918756774677_6...</td>
      <td>2021-02-06 21:00:30</td>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>21.000000</td>
      <td>mistyrose</td>
    </tr>
    <tr>
      <th>536</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/145893246_179494400637566_8...</td>
      <td>2021-02-05 21:01:06</td>
      <td>18</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>21.016667</td>
      <td>bisque</td>
    </tr>
    <tr>
      <th>537</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/145403028_1579481252439746_...</td>
      <td>2021-02-04 21:00:37</td>
      <td>13</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>21.000000</td>
      <td>bisque</td>
    </tr>
    <tr>
      <th>538</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/145495364_846748032724421_7...</td>
      <td>2021-02-03 21:00:35</td>
      <td>10</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>21.000000</td>
      <td>linen</td>
    </tr>
    <tr>
      <th>539</th>
      <td>ð¥ We are open for Design work!\nð© DM us,...</td>
      <td>media/posts/202102/144959755_1815253618650357_...</td>
      <td>2021-02-02 21:00:39</td>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>21.000000</td>
      <td>lightpink</td>
    </tr>
  </tbody>
</table>
<p>540 rows × 12 columns</p>
</div>



## Drop unwanted columns



```python
if df.__contains__('index'):
    df.reset_index(drop=True, inplace=True)
if df.__contains__('title'):
    df.drop(columns=['title'], inplace=True)
if df.__contains__('creation_timestamp'):
    df.drop(columns=['creation_timestamp'], inplace=True)
if df.__contains__('post_media'):
    df.drop(columns=['post_media'], inplace=True)
if df.__contains__('hashtags'):
    df.drop(columns=['hashtags'], inplace=True)
df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>post_likes</th>
      <th>uitrends</th>
      <th>appdesign</th>
      <th>mobiledesign</th>
      <th>dailyui</th>
      <th>userinterfacedesign</th>
      <th>post_weekday</th>
      <th>post_time</th>
      <th>post_dominant_colors</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>21.250000</td>
      <td>black</td>
    </tr>
    <tr>
      <th>1</th>
      <td>19</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>22.550000</td>
      <td>video</td>
    </tr>
    <tr>
      <th>2</th>
      <td>30</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>22.500000</td>
      <td>video</td>
    </tr>
    <tr>
      <th>3</th>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>22.466667</td>
      <td>video</td>
    </tr>
    <tr>
      <th>4</th>
      <td>18</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>22.066667</td>
      <td>darkslategrey</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>535</th>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>21.000000</td>
      <td>mistyrose</td>
    </tr>
    <tr>
      <th>536</th>
      <td>18</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>21.016667</td>
      <td>bisque</td>
    </tr>
    <tr>
      <th>537</th>
      <td>13</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>21.000000</td>
      <td>bisque</td>
    </tr>
    <tr>
      <th>538</th>
      <td>10</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>21.000000</td>
      <td>linen</td>
    </tr>
    <tr>
      <th>539</th>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>21.000000</td>
      <td>lightpink</td>
    </tr>
  </tbody>
</table>
<p>540 rows × 9 columns</p>
</div>



## Save this clean Dataset


```python
df.to_csv(parent_folder_path+file_name+'_clean.csv')

```

## Visualise the most liked Data

From the entire dataset, show the charecteristics of the top n most liked posts

we will use _sweetviz_ to visualize the data



```python
import sweetviz as sv

# Creates the graph of the top n (index_to_truncate_from) most liked posts


def visualize_top_n_most(index_to_truncate_from):
    df_report = df.sort_values(
        by='post_likes', ascending=False).head(index_to_truncate_from)
    sv.analyze(df_report).show_html()


# Creates the report graph of the top 10 most liked posts
visualize_top_n_most(10)

```

    Done! Use 'show' commands to display/save.   |██████████| [100%]   00:01 -> (00:00 left)

    Report SWEETVIZ_REPORT.html was generated! NOTEBOOK/COLAB USERS: the web browser MAY not pop up, regardless, the report IS saved in your notebook/colab files.


    


## Encode categorical data



```python
from sklearn.preprocessing import LabelEncoder

le_post_dominant_colors = LabelEncoder()
df['post_dominant_colors'] = le_post_dominant_colors.fit_transform(
    df['post_dominant_colors'])

df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>post_likes</th>
      <th>uitrends</th>
      <th>appdesign</th>
      <th>mobiledesign</th>
      <th>dailyui</th>
      <th>userinterfacedesign</th>
      <th>post_weekday</th>
      <th>post_time</th>
      <th>post_dominant_colors</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>21.250000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>19</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>22.550000</td>
      <td>48</td>
    </tr>
    <tr>
      <th>2</th>
      <td>30</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>22.500000</td>
      <td>48</td>
    </tr>
    <tr>
      <th>3</th>
      <td>19</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>22.466667</td>
      <td>48</td>
    </tr>
    <tr>
      <th>4</th>
      <td>18</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>22.066667</td>
      <td>10</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>535</th>
      <td>24</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>21.000000</td>
      <td>30</td>
    </tr>
    <tr>
      <th>536</th>
      <td>18</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>4</td>
      <td>21.016667</td>
      <td>3</td>
    </tr>
    <tr>
      <th>537</th>
      <td>13</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>21.000000</td>
      <td>3</td>
    </tr>
    <tr>
      <th>538</th>
      <td>10</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>21.000000</td>
      <td>24</td>
    </tr>
    <tr>
      <th>539</th>
      <td>7</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>21.000000</td>
      <td>20</td>
    </tr>
  </tbody>
</table>
<p>540 rows × 9 columns</p>
</div>



## Create Training data sets



```python
from sklearn.model_selection import train_test_split

# Let's extract features data assigning to X and labels data assigning to:
X = pd.DataFrame(df.values, columns=df.columns)
y = pd.DataFrame(df['post_likes'], columns=["post_likes"])

(X_train, X_tmp, y_train, y_tmp) = train_test_split(
    X, y, train_size=0.7, random_state=1)
(X_test, X_val, y_test, y_val) = train_test_split(
    X_tmp, y_tmp, train_size=0.6, random_state=1)


print('Training data shape: ', X_train.shape)
print('Validation data shape: ', X_val.shape)
print('Testing data shape: ', X_test.shape)

```

    Training data shape:  (378, 9)
    Validation data shape:  (65, 9)
    Testing data shape:  (97, 9)


## Train Decision Tree



```python
from IPython.display import Image
from subprocess import call
from sklearn.tree import export_graphviz
from sklearn.tree import DecisionTreeClassifier

# Defining and fitting a DecisionTreeClassifier instance
tree1 = DecisionTreeClassifier()
tree1.fit(X_train, y_train)

```




    DecisionTreeClassifier()



## Validate the Decision tee



```python
tree1.score(X_val, y_val)

```




    0.8615384615384616



## Test the Decision tee



```python
tree1.score(X_test, y_test)

```




    0.8865979381443299



## Make a Prediction

Now that we know how our decision tree works, let us make predictions.



```python
#sample_one_pred_tree1 = int(tree1.predict([[5, 5, 1, 3]]))
```
