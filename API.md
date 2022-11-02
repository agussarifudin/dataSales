# API Documentation

#### Description

This list is a brief explanation of functions, it's parameters and what it returns. Bear in mind that only the function that has parameters or has complex logic will be explained, regular function like `index()` function that only return a view will not be here.

## API Controller

### 1. getToken function

```sh
getToken()
```

#### Description

get token for API access with JWT

#### Return

bearer token for API authentication

### 2. getAllArticles function

```sh
getAllArticles($id=false,$limit=10,$offset=0,$sch_id=false)
```

#### Description

Show list articles with parameters

#### Parameters

| Parameters | Data Type | Description                                         |
| ---------- | --------- | --------------------------------------------------- |
| id         | int       | Article id (id) in article table                    |
| limit      | int       | Amount of articles returned                         |
| offset     | int       | first id to be displayed orderly from article table |
| sch_id     | int       | Schedule id (sch_id) in article table               |

#### Return

article list view based on parameters

### 3. getAllUser function

```sh
getAllUser()
```

#### Description

get list of all users

#### Return

list of all users from users table

### 4. getAllTask function

```sh
getAllTask($id=false,$status=false)
```

#### Description

Show list schedule based on id and status

#### Parameters

| Parameters | Data Type | Description                                      |
| ---------- | --------- | ------------------------------------------------ |
| id         | int       | Schedule id (id) in schedule table               |
| status     | boolean   | Status of schedule whether it's done or progress |

#### Return

list of schedule based on id and status

## Article Controller

### 1. List function

```sh
list($id)
```

#### Description

Show the list of articles.

#### Parameters

| Parameters | Data Type | Description                           |
| ---------- | --------- | ------------------------------------- |
| id         | int       | Schedule id (sch_id) in article table |

#### Return

article list view and sidebar data variable

### 2. Detail function

```sh
detail($id)
```

#### Description

Return the details of the article.

#### Parameters

| Parameters | Data Type | Description                      |
| ---------- | --------- | -------------------------------- |
| id         | int       | Article id (id) in article table |

#### Return

article detail view and related articles

### 3. Fetch function

```sh
fetch($id)
```

#### Description

get id for keyword from model and elastic data from elastic api function.

#### Parameters

| Parameters | Data Type | Description                           |
| ---------- | --------- | ------------------------------------- |
| id         | int       | Schedule id (sch_id) in article table |

#### Return

JSON data for article filter in article list page

### 4. Elastic API function

```sh
elastic_api($keywords = false, $source = [],$reversed = false,$sort_date = false,$sort_title = false,$sort_relevance = false,$to = false,$from = false,$search_field = '',$sch_id,$start,$limit)
```

#### Description

Make the elasticsearch query from available filters.

#### Parameters

| Parameters     | Data Type | Description                                                                                                |
| -------------- | --------- | ---------------------------------------------------------------------------------------------------------- |
| keywords       | String    | Keywords string for the relevance query (default value is **false**)                                       |
| source         | Array     | Sources array for filtering by source query (default value is **[]**)                                      |
| reversed       | String    | Reversed radio button value for reversing sort query to **desc** (default value is **false**)              |
| sort_date      | String    | Sort date radio button value for sorting article list by date query (default value is **false**)           |
| sort_title     | String    | Sort title radio button value for sorting article list by title query (default value is **false**)         |
| sort_relevance | String    | Sort relevance radio button value for sorting article list by relevance query (default value is **false**) |
| to             | String    | Limit date value range in for date range query (default value is **false**)                                |
| from           | String    | Start date value range in for date range query (default value is **false**)                                |
| search_field   | String    | Search string in search field for detailed string search query (default value is **false**)                |
| sch_id         | Integer   | Schedule id for the search filter                                                                          |
| start          | Integer   | Start offset value for the filter query                                                                    |
| limit          | Integer   | Size number for the query to display                                                                       |

#### Return

Elasticsearch hit records from available parameters

## Report Controller

### 1. Get Report

```sh
get_result($id=71)
```

#### Description

Get all calculated report data from other functions

```sh
$data['sch'] = $data['scheduleModel']->getAllSchedule();
$data['keyword'] = explode(',',$schedule[0]['keyword']);
$data['task_name'] = $schedule;
$data['word_cloud'] = $this->get_word_cloud($sch_id);
$data['total_date'] = $this->get_total_date($sch_id);
$data['total_art_source_by_date'] = $this->total_art_source_by_date($data['total_date']['date_done'],$data['total_date']['source_done'],$sch_id);
$data['total_art_by_source'] = $this->total_art_by_source($sch_id,$data['total_date']['source_done']);
$data['total_mention_by_keyword'] =$this->total_mention_by_keyword($sch_id,$data['total_art_by_source']['source_tot'],$data['keyword']);
$data['most_tag'] = $this->get_most_tag($sch_id);
```

