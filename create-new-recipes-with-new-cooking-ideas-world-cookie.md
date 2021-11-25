**Some Cooking Ideas for Tonight**

* The idea is to create some new recipes when people are looking for something to eat at home
* Build some ingredients set for each cuisine and randomly choose the ingredients


```python
# This Python 3 environment comes with many helpful analytics libraries installed
# It is defined by the kaggle/python docker image: https://github.com/kaggle/docker-python
# For example, here's several helpful packages to load in 

#Libraries import
import pandas as pd
import numpy as np
import csv as csv
import json
import re
import random #Used to randomly choose ingredients

import os
print(os.listdir("../input"))

# Any results you write to the current directory are saved as output.
```

    ['sample_submission.csv.zip', 'train.json', 'test.json']
    


```python
with open('../input/train.json', 'r') as f:
    train = json.load(f)
train_raw_df = pd.DataFrame(train)

with open('../input/test.json', 'r') as f:
    test = json.load(f)
test_raw_df = pd.DataFrame(test)
```

**Some Basic Data Cleaning**


```python
# Remove numbers and only keep words
# substitute the matched pattern
# update the ingredients
def sub_match(pattern, sub_pattern, ingredients):
    for i in ingredients.index.values:
        for j in range(len(ingredients[i])):
            ingredients[i][j] = re.sub(pattern, sub_pattern, ingredients[i][j].strip())
            ingredients[i][j] = ingredients[i][j].strip()
    re.purge()
    return ingredients

#remove units
p0 = re.compile(r'\s*(oz|ounc|ounce|pound|lb|inch|inches|kg|to)\s*[^a-z]')
train_raw_df['ingredients'] = sub_match(p0, ' ', train_raw_df['ingredients'])
# remove digits
p1 = re.compile(r'\d+')
train_raw_df['ingredients'] = sub_match(p1, ' ', train_raw_df['ingredients'])
# remove non-letter characters
p2 = re.compile('[^\w]')
train_raw_df['ingredients'] = sub_match(p2, ' ', train_raw_df['ingredients'])

y_train = train_raw_df['cuisine'].values
train_ingredients = train_raw_df['ingredients'].values
train_ingredients_update = list()
for item in train_ingredients:
    item = [x.lower().replace(' ', '+') for x in item]
    train_ingredients_update.append(item)
X_train = [' '.join(x) for x in train_ingredients_update]
```


```python
# Create the dataframe for creating new recipes
food_df = pd.DataFrame({'cuisine':y_train
              ,'ingredients':train_ingredients_update})
```

**Randomly choose ingredients for the desired cuisine**


```python
# the randomly picked function
def random_generate_recipe(raw_df, food_type, num_ingredients):
    if food_type not in raw_df['cuisine'].values:
        print('Food type is not existing here')
    food_ingredients_lst = list()
    [food_ingredients_lst.extend(recipe) for recipe in raw_df[raw_df['cuisine'] == food_type]['ingredients'].values] 
    i = 0
    new_recipe, tmp = list(), list()
    while i < num_ingredients:
        item = random.choice(food_ingredients_lst)
        if item not in tmp:
            tmp.append(item)
            new_recipe.append(item.replace('+', ' '))
            i+=1
        else:
            continue
    recipt_str = ', '.join(new_recipe)
    print('The new recipte for %s can be: %s' %(food_type, recipt_str))
    return new_recipe
```

#중국 음식


```python
#Say you want some chinese food and you want to only have 10 ingredients in it
random_generate_recipe(food_df, 'chinese', 10)
```

    The new recipte for chinese can be: red pepper, pineapple, ground black pepper, seeds, water, sherry, chopped garlic, onions, sugar, dry sherry
    




    ['red pepper',
     'pineapple',
     'ground black pepper',
     'seeds',
     'water',
     'sherry',
     'chopped garlic',
     'onions',
     'sugar',
     'dry sherry']



#인도 음식


