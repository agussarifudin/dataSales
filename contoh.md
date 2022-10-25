<p align="center" >
  <img src="https://drive.google.com/uc?id=1HITjoRu0Yena0phoKnyUU0CmlxR3yCQl" />
</p>

<h1 align="center">Nettrace (CI4)</h1>

## Description

Nettrace is a crawling application that scrape data from news websites such as : [Detik.com](https://detik.com/), [Kompas.com](https://kompas.com/), [Cnnindonesia.com](https://cnnindonesia.com/) and then analyze the data to be represented on Graphical UI.

Nettrace uses [Codeigniter](https://codeigniter.com/user_guide/intro/index.html) for the back-end logic and front-end views. It also uses NodeJS Express for the schedulling and [puppeteer](https://github.com/puppeteer/puppeteer) library to scrape the data.

## Prerequisites

|              |        |
| ------------ | ------ |
| **Bold Key** | Value1 |
| Normal Key   | Value2 |

PHP version 7.3 or higher is required, with the following extensions installed:

| Extensions                                                     | Description                                     |
| -------------------------------------------------------------- | ----------------------------------------------- |
| [intl](http://php.net/manual/en/intl.requirements.php)         | to enable locale-aware operations               |
| [libcurl](http://php.net/manual/en/curl.requirements.php)      | if you plan to use the HTTP\CURLRequest library |
| json                                                           | enabled by default - don't turn it off          |
| [mbstring](http://php.net/manual/en/mbstring.installation.php) | to enable mbstring functions                    |
| [mysqlnd](http://php.net/manual/en/mysqlnd.install.php)        | for mysql                                       |
| xml                                                            | enabled by default - don't turn it off          |

NodeJS version 16.0.0 and NPM version 8.0.0 or higher is required but you can just install nvm latest version
|Extensions|Description|
|-|-|
|[nvm](https://github.com/nvm-sh/nvm)|for npm and node|
|[nodemon](https://www.npmjs.com/package/nodemon)|for testing service|
|[pm2](https://www.npmjs.com/package/pm2)|for to run the service and it's configuration|

## Installation

`git clone https://github.com/AtapTeduh/SmartTrace-v2.git` to clone the project to your local environment. You can also download the project manually by choosing the `ci4` branch and download the `zip` file.

After you clone the project do `composer install` at the root folder of the project to install required codeigniter library. then `cd node_smarttrace` and `npm i` to download NodeJS modules required.

In order for [puppeteer](https://github.com/puppeteer/puppeteer) to run it needs a google chrome browser installed. You can use the default chromium browser for puppeteer but it is highly recommended to use native google chrome browser. You can check if chrome is installed correctly from this folder `/opt/google/chrome/google-chrome`

## Setup

Edit the database credentials at `.env` file to fit your local database credentials. Don't forget to change the project `ENVIRONMENT` to `PRODUCTION` when you are pushing to the production server.

For the NodeJS database configuration edit the `node_smarttrace\config\db.config.js` file to fit your local database credentials.

## Source Tagging

Currently manual tagging is required to add new source. When the user added new source the status of the source will be automatically added to `Waiting for Approval`. The administrator have to manually added the HTML Tagging for the data that is going to be scrapped.

You can check the step by step tutorial on how to add new source tagging [here](https://github.com/maulana020716/nettrace_v2/blob/main/Source_tagging.md)

## Running the scheduler

I have prepared one `server.js` to get the list of article to be scrapped and 4 `server_article.js` file to get the contents of the article to be run as a service.

You can use [PM2](https://www.npmjs.com/package/pm2) library to run the service and configure your services more easily.

PM2 Config :

`pm2 start server.js` to start the server

`pm2 stop server.js` to stop the server

`pm2 list` to get the list of running services

## How it works

The user can make a schedule to do a crawling based on the keyword, sources, and how often the crawling happens.

When the `server.js` service is started, it will check the `schedule` table every 1 second whether there is a schedule at the moment. and when there is, the service will run a browser headless as to access the url for example `https://www.kompas.com/tag/` followed by the keyword that the user inserted. And if the html tagging is suitable to what we have at the `source` table it will scrape the data such as link, title and thumbnail from the article and insert it to `article` table.

The status of record inserted by the `server.js` service is still `0` or `false`, so it will not be dispayed in the Browsing Article Tab. the `server_article.js` will get the contents of the article such as tagging, contents, article date, insert them to the `article` tab and then change the status to `1` or `true`.

When the `article_status` is true it can be displayed at the **Browsing Article** tab, and it will also be able to be get analyzed in the **Report** tab.

## Development and Usage

User Guide is available in the application for the end-user to use the app. CodeIgniter-Ion-Auth is the primary user authentication. You can check the [documentation](https://github.com/benedmunds/CodeIgniter-Ion-Auth/tree/4) for usage.

Codeigniter filter is used for checking if the user has logged in or not. You can add more uri for the check login feature in `app/Filters.php` file.

```sh
public $filters = [
	'checkLogin' => ['before' => ['dashboard/*', '/','schedule/*','source/*','report/*','article/*']],
];
```

You can check the API documentation for all Controllers [here](https://github.com/AtapTeduh/SmartTrace-v2/blob/ci4/API.md).

For API access and request examples you can check them [here](https://github.com/AtapTeduh/SmartTrace-v2/blob/ci4/API_credentials.md)

## Elasticsearch Installation

[Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.16/index.html) is used by default for the filter feature in **Browsing Article** tab, but using models is also available.

```sh
function fetch($id)
{
	$data = $this->data;
	$keywords = $data['articleModel']->getArticleKeyword($id)[0]['keyword'];
	// IF YOU WANT TO USE THE MODEL
	// $datas = $data['articleModel']->fetch_data($this->request->getPost('arr_source'),$this->request->getPost('reserved'),$this->request->getPost('sort_date'),$this->request->getPost('sort_title'),$this->request->getPost('sort_relevance'),$this->request->getPost('date_range_to'),$this->request->getPost('date_range_from'),$this->request->getPost('searchall'),$this->request->getPost('search'),$this->request->getPost('limit'), $this->request->getPost('start'));
	// return $this->response->setJSON($datas->getResultArray());

	// IF YOU WANT TO USE ELASTICSEARCH
	$datas = $this->elastic_api($keywords, $this->request->getPost('arr_source'),$this->request->getPost('reserved'),$this->request->getPost('sort_date'),$this->request->getPost('sort_title'),$this->request->getPost('sort_relevance'),$this->request->getPost('date_range_to'),$this->request->getPost('date_range_from'),$this->request->getPost('searchall'),$this->request->getPost('search'),$this->request->getPost('start'),$this->request->getPost('limit'));
	return $this->response->setJSON($datas);
}
```

You can follow this [tutorial](https://phoenixnap.com/kb/how-to-install-elk-stack-on-ubuntu) on how to install the ELK Stack to your local environment.

When you have install ELK, you can change the RAM usage for elasticsearch java service at the `jvm.options` file in `/etc/elasticsearch/` directory. The documentation suggest to set the heap size to at least 50% of your total RAM.

```sh
-Xms4g
-Xms4g
```

## Elasticseach Usage

[Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/7.16/index.html) is used with guzzle to access the elasticsearch host. Don't forget to start the service before running the application.

You can use [logstash](https://www.elastic.co/guide/en/elasticsearch/reference/7.16/logstash-apis.html) for migrating data from MySQL to Elasticsearch. Follow this [tutorial](https://qbox.io/blog/migrating-mysql-data-into-elasticsearch-using-logstash) on how to migrate MySQL to Elastic Search

Make sure that there is only one `logstash.conf` file on `/etc/logstash` directory, as it will read all the `.conf` file and make duplicates if there is more than one.

Here is the logstash.conf that i used

```sh
input {
   jdbc {
    jdbc_connection_string => "jdbc:mysql://localhost:3306/smarttrace"
    # The user we wish to execute our statement as
    jdbc_user => "root"
    jdbc_password => "password"
    # The path to our downloaded jdbc driver
    jdbc_driver_library => "/usr/share/java/mysql-connector-java-8.0.28.jar"
    jdbc_driver_class => "com.mysql.cj.jdbc.Driver"
    # our query
    schedule => "* * * * *"
    statement => "SELECT * FROM article WHERE updated_at > :sql_last_value"
    use_column_value => true
    tracking_column => "updated_at"
   }
}

output {
  stdout { codec => json_lines }
  elasticsearch {
  "hosts" => "localhost:9200"
  "index" => "article"
  "document_type" => "data"
  "document_id" => "%{id}"
  }
}
```

## License

The MIT License (MIT)