and reassign the values to new variables.

```sh
$data_1 = [];
$i = 0;
# Assign new key value for total_art_source_by_date variable to name and data
foreach ($data['total_art_source_by_date'] as $key => $value) {
	$data_1[$i]['name'] = $key;
	$data_1[$i]['data'] = $value;
	$i++;
}
# Convert data to json
$data['area_chart_series'] = json_encode($data_1); // DB
# Reassign the array to area_chart_categories
$data['area_chart_categories'] = $data['total_date']['date_apos'];
```

#### Parameters

| Parameters | Data Type | Description                           |
| ---------- | --------- | ------------------------------------- |
| id         | int       | Schedule id (sch_id) in article table |

#### Return

Data array that consist of all required variables to display report charts.

### 2. Get Total Date

```sh
get_total_date($id)
```

#### Description

Make array of date from all articles by schedule id in string and array data type, also array of available source from articles by schedule id.

#### Parameters

| Parameters | Data Type | Description                           |
| ---------- | --------- | ------------------------------------- |
| id         | int       | Schedule id (sch_id) in article table |

#### Return

Array of `$data` which consists date with string and array datatype, also array of source.

```sh
array(2) {
  ["date_done"]=>
  array(5) {
    [0]=>
    string(10) "2021-11-07"
    [1]=>
    string(10) "2021-11-08"
    [2]=>
    string(10) "2021-11-09"
    [3]=>
    string(10) "2021-11-10"
    [4]=>
    string(10) "2021-11-11"
  }
  ["source_done"]=>
  array(3) {
    [0]=>
    string(16) "cnnindonesia.com"
    [13]=>
    string(9) "detik.com"
    [33]=>
    string(10) "kompas.com"
  }
}
```

### 3. Get Total Article by Source

```sh
total_art_by_source($id,$source_arr)
```

#### Description

Count total of article by source from required schedule id.

#### Parameters

| Parameters | Data Type | Description                           |
| ---------- | --------- | ------------------------------------- |
| id         | int       | Schedule id (sch_id) in article table |
| source_ar  | array     | Array of source by schedule id        |

#### Return

Array of `$data` which consists total of article from all source and total articles by source.

```sh
array(2) {
  ["source_tot"]=>
  array(3) {
    ["cnnindonesia.com"]=>
    int(13)
    ["detik.com"]=>
    int(20)
    ["kompas.com"]=>
    int(37)
  }
  ["tot_art"]=>
  int(70)
}
```

### 4. Get Total Mention By Keyword on every Source

```sh
total_mention_by_keyword($id,$source_and_tot_art,$keyword)
```

#### Description

Count total of keyword mentions by source from required schedule id.

#### Parameters

| Parameters         | Data Type | Description                               |
| ------------------ | --------- | ----------------------------------------- |
| id                 | int       | Schedule id (sch_id) in article table     |
| source_and_tot_art | array     | Array of source as key and total as value |
| keyword            | array     | Array of keyword                          |

#### Return

Array of `$data` which consists total of mentions from all source and keywords, also total mention on all articles.

```sh
array(2) {
  ["source_tot"]=>
  array(3) {
    ["cnnindonesia.com"]=>
    array(3) {
      ["vanessa angel"]=>
      int(97)
      ["vanessa angel kecelakaan"]=>
      int(0)
      ["vanessa angel meninggal"]=>
      int(5)
    }
    ["detik.com"]=>
    array(3) {
      ["vanessa angel"]=>
      int(332)
      ["vanessa angel kecelakaan"]=>
      int(26)
      ["vanessa angel meninggal"]=>
      int(50)
    }
    ["kompas.com"]=>
    array(3) {
      ["vanessa angel"]=>
      int(159)
      ["vanessa angel kecelakaan"]=>
      int(0)
      ["vanessa angel meninggal"]=>
      int(1)
    }
  }
  ["keyword_tot"]=>
  int(670)
}
```

### 5. Get Total Article From Every Source By Date

```sh
total_art_source_by_date($date_arr,$source_arr,$id)
```

#### Description

Count articles by date on every source. if the total is 0 it is required to still be appended because the order of element is according to the available date.

#### Parameters

| Parameters         | Data Type | Description                               |
| ------------------ | --------- | ----------------------------------------- |
| id                 | int       | Schedule id (sch_id) in article table     |
| source_and_tot_art | array     | Array of source as key and total as value |
| keyword            | array     | Array of keyword                          |

