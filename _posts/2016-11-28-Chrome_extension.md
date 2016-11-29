---
layout: post
title: 'Create chrome extension to solve special problem'
tags:
  - chrome
  - extension
  - js
category: tool
---
Chrome extension can be used to do many operatioin to the website, such as graping the data, modifying the cookies and so on. So the extension can get what ever the website expose.

Next I will create a extension to implement the reverse proxy which will redirect the request with cookies.
<!--more-->

# Basic knowledge
It is very easy to create a chrome extension. The only needed is a chrome browser and some knowledge about JS and CSS.

# Create the first extension

 * create a folder to store the extension.
 * configure the file of "manifest.json" which is only mandatory for this extension

        {
             "manifest_version": 2,
             "name": "Reverse Proxy",
             "description": "This extentioin will redirect the request to the defination domain with cookies",
             "version": "1.0"
        }


Until now we alread have created a basic extension.

# Load and Run
Then we will load and run this extension.

 * go to the page of "chrome://extensions/"
 * enable the developer mode
 * click the button of "Load unpacked extension" to load the extension
 * select your created folder, then the logo will be dispalyed on the menu

# Add popup HTML to the extension
It is very easy to add the popup html to the extension, you can just add the source below to "manifest.json" file.

    "browser_action": {
        "default_popup": "popup.html"
    }

Then create the file of "popup.html", reload the extension, click the logo of your extensioin, you will see what you define in the file of "popup.html".

# Add JS to your HTML
Many operation will be create in the javascript file, so this step will teach you how to add javascript file to your extension. In order to add the JS file, please refer below source and add it to "manifest.json".

    "background": {"scripts":["jquery.min.js", "popup.js", "cookie.js"]},

`Note` that the js file is loaded by the order of which your defined, if some js file does not work, please check your order.

# Add permissions
In this step you can define that what operation your extension can do and which request can access to your extension. The below  source is also need to add to the "manifest.json" file.

    "permissions": [
      "webRequest",
      "*://domain/*",
      "http://localhost:8080/*",
      "webRequestBlocking",
      "cookies",
      "storage"
    ]

 * "webrequest" means the request of website can use your extension
 * "*://domain/*" means the request to this domain can use your extension
 * "cookies" means your extension can do the operation of cookies
 * "storage" means your extension can store some variable to your localStorage.

# Add event to get or set cookies
Add the below source to "cookie.js"

    var originCookie;

    chrome.webRequest.onBeforeRequest.addListener(
        function(details) {
            getCookie();
            if (originCookie) {
                setCookies();
                return {redirectUrl: localStorage["redirect"] + details.url.match(/^https?:\/\/[^\/]+([\S\s]*)/)[1]};
            }
             return {cancel: false};
        },
        {urls: ["<all_urls>"]},
        ["blocking"]
    );

    function getCookie() {
        chrome.cookies.get({'url': localStorage["origin"], 'name': localStorage["name"]}, function(cookie) {
            originCookie = cookie;
        });
    }

    function setCookies() {
        chrome.cookies.set({"url": localStorage["redirect"], "value": originCookie.value,"name": originCookie.name,}, function() {});
    }

# Add event listener to dom object
Add the below source to "popup.js"

    document.addEventListener('DOMContentLoaded', function () {
        $("#redirect").val(localStorage["redirect"]);
        $("#origin").val(localStorage["origin"]);
        $("#name").val(localStorage["name"]);

        $("#redirect").on('change', function() {
            localStorage["redirect"] = $('#redirect').val();
        });

        $("#name").on('change', function() {
            localStorage["name"] = $('#name').val();
        });

        $("#origin").on('change', function() {
            localStorage["origin"] = $('#origin').val();
        });
    });

# Add HTML to popup.html

    <!DOCTYPE html>
    <html>
        <head>
            <style>
                body {
                    min-width: 420px;
                    overflow-x: hidden;
                    font-family: Arial, sans-serif;
                    font-size: 12px;
                }
                input, textarea {
                    width: 420px;
                }
                input#save {
                    font-weight: bold; width: auto;
                }
            </style>
            <script src="jquery.min.js"></script>
            <script src="popup.js"></script>
        </head>
        <body>
            <form id="addRedirectInfo">
                <p><label for="origin">Origin URL</label><br />
                <input type="text" id="origin" name="origin" size="50" value="" /></p>
                <p><label for="redirect">Redirect URL</label><br />
                <input type="text" id="redirect" name="redirect" size="50" value="" /></p>
                <p><label for="name">Cookie Name</label><br />
                <input type="text" id="name" name="name" size="50" value="" /></p>
            </form>
        </body>
    </html>