```python
#Say you want some indian food and you want to only have 12 ingredients in it
random_generate_recipe(food_df, 'indian', 12)
```

    The new recipte for indian can be: dried mint flakes, cooking oil, chili powder, ground cumin, mango, ground ginger, cumin seed, couscous, chicken breasts, red chili powder, diced tomatoes, coriander seeds
    




    ['dried mint flakes',
     'cooking oil',
     'chili powder',
     'ground cumin',
     'mango',
     'ground ginger',
     'cumin seed',
     'couscous',
     'chicken breasts',
     'red chili powder',
     'diced tomatoes',
     'coriander seeds']



#프랑스 음식


```python
#Say you want some french food and you want to only have 8 ingredients in it
random_generate_recipe(food_df, 'french', 12)
```

    The new recipte for french can be: dried tarragon leaves, pin beans, carrots, anise, shredded mozzarella cheese, fresh basil, large egg whites, mussels, sliced carrots, whiskey, heavy cream, all purpose flour
    




    ['dried tarragon leaves',
     'pin beans',
     'carrots',
     'anise',
     'shredded mozzarella cheese',
     'fresh basil',
     'large egg whites',
     'mussels',
     'sliced carrots',
     'whiskey',
     'heavy cream',
     'all purpose flour']



#한국 음식


```python
random_generate_recipe(food_df, 'korean', 12)
```

    The new recipte for korean can be: brown sugar, scallions, water, ginger, soybean sprouts, canola oil, white distilled vinegar, sugar, chicken stock, vegetable broth, kimchi, grapeseed oil
    




    ['brown sugar',
     'scallions',
     'water',
     'ginger',
     'soybean sprouts',
     'canola oil',
     'white distilled vinegar',
     'sugar',
     'chicken stock',
     'vegetable broth',
     'kimchi',
     'grapeseed oil']



#러시아 음식


```python
random_generate_recipe(food_df, 'russian', 12)
```

    The new recipte for russian can be: red wine vinegar, white pepper, beef broth, fresh basil, sour cream, plums, onions, carrots, water, oil, milk, butter
    




    ['red wine vinegar',
     'white pepper',
     'beef broth',
     'fresh basil',
     'sour cream',
     'plums',
     'onions',
     'carrots',
     'water',
     'oil',
     'milk',
     'butter']



#그리스 음식


```python
random_generate_recipe(food_df, 'greek', 12)
```

    The new recipte for greek can be: grated parmesan cheese, great northern beans, romaine lettuce, heavy cream, sugar, garlic cloves, green bell pepper, fresh lemon juice, flat leaf parsley, paprika, olive oil, salt
    




    ['grated parmesan cheese',
     'great northern beans',
     'romaine lettuce',
     'heavy cream',
     'sugar',
     'garlic cloves',
     'green bell pepper',
     'fresh lemon juice',
     'flat leaf parsley',
     'paprika',
     'olive oil',
     'salt']



#스페인 음식


```python
random_generate_recipe(food_df, 'spanish',12)
```

    The new recipte for spanish can be: hungarian paprika, semisweet chocolate, dijon mustard, fresh lime juice, bay leaves, hothouse cucumber, serrano, veal shanks, olive oil, chopped fresh mint, onions, carrots
    




    ['hungarian paprika',
     'semisweet chocolate',
     'dijon mustard',
     'fresh lime juice',
     'bay leaves',
     'hothouse cucumber',
     'serrano',
     'veal shanks',
     'olive oil',
     'chopped fresh mint',
     'onions',
     'carrots']



#이탈리아 음식


```python
random_generate_recipe(food_df, 'italian', 12)
```

    The new recipte for italian can be: almonds, worcestershire sauce, olive oil, ground black pepper, eggplant, white beans, bread flour, freshly grated parmesan, large egg yolks, parmigiano reggiano cheese, dried oregano, artichokes
    




    ['almonds',
     'worcestershire sauce',
     'olive oil',
     'ground black pepper',
     'eggplant',
     'white beans',
     'bread flour',
     'freshly grated parmesan',
     'large egg yolks',
     'parmigiano reggiano cheese',
     'dried oregano',
     'artichokes']



#일본 음식