#### Return

Array of key value with source as key and amount of articles on that date as value.

```sh
array(3) {
  ["cnnindonesia.com"]=>
  array(5) {
    [0]=>
    int(3)
    [1]=>
    int(3)
    [2]=>
    int(4)
    [3]=>
    int(2)
    [4]=>
    int(1)
  }
  ["detik.com"]=>
  array(5) {
    [0]=>
    int(0)
    [1]=>
    int(0)
    [2]=>
    int(6)
    [3]=>
    int(6)
    [4]=>
    int(8)
  }
  ["kompas.com"]=>
  array(5) {
    [0]=>
    int(0)
    [1]=>
    int(9)
    [2]=>
    int(10)
    [3]=>
    int(9)
    [4]=>
    int(9)
  }
}
```

### 6. Get Total Mention From Every Source By Date

```sh
get_total_mention_by_date($id,$date,$source,$keyword)
```

#### Description

Count total of keyword mentions by date from date.

#### Parameters

| Parameters         | Data Type | Description                               |
| ------------------ | --------- | ----------------------------------------- |
| id                 | int       | Schedule id (sch_id) in article table     |
| source_and_tot_art | array     | Array of source as key and total as value |
| keyword            | array     | Array of keyword                          |

#### Return

Array of `$data` which consists total of mentions from all source and keywords, also total mention on all articles.

```sh
array(3) {
  [0]=>
  array(2) {
    ["name"]=>
    string(16) "cnnindonesia.com"
    ["data"]=>
    array(5) {
      [0]=>
      int(15)
      [1]=>
      int(18)
      [2]=>
      int(35)
      [3]=>
      int(28)
      [4]=>
      int(6)
    }
  }
  [1]=>
  array(2) {
    ["name"]=>
    string(9) "detik.com"
    ["data"]=>
    array(5) {
      [0]=>
      int(0)
      [1]=>
      int(0)
      [2]=>
      int(117)
      [3]=>
      int(118)
      [4]=>
      int(173)
    }
  }
  [2]=>
  array(2) {
    ["name"]=>
    string(10) "kompas.com"
    ["data"]=>
    array(5) {
      [0]=>
      int(0)
      [1]=>
      int(39)
      [2]=>
      int(47)
      [3]=>
      int(29)
      [4]=>
      int(45)
    }
  }
}
```

### 7. Get Word Cloud

```sh
get_word_cloud($schedule)
```

#### Description

Get 100 words with most occurence in all articles

#### Parameters

| Parameters | Data Type | Description                        |
| ---------- | --------- | ---------------------------------- |
| sch_id     | int       | Schedule id (id) in schedule table |

#### Return

JSON String of word cloud that consists of key value pair of the word and number of occurence.

