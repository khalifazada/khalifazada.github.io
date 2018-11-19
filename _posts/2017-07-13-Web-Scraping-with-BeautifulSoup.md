---
layout: post
title: Web Scraping with BeautifulSoup
---

![beautifulsoup](https://www.crummy.com/software/BeautifulSoup/10.1.jpg 'beautifulsoup')

# How to do Web Scraping

With [`requests`](http://docs.python-requests.org/en/master/) and [`beautifulsoup`](https://www.crummy.com/software/BeautifulSoup/)

___
[TOC]
___

## Introduction
**`requests`** allows sending HTTP requests
**`beautifulsoup`** allows extracting tags from an HTML document

## Importing & Initializing

To import *BeautifulSoup* we request **`from bs4 import BeautifulSoup`** and request the content of the desired URL.

	response = requests.get('URL')
	content = response.content

We then pass `content` to *BeautifulSoup* parser:

	parser = BeautifulSoup(content, 'html.parser')

## Extracting Tags & Showing Text

To extract content we request it from the parser:

	body = parser.body
	print(body.text)	
	
	title = parser.title
	print(title.text)

The text from the following tags will be extracted and printed.

### Using `find_all`

Using the **`find_all`** method to extract all occurrences of a particular tag into a **list** `tag_nm`.

	# create a list of all occurrences of a particular html tag
	tag_nm_lst = parser.find_all('html_tag_name')

	# print content from every occurrence
	for tag_occur in tag_nm_lst:
		print(tag_occur.text)

#### Element IDs

To find an element divided by an `id` we pass it as an additional attribute into the `find_all` method

	tag_nm_lst = parser.find_all('html_tag_name', id='id_name')

#### Element Classes

To find elements that share a common characteristic weuse **`classes`** attribute to find all such tags.

	tag_nm_lst = parser.find_all('tag_name', class_='class_name')

#### CSS Selectors

We can use *BeautifulSoup's* **`.select()`** method to work with *CSS* selectors.

	tag_nm_lst = parser.select('.class_name')
	tag_nm_lst = parser.select('#id_name')

We can also use **nested** structure of *CSS* selectors and/or *ID* tags

	nested_tags = 'body div .first #inner-text'
	tag_nm_lst = parser.select('nested_tags)