```python
random_generate_recipe(food_df, 'japanese', 12)
```

    The new recipte for japanese can be: table salt, butternut squash, shiro miso, cabbage, beef stock, salmon fillets, dark sesame oil, ground turmeric, konbu, water, carrots, quinoa
    




    ['table salt',
     'butternut squash',
     'shiro miso',
     'cabbage',
     'beef stock',
     'salmon fillets',
     'dark sesame oil',
     'ground turmeric',
     'konbu',
     'water',
     'carrots',
     'quinoa']



#태국 음식


```python
random_generate_recipe(food_df, 'thai', 12)
```

    The new recipte for thai can be: lime zest, fresh ginger root, fresh basil, sesame seeds, carrots, cilantro, peanuts, lemon grass, coconut milk, garlic cloves, baby spinach leaves, fish sauce
    




    ['lime zest',
     'fresh ginger root',
     'fresh basil',
     'sesame seeds',
     'carrots',
     'cilantro',
     'peanuts',
     'lemon grass',
     'coconut milk',
     'garlic cloves',
     'baby spinach leaves',
     'fish sauce']



#남아메리카 음식


```python
random_generate_recipe(food_df, 'southern_us', 12)
```

    The new recipte for southern_us can be: butter, freshly ground pepper, tomatoes, sugar, flour, kosher salt, orange glaze, orange juice, pecans, diced tomatoes, all purpose flour, hot red pepper flakes
    




    ['butter',
     'freshly ground pepper',
     'tomatoes',
     'sugar',
     'flour',
     'kosher salt',
     'orange glaze',
     'orange juice',
     'pecans',
     'diced tomatoes',
     'all purpose flour',
     'hot red pepper flakes']



#멕시코 음식


```python
random_generate_recipe(food_df, 'mexican', 12)
```

    The new recipte for mexican can be: spaghetti squash, shredded monterey jack cheese, onions, oregano, dried oregano, mayonaise, corn tortillas, salsa, salt, cilantro, shredded mozzarella cheese, kosher salt
    




    ['spaghetti squash',
     'shredded monterey jack cheese',
     'onions',
     'oregano',
     'dried oregano',
     'mayonaise',
     'corn tortillas',
     'salsa',
     'salt',
     'cilantro',
     'shredded mozzarella cheese',
     'kosher salt']



#아이랜드 음식


```python
random_generate_recipe(food_df, 'irish', 12)
```

    The new recipte for irish can be: butter, sugar, cider vinegar, cooking spray, boiling potatoes, wholemeal flour, potatoes, all purpose flour, flax seed meal, eggs, unsalted butter, beef brisket
    




    ['butter',
     'sugar',
     'cider vinegar',
     'cooking spray',
     'boiling potatoes',
     'wholemeal flour',
     'potatoes',
     'all purpose flour',
     'flax seed meal',
     'eggs',
     'unsalted butter',
     'beef brisket']



#모로코 음식


```python
random_generate_recipe(food_df, 'moroccan', 12)
```

    The new recipte for moroccan can be: carrots, ground cardamom, hot water, salt, pinenuts, freshly ground pepper, ground cumin, cinnamon sticks, minced garlic, couscous, zucchini, rice
    




    ['carrots',
     'ground cardamom',
     'hot water',
     'salt',
     'pinenuts',
     'freshly ground pepper',
     'ground cumin',
     'cinnamon sticks',
     'minced garlic',
     'couscous',
     'zucchini',
     'rice']



#브라질 음식


```python
random_generate_recipe(food_df, 'brazilian', 12)
```

    The new recipte for brazilian can be: diced tomatoes, protein powder, canola oil, orange slices, garlic cloves, black beans, parsley, carrots, eggs, minced garlic, flaked coconut, fresh lime juice
    




    ['diced tomatoes',
     'protein powder',
     'canola oil',
     'orange slices',
     'garlic cloves',
     'black beans',
     'parsley',
     'carrots',
     'eggs',
     'minced garlic',
     'flaked coconut',
     'fresh lime juice']



#영국 음