```sh
string(3310) "[{"name":["vanessa"],"weight":[626]},{"name":["angel"],"weight":[418]},{"name":["kecelakaan"],"weight":[188]},{"name":["joddy"],"weight":[179]},{"name":["gala"],"weight":[136]},{"name":["saya"],"weight":[107]},{"name":["bibi"],"weight":[98]},{"name":["tubagus"],"weight":[80]},{"name":["mobil"],"weight":[76]},{"name":["ke"],"weight":[70]},{"name":["tol"],"weight":[67]},{"name":["tersangka"],"weight":[64]},{"name":["jombang"],"weight":[62]},{"name":["spdp"],"weight":[56]},{"name":["aku"],"weight":[55]},{"name":["dalam"],"weight":[54]},{"name":["sopir"],"weight":[45]},{"name":["makam"],"weight":[44]},{"name":["ayat"],"weight":[43]},{"name":["doddy"],"weight":[41]},{"name":["milano"],"weight":[35]},{"name":["aris"],"weight":[34]},{"name":["pasal"],"weight":[34]},{"name":["sudah"],"weight":[32]},{"name":["jalan"],"weight":[29]},{"name":["jakarta"],"weight":[29]},{"name":["dia"],"weight":[28]},{"name":["value"],"weight":[27]},{"name":["penyidikan"],"weight":[27]},{"name":["tahun"],"weight":[26]},{"name":["saat"],"weight":[26]},{"name":["video"],"weight":[25]},{"name":["paling"],"weight":[25]},{"name":["'"],"weight":[24]},{"name":["meninggal"],"weight":[23]},{"name":["faisal"],"weight":[23]},{"name":["pada"],"weight":[23]},{"name":["baca"],"weight":[22]},{"name":["keluarga"],"weight":[22]},{"name":["polisi"],"weight":[22]},{"name":["kita"],"weight":[21]},{"name":["fuji"],"weight":[21]},{"name":["latif"],"weight":[20]},{"name":["tom"],"weight":[20]},{"name":["mayang"],"weight":[20]},{"name":["gatot"],"weight":[19]},{"name":["karena"],"weight":[18]},{"name":["imran"],"weight":[18]},{"name":["belum"],"weight":[18]},{"name":["perkara"],"weight":[17]},{"name":["lucyana"],"weight":[16]},{"name":["fitri"],"weight":[16]},{"name":["asuransi"],"weight":[15]},{"name":["kasus"],"weight":[15]},{"name":["saksi"],"weight":[15]},{"name":["lagu"],"weight":[15]},{"name":["pemeriksaan"],"weight":[15]},{"name":["liwafa"],"weight":[14]},{"name":["harus"],"weight":[14]},{"name":["uang"],"weight":[13]},{"name":["luar"],"weight":[13]},{"name":["orang"],"weight":[13]},{"name":["mereka"],"weight":[12]},{"name":["chintya"],"weight":[11]},{"name":["ya"],"weight":[11]},{"name":["air"],"weight":[11]},{"name":["bag"],"weight":[11]},{"name":["anak"],"weight":[11]},{"name":["nggak"],"weight":[11]},{"name":["gelar"],"weight":[11]},{"name":["baru"],"weight":[10]},{"name":["lalu"],"weight":[10]},{"name":["fadly"],"weight":[10]},{"name":["jomo"],"weight":[10]},{"name":["kami"],"weight":[10]},{"name":["konten"],"weight":[10]},{"name":["masih"],"weight":[9]},{"name":["kejari"],"weight":[9]},{"name":["astra"],"weight":[9]},{"name":["sky"],"weight":[9]},{"name":["kamu"],"weight":[9]},{"name":["denny"],"weight":[9]},{"name":["kepada"],"weight":[8]},{"name":["arwah"],"weight":[8]},{"name":["telah"],"weight":[8]},{"name":["benturan"],"weight":[8]},{"name":["sempat"],"weight":[8]},{"name":["ussy"],"weight":[8]},{"name":["merangkak"],"weight":[8]},{"name":["lintas"],"weight":[8]},{"name":["kata"],"weight":[8]},{"name":["sama"],"weight":[8]},{"name":["siska"],"weight":[8]},{"name":["ansori"],"weight":[8]},{"name":["kecepatan"],"weight":[8]},{"name":["sebelum"],"weight":[7]},{"name":["adiknya"],"weight":[7]},{"name":["banget"],"weight":[7]},{"name":["pernah"],"weight":[7]},{"name":["alam"],"weight":[7]}]"
```

### 8. Get Most Tags Occurence

```sh
get_most_tag($sch_id)
```

#### Description

get ranking of 10 tags which have most occurence in all articles

#### Parameters

| Parameters | Data Type | Description                        |
| ---------- | --------- | ---------------------------------- |
| sch_id     | int       | Schedule id (id) in schedule table |

#### Return

Array of `$data` which consists of 10 tags with most occurence and the amount of occurence it has.

```sh
array(2) {
  ["tag"]=>
  string(205) "'vanessa angel','vanessa angel meninggal','vanessa angel kecelakaan','tubagus joddy','gala sky','bibi andriansyah','jombang','tubagus joddy tersangka','vanessa angel tewas kecelakaan','vanessa angel tewas'"
  ["amount"]=>
  string(27) "55,35,34,11,11,10,10,10,8,8"
}
```

### 9. Calculate most frequent words

```sh
most_frequent_words($string,$status, $stop_words = ['px','yang','di','dan','carpet','flying','googletag','not','found','important','tblock','sc','cls-','a','translate	z','show','width','tr','max-width','mr','table','tbody','div','top','td','-','dari','untuk','juga','itu','ada','ini','dengan','akan','tidak','kompas','indonesia','com','null','nbsp','var'], $limit = 10)
```

#### Description

Calculate most words in all articles and most frequent tags in all articles

#### Parameters

| Parameters | Data Type | Description                       |
| ---------- | --------- | --------------------------------- |
| string     | string    | string of article content or tags |
| status     | string    | string of article content or tags |

#### Return

Array of `$data` which consists of 10 tags with most occurence and the amount of occurence it has.

```sh
array(2) {
  ["tag"]=>
  string(205) "'vanessa angel','vanessa angel meninggal','vanessa angel kecelakaan','tubagus joddy','gala sky','bibi andriansyah','jombang','tubagus joddy tersangka','vanessa angel tewas kecelakaan','vanessa angel tewas'"
  ["amount"]=>
  string(27) "55,35,34,11,11,10,10,10,8,8"
}
```
