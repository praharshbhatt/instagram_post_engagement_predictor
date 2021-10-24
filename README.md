# Instagram Post insights analysis

## This is an insight generator and prediction tool for your Instagram posts

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

## Encode categorical data



```python
from sklearn.preprocessing import LabelEncoder

le_post_dominant_colors = LabelEncoder()
df['post_dominant_colors'] = le_post_dominant_colors.fit_transform(
    df['post_dominant_colors'])

df
```

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

## Validate the Decision tee



```python
tree1.score(X_val, y_val)

```

## Test the Decision tee



```python
tree1.score(X_test, y_test)

```

## Make a Prediction

Now that we know how our decision tree works, let us make predictions.



```python
#sample_one_pred_tree1 = int(tree1.predict([[5, 5, 1, 3]]))
```


### That's it!
<img src='assets/images/Pink and Purple Professional LinkedIn Banner.gif' width='600'>