```python
random_generate_recipe(food_df, 'british', 12)
```

    The new recipte for british can be: salt, candied orange peel, ground cinnamon, dry hard cider, eggs, whole milk, vanilla essence, bread flour, skinless haddock, dried apricot, ground beef, active dry yeast
    




    ['salt',
     'candied orange peel',
     'ground cinnamon',
     'dry hard cider',
     'eggs',
     'whole milk',
     'vanilla essence',
     'bread flour',
     'skinless haddock',
     'dried apricot',
     'ground beef',
     'active dry yeast']



#필리핀 음식


```python
random_generate_recipe(food_df, 'filipino', 12)
```

    The new recipte for filipino can be: pork belly, green beans, oyster sauce, lettuce leaves, ginger, pineapple chunks, pork butt, lean ground beef, sugar, spices, carrots, salt
    




    ['pork belly',
     'green beans',
     'oyster sauce',
     'lettuce leaves',
     'ginger',
     'pineapple chunks',
     'pork butt',
     'lean ground beef',
     'sugar',
     'spices',
     'carrots',
     'salt']




```python
print("First five elements in our training sample:")
train_raw_df.head()
```

    First five elements in our training sample:
    




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
      <th>cuisine</th>
      <th>id</th>
      <th>ingredients</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>greek</td>
      <td>10259</td>
      <td>[romaine lettuce, black olives, grape tomatoes...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>southern_us</td>
      <td>25693</td>
      <td>[plain flour, ground pepper, salt, tomatoes, g...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>filipino</td>
      <td>20130</td>
      <td>[eggs, pepper, salt, mayonaise, cooking oil, g...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>indian</td>
      <td>22213</td>
      <td>[water, vegetable oil, wheat, salt]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>indian</td>
      <td>13162</td>
      <td>[black pepper, shallots, cornflour, cayenne pe...</td>
    </tr>
  </tbody>
</table>
</div>




```python
train_raw_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 39774 entries, 0 to 39773
    Data columns (total 3 columns):
    cuisine        39774 non-null object
    id             39774 non-null int64
    ingredients    39774 non-null object
    dtypes: int64(1), object(2)
    memory usage: 932.3+ KB
    


```python
test_raw_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 9944 entries, 0 to 9943
    Data columns (total 2 columns):
    id             9944 non-null int64
    ingredients    9944 non-null object
    dtypes: int64(1), object(1)
    memory usage: 155.5+ KB
    


```python
train_raw_df.shape
```




    (39774, 3)




```python
test_raw_df.shape
```




    (9944, 2)




```python
print("The test data consists of {} recipes".format(len(test_raw_df)))
```

    The test data consists of 9944 recipes
    


```python
print("First five elements in our test sample:")
test_raw_df.head()
```

    First five elements in our test sample:
    




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
      <th>id</th>
      <th>ingredients</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>18009</td>
      <td>[baking powder, eggs, all-purpose flour, raisi...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>28583</td>
      <td>[sugar, egg yolks, corn starch, cream of tarta...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>41580</td>
      <td>[sausage links, fennel bulb, fronds, olive oil...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>29752</td>
      <td>[meat cuts, file powder, smoked sausage, okra,...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>35687</td>
      <td>[ground black pepper, salt, sausage casings, l...</td>
    </tr>
  </tbody>
</table>
</div>




```python
print("Number of cuisine categories: {}".format(len(train_raw_df.cuisine.unique())))
train_raw_df.cuisine.unique()
```

    Number of cuisine categories: 20
    




    array(['greek', 'southern_us', 'filipino', 'indian', 'jamaican',
           'spanish', 'italian', 'mexican', 'chinese', 'british', 'thai',
           'vietnamese', 'cajun_creole', 'brazilian', 'french', 'japanese',
           'irish', 'korean', 'moroccan', 'russian'], dtype=object)




```python
train_raw_df.describe()
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
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>39774.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>24849.536959</td>
    </tr>
    <tr>
      <th>std</th>
      <td>14360.035505</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>12398.250000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>24887.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>37328.500000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>49717.000000</td>
    </tr>
  </tbody>
</table>
</div>


