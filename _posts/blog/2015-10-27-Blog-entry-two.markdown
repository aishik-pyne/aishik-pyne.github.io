---
layout: post
title:  "Blog entry two"
date:   2015-10-27 18:11:16
categories: blog
---

# Web Development Course (Journal)

### Two broad classifications:

![Front_vs_BackEnd](https://davidmles.com/wp-content/uploads/2017/02/frontend-vs-backend-768x432.png)

1. **Frontend** - Manages the looks and design of the website. Everytime a user(client) requests a webpage a copy of the Frontend matter is send to the clients browser and the browser displays it.
2. **Backend** - This code lies is a server hosted by admin. This can contain sensetive data like username and passwords and also website data. When a request is made and language like **PHP** which runs in the background parses the requests and generates a Frontend matter which is sent to the client.

---
### Content Management System:

![CMS](https://image.slidesharecdn.com/implementingandmanagingcms-160425070428/95/implementing-and-managing-content-management-systems-20-638.jpg?cb=1461567949)

 The overall process can be managed for blog websites by a **CMS** (Content Management System) e.g. Wordpress , which requires almost no coding experience. It's a GUI based platform which is hosted in a server.

However the functionality of this kind of system is quite restricted and only uselful to handle _static blogs_.  
Hence to have more control over the backend we use languages and services like **PHP, MySQL, Django** etc.

---
## Frontend

### Frontend Language

Before moving on to any backend stuff a basic knowlegde of a webpage and Frontend languages like HTML, CSS and JavaScript is important. This isn't intimidating and quite easy to learn.
![Frontend-Languages](https://www.planet-source-code.com/vb/2010Redesign/images/LangugeHomePages/HTML5_CSS_JavaScript.png)
1. **HTML** - This gives the basic skeleton of how data (text and images are dispalyed in the browser)

2. **CSS** - This beautifies the skeleton adding colors and shapes  

3. **JS** - A scripting language to handle local client interactions like animations      

Here is a cheat sheet website to all kind of Web Development Languages and tuorials.  
[W3 Schools](https://www.w3schools.com/html/)

You can make small webpages and save them locally and keep testing and modifying them.  
Have a good practice in these languages before moving on to the next sections.

---
## Backend

Before going into the coding part of the backend let's dicuss about **Domains** and **Hosting**.  

### Hosting

The website/webpages to be specific are nothing but files which are requested by clients and the server sends them the files.
| Static | Dynamic |
| :------------- | :------------- |
| This are static files which once written and saved, will be sent to clients.   | This are pages pages will on request with certain parameters as inputs will generate the page as per demand.  |
| **e.g.** a blog post  | **e.g.** A user name and password is passed to the server as parameters and the server looks up the database to verfy it and then sends back to the client a post-login dashboard page  |

![client-server](http://www.whoishostingthis.com/blog/wp-content/uploads/2015/04/process.png)  
This handling of requests with parameters are done by PhP.

Now the server is a computer which can be your laptop too. But the problem with this is , it will have to be kept on always to meet demans of client. Thus we upload your files to a hosting provider like GoDaddy Or Firebase and people can request the files from their servers which are on 24 * 7.

---
### Domains

Every machine is identified using an IP address. **e.g.** 127.123.0.53  
These are hard to remember for the clients. So we give these IP's Nick Names called **Domains**. When we requesting something from www.jujoural.com the request hits the server with the IP where jujournal is hosted.

---
### Backend Language

<img src="https://newrelic.com/assets/pages/apm/php/php-elephant-logo-bd4f9d83be8c8563248fe4793f90bae7.png" style="width: 200px"/>  

Again among various languages we can choose form the one we are currently using is **PhP 5.5**.  
[Here](https://www.w3schools.com/php/) is a tutorial to get you started with that.

You can locally launch a server on your laptop to run PhP using [Apache server](https://www.guru99.com/apache.html).  

---
### Database Management System (DBMS)

<img src="https://upload.wikimedia.org/wikipedia/en/thumb/6/62/MySQL.svg/1200px-MySQL.svg.png" style="width: 200px"/>  


There are a varity of Database we can choose from like **MySQL, MariaDB**. We'll focus on **MySQL**  as The Journal website currently uses it.  
[Here's](https://www.tutorialspoint.com/mysql/ "Tutorial Point") a tutorial to get you get started with.



---
This article has been written by  
Aishik Pyne  
<a href = "mailto: aishikpyne@gmail.com">aishikpyne@gmail.com</a>

