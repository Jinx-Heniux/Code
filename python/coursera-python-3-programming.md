# Coursera - Python 3 Programming



## Foundations of Python Programming

### 24.14. Project - OMDB and TasteDive

{% code title="omdb_tastedive.py" %}
```python
import requests_with_caching
import json

def get_movies_from_tastedive(str):
    base_url = 'https://tastedive.com/api/similar'
    d = {'q': str, 'type': 'movies', 'limit': 5}
    results = requests_with_caching.get(base_url, params=d)
    # print(results.url)
    # print(results.text)
    return results.json()
    #return json.loads(results.text)
    
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# get_movies_from_tastedive("Bridesmaids")
# get_movies_from_tastedive("Black Panther")

def extract_movie_titles(info_dict):
    name_list = []
    res_list = info_dict["Similar"]["Results"]
    # print(type(res_list))
    # print(res_list)
    for item in res_list:
        if item['Name'] not in name_list:
            name_list.append(item['Name'])
    return name_list
    
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# extract_movie_titles(get_movies_from_tastedive("Tony Bennett"))
# extract_movie_titles(get_movies_from_tastedive("Black Panther"))

def get_related_titles(movie_titles_lst):
    title_lst = []
    for item in movie_titles_lst:
        title_lst = title_lst + extract_movie_titles(get_movies_from_tastedive(item))
    return list(set(title_lst)) # 利用python中集合元素惟一性特点，将列表转为集合，将转为列表返回，实现清除列表中重复元素。

# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# get_related_titles(["Black Panther", "Captain Marvel"])
# get_related_titles([])


def get_movie_data(str):
    base_url = 'http://www.omdbapi.com/'
    d = {'t': str, 'r': 'json'}
    results = requests_with_caching.get(base_url, params=d)
    # print(results.url)
    # print(results.text)
    return results.json()
        
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# get_movie_data("Venom")
# get_movie_data("Baby Mama")

def get_movie_rating(movie_dict):
    for item in movie_dict["Ratings"]:
        if item["Source"] == "Rotten Tomatoes":
            value = item["Value"]
            return int(value[:-1])
    return 0    
            
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
# get_movie_rating(get_movie_data("Deadpool 2"))


def get_sorted_recommendations(movie_titles_lst):
    r_movie_titles_lst = get_related_titles(movie_titles_lst)
    # print(r_movie_titles_lst)
    d = {}
    for item in r_movie_titles_lst:
        if item not in d:
            d[item] = get_movie_rating(get_movie_data(item))
    # print(d)
    s = sorted(d, key=lambda k: (d[k], k), reverse=True) # 16.5. Breaking Ties: Second Sorting
    # print(type(s))
    # print(s)
    return s
    
# some invocations that we use in the automated tests; uncomment these if you are getting errors and want better error messages
get_sorted_recommendations(["Bridesmaids", "Sherlock Holmes"])

```
{% endcode %}



{% embed url="https://gist.github.com/Jinx-Heniux/8e056d89b9956fc9337d3dcd65e44576" %}



## Links

* [Python 3 Programming | Coursera](https://www.coursera.org/specializations/python-3-programming)
* [Table of Contents — Foundations of Python Programming](https://fopp.umsi.education/books/published/fopp/index.html) || 在线 书籍
