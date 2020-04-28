---

layout: post
title: "Expecting property name enclosed in double quotes"
key: 2020-03-21-json
pageview: true
modify_date: 
shraring: true
show_author_profile: true
show_subscribe: true
---
# Expecting property name enclosed in double quotes
## Background
In this project, I have to dump json file and open this json file in a loop. At first, I looked up some blogs on the internet, most of them talks about the misuse of quotes and double quotes, I checked my code, I found all of the keys in the dictionary used quotes instead of double quotes. So I thought this might be the problem. However, After editing this the problem still exists. I checked the error reporting again, and I foudn that it says the error took place in line 3226, that's the end of the file.  
I went to check line 3226, and I found I didn't deal with the iteration when file ends, so there is a comma left, and after deleting it, error disappears.